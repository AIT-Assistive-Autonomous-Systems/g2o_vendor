cmake_minimum_required(VERSION 3.8)
project(g2o_vendor)

# Default to C++17
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic -g)
endif()

set(BUILD_SHARED_LIBS ON)

find_package(ament_cmake REQUIRED)
find_package(ament_cmake_core REQUIRED)
find_package(ament_cmake_vendor_package REQUIRED)

find_package(spdlog_vendor)
find_package(G2O NAMES g2o G2O)

if(NOT G2O_FOUND)
  list(INSERT CMAKE_MODULE_PATH 0 ${CMAKE_CURRENT_SOURCE_DIR}/cmake/Modules)
  set(G2O_AMENT_EXTRAS CONFIG_EXTRAS g2o_vendor-extras.cmake)

  find_package(G2O NAMES g2o G2O)
endif()

# CXX Flasgs (currently not LD), shared libs, build type auto forwarded by vendor.
# sor currently custom LDFLAGS must be set by env LDFLAGS for cmake beaded vendors
# and manually for all others anyway.

set(SOURCE_DIR ${CMAKE_CURRENT_LIST_DIR}/thirdparty/g2o)

ament_vendor(g2o_vendor
  SATISFIED ${G2O_FOUND}
  VCS_URL ${SOURCE_DIR}
  VCS_VERSION f9e1c12ac1ce0fb7fe1eccbea090f05c5d1e777d
  GLOBAL_HOOK
  CMAKE_ARGS
    -DG2O_BUILD_APPS=OFF
    -DG2O_BUILD_EXAMPLES=OFF
    -DG2O_USE_CHOLMOD=OFF
  )

include(ExternalProject)

find_program(GIT_COMMAND NAMES git REQUIRED)

# Allows source to be kept
if(TARGET g2o_vendor)
  ExternalProject_Add_Step(g2o_vendor submodule_download
    COMMAND ${GIT_COMMAND} submodule update --init --recursive --recommend-shallow --force
    COMMAND ${GIT_COMMAND} submodule foreach --recursive git clean -xfd
    WORKING_DIRECTORY ${CMAKE_CURRENT_LIST_DIR}
    DEPENDEES mkdir
    DEPENDERS download
  )

  install(
    FILES ${SOURCE_DIR}/doc/license-bsd.txt ${SOURCE_DIR}/doc/license-lgpl.txt
      ${SOURCE_DIR}/doc/license-gpl.txt
      ${SOURCE_DIR}/doc/robots.bib ${SOURCE_DIR}/README.md
    DESTINATION opt/${PROJECT_NAME}/share/doc/g2o/
    )

  install(
    FILES ${SOURCE_DIR}/g2o/autodiff/LICENSE
    DESTINATION opt/${PROJECT_NAME}/share/doc/autodiff/
    )

  install(
    FILES ${SOURCE_DIR}/g2o/EXTERNAL/freeglut/COPYING
    DESTINATION opt/${PROJECT_NAME}/share/doc/freeglut/
    )

    set(G2O_AMENT_EXTRAS)
endif()

install(
  FILES LICENSE
  DESTINATION share/doc/${PROJECT_NAME}
  )


if(G2O_AMENT_EXTRAS)
  install(DIRECTORY cmake DESTINATION share/${PROJECT_NAME})
endif()

ament_package(${G2O_AMENT_EXTRAS})

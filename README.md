# g2o_vendor

This is a vendor package either finding or building the g2`g2o` library. Currently there is no official rosdep key for this library thus it will always build it unless you manually installed it.

# License

The vendoring project is licensed using MIT (see [License](LICENSE)). Please refer to [g2o][g2o-master] and their third party libs for total effective license. See below for an excerpt of the notice from g2o for the current [default version][g2o-default]. For system or manual provided g2o implementations check their licenses.

## Build options

We enable the build shared lib options and as long as no changes are made to this repo or `g2o` itself, this should not include any GPL license code as we disabled the examples and apps. But as noted below some libs like suitesparse may be compiled with GPL features. So either don't use it or remcompile that if it is an issue.

## g2o notes
g2o is licensed under the BSD License. However, some libraries are available
under different license terms. See below.

The following parts are licensed under LGPL v2.1+:

-   csparse_extension

The following parts are licensed under GPL3+:

-   g2o_viewer
-   g2o_incremental
-   slam2d_g2o (example for 2D SLAM with a QGLviewer GUI)

Please note that some features of CHOLMOD (which may be used by g2o, see
libsuitesparse below) are licensed under the GPL. To avoid the GPL, you may
have to re-compile CHOLMOD without including its GPL features. The CHOLMOD
library distributed with, for example, Ubuntu or Debian includes the GPL
features. For example, the supernodal factorization that is licensed under GPL
is considered by g2o if it is available.

Within sub-folders we include software not written by us to guarantee easy compilation and integration into g2o itself.

-   ceres: BSD (see g2o/autodiff/LICENSE)
    Extracted headers to perform Automatic Differentiation.

-   freeglut: X-Consortium (see g2o/EXTERNAL/freeglut/COPYING)
    Copyright (c) 1999-2000 Pawel W. Olszta
    We use a stripped down version for drawing text in OpenGL.

See the doc folder for the full text of the licenses.

g2o is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
licenses for more details.

## Copied code

We included the cmake file `FindG2O.cmake` to find g2o to test for the existence of the project prior to building it.

[g2o-default]: https://github.com/RainerKuemmerle/g2o/tree/f9e1c12ac1ce0fb7fe1eccbea090f05c5d1e777d
[g2o-master]: https://github.com/RainerKuemmerle/g2o/tree/master
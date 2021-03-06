# Copyright 2012, 2013, 2014 CNRS-LAAS
#
# Author: Florent Lamiraux, Thomas Moulard
#
# This file is part of test-hpp
# hpp-model-urdf is free software: you can redistribute it and/or modify
# it under the terms of the GNU Lesser General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# hpp-model-urdf is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Lesser Public License for more details.
# You should have received a copy of the GNU Lesser General Public License
# along with test-hpp  If not, see <http://www.gnu.org/licenses/>.

INCLUDE_DIRECTORIES(${Boost_INCLUDE_DIRS})

# Make Boost.Test generates the main function in test cases.
#ADD_DEFINITIONS(-DBOOST_TEST_DYN_LINK -DBOOST_TEST_MAIN)

# ADD_TESTCASE(NAME)
# ------------------------
#
# Define a test named `NAME'.
#
# This macro will create a binary from `NAME.cc', link it against
# Boost and add it to the test suite.
#
MACRO(ADD_TESTCASE NAME GENERATED)
  IF (${GENERATED} STREQUAL TRUE)
    ADD_EXECUTABLE(${NAME} ${CMAKE_CURRENT_BINARY_DIR}/${NAME}.cc)
  ELSE()
    ADD_EXECUTABLE(${NAME} ${NAME}.cc)
  ENDIF()
  ADD_TEST(${NAME} ${RUNTIME_OUTPUT_DIRECTORY}/${NAME})

  # Link against Boost and project library.
  TARGET_LINK_LIBRARIES(${NAME}
    ${Boost_LIBRARIES}
    )
  PKG_CONFIG_USE_DEPENDENCY(${NAME} hpp-model-urdf)
ENDMACRO(ADD_TESTCASE)

CONFIG_FILES (
  body-positions-mesh.cc
  interbody-distances-mesh.cc
  interbody-distances-capsule.cc
  interbody-distance-time-of-computation.cc
  )

ADD_TESTCASE (body-positions-mesh TRUE)
ADD_TESTCASE (interbody-distances-mesh TRUE)
ADD_TESTCASE (interbody-distances-capsule TRUE)
ADD_TESTCASE (interbody-distance-time-of-computation TRUE)
ADD_TESTCASE (projection FALSE)
PKG_CONFIG_USE_DEPENDENCY(projection hpp-constraints)
PKG_CONFIG_USE_DEPENDENCY(projection hpp-wholebody-step)
PKG_CONFIG_USE_DEPENDENCY(projection hpp-core)

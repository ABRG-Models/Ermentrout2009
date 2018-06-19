# AUTOMOC requires 2.8.6. Per-target include directories would require 2.8.8
cmake_minimum_required(VERSION 2.8.8)
project(Ermentrout2009)

message(STATUS "Install prefix: ${CMAKE_INSTALL_PREFIX}")
message(STATUS "  (This can be changed with `cmake -DCMAKE_INSTALL_PREFIX=/some/place`")

set(CMAKE_CXX_FLAGS "-Wall -std=c++11")
set(CMAKE_C_FLAGS "-Wall")

# Lib finding - we have to do this, so that the correct include paths
# are present for OpenGL, OpenCV, etc; they're all included by
# morphologica header files.
find_package(OpenCV REQUIRED)
find_package(OpenGL REQUIRED)
find_package(GLUT REQUIRED)
find_package(X11 REQUIRED)
include_directories(${OPENCV_INCLUDE_DIR} ${OPENGL_INCLUDE_DIRS} ${GLUT_INCLUDE_DIRS})

# Find libmorphologica using pkgconfig
find_package(PkgConfig)
if(PKG_CONFIG_FOUND)
  pkg_check_modules(MORPH REQUIRED libmorphologica)
  if (MORPH_FOUND)
    include_directories(${MORPH_INCLUDEDIR})
  else()
    message(FATAL_ERROR "You need libmorphologica from github.com/ABRG_Models/morphologica")
  endif(MORPH_FOUND)
else()
  message(WARNING "There's no pkg-config on this system to check for libmorphologica. "
    "You can set MORPH_LIB with `cmake -DMORPH_LIB=/pathto/lib` ...")
endif()


# The main process executable
add_executable(process process.cpp)
target_link_libraries(process ${MORPH_LDFLAGS})

# For debugging of variables:
set(DEBUG_VARIABLES ON)
if(DEBUG_VARIABLES)
  get_cmake_property(_variableNames VARIABLES)
  foreach (_variableName ${_variableNames})
    message(STATUS "${_variableName}=${${_variableName}}")
  endforeach()
endif(DEBUG_VARIABLES)

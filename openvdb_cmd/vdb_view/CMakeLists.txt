# Copyright Contributors to the OpenVDB Project
# SPDX-License-Identifier: Apache-2.0
#
#[=======================================================================[

  CMake Configuration for VDB View

#]=======================================================================]

cmake_minimum_required(VERSION 3.20)
project(VDBView LANGUAGES CXX)

include(GNUInstallDirs)

find_package(OpenGL REQUIRED)

if(WIN32)
  find_package(GLEW REQUIRED)
endif()

find_package(glfw3 3.3 REQUIRED)

set(SOURCE_FILES
  Camera.cc
  ClipBox.cc
  Font.cc
  main.cc
  RenderModules.cc
  Viewer.cc
)

# Create console application (not WIN32 GUI)
add_executable(vdb_view ${SOURCE_FILES})

if(WIN32)
  # Add Windows system libraries needed by GLFW
  set(WINDOWS_SYSTEM_LIBS
    user32
    gdi32
    shell32
    winmm
    setupapi
    version
    imm32
  )
  
  target_compile_definitions(vdb_view PRIVATE 
    -DGL_GLEXT_PROTOTYPES=1
    -DGLFW_STATIC
    -DGLEW_STATIC
  )
endif()

target_link_libraries(vdb_view
  ${OPENVDB_BINARIES_DEPENDENT_LIBS}
  OpenGL::GL
  glfw
  openvdb
)

if(WIN32)
  target_link_libraries(vdb_view 
    GLEW::GLEW
    ${WINDOWS_SYSTEM_LIBS}
  )
endif()

install(TARGETS vdb_view RUNTIME DESTINATION ${OPENVDB_INSTALL_BINDIR})

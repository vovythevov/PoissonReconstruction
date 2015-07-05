cmake_minimum_required(VERSION 2.8)

project(PoissonReconstruction)

set(CMAKE_MACOSX_RPATH 1)

if (Fetch_PoissonReconstruction OR Module_PoissonReconstruction)
  message(STATUS "PoissonReconstruction: Building PoissonReconstruction as a Remote Module")
  set(EXECUTABLE_OUTPUT_PATH ${ITK_BINARY_DIR})
  set(PoissonReconstruction_BINARY_DIR ${EXECUTABLE_OUTPUT_PATH})
  itk_module_impl()
else()
  FIND_PACKAGE(VTK REQUIRED)
  INCLUDE(${VTK_USE_FILE})
  message(STATUS "PoissonReconstruction: Building PoissonReconstruction stand-alone")
endif()

if( ${VTK_BUILD_SHARED_LIBS} MATCHES ON )
  set( SHARED_LIBS "SHARED" )
  add_definitions( "-DvtkPoissonReconstruction_EXPORTS" )
  add_definitions( "-DBUILD_SHARED_LIBS" )
else(${VTK_BUILD_SHARED_LIBS} MATCHES ON )
  set( SHARED_LIBS "STATIC" )
endif(${VTK_BUILD_SHARED_LIBS} MATCHES ON )

set( CMAKE_RUNTIME_OUTPUT_DIRECTORY
  ${PoissonReconstruction_BINARY_DIR}/bin
  CACHE PATH "path for all executables"
)

set( CMAKE_ARCHIVE_OUTPUT_DIRECTORY
  ${PoissonReconstruction_BINARY_DIR}/lib
  CACHE PATH "path for all libraries"
)

SET( CMAKE_LIBRARY_OUTPUT_DIRECTORY
  ${PoissonReconstruction_BINARY_DIR}/lib
  CACHE PATH "path for all libraries"
)

include_directories(
  ${PoissonReconstruction_SOURCE_DIR}/source
  ${PoissonReconstruction_BINARY_DIR}
)

add_subdirectory(source)

include( CTest )
add_subdirectory(example)

set(BUILD_PARAVIEW_PLUGIN OFF CACHE BOOL "Build Paraview plugin?")

if(BUILD_PARAVIEW_PLUGIN)
  add_subdirectory(plugin)
endif(BUILD_PARAVIEW_PLUGIN)

option( BUILD_DOCUMENTATION "Build Doxygen Documentation" OFF )

if( BUILD_DOCUMENTATION )
  add_subdirectory( documentation )
endif( BUILD_DOCUMENTATION )


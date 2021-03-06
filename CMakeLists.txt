cmake_minimum_required(VERSION 2.8)

project(PoissonReconstruction)

set(CMAKE_MACOSX_RPATH 1)

if (Module_PoissonReconstruction)
   message(STATUS "PoissonReconstruction: Building PoissonReconstruction as a Remote VTK Module")
  set(EXECUTABLE_OUTPUT_PATH ${VTK_BINARY_DIR})
message(STATUS "EXECUTABLE_OUTPUT_PATH ${EXECUTABLE_OUTPUT_PATH}")
  set(PoissonReconstruction_BINARY_DIR ${EXECUTABLE_OUTPUT_PATH})
  set( CMAKE_RUNTIME_OUTPUT_DIRECTORY
    ${VTK_BINARY_DIR}/bin
    CACHE PATH "path for all executables"
  )
  set( CMAKE_ARCHIVE_OUTPUT_DIRECTORY
    ${VTK_BINARY_DIR}/lib
    CACHE PATH "path for all libraries"
  )

  SET( CMAKE_LIBRARY_OUTPUT_DIRECTORY
    ${VTK_BINARY_DIR}/lib
    CACHE PATH "path for all libraries"
  )
  find_package(VTK PATHS ${VTK_BINARY_DIR})
  include(${VTK_USE_FILE})
  vtk_module_impl()
else()
  message(STATUS "PoissonReconstruction: Building PoissonReconstruction stand-alone")
  set( CMAKE_RUNTIME_OUTPUT_DIRECTORY
      ${PoissonReconstruction_BINARY_DIR}/bin
        CACHE PATH "path for all executables"
  )
  set( CMAKE_ARCHIVE_OUTPUT_DIRECTORY
    ${PoissonReconstruction_BINARY_DIR}/lib
    CACHE PATH "path for all libraries"
  )

  set( CMAKE_LIBRARY_OUTPUT_DIRECTORY
    ${PoissonReconstruction_BINARY_DIR}/lib
    CACHE PATH "path for all libraries"
  )
  find_package(VTK REQUIRED)
  include(${VTK_USE_FILE})
endif()

if( ${VTK_BUILD_SHARED_LIBS} MATCHES ON )
  set( SHARED_LIBS "SHARED" )
  add_definitions( "-DvtkPoissonReconstruction_EXPORTS" )
  add_definitions( "-DBUILD_SHARED_LIBS" )
else(${VTK_BUILD_SHARED_LIBS} MATCHES ON )
  set( SHARED_LIBS "STATIC" )
endif(${VTK_BUILD_SHARED_LIBS} MATCHES ON )

list(APPEND PoissonReconstruction_INCLUDE_DIRS
  ${PoissonReconstruction_SOURCE_DIR}/source
  ${PoissonReconstruction_BINARY_DIR}
)
include_directories(${PoissonReconstruction_INCLUDE_DIRS})

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

if (NOT Module_PoissonReconstruction)
  set(Poisson_TARGET_FILE ${PoissonReconstruction_BINARY_DIR}/PoissonReconstructionTargets.cmake)
  get_property(PoissonReconstruction_TARGETS GLOBAL PROPERTY PoissonReconstruction_TARGETS)
  export(TARGETS ${PoissonReconstruction_TARGETS} APPEND FILE ${Poisson_TARGET_FILE})
  configure_file(
    ${PoissonReconstruction_SOURCE_DIR}/CMake/PoissonReconstructionConfig.cmake.in
    ${PoissonReconstruction_BINARY_DIR}/PoissonReconstructionConfig.cmake
    @ONLY)
endif()

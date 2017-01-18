configure_file(
  ${PoissonReconstruction_SOURCE_DIR}/source/vtkPoissonReconstructionConfigure.h.in
  ${PoissonReconstruction_BINARY_DIR}/vtkPoissonReconstructionConfigure.h
  @ONLY IMMEDIATE
)

add_library( vtkPoissonReconstruction
  ${SHARED_LIBS}
  vtkPoissonReconstruction.cxx
  Factor.cpp
  Geometry.cpp
  MarchingCubes.cpp
  MultiGridOctest.cpp
  MultiGridOctreeData.cpp
)

target_link_libraries( vtkPoissonReconstruction
  ${VTK_LIBRARIES}
)

install(TARGETS vtkPoissonReconstruction
  RUNTIME DESTINATION bin COMPONENT RuntimeLibraries
  LIBRARY DESTINATION lib COMPONENT RuntimeLibraries
  ARCHIVE DESTINATION lib COMPONENT Development
)
set_property(GLOBAL APPEND PROPERTY PoissonReconstruction_TARGETS vtkPoissonReconstruction)

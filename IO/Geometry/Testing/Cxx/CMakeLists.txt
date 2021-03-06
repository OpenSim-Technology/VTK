# Tests with data
ExternalData_Expand_Arguments(VTKData _
  "DATA{${VTK_TEST_INPUT_DIR}/OpenFOAM/cavity/,REGEX:.*}"
  "DATA{${VTK_TEST_INPUT_DIR}/OpenFOAM/cavity/0/,REGEX:.*}"
  "DATA{${VTK_TEST_INPUT_DIR}/OpenFOAM/cavity/0.5/,REGEX:.*}"
  "DATA{${VTK_TEST_INPUT_DIR}/OpenFOAM/cavity/0.5/uniform/,REGEX:.*}"
  "DATA{${VTK_TEST_INPUT_DIR}/OpenFOAM/cavity/1/,REGEX:.*}"
  "DATA{${VTK_TEST_INPUT_DIR}/OpenFOAM/cavity/1/uniform/,REGEX:.*}"
  "DATA{${VTK_TEST_INPUT_DIR}/OpenFOAM/cavity/1.5/,REGEX:.*}"
  "DATA{${VTK_TEST_INPUT_DIR}/OpenFOAM/cavity/1.5/uniform/,REGEX:.*}"
  "DATA{${VTK_TEST_INPUT_DIR}/OpenFOAM/cavity/2/,REGEX:.*}"
  "DATA{${VTK_TEST_INPUT_DIR}/OpenFOAM/cavity/2/uniform/,REGEX:.*}"
  "DATA{${VTK_TEST_INPUT_DIR}/OpenFOAM/cavity/2.5/,REGEX:.*}"
  "DATA{${VTK_TEST_INPUT_DIR}/OpenFOAM/cavity/2.5/uniform/,REGEX:.*}"
  "DATA{${VTK_TEST_INPUT_DIR}/OpenFOAM/cavity/constant/,REGEX:.*}"
  "DATA{${VTK_TEST_INPUT_DIR}/OpenFOAM/cavity/constant/polyMesh/,REGEX:.*}"
  "DATA{${VTK_TEST_INPUT_DIR}/OpenFOAM/cavity/system/,REGEX:.*}"
  "DATA{${VTK_TEST_INPUT_DIR}/OpenFOAM/64BitFloats/,REGEX:.*}"
  "DATA{${VTK_TEST_INPUT_DIR}/OpenFOAM/64BitFloats/0/,REGEX:.*}"
  "DATA{${VTK_TEST_INPUT_DIR}/OpenFOAM/64BitFloats/constant/,REGEX:.*}"
  "DATA{${VTK_TEST_INPUT_DIR}/OpenFOAM/64BitFloats/constant/polyMesh/,REGEX:.*}"
  "DATA{${VTK_TEST_INPUT_DIR}/OpenFOAM/64BitFloats/system/,REGEX:.*}"
  )
vtk_add_test_cxx(${vtk-module}CxxTests tests
  TestDataObjectIO.cxx,NO_VALID
  TestIncrementalOctreePointLocator.cxx,NO_VALID
  UnstructuredGridCellGradients.cxx
  UnstructuredGridFastGradients.cxx
  UnstructuredGridGradients.cxx
  TestOBJReaderMaterials.cxx,NO_VALID
  TestOBJReaderMultiTexture.cxx,NO_VALID
  TestOBJReaderNormalsTCoords.cxx,NO_VALID
  TestOBJReaderRelative.cxx,NO_VALID
  TestOBJReaderSingleTexture.cxx,NO_VALID
  TestOpenFOAMReader.cxx
  TestOpenFOAMReader64BitFloats.cxx
  TestProStarReader.cxx
  TestTecplotReader.cxx
  TestAMRReadWrite.cxx,NO_VALID
  TestSimplePointsReaderWriter.cxx,NO_VALID
  TestHoudiniPolyDataWriter.cxx,NO_VALID
  UnitTestSTLWriter.cxx,NO_VALID
  )

vtk_add_test_cxx(${vtk-module}CxxTests tests
  TestSTLReaderSinglePatch,TestSTLReader.cxx DATA{${VTK_TEST_INPUT_DIR}/42400-IDGH.stl}
)

vtk_add_test_cxx(${vtk-module}CxxTests tests
  TestSTLReaderMultiplePatches,TestSTLReader.cxx DATA{${VTK_TEST_INPUT_DIR}/multiple_patches.stl}
)

vtk_add_test_cxx(${vtk-module}CxxTests tests
  TestPTSReader,TestPTSReader.cxx DATA{${VTK_TEST_INPUT_DIR}/samplePTS.pts}
)

vtk_add_test_cxx(${vtk-module}CxxTests tests
  TestAVSucdReaderContiguousASCII,TestAVSucdReader.cxx DATA{${VTK_TEST_INPUT_DIR}/cellsnd.ascii.inp}
)
vtk_add_test_cxx(${vtk-module}CxxTests tests
  TestAVSucdReaderNonContiguousASCII,TestAVSucdReader.cxx DATA{${VTK_TEST_INPUT_DIR}/cellsnd.noncontiguous.ascii.inp}
)
vtk_add_test_cxx(${vtk-module}CxxTests tests
  TestAVSucdReaderBinary,TestAVSucdReader.cxx DATA{${VTK_TEST_INPUT_DIR}/cellsnd.bin.inp}
)

set(_known_little_endian FALSE)
if (DEFINED VTK_WORDS_BIGENDIAN)
  if (NOT VTK_WORDS_BIGENDIAN)
    set(_known_little_endian TRUE)
  endif()
endif()
if(VTK_USE_LARGE_DATA AND _known_little_endian)
  # Tell ExternalData to fetch test input at build time.
  ExternalData_Expand_Arguments(VTKData _
    "DATA{${VTK_TEST_INPUT_DIR}/WindBladeReader/,REGEX:.*}"
    "DATA{${VTK_TEST_INPUT_DIR}/WindBladeReader/field/,REGEX:.*}"
    "DATA{${VTK_TEST_INPUT_DIR}/WindBladeReader/turbine/,REGEX:.*}"
    "DATA{${VTK_TEST_INPUT_DIR}/MFIXReader/,REGEX:.*}"
    )

  vtk_add_test_cxx(${vtk-module}CxxTests large_data_tests
    TestWindBladeReader.cxx
    TestMFIXReader.cxx,NO_VALID
    )
  list(APPEND tests
    ${large_data_tests}
    )
endif()

vtk_test_cxx_executable(${vtk-module}CxxTests tests)

# copy mesh files from current source dir to binary dir
configure_file(  ${CMAKE_CURRENT_SOURCE_DIR}/soap.xml
                 ${CMAKE_CURRENT_BINARY_DIR}/soap.xml COPYONLY)
configure_file(  ${CMAKE_CURRENT_SOURCE_DIR}/soap.exo
                 ${CMAKE_CURRENT_BINARY_DIR}/soap.exo COPYONLY)
configure_file(  ${CMAKE_CURRENT_SOURCE_DIR}/amgx.json
                 ${CMAKE_CURRENT_BINARY_DIR}/amgx.json COPYONLY)

set( CONFIG_FILE "soap.xml" )
set( RUN_TIME 0.8 )
set( NUM_RUNS 10 )
set( OUTPUT_DIR "output_data" )

get_filename_component(testDirName ${CMAKE_CURRENT_SOURCE_DIR} NAME)
set( TEST_NAME ${testDirName} )

## create test
add_test( NAME ${TEST_NAME}_timing
          COMMAND ${CMAKE_COMMAND}
          -DANALYZE_EXECUTABLE=${CMAKE_BINARY_DIR}/analyze
          -DCONFIG_FILE=${CONFIG_FILE}
          -DRUN_TIME=${RUN_TIME}
          -DNUM_RUNS=${NUM_RUNS}
          -P ${CMAKE_SOURCE_DIR}/tests/tools/timer.cmake )

## create test
add_test( NAME ${TEST_NAME}_regression
          COMMAND ${CMAKE_COMMAND}
          -DANALYZE_EXECUTABLE=${CMAKE_BINARY_DIR}/analyze
          -DCONFIG_FILE=${CONFIG_FILE}
          -DOUTPUT_DIR=${OUTPUT_DIR}
          -DGOLD_DIR=${CMAKE_CURRENT_SOURCE_DIR}/gold_data
          -DVTKDIFF_CONF=${CMAKE_CURRENT_SOURCE_DIR}/vtk.diff
          -DVTKDIFF=${VTKDIFF}
          -P ${CMAKE_SOURCE_DIR}/tests/tools/compare.cmake )

# copy mesh files from current source dir to binary dir
configure_file(  ${CMAKE_CURRENT_SOURCE_DIR}/bar.xml
                 ${CMAKE_CURRENT_BINARY_DIR}/bar.xml COPYONLY)
configure_file(  ${CMAKE_CURRENT_SOURCE_DIR}/amgx.json
                 ${CMAKE_CURRENT_BINARY_DIR}/amgx.json COPYONLY)
configure_file(  ${CMAKE_CURRENT_SOURCE_DIR}/bar.exo
                 ${CMAKE_CURRENT_BINARY_DIR}/bar.exo COPYONLY)

set( CONFIG_FILE "bar.xml" )
set( RUN_TIME 0.7 )
set( NUM_RUNS 10 )

get_filename_component(testDirName ${CMAKE_CURRENT_SOURCE_DIR} NAME)
set( TEST_NAME ${testDirName} )

## create test
add_test( NAME ${TEST_NAME}
          COMMAND ${CMAKE_COMMAND}
          -DLGR_EXECUTABLE=${LGR_BINARY_DIR}/lgr
          -DCONFIG_FILE=${CONFIG_FILE}
          -DRUN_TIME=${RUN_TIME}
          -DNUM_RUNS=${NUM_RUNS}
          -P ${CMAKE_SOURCE_DIR}/tests/timer.cmake )

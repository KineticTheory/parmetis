
# Create programs.
add_executable(ptest ptest.c io.c adaptgraph.c)
add_executable(mtest mtest.c io.c)
add_executable(parmetis_prog parmetis.c io.c adaptgraph.c)
set_target_properties(parmetis_prog PROPERTIES OUTPUT_NAME parmetis)
add_executable(pometis pometis.c io.c)
# Link to libparmetis.
foreach(prog ptest mtest parmetis_prog pometis)
  target_link_libraries(${prog} parmetis)
  target_include_directories(${prog}
    # source directory or install location
    PUBLIC
      $<BUILD_INTERFACE:${ParMETIS_SOURCE_DIR}/programs>
#      $<BUILD_INTERFACE:${ParMETIS_SOURCE_DIR}/include>
#      $<BUILD_INTERFACE:${METIS_PATH}/include/GKlib>
#      $<BUILD_INTERFACE:${METIS_PATH}/include>
      $<INSTALL_INTERFACE:include>)
endforeach(prog)

install(TARGETS ptest mtest parmetis_prog pometis
  RUNTIME DESTINATION bin)


message(STATUS "Processing libparmetis...")

# Find sources.
file(GLOB parmetis_sources *.c)

# Create libparmetis (depends on libmetis and libmpi)
add_library(parmetis ${parmetis_sources})
target_link_libraries(parmetis ${METIS_LIBRARY} MPI::MPI_C)
if(UNIX)
  target_link_libraries(parmetis m)
endif()
target_include_directories(parmetis
  # source directory or install location
  PUBLIC
    $<BUILD_INTERFACE:${ParMETIS_SOURCE_DIR}/libparmetis>
    $<BUILD_INTERFACE:${ParMETIS_SOURCE_DIR}/include>
    $<BUILD_INTERFACE:${METIS_PATH}/include/GKlib>
    $<BUILD_INTERFACE:${METIS_PATH}/include>
    $<INSTALL_INTERFACE:include>)
set_target_properties(parmetis 
  PROPERTIES 
    WINDOWS_EXPORT_ALL_SYMBOLS ON 
    COMPILE_DEFINITIONS DLL_EXPORT)

# set_target_properties(parmetis PROPERTIES LINK_FLAGS "${MPI_LINK_FLAGS}")

install(TARGETS parmetis
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION lib
  ARCHIVE DESTINATION lib)
install(
  FILES 
    ${ParMETIS_SOURCE_DIR}/libparmetis/struct.h 
    ${ParMETIS_SOURCE_DIR}/libparmetis/rename.h 
    ${ParMETIS_SOURCE_DIR}/libparmetis/proto.h 
    ${ParMETIS_SOURCE_DIR}/libparmetis/parmetislib.h 
    ${ParMETIS_SOURCE_DIR}/libparmetis/macros.h 
    ${ParMETIS_SOURCE_DIR}/libparmetis/defs.h 
  DESTINATION include)

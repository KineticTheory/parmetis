cmake_minimum_required(VERSION 3.9)
project(ParMETIS C)

set(SHARED TRUE CACHE BOOL "build a shared library")

if( NOT EXISTS "${METIS_PATH}/include/metis.h" )
  message( FATAL_ERROR "Metis directory not found.  
  METIS_PATH = ${METIS_PATH}
  Expected include/metis.h to be found at this location." )
else()
  set( METIS_PATH "${METIS_PATH}" CACHE PATH "Metis install location")
endif()

# Symlink or copy ./metis to wherever metis is. This allows files to be
# included from metis/libmetis/.
# if( NOT EXISTS "${PROJECT_SOURCE_DIR}/metis/include/metis.h")
  # if (WIN32)
    # execute_process(COMMAND ${CMAKE_COMMAND} -E copy_directory ${METIS_PATH}
      # metis)
  # else()
    # execute_process(COMMAND ${CMAKE_COMMAND} -E create_symlink ${METIS_PATH}
      # metis)
  # endif()
# endif()

if( NOT EXISTS "${METIS_PATH}/include/GKlib/GKlib.h" )
  message( FATAL_ERROR "GKlib directory not found.  
  METIS_PATH = ${METIS_PATH}
  Expected include/GKlib/GKlib.h to be found at this location." )
else()
  set( GKLIB_PATH "${METIS_PATH}/include/GKlib" CACHE PATH 
    "GKlib install location")
endif()

# Search for MPI.
find_package(MPI)
if(NOT MPI_C_FOUND)
  message(FATAL_ERROR "MPI was not found. Ensure that mpiexec is in your PATH")
endif()

# Extract some setup from Metis/GKlib/GKlibSystem.cmake
# - compiler options
# - extra compile_definitions
list( APPEND CMAKE_MODULE_PATH "${METIS_PATH}/cmake" )
include( GKlibSystem )

# Prepare libraries.
if(SHARED)
  set(ParMETIS_LIBRARY_TYPE SHARED)
  set(BUILD_SHARED_LIBS TRUE CACHE BOOL "STATIC|SHARED libary type.")
else()
  set(ParMETIS_LIBRARY_TYPE STATIC)
  set(BUILD_SHARED_LIBS FALSE CACHE BOOL "STATIC|SHARED libary type.")
endif()

# List of paths that the compiler will search for header files.
# i.e., the -I equivalent
# include_directories(${GKLIB_PATH})
# include_directories(include)
# include_directories(${MPI_INCLUDE_PATH})
# include_directories(${GKLIB_PATH})
# include_directories(${METIS_PATH}/include)

# List of directories that cmake will look for CMakeLists.txt
find_library(METIS_LIBRARY metis PATHS ${METIS_PATH}/lib)

add_subdirectory(include)
add_subdirectory(libparmetis)
add_subdirectory(programs)

# This is for testing during development and is not being distributed
#add_subdirectory(test)

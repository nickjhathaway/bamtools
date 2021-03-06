# ==========================
# BamTools CMakeLists.txt
# (c) 2010 Derek Barnett
#
# top-level
# ==========================

# set project name
project( BamTools )

# Cmake requirements
cmake_minimum_required( VERSION 3.1.0 )

# Force the build directory to be different from source directory
macro( ENSURE_OUT_OF_SOURCE_BUILD MSG )
    string( COMPARE EQUAL "${CMAKE_SOURCE_DIR}" "${CMAKE_BINARY_DIR}" insource )
    get_filename_component( PARENTDIR ${CMAKE_SOURCE_DIR} PATH )
    string( COMPARE EQUAL "${CMAKE_SOURCE_DIR}" "${PARENTDIR}" insourcesubdir )
    IF( insource OR insourcesubdir )
        message( FATAL_ERROR "${MSG}" )
    ENDIF( insource OR insourcesubdir )
endmacro( ENSURE_OUT_OF_SOURCE_BUILD )

set(CMAKE_INSTALL_RPATH "$ORIGIN/../lib/bamtools")

#
set (CMAKE_CXX_STANDARD 11)

# set BamTools version information
set( BamTools_VERSION_MAJOR 2 )
set( BamTools_VERSION_MINOR 5 )
set( BamTools_VERSION_BUILD 2 )

# set our library and executable destination dirs
set( EXECUTABLE_OUTPUT_PATH "${CMAKE_SOURCE_DIR}/bin" )
set( LIBRARY_OUTPUT_PATH    "${CMAKE_SOURCE_DIR}/lib" )

# define compiler flags for all code
set( CMAKE_BUILD_TYPE Release )
add_definitions( -Wall -D_FILE_OFFSET_BITS=64 )

# -----------------------------------------------
# handle platform-/environment-specific defines

# If planning to run in Node.js environment, run:
# cmake -DEnableNodeJS=true
if( EnableNodeJS )
    add_definitions( -DSYSTEM_NODEJS=1 )
endif()

# If running on SunOS
if( "${CMAKE_SYSTEM_NAME}" MATCHES "SunOS" )
    add_definitions( -DSUN_OS )
endif()

# -------------------------------------------

# add our includes root path
include_directories( src )

# list subdirectories to build in
add_subdirectory( src )






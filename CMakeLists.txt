# ==========================
# BamTools CMakeLists.txt
# (c) 2010 Derek Barnett
#
# top-level
# ==========================

cmake_minimum_required( VERSION 3.12...3.31 )

project( BamTools VERSION 2.5.2 LANGUAGES CXX )
set( BamTools_VERSION_BUILD ${BamTools_VERSION_PATCH} )

# default to a Release build
if( NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES )
    set( CMAKE_BUILD_TYPE Release CACHE STRING "Build type" FORCE )
endif()

set( CMAKE_CXX_STANDARD 11 )
set( CMAKE_CXX_STANDARD_REQUIRED ON )
set( CMAKE_POSITION_INDEPENDENT_CODE ON )
set( CMAKE_INSTALL_RPATH "$ORIGIN/../lib/bamtools" )

# compiler flags for all code
if( CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang" )
    add_compile_options( -Wall )
endif()
add_compile_definitions( _FILE_OFFSET_BITS=64 )

# -----------------------------------------------
# platform-/environment-specific defines

# for running in a Node.js environment: cmake -DEnableNodeJS=ON
option( EnableNodeJS "Build for a Node.js environment" OFF )
if( EnableNodeJS )
    add_compile_definitions( SYSTEM_NODEJS=1 )
endif()

if( CMAKE_SYSTEM_NAME MATCHES "SunOS" )
    add_compile_definitions( SUN_OS )
endif()

# -------------------------------------------

add_subdirectory( src )

# ==========================
# BamTools CMakeLists.txt
# (c) 2010 Derek Barnett
#
# src/utils/
# ==========================

# list include paths
include_directories( ${BamTools_SOURCE_DIR}/src/api )

# add compiler definitions 
add_definitions( -DBAMTOOLS_UTILS_LIBRARY ) # (for proper exporting of library symbols)
add_definitions( -fPIC ) # (attempt to force PIC compiling on CentOS, not being set on shared libs by CMake)

# create BamTools utils library
add_library( BamTools-utils STATIC
             bamtools_fasta.cpp
             bamtools_options.cpp
             bamtools_pileup_engine.cpp
             bamtools_utilities.cpp
           )

# link BamTools-utils library with BamTools automatically
target_link_libraries( BamTools-utils BamTools )

# set BamTools library properties
set_target_properties( BamTools-utils PROPERTIES
                       OUTPUT_NAME bamtools-utils
                       PREFIX "lib"
                     )


# use, i.e. don't skip the full RPATH for the build tree
set( CMAKE_SKIP_BUILD_RPATH  FALSE)

# when building, don't use the install RPATH already
# (but later on when installing)
set( CMAKE_BUILD_WITH_INSTALL_RPATH FALSE) 

set( CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib/bamtools")

# add the automatically determined parts of the RPATH
# which point to directories outside the build tree to the install RPATH
set( CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

# the RPATH to be used when installing, but only if it's not a system directory
#LIST(FIND CMAKE_PLATFORM_IMPLICIT_LINK_DIRECTORIES "${CMAKE_INSTALL_PREFIX}/lib/bamtools" isSystemDir)
#IF("${isSystemDir}" STREQUAL "-1")
#   SET(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib/bamtools")
#ENDIF("${isSystemDir}" STREQUAL "-1")


set_target_properties(BamTools-utils PROPERTIES INSTALL_RPATH_USE_LINK_PATH TRUE)


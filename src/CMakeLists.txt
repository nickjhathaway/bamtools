# ==========================
# BamTools CMakeLists.txt
# (c) 2010 Derek Barnett
#
# src/
# ==========================

add_subdirectory( api )
add_subdirectory( third_party )
add_subdirectory( toolkit )
add_subdirectory( utils )

# export shared headers
include( ExportHeader.cmake )
set( SharedIncludeDir "shared" )
ExportHeader( SharedHeaders shared/bamtools_global.h ${SharedIncludeDir} )



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




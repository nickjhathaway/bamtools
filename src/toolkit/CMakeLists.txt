# ==========================
# BamTools CMakeLists.txt
# (c) 2010 Derek Barnett
#
# src/toolkit
# ==========================

# version info for the application
configure_file( bamtools_version.h.in ${CMAKE_CURRENT_BINARY_DIR}/bamtools_version.h )

# main bamtools application
add_executable( bamtools_cmd
                bamtools_convert.cpp
                bamtools_count.cpp
                bamtools_coverage.cpp
                bamtools_filter.cpp
                bamtools_header.cpp
                bamtools_index.cpp
                bamtools_merge.cpp
                bamtools_random.cpp
                bamtools_resolve.cpp
                bamtools_revert.cpp
                bamtools_sort.cpp
                bamtools_split.cpp
                bamtools_stats.cpp
                bamtools.cpp
              )
target_include_directories( bamtools_cmd PRIVATE
                            ${CMAKE_CURRENT_BINARY_DIR}
                            ${BamTools_SOURCE_DIR}/src
                            ${BamTools_SOURCE_DIR}/src/api
                            ${BamTools_SOURCE_DIR}/src/utils )

# jsoncpp, found through its installed CMake package (set CMAKE_PREFIX_PATH to its install directory if needed)
find_package( jsoncpp CONFIG REQUIRED )
if( TARGET JsonCpp::JsonCpp )
    set( JsonCppTarget JsonCpp::JsonCpp )
elseif( TARGET jsoncpp_lib )
    set( JsonCppTarget jsoncpp_lib )
else()
    set( JsonCppTarget jsoncpp_static )
endif()

target_link_libraries( bamtools_cmd PRIVATE BamTools BamTools-utils ${JsonCppTarget} )
set_target_properties( bamtools_cmd PROPERTIES
                       VERSION "${BamTools_VERSION}"
                       OUTPUT_NAME "bamtools"
                       INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib/bamtools"
                       INSTALL_RPATH_USE_LINK_PATH TRUE )

# application install destination
install( TARGETS bamtools_cmd DESTINATION "bin" )

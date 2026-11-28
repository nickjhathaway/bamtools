# ==========================
# BamTools CMakeLists.txt
# (c) 2010 Derek Barnett
#
# src/utils/
# ==========================

# BamTools utils library
add_library( BamTools-utils STATIC
             bamtools_fasta.cpp
             bamtools_options.cpp
             bamtools_pileup_engine.cpp
             bamtools_utilities.cpp
           )
target_compile_definitions( BamTools-utils PRIVATE BAMTOOLS_UTILS_LIBRARY ) # (for proper exporting of library symbols)
target_include_directories( BamTools-utils PUBLIC ${BamTools_SOURCE_DIR}/src/api )
target_link_libraries( BamTools-utils PUBLIC BamTools )
set_target_properties( BamTools-utils PROPERTIES
                       OUTPUT_NAME bamtools-utils
                       PREFIX "lib" )

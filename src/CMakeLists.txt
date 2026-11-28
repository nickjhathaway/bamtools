# ==========================
# BamTools CMakeLists.txt
# (c) 2010 Derek Barnett
#
# src/
# ==========================

add_subdirectory( api )
add_subdirectory( toolkit )
add_subdirectory( utils )

# install shared headers
install( FILES shared/bamtools_global.h DESTINATION "include/bamtools/shared" )

# ==========================
# BamTools CMakeLists.txt
# (c) 2010 Derek Barnett
#
# src/api/
# ==========================

# fetch all internal source files
add_subdirectory( internal )

# make list of all API source files
set( BamToolsAPISources
        BamAlignment.cpp
        BamMultiReader.cpp
        BamReader.cpp
        BamWriter.cpp
        SamHeader.cpp
        SamProgram.cpp
        SamProgramChain.cpp
        SamReadGroup.cpp
        SamReadGroupDictionary.cpp
        SamSequence.cpp
        SamSequenceDictionary.cpp
        ${InternalSources}
)

# compile the API sources once, for both the shared and the static library
add_library( BamTools-objects OBJECT ${BamToolsAPISources} )
target_compile_definitions( BamTools-objects PRIVATE BAMTOOLS_API_LIBRARY ) # (for proper exporting of library symbols)
target_include_directories( BamTools-objects PRIVATE ${BamTools_SOURCE_DIR}/src )

# libraries linked automatically: zlib (and Winsock2, if applicable)
set( APILibs z )
if( WIN32 )
    list( APPEND APILibs ws2_32 )
endif()

# main BamTools API shared and static libraries
add_library( BamTools SHARED $<TARGET_OBJECTS:BamTools-objects> )
set_target_properties( BamTools PROPERTIES
                       SOVERSION "${BamTools_VERSION}"
                       OUTPUT_NAME "bamtools" )

add_library( BamTools-static STATIC $<TARGET_OBJECTS:BamTools-objects> )
set_target_properties( BamTools-static PROPERTIES
                       OUTPUT_NAME "bamtools"
                       PREFIX "lib" )

foreach( lib BamTools BamTools-static )
    target_include_directories( ${lib} PUBLIC
                                $<BUILD_INTERFACE:${BamTools_SOURCE_DIR}/src>
                                $<INSTALL_INTERFACE:include/bamtools> )
    target_link_libraries( ${lib} PUBLIC ${APILibs} )
endforeach()

# library install destinations
install( TARGETS BamTools        LIBRARY DESTINATION "lib/bamtools" RUNTIME DESTINATION "bin" )
install( TARGETS BamTools-static ARCHIVE DESTINATION "lib/bamtools" )

# install API headers
install( FILES
         api_global.h
         BamAlgorithms.h
         BamAlignment.h
         BamAux.h
         BamConstants.h
         BamIndex.h
         BamMultiReader.h
         BamReader.h
         BamWriter.h
         IBamIODevice.h
         SamConstants.h
         SamHeader.h
         SamProgram.h
         SamProgramChain.h
         SamReadGroup.h
         SamReadGroupDictionary.h
         SamSequence.h
         SamSequenceDictionary.h
         DESTINATION "include/bamtools/api" )
install( FILES algorithms/Sort.h DESTINATION "include/bamtools/api/algorithms" )

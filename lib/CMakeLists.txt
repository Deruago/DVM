file(GLOB_RECURSE SOURCE_LIST "${DVM_SOURCE_DIR}/lib/*.cpp")
file(GLOB_RECURSE HEADER_LIST "${DVM_SOURCE_DIR}/include/*.h")

add_library(DVM STATIC ${HEADER_LIST})
target_sources(DVM PRIVATE ${SOURCE_LIST})
target_include_directories(DVM PUBLIC 
    $<BUILD_INTERFACE:${DVM_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

target_compile_features(DVM PUBLIC cxx_std_17)
set_target_properties(DVM PROPERTIES LINKER_LANGUAGE CXX POSITION_INDEPENDENT_CODE ON)

add_library(DVM::DVM ALIAS DVM)

# Install Targets

install(TARGETS DVM
    EXPORT DVM_Exports
    LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}/DVM"
    RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}/DVM"
    ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}/DVM"
    )

export(TARGETS
	DVM
    NAMESPACE DVM::
	FILE "${CMAKE_CURRENT_BINARY_DIR}/DVM_Exports.cmake"
)

install(EXPORT
	DVM_Exports
    NAMESPACE DVM::
	DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/DVM"
)

# Install Headers

install(DIRECTORY ${DVM_SOURCE_DIR}/include/DVM
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    FILES_MATCHING
    PATTERN "*.h"
    PATTERN "LICENSE"
    )
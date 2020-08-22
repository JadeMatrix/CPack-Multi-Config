ADD_EXECUTABLE( executable )
TARGET_SOURCES( executable PRIVATE "src/executable.cpp" )
TARGET_LINK_LIBRARIES( executable PRIVATE library )
SET_TARGET_PROPERTIES(
    executable
    PROPERTIES
        DEBUG_POSTFIX "-debug"
)

INSTALL(
    TARGETS executable
    EXPORT "CPackMultiConfigConfig"
    CONFIGURATIONS Release
    COMPONENT executables-release
    RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}"
)
INSTALL(
    TARGETS executable
    EXPORT "CPackMultiConfigConfig-debug"
    CONFIGURATIONS Debug
    COMPONENT #[[executables-]] debug
    RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}/"
)

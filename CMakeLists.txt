CMAKE_MINIMUM_REQUIRED( VERSION 3.17 )

PROJECT( "CPack Multi-Config" VERSION 1 LANGUAGES CXX )

INCLUDE( GNUInstallDirs )


# Override configurations for this example
SET( CMAKE_CONFIGURATION_TYPES Release Debug )

SET( CMAKE_DEBUG_POSTFIX "d" )


ADD_SUBDIRECTORY( "executable/" )
ADD_SUBDIRECTORY( "library/"    )


INCLUDE( CMakePackageConfigHelpers )
WRITE_BASIC_PACKAGE_VERSION_FILE(
    "${PROJECT_BINARY_DIR}/CPackMultiConfigConfigVersion.cmake"
    COMPATIBILITY AnyNewerVersion
)
INSTALL(
    FILES "${PROJECT_BINARY_DIR}/CPackMultiConfigConfigVersion.cmake"
    DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/CPackMultiConfig/"
    COMPONENT cmake-configs
)
# INSTALL(
#     EXPORT "CPackMultiConfigConfig"
#     NAMESPACE "CPackMultiConfig::"
#     DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/CPackMultiConfig/"
#     COMPONENT cmake-configs
# )
INSTALL(
    EXPORT "CPackMultiConfigConfig"
    # EXPORT "CPackMultiConfigConfig-release"
    # FILE "CPackMultiConfigConfig-release.cmake"
    NAMESPACE "CPackMultiConfig::"
    DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/CPackMultiConfig/"
    CONFIGURATIONS Release
    # COMPONENT release
    # COMPONENT cmake-configs-release
    COMPONENT cmake-configs
)
INSTALL(
    EXPORT "CPackMultiConfigConfig-debug"
    # FILE "CPackMultiConfigConfig-debug.cmake"
    FILE "CPackMultiConfigConfig.cmake"
    NAMESPACE "CPackMultiConfig::"
    DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/CPackMultiConfig/"
    CONFIGURATIONS Debug
    COMPONENT debug
    # COMPONENT cmake-configs-debug
)
# EXPORT( EXPORT "CPackMultiConfigConfig" FILE "CPackMultiConfigConfig.cmake" )


SET( CPACK_VERBATIM_VARIABLES TRUE )
SET( CPACK_GENERATOR "TGZ" )
SET( CPACK_BUILD_CONFIG ${CMAKE_CONFIGURATION_TYPES} )

SET( CPACK_ARCHIVE_COMPONENT_INSTALL ON )

INCLUDE( CPack )

CPACK_ADD_COMPONENT_GROUP(
    release
    DISPLAY_NAME "Release"
    DESCRIPTION "Release executables & libraries"
)
CPACK_ADD_COMPONENT_GROUP(
    devel
    DISPLAY_NAME "Development"
    DESCRIPTION "Development resources (headers, CMake configs, etc.)"
)
CPACK_ADD_COMPONENT_GROUP(
    debug
    DISPLAY_NAME "Debug"
    DESCRIPTION "Debug executables & libraries"
)

CPACK_ADD_COMPONENT(
    #[[executables-]] release
    DISPLAY_NAME "Release binaries"
    REQUIRED
    GROUP release
)
# CPACK_ADD_COMPONENT(
#     executables-release
#     DISPLAY_NAME "Executables (release)"
#     REQUIRED
#     GROUP release
# )
# CPACK_ADD_COMPONENT(
#     libraries-release
#     DISPLAY_NAME "Libraries (release)"
#     REQUIRED
#     GROUP release
# )

CPACK_ADD_COMPONENT(
    public-headers
    DISPLAY_NAME "Public headers"
    REQUIRED
    GROUP devel
)
CPACK_ADD_COMPONENT(
    cmake-configs
    DISPLAY_NAME "CMake package configuration"
    REQUIRED
    GROUP devel
)
# CPACK_ADD_COMPONENT(
#     cmake-configs-release
#     DISPLAY_NAME "CMake package configuration"
#     REQUIRED
#     GROUP devel
# )

CPACK_ADD_COMPONENT(
    #[[executables-]] debug
    DISPLAY_NAME "Debug binaries"
    REQUIRED
    GROUP debug
)
# CPACK_ADD_COMPONENT(
#     executables-debug
#     DISPLAY_NAME "Executables (debug)"
#     REQUIRED
#     GROUP debug
# )
# CPACK_ADD_COMPONENT(
#     libraries-debug
#     DISPLAY_NAME "Libraries (debug)"
#     REQUIRED
#     GROUP debug
# )
# CPACK_ADD_COMPONENT(
#     cmake-configs-debug
#     DISPLAY_NAME "CMake package configuration"
#     REQUIRED
#     GROUP devel
# )

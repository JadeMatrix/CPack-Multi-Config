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
INSTALL(
    EXPORT "CPackMultiConfigConfig"
    NAMESPACE "CPackMultiConfig::"
    DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/CPackMultiConfig/"
    CONFIGURATIONS Release
    COMPONENT cmake-configs #[[-release]]
)
INSTALL(
    EXPORT "CPackMultiConfigConfig-debug"
    # Here is the hack to get CPack to separate out `<config>-debug.cmake` into
    # the `debug` package.  Each target is installed exporting to the normal
    # config under Release and some other "debug" config under Debug.  Then this
    # other "debug" config gets installed with the same (base) filename as the
    # Release config.  This results in a duplicate `<config>.cmake` file in the
    # `debug` package, but the contents are the same as the one in the `devel`
    # package.
    FILE "CPackMultiConfigConfig.cmake"
    NAMESPACE "CPackMultiConfig::"
    DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/CPackMultiConfig/"
    CONFIGURATIONS Debug
    COMPONENT #[[cmake-configs-]] debug
)
# EXPORT( EXPORT "CPackMultiConfigConfig" FILE "CPackMultiConfigConfig.cmake" )


SET( CPACK_VERBATIM_VARIABLES TRUE )
SET( CPACK_GENERATOR "TGZ" CACHE STRING "CPack generators" )
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
    executables-release
    DISPLAY_NAME "Executables (release)"
    REQUIRED
    GROUP release
)
CPACK_ADD_COMPONENT(
    libraries-release
    DISPLAY_NAME "Libraries (release)"
    REQUIRED
    GROUP release
)

CPACK_ADD_COMPONENT(
    public-headers
    DISPLAY_NAME "Public headers"
    REQUIRED
    GROUP devel
)
CPACK_ADD_COMPONENT(
    cmake-configs #[[-release]]
    DISPLAY_NAME "CMake package configuration"
    REQUIRED
    GROUP devel
)

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

ADD_LIBRARY( library SHARED )
TARGET_SOURCES( library PRIVATE "src/library.cpp" )
TARGET_INCLUDE_DIRECTORIES( library
    PUBLIC
        "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/>"
        "$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>"
)
INSTALL(
    DIRECTORY "include/"
    DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}"
    COMPONENT public-headers
    EXCLUDE_FROM_ALL
)

INSTALL(
    TARGETS library
    EXPORT "CPackMultiConfigConfig"
    CONFIGURATIONS Release
    COMPONENT libraries-release
    RUNTIME DESTINATION "${CMAKE_INSTALL_LIBDIR}"
)
INSTALL(
    TARGETS library
    EXPORT "CPackMultiConfigConfig-debug"
    CONFIGURATIONS Debug
    COMPONENT #[[libraries-]] debug
    RUNTIME DESTINATION "${CMAKE_INSTALL_LIBDIR}"
)

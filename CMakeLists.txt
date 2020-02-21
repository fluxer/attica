project(attica)
cmake_minimum_required(VERSION 3.0.0)

# since cmake 2.8.6
set(CMAKE_AUTOMOC ON)
# since cmake 3.0.0
set(CMAKE_AUTOUIC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH  TRUE)

# Used to create CMake config files
include(CMakePackageConfigHelpers)

# Used to set installation paths
include(GNUInstallDirs)

# Set the suffix to avoid conflicts
set(ATTICA_SUFFIX "" CACHE STRING "Project suffix")

# Set the different paths
set(LIB_DESTINATION         "${CMAKE_INSTALL_FULL_LIBDIR}" CACHE PATH "Library directory name")
set(INCLUDE_DESTINATION     "${CMAKE_INSTALL_FULL_INCLUDEDIR}" CACHE PATH "The subdirectory to the header prefix")

find_package(Katie REQUIRED)

set(CMAKE_LIBATTICA_VERSION_MAJOR 0)
set(CMAKE_LIBATTICA_VERSION_MINOR 4)
set(CMAKE_LIBATTICA_VERSION_PATCH 2)
set(ATTICA_LIB_SONAME attica${ATTICA_SUFFIX})
set(ATTICA_LIB_SONAME_CAMEL LibAttica${ATTICA_SUFFIX})
set(ATTICA_LIB_NAMESPACE LibAttica${ATTICA_SUFFIX})
set(CMAKE_LIBATTICA_VERSION_STRING  "${CMAKE_LIBATTICA_VERSION_MAJOR}.${CMAKE_LIBATTICA_VERSION_MINOR}.${CMAKE_LIBATTICA_VERSION_PATCH}")

add_definitions(
    -DQT_NO_CAST_FROM_ASCII
    -DQT_NO_CAST_TO_ASCII
)

include_directories(
    ${CMAKE_CURRENT_BINARY_DIR}/src
    ${CMAKE_CURRENT_SOURCE_DIR}/src
    ${QT_INCLUDES}
)

# Enable static build
option(ATTICA_STATIC_BUILD "Build a static library" Off)

add_subdirectory(src)

# Enable unit testing
if (ENABLE_TESTING)
    enable_testing()
    add_subdirectory(autotests)
    add_subdirectory(tests)
endif()


# Write LibAtticaConfigVersion.cmake
write_basic_package_version_file(
    ${attica_BINARY_DIR}/${ATTICA_LIB_SONAME_CAMEL}ConfigVersion.cmake
    VERSION ${CMAKE_LIBATTICA_VERSION_STRING}
    COMPATIBILITY SameMajorVersion
)

# Create the CMake Config files
configure_package_config_file(
    LibAtticaConfig.cmake.in
    ${attica_BINARY_DIR}/${ATTICA_LIB_SONAME_CAMEL}Config.cmake
    INSTALL_DESTINATION ${LIB_DESTINATION}/cmake/${ATTICA_LIB_SONAME_CAMEL}
    PATH_VARS LIB_DESTINATION INCLUDE_DESTINATION
)

install(
    FILES
    ${attica_BINARY_DIR}/${ATTICA_LIB_SONAME_CAMEL}Config.cmake
    ${attica_BINARY_DIR}/${ATTICA_LIB_SONAME_CAMEL}ConfigVersion.cmake
    DESTINATION ${LIB_DESTINATION}/cmake/${ATTICA_LIB_SONAME_CAMEL}
)

install(
    EXPORT LibAtticaExport
    FILE ${ATTICA_LIB_SONAME_CAMEL}Targets.cmake
    DESTINATION ${LIB_DESTINATION}/cmake/${ATTICA_LIB_SONAME_CAMEL}
    NAMESPACE ${ATTICA_LIB_NAMESPACE}::
)

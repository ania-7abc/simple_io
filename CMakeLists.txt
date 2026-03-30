cmake_minimum_required(VERSION 3.14)
project(simple_io VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(simple_io INTERFACE)
set_target_properties(simple_io PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION 1
)
target_include_directories(simple_io INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

install(TARGETS simple_io EXPORT simple_ioTargets
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
)
install(EXPORT simple_ioTargets
    FILE simple_ioTargets.cmake
    NAMESPACE ania-7abc::
    DESTINATION lib/cmake/simple_io
)

include(CMakePackageConfigHelpers)
configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake/simple_ioConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/simple_ioConfig.cmake
    INSTALL_DESTINATION lib/cmake/simple_io
)
write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/simple_ioConfigVersion.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion
)
install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/simple_ioConfig.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/simple_ioConfigVersion.cmake
    DESTINATION lib/cmake/simple_io
)

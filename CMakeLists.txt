cmake_minimum_required(VERSION 3.23)

project(Qing
    VERSION 0.1.0
    DESCRIPTION "A fast, cross-platform application launcher"
    LANGUAGES CXX
)

include(GNUInstallDirs)
include(CTest)

option(QING_BUILD_TESTS "Build Qing tests" OFF)
option(QING_ENABLE_WARNINGS "Enable strict compiler warnings" ON)

add_executable(Qing
    main.cpp
)

add_executable(Qing::Qing ALIAS Qing)

target_compile_features(Qing PRIVATE cxx_std_17)

set_target_properties(Qing PROPERTIES
    CXX_EXTENSIONS OFF
    VERSION ${PROJECT_VERSION}
    SOVERSION ${PROJECT_VERSION_MAJOR}
    EXPORT_NAME Qing
)

if(QING_ENABLE_WARNINGS)
    if(MSVC)
        target_compile_options(Qing PRIVATE /W4 /permissive-)
    else()
        target_compile_options(Qing PRIVATE -Wall -Wextra -Wpedantic)
    endif()
endif()

if(WIN32)
    target_compile_definitions(Qing PRIVATE NOMINMAX UNICODE _UNICODE)
endif()

install(TARGETS Qing
    EXPORT QingTargets
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

install(EXPORT QingTargets
    FILE QingTargets.cmake
    NAMESPACE Qing::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/Qing
)

if(QING_BUILD_TESTS AND EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/tests/CMakeLists.txt")
    add_subdirectory(tests)
elseif(QING_BUILD_TESTS)
    message(WARNING "QING_BUILD_TESTS is enabled, but the tests directory does not exist")
endif()

message(STATUS "Configuring ${PROJECT_NAME} ${PROJECT_VERSION}")
message(STATUS "  Build type: ${CMAKE_BUILD_TYPE}")
message(STATUS "  Tests:      ${QING_BUILD_TESTS}")

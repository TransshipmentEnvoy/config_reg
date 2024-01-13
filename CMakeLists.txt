cmake_minimum_required (VERSION 3.21..3.27)

# sanity check
if(CMAKE_BINARY_DIR STREQUAL CMAKE_SOURCE_DIR)
    message(FATAL_ERROR "Building in-source is not supported! Create a build dir and remove ${CMAKE_SOURCE_DIR}/CMakeCache.txt")
endif()

# project 
project(config_reg)

# import cmake module
enable_language(CXX)

include(GNUInstallDirs)
include(CMakePrintHelpers)

# dependency
#find_package(CLI11 REQUIRED)
#message(STATUS "${PROJECT_NAME}: use cli11: ${CLI11_DIR}")

find_package(nlohmann_json 3.11.0 REQUIRED)
message(STATUS "${PROJECT_NAME}: use nlohmann_json: ${nlohmann_json_DIR}")

## dep: unittest
find_package(Catch2 3 REQUIRED)
message(STATUS "${PROJECT_NAME}: use Catch2: ${Catch2_DIR}")

# library
add_library(config_reg INTERFACE)
target_include_directories(config_reg INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)
target_link_libraries(config_reg INTERFACE
    CLI11::CLI11
    nlohmann_json::nlohmann_json
)

# install
install(TARGETS config_reg
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

# unittest
option(CONFIG_REG_WITH_TEST "config_reg build test" ON)
if(CONFIG_REG_WITH_TEST)
    enable_testing()
    add_subdirectory(test)
endif()
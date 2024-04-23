# @file CMakeLists.txt
# 
# CMake files for XMLFramework

# Can be run by any version of CMake >= this
cmake_minimum_required(VERSION 3.25)

# Project info
project(XMLFramework)

# Common bin directory for all executables
get_property(is_multi_config GLOBAL PROPERTY GENERATOR_IS_MULTI_CONFIG)
if(NOT is_multi_config)
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
endif()

# Build Library
add_subdirectory(Library)

# Build Toolkit
add_subdirectory(Toolkit)

# Build Framework
add_subdirectory(Framework)

# Build Applications
add_subdirectory(Apps)

# View @TODOs
add_custom_target(todo
        COMMENT "View remaining @TODOs"
        COMMAND git grep -h "@TODO" -- ':!*.txt' | sed -e 's/^[ ]*//' | sort | uniq --count | sort --reverse --sort=numeric
        USES_TERMINAL
        WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
)

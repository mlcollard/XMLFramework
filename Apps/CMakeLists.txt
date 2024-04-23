# @file CMakeLists.txt
# 
# CMake build files for XMLFramework/Apps
#
# Place any custom app run commands in this build file

# Install CLI11 internally

# include(FetchContent)
# FetchContent_Declare(cli11
#   URL https://github.com/CLIUtils/CLI11/releases/download/v2.3.2/CLI11.hpp
#   DOWNLOAD_NO_EXTRACT TRUE
# )
# FetchContent_MakeAvailable(cli11)
# add_library(cli11 INTERFACE IMPORTED)
# target_include_directories(cli11 SYSTEM INTERFACE ${cli11_SOURCE_DIR})

include(FetchContent)
FetchContent_Declare(
    cli11_proj
    QUIET
    GIT_REPOSITORY https://github.com/CLIUtils/CLI11.git
    GIT_TAG v2.4.1
)

FetchContent_MakeAvailable(cli11_proj)

# Assume each .cpp file is a separate application
file(GLOB APPLICATIONS "*.cpp")

# Generate executable for each application
foreach(APP ${APPLICATIONS})

    # Use basename (no path, no extension) of .cpp file for application name
    get_filename_component(APPBASENAME "${APP}" NAME_WLE)

    # Application is based on individual .cpp file only
    add_executable(${APPBASENAME} ${APPBASENAME}.cpp)
    target_compile_features(${APPBASENAME} PRIVATE cxx_std_17)
    set_target_properties(${APPBASENAME} PROPERTIES
        CXX_STANDARD_REQUIRED ON
        CXX_EXTENSIONS OFF
    )
    target_compile_options(${APPBASENAME} PRIVATE
         $<$<OR:$<CXX_COMPILER_ID:Clang>,$<CXX_COMPILER_ID:AppleClang>,$<CXX_COMPILER_ID:GNU>>: -Wall>
         $<$<CXX_COMPILER_ID:MSVC>: /W4>
    )

    # Libraries for entire Framework
    target_link_libraries(${APPBASENAME} PRIVATE CLI11::CLI11 PUBLIC XMF::Framework XMF::Toolkit XMF::Library)

endforeach()

# Run command-line client example
configure_file(fragment.cpp.xml ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/fragment.cpp.xml COPYONLY)
add_custom_target(run COMMENT "Run example command-line client xmlpipeline"
                      COMMAND $<TARGET_FILE:xmlpipeline> --xpath //src:unit fragment.cpp.xml
                      DEPENDS xmlpipeline
                      USES_TERMINAL
                      WORKING_DIRECTORY ${CMAKE_RUNTIME_OUTPUT_DIRECTORY})

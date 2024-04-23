# @file CMakeLists.txt
#
# CMake files for XMLFramework/Toolkit

# Create an object library for the Toolkit
add_library(XMLToolkit STATIC)
target_compile_features(XMLToolkit PRIVATE cxx_std_17)
set_target_properties(XMLToolkit PROPERTIES
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
)
target_compile_options(XMLToolkit PRIVATE
     $<$<OR:$<CXX_COMPILER_ID:Clang>,$<CXX_COMPILER_ID:AppleClang>,$<CXX_COMPILER_ID:GNU>>: -Wall>
     $<$<CXX_COMPILER_ID:MSVC>: /W4>
)

# Set the static library filename
set_target_properties(XMLToolkit PROPERTIES OUTPUT_NAME xmlframework_toolkit)

# Find the source files
file(GLOB SOURCES "src/*.cpp")

# Add the sources to the Toolkit
target_sources(XMLToolkit PRIVATE ${SOURCES})

# Add the include files for using the Toolkit
target_include_directories(XMLToolkit PUBLIC include)

# Add the libraries used by the Toolkit and applications using the Toolkit
target_link_libraries(XMLToolkit PUBLIC XMF::Library)

# Create the external Toolkit library name
add_library(XMF::Toolkit ALIAS XMLToolkit)

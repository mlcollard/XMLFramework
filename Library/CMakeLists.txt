# @file CMakeLists.txt
#
# CMake files for XMLLibrary

# Create an object Library for the Library
add_library(XMLLibrary STATIC)
target_compile_features(XMLLibrary PRIVATE cxx_std_17)
set_target_properties(XMLLibrary PROPERTIES
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
)
target_compile_options(XMLLibrary PRIVATE
     $<$<OR:$<CXX_COMPILER_ID:Clang>,$<CXX_COMPILER_ID:AppleClang>,$<CXX_COMPILER_ID:GNU>>: -Wall>
     $<$<CXX_COMPILER_ID:MSVC>: /W4>
)

# Set the output library filename
set_target_properties(XMLLibrary PROPERTIES OUTPUT_NAME xmlframework_library)

# Find the .cpp files
file(GLOB SOURCES "src/*.cpp")

# Add the sources to the Library
target_sources(XMLLibrary PRIVATE ${SOURCES})

# Add the include files for using the Library
target_include_directories(XMLLibrary PUBLIC include)

# Create the external Library library name
add_library(XMF::Library ALIAS XMLLibrary)

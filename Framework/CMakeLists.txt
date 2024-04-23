# @file CMakeLists.txt
#
# CMake files for XMLFramework/Framework

# Create an object library for the Framework
add_library(XMLFramework OBJECT)
target_compile_features(XMLFramework PRIVATE cxx_std_17)
set_target_properties(XMLFramework PROPERTIES
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
)
target_compile_options(XMLFramework PRIVATE
     $<$<OR:$<CXX_COMPILER_ID:Clang>,$<CXX_COMPILER_ID:AppleClang>,$<CXX_COMPILER_ID:GNU>>: -Wall>
     $<$<CXX_COMPILER_ID:MSVC>: /W4>
)

# Find .cpp files
file(GLOB SOURCES "src/*.cpp")

# Add the sources to the Framework
target_sources(XMLFramework PRIVATE ${SOURCES})

# Add the include files
target_include_directories(XMLFramework PUBLIC include)

# Add the libraries used by the Framework and applications using the Framework
target_link_libraries(XMLFramework PUBLIC XMF::Toolkit XMF::Library)

# Create the external framework library name
add_library(XMF::Framework ALIAS XMLFramework)

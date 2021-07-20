cmake_minimum_required(VERSION 3.14)
project(functional_vector)

# GoogleTest requires at least C++11
set(CMAKE_CXX_STANDARD 17)

# Caching googletest source and libs
include(FetchContent)
FetchContent_Declare(
  googletest
  URL https://github.com/google/googletest/archive/8d51ffdfab10b3fba636ae69bc03da4b54f8c235.zip
)

# For Windows: Prevent overriding the parent project's compiler/linker settings
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)

# Set compiler settings based on the current platform
if(CMAKE_HOST_UNIX)
    add_definitions(-std=c++17 -w -fpermissive -Wshadow -Wunused -Woverloaded-virtual -Wnon-virtual-dtor -Werror)
    set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -w -Wshadow -Wunused -Woverloaded-virtual -Wnon-virtual-dtor -Werror")
else()
    set(CMAKE_CXX_FLAGS "/Ox /Ob2 /Oi /Ot /Oy /GT /GL /W4")
    set(CMAKE_SHARED_LINKER_FLAGS_RELEASE "${CMAKE_SHARED_LINKER_FLAGS_RELEASE} /OPT:REF /OPT:ICF" )

    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/compile_results)
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/compile_results)
endif()


# Turn on the ability to create folders to organize projects
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

# Include the testing subdirectory
add_subdirectory(tests)
set (LIBNAME functional_vector)

# Properties->C/C++->General->Additional Include Directories
include_directories ("../include")

# Collect files without having to explicitly list each header and source file
file (GLOB HEADERS
	  "../include/*.h"
	  "../include/*.hpp")

file (GLOB SOURCES
      "*.cpp"
      "*.cc")

# Create named folders for the sources within the project
source_group("include" FILES ${HEADERS})
source_group("src" FILES ${SOURCES})

# Set Properties->General->Configuration Type to Dynamic Library (.dll/.so/.dylib)
add_library(${LIBNAME} SHARED ${HEADERS} ${SOURCES})

if(CMAKE_HOST_UNIX AND NOT CMAKE_HOST_APPLE)
target_link_libraries(${LIBNAME} PUBLIC tbb)
endif()

# creates preprocessor definition used for library exports
add_compile_definitions("FUNCTIONAL_VECTOR_EXPORTS")

set_target_properties(${LIBNAME} PROPERTIES 
	OUTPUT_NAME ${LIBNAME}
	FOLDER "lib")

# Adds logic to install the library to the destination directory
install (TARGETS ${LIBNAME}
		 RUNTIME DESTINATION ${PROJECT_BINARY_DIR}/bin)

cmake_minimum_required (VERSION 2.6)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

find_package(OpenGL REQUIRED)

include_directories(include)
include_directories(lib/glfw/include)

add_definitions( -D_DEBUG )

add_library(gl3w STATIC lib/gl3w.c)
add_library(LoadShaders STATIC lib/LoadShaders.cpp)

set(COMMON_LIBS ${OPENGL_LIBRARIES} glfw ${GLFW_LIBRARIES} GL gl3w LoadShaders)
set(COMMON_HEADERS include/vgl.h)

set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
add_subdirectory(lib/glfw)

set(SOURCES
	triangles
)

file(COPY src/shaders DESTINATION ${CMAKE_BINARY_DIR}/bin)

foreach(SOURCE ${SOURCES})
	add_executable(${SOURCE} src/${SOURCE}.cpp ${COMMON_HEADERS})
	target_link_libraries(${SOURCE} ${COMMON_LIBS})
endforeach(SOURCE)
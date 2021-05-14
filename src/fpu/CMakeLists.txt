cmake_minimum_required(VERSION 3.0)

# C and C++ sources are freely mixed.
set(SOURCES fpu.cpp)

add_library(fpu STATIC ${SOURCES})
target_include_directories(fpu PRIVATE ${CMAKE_SOURCE_DIR}/include ${CMAKE_BINARY_DIR})


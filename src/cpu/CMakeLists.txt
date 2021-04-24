cmake_minimum_required(VERSION 3.0)

# C and C++ sources are freely mixed.
set(SOURCES
  callback.cpp)

add_library(cpu STATIC ${SOURCES})
target_include_directories(cpu PRIVATE ${CMAKE_SOURCE_DIR}/include ${CMAKE_BINARY_DIR})


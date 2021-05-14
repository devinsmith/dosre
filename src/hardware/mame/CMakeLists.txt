cmake_minimum_required(VERSION 3.0)

# C and C++ sources are freely mixed.
set(SOURCES saa1099.cpp sn76496.cpp)

add_library(mame STATIC ${SOURCES})
target_include_directories(mame PRIVATE ${CMAKE_SOURCE_DIR}/include ${CMAKE_BINARY_DIR})


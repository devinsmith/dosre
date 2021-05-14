cmake_minimum_required(VERSION 3.0)

# C and C++ sources are freely mixed.
set(SOURCES cross.cpp  messages.cpp  programs.cpp  setup.cpp  support.cpp)

add_library(misc STATIC ${SOURCES})
target_include_directories(misc PRIVATE ${CMAKE_SOURCE_DIR}/include ${CMAKE_BINARY_DIR})


cmake_minimum_required(VERSION 3.0)

# C and C++ sources are freely mixed.
set(SOURCES gui_tk.cpp)

add_library(gui_tk STATIC ${SOURCES})
target_include_directories(gui_tk PRIVATE ${CMAKE_SOURCE_DIR}/include ${CMAKE_BINARY_DIR})


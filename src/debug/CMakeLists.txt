cmake_minimum_required(VERSION 3.0)

# C and C++ sources are freely mixed.
set(SOURCES debug.cpp debug_gui.cpp debug_disasm.cpp
  debug_win32.cpp)

add_library(debuglib STATIC ${SOURCES})
target_include_directories(debuglib PRIVATE ${CMAKE_SOURCE_DIR}/include ${CMAKE_BINARY_DIR})


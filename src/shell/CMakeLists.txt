cmake_minimum_required(VERSION 3.0)

# C and C++ sources are freely mixed.
set(SOURCES shell_batch.cpp  shell_cmds.cpp  shell.cpp  shell_misc.cpp)

add_library(shell STATIC ${SOURCES})
target_include_directories(shell PRIVATE ${CMAKE_SOURCE_DIR}/include ${CMAKE_BINARY_DIR})


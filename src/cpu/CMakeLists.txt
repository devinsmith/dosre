cmake_minimum_required(VERSION 3.0)

# C and C++ sources are freely mixed.
set(SOURCES
  callback.cpp
  cpu.cpp
  flags.cpp
  modrm.cpp
  core_full.cpp
  paging.cpp
  core_normal.cpp
  core_simple.cpp
  core_prefetch.cpp
  core_dynrec.cpp)

add_library(cpu STATIC ${SOURCES})
target_include_directories(cpu PRIVATE ${CMAKE_SOURCE_DIR}/include ${CMAKE_BINARY_DIR})


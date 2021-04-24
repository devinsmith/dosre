cmake_minimum_required(VERSION 3.0)

project(dosre)
set(TARGET_NAME dosre)

add_subdirectory(cpu)

# C and C++ sources are freely mixed.
set(SOURCES
  dosbox.cpp)



add_executable(${TARGET_NAME} ${SOURCES} ${HEADERS})
target_include_directories(${TARGET_NAME} PRIVATE ${CMAKE_SOURCE_DIR}/include ${CMAKE_BINARY_DIR})
target_link_libraries(${TARGET_NAME} PRIVATE cpu)

#if(MSVC)
#  target_compile_options(${TARGET_NAME} PRIVATE /W4 /WX)
#else()
#  target_compile_options(${TARGET_NAME} PRIVATE -Wall -Werror)
#endif()


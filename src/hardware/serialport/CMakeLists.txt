cmake_minimum_required(VERSION 3.0)

set(TARGET_NAME serialport)

# C and C++ sources are freely mixed.
set(SOURCES directserial.cpp  libserial.cpp  serialdummy.cpp  serialport.cpp)

add_library(${TARGET_NAME} STATIC ${SOURCES})
target_include_directories(${TARGET_NAME} PRIVATE ${CMAKE_SOURCE_DIR}/include ${CMAKE_BINARY_DIR}
  ${SDL_INCLUDE_DIR})


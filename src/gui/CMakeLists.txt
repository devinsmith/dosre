cmake_minimum_required(VERSION 3.0)

set(TARGET_NAME gui)

# C and C++ sources are freely mixed.
set(SOURCES midi.cpp render.cpp  render_scalers.cpp  sdl_gui.cpp  sdlmain.cpp  sdl_mapper.cpp)

add_library(${TARGET_NAME} STATIC ${SOURCES})
target_include_directories(${TARGET_NAME} PRIVATE ${CMAKE_SOURCE_DIR}/include ${CMAKE_BINARY_DIR}
  ${SDL_INCLUDE_DIR})


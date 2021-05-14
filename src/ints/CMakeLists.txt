cmake_minimum_required(VERSION 3.0)

set(TARGET_NAME ints)

# C and C++ sources are freely mixed.
set(SOURCES
  bios.cpp bios_keyboard.cpp int10_char.cpp int10_memory.cpp int10_modes.cpp
  int10_put_pixel.cpp int10_video_state.cpp mouse.cpp bios_disk.cpp ems.cpp
  int10.cpp int10_misc.cpp int10_pal.cpp int10_vesa.cpp int10_vptable.cpp xms.cpp)


add_library(${TARGET_NAME} STATIC ${SOURCES})
target_include_directories(${TARGET_NAME} PRIVATE ${CMAKE_SOURCE_DIR}/include ${CMAKE_BINARY_DIR}
  ${SDL_INCLUDE_DIR})


cmake_minimum_required(VERSION 3.0)

add_subdirectory(mame)
add_subdirectory(serialport)

set(TARGET_NAME hardware)

# C and C++ sources are freely mixed.
set(SOURCES
  adlib.cpp disney.cpp gus.cpp joystick.cpp mixer.cpp pcspeaker.cpp
  tandy_sound.cpp vga.cpp vga_draw.cpp vga_misc.cpp vga_s3.cpp vga_xga.cpp
  cmos.cpp dma.cpp hardware.cpp keyboard.cpp mpu401.cpp pic.cpp timer.cpp
  vga_crtc.cpp vga_gfx.cpp vga_other.cpp vga_seq.cpp dbopl.cpp gameblaster.cpp
  iohandler.cpp memory.cpp opl.cpp sblaster.cpp vga_attr.cpp vga_dac.cpp
  vga_memory.cpp vga_paradise.cpp vga_tseng.cpp)

add_library(hardware STATIC ${SOURCES})
target_include_directories(${TARGET_NAME} PRIVATE ${CMAKE_SOURCE_DIR}/include ${CMAKE_BINARY_DIR}
  ${SDL_INCLUDE_DIR})


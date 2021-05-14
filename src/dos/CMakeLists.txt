cmake_minimum_required(VERSION 3.0)

set(TARGET_NAME dos)

# C and C++ sources are freely mixed.
set(SOURCES
  cdrom_aspi_win32.cpp cdrom_image.cpp cdrom_ioctl_os2.cpp dos_classes.cpp
  dos_devices.cpp dos_files.cpp dos_keyboard_layout.cpp dos_misc.cpp
  dos_programs.cpp drive_cache.cpp drive_iso.cpp drives.cpp
  cdrom.cpp cdrom_ioctl_linux.cpp cdrom_ioctl_win32.cpp dos.cpp
  dos_execute.cpp dos_ioctl.cpp dos_memory.cpp dos_mscdex.cpp
  dos_tables.cpp drive_fat.cpp drive_local.cpp drive_virtual.cpp)

add_library(${TARGET_NAME} STATIC ${SOURCES})
target_include_directories(${TARGET_NAME} PRIVATE ${CMAKE_SOURCE_DIR}/include ${CMAKE_BINARY_DIR}
  ${SDL_INCLUDE_DIR})


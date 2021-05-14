cmake_minimum_required(VERSION 3.0)

project(dosre)
set(TARGET_NAME dosre)

add_subdirectory(cpu)
add_subdirectory(debug)
add_subdirectory(dos)
add_subdirectory(fpu)
add_subdirectory(gui)
add_subdirectory(hardware)
add_subdirectory(libs)
add_subdirectory(ints)
add_subdirectory(misc)
add_subdirectory(shell)

# C and C++ sources are freely mixed.
set(SOURCES dosbox.cpp)

set(LINK_LIBRARIES ${SDL_LIBRARY})
if (ALSA_FOUND)
  list(APPEND LINK_LIBRARIES ${ALSA_LIBRARY})
endif()

if (X11_FOUND)
  list(APPEND LINK_LIBRARIES ${X11_LIBRARIES})
endif()

if (OPENGL_FOUND)
  list(APPEND LINK_LIBRARIES ${OPENGL_gl_LIBRARY})
endif()

if (USE_SNDIO)
  list(APPEND LINK_LIBRARIES sndio)
endif()

add_executable(${TARGET_NAME} ${SOURCES} ${HEADERS})
target_include_directories(${TARGET_NAME} PRIVATE ${CMAKE_SOURCE_DIR}/include ${CMAKE_BINARY_DIR}
  ${SDL_INCLUDE_DIR})
target_link_libraries(${TARGET_NAME} PRIVATE cpu debuglib dos fpu gui hardware mame serialport gui_tk ints
  misc shell ${LINK_LIBRARIES})

#if(MSVC)
#  target_compile_options(${TARGET_NAME} PRIVATE /W4 /WX)
#else()
#  target_compile_options(${TARGET_NAME} PRIVATE -Wall -Werror)
#endif()


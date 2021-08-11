cmake_minimum_required(VERSION 3.7.2)
project(dosre)

set(PACKAGE dosre)
set(VERSION 0.01)

# Prefer OpenGL old version
set (OpenGL_GL_PREFERENCE LEGACY)

# TODO: Need to get rid of this!
if (POLICY CMP0075)
  cmake_policy(SET CMP0075 NEW)
endif()

find_package(SDL REQUIRED)
find_package(SDL_sound)
find_package(OpenGL)
find_package(PNG)
find_package(X11)
find_package(ALSA)
find_package(Curses)

include(CheckTypeSize)
include(CheckIncludeFileCXX)
include(CheckCXXSourceCompiles)
include(CheckCXXSymbolExists)
include(CMakeDependentOption)
include(CheckFunctionExists)

check_type_size("int *" SIZEOF_INT_P)
check_type_size("unsigned char" SIZEOF_UNSIGNED_CHAR)
check_type_size("unsigned int" SIZEOF_UNSIGNED_INT)
check_type_size("unsigned long" SIZEOF_UNSIGNED_LONG)
check_type_size("unsigned long long" SIZEOF_UNSIGNED_LONG_LONG)
check_type_size("unsigned short" SIZEOF_UNSIGNED_SHORT)

# always_inline
check_cxx_source_compiles("inline void __attribute__((always_inline)) test(){}
int main() { test(); return 0; }" C_ATTRIBUTE_ALWAYS_INLINE)

# fastcall
check_cxx_source_compiles("inline void __attribute__((fastcall)) test(){}
int main() { test(); return 0; }" C_ATTRIBUTE_FASTCALL)

option(C_CORE_INLINE "Enable inlined memory handling in CPU Core" OFF)
option(C_DEBUG "Enable internal debugger, requires libcurses" OFF)
cmake_dependent_option(C_HEAVY_DEBUG "Enable heavy debugging, also have to enable C_DEBUG" OFF "C_DEBUG" OFF)
option(C_DIRECTSERIAL "Enable serial passthrough support" ON)
option(C_DYNREC "Enable dynamic recompiling CPU Core" ON)
option(C_FPU "Enable FPU emulation" ON)
cmake_dependent_option(C_SSHOT "Enable screenshots, requires libpng" OFF "PNG_FOUND" OFF)

# attribute((packed))
check_cxx_source_compiles("typedef struct { } __attribute__((packed)) junk;
int main() { return 0; }" C_HAS_ATTRIBUTE)

# __builtin_expect
check_cxx_source_compiles("int main() { int x = 10; if (__builtin_expect((x==1), 0)); return 0; }"
  C_HAS_BUILTIN_EXPECT)

check_cxx_symbol_exists(mprotect "sys/mman.h" C_HAVE_MPROTECT)

if (OPENGL_FOUND)
  set(C_OPENGL ON)
endif()

if (SDL_SOUND_FOUND)
  set(C_SDL_SOUND ON)
endif()

# setpriority
check_cxx_source_compiles("#include <sys/resource.h>
int main() { return setpriority(PRIO_PROCESS, 0, PRIO_MIN+PRIO_MAX); }"
  C_SET_PRIORITY)

string(TOUPPER ${CMAKE_HOST_SYSTEM_PROCESSOR} UPPERCASE_SYSTEM_PROCESSOR)
if (UPPERCASE_SYSTEM_PROCESSOR MATCHES "(X64|X86_64|AMD64)")
  set(C_UNALIGNED_MEMORY ON)
elseif(UPPSERCASE_SYSTEM_PROCESSOR MATCHES "(I[3-6]86|X86)")
  set(C_UNALIGNED_MEMORY ON)
else()
  set(C_UNALIGNED_MEMORY OFF)
endif()

if (X11_Xkb_FOUND)
  set(C_X11_XKB ON)
endif()

# check if our math library has powf
if(NOT POWF_FUNCTION_EXISTS AND NOT NEED_LINKING_AGAINST_LIBM)
  CHECK_FUNCTION_EXISTS(powf POWF_FUNCTION_EXISTS)
  if(NOT POWF_FUNCTION_EXISTS)
    unset(POWF_FUNCTION_EXISTS CACHE)
      list(APPEND CMAKE_REQUIRED_LIBRARIES m)
      CHECK_FUNCTION_EXISTS(powf POWF_FUNCTION_EXISTS)
      if(POWF_FUNCTION_EXISTS)
          set(NEED_LINKING_AGAINST_LIBM True CACHE BOOL "" FORCE)
      else()
          message(FATAL_ERROR "Failed making the powf() function available")
      endif()
  endif()
endif()

if(NOT POWF_FUNCTION_EXISTS)
  set(DB_HAVE_NO_POWF ON)
endif()

# dirent.d_type
check_cxx_source_compiles("#include <sys/types.h>
#include <dirent.h>

int main() {
struct dirent d_test;
d_test.d_type = 0;
return 0;
}" DIRENT_HAS_D_TYPE)

# environ checks
check_cxx_source_compiles("#include <stdio.h>
extern char **environ;
int main() {
  char **s = environ;

  return 0;
}" ENVIRON_LINKED)

if (ENVIRON_LINKED)
  set(ENVIRON_INCLUDED ON)
endif()

if (ALSA_FOUND)
  set(HAVE_ALSA ON)
endif()

check_include_file_cxx(ddraw.h HAVE_DDRAW_H)
check_include_file_cxx(inttypes.h HAVE_INTTYPES_H)
check_include_file_cxx(memory.h HAVE_MEMORY_H)
check_include_file_cxx(netinet/in.h HAVE_NETINET_IN_H)
check_include_file_cxx(pwd.h HAVE_PWD_H)

check_cxx_source_compiles("#include <limits.h>
#include <stdlib.h>

int main() {
  char buf[PATH_MAX];
  char *res = realpath(\"file.c\", buf);
  return 0;
  }" HAVE_REALPATH)

check_include_file_cxx(stdint.h HAVE_STDINT_H)
check_include_file_cxx(stdlib.h HAVE_STDLIB_H)
check_include_file_cxx(strings.h HAVE_STRINGS_H)
check_include_file_cxx(string.h HAVE_STRING_H)
check_include_file_cxx(sys/socket.h HAVE_SYS_SOCKET_H)
check_include_file_cxx(sys/stat.h HAVE_SYS_STAT_H)
check_include_file_cxx(sys/types.h HAVE_SYS_TYPES_H)
check_include_file_cxx(unistd.h HAVE_UNISTD_H)
check_include_file_cxx(sndio.h HAVE_SNDIO_H)
cmake_dependent_option(USE_SNDIO "Enable sndio" OFF "HAVE_SNDIO_H" OFF)

# There is no good way to detect the platform that this project runs on
# and platform/OS specific checks are not really even a good idea anyway.
# We need to check for individual features like we do above.
# The code base has various #ifdef BSD || MACOSX || LINUX which is not a
# great idea, and we should work on removing them over time.
# For now, set LINUX ON until we can remove this.
set(LINUX ON)
#set(BSD ON)
#set(MACOSX ON)

set(C_DEBUG ON)
set(C_HEAVY_DEBUG ON)

configure_file(config.h.in config.h)

add_subdirectory(src)

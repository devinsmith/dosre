cmake_minimum_required(VERSION 3.8)
project(dosre)

find_package(SDL REQUIRED)

include(CheckTypeSize)
include(CheckCXXSourceCompiles)
include(CheckCXXSymbolExists)
include(CMakeDependentOption)

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

# attribute((packed))
check_cxx_source_compiles("typedef struct { } __attribute__((packed)) junk;
int main() { return 0; }" C_HAS_ATTRIBUTE)

# __builtin_expect
check_cxx_source_compiles("int main() { int x = 10; if (__builtin_expect((x==1), 0)); return 0; }"
  C_HAS_BUILTIN_EXPECT)

check_cxx_symbol_exists(mprotect "sys/mman.h" C_HAVE_MPROTECT)


configure_file(config.h.in config.h)

add_subdirectory(src)

cmake_minimum_required(VERSION 3.8)
project(dosre)

find_package(SDL REQUIRED)

include(CheckTypeSize)
include(CheckCXXSourceCompiles)

check_type_size("int *" SIZEOF_INT_P)
check_type_size("unsigned char" SIZEOF_UNSIGNED_CHAR)
check_type_size("unsigned int" SIZEOF_UNSIGNED_INT)
check_type_size("unsigned long" SIZEOF_UNSIGNED_LONG)
check_type_size("unsigned long long" SIZEOF_UNSIGNED_LONG_LONG)
check_type_size("unsigned short" SIZEOF_UNSIGNED_SHORT)

check_cxx_source_compiles("inline void __attribute__((always_inline)) test(){}
int main() { test(); return 0; }" C_ATTRIBUTE_ALWAYS_INLINE)

configure_file(config.h.in config.h)

add_subdirectory(src)

cmake_minimum_required(VERSION 3.8)
project(dosre)

find_package(SDL REQUIRED)

include(CheckTypeSize)
check_type_size("int *" SIZEOF_INT_P)
check_type_size("unsigned char" SIZEOF_UNSIGNED_CHAR)
check_type_size("unsigned int" SIZEOF_UNSIGNED_INT)
check_type_size("unsigned long" SIZEOF_UNSIGNED_LONG)
check_type_size("unsigned long long" SIZEOF_UNSIGNED_LONG_LONG)
check_type_size("unsigned short" SIZEOF_UNSIGNED_SHORT)

configure_file(config.h.in config.h)

add_subdirectory(src)

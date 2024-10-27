cmake_minimum_required(VERSION 3.12 FATAL_ERROR)

project(tlse VERSION 0.1.0 LANGUAGES C)

option(BUILD_EXAMPLES "Enable building examples")

find_program(AFL_C_COMPILER afl-clang-fast)
find_program(AFL_CXX_COMPILER afl-clang-fast++)

find_program(AFL_C_COMPILER clang)
find_program(AFL_CXX_COMPILER clang++)

set(CMAKE_C_COMPILER ${AFL_C_COMPILER})
set(CMAKE_CXX_COMPILER ${AFL_CXX_COMPILER})

message("Building with CMake version: ${CMAKE_VERSION}")
add_compile_options("-DTLS_CURVE25519=1")
add_library(tlse STATIC
    curve25519.c
    tlse.c
    tlse.h
    libtomcrypt.c
)
add_library(tlse::tlse ALIAS tlse)
target_include_directories(tlse PUBLIC ${CMAKE_CURRENT_LIST_DIR})
target_compile_definitions(tlse PUBLIC ${TLSE_COMPILE_DEFINITIONS})

if (BUILD_EXAMPLES)
    #add_subdirectory(examples)
endif ()

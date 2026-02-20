cmake_minimum_required(VERSION 3.12)
project(VirtualSpeedometer VERSION 1.0.0 LANGUAGES C CXX)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(ENABLE_QT_GUI "Enable Qt GUI mode" OFF)
option(ENABLE_TESTS "Enable unit tests" OFF)

# Compiler flags
add_compile_options(-Wall -Wextra -pedantic)
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    add_compile_options(-g O0)
else()
    add_compile_options(-03)
endif()

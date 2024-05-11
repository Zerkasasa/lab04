cmake_minimum_required(VERSION 3.22.1)

project(formatter_ex_lib)

set(CMAKE_CXX_STANDART 20)
set(CMAKE_CXX_STANDART_REQUIRED ON)
set(CMAKE_CURRENT_SOURCE_DIR /home/vboxuser/Zerkasasa/workspace/projects/lab03/formatter_ex_lib)

add_library(formatter_ex STATIC /formatter_ex.cpp)

include_directories()
include_directories(/formatter_lib)

target_link_libraries(formatter_ex formatter)


cmake_minimum_required(VERSION 3.22)
project(hw3)

set(CMAKE_CXX_STANDART 11)
set(CMAKE_CXX_STANDART_REQUIRED ON)

add_library(hello_world_application hello_world.cpp)

include_directories("../formatter_ex_lib")
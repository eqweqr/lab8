cmake_minimum_required(VERSION 3.22)
project(hw3_1)

set(CMAKE_CXX_STANDART 11)
set(CMAKE_CXX_STANDART_REQUIRED ON)

include_directories("../formatter_lib")

add_library(formatter_ex_lib STATIC formatter_ex.cpp)

target_link_libraries(formatter_ex_lib PUBLIC formatter_lib)
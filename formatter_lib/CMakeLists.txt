cmake_minimum_required(VERSION 3.22)
project (hw3)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(formatter STATIC formatter.cpp)
include_directories()
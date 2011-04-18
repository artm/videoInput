project(videoInput)
cmake_minimum_required(VERSION 2.8)

add_library(videoInput videoInput.cpp)

include_directories(${CMAKE_SOURCE_DIR}/Include/BaseClasses)
include_directories(${CMAKE_SOURCE_DIR}/Include/DShow)

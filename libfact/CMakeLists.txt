cmake_minimum_required(VERSION 3.25)

project(libfact)

add_library(${PROJECT_NAME} fact.cpp)

target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)

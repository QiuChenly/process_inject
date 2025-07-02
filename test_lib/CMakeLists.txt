cmake_minimum_required(VERSION 3.10)
project(test_lib)

set(CMAKE_CXX_STANDARD 17)

add_library(${PROJECT_NAME} SHARED inject.mm)

target_link_libraries(${PROJECT_NAME} PRIVATE "-framework Foundation")

set(CMAKE_OSX_ARCHITECTURES "x86_64;arm64")

set(CMAKE_INSTALL_PREFIX ${CMAKE_SOURCE_DIR}/build)

install(TARGETS ${PROJECT_NAME} DESTINATION ${CMAKE_INSTALL_PREFIX})

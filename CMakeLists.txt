cmake_minimum_required(VERSION 3.10)
project(process_inject)

set(CMAKE_CXX_STANDARD 17)

add_executable(${PROJECT_NAME} main.mm)

target_link_libraries(${PROJECT_NAME} PRIVATE "-framework Foundation")

set(CMAKE_OSX_ARCHITECTURES "x86_64;arm64")

set(CMAKE_INSTALL_PREFIX ${CMAKE_SOURCE_DIR}/build)

install(TARGETS ${PROJECT_NAME} DESTINATION ${CMAKE_INSTALL_PREFIX})

# sudo codesign -f -s - --all-architectures --deep --entitlements "process_inject.entitlements" process_inject
# 后处理执行
add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E echo "开始后处理..."
    COMMAND codesign -fs - --all-architectures --deep $<TARGET_FILE:${PROJECT_NAME}> --entitlements "${CMAKE_SOURCE_DIR}/debug.entitlements"
    COMMAND ${CMAKE_COMMAND} -E echo "代码签名完成"
    COMMENT "执行代码签名后处理"
)

# 添加测试程序子目录
add_subdirectory(${CMAKE_SOURCE_DIR}/test_lib)
add_subdirectory(${CMAKE_SOURCE_DIR}/test_bin)

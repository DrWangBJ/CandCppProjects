# Set the minimum version of cmake required to build this project
cmake_minimum_required(VERSION 3.30)

# 指定 C 编译器
set(CMAKE_C_COMPILER "gcc" CACHE FILEPATH "C compiler")

# 指定 C++ 编译器
set(CMAKE_CXX_COMPILER "g++" CACHE FILEPATH "C++ compiler")

# 获取当前文件的完整路径
get_filename_component(CURRENT_FOLDER_ABSOLUTE ${CMAKE_CURRENT_LIST_FILE} PATH)

# 从完整路径中提取文件夹名称
get_filename_component(projectName ${CURRENT_FOLDER_ABSOLUTE} NAME)

project(${projectName} C)


# ##设置Release编译选项,需要发布时，取消下面三行的注释，默认是debug
# set(CMAKE_BUILD_TYPE "Release") #Debug;Release;RelWithDebInfo;MinSizeRel
# set(CMAKE_C_FLAGS_RELEASE "-O3 -DNDEBUG -mwindows")
# set(CMAKE_CXX_FLAGS_RELEASE "-O3 -DNDEBUG -mwindows")

# 设置pkg-config的路径，在c:/msys64/usr/bin中也有这个文件，版本不对，如果不设置这个，会找不到头文件
set(PKG_CONFIG_EXECUTABLE "C:/msys64/ucrt64/bin/pkg-config.exe")

find_package(PkgConfig)
pkg_search_module(GTK4 REQUIRED gtk4)

# 设置可执行文件的输出目录为项目根目录下的bin目录
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)

# 确保bin目录存在
file(MAKE_DIRECTORY ${CMAKE_RUNTIME_OUTPUT_DIRECTORY})

# 清空bin目录（如果需要）
file(REMOVE_RECURSE ${CMAKE_RUNTIME_OUTPUT_DIRECTORY})

include_directories(${GTK4_INCLUDE_DIRS})
add_executable(${projectName} main.c)
target_link_libraries(${projectName} ${GTK4_LIBRARIES})


message(STATUS "${PROJECT_BINARY_DIR}")
message(STATUS "libs=${GTK4_LIBRARIES}")
message(STATUS "include=${GTK4_INCLUDE_DIRS}")
message("Generated with config types: ${CMAKE_CONFIGURATION_TYPES}")



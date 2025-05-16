cmake_minimum_required(VERSION 3.29.0) # 设置CMake的最低版本要求为3.29.0
project(planning) # 设置项目名称为planning
set(CMAKE_CXX_STANDARD 17) # 设置C++标准为C++17

# 确保动态库和可执行文件输出到不同的目录
# 设置可执行文件的输出目录为构建目录下的bin文件夹
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/bin)

# 设置动态库的输出目录为构建目录下的lib文件夹
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/lib)

# 设置静态库的输出目录为构建目录下的lib文件夹
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/lib)

# 自动查找src目录下的所有源文件，并将它们添加到SRC_LISTS变量中 (动态库)
aux_source_directory(${PROJECT_SOURCE_DIR}/src SRC_LISTS)

# 添加一个名为common的共享库（动态库），其源文件列表为SRC_LISTS
add_library(common SHARED ${SRC_LISTS})

# 指定common库的公共包含目录，这样使用该库的项目可以找到头文件
target_include_directories(common PUBLIC ${PROJECT_SOURCE_DIR}/include)

# 添加一个名为planning_main的可执行文件，其源文件为planning_main.cpp
add_executable(planning_main planning_main.cpp)

# 指定planning_main可执行文件的公共包含目录，这样可以找到头文件
target_include_directories(planning_main PUBLIC ${PROJECT_SOURCE_DIR}/include)

# 将common库链接到planning_main可执行文件中
target_link_libraries(planning_main PUBLIC common)
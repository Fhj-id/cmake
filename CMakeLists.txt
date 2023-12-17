cmake_minimum_required(VERSION 3.0)
#增加-std=c++11
# set(CMAKE_CXX_STANDARD 11)
# #增加-std=c++14
# set(CMAKE_CXX_STANDARD 14)
# #增加-std=c++17
# set(CMAKE_CXX_STANDARD 17)
project(CALC)
# 指定头文件路径
include_directories(${PROJECT_SOURCE_DIR}/include) #已经通过ide设置了

# 静态库会在生成可执行程序的链接阶段被打包到可执行程序中，所以可执行程序启动，静态库就被加载到内存中了。
# 动态库在生成可执行程序的链接阶段不会被打包到可执行程序中，当可执行程序被启动并且调用了动态库中的函数的时候，动态库才会被加载到内存
# 因此，在cmake中指定要链接的动态库的时候，应该将命令写到生成了可执行文件之后

# 静态库
# 指定库文件路径
link_libraries(${PROJECT_SOURCE_DIR}/lib/static) #该静态库不是系统提供的（自己制作或者使用第三方提供的静态库）可能出现静态库找不到的情况，此时可以将静态库的路径也指定出来
# 链接静态库,指具体的库名，不需要加lib前缀和.a后缀
link_libraries(calc)

# 动态库
# 指定库文件路径
# link_directories(${PROJECT_SOURCE_DIR}/lib/shared)
# target_link_libraries(app calcdong)

# 输出路径
# set(HOME /home/fl/code/learnlab/cmake) #相当于输出的根目录
# set(EXECUTABLE_OUTPUT_PATH ${HOME}/bin) #可执行文件输出路径
# # set(LIBRARY_OUTPUT_PATH ${HOME}/lib) # 不论静态还是动态库 都输出同一个路径
# set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${HOME}/lib/static)# 设置静态库输出路径
# set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${HOME}/lib/shared)# 设置动态库输出路径

# 搜索文件 如果一个项目里边的源文件很多，在编写CMakeLists.txt文件的时候不可能将项目目录的各个文件一一罗列出来，这样太麻烦也不现实。
# 所以，在CMake中为我们提供了搜索文件的命令，可以使用aux_source_directory命令或者file命令。
# set(SRC_LIST add.c  div.c   main.c  mult.c  sub.c)
aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/src SRC_LIST)
list(APPEND SRC_LIST ${CMAKE_CURRENT_SOURCE_DIR}/main.c)


# 生成库
add_library(calc STATIC ${SRC_LIST})  # 静态库
add_library(calcdong SHARED ${SRC_LIST}) # 动态库
# 生成可执行文件
add_executable(app ${SRC_LIST}) 

# 指定动态链接库
# target_link_libraries(app pthread calcdong)
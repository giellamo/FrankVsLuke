cmake_minimum_required(VERSION 3.4.0)
project(LukeVSFrank VERSION 0.1.0)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

#include(CTest)
#enable_testing()

#You need to set the dafaults to propagate to subdirectories
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Link this 'library' to set the c++ standard / compile-time options requested
add_library(project_options INTERFACE)
target_compile_features(project_options INTERFACE cxx_std_17)

# Prefer pthread over system thread - DEVONO stare prima del find_package!
set(CMAKE_THREAD_PREFER_PTHREAD TRUE)
set(THREADS_PREFER_PTHREAD_FLAG TRUE)
find_package(Threads REQUIRED)

add_library(Luke src/gfx/bitmap/ABitmap.h
    src/gfx/bitmap/ABitmap.cpp
    src/gfx/bitmap/sprite.h
    src/gfx/bitmap/sprite.cpp
    src/gfx/common/coord.h
    src/gfx/common/coord.cpp
    src/gfx/common/IMovable.h
    src/gfx/common/size.h)
target_link_libraries(Luke PUBLIC project_options SDL2 SDL2_image Threads::Threads )
target_include_directories(Luke PUBLIC /usr/include/SDL2)

add_executable(LukeVSFrank    src/main.cpp)
target_compile_options(LukeVSFrank INTERFACE   -Wall -Wextra  -Wpedantic -Wnull-dereference -Wformat=2 -Wmisleading-indentation -Wduplicated-cond -Wduplicated-branches)
target_include_directories(LukeVSFrank PUBLIC /usr/include/SDL2)
target_link_libraries(LukeVSFrank PUBLIC Luke)

configure_file(assets/test.png assets/test.png COPYONLY)

add_executable(LukeTest src/test.cpp)
target_link_libraries(LukeTest PUBLIC Luke)
target_include_directories(LukeTest PUBLIC extern/)


set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)

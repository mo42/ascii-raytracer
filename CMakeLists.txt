cmake_minimum_required (VERSION 2.8)
project(ascii-raytracer)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(OpenMP)
if(OPENMP_FOUND)
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
  set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

file(GLOB SOURCES *.h *.cpp)
add_executable(${PROJECT_NAME} ${SOURCES})

find_library(NCURSESW_LIB NAMES ncursesw)
find_path(NCURSESW_INCLUDE_DIR NAMES ncurses.h PATH_SUFFIXES ncurses)

if(NCURSESW_LIB AND NCURSESW_INCLUDE_DIR)
  include_directories(${NCURSESW_INCLUDE_DIR})
  message(STATUS "Found ncursesw: ${NCURSESW_LIB}")
else()
  message(FATAL_ERROR "ncursesw not found!")
endif()

# Link ncurses
target_link_libraries(${PROJECT_NAME} ${NCURSESW_LIB})

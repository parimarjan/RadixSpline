cmake_minimum_required(VERSION 3.10)
project(radixspline)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -march=native")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -g3 -Wall -Wextra")

find_package(Threads REQUIRED)
set(THREADS_PREFER_PTHREAD_FLAG ON)

#include("${CMAKE_SOURCE_DIR}/cmake_modules/googletest.cmake")
include(FetchContent)
FetchContent_Declare(
  googletest
  URL https://github.com/google/googletest/archive/03597a01ee50ed33e9dfd640b249b4be3799d395.zip
)
FetchContent_MakeAvailable(googletest)

include_directories(
        ${GTEST_INCLUDE_DIR}
        ${CMAKE_SOURCE_DIR}
)

file(GLOB INCLUDE_H "include/rs/*.h")
set(EXAMPLE_FILES example.cc)
set(BENCH_FILES bench.cc)
file(GLOB TEST_CC "test/*_test.cc")

add_executable(example ${INCLUDE_H} ${EXAMPLE_FILES})
add_executable(bench ${INCLUDE_H} ${BENCH_FILES})

add_executable(tester ${TEST_CC})
target_link_libraries(tester gtest gtest_main Threads::Threads)

set(SOURCE_FILES util.h)
add_executable(generate generate.cc ${SOURCE_FILES})
target_link_libraries(generate Threads::Threads)

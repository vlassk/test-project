cmake_minimum_required(VERSION 3.14.0)
project(test_project VERSION 0.1.0 LANGUAGES C CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include_directories(include)

add_executable(main main.cpp src/my_add.cpp)

# testing and benchmarking
include(FetchContent)
FetchContent_Declare(
    googletest
    GIT_REPOSITORY https://github.com/google/googletest.git
    GIT_TAG v1.15.2
)
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)
FetchContent_Declare(
    googlebenchmark
        GIT_REPOSITORY https://github.com/google/benchmark.git
        GIT_TAG v1.9.0
)
FetchContent_MakeAvailable(googlebenchmark)

enable_testing()
add_executable(my_test tests/my_test.cpp src/my_add.cpp)
target_link_libraries(my_test GTest::gtest_main)
include(GoogleTest)
gtest_discover_tests(my_test)

add_executable(my_bench benchmarks/my_bench.cpp src/my_add.cpp)
target_link_libraries(my_bench benchmark::benchmark)
cmake_minimum_required(VERSION 3.10)
project(MyProject)

# Set the C++ standard to 20
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Enable all warnings and treat warnings as errors
if (CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
    add_compile_options(-Wall -Wextra -Wpedantic -Werror -Wno-sign-compare)
elseif (CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
    add_compile_options(/W4 /WX /wd4018)
endif()

# Clang-Tidy integration to enforce conformity to the C++ Core Guidelines
find_program(CLANG_TIDY_EXE NAMES "clang-tidy" REQUIRED)
set(CMAKE_CXX_CLANG_TIDY "${CLANG_TIDY_EXE}")

# Create a library from the project files
# ADD ALL YOUR SOURCE FILES HERE
add_library(myproject src/project.cpp)
target_include_directories(myproject PUBLIC include)

# Create an executable from the main file, linked to the project library
add_executable(app src/main.cpp)
target_link_libraries(app myproject)

# Find GoogleTest package and create a test executable
find_package(GTest REQUIRED)
enable_testing()
add_executable(tests tests/test.cpp) # ADD MORE TEST FILES HERE
target_link_libraries(tests GTest::GTest GTest::Main myproject)
add_test(NAME Tests COMMAND tests)



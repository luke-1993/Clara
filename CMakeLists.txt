cmake_minimum_required(VERSION 3.6.2)
project(Clara)

set(SOURCE_FILES src/main.cpp src/ClaraTests.cpp include/clara.hpp)
include_directories( include third_party )
add_executable(ClaraTests ${SOURCE_FILES})

set_property(TARGET ClaraTests PROPERTY CXX_STANDARD 11)
set_property(TARGET ClaraTests PROPERTY CXX_STANDARD_REQUIRED ON)
set_property(TARGET ClaraTests PROPERTY CXX_EXTENSIONS OFF)


if( CMAKE_CXX_COMPILER_ID MATCHES "Clang|AppleClang|GNU" )
    target_compile_options( ClaraTests PRIVATE -Wall -Wextra -pedantic -Werror )
endif()
if( CMAKE_CXX_COMPILER_ID MATCHES "MSVC" )
	target_compile_options( ClaraTests PRIVATE /W4 /WX )
endif()

if (ENABLE_COVERAGE)
    list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/CMake")
    find_package(codecov)
    add_coverage(ClaraTests)
    list(APPEND LCOV_REMOVE_PATTERNS "/usr/")
    coverage_evaluate()
endif()

include(CTest)
add_test(NAME RunTests COMMAND $<TARGET_FILE:ClaraTests>)

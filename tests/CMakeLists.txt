# CMakeLists.txt --- CMake project settings
#    ex) cmake -G "Visual Studio 9 2008"
#    ex) cmake -DCMAKE_BUILD_TYPE=Release -G "MSYS Makefiles"
##############################################################################

# CMake minimum version
cmake_minimum_required(VERSION 2.4)

# enable testing
enable_testing()

# project name and language
project(strunsafe C)

# check build type
if (NOT CMAKE_BUILD_TYPE)
    message(STATUS "No build type selected, default to Debug")
    set(CMAKE_BUILD_TYPE "Debug")
endif()

# make uppercase string of build type
string(TOUPPER "${CMAKE_BUILD_TYPE}" uppercase_CMAKE_BUILD_TYPE)

# check build type again
if (NOT uppercase_CMAKE_BUILD_TYPE MATCHES "^(DEBUG|RELEASE|RELWITHDEBINFO|MINSIZEREL)$")
    message(FATAL_ERROR "Invalid value for CMAKE_BUILD_TYPE: ${CMAKE_BUILD_TYPE}")
endif()

# set output directory (build/)
set(CMAKE_BINARY_DIR ${CMAKE_SOURCE_DIR}/build)
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR})
set(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR})

if (CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
    # using Clang
    set(CMAKE_C_FLAGS "-static")
    set(CMAKE_CXX_FLAGS "-static")
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    # using GCC
    set(CMAKE_C_FLAGS "-static -static-libgcc -static-libstdc++")
    set(CMAKE_CXX_FLAGS "-static -static-libgcc -static-libstdc++")
elseif (MSVC)
    # replace "/MD" with "/MT" (building without runtime DLLs)
    set(CompilerFlags
        CMAKE_C_FLAGS
        CMAKE_C_FLAGS_DEBUG
        CMAKE_C_FLAGS_RELEASE
        CMAKE_C_FLAGS_RELWITHDEBINFO
        CMAKE_CXX_FLAGS
        CMAKE_CXX_FLAGS_DEBUG
        CMAKE_CXX_FLAGS_RELEASE
        CMAKE_CXX_FLAGS_RELWITHDEBINFO)
    foreach(CompilerFlags ${CompilerFlags})
        string(REPLACE "/MD" "/MT" ${CompilerFlags} "${${CompilerFlags}}")
    endforeach()
endif()

##############################################################################

# include
include_directories(..)

if (WIN32)
    add_executable(strsafe_test strsafe_test.c)
    add_test(NAME SafeStringCchCatA COMMAND $<TARGET_FILE:strsafe_test> StringCchCatA)
    add_test(NAME SafeStringCchCatNA COMMAND $<TARGET_FILE:strsafe_test> StringCchCatNA)
    add_test(NAME SafeStringCchCopyA COMMAND $<TARGET_FILE:strsafe_test> StringCchCopyA)
    add_test(NAME SafeStringCchCopyNA COMMAND $<TARGET_FILE:strsafe_test> StringCchCopyNA)
    add_test(NAME SafeStringCchLengthA COMMAND $<TARGET_FILE:strsafe_test> StringCchLengthA)
    add_test(NAME SafeStringCchPrintfA COMMAND $<TARGET_FILE:strsafe_test> StringCchPrintfA)

    add_test(NAME SafeStringCchCatW COMMAND $<TARGET_FILE:strsafe_test> StringCchCatW)
    add_test(NAME SafeStringCchCatNW COMMAND $<TARGET_FILE:strsafe_test> StringCchCatNW)
    add_test(NAME SafeStringCchCopyW COMMAND $<TARGET_FILE:strsafe_test> StringCchCopyW)
    add_test(NAME SafeStringCchCopyNW COMMAND $<TARGET_FILE:strsafe_test> StringCchCopyNW)
    add_test(NAME SafeStringCchLengthW COMMAND $<TARGET_FILE:strsafe_test> StringCchLengthW)
    add_test(NAME SafeStringCchPrintfW COMMAND $<TARGET_FILE:strsafe_test> StringCchPrintfW)
endif(WIN32)

add_executable(strunsafe_test strunsafe_test.c)
add_test(NAME UnsafeStringCchCatA COMMAND $<TARGET_FILE:strunsafe_test> StringCchCatA)
add_test(NAME UnsafeStringCchCatNA COMMAND $<TARGET_FILE:strunsafe_test> StringCchCatNA)
add_test(NAME UnsafeStringCchCopyA COMMAND $<TARGET_FILE:strunsafe_test> StringCchCopyA)
add_test(NAME UnsafeStringCchCopyNA COMMAND $<TARGET_FILE:strunsafe_test> StringCchCopyNA)
add_test(NAME UnsafeStringCchLengthA COMMAND $<TARGET_FILE:strunsafe_test> StringCchLengthA)
add_test(NAME UnsafeStringCchPrintfA COMMAND $<TARGET_FILE:strunsafe_test> StringCchPrintfA)

if (WIN32)
    add_test(NAME UnsafeStringCchCatW COMMAND $<TARGET_FILE:strunsafe_test> StringCchCatW)
    add_test(NAME UnsafeStringCchCatNW COMMAND $<TARGET_FILE:strunsafe_test> StringCchCatNW)
    add_test(NAME UnsafeStringCchCopyW COMMAND $<TARGET_FILE:strunsafe_test> StringCchCopyW)
    add_test(NAME UnsafeStringCchCopyNW COMMAND $<TARGET_FILE:strunsafe_test> StringCchCopyNW)
    add_test(NAME UnsafeStringCchLengthW COMMAND $<TARGET_FILE:strunsafe_test> StringCchLengthW)
    add_test(NAME UnsafeStringCchPrintfW COMMAND $<TARGET_FILE:strunsafe_test> StringCchPrintfW)
endif(WIN32)

##############################################################################

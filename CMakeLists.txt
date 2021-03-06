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

# check exisistence of functions
include(CheckSymbolExists)

check_symbol_exists(vsnprintf stdio.h HAVE_VSNPRINTF)
check_symbol_exists(vsnprintf_s stdio.h HAVE_VSNPRINTF_S)
check_symbol_exists(_vsnprintf stdio.h HAVE__VSNPRINTF)
check_symbol_exists(_vsnprintf_s stdio.h HAVE__VSNPRINTF_S)

check_symbol_exists(vsnwprintf stdio.h HAVE_VSNWPRINTF)
check_symbol_exists(vsnwprintf_s stdio.h HAVE_VSNWPRINTF_S)
check_symbol_exists(_vsnwprintf stdio.h HAVE__VSNWPRINTF)
check_symbol_exists(_vsnwprintf_s stdio.h HAVE__VSNWPRINTF_S)

if (HAVE_VSNPRINTF)
    add_definitions(-DHAVE_VSNPRINTF)
endif()
if (HAVE_VSNPRINTF_S)
    add_definitions(-DHAVE_VSNPRINTF_S)
endif()
if (HAVE__VSNPRINTF)
    add_definitions(-DHAVE__VSNPRINTF)
endif()
if (HAVE__VSNPRINTF_S)
    add_definitions(-DHAVE__VSNPRINTF_S)
endif()
if (HAVE_VSNWPRINTF)
    add_definitions(-DHAVE_VSNWPRINTF)
endif()
if (HAVE_VSNWPRINTF_S)
    add_definitions(-DHAVE_VSNWPRINTF_S)
endif()
if (HAVE__VSNWPRINTF)
    add_definitions(-DHAVE__VSNWPRINTF)
endif()
if (HAVE__VSNWPRINTF_S)
    add_definitions(-DHAVE__VSNWPRINTF_S)
endif()

subdirs(tests)

##############################################################################

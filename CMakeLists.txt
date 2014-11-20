#
#   Copyright (c) 2012 Martin Sustrik  All rights reserved.
#   Copyright (c) 2013 GoPivotal, Inc.  All rights reserved.
#
#   Permission is hereby granted, free of charge, to any person obtaining a copy
#   of this software and associated documentation files (the "Software"),
#   to deal in the Software without restriction, including without limitation
#   the rights to use, copy, modify, merge, publish, distribute, sublicense,
#   and/or sell copies of the Software, and to permit persons to whom
#   the Software is furnished to do so, subject to the following conditions:
#
#   The above copyright notice and this permission notice shall be included
#   in all copies or substantial portions of the Software.
#
#   THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
#   IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
#   FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL
#   THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
#   LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
#   FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS
#   IN THE SOFTWARE.
#

cmake_minimum_required (VERSION 2.8)
include (CheckIncludeFiles)
include (CheckSymbolExists)
include (CheckCSourceCompiles)

project (nanomsg C)
enable_testing ()

#  Platform checks.

if (${CMAKE_SYSTEM_NAME} MATCHES "Windows")
    add_definitions (-DNN_HAVE_WINDOWS)
    add_definitions (-D_CRT_SECURE_NO_WARNINGS)

    if (MINGW)
        add_definitions (-DNN_HAVE_MINGW -DNN_HAVE_STDINT -D_WIN32_WINNT=0x0600)
    endif ()
else ()
    message (FATAL_ERROR "ERROR: CMake build system is intended only to generate MSVC solution files.\nUse autotools build system for any other purpose." )
endif ()

#  Build the library itself.

add_subdirectory (src/nanomsg)

get_directory_property(HAS_PARENT_SCOPE PARENT_DIRECTORY)
if(HAS_PARENT_SCOPE)
    set(NANOMSG_INCLUDE_DIRS ${CMAKE_CURRENT_SOURCE_DIR}/src PARENT_SCOPE)
    set(NANOMSG_LIBRARIES nanomsg PARENT_SCOPE)
endif()


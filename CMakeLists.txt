# This source file is part of the Swift.org open source project
#
# Copyright (c) 2014 - 2019 Apple Inc. and the Swift project authors
# Licensed under Apache License v2.0 with Runtime Library Exception
#
# See http://swift.org/LICENSE.txt for license information
# See http://swift.org/CONTRIBUTORS.txt for Swift project authors

cmake_minimum_required(VERSION 3.15.1)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake/modules)

project(SwiftPM LANGUAGES C Swift)

set(SWIFT_VERSION 5)
set(CMAKE_Swift_LANGUAGE_VERSION ${SWIFT_VERSION})
if(CMAKE_VERSION VERSION_LESS 3.16)
    add_compile_options($<$<COMPILE_LANGUAGE:Swift>:-swift-version$<SEMICOLON>${SWIFT_VERSION}>)
    set(CMAKE_LINK_LIBRARY_FLAG "-l")
endif()

set(CMAKE_Swift_MODULE_DIRECTORY ${CMAKE_BINARY_DIR}/swift)

if(CMAKE_VERSION VERSION_LESS 3.16 AND CMAKE_SYSTEM_NAME STREQUAL Windows)
  set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
  set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
else()
  set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
  set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
endif()
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

option(BUILD_SHARED_LIBS "Build shared libraryes by default" YES)

find_package(TSC CONFIG REQUIRED)

find_package(LLBuild CONFIG)
if(NOT LLBuild_FOUND)
  find_package(LLBuild REQUIRED)
endif()

find_package(dispatch QUIET)
find_package(Foundation QUIET)

find_package(Yams CONFIG REQUIRED)
find_package(ArgumentParser CONFIG REQUIRED)
find_package(SwiftDriver CONFIG REQUIRED)

add_subdirectory(Sources)

#
# Copyright 2018 Capitar IT Group BV <info@capitar.com>
# Copyright 2018 Staysail Systems, Inc. <info@staysail.tech>
#
# This software is supplied under the terms of the MIT License, a
# copy of which should be located in the distribution where this
# file was obtained (LICENSE.txt).  A copy of the license may also be
# found online at https://opensource.org/licenses/MIT.

cmake_minimum_required(VERSION 3.13)

project(nng-asyncdemo)

set(PARALLEL 128 CACHE STRING "Parallelism (min 4, max 1000)")

if (BUILD_DEMO)
else ()
# Call this from your own project's makefile.
  find_package(nng CONFIG REQUIRED)
endif (BUILD_DEMO)

find_package(Threads)

if (DEBUG)
  SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -g")
  if (ASAN)
    SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fsanitize=address")
  endif (ASAN)
  if (TSAN)
    SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fsanitize=thread")
  endif (TSAN)
endif (DEBUG)

add_executable(server server.c)
target_link_libraries(server nng)
target_compile_definitions(server PRIVATE NNG_ELIDE_DEPRECATED PARALLEL=${PARALLEL})

add_executable(client client.c)
target_link_libraries(client nng)
target_compile_definitions(client PRIVATE NNG_ELIDE_DEPRECATED)

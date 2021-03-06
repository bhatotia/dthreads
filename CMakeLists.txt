cmake_minimum_required(VERSION 2.8.11)
enable_testing()
project(dthreads)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake)

set(CMAKE_THREAD_PREFER_PTHREAD)
find_package(Threads)
if(NOT CMAKE_USE_PTHREADS_INIT)
  message(SEND_ERROR "Pthreads required")
endif(NOT CMAKE_USE_PTHREADS_INIT)

find_package(LibDL)
if(NOT LIBDL_FOUND)
  message(SEND_ERROR "libdl required")
endif(NOT LIBDL_FOUND)

add_subdirectory(src)
add_subdirectory(test)

if(MSVC)
  # Force to always compile with W4
  if(CMAKE_CXX_FLAGS MATCHES "/W[0-4]")
    string(REGEX REPLACE "/W[0-4]" "/W4" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
  else()
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4")
  endif()
elseif(CMAKE_COMPILER_IS_GNUCC OR CMAKE_COMPILER_IS_GNUCXX)
  # Update if necessary
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wno-long-long -pedantic")
endif()

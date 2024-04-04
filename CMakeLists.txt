cmake_minimum_required(VERSION 3.26)

set(default_build_type "Release")
set(CMAKE_CXX_STANDARD 20)
set(THREADS_PREFER_PTHREAD_FLAG TRUE)

project(foo VERSION 0.1.0 LANGUAGES CXX)
include(FetchContent)
include(CheckIPOSupported)

add_executable(foo)

set(FETCH_EMBED OFF CACHE BOOL "Fetch battery-embed")
if(FETCH_EMBED)
FetchContent_Declare(
  battery-embed
  GIT_REPOSITORY https://github.com/batterycenter/embed.git
  GIT_TAG        v1.2.8
)
FetchContent_MakeAvailable(battery-embed)
endif()

check_ipo_supported(RESULT LTO_supported OUTPUT error)
if(LTO_supported)
  message(STATUS "Enabling IPO/LTO")
  set_property(TARGET foo PROPERTY INTERPROCEDURAL_OPTIMIZATION TRUE)
else()
  message(STATUS "IPO/LTO not supported: <${error}>")
endif()

target_sources(foo
  PRIVATE
  foo.cc
)

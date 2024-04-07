cmake_minimum_required(VERSION 3.26)

set(CMAKE_CXX_STANDARD 20)

project(foo VERSION 0.1.0 LANGUAGES CXX)
include(FetchContent)
include(CheckIPOSupported)

add_executable(foo)

FetchContent_Declare(
  battery-embed
  GIT_REPOSITORY https://github.com/batterycenter/embed.git
  GIT_TAG        v1.2.8
)
FetchContent_MakeAvailable(battery-embed)
#b_embed(foo subdir/prayer.txt)

add_subdirectory(subdir)

check_ipo_supported(RESULT LTO_supported OUTPUT error)
if(LTO_supported)
  message(STATUS "Enabling IPO/LTO")
  set_property(TARGET foo PROPERTY INTERPROCEDURAL_OPTIMIZATION TRUE)
else()
  message(STATUS "IPO/LTO not supported: <${error}>")
endif()

cmake_minimum_required(VERSION 3.16)

project(my_target C ASM)

include(FetchContent)

FetchContent_Declare(stm32-cmake
        GIT_REPOSITORY https://github.com/ObKo/stm32-cmake
        GIT_TAG v2.1.0
)
FetchContent_MakeAvailable(stm32-cmake)

set(STM32_CMAKE_DIR ${stm32-cmake_SOURCE_DIR}/cmake/)

list(APPEND CMAKE_MODULE_PATH ${STM32_CMAKE_DIR})
include(stm32/common)
include(stm32/devices)

message(STATUS "Fetching dependencies: stm32-cmsis")
stm32_fetch_cmsis(L4)

set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE BOTH)

find_package(CMSIS COMPONENTS STM32L4R9AI REQUIRED)

add_executable(my_target main)
target_link_libraries(my_target CMSIS::STM32::L4R9AI STM32::NoSys)
stm32_add_linker_script(my_target PRIVATE custom.ld)

cmake_minimum_required(VERSION 3.20)

project(taskcli LANGUAGES CXX)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# For multi-config generators (Visual Studio): keep only Release
set(CMAKE_CONFIGURATION_TYPES "Release" CACHE STRING "" FORCE)

if(MSVC)
    add_compile_options(/W4 /permissive-)
else()
    add_compile_options(-Wall -Wextra -Wpedantic)
endif()

set(SOURCES
    src/main.cpp
    src/cli.cpp
    src/core.cpp
    src/storage_sqlite.cpp
    vendor/sqlite/sqlite3.c
)

add_executable(taskcli ${SOURCES})

target_include_directories(taskcli PRIVATE
    include
    vendor/sqlite
)

# Put taskcli.exe in the project root for all configs
set_target_properties(taskcli PROPERTIES
  RUNTIME_OUTPUT_DIRECTORY                 "${CMAKE_SOURCE_DIR}"
  RUNTIME_OUTPUT_DIRECTORY_RELEASE         "${CMAKE_SOURCE_DIR}"
  RUNTIME_OUTPUT_DIRECTORY_RELWITHDEBINFO  "${CMAKE_SOURCE_DIR}"
  RUNTIME_OUTPUT_DIRECTORY_MINSIZEREL      "${CMAKE_SOURCE_DIR}"
  RUNTIME_OUTPUT_DIRECTORY_DEBUG           "${CMAKE_SOURCE_DIR}"
)

include(CTest)
enable_testing()

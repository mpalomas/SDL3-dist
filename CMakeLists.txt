cmake_minimum_required(VERSION 3.26)
project(SDL3-dist VERSION 0.0.1 DESCRIPTION "SDL3-dist" LANGUAGES CXX C)
set(CMAKE_SUPPRESS_REGENERATION true)

# Set a default build type if none was specified
set(default_build_type "Release")

if(CMAKE_SYSTEM_NAME STREQUAL Darwin)
    set(platform "apple-clang-arm64")
elseif(CMAKE_SYSTEM_NAME STREQUAL Linux)
    set(platform "linux-clang-x64")
else()
    set(platform "win32-x64")
endif()
set(lib_root_folder "${PROJECT_SOURCE_DIR}/lib/${platform}")
set(build_type_folder "release")

set(lib_root_folder "${lib_root_folder}/shared/${build_type_folder}")
# TODO test linux
if(CMAKE_SYSTEM_NAME STREQUAL Darwin)
    set(sdl3_shared_lib_name "${lib_root_folder}/libSDL3.1.0.0.dylib")
elseif(CMAKE_SYSTEM_NAME STREQUAL Linux)
    set(sdl3_shared_lib_name "${lib_root_folder}/libSDL3.so")
else()
    set(sdl3_sym_lib_name "${lib_root_folder}/SDL3.lib")
    set(sdl3_shared_lib_name "${lib_root_folder}/SDL3.dll")
endif()

add_library(SDL3_shared SHARED IMPORTED GLOBAL)
add_library(SDL3::SDL3_shared ALIAS SDL3_shared)
if(CMAKE_SYSTEM_NAME STREQUAL Darwin OR CMAKE_SYSTEM_NAME STREQUAL Linux)
    set_property(TARGET SDL3_shared PROPERTY
                IMPORTED_LOCATION ${sdl3_shared_lib_name})
else()
    set_property(TARGET SDL3_shared PROPERTY
                IMPORTED_LOCATION ${sdl3_shared_lib_name})
    set_property(TARGET SDL3_shared PROPERTY
                IMPORTED_IMPLIB ${sdl3_sym_lib_name})
endif()
add_library(SDL3 INTERFACE IMPORTED GLOBAL)
set_property(TARGET SDL3 PROPERTY
    INTERFACE_LINK_LIBRARIES SDL3_shared)
add_library(SDL3::SDL3 ALIAS SDL3)
set_target_properties(SDL3 PROPERTIES INTERFACE_INCLUDE_DIRECTORIES ${PROJECT_SOURCE_DIR}/include)

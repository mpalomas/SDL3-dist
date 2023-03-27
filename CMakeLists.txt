cmake_minimum_required(VERSION 3.25)
project(SDL3-dist VERSION 0.0.1 DESCRIPTION "SDL3-dist" LANGUAGES CXX C)
set(CMAKE_SUPPRESS_REGENERATION true)

# Set a default build type if none was specified
set(default_build_type "Release")

# Set all configuration types
set(CMAKE_CONFIGURATION_TYPES Debug Release RelWithDebInfo)
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    message(STATUS "Setting build type to '${default_build_type}' as none was specified.")
    set(CMAKE_BUILD_TYPE "${default_build_type}" CACHE
        STRING "Choose the type of build." FORCE)
    # Set the possible values of build type for cmake-gui
    set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS
    "Release" "RelWithDebInfo")
endif()

set(platform "win32-x64")
set(lib_root_folder "${PROJECT_SOURCE_DIR}/lib/${platform}")
set(build_type_folder "release")

set(SDL3_BUILD_DLL TRUE)
set(lib_root_folder "${lib_root_folder}/shared/${build_type_folder}")
# TODO test for macos and linux
set(sdl3_sym_lib_name "${lib_root_folder}/SDL3.lib")
set(sdl3_shared_lib_name "${lib_root_folder}/SDL3.dll")
# TODO macos and linux

add_library(sdl3_shared SHARED IMPORTED GLOBAL)
add_library(sdl3::sdl3_shared ALIAS sdl3_shared)
set_property(TARGET sdl3_shared PROPERTY
            IMPORTED_LOCATION ${sdl3_shared_lib_name})
set_property(TARGET sdl3_shared PROPERTY
            IMPORTED_IMPLIB ${sdl3_sym_lib_name})
add_library(sdl3 INTERFACE IMPORTED GLOBAL)
set_property(TARGET sdl3 PROPERTY
    INTERFACE_LINK_LIBRARIES sdl3_shared)
add_library(sdl3::sdl3 ALIAS sdl3)
set_target_properties(wgpu PROPERTIES INTERFACE_INCLUDE_DIRECTORIES ${PROJECT_SOURCE_DIR}/include)

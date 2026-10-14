# .txt
# TODO CMake Configuration
# by NAME

# Initialize CMake
cmake_minimum_required(VERSION 4.3)
if (NOT DEFINED CMAKE_TOOLCHAIN_FILE)
    if (DEFINED ENV{HOME})
        set(CMAKE_TOOLCHAIN_FILE
                "$ENV{HOME}/vcpkg/scripts/buildsystems/vcpkg.cmake"
                CACHE STRING "vcpkg toolchain file"
        )
    elseif (DEFINED ENV{USERPROFILE})
        set(CMAKE_TOOLCHAIN_FILE
                "$ENV{USERPROFILE}/vcpkg/scripts/buildsystems/vcpkg.cmake"
                CACHE STRING "vcpkg toolchain file"
        )
    endif ()
endif ()
project(libTODO C)

# Set C compiler version
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS OFF)

# Set output directories
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/build/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/build/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/build/lib)

# Build libTODO
file(GLOB_RECURSE SOURCE CONFIGURE_DEPENDS ${PROJECT_SOURCE_DIR}/src/*.c)
list(REMOVE_ITEM SOURCE ${PROJECT_SOURCE_DIR}/src/main.c)
add_library(libTODO SHARED ${SOURCE})

# Rename libTODO
set_target_properties(libTODO PROPERTIES OUTPUT_NAME "TODO")

# Set include directory and build macros
target_include_directories(libTODO PUBLIC ${PROJECT_SOURCE_DIR}/include)
target_compile_definitions(libTODO PUBLIC $<$<CONFIG:Debug>:_DEBUG>)
target_compile_definitions(libTODO PRIVATE
        TODO_EXPORTS
        SDL_MAIN_HANDLED
)

# Find external libraries
find_package(SDL3 CONFIG REQUIRED)
find_package(cJSON CONFIG REQUIRED)

# Link external libraries to libTODO
target_link_libraries(libTODO PUBLIC
        SDL3::SDL3
        cjson
)

# Build TODO
add_executable(TODO ${PROJECT_SOURCE_DIR}/src/main.c)
target_link_libraries(TODO PRIVATE libTODO)

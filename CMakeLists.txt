# .txt
# TODO CMake Configuration
# by NAME

# Initialize CMake with vcpkg (if Windows)
#set(CMAKE_TOOLCHAIN_FILE "$ENV{USERPROFILE}/vcpkg/scripts/buildsystems/vcpkg.cmake" CACHE FILEPATH "vcpkg Toolchain File")
cmake_minimum_required(VERSION 3.16)
project(libTODO C)

# Set C compiler version
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS OFF)

# Set output directories
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/build/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/build/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/build/lib)

# Build libTODO
add_library(libTODO SHARED
        ${CMAKE_SOURCE_DIR}/include/TODO.h
        ${CMAKE_SOURCE_DIR}/include/External.h
        ${CMAKE_SOURCE_DIR}/include/Export.h
        ${CMAKE_SOURCE_DIR}/include/Test.h
        ${CMAKE_SOURCE_DIR}/src/Test.c
)

# Rename libTODO
set_target_properties(libTODO PROPERTIES OUTPUT_NAME "TODO")

# Set include directory and build macro
target_include_directories(libTODO PUBLIC ${CMAKE_SOURCE_DIR}/include)
target_compile_definitions(libTODO PUBLIC
        $<$<CONFIG:Debug>:_DEBUG>
)
target_compile_definitions(libTODO PRIVATE
        TODO_EXPORTS
        SDL_MAIN_HANDLED
)

# Find external libraries
find_package(SDL2 REQUIRED)

# Include external libraries
target_include_directories(libTODO PUBLIC
        ${SDL2_INCLUDE_DIRS}
)

# Link external libraries to libTODO
target_link_libraries(libTODO PUBLIC
        SDL2::SDL2
        SDL2::SDL2main
)

# Build TODO
add_executable(TODO ${CMAKE_SOURCE_DIR}/src/main.c)
target_link_libraries(TODO PRIVATE libTODO)
target_include_directories(TODO PRIVATE ${CMAKE_SOURCE_DIR}/include)

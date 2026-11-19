cmake_minimum_required(VERSION 3.21)
project(GazBodyslideReader VERSION 1.0.0 LANGUAGES CXX)

if(NOT WIN32 OR NOT MSVC OR NOT CMAKE_SIZEOF_VOID_P EQUAL 8)
    message(FATAL_ERROR "Build with an x64 MSVC-compatible Windows toolchain.")
endif()

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(COMMONLIB_ENABLE_IPO OFF CACHE BOOL "" FORCE)

include(FetchContent)
FetchContent_Declare(
    CommonLibSSE
    GIT_REPOSITORY https://github.com/alandtse/CommonLibSSE-NG.git
    GIT_TAG 73dcfe6d0cab4438d77fd1b9fbb590962482cd3a # v16.8.0
    GIT_SUBMODULES_RECURSE TRUE
)
FetchContent_MakeAvailable(CommonLibSSE)

find_path(RAPIDXML_INCLUDE_DIR NAMES rapidxml.hpp PATH_SUFFIXES rapidxml REQUIRED)

# Main.cpp already declares the exported SKSE metadata and entry points.
add_library(${PROJECT_NAME} SHARED
    src/Main.cpp
    src/Logging.cpp
    src/Papyrus.cpp
    src/BodyslideReader.cpp
)
target_include_directories(${PROJECT_NAME} PRIVATE src "${RAPIDXML_INCLUDE_DIR}")
target_precompile_headers(${PROJECT_NAME} PRIVATE src/PCH.h)
target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_23)
target_compile_options(${PROJECT_NAME} PRIVATE /bigobj /utf-8)
target_link_libraries(${PROJECT_NAME} PRIVATE CommonLibSSE::CommonLibSSE)

install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION SKSE/Plugins)
install(FILES LICENSE DESTINATION .)
install(FILES papyrus/BodyslideReader.psc DESTINATION Source/Scripts)

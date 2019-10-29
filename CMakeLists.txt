cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_COVERAGE "Build coverage" OFF)

set(
  HUNTER_CACHE_SERVERS
  "https://github.com/bmstu-iu8-cpp-sem-3/hunter-cache"
  CACHE STRING "Default cache server"
)

include("tools/gate/cmake/HunterGate.cmake")

HunterGate(
  URL "https://github.com/cpp-pm/hunter/archive/v0.23.218.tar.gz"
  SHA1 "9a3594a61227967fb92159339ba326701b287b19"
)


# TODO: rename project and delete this comment
project(server)
string(APPEND CMAKE_CXX_FLAGS " -pedantic -Werror -Wall -Wextra")
string(APPEND CMAKE_CXX_FLAGS " -Wno-unused-command-line-argument")
string(APPEND CMAKE_CXX_FLAGS " -Wshadow -Wnon-virtual-dtor")

hunter_add_package(GTest)
find_package(GTest CONFIG REQUIRED)

hunter_add_package(Boost)
find_package(Boost CONFIG REQUIRED)

hunter_add_package(Boost COMPONENTS log thread filesystem system)
find_package(Boost CONFIG REQUIRED log thread filesystem system)

add_library(server STATIC
        ${CMAKE_CURRENT_SOURCE_DIR}/sources/Source.cpp
        )
add_executable(main
        ${CMAKE_CURRENT_SOURCE_DIR}/sources/Source.cpp
        )

target_link_libraries(main Boost::boost Boost::log Boost::thread Boost::system Boost::filesystem server)

target_link_libraries(server Boost::log Boost::thread Boost::system Boost::filesystem Boost::boost)
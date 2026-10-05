cmake_minimum_required(VERSION 3.26)
project(JPAssist LANGUAGES C CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include(FetchContent)

find_package(nlohmann_json CONFIG QUIET)
if(NOT nlohmann_json_FOUND)
    FetchContent_Declare(
        nlohmann_json
        GIT_REPOSITORY https://github.com/nlohmann/json.git
        GIT_TAG v3.11.3
        GIT_SHALLOW TRUE
    )
    FetchContent_MakeAvailable(nlohmann_json)
endif()

FetchContent_Declare(
    dr_libs
    GIT_REPOSITORY https://github.com/mackron/dr_libs.git
    GIT_TAG da35f9d6c7374a95353fd1df1d394d44ab66cf01
)
FetchContent_MakeAvailable(dr_libs)

add_subdirectory(packages/study-mod-sdk)
add_subdirectory(packages/study-core)

option(JPASSIST_BUILD_OOT_PLUGIN "Build the Ocarina of Time Shipwright plugin" ON)
if(JPASSIST_BUILD_OOT_PLUGIN)
    add_subdirectory(games/ocarina-of-time/adapters/shipwright/plugin)
endif()

include(CTest)
if(BUILD_TESTING)
    add_subdirectory(tests/study-core)
endif()

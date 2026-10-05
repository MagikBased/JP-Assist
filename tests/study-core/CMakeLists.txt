include(FetchContent)

find_package(GTest CONFIG QUIET)
if(NOT GTest_FOUND)
    FetchContent_Declare(
        googletest
        GIT_REPOSITORY https://github.com/google/googletest.git
        GIT_TAG v1.16.0
        GIT_SHALLOW TRUE
    )
    set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
    FetchContent_MakeAvailable(googletest)
endif()

add_executable(jp_assist_study_core_tests
    test_audio_math.cpp
    test_learning_sync_client.cpp
    test_learning_sync_store.cpp
    test_overlay_layout.cpp
    test_study_selection_memory.cpp
    test_study_session.cpp
)
target_link_libraries(jp_assist_study_core_tests PRIVATE
    GTest::gtest_main
    jp_assist_study_core
)

include(GoogleTest)
gtest_discover_tests(jp_assist_study_core_tests)

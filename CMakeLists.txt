project(ct_regex)
cmake_minimum_required(VERSION 3.23)

set(CMAKE_CXX_STANDARD 17)
include(FetchContent)

# Fetch boost
FetchContent_Declare(
    boostorg
    GIT_REPOSITORY  https://github.com/boostorg/boost.git
    GIT_TAG         master
    GIT_PROGRESS    TRUE
)

FetchContent_GetProperties(boostorg)
if (NOT boostorg_POPULATED)
    FetchContent_Populate(boostorg)
    set(BOOST_ENABLE_CMAKE ON)
    add_subdirectory(${boostorg_SOURCE_DIR} ${boostorg_BINARY_DIR})
endif ()

include_directories(${boostorg_SOURCE_DIR})

add_executable(${PROJECT_NAME}
    main.cpp
)

cmake_minimum_required(VERSION 3.23)

project(regex VERSION 0.1.0)

set(CMAKE_CXX_STANDARD 17)
include(FetchContent)

# Fetch boost
FetchContent_Declare(
    Boost

    GIT_REPOSITORY  https://github.com/boostorg/boost.git
    GIT_TAG         master
    GIT_PROGRESS    TRUE

    OVERRIDE_FIND_PACKAGE
)
if(WIN32)
    # We have to set _WIN32_WINNT for b2
    if(${CMAKE_SYSTEM_VERSION} EQUAL 10) # Windows 10
        list(APPEND B2_OPTIONS define=BOOST_USE_WINAPI_VERSION=0x0A00)
    elseif(${CMAKE_SYSTEM_VERSION} EQUAL 6.3) # Windows 8.1
        list(APPEND B2_OPTIONS define=BOOST_USE_WINAPI_VERSION=0x0603)
    elseif(${CMAKE_SYSTEM_VERSION} EQUAL 6.2) # Windows 8
        list(APPEND B2_OPTIONS define=BOOST_USE_WINAPI_VERSION=0x0602)
    elseif(${CMAKE_SYSTEM_VERSION} EQUAL 6.1) # Windows 7
        list(APPEND B2_OPTIONS define=BOOST_USE_WINAPI_VERSION=0x0601)
    elseif(${CMAKE_SYSTEM_VERSION} EQUAL 6.0) # Windows Vista
        list(APPEND B2_OPTIONS define=BOOST_USE_WINAPI_VERSION=0x0600)
    else() # Windows XP (5.1)
        list(APPEND B2_OPTIONS define=BOOST_USE_WINAPI_VERSION=0x0501)
    endif()
endif()

FetchContent_MakeAvailable(Boost)
find_package(Boost
    COMPONENTS
        log
    REQUIRED
)

message(STATUS "Boost found. version:${Boost_VERSION_STRING}, source path: ${Boost_SOURCE_DIR}")

include_directories(${Boost_INCLUDE_DIRS})
add_executable(${PROJECT_NAME}
    main.cpp
)
target_link_libraries(${PROJECT_NAME} Boost::log_setup Boost::log)

cmake_minimum_required(VERSION 3.17)

project(EmbeddedArtistryMinimalLibc
        VERSION 1.0
        DESCRIPTION "A libc implementation for microncontroller-based embedded system."
        LANGUAGES C CXX)

set_property(GLOBAL PROPERTY C_STANDARD 11)

include(cmake/CPM.cmake)

# External Dependencies
# We're treating them as interface libraries, to use separate sources, include-dirs and flags 
# than libc.

# printf

CPMAddPackage(
        NAME printf
        GITHUB_REPOSITORY        mpaland/printf
        VERSION 4.0.0
        DOWNLOAD_ONLY
)
add_library(printf INTERFACE)
target_sources(printf INTERFACE ${printf_SOURCE_DIR}/printf.c)
target_include_directories(printf SYSTEM INTERFACE ${printf_SOURCE_DIR})

# openlibm

CPMAddPackage(
        NAME openlibm
        GITHUB_REPOSITORY       JuliaMath/openlibm
        VERSION 0.7.0
        DOWNLOAD_ONLY
)
add_library(openlibm INTERFACE)
target_include_directories(openlibm SYSTEM INTERFACE ${openlibm_SOURCE_DIR}/include)
target_include_directories(openlibm SYSTEM INTERFACE ${openlibm_SOURCE_DIR}/src)

# CMocka

#Include CMocka if it's installed,  download and compile if not as fallback.
find_package(cmocka QUIET)
if(NOT cmocka_FOUND)
        CPMAddPackage(
                NAME cmocka
                GIT_REPOSITORY https://git.cryptomilk.org/projects/cmocka.git/
                VERSION 1.1.5
                GIT_TAG cmocka-1.1.5
                OPTIONS
                        "WITH_EXAMPLES OFF"
                        "CMAKE_BUILD_TYPE DEBUG"
        )
        #Maintain build compatibility between find_package() and CMakeLists.txt variants.
        set(CMOCKA_LIBRARIES cmocka)
endif()

if(CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
        include(CTest)
endif()

if(BUILD_TESTING)
        add_executable(printf_tests)
        target_sources(printf_tests PRIVATE ${printf_SOURCE_DIR}/test/test_suite.cpp)
        target_include_directories(printf_tests PRIVATE ${printf_SOURCE_DIR})
        set_target_properties(printf_tests PROPERTIES CXX_STANDARD 11)
        add_test(NAME Printf.Test COMMAND printf_tests -s -r junit -o ${CMAKE_BINARY_DIR}/printf_tests.xml)

        add_subdirectory(test)
endif()

add_subdirectory(src)

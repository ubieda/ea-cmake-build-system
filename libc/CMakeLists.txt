cmake_minimum_required(VERSION 3.17)

project(EmbeddedArtistryMinimalLibc
        VERSION 1.0
        DESCRIPTION "A libc implementation for microncontroller-based embedded system."
        LANGUAGES C CXX)

set_property(GLOBAL PROPERTY C_STANDARD 11)

include(cmake/CPM.cmake)

# Source Files
add_subdirectory(src)


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

# CMocka Dependency

#Include CMocka if it's installed,  download and compile if not as fallback.
find_package(cmocka QUIET)
if(NOT cmocka_FOUND)
        CPMAddPackage(
                NAME cmocka
                GIT_REPOSITORY https://git.cryptomilk.org/projects/cmocka.git/
                VERSION 1.1.5
                GIT_TAG cmocka-1.1.5
                DOWNLOAD_ONLY YES # Only download it becuse we're treating it as an external project.
        )

        # Convenience to shorten the code below.
        set(CMOCKA_STATIC_FILENAME
                ${CMAKE_STATIC_LIBRARY_PREFIX}cmocka-static${CMAKE_STATIC_LIBRARY_SUFFIX}
        )

        # We would normally use CPMAddPackage, but CMocka's build presents problems.
        # So we include it as an external project, which allows us to build the lib
        # separately and then link it in.
        include (ExternalProject)
        ExternalProject_Add(project_cmocka
                SOURCE_DIR ${cmocka_SOURCE_DIR}
                PREFIX ${CMAKE_CURRENT_BINARY_DIR}/cmocka
                BINARY_DIR ${CMAKE_CURRENT_BINARY_DIR}/cmocka
                CMAKE_ARGS
                        -DBUILD_STATIC_LIB=ON
                        -DWITH_STATIC_LIB=ON # Without this, Cmocka will not install the static lib
                        -DWITH_EXAMPLES=OFF
                        -DCMAKE_BUILD_TYPE=Debug
                        -DCMAKE_INSTALL_PREFIX=${CMAKE_CURRENT_BINARY_DIR}/cmocka
                # This is needed with Ninja generators to prevent it from failing due to
                # the library being missing before the build has been run
                BUILD_BYPRODUCTS ${CMAKE_CURRENT_BINARY_DIR}/cmocka/lib/${CMOCKA_STATIC_FILENAME}
        )

        # We need to get the CMocka installation directory to know where our
        # built libraries can be found
        ExternalProject_Get_Property(project_cmocka BINARY_DIR)

        add_library(cmocka-static STATIC IMPORTED)
        set_target_properties(cmocka-static PROPERTIES
                IMPORTED_LOCATION ${BINARY_DIR}/lib/${CMOCKA_STATIC_FILENAME}
        )
        add_dependencies(cmocka-static project_cmocka)

        # Maintain build compatibility between find_package and CMakeLists.txt variants.
        set(CMOCKA_LIBRARIES cmocka-static)
        set(CMOCKA_INCLUDE_DIR ${BINARY_DIR}/include)
endif()

# Tests

if(CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
        include(CTest)
endif()

if(BUILD_TESTING)
        add_executable(printf_tests)
        target_sources(printf_tests PRIVATE ${printf_SOURCE_DIR}/test/test_suite.cpp)
        target_include_directories(printf_tests PRIVATE
                ${printf_SOURCE_DIR}
        )
        set_target_properties(printf_tests PROPERTIES CXX_STANDARD 11)
        add_test(NAME Printf.Test COMMAND printf_tests -s -r junit -o ${CMAKE_BINARY_DIR}/printf_tests.xml)

        # Re-execute the tests (w/ console output) instead of getting the test information as an xml file.
        add_custom_target(test-printf printf_tests)

        # Clear XML files every time the build-system is run, such that new results are outputted as XML files.
        # As CMocka does not overwrite existing XML files.
        add_custom_target(test-clear-results
                COMMAND ${CMAKE_COMMAND} -E rm -f ${CMAKE_BINARY_DIR}/test/*.xml
                COMMENT "Cleaning XML files from test directory"
        )

        add_subdirectory(test)
endif()

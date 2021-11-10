cmake_minimum_required(VERSION 3.17)

project(EmbeddedArtistryMinimalLibc
        VERSION 1.0
        DESCRIPTION "A libc implementation for microncontroller-based embedded system."
        LANGUAGES C)

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

add_subdirectory(src)

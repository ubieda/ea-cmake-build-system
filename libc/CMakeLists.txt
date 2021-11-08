cmake_minimum_required(VERSION 3.17)

project(EmbeddedArtistryMinimalLibc
        VERSION 1.0
        DESCRIPTION "A libc implementation for microncontroller-based embedded system."
        LANGUAGES C)

set_property(GLOBAL PROPERTY C_STANDARD 11)

add_subdirectory(src)
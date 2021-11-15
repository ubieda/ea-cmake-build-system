cmake_minimum_required(VERSION 3.17)

project(
    VerifyCmockaInstalled
    VERSION 1.0
    DESCRIPTION "Prints out a message indicating whether CMocka is installed in your system"
)

find_package(cmocka)

if(cmocka_FOUND)
    message(STATUS "CMocka is installed in your system. Congrats!")
else()
    message(WARNING "CMocka is not installed in your system")
endif()
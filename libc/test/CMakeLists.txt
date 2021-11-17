add_executable(libc_tests)

target_include_directories(libc_tests PRIVATE
    .
    test
)

target_include_directories(libc_tests SYSTEM PRIVATE ${CMOCKA_INCLUDE_DIR})

target_sources(libc_tests PRIVATE
    main.c
)

target_sources(libc_tests PRIVATE
    ctype/ctype_tests.c
    ctype/isalnum.c
    ctype/isalpha.c
    ctype/isascii.c
    ctype/isblank.c
    ctype/iscntrl.c
    ctype/isdigit.c
    ctype/isgraph.c
    ctype/islower.c
    ctype/isprint.c
    ctype/ispunct.c
    ctype/isspace.c
    ctype/isupper.c
    ctype/isxdigit.c
    ctype/toascii.c
    ctype/tolower.c
    ctype/toupper.c
)

target_sources(libc_tests PRIVATE
    stdlib/abs.c
    stdlib/atof.c
    stdlib/atoi.c
    stdlib/atol.c
    stdlib/atoll.c
    stdlib/bsearch.c
    stdlib/calloc.c
    stdlib/div.c
    stdlib/heapsort_r.c
    stdlib/heapsort.c
    stdlib/imaxabs.c
    stdlib/imaxdiv.c
    stdlib/labs.c
    stdlib/ldiv.c
    stdlib/llabs.c
    stdlib/lldiv.c
    stdlib/qsort_r.c
    stdlib/qsort.c
    stdlib/rand.c
    stdlib/realloc.c
    stdlib/stdlib_tests.c
    stdlib/strtod.c
    stdlib/strtof.c
    stdlib/strtol.c
    stdlib/strtoll.c
    stdlib/strtoul.c
    stdlib/strtoull.c
)

target_sources(libc_tests PRIVATE
    string/memcmp.c
    string/memcpy.c
    string/memmem.c
    string/memmove.c
    string/memset.c
    string/strcat.c
    string/strchr.c
    string/strcmp.c
    string/strcpy.c
    string/strdup.c
    string/string_tests.c
    string/strlen.c
    string/strncat.c
    string/strncmp.c
    string/strncpy.c
    string/strndup.c
    string/strnlen.c
    string/strnstr.c
    string/strrchr.c
    string/strstr.c
    string/strtok.c
)

target_sources(libc_tests PRIVATE
    test/rand.c
)

# Link External dependencies

target_link_libraries(libc_tests PRIVATE c)
target_link_libraries(libc_tests PRIVATE ${CMOCKA_LIBRARIES})

add_test(NAME Libc.Test COMMAND libc_tests)

# # Re-execute the tests (w/ console output) instead of getting the test information as an xml file.
add_custom_target(test-libc
    COMMAND export CMOCKA_MESSAGE_OUTPUT=stdout
    COMMAND libc_tests
)

# Create a dependency so we can then clear the test results every-time they're run.
add_test(NAME Libc.ClearResults
    COMMAND ${CMAKE_COMMAND} --build ${CMAKE_BINARY_DIR} --target test-clear-results
)

set_tests_properties(Libc.Test PROPERTIES
    ENVIRONMENT CMOCKA_XML_FILE=${CMAKE_CURRENT_BINARY_DIR}/%g.xml
    DEPENDS Libc.ClearResults
)

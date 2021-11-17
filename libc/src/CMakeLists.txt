# Create library named gdtoa, which handles conversions between strings and floats.

add_library(gdtoa INTERFACE)

target_include_directories(gdtoa INTERFACE gdtoa/include)

target_sources(gdtoa INTERFACE
	gdtoa/src/dmisc.c
	gdtoa/src/dtoa.c
	gdtoa/src/g__fmt.c
	gdtoa/src/g_ddfmt.c
	gdtoa/src/g_dfmt.c
	gdtoa/src/g_ffmt.c
	gdtoa/src/g_Qfmt.c
	gdtoa/src/g_xfmt.c
	gdtoa/src/g_xLfmt.c
	gdtoa/src/gdtoa.c
	gdtoa/src/gethex.c
	gdtoa/src/gmisc.c
	gdtoa/src/hd_init.c
	gdtoa/src/hexnan.c
	gdtoa/src/misc.c
	gdtoa/src/smisc.c
	gdtoa/src/strtod.c
	gdtoa/src/strtodg.c
	gdtoa/src/strtodI.c
	gdtoa/src/strtof.c
	gdtoa/src/strtoId.c
	gdtoa/src/strtoIdd.c
	gdtoa/src/strtoIf.c
	gdtoa/src/strtoIg.c
	gdtoa/src/strtoIQ.c
	gdtoa/src/strtoIx.c
	gdtoa/src/strtoIxL.c
	gdtoa/src/strtopd.c
	gdtoa/src/strtopdd.c
	gdtoa/src/strtopf.c
	gdtoa/src/strtopQ.c
	gdtoa/src/strtopx.c
	gdtoa/src/strtopxL.c
	gdtoa/src/strtord.c
	gdtoa/src/strtordd.c
	gdtoa/src/strtorf.c
	gdtoa/src/strtorQ.c
	gdtoa/src/strtorx.c
	gdtoa/src/strtorxL.c
	gdtoa/src/sum.c
	gdtoa/src/ulp.c
)

target_compile_definitions(gdtoa INTERFACE
	NO_ERRNO
	IFNAN_CHECK
	GDTOA_NO_ASSERT
	NO_FENV_H
)

# Create static libc library

# STATIC keyword not necessary as we're disabling shared libs by default.
add_library(c)

target_include_directories(c PRIVATE ${PROJECT_SOURCE_DIR}/include)
target_include_directories(c PRIVATE ${PROJECT_SOURCE_DIR}/arch/x86_64/include)


target_include_directories(c SYSTEM INTERFACE ${PROJECT_SOURCE_DIR}/include)
target_include_directories(c SYSTEM INTERFACE ${PROJECT_SOURCE_DIR}/arch/x86_64/include)

target_include_directories(c SYSTEM INTERFACE $<TARGET_PROPERTY:openlibm,INTERFACE_INCLUDE_DIRECTORIES>)

target_sources(c PRIVATE
	assert/assert.c
)

target_sources(c PRIVATE
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

target_sources(c PRIVATE
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
	stdlib/labs.c
	stdlib/ldiv.c
	stdlib/llabs.c
	stdlib/lldiv.c
	stdlib/qsort_r.c
	stdlib/qsort.c
	stdlib/rand.c
	stdlib/realloc.c
	stdlib/strtol.c
	stdlib/strtold.c
	stdlib/strtoll.c
	stdlib/strtoul.c
	stdlib/strtoull.c
)

target_sources(c PRIVATE
	locale/langinfo.c
)

target_sources(c PRIVATE
	math/fabs.c
	math/fabsf.c
)

target_sources(c PRIVATE
	stdio/asprintf.c
	stdio/putchar.c
	stdio/puts.c
	stdio/vasprintf.c
)

target_sources(c PRIVATE
	stdio/putchar_native.c
)

target_sources(c PRIVATE
	support/fls.c
	support/flsl.c
	support/flsll.c
)

target_sources(c PRIVATE
	time/asctime.c
	time/asctime_r.c
)

target_sources(c PRIVATE
	string/memcmp.c
	string/memcpy.c
	string/memmem.c
	string/memmove.c
	string/memchr.c
	string/memrchr.c
	string/memset.c
	string/strcat.c
	string/strchr.c
	string/strchrnul.c
	string/strcmp.c
	string/strcoll.c
	string/strcpy.c
	string/strcspn.c
	string/strdup.c
	string/strerror.c
	string/strerror_r.c
	string/strlen.c
	string/strncat.c
	string/strncmp.c
	string/strncpy.c
	string/strndup.c
	string/strnlen.c
	string/strnstr.c
	string/strpbrk.c
	string/strrchr.c
	string/strspn.c
	string/strstr.c
	string/strtok.c
	string/strxfrm.c
)

# Link External dependencies

target_link_libraries(c PRIVATE printf)
target_link_libraries(c PRIVATE openlibm)
target_link_libraries(c PRIVATE gdtoa)
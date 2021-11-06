.. _cmake_cheatsheet:

CMake Syntax Cheatsheet
#######################

.. contents::
    :local:
    :depth: 2

This document is a cheatsheet of general CMake syntax, which will become handy for the remaining of the course.

Variables
*********

+--------------------+--------------------------------------------+
| Action             | Syntax Example                             |
+====================+============================================+
| Set                | `set(var value)`                           |
+--------------------+--------------------------------------------+
| Unset              | `unset(var)`                               |
+--------------------+--------------------------------------------+
| Reference          | `${var}`                                   |
+--------------------+--------------------------------------------+
| Reference (nested) | `${outer_${inner_variable}_variable}`      |
+--------------------+--------------------------------------------+
| Set (lists)        | `set(sources a.c b.c c.c) # "a.c;b.c;c.c"` |
+--------------------+--------------------------------------------+

Variables Scope
***************

+-------------------------+-----------------------------------------------------------------------------------+
| Scope                   | Description                                                                       |
+=========================+===================================================================================+
|| Function Scope         || - Variable is defined inside a function.                                         |
||                        || - Available to current function and nested call.                                 |
||                        || - Ends when the function returns                                                 |
+-------------------------+-----------------------------------------------------------------------------------+
|| Directory Scope        || - Any variable defined outside a function.                                       |
||                        || - Available to the current CMakeLists.txt and any subdirectory().                |
||                        || - Unavailable to parents directories, nor directories included through include() |
+-------------------------+-----------------------------------------------------------------------------------+
|| Persistent Cache Scope || - Stored as a separate set of cached variables y CMake.                          |
||                        || - Available to the entire build tree.                                            |
||                        || - They persist across build invocations.                                         |
+-------------------------+-----------------------------------------------------------------------------------+

- Note: Scope can be overriden by setting `CACHE`, `PARENT_SCOPE` or other options throughout the code.

Common Control Structures
*************************

+--------------------------------------------+--------------------------------------+
| Operation                                  | Syntax Example                       |
+============================================+======================================+
|| If, Else if, Else                         || `if(<statement-1>)`                 |
||                                           || `<action-1>`                        |
||                                           || `elif(<statement-2>)`               |
||                                           || `<action-2>`                        |
||                                           || `else()`                            |
||                                           || `<action-3>`                        |
||                                           || `endif()`                           |
+--------------------------------------------+--------------------------------------+
|| While                                     || `while(<condition>)`                |
||                                           || `<commands>`                        |
||                                           || `endwhile()`                        |
+--------------------------------------------+--------------------------------------+
|| For-Loop (iterate through list elements)  || `foreach(<loop_var> <items>)`       |
||                                           || `<commands>`                        |
||                                           || `endforeach()`                      |
+--------------------------------------------+--------------------------------------+
|| For-Loop (skip iterations and break loop) || `foreach(<loop_var> <items>)`       |
||                                           || `<commands>`                        |
||                                           || `...`                               |
||                                           || `continue()`                        |
||                                           || `...`                               |
||                                           || `break()`                           |
||                                           || `...`                               |
||                                           || `endforeach()`                      |
+--------------------------------------------+--------------------------------------+
|| For-Loop (iterate through numbers)        || `foreach(<loop_var> RANGE <stop>)`  |
||                                           || `<commands>`                        |
||                                           || `endforeach()`                      |
+--------------------------------------------+--------------------------------------+
|| Function Definition                       || `function(my_command input output)` |
||                                           || `<commands>`                        |
||                                           || `endfunction()`                     |
+--------------------------------------------+--------------------------------------+
|| Macro Definition                          || `macro(my_command)`                 |
||                                           || `<commands>`                        |
||                                           || `endmacro()`                        |
+--------------------------------------------+--------------------------------------+

Defining a project
******************

- Declare a project with: `cmake_minimum_required(VERSION 3.17 <or other version>)`.
- Have a `project()` funct call with:`
        `project(<PROJECT-NAME>`
            `[VERSION <major>[.<minor>[.<patch>[.<weak>]]]]`
            `[DESCRIPTION <project-description-string>]`
            `[HOMEPAGE_URL <url-string>]`
            `[LANGUAGES <language-name>...]`
            `)`.
- Add Source Files to a target using `target_sources()`.
- Add Header Files to a target using `target_include_directories()`.
- (Optional) Define a target through `add_executable()`.
- (Optional) Add libraries using `add_library()`.

Common Functions
****************

+-----------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------+
| Name                                                                                                                        | Purpose                                                             |
+=============================================================================================================================+=====================================================================+
| `list() <https://cmake.org/cmake/help/latest/command/list.html>`_                                                           | Simplify operations on list variables                               |
+-----------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------+
| `string() <https://cmake.org/cmake/help/latest/command/string.html>`_                                                       | Perform string operations on a variable                             |
+-----------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------+
| `math() <https://cmake.org/cmake/help/latest/command/math.html>`_                                                           | Perform math operations on a variable                               |
+-----------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------+
| `add_executable() <https://cmake.org/cmake/help/latest/command/add_executable.html>`_                                       | Adds an executable to the project using the specified source files. |
+-----------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------+
| `add_library() <https://cmake.org/cmake/help/latest/command/add_library.html>`_                                             | Adds a library target to be built from the source files listed.     |
+-----------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------+
| `target_sources() <https://cmake.org/cmake/help/latest/command/target_sources.html>`_                                       | Specifies sources when building a target                            |
+-----------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------+
| `target_include_directories() <https://cmake.org/cmake/help/latest/command/target_include_directories.html>`_               | Specifies include directories when building a target                |
+-----------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------+
| `target_compile_options() <https://cmake.org/cmake/help/latest/command/target_compile_options.html>`_                       | Specify compilation options for a target.                           |
+-----------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------+
| `add_compile_options() <https://cmake.org/cmake/help/latest/command/add_compile_options.html#command:add_compile_options>`_ | Sets global compilation flags.                                      |
+-----------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------+
| `target_compile_definitions() <https://cmake.org/cmake/help/latest/command/target_compile_definitions.html>`_               | Add compiler definitions                                            |
+-----------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------+
| `target_compile_features() <https://cmake.org/cmake/help/latest/command/target_compile_features.html>`_                     | Set flags related to compilation features.                          |
+-----------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------+
| `target_link_libraries() <https://cmake.org/cmake/help/latest/command/target_link_libraries.html>`_                         | Specifies libraries to link to a specific libraries or target.      |
+-----------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------+
| `target_link_options() <https://cmake.org/cmake/help/latest/command/target_link_options.html>`_                             | Specifies link flags                                                |
+-----------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------+
| `set_target_properties() <https://cmake.org/cmake/help/latest/command/set_target_properties.html>`_                         | Set CMake target properties                                         |
+-----------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------+
| `add_subdirectory() <https://cmake.org/cmake/help/latest/command/add_subdirectory.html>`_                                   | Add subdirectory to build                                           |
+-----------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------+
| `include() <https://cmake.org/cmake/help/latest/command/include.html>`_                                                     | Loads code from a CMake file or module                              |
+-----------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------+

Target Operations Scope
***********************

+-------------+-------------------------------------------------------------------------------------+
| Keyword     | Scope                                                                               |
+=============+=====================================================================================+
| `PRIVATE`   | Relevant only when building for the specified target (not when used as dependency). |
+-------------+-------------------------------------------------------------------------------------+
| `INTERFACE` | Relevant only when using target as dependency.                                      |
+-------------+-------------------------------------------------------------------------------------+
| `PUBLIC`    | Relevant for building target and using as dependency.                               |
+-------------+-------------------------------------------------------------------------------------+

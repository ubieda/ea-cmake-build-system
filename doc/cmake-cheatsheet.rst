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

- Make sure CMAKE_GENERATOR=Ninja environment variable is set.
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
- Use Toolchain Files to handle cross-compilation and alternate Toolchains.
- Create a CMakeLists.txt at the project root.
- Create a build directory and cd to it.
- Type `cmake ..`
- Build with the default all target by typing: `make` or `ninja`. 
- Use `ninja/make help` to list all available target.

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
| `target_compile_definitions() <https://cmake.org/cmake/help/latest/command/target_compile_definitions.html>`_               | Add compiler definitions (pre-processor)                            |
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

Modules in CMake
****************

- Are script files that provide additional build code, variables, targets, functions and others.
- Have a .cmake extension.
- Some are build-in, others are custom.
- To include modules in your build, use `include()`, in the form: `include(<file|module> [OPTIONAL] [RESULT_VARIABLE <var>] [NO_POLICY_SCOPE])`
- Follows the scope of the caller.
- Use CMAKE_MODULE_PATH to indicate directories where CMake should look for modules (blank by default).

External Dependency Management
******************************

+---------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------+
| Approach                                                                                    | Description                                                                                     |
+=============================================================================================+=================================================================================================+
| `FetchContent Module <https://cmake.org/cmake/help/latest/module/FetchContent.html>`_       | - Enables populating content at build configuration time.                                       |
|                                                                                             | - Add `include(FetchContent)` before using it.                                                  |
|                                                                                             | - `FetchContent_Declare()` declares the external dependency.                                    |
|                                                                                             | - `FetchContent_MakeAvailable()` downloads it and adds it.                                      |
|                                                                                             | - See `FetchContent_GetProperties()` for finer-grain control.                                   |
+---------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------+
| `ExternalProject Module <https://cmake.org/cmake/help/latest/module/ExternalProject.html>`_ | - Populates the content at build-time (as opposed to FetchContent)                              |
|                                                                                             | - Add `include(ExternalProject)` before using it.                                               |
|                                                                                             | - `ExternalProject_Add()` downloads it and adds it.                                             |
+---------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------+
| `find_library() <https://cmake.org/cmake/help/latest/command/find_library.html>`_           | - Finds libraries installed on our system.                                                      |
+---------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------+
| `find_package() <https://cmake.org/cmake/help/latest/command/find_package.html>`_           | - Searches for CMake packages installed on our system.                                          |
|                                                                                             | - Either works in Module or Config modes.                                                       |
+---------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------+
| `FindPkgConfig Module <https://cmake.org/cmake/help/latest/module/FindPkgConfig.html>`_     | - Uses pkg-config to manage dependencies.                                                       |
|                                                                                             | - Add `include(FindPkfgConfig)` or `find_package (PkgConfig REQUIRED)` before using it.         |
|                                                                                             | - `pkg_check_modules()` searches for modules with pkg-config.                                   |
+---------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------+
| `CPMAddPackage Module (third-party) <https://github.com/cpm-cmake/CPM.cmake>`_              | - Wrapper around FetchContent and ExternalProject.                                              |
|                                                                                             | - The two APIs CPM provides are: `CPMAddPackage()` and `CPMFindPackage()`.                      |
|                                                                                             | - `CPMFindPackage()` tries `find_package()` before trying with `CPMFindPackage()`.              |
|                                                                                             | - Fetched repository is placed in build_dir/_deps.                                              |
|                                                                                             | - CPM creates a variable target <name>_SOURCE_DIR referencing the path to the fetched dependency. |
+---------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------+
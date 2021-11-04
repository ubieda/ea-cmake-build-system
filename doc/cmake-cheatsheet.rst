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

Common Operations
*****************

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
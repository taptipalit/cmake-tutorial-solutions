## CMake Tutorial

This directory contains source code examples for the [CMake
Tutorial](https://cmake.org/cmake/help/latest/guide/tutorial/index.html).

Each step has its own subdirectory containing code that may be used as a
starting point. The tutorial examples are progressive so that each step
provides the complete solution for the previous step.

### High Level Idea

It's tempting to view CMake as a more convoluted automake-like system, but
it's so much more than that.
CMake allows you to decompose your project into different libraries (or
targets as they're called in the CMake world). Each target has its own include
directories and further dependencies (called <em>Usage Requirements</em> in
the CMake land). If you import a library, you inherit all
its dependencies automatically. 

Essentially, it turns the project build
process into a nice dependency graph of 
objects (targets) that can be hierarchically compiled to build
the final executable (or final library). In contrast to systems like
`automake`, CMake is a rich, descriptive, language in itself that can
decouple the relationships between the build artifacts from the actual compilation steps.

In CMake, each target has properties, such as `LINK_LIBRARIES`,
`INCLUDE_DIRECTORIES`, `COMPILE_DEFINITIONS`, and commands that update these
properties, such as `target_link_libraries`, `target_include_directories`,
`target_compile_definitions`. Each of these commands have an item list and
options that can customize the operation of these commands.

For example, the `PRIVATE`, `PUBLIC`, and `INTERFACE` inheritance allows you
to describe whether the **only** the target library, the target library **and** all
its dependencies, or **only** the dependencies of the target library, link
with the specified library or libraries. (e.g.
`target_link_libraries(BestExecutableInTheWorld PUBLIC
BestLibraryInTheWorld)`. 

Finally of course, this dependency graph is turned in the specified native
build system (Ninja, Makefile, etc). 

#### Richness of CMake language

CMake supports list operations, if-then-else conditionals and much more. It
also supports <em>[Generator
Expressions]</em>(https://cmake.org/cmake/help/latest/manual/cmake-generator-expressions.7.html#manual:cmake-generator-expressions(7)). 
These generator expressions are evaluated during the build to produce
information about the build system. They are written as `$< ... >`. 

For example, `$<C_COMPILER_ID>` returns the compiler id. 
`$<COMPILE_LANG_AND_ID:CXX,AppleClang,Clang,ARMClang,GNU,LCC>` return `True`
if any of the compiler id is one of `AppleClang`, `Clang`, and so on. These
generator expressions can thus be used to enable compiler-specific options,
via the `target_compile_options` command using another generator expression.
E.g. `target_compile_options(MyTarget INTERFACE
"$<${GCC_LIKE_CC}:-Wall;-Wextra;-Wunused>").


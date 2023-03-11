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
directories and further dependencies. If you import a library, you inherit all
its dependencies automatically. 

Essentially, it turns the project build
process into a nice dependency graph of 
objects (targets) that can be hierarchically compiled to build
the final executable (or final library). In contrast to systems like
`automake`, CMake is a rich, descriptive, language in itself that can
decouple the relationships between the build artifacts from the actual compilation steps.

For example, the `PRIVATE`, `PUBLIC`, and `INTERFACE` inheritance allows you
to describe whether the **only** the target library, the target library **and** all
its dependencies, or **only** the dependencies of the target library, link
with the specified library or libraries. (e.g.
`target_link_libraries(BestExecutableInTheWorld PUBLIC
BestLibraryInTheWorld)`. 

Finally of course, this dependency graph is turned in the specified native
build system (Ninja, Makefile, etc). 

# Straightforward C Project Standard (SCPS)

## Abstract

This document outlines a project structure for C(++) programs and libraries
that allows for easy interoperability with compilers, build-systems, 
and linking programs. There are six levels of the standard, each 
with a more severe degree of simplicity.

## Introduction

C currently has no de-facto standard package manager. As such, when 
integrating C libraries or projects into an overarching project, it
can be a hassle to build the underlying library, requiring reading
through documentation to determine the exact build process, if a
compatible one is not available, or relying on ancient autoconfigs.

These days, for *most* use cases, things are relatively simple. One
is able to compile or cross-compile for the major operating systems
with a relatively unified code-base, so long as one is not doing
anything particularly platform specific, and are on a reasonably modern
operating system. Processors and compilers are now fast enough, 
that most one-off compiles do not require sophisticated build systems
to manage intermediate objects, or dependency graphs of headers and 
source files. The vast majority of libraries do not need to bother 
with extremely precise flag definitions, or highly specific compilation 
paths. 

This standard *does not* preclude the use of these sophisticated
build systems in a particular project, but rather allows the project
to be structured in such a way that it can follow these standards for
a simpler build process.

### Rationale

The goal of the standard is to make it easy to identify projects
that are extremely straightforward to pull off-the-shelf, and integrate
into any project without regard to build system, or complex build
dependencies.

## Definition of the SCPS

SCPS has six progressively more restrictive levels. 

The following terms are used in their definitions.

### Define Switch

A switch passed to a compiler that defines a macro for the C preprocessor
to interpret. In `gcc`, `clang`, this is the `-D` flag passed to the 
compiler frontend.

### Source Directory Tree (SDT)

A directory tree containing all source files necessary to build the
application. An SDT is defined as only the directories which contain
source code necessary for a simple build of the application; test
code, or example programs, so long as they are indepedent directories
that are either siblings, or ancestors of the SDT are excluded.

### Source Code

Unless otherwise specified, source code refers to code that is necessary
for the simplest possible build of the application, such that the
application can still fulfill its stated function.

### Project

In this document, a project refers to an executable or library
that is contained within a single git repository, that performs
a particular function. Monorepos containing multiple projects
are acceptable.

## SCPS5

In order to conform to SCPS5, the project *must*:

* Be licensed in a under an open-source license. (MIT, Apache, etc..)
* Be a git repository, publically accessible via HTTPS.
* Have all source code necessary in a single source directory tree.
* Have the *SDT* located at either `.` or `src`.
* Have all headers and included files have a file extension of `.h` or `.hpp`
* If C, conforms to the C standard <=C11. If C++, confirms to <=C++11.
* Contain a README/README.md with a list of all dependencies, as well
as all necessary steps to build the library on a standard POSIX system 
using `gcc`/`g++`, or `clang`, including a sample build command that 
should work on most POSIX systems, if all dependencies are built and
installed. If no build (SCPS1 or SCPS0), or dependencies, can be 
excluded.
* Be able to be compiled via a single invocation of most compilers.
* Have no more than three simple pre-build steps. (i.e. copying
input config files, writing simple configuration files, etc..)

### Example

* https://github.com/PCRE2Project/pcre2 (README non-conforming)
* https://github.com/freetype/freetype (README non-conforming)

## SCPS4

In order to conform to SCPS4, the project *must*:

* Conform to SCPS5.
* Have exactly 0 pre-build steps.
* All headers must have a file extension of `.h`.
* Have differing functionality toggled by define switches *only*.
* If a library, be able to be linked via single invocation of most linkers.

### Examples

* https://github.com/lite-xl/lite-xl (README non-conforming)

## SCPS3

In order to conform to SCPS3, the project *must*:

* Conform to SCPS4.
* Bundle dependencies as git submodules, or folders under directory `lib`.
All dependencies must have relatively standard build steps without complex 
use of depedendency configuration options.
* Single line invocation of compiler must target no more than two 
independent files, or bash-like wildcards, and no more than two required
*define switches* for *all* officially supported platforms.

### Examples

* https://github.com/adamharrison/lite-xl-plugin-manager
* https://github.com/adamharrison/lite-xl-simplified 

## SCPS2

In order to conform to SCPS2, the project *must*:

* Conform to SCPS3.
* Have no linking dependencies.
* The SDT must contain no directories that contain source code.

### Examples

* https://github.com/rxi/microtar
* https://github.com/lua/lua (README non-conforming)
* https://github.com/madler/zlib (README non-conforming)

## SCPS1 (Header-Only Libraries)

This is the most restrictive form of the SCPS, and takes the form
of a header-only library. These are commonplace in C and C++, and
represent an extremely simple form of library, that can be included
in a project with a simple set of `#include` statements. Occasionally,
this will also require the use of a definition in a single source file
to emit the actual implementation for the interface.

In order to conform to SCPS1, the project *must*:

* Conform to SCPS2.
* Have all source code be either C, or C++.
* The SDT contains *only* header files.
* No independent build process is necessary.

### Examples

* https://github.com/nothings/stb

## SCPS0 (Single Header-Only Libraries)

A special case of SCPS1, where one repository has exactly
one header file to be included.

In order to conform to SCPS1, the project *must*:

* Conform to SCPS0.
* Contain exactly one `.h` or `.hpp` file.

### Examples

* https://github.com/jeremycw/httpserver.h

# Straightforward C Project Standard (SCPS)

## Abstract

This document outlines a project structure for C(++) programs and libraries
that allows for easy interoperability with compilers, build-systems, 
and linking programs. There are six levels of the standard, each 
with a more severe degree of simplicity.

* [SCPS5](#SCPS5)
* [SCPS4](#SCPS4)
* [SCPS3](#SCPS3)
* [SCPS2](#SCPS2)
* [SCPS1](#SCPS1)
* [SCPS0](#SCPS0)

## Introduction

C currently has no de-facto standard package manager, or build system. 
As such, when integrating C libraries or projects into an overarching 
project, it can be a hassle to build the underlying library, requiring
reading through documentation to determine the exact build process, if 
a compatible one is not available, or relying on ancient autoconfigs.

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

This is also meant to be a **rule of thumb**, rather than anything
extra-rigorous; an easy way to identify libraries that are straightforward
to integrate.

It is not attempt to make any sort of software that handles dependencies
automatically.

## Definitions

SCPS has six progressively more restrictive levels. 

The following terms are used in their definitions.

### Paths

`/` is defined as the repository root, not the system VFS root in this
standard, unless otherwise stated.

### Define Switch

A switch passed to a compiler that defines a macro for the C preprocessor
to interpret. In `gcc`, `clang`, this is the `-D` flag passed to the 
compiler frontend.

### Source Directory Tree (SDT)

A directory tree containing all source files necessary to build the
application. An SDT is defined as only the directories which contain
source code necessary for a simple build of the application; test
code, or example programs, so long as they are indepedent directories
that are either siblings or ancestors of the SDT, or are in the repository
root are excluded from the SDT definition.

### Source Code

Unless otherwise specified, source code refers to code that is necessary
for the simplest possible build of the application, such that the
application can still fulfill its stated function.

### Project

In this document, a project refers to an executable or library
that is contained within a single git repository, that performs
a particular function. Monorepos containing multiple projects
are acceptable.

### Dependency

In this document, when dependency is mentioned, unless otherwise
stated, it is referring *only* to external dependencies by third party
vendors; not system dependencies that are included with almost all versions
of targeted platforms, such as: `winsock`, `libc`, or `pthread`.

## SCPS5

In order to conform to SCPS5, the project **must**:

* Be licensed in a under an open-source license. (MIT, Apache, etc..)
* Be a git repository, publically accessible via HTTPS.
* Have all source code necessary in a single *SDT*.
* Have the *SDT* located at either `/` or `/src`.
* Have all headers and included files have a file extension of `.h` or `.hpp`
* If C, conforms to the C standard <=C11. If C++, confirms to <=C++11.
* Be able to be compiled via a single invocation of most compilers.
* Have no more than three simple pre-build steps. (i.e. copying
input config files, writing simple configuration files, etc..)
* Contain a README/README.md with a list of all dependencies, as well
as all necessary steps to build the library on a standard POSIX system 
using `gcc`/`g++`, or `clang`, including a sample build command that 
should work on most POSIX systems, if all dependencies are built and
installed. If no independent build (SCPS1 or SCPS0), or dependencies, can be 
excluded.

### Example

* https://github.com/PCRE2Project/pcre2 (README non-conforming)
* https://github.com/freetype/freetype (README non-conforming)

## SCPS4

In order to conform to SCPS4, the project **must**:

* Conform to [SCPS5](#SCPS5).
* Have exactly 0 pre-build steps.
* All headers must have a file extension of `.h`.
* If a library, be able to be linked via single invocation of most linkers/archivers.

### Examples

* https://github.com/lite-xl/lite-xl (README non-conforming)

## SCPS3

In order to conform to SCPS3, the project **must**:

* Conform to [SCPS4](#SCPS4).
* Have differing functionality toggled by define switches **only**, with no 
required define switches.
* Bundle dependencies as git submodules, or folders under directory `/lib`.
All dependencies must have relatively standard build steps without complex 
use of depedendency configuration options.
* Single line invocation of compiler must target no more than two 
independent files, or bash-like wildcards.

### Examples

* https://github.com/adamharrison/lite-xl-plugin-manager
* https://github.com/adamharrison/lite-xl-simplified 

## SCPS2

In order to conform to SCPS2, the project **must**:

* Conform to [SCPS3](#SCPS3).
* Have all source code be either C, or C++.
* Have no linking, or include dependencies.
* Have the *SDT* contain no directories that contain source code.

### Examples

* https://github.com/rxi/microtar
* https://github.com/lua/lua (README non-conforming)
* https://github.com/madler/zlib (README non-conforming)

## SCPS1 (Header-Only Libraries)

This is the most restrictive form of the *SCPS*, and takes the form
of a header-only library. These are commonplace in C and C++, and
represent an extremely simple form of library, that can be included
in a project with a simple set of `#include` statements. Occasionally,
this will also require the use of a definition in a single source file
to emit the actual implementation for the interface.

In order to conform to SCPS1, the project **must**:

* Conform to [SCPS2](#SCPS2).
* Have the *SDT* contain **only** header files.
* No independent build process is necessary.

### Examples

* https://github.com/nothings/stb

## SCPS0 (Single Header-Only Libraries)

A special case of [SCPS1](#SCPS1), where one repository has exactly
one header file to be included.

In order to conform to [SCPS0](#SCPS0), the project **must**:

* Conform to [SCPS1](#SCPS1).
* Have *SDT* contain exactly one header file.

### Examples

* https://github.com/jeremycw/httpserver.h

---
title: Compile GCC5.1 from Source Code
date: 2016-08-02 23:05:38
categories: Compiler
tags: GCC
---

# Tested Environment

Ubuntu 16.04 64-bit on Thinkpad T440, 8/2/2016

<!-- more -->

# Installation Guide

1. [Download](https://github.com/gcc-mirror/gcc/releases) and unzip the source file
 * Use `7z` may be easier, `sudo apt install p7zip-full`
 * Run `7z x __FILENAME__` to unzip files

2. Download prerequisites
 * `./contrib/download_prerequisites`, a supercharged way to avoid future problems
 * `sudo apt-get install gcc-multilib flex`, you will run into errors as stated below if you don't have these installed

3. Make a folder for the compiled compiler to be placed
 * `mkdir __THE_DIR_FOR_COMPILER__`, at wherever you want it to be

4. Configure and run makefile
 * `./configure --prefix=$HOME/Desktop/__THE_DIR_FOR_COMPILER__ --enable-languages=c,c++`, I usually put my recent work at Desktop.
    * A handy function for looking up the directory for `--prefix` is `pwd`
 * `make` (This will take a while. To be precise, it took me 145 minutes 51.179 seconds on 8/3/2016)
    * `-j`: compile on multiple cores
 * `make install`

  P.S I usually time the process when the compiler is being built :) Just add `time` in front of the commands aforementioned.

# Notes

* If the re-run is needed (when you need to rebuild after resolving errors), run the following command first:
 `make clean && make distclean`

# Common errors

* Error message: `g++: error: gengtype-lex.c: No such file or directory`
  * Solution: `sudo apt-get install flex`

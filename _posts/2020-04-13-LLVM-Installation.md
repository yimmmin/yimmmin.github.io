---
layout: post
title: LLVM Installation
date: 2020-04-13 13:40:00
categories: code
tags: 编译 LLVM Installation
excerpt: Official Tutorial LLVM的命名最早来源于底层语言虚拟机（Low Level Virtual Machine）的缩写。它是一个用于建立编译器的基础框架，以C++编写。Clang则是LLVM原生的C/C++/Objective-C编译器前端。 1 获取源码 GitHub 官网 需注意，LLVM和clang的源码是分开的，需要将clang源码放置于LLVM源码tools目录下（如果是官网下的源码...
---

[Official Tutorial](http://llvm.org/docs/GettingStarted.html)

LLVM的命名最早来源于底层语言虚拟机（Low Level Virtual Machine）的缩写。它是一个用于建立编译器的基础框架，以C++编写。Clang则是LLVM原生的C/C++/Objective-C编译器前端。

### 1. 获取源码

1. GitHub: [https://github.com/llvm/llvm-project](https://github.com/llvm/llvm-project)
2. 官网: [https://llvm.org](https://llvm.org)

- 需注意，LLVM和clang的源码是分开的，需要将clang源码放置于LLVM源码./llvm/tools目录下（如果是官网下的源码，最好把源码文件夹分别改名为llvm和clang）

### 2. 编译安装

```
cd <path to build>
cmake -G <generator> [options] <path to llvm sources>
cmake --build . [--target <target>]
```
1. path to build编译路径

    虽然网上都说LLVM内部编译会失败，然而我9.0.1和3.2的版本都在LLVM源码目录下建了一个build文件夹成功编译。

2. generator指定编译工具
    - `Ninja`: for generating Ninja build files
    - `Unix Makefiles`: for generating make-compatible parallel makefiles
    - `Visual Studio`: for generating Visual Studio projects and solutions
    - `Xcode`: for generating Xcode projects

    总之Linux选Unix，Windows选Visual Studio，Mac OS选Xcode就行吧，带双引号

3. option编译选项
    - `-DLLVM_ENABLE_PROJECTS='...'`: semicolon-separated list of the LLVM subprojects you’d like to additionally build. Can include any of: clang, clang-tools-extra, libcxx, libcxxabi, libunwind, lldb, compiler-rt, lld, polly, or debuginfo-tests.For example, to build LLVM, Clang, libcxx, and libcxxabi, use -DLLVM_ENABLE_PROJECTS="clang;libcxx;libcxxabi".
    - `-DCMAKE_INSTALL_PREFIX=directory`: Specify for directory the full pathname of where you want the LLVM tools and libraries to be installed (default /usr/local).
    - `-DCMAKE_BUILD_TYPE=type`: Valid options for type are Debug, Release, RelWithDebInfo, and MinSizeRel. Default is Debug.
    - `-DLLVM_ENABLE_ASSERTIONS=On`: Compile with assertion checks enabled (default is Yes for Debug builds, No for all other build types).
    - `--enable-optimized`: 打开优化，默认情况下会关闭，这样会导致生成大量debug信息
    - `--enable-targets=host-only`: 选择目标平台，默认情况下会生成所有平台的，设置host-only则只选择本机。

    所以2和3加起来我选
    ```
    cmake -G "Unix Makefiles" --enable-optimized ..
    ```

4. path to llvm sources源码路径

5. `cmake --build . [--target <target>]`
    - The default target (i.e. `cmake --build .` or `make`) will build all of LLVM.
    - The `check-all` target (i.e. `ninja check-all`) will run the regression tests to ensure everything is in working order.
    - CMake will generate build targets for each tool and library, and most LLVM sub-projects generate their own `check-<project>` target.
    - Running a serial build will be slow. To improve speed, try running a parallel build. That’s done by default in Ninja; for make, use the option `-j NN`, where NN is the number of parallel jobs, e.g. the number of available CPUs.

    CPU性能足够的话强烈建议`make -j NN`，NN可以取CPU核心数两倍，这个NN说法是网上找的，确实快很多。不过加`-j`的话，可能在并行的时候会导致编译关连错误，报错的话适当减少一下NN数即可。

### 3. 重新编译

```
make clean
make -j32
```

### 参考连接

1. [安装 LLVM + Clang - juniway - 简书](https://www.jianshu.com/p/598b7094b8c1)
# CMake自学笔记

在[cmake-examples](http://ttroy50.github.io/cmake-examples/)仓库的基础上，做的翻译笔记。

# 介绍

CMake是一个跨平台的开源元构建系统，其可以构建、测试和打包软件。他被用来创建多种本地构建环境，例如：苹果的xcode和微软的Viusal Studio

这个仓库将包含一些现代的CMake配置实例，这些实例的格式类似于教程。第一个例子非常的基础，并且会在前面的基础上慢慢增加复杂性，以展示更加复杂的案例。

这些实例已经在Ubuntu 16.04上验证国了，但是可以在任何支持CMake3.5+的Linux上允许。

# 需要

大多数案例的基础需求是：

* CMake v3.5+
* 一个c++的编译器（默认是gcc）
* make

# 在Ubuntu上安装

最简单的安装方法如下：

```bash
$ sudo apt-get install build-essential
$ sudo apt-get install cmake
```

某些特定的案例可能需要其他工具包括

* [boost](http://www.boost.org/)

  ```bash
  $ sudo apt-get install libboost-all-dev
  ```

* [protobuf](https://github.com/google/protobuf)

  ```bash
  $ sudo apt-get install libprotobuf-dev
  $ sudo apt-get install protobuf-compiler
  ```

* [cppcheck](http://cppcheck.sourceforge.net/)

  ```bash
  $ sudo apt-get install cppcheck
  ```

* [clang](http://clang.llvm.org/)

  ```bash
  $ sudo apt-get install clang-3.6
  ```

* [ninja](https://ninja-build.org/)

  ```bash
  $ sudo apt-get install ninja-build
  ```

* [conan](https://conan.io/)

  ```bash
  $ sudo apt-get install python3 python3-pip
  $ sudo pip3 install conan
  ```

## Docker

这是一个包含了所有需求和多个CMake版本的docker容器，以便用来测试案例。这是docker hub仓库的地址[matrim/cmake-examples](https://hub.docker.com/r/matrim/cmake-examples/)。

构建所欲的cmake-example测试集你可以执行以下命令:

```bash
docker run -it matrim/cmake-examples:3.5.1
cd ~
git clone https://github.com/ttroy50/cmake-examples.git code
cd code
./test.sh
```

更多的构建和运行docker容器的细节 [dockerfiles](https://github.com/xhguleixin123/cmake-examples/blob/master/here)。

# Other Links

这些是在线CMake教程。以下是在作者CMake生涯中产生帮助的教程链接。

- [Modern CMake Slides](https://web.archive.org/web/20160314094326/https://www.kdab.com/~stephen/moderncmake.pdf)
- [rix0r Modern CMake Blog](https://rix0r.nl/blog/2015/08/13/cmake-guide/)
- [Official CMake Tutorial](https://cmake.org/cmake-tutorial/)
- [Official CMake Wiki](https://gitlab.kitware.com/cmake/community/wikis/home)
- [CMake Useful Variables](https://gitlab.kitware.com/cmake/community/wikis/doc/cmake/Useful-Variables)
- [Derek Molloy - Intro to CMake](http://derekmolloy.ie/hello-world-introductions-to-cmake/)
- [Modular C++ Projects](http://techminded.net/blog/modular-c-projects-with-cmake.html)
- [Common CMake Anti-Patterns](https://web.archive.org/web/20190320121339/http://voices.canonical.com/jussi.pakkanen/2013/03/26/a-list-of-common-cmake-antipatterns/)
- [Using clang static analyser with CMake](http://baptiste-wicht.com/posts/2014/04/install-use-clang-static-analyzer-cmake.html)
- [Static Analysis with CDash](https://cmake.org/pipermail/cmake/2011-April/043709.html) - Includes some info about using CppCheck with CMake
- [CMake Tips](https://samthursfield.wordpress.com/2015/10/20/some-cmake-tips/)
- [John Lamp - CMake Tutorial](https://www.johnlamp.net/cmake-tutorial.html)
- [Conan Documentation](https://docs.conan.io/)
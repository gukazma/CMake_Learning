# C++标准

随着C++11和C++14发布后，一个一般的案例就是调用编译器来使用这些标准。随着CMake的发展，它增加了一些功能，使这变得更容易，新版本的CMake改变了实现这一点的方式。

下面的例子展示了设置c++标准的不同方法，以及它们可以从哪个版本的CMake中获得。

案例包括：

* Common-method 一个一般的方法，适用于大部分CMake版本。
* cxx-standard 使用`CMake_CXX_STANDARD`变量，在CMake v3.1时被引入。
* compile-feature 使用`target_compile_features`函数，在CMake v3.1时被引入。


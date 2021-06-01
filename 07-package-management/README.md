# 包管理

# 系统自带的包管理

系统安装的包会自动安装到默认位置，CMake的`find_pagckage`函数会找到这个包，参考01-basic/H-third-party-library。

# Vendering code

利用`git sub-modules`或者`git subtree`命令，可以自动获取第三方包。然后使用CMake的`add_subdirectory()`函数，来链接这个第三方包。参考02-sub-projects。
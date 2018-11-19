# 安装
    npm install sharp
    yarn add sharp
### 条件
* Node v4.5.0+

#### 从源代码构建
为sharp提供的预编译二进制文件可用于使用(6、8和10)版本node的64位Windows、OS X和Linux平台

sharp会打包这些资源当如下内容安装后：
* 全局安装的 libvips
* 对于当前平台和node版本，不存在预编译的二进制文件 或当命令npm install --build-from-source 执行时

#### 从代码构建需要
* C++11 稳定版本编译工具例如 gcc 4.8+、 clang 3.0+ 或 MSVC 2013+
* [node-gyp](https://github.com/nodejs/node-gyp#installation) 和它的依赖(包括 Python 2.7)

### libvips

#### linux
libvips和它的依赖被下载放在 `node_modules/sharp/vendor` 目录下当执行 `npm install` 命令时。这涉及到大约8MB的自动HTTPS下载。

大多数运行在x64和ARMv6+ cpu上的基于linux的(glibc, musl)操作系统应该“正常工作”，例如:

* Debian 7+
* Debian 7+
* Ubuntu 14.04+
* Centos 7+
* Alpine 3.8+ (Node 8 and 10)
* Fedora
* openSUSE 13.2+
* Archlinux
* Raspbian Jessie
* Amazon Linux
* Solus

要使用全局安装的libvip版本，而不是提供的二进制文件，请确保它至少是`package.json`列出的在`config.libvips`下的版本，它可以通过`pkg-config -modversion vips-cpp`找到。

如果您使用非标准路径(除了 `/usr` 或 `/usr/local` 之外的任何路径)，您可能需要在 `npm install` 期间设置 `PKG_CONFIG_PATH`，并在运行时设置 `LD_LIBRARY_PATH`。

允许使用更新版本的libvip和较旧版本的sharp。

[对于32位的Intel cpu和较老的基于linux的操作系统(如Centos 6)，建议从源代码编译libvip。](https://libvips.github.io/libvips/install.html#building-libvips-from-a-source-tarball)

#### Alpine Linux

libvip在[测试库](https://pkgs.alpinelinux.org/packages?name=vips-dev)中可用:

    apk add vips-dev fftw-dev build-base --update-cache --repository https://dl-3.alpinelinux.org/alpine/edge/testing/

musl libc的堆栈大小较小，这意味着libvip可能需要在没有缓存的情况下通过 `sharp.cache(false)`使用，以避免堆栈溢出。


#### Mac OS

在npm安装期间，libvips及其依赖项被提取并存储在`node_modules/sharp/vendor`中。这涉及到大约7MB的自动HTTPS下载。

要使用您自己的libvip版本而不是提供的二进制文件，请确保它至少是`package.json`列出的在`config.libvips`下的版本。它可以通过`pkg-config -modversion vips-cpp`找到。


#### Windows x64

在npm安装期间，libvips及其依赖项被提取并存储`1node_modules\sharp\vendor`中。这涉及到一个大约13MB的自动HTTPS下载。

只有64位的node是支持的。


更多安装方式请参考：[libvips](http://sharp.pixelplumbing.com/en/stable/install/#libvips)
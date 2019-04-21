---
title: CentOS6.6编译安装Clang7.0.0
mathjax: false
toc: true
category:
  - Clang
tags:
  - clang
  - centos
abbrlink: 2372579481
date: 2018-12-08 15:52:16
---
# 下载安装包
编译安装`Clang`需要下载7个文件，分别是

1、`llvm-7.0.0.src.tar.xz`
2、`cfe-7.0.0.src.tar.xz1`
3、`clang-tools-extra-7.0.0.src.tar.xz`
4、`compiler-rt-7.0.0.src.tar.xz`
5、`libcxx-7.0.0.src.tar.xz`
6、`libcxxabi-7.0.0.src.tar.xz`
7、`libunwind-7.0.0.src.tar.xz`

```bash
version=7.0.0
url=http://releases.llvm.org/$version
file1=llvm-$version.src
file2=cfe-$version.src
file3=clang-tools-extra-$version.src
file4=compiler-rt-$version.src
file5=libcxx-$version.src
file6=libcxxabi-$version.src
file7=libunwind-$version.src

for file in $file{1..7}
do
    [ -f $file.tar.xz ] || wget $url/$file.tar.xz
done
```

# 编译安装
```bash
# 进入安装脚本所在的目录
path=$PWD/$0
cd ${path%/*}

# 获取 clang 版本号
for file in `ls .`
do
    [[ $file =~ "llvm" ]] && version=$file
done
version=${version/*-}
version=${version%.src*}

# 新建 build 目录
mkdir build && cd build

# 解压
for file in $file{1..7}
do
    tar xvf ../$file.tar.xz
done

# 移动目录
mv $file2 $file1/tools/clang
mv $file3 $file1/tools/clang/tools/extra
mv $file4 $file1/projects/compiler-rt
mv $file5 $file1/projects/libcxx
mv $file6 $file1/projects/libcxxabi
mv $file7 $file1/projects/libunwind

# 编译及安装
# 由于集群上默认gcc版本太低，所以必须切换到较高版本。
# 即便如此，仍需在shell中设定CC、CXX或者添加CMAKE选项指定C、CXX编译器才能通过检测

export CC=gcc
export CXX=g++

# 若进行了上面的设置，则可省去选项 -DCMAKE_C_COMPILER 和 -DCMAKE_CXX_COMPILER
cmake -G"Unix Makefiles" \
    -DCMAKE_CXX_COMPILER=/path/to/specified/g++ \
    -DCMAKE_C_COMPILER=/path/to/specified/gcc \
    -DCMAKE_INSTALL_PREFIX=/path/to/installation \
    -DCMAKE_BUILD_TYPE=Release  \
    -DLLVM_TARGETS_TO_BUILD="X86" \
    $file1
    # -DCLANG_DEFAULT_CXX_STDLIB=libc++ \

make -j 4
make install
make install-cxx install-cxxabi
```

# 打包clang动态库
手动安装`clang`的目的主要是为了安装`YouCompleteMe`，需要用到的其实就只有一个动态库文件`libclang.so.7`。
```bash
mkdir lib
cp -l /path/to/installation/lib/libclang.so* lib
tar -cjf libclang-$version-x86_64-unknown-linux-gnu.tar.bz2 lib
rm -rf lib
```

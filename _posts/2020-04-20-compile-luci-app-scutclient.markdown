---
layout: post
title:  "用openwrt源码编译luci-app-scutclient和scutclient"
date:   2020-04-20 18:46:03 +0800
---
为方便起见，假设环境为wsl2
编译全程需要联网
## 前期准备
```bash
cd ~
sudo apt update
sudo apt ungrade
sudo apt install g++ libncurses5-dev zlib1g-dev bison flex unzip autoconf gawk make gettext gcc binutils patch bzip2 libz-dev asciidoc subversion
```

## 源码下载
```bash
git clone https://git.openwrt.org/openwrt/openwrt.git
git clone https://github.com/scutclient/luci-app-scutclient
git clone https://github.com/scutclient/scutclient.git
```

## 编译环境设置
```bash
cd openwrt
./scripts/feeds update -a
mkdir package/scutclient
cp ../scutclient/openwrt/Makefile package/scutclient/
cp -r ../luci-app-scutclient feeds/luci/applications/
./scripts/feeds install -a
export PATH=/home/cq/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
# 在wsl中，没有这行编译会报错
```

## 编译选项配置
``` bash
make menuconfig
```
1. Target System、Subtarget、Target Profile选择目标操作系统
2. LuCI-->Applications-->luci-app-scutclient 选上(按空格选中)
3. 其它选项看情况选择
4. 保存

## 编译
```bash
make -j 12 V=99
```
- 编译过程需要联网，若 网速较慢则编译耗时较长
- 12改成cpu线程数
- 若编译失败，可以尝试 make -j 1 V=99
- 编译完成以后，可在bin\目录下找到相应的ipk和系统映像
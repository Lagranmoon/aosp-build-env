# 使用Docker构建Android编译环境
基于Android和官方教程构建Android编译环境

编译AOSP版本为Android7.0，Ubuntu版本为14.04，jdk版本为openjdk1.8，Docker版本为17.03.1-ce

# Dockerfile文件
```
FROM ubuntu:14.04
Run sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list\
    #更换软件源并更新
    && apt update \ 
    #安装wget工具，下载和安装jdk8
    && apt install -y wget \
    && wget https://mirrors.ustc.edu.cn/ubuntu/pool/universe/o/openjdk-8/openjdk-8-jre-headless_8u45-b14-1_amd64.deb \
            https://mirrors.ustc.edu.cn/ubuntu/pool/universe/o/openjdk-8/openjdk-8-jre_8u45-b14-1_amd64.deb \
            https://mirrors.ustc.edu.cn/ubuntu/pool/universe/o/openjdk-8/openjdk-8-jdk_8u45-b14-1_amd64.deb \
    && dpkg -i *.deb || apt -f -y install \
    #开启64位系统的32位支持  
    && dpkg --add-architecture i386 && apt update \
    && apt install -y lib32z1 lib32ncurses5 lib32bz2-1.0 \
    #安装相关编译工具和包
    && apt install -y git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev gcc-multilib \
                      g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev \
                      ccache libgl1-mesa-dev libxml2-utils xsltproc unzip python-networkx libnss-sss:i386 \
    #删除下载的jdk安装包
    && rm -rf *.deb
```

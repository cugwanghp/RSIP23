# 课设开发环境配置--VS2017+Qt5+GDAL

> 工欲善其事，必先利其器。以下对VS2017+Qt5+GDAL库的配置做简要说明。

## 一、工具下载
### 1.1 VS2017下载
- [VS2017官方地址](https://visualstudio.microsoft.com/zh-hans/downloads/?rr=https%3A%2F%2Fsupport.microsoft.com%2Fzh-cn%2Fhelp%2F2977003%2Fthe-latest-supported-visual-c-downloads)

### 1.2 Qt下载
- [Qt5.10.1下载地址](http://download.qt.io/archive/qt/5.10/5.10.1/qt-opensource-windows-x86-5.10.1.exe)

- 或者其他[Qt版本](http://download.qt.io/archive/qt)

### 1.3 GDAL下载
- [GDAL开发版下载](http://www.gisinternals.com/sdk.php)
> 注意选择正确的版本。
 
## 二、开发环境安装与配置
### 2.1 VS2017安装
下载安装包，在线安装VS2017。安装流程参考 [VS2017安装流程](https://jingyan.baidu.com/article/a948d6512f00d70a2dcd2edc.html)

#### 2.2. Qt Setup
- Qt安装安装步骤参照<https://blog.csdn.net/liang19890820/article/details/53931813#%E9%85%8D%E7%BD%AE-qt-creator>

> 注意在安装的时候一定要勾选MSVC2017的编译器版本。

#### 2.3. GDAL配置
下载的GDAL开发库，主要包括include头文件、lib库文件、bin动态库文件。在VC++目录中配置相应的include、lib目录即可。同时，调试和执行时，gdal动态库需要拷贝到执行程序目录下。

---

**更为详细的配置说明文档，参照113162班陈瑞麟同学分享的[配置文档](./%E9%85%8D%E7%BD%AEGDAL%2BVS2017%2BQT.pdf)。Thank you！！**

---
[Home](https://wanghp119.github.io/RSIP/) | [Return](#目录)  | [Next](./D2_RasterIO.md)


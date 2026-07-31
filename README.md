# Alioth-Kernel-Droidspaces
* 本仓库为 Alioth-Kernel-Droidspaces 的仓库，仅存储为 红米K40 / 小米11X / POCO F3 编译的 Alioth-Kernel-Droidspaces 内核
----
Droidspaces 完整详细介绍
 
一、项目基础定位
 
Droidspaces（开源仓库名：Droidspaces-OSS，作者ravindu644）是Android平台专用Linux容器运行时，对标LXC/LXD，专为手机/平板设计，依靠Linux内核Namespace、Cgroups实现完整容器隔离，解决传统chroot、Termux无法正常运行标准Linux（带systemd）的痛点。
简单概括：在安卓手机上跑完整、隔离的标准Linux发行版（Debian/Ubuntu/Alpine等），性能接近原生，比虚拟机轻量、比chroot隔离更强。
 
核心底层原理
 
1. 区别于chroot
chroot仅修改根目录，无进程/网络/PID隔离，容器内进程可直接杀死安卓系统后台，不支持systemd；
Droidspaces启用全套内核隔离特性：独立PID树、挂载表、主机名、IPC、Cgroup层级，每个容器像一台轻量裸机。
2. 零性能损耗
共享安卓主机Linux内核，无需虚拟机Hypervisor，CPU/内存直通，性能远超QEMU、VMOS等安卓虚拟机。
3. 支持完整init系统
容器内PID1可以是 systemd /OpenRC，完整支持服务管理、日志、开机自启，和电脑端Linux行为完全一致，这是Termux、普通chroot做不到的核心优势。
 
二、核心特性与功能
 
1. 容器隔离能力
 
- 三种网络模式：Host（共享手机网络）、NAT（独立容器内网）、None（断网隔离）
- 端口转发、自定义主机名、独立用户权限（支持 sudo 提权）
- 自定义绑定挂载（Bind Mount）：可把手机存储、GPU设备、音频设备映射进容器，实现图形/硬件加速
- 资源限制：CPU、内存Cgroup配额，防止容器占用全部手机硬件资源
 
2. 图形界面（Android APP）
 
自带原生安卓GUI管理工具，无需敲复杂命令行：
 
- 多容器批量管理：创建、启动、停止、删除、导出/导入rootfs镜像
- 一键内核兼容性检测，自动列出缺失的内核Namespace功能
- 可视化Systemd服务面板：启停、启用/禁用Linux系统服务（NetworkManager、ssh等）
- WebUI远程管理、后台守护进程、屏幕常亮 wakelock 防休眠
- 多语言支持（含简体中文），最新v6.3.0完善中文文档与图形音频配置向导
 
3. 系统与硬件兼容
 
架构支持
 
静态单二进制文件（仅260KB，无依赖），统一包支持：aarch64(主流安卓)、armhf、x86_64、x86平板/模拟器。
 
支持Linux发行版
 
Debian 13、Ubuntu、Alpine、Fedora等标准glibc发行版（Termux仅Bionic libc，大量软件无法运行）。
 
硬件扩展
 
- GPU硬件加速：VirGL、dma_heap设备挂载，可跑XFCE/KDE桌面
- 音频转发：PulseAudio直通容器，容器内播放声音、麦克风可用
- 配合Termux-X11实现完整Linux图形桌面输出到手机屏Linux kernel
============

There are several guides for kernel developers and users. These guides can
be rendered in a number of formats, like HTML and PDF. Please read
Documentation/admin-guide/README.rst first.

In order to build the documentation, use ``make htmldocs`` or
``make pdfdocs``.  The formatted documentation can also be read online at:

    https://www.kernel.org/doc/html/latest/

There are various text files in the Documentation/ subdirectory,
several of them using the Restructured Text markup notation.
See Documentation/00-INDEX for a list of what is contained in each file.

Please read the Documentation/process/changes.rst file, as it contains the
requirements for building and running the kernel, and information about
the problems which may result by upgrading your kernel.

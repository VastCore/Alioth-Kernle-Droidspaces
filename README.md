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
- 配合Termux-X11实现完整Linux图形桌面输出到手机屏幕
 
4. 底层技术优势
 
- 静态musl编译单文件，无需额外依赖，可在安卓Rec、ramdisk中直接运行
- 自动处理安卓特有问题：SELinux冲突、加密分区、内核兼容性补丁、安卓特殊权限限制
- 兼容KernelSU/Magisk root，自动适配主流定制内核
 
三、运行前置硬性要求（门槛较高）
 
1. 设备必须解锁BL（Bootloader）
小米、一加、高通机型相对友好；大部分国产锁BL机型无法使用。
2. 内核支持全套Linux Namespace
要求内核开启：UTS、PID、mount、IPC、user、cgroup、net namespace；
内核版本最低3.18，推荐5.15/6.x新版内核；原厂内核大多缺失，需刷社区修补内核或自行编译打补丁。
3. 完整Root权限
KernelSU / Magisk，普通免root方案无法运行（底层容器需要内核权限）。
4. 芯片适配现状
- 高通骁龙：社区完善，大量预编译内核，稳定性最好
- 天玑联发科：兼容性差，GPU加速难，仅命令行服务器可用，桌面极易崩溃
- 三星、华为锁BL机型基本不可用
 
四、适用场景
 
1. 手机便携Linux服务器
搭建SSH、Web服务、数据库、Python/Go开发环境、Docker（容器套容器）。
2. 完整Linux桌面环境
搭配Termux-X11运行XFCE桌面，手机变迷你笔记本，编译代码、办公。
3. 开发测试
隔离环境编译程序、测试服务，不污染安卓主机系统。
4. 离线工具环境
离线爬虫、AI推理、脚本自动化，脱离电脑随时使用。
 
五、对比同类安卓Linux方案
 
方案 隔离强度 Systemd支持 性能 使用门槛 
Droidspaces 高（完整Namespace） ✅完美支持 原生零损耗 高（解锁BL+定制内核+root） 
普通chroot 极低 ❌基本失效 良好 中（仅root） 
Termux 无隔离（Bionic libc） ❌不支持 良好，但软件兼容差 极低（无需root） 
LXC安卓版 中 支持 良好 极高（官方停止维护，配置复杂） 
QEMU虚拟机 完全隔离 ✅ 损耗大 中（无需解锁BL） 
 
六、优缺点总结
 
优点
 
1. 唯一安卓端低成本实现标准systemd完整Linux的容器方案
2. 内核级强隔离，安全性远高于chroot、Termux
3. 无虚拟化开销，CPU/内存性能拉满，远超虚拟机
4. 可视化GUI，新手友好，不用记忆大量LXC命令
5. 完善图形、音频、GPU硬件加速生态
6. 轻量单二进制，可在Recovery环境运行
 
缺点
 
1. 使用门槛极高：必须解锁BL+修补内核+root，原厂系统基本无法直接用
2. 联发科芯片适配差，桌面环境容易死机重启
3. 部分厂商GKI内核存在兼容性bug（一加、小米部分机型偶发重启）
4. 不能免root，普通用户无法体验
5. 图形桌面配置步骤繁琐，踩坑点多
 
七、版本与开源信息
 
- 开源仓库：GitHub ravindu644/Droidspaces-OSS
- 当前稳定版：v6.3.0（2026年7月更新），新增统一图形音频配置文档、中文翻译、远程WebUI优化、防休眠适配
- 授权：开源自由使用，社区提供适配内核、镜像、图文教程（酷安、技术博客为主）
 
八、与Termux核心区别（容易混淆）
 
1. Libc底层：Termux用安卓Bionic，大量标准Linux软件编译失败；Droidspaces容器内是标准glibc，Debian/Ubuntu软件源全部兼容。
2. 系统服务：Termux无systemd，无法管理后台常驻服务；Droidspaces原生支持完整systemd服务栈。
3. 隔离：Termux和安卓共享进程空间；Droidspaces容器进程完全隔离，互不干扰。
4. 图形：Termux图形依赖第三方补丁；Droidspaces原生支持GPU直通、X11桌面。
---
> 该内核基于 github@liyafe1997/kernel_xiaomi_sm8250_mod 修改

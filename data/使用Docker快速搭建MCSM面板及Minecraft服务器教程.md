# 使用Docker快速搭建MCSM面板及Minecraft服务器教程

## 前言
本教程将指导您通过Docker容器技术快速部署MCSManager面板和Minecraft服务器。Docker方案能方便切换不同Java版本，支持同时运行多个MC服务端实例，是开服玩家的理想选择。

## 核心组件介绍

### MCSManager面板
MCSManager（简称MCSM）是一款轻量级、全中文的Minecraft服务端管理面板，具有以下优势：
- 开箱即用的设计
- 支持多实例管理
- 完善的Docker集成
- 完善的安全权限系统
- 适合个人和商业用途

### Mohist服务端
Mohist是一个优秀的Minecraft Forge服务端实现，特点包括：
- 同时支持Bukkit/Spigot/Paper API
- 完美兼容模组和插件
- 提供出色的运行稳定性
- 优化后的高性能表现

👉 [【点击查看】2025年最新雨云优惠码及特价云服务器方案汇总](https://bit.ly/RainYun)

## 准备工作

### 服务器配置建议
根据Minecraft版本不同，推荐配置如下：
- 1.17+高版本：建议5900X/13900K等高主频CPU，4核8G内存起
- 1.16.5及以下：1核2G即可流畅运行
- Mod服：建议4核8G或更高配置

## 详细安装步骤

### 1. 服务器环境准备
bash
# 更新系统并安装必要工具
apt update && apt install -y wget git

### 2. 安装MCSM面板
bash
# 一键安装脚本（支持Ubuntu/Debian/CentOS等）
wget -qO- https://gitee.com/mcsmanager/script/raw/master/setup.sh | bash

# 启动服务
systemctl start mcsm-{daemon,web}

# 设置开机自启
systemctl enable mcsm-{daemon,web}.service

### 3. 配置Docker环境
bash
# 安装Docker
apt install -y docker.io

# 启动Docker服务
systemctl enable docker && systemctl start docker

### 4. 面板初始化
1. 访问 `http://服务器IP:23333` 
2. 设置管理员账号密码
3. 在"节点管理"中添加服务器节点
4. 端口映射配置：23333、24444、25565

### 5. 创建Java环境镜像
1. 进入"镜像管理"
2. 选择创建OpenJDK 17镜像（1.17+版本推荐）
3. 等待镜像构建完成

### 6. 部署Mohist服务端
1. 从[Mohist官网](https://new.mohistmc.com)下载核心文件
2. 在面板创建新实例
3. 上传服务端jar文件
4. 配置参数：
   - 进程启动方式：虚拟化容器
   - 环境镜像：mcsm-openjdk:17
   - 网络模式：host
5. 设置25565端口映射

### 7. 启动服务器
1. 编辑eula.txt同意用户协议
2. 保存配置后启动实例
3. 在游戏中通过"服务器IP:映射端口"连接

## 常见问题
- 端口不通：检查防火墙和端口映射
- 启动失败：确认Java版本与MC版本匹配
- 性能不足：升级服务器配置

通过本教程，您已成功搭建了专业的Minecraft服务器环境。MCSM面板将大大简化日常运维工作，而Docker方案则提供了灵活的版本管理能力。
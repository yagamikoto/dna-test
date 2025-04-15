# Forsaken Mail 临时邮箱搭建教程及最新云服务器优惠活动

## 一、Forsaken Mail 临时邮箱搭建指南

Forsaken Mail 是一款开源的临时邮箱系统，支持自定义邮箱前缀，邮件阅后即焚。本文将详细介绍通过 Docker 和 NPM 两种方式部署该系统的完整流程。

### 准备工作
- **VPS 要求**：确保服务器已开启 25 端口（需联系主机商开通）
- **域名配置**：为 `xx.com` 添加以下 MX 解析记录：
  
  @     MX  10  mx.xx.com
  mx    A      服务器IP
  

### 方法一：NPM 安装方案

#### 1. 环境配置
bash
# Debian/Ubuntu
curl -sL https://deb.nodesource.com/setup_10.x | bash -
apt-get install -y nodejs git screen

# CentOS
curl -sL https://rpm.nodesource.com/setup_10.x | bash -
yum install nodejs git screen -y

#### 2. 部署服务
bash
git clone https://github.com/malaohu/forsaken-mail.git
cd forsaken-mail
npm install
npm install -g pm2

# 关闭冲突服务
killall sendmail
/etc/init.d/postfix stop
chkconfig --level 2345 postfix off
chkconfig --level 2345 sendmail off

# 启动服务
pm2 start bin/www
pm2 startup
pm2 save

#### 3. 防火墙配置（CentOS）
bash
# CentOS 6
iptables -I INPUT -p tcp --dport 3000 -j ACCEPT
service iptables save && service iptables restart

# CentOS 7
firewall-cmd --zone=public --add-port=3000/tcp --permanent
firewall-cmd --reload

### 方法二：Docker 快速部署
bash
# 安装Docker
curl -sSL https://get.docker.com/ | sh
systemctl start docker && systemctl enable docker

# 启动容器
docker run --name forsaken-mail -d -p 25:25 -p 3000:3000 denghongcai/forsaken-mail

### HTTPS 安全访问配置
使用 Caddy 实现 HTTPS 反代：
bash
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubiBackup/doubi/master/caddy_install.sh
chmod +x caddy_install.sh && bash caddy_install.sh

# 配置Caddyfile
echo "xx.com {
 gzip
 tls admin@xkx.me
 proxy / mx.xx.com:3000
}" > /usr/local/caddy/Caddyfile

/etc/init.d/caddy start

👉 [【点击查看】2025年最新 Hosteons 优惠码及特价云服务器方案汇总](https://bit.ly/hosteons)

## 二、最新云服务器优惠活动

### 限时特惠方案
1. **OVZ套餐年付5折**  
   优惠码 `RECURR50`：适用于 OVZ1-OVZ7 套餐年付方案

2. **KVM套餐8折优惠**  
   优惠码 `RECURR20`：适用于洛杉矶/纽约机房 KVM VPS1-VPS7 套餐

3. **免费Windows授权**  
   年付 KVM VPS3-VPS7 套餐可免费获取 Windows Server 2019 授权（下单后提交工单激活）

4. **增值服务**  
   - 免费 DirectAdmin 控制面板（KVM VPS2+/OVZ VPS2+）
   - 免费 Blesta 授权（提交工单获取）
   - 免费套餐升级（适用于 KVM VPS1-VPS6 / OVZ VPS1-VPS6）
   - 双倍存储空间（使用优惠码 `DOUBLEDISK`）

> 提示：部分优惠可叠加使用，建议通过工单咨询最优配置方案
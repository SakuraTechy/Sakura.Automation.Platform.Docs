# 快速开始

## 环境准备

### 服务器要求

准备一台服务器（虚拟机，实体机，阿里云，腾讯云，华为云）都可以，要求如下：

:::tip
公司部署使用，推荐内部虚拟机或实体机服务器，可以通过内网访问，速度相对较快，没有带宽限制

个人部署使用，推荐阿里云，腾讯云，华为云等，可以通过外网访问，速度相对较慢，会有带宽限制
:::

- CPU：4核或8核（推荐 8核）
- 内存：8G或16G（推荐 16G）
- 硬盘：200G或400G（推荐系统盘200G + 数据盘200G）
- 网络：可访问互联网

### ISO 镜像下载

- [点击下载](http://172.19.5.227:8079/Sakura.Automation.Platform/iso/Sakura.Automation.Platform_V1.6.0.iso)

### ISO 镜像制作

推荐使用Mondo Rescue进行Linux系统镜像制作，可进行备份和还原。

- 官方地址：[点击访问](http://www.mondorescue.org/)
- 使用教程：[点击访问](https://www.cnblogs.com/safe-rabbit/p/17550182.html)

![alt text](image-17.png)

## 虚拟机部署

### 安装教程

推荐使用VMware Workstation，下载最新版本，采用默认设置安装即可。

- 官方地址：[点击访问](https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html)
- 官方教程：[点击访问](https://www.vmware.com/support/ws55/doc/ws_new_install.html)

### 创建虚拟机

- 启动虚拟机，创建新的虚拟机，采用镜像安装方式，选择下载ISO镜像文件。
![alt text](image.png)
- 指定虚拟机名称和安装位置。
![alt text](image-1.png)
- 设置磁盘容量大小。
![alt text](image-2.png)
- 完成创建。
![alt text](image-3.png)

### 启动虚拟机

完成创建虚拟机后，启动该虚拟机，使用默认账号和密码进入系统。

:::tip

- 账号：root
  
- 密码：3edc$RFV
  
:::
![alt text](image-5.png)

## 实体机部署

### 软件下载

推荐使用UltraISO，下载最新版本，采用默认设置安装即可。

- 官方地址：[点击访问](https://cn.ultraiso.net/xiazai.html)
- 官方教程：[点击访问](https://cn.ultraiso.net/jiaochengzhongxin.html)

### U盘启动

- 双击打开UltraISO，点击文件->打开->选择下载好的ISO文件->打开。
![alt text](image-8.png)
- 点击启动->写入硬盘映像。(写入方式建议使用USB-HDD+，隐藏启动分区选 无)。
![alt text](image-9.png)
- 选择对应U盘，进行格式化，点击开始
![alt text](image-10.png)
- 点击开始，弹出数据将格式化警告，确定后开始写入
![alt text](image-11.png)
- 点击写入，选择是，耐心等待写入完成，制作成功！
![alt text](image-13.png)
![alt text](image-12.png)
- 把U盘插到服务器，设置开机U盘启动，输入nuke，回车。
![alt text](image-14.png)
- 自动恢复中，等待恢复完成
![alt text](image-20.png)
![alt text](image-18.png)
- 恢复完成后，输入exit，重启系统。
![alt text](image-16.png)
- 使用默认账号和密码进入系统。

:::tip

- 账号：root
  
- 密码：3edc$RFV
  
:::
![alt text](image-19.png)

## 修改IP地址

进入系统后，修改IP地址，然后重启网络。
:::tip
修改前请先ping一下IP地址，确保该IP地址可用，且未被其他人使用，不然会IP冲突。
:::

```bash
# 修改IP地址
vi /etc/sysconfig/network-scripts/ifcfg-ens192

# 参考以下配置
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no #默认是yes，修改为no
BOOTPROTO=static # 默认是dhcp，修改为static
DEFROUTE=yes
IPV4_FAILURE_FATAL=yes # 默认是no，修改为yes
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens192
UUID=6280f4db-5773-4760-b83c-e5fc57cc086c
DEVICE=ens192
ONBOOT=yes  # 默认是no，修改为yes
IPADDR=172.19.5.227 # 修改为你的IP地址，没有就新增
GATEWAY=172.19.5.1  # 修改为你的网关，没有就新增
NETMASK=255.255.255.0 # 修改为你的子网掩码，没有就新增
DNS1=114.114.114.114 # 修改为你的DNS，没有就新增
ZONE=public

# 重启网络
service network restart
```

## 网站配置

:::tip

默认已经安装好1Panel面板并创建好前端访问站点，可直接使用，详情请进入1Panel网站面板查看

- 地址：http://172.19.5.227:28134/c0464ecc59 （地址中的IP请改为你的服务器IP）
- 账号：root
- 密码：3edc$RFV
- 路径：打开左侧网站菜单，选择172.19.5.227:8083，修改配置文件

```bash
# 参考以下配置
server {
    listen 8083 ; 
    server_name 172.19.5.227; 
    index index.php index.html index.htm default.php default.htm default.html; 
    try_files $uri $uri/ /index.html; 
    proxy_set_header Host $host; 
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
    proxy_set_header X-Forwarded-Host $server_name; 
    proxy_set_header X-Real-IP $remote_addr; 
    proxy_http_version 1.1; 
    proxy_set_header Upgrade $http_upgrade; 
    proxy_set_header Connection "upgrade"; 
    access_log /www/sites/sakura.automation.platform.web.prod/log/access.log; 
    error_log /www/sites/sakura.automation.platform.web.prod/log/error.log; 
    location ^~ /.well-known/acme-challenge {
        allow all; 
        root /usr/share/nginx/html; 
    }
    location /api {
        rewrite ^/api/(.*)$ /$1 break; 
        proxy_pass http://172.19.5.227:8084; 
        index index.html; 
    }
    root /www/sites/sakura.automation.platform.web.test/index/dist; 
}
```

:::
![alt text](image-15.png)

## 访问域名

客户端通过浏览器访问以下地址，输入默认账号和密码，即可开始使用。
:::tip

- 地址: http://172.19.5.227:8083（地址中的IP请改为你的服务器IP）
- 账号：管理员
- 密码：111111
- 可自己注册账号使用

:::
![alt text](image-7.png)
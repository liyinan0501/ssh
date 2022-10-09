# SSH

SSH Secure Shell 是一种能提供安全远程登录会话的协议。

作用

- 远程连接
- 远程文件传输。

默认端口是：22、

SSH服务名：sshd(ssh daemon) 守护进程。

**Ubuntu作为服务器，需要安装ssh服务端软件。**

```bash
sudo apt-get install openssh-server
```

**sshd 服务提供两种安全验证的方法：**

1. 基于口令的安全验证：经过验证账号与密码即可登录到远程主机。
2. 基于密钥的安全验证：在本地生成密钥后将公匙传入服务器，进行公共密钥的比较。

## 基于口令的安全验证

默认为端口号是22，-p22可省略

```bash
ssh -p22 yinanli@192.168.22.128
```

## 基于密钥的安全验证

**生成密钥**

```bash
ssh-keygen
```

**将公钥传送到远程主机**

```bash
ssh-copy-id 172.66.99.154
```

**在远程服务器中修改sshd服务的配置文件**

/etc/ssh/sshd_config

```bash
# 允许密钥验证 不变yes
PubkeyAuthentication yes

# 允许密码验证 改为no
PasswordAuthentication yes

# 保存退出，重启ssh服务
systemctl restart sshd
```

**无密码远程登录**

```
ssh yinanli@172.66.99.154
```

## 修改sshd配置文件

### 修改sshd服务器端口号

- 端口范围0-65535

- 不能使用别的服务已经占用的端口，例如20 ,21, 23, 25, 80, 443, 3389, 3306, 11211等。

修改/etc/ssh/sshd_config默认的端口号。

```bash
Port 2222

# 保存退出，重启ssh服务
systemctl restart sshd
```

之后尝试连接服务，会报错。

```bash
ssh: connect to host 172.66.99.154 port 2222: No route to host
```

因为修改了默认端口号，服务器防火墙和selinux 阻止端口号链接。

```
setenforce 0
systemctl stop firwalld
```

---



# SCP

scp是基于ssh进行远程文件拷贝。

## **本地复制粘贴到远程**

````bash
cd 到文件所在位置

# 拷贝文件
scp 1.txt yinanli@172.66.99.154:/home/yinanli/Desktop

# 拷贝文件夹
scp -r test yinanli@172.66.99.154:/home/yinanli/Desktop
````

## **远程复制粘贴到本地**

```bash
# 拷贝文件
scp yinanli@172.66.99.154:/home/yinanli/Desktop/2.txt .
# . 粘贴到本地当前桌面

# 拷贝文件夹
scp -r yinanli@172.66.99.154:/home/yinanli/Desktop/test .
```














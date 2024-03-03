---
layout: wiki
title: SSH客户端和服务器端的配置
categories: Tools
description: SSH client and server config
keywords: SSH client, SSH server, OpenSSH
---

# 客户端

Windows, Linux, MacOS 的设置是差不多的，都是这几个步骤：

1. 确认ssh客户端安装好了，MacOS和Linux基本上自带了的，Windows可以在“可选功能”里面安装。
2. 运行 ssh 客户端，ssh-agent 命令，在 Linux 上可能需要设置启动时自动运行或者每次开机后手动运行。
3. 使用 ssh-add -L 检查已经添加到ssh客户端的key，然后用 ssh-add $keyfile 添加 private key
4. 在不同系统中有不同的快速配置位置
    1. MacOS 是在 .ssh/config 文件里添加一组内容如下
    
    ```
    Host ginkgo
      HostName $ip
      User $username
      IdentityFile ~/.ssh/$private_key_file
    ```
    
5. 使用 ssh 连接

# Windows 服务器

先在可选功能（在设置里搜索“可选”就能找到）里选择“添加可选功能”，然后添加 OpenSSH 服务器。

生成一对 private key 和 public key ，然后 private key 放在本地客户端，public key 放在远程服务器端。本地添加方法参照上面文字。

1. public key 的内容放在服务器端的 C:/Users/$用户名/.ssh 这个文件夹下。在这个文件夹下新建一个authorized_keys 文件，然后用记事本或者什么文本编辑器打开，把public_key里面的内容复制进去，不需要在 .ssh 文件夹里放 public key 那个 .pub 文件。
2. 【非常重要！】调整 authorized_keys 的权限。右键菜单→属性→安全→高级→点击“禁用继承”，然后选择“将已继承的权限转换为此对象的显式权限”。然后应用，点确定关闭窗口。
3. 【非常重要！】C:/ProgramData/ssh/sshd_config 文件的内容。

    ```
    确保以下 2 项设置内容，并且没有被注释掉
    PubkeyAuthentication yes
    AuthorizedKeysFile	.ssh/authorized_keys

    为了增加安全系数，可以考虑关闭明文传输密码的登录方式
    PasswordAuthentication no 

    非常重要的是，确保以下两行被注释掉！
    # Match Group administrators
    #        AuthorizedKeysFile __PROGRAMDATA__/ssh/administrators_authorized_keys
    ```

- Get-Service sshd 查看状态，Restart-Service sshd 重启服务，也可以 Win键+R 运行 services.msc 在服务里面找 OpenSSH 服务端
- authorized_keys 的访问权限并不需要改成只读。
- 先创建了本地用户，后在这个用户里登陆了微软账户，似乎本地用户名（例如vysitu）和微软邮箱两个登录名都可以用，登录进去之后shell显示的是vysitu这个本地用户名。
- 默认登陆 shell 是 CMD，需要运行 powershell.exe 才能打开 PowerShell
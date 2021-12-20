---
title: "OpenVPN 同时连接多个 VPN 服务器"
excerpt_separator: "<!--more-->"
categories:
  - Software
tags:
  - Windows
toc: true
toc_label: "目录"
toc_icon: "cog"
---

OpenVPN 同时连接多个 VPN 服务器。不想来回的切换连接点，同时连接两个服务器，并且都能够正常访问

<!--more-->

## OpenVPN 下载

官方社区下载: [https://openvpn.net/community-downloads/](https://openvpn.net/community-downloads/)

## 添加虚拟网卡

在该目录下执行命令：`C:\Program Files\OpenVPN\bin`

安装完成后默认会有一个`TUN adapter`和一个`TAP adapter`，查看当前节点

```bash
openvpn --show-adapters

# => Available TAP-WIN32 / Wintun adapters [name, GUID, driver]:
# => 'OpenVPN Wintun' {1D1B0C94-0381-46BD-949A-D15173178BFC} wintun
# => 'OpenVPN TAP-Windows6' {13336BD7-E675-49E2-819D-11255F0EC9DA} tap-windows6
```

新增一个`TAP adapter`

```bash
# 展示
tapctl list

# 新增(name不能是中文)
tapctl.exe create --name "OpenVPN TAP-Windows6 2" --hwid root\tap0901

#
# {5736A029-00DB-4ECC-B918-C07D6BA1838B}

# 删除
tapctl delete {5736A029-00DB-4ECC-B918-C07D6BA1838B}
```

只需要保证有两个`TAP adapter`, OpenVPN 连接时会自动去选择并使用

## 参考
> [使用Openvpn同时连接多个vpn服务器](https://www.swack.cn/wiki/001557409799713ca16fa7271334e4cadbf9cc76fd0d933000/0016044578258439cfe7182030c4992b67cd15ff7e07c59000)

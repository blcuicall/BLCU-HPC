# BLCU-HPC

北京语言大学高性能 GPU 计算集群。

## 🧭 使用手册

点击链接访问：[BLCU-HPC使用指南](https://blcuicall.org/hpc/#/)

## ❓️ 常见问题

1. **密码更新后无法连接到计算节点？**

该问题是由于 Linux 密码更新机制和 NIS 系统未协作导致的。在密码过期后，系统设置的密码和更新机制并不会将密码信息同步至每一个节点。

用户在个人 `旧密码` 过期后，系统会强制让用户设置 `新密码`。在此之后，可以登录主节点，使用 `yppasswd` 指令再次更新密码。首先输入系统强制你更新的 `新密码`，然后再设置 `新新密码` 以在集群节点上同步。

```shell
$ yppasswd
NIS account information for user on admin.cmd.blculease 
enter old password: [输入当前密码（新密码）]
changing NIs password for szxw68136 on admin.cmd.blcuPlease 
enter new password: [更新密码（新新密码）]
Please retype new password: [再次输入，确认]
```

## 📨 问题反馈

请将您遇到的问题提交至 Issue 区，我们会尽快查看并回复。
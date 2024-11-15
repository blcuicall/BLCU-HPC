# BLCU-HPC

北京语言大学高性能 GPU 计算集群。

## 🧭 使用手册

点击链接访问：[BLCU-HPC使用指南](https://blcuicall.org/hpc/#/)

## ⚠️ 注意

最近很多同学反映遇到一些问题，包括：

- VSCODE 链接时一直卡在 `正在使用scp...` 但无法成功连接，但是使用 `ssh` 指令可以正常连接。
- conda/pip install 报错 `OS error: No space left on device`。
- 无法修改自己 `/home/uername` 目录下的文件

以上错误均是由于 `/home` 目录下的空间不足导致的。集群服务器为每位同学分配了 50G 的空间，但是由于一些同学在 `/home` 目录下存储了大量的数据，或拥有太多 conda 环境导致包缓存占满空间。我们建议将一些大文件夹移动到 `/data` 目录下。

为验证是否是空间不足导致的问题，可以用quota指令查看自己的磁盘配额。

```shell
quota -s
# 以下是输出
Disk quotas for user xxxx6606 (uid 1218):
     Filesystem   space   quota   limit   grace   files   quota   limit   grace
storage2.data.blcu:/data
                 76026M    500G    500G            191k       0       0
storage1.data.blcu:/home
                  51200M  51200M  51200M           92192       0       0
```

可以观察到 `/home` 目录下的配额为 51200M，即 50G，且已经使用的空间（space）为 51200M，即已经使用完了。

通常情况下，conda 环境和包缓存位于 `~/.conda` 和 `~/.cache` 目录下，且会占用大量空间。如果属于这个情况，可以将这两个文件夹移动到 `/data` 目录下，并使用软链接的方式链接到 `/home/username`。

1.查看文件大小

```shell
du -sh ~/* # 查看非隐藏文件
```

由于上述命令不显示隐藏文件，可以使用以下命令查看隐藏文件的大小

```shell
du -sh ~/.conda
du -sh ~/.cache
```

备注：如果正在使用 zsh，可以用 `du -sh ~/.*` 查看所有隐藏文件的大小，而 bash 这条指令会试图访问 `..` 导致报错。不知道什么是 bash 和 zsh 的同学就是在使用 bash，不用管这条备注。 

如果发现确实是 `~/.conda` 和 `~/.cache` 文件夹占用了大量空间，可以将其移动到 `/data` 目录下并建立软连接。这是为了保证移动后系统仍然能够正常地按原来的路径访问 conda 环境和包缓存。

如果是自己的数据文件占用了空间，请直接移动到 `/data` 目录下并修改代码调用关系，不必建立软链接。

2.移动大文件到data盘

以移动conda环境为例
```shell
mv .conda ~/workspace/
mv .cache ~/workspace/
```

3.软链接到home目录下 

```shell
ln -s ~/workspace/.conda ~/.conda
ln -s ~/workspace/.cache ~/.cache
```



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
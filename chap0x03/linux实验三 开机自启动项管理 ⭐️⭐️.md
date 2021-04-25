# linux实验三 开机自启动项管理 ⭐️⭐️

## 一、实验环境

* Virtualbox 
* Ubuntu 20.04 Server 64bit
* Windows 10
* asciinema

## 二、实验目的

1. 动手实战systemd

2. 参照第2章作业的要求，完整实验操作过程通过[asciinema](https://asciinema.org/)进行录像并上传，文档通过github上传

3. 完成自查清单

   

## 三、实验过程

### 一、安装Apache2

```shell
$ sudo apt-get update
$ sudo apt-get install apache2
```

### 二、学习视频

#### 入门篇

* 1-3.3 [![asciicast](https://asciinema.org/a/409776.svg)](https://asciinema.org/a/409776)

* 3.4-3.5 [![asciicast](https://asciinema.org/a/409778.svg)](https://asciinema.org/a/409778)

* 3.6-4.3 [![asciicast](https://asciinema.org/a/409782.svg)](https://asciinema.org/a/409782)

* 4.4-5.2 [![asciicast](https://asciinema.org/a/409794.svg)](https://asciinema.org/a/409794)

* 5.3-6 [![asciicast](https://asciinema.org/a/409798.svg)](https://asciinema.org/a/409798)

* 7 [![asciicast](https://asciinema.org/a/409835.svg)](https://asciinema.org/a/409835)

#### 实战篇



* [![asciicast](https://asciinema.org/a/409841.svg)](https://asciinema.org/a/409841)

## 四、自查清单

1. 如何添加一个用户并使其具备`sudo`执行程序的权限？

   * 添加用户:`useradd username`
   * 提升权限:`usermod -a -G adm username`

2. 如何将一个用户添加到一个用户组？

   * 将一个用户添加到用户组中，不能直接用： `usermod -G groupA` 
     * 这样做会使你离开其他用户组，仅仅做为 这个用户组 groupA 的成员。 
   * 应该用 加上 -a 选项： `usermod -a -G groupA user`
     * -a 代表 append， 也就是 将自己添加到 用户组groupA 中，而不必离开 其他用户组。 

3. 如何查看当前系统的分区表和文件系统详细信息？

   - 查看分区表:`sudo fdisk -l`
   - 查看文件系统详细信息:`df -h`

4. 如何实现开机自动挂载`Virtualbox`的共享目录分区？

   - 创建共享文件的挂载目录:`mkdir /mnt/share`
   - 实现挂载:`mount-t vboxsf share_file_name /mnt/share`
   - 进入/etc/fstab进行编辑:`share_file_name /mnt/share vboxsf default 0 0`

5. 基于`LVM`（逻辑分卷管理）的分区如何实现动态扩容和缩减容量？

   - 创建分区并修改为LVM模式:` fdisk 分区名`
   - 动态扩容: `lvexd -L {{size}}`
   - 缩减容量: `lvreduce -L {{size}}`

6. 如何通过`systemd`设置实现在网络连通时运行一个指定脚本，在网络断开时运行另一个脚本？

   - 修改systemd-networkd中的Service
   - ExecStartPost=网络联通时运行的指定脚本
   - ExecStopPost=网络断开时运行的另一个脚本

7. 如何通过`systemd`设置实现一个脚本在任何情况下被杀死之后会立即重新启动？实现杀不死

   ```shell
   $ sudo systemctl vi scriptname
   
   $ Restart = alway
   
   $ sudo systemctl daemon-reload
   ```

   

## 五、参考资料

1. 虚拟机报错Failed to open/create the internal network 'HostInterfaceNetworking-VirtualBox Host](https://blog.csdn.net/u012269267/article/details/103976666?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-3.control&dist_request_id=1332041.11799.16192543515191859&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-3.control)
   * 重启虚拟网卡
2. [Systemd 入门教程：命令篇](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html)
3. [Systemd 入门教程：实战篇](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-part-two.html)



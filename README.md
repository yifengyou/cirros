# cirros用例

![20220320_175532_52](image/20220320_175532_52.png)

## 相关站点

* <http://download.cirros-cloud.net/>
* <https://launchpad.net/cirros>
* <https://github.com/cirros-dev/cirros>

## 镜像获取

* <https://download.cirros-cloud.net/0.5.2/cirros-0.5.2-x86_64-disk.img>


## 启动脚本

```
#!/bin/bash

qemu-system-x86_64 \
	-m 128 \
	-smp 2 \
	--enable-kvm \
	-boot d \
	-hda cirros-0.5.2-x86_64-disk.img \
	-nographic

```


## 快照/还原

```
# 创建还原点/快照
qemu-img snapshot -c 'original' cirros-0.5.2-x86_64-disk.img

# 罗列还原点/快照
qemu-img snapshot -l cirros-0.5.2-x86_64-disk.img

# 还原
qemu-img snapshot -a 1 cirros-0.5.2-x86_64-disk.img
```


## 优化脚本

```
sudo su
cat >> /etc/profile << EOF
PS1='\[\e[32;1m\][\[\e[31;1m\]\u\[\e[33;1m\]@\[\e[35;1m\]\h\[\e[36;1m\] \w\[\e[32;1m\]]\[\e[37;1m\]\$\[\e[0m\] '
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* -a --color=auto'
alias ll='ls -l -a --color=auto'
alias ls='ls -a --color=auto'
alias cp='cp -i'
alias mv='mv -i'
alias rm='rm -i'
alias xzegrep='xzegrep --color=auto'
alias xzfgrep='xzfgrep --color=auto'
alias xzgrep='xzgrep --color=auto'
alias zegrep='zegrep --color=auto'
alias zfgrep='zfgrep --color=auto'
alias zgrep='zgrep --color=auto'
alias push='git push'
EOF
uuid-gen > /etc/machine-id
mkdir /etc/rc3.d/.back
mv /etc/rc3.d/*cirros* /etc/rc3.d/.back
mv /etc/rc3.d/S50-dropbear /etc/rc3.d/.back
mv /etc/rc3.d/S40-network /etc/rc3.d/.back
```

其他命令

```
# 启用网络
ifup eth0

# 启用SSHD
dropbear -R

# 关机且断电
poweroff -f

# 重启
reboot -f
```


















---

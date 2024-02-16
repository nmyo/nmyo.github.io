**启动SSH服务时，提示如下错误。**

```shell
could not load host key：/etc/ssh/ssh_host_rsa_key
```

**登录问题服务器，执行如下命令，重启服务，没有信息显示，也有可能提示启动失败**

```SHELL
systemctl restart sshd.service
```

**执行如下命令，发现ssh启动失败。**

```shell
systemctl status sshd.service 
```

**系统显示类似如下，确认`/etc/ssh/ssh_host_rsa_key`文件存在问题。**

```shell
could not load host key：/etc/ssh/ssh_host_rsa_key
```

**执行如下命令，查看文件权限，正常情况下该文件权限为640，如果提示没有文件，直接执行重新生成，如果无报错**

```shell
sshd -t
ls -al /etc/ssh/ssh_host_rsa_key
```

**执行如下命令，系统显示存在乱码，说明文件内容存在问题。**

```shell
cat /etc/ssh_host_rsa_key
```

**执行如下命令，重新生成ssh_host_rsa_key文件，这里可能会提示缺失好几个文件，分别重新创建这几个文件，实际文件名看你报错**

```shell
ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
ssh-keygen -t rsa -f /etc/ssh/ssh_host_ed25519_key
ssh-keygen -t rsa -f /etc/ssh/ssh_host_ecdsa_key
```
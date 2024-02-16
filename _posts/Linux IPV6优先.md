**取消掉这个注释**

```bash
> sed -i "s/#\?precedence ::ffff:0:0\/96 100/precedence ::ffff:0:0\/96 100/" /etc/gai.conf
```

**改回去（恢复系统默认）**

```bash
sed -i "s/^precedence ::ffff:0:0\/96 100/\#precedence ::ffff:0:0\/96 100/" /etc/gai.conf
```
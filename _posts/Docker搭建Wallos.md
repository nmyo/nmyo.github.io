更新系统

```bash
apt update -y && apt upgrade -y
```

安装必要工具

```bash
apt install wget curl sudo nano git  -y
```

### 🐋安装docker，docker-compose并配置

1. 下载docker

```bash
wget -qO- get.docker.com | bash
```

1. 设置docker开机自启

```bash
systemctl enable docker
```

1. 重启docker

```bash
systemctl restart docker
```

1. 安装docker-compose

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

1. docker-compose赋权

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

1. 修改时区为上海

```bash
sudo timedatectl set-timezone Asia/Shanghai
```

### 🫤项目部署

1. 创建存配置文件的目录,然后进入目录下。

```bash
mkdir /opt/wallos
cd /opt/wallos
```

创建docker-compose.yaml

写入以下内容：

```yaml
cat > docker-compose.yaml <<EOF
version: '3.0'

services:
  wallos:
    container_name: wallos
    image: bellamy/wallos:latest
    ports:
      - "8282:80/tcp"        #冒号左边为打开网页时输入的端口
    environment:
      TZ: 'Asia/Shanghai'
    # Volumes store your data between container upgrades
    volumes:
      - './db:/var/www/html/db'
      - './logos:/var/www/html/images/uploads/logos'
    restart: unless-stopped
EOF
```

拉取镜像部署

```bash
docker-compose up -d
```
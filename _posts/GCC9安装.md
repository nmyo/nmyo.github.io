## CentOS

```bash
sudo yum install -y centos-release-scl
sudo yum install -y devtoolset-9-gcc devtoolset-9-gcc-c++
scl enable devtoolset-9 bash
gcc -v
```

## Ubuntu

```bash
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo update-alternatives --remove-all gcc
sudo update-alternatives --remove-all g++
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 900
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-9 900
```

### 检查gcc版本

```bash
gcc -v
```

## Debian11

```shell
sudo apt update
sudo apt upgrade
sudo apt install build-essenti
```

**安装apt管理包**

```shell
sudo apt install manpages-dev
```

**检查版本**

```shell
gcc --version
```


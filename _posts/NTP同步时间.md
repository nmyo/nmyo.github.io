### 基于 DEB 的系统

1. 要在 Ubuntu、Debian 和 Linux Mint 上安装 NTP 客户端包：

   ```bash
   sudo apt install ntpdate
   ```

2. 安装 ntpdate 包后我们需要做的第一件事是打开命令行终端，并确保禁用 Ubuntu 默认的 timesyncd 服务，因为这会与我们的尝试冲突与 NTP 服务器同步。

   ```bash
   sudo timedatectl set-ntp off
   ```

3. 如果您希望将系统时间与世界时间服务器同步，您只需使用 root 权限发出以下 Linux 命令即可：

   ```bash
   ntpdate pool.ntp.org
   ```

4. 上述命令将同步您的系统时间/时钟。然而，如果你想保持同步，你需要做更多的工作。该工作涉及NTPD守护进程的安装和配置。 NTPD 使用 NTP（网络时间协议）通过 Internet 访问指定的时间服务器。稍后，它将使您的系统时间保持同步，无需您进一步干预。

   ```bash
   sudo apt install ntp
   ```

5. 在大多数情况下，您的 NTP 守护进程将被配置为开箱即用，您只需安装 ntp 软件包即可使您的系统时间与互联网标准时间服务器保持同步。然而，在我们将系统时间交给 NTP 守护进程之前，最好检查一下某些设置是否已到位。这些设置在 `ntp.conf` 文件内配置。

   ```bash
   sudo nano /etc/ntp.conf
   ```

6. 在此文件中，放置我们打算用于时间同步的时间服务器的完全限定域名。例如，默认设置如下所示：

   ```bash
   server 0.debian.pool.ntp.org iburst
   server 1.debian.pool.ntp.org iburst
   server 2.debian.pool.ntp.org iburst
   server 3.debian.pool.ntp.org iburst
   ```

   您可以使用 NTP 池项目网站查找距离您所在位置最近的 NTP 服务器池。

7. 对 `ntp.conf` 文件进行任何必要的更改后，请务必保存并退出该文件。然后，通过重新启动 NTP 守护程序使更改生效。

   ```bash
   sudo systemctl restart ntp
   ```

8. 最后，使用 ntpq 命令列出 NTP 时间同步队列：

   ```bash
   ntpq -p
   ```

### 基于 RPM 的系统

1. 要在 Fedora、CentOS、AlmaLinux 和 Red Hat 上安装 NTP 客户端包：

   ```bash
   sudo dnf install chrony
   ```

2. 确保将 `chrony` 设置为在启动时自动启动：

   ```bash
   systemctl enable --now chrony
   ```

3. 在大多数情况下，chrony 将进行开箱即用的配置和安装，您无需执行任何其他操作即可使系统时间与互联网标准时间服务器保持同步。但是，在我们将系统时间交给 chrony 之前，最好检查某些设置是否已到位。这些设置在 chrony.conf 文件内配置。

   ```bash
   sudo nano /etc/chrony.conf
   ```

4. 在此文件中，放置我们打算用于时间同步的时间服务器的完全限定域名。例如，默认设置如下所示：

   ```bash
   pool 2.fedora.pool.ntp.org iburst
   ```

   您可以使用 NTP 池项目网站查找距离您所在位置最近的 NTP 服务器池。

5. 对 `chrony.conf` 文件进行任何必要的更改后，请务必保存并退出该文件。然后，通过重新启动 chrony 使更改生效。

   ```bash
   sudo systemctl restart chronyd
   ```

6. 最后，使用 chronycsources 命令列出 NTP 时间同步队列：

   ```bash
   chronyc sources
   ```

   默认情况下，慢性 NTP 客户端将每 64 秒执行一次时间同步。
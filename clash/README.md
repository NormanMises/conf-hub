# Linux 无 GUI 配置 mihomo 教程
1. 下载 [config.yaml](config.yaml) 到 Linux 中
2. 修改 config.yaml 中的 `proxy-providers` 里的 `url` 为自己的订阅地址

    [可选] 可以添加多个订阅，即添加多个
    ```yaml
    provider1: # 订阅名称，可以自己修改，但不能重复
    type: http
    url: "" # 订阅地址
    interval: 7200
    proxy: 直连
    header:
      User-Agent:
      - "Clash/v1.18.0"
      - "mihomo/1.18.3"
    health-check:
      enable: true
      url: https://www.gstatic.com/generate_204
      interval: 300
    overwrite:
      udp: true
      skip-cert-verify: true
    ```
3. 下载 [mihomo](https://wiki.metacubex.one/startup/) 最新版如 `mihomo-linux-amd64-v1.19.15.gz`，并解压，将解压后的文件名修改为 `mihomo`
4. 将 `mihomo` 和 `config.yaml` 放到同一个目录下
5. 给 `mihomo` 添加执行权限
    ```bash
    chmod +x mihomo
    ```
6. 运行 mihomo
    ```bash
    ./mihomo -d .
    ```
    可以把此命令写成一个脚本，如 `start.sh`，然后给 `start.sh` 添加执行权限，然后就可以用 `./start.sh` 来启动了
7. 要保证 mihomo 在后台运行，可以使用 `tmux` 等工具挂在后台
8. http 代理默认为 `http://127.0.0.1:7890`，socks5 代理为 `socks5://127.0.0.1:7890`。端口为`config.yaml`中`mixed-port`字段
9. 在`.bashrc`中写入
    ```bash
    # Open proxy
    on() {
        export https_proxy="http://127.0.0.1:7890"
        export http_proxy="http://127.0.0.1:7890"
        echo "HTTP/HTTPS Proxy on"
    }

    # Close proxy
    off() {
        unset http_proxy
        unset https_proxy
        echo "HTTP/HTTPS Proxy off"
    }
    ```
    然后就可以用 `on` 和 `off` 在命令行中来选择是否使用代理了
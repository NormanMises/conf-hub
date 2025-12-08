# Linux 无 GUI 配置 mihomo 教程
1. 下载 [config.yaml](https://gist.github.com/NormanMises/50e94c571b1ef8debf829974d945e82b) 到 Linux 中
2. 在 `config.yaml` 中 `proxy-providers` 添加自己的订阅

    [可选] 可以添加多个订阅，把多个订阅里的节点合并成一个订阅使用
    ```yaml
    provider1: # 订阅名称，自己修改，但不能重复
      <<: *Providers
      url: "" # 订阅地址
      override: # 给这个订阅里的所有节点前面加上这个前缀，不用可以删掉这两行
        additional-prefix: 'provider1 | ' # 给这个订阅里的所有节点前面加上这个前缀，不用可以删掉这两行
    ```
3. 下载 [mihomo 内核](https://github.com/MetaCubeX/mihomo/releases/latest) 最新版如 `mihomo-linux-amd64-v1.19.17.gz`，并解压，将解压后的文件名修改为 `mihomo`
4. 将 `mihomo` 和 `config.yaml` 放到同一个目录下
5. 给 `mihomo` 添加执行权限
    ```bash
    chmod +x mihomo
    ```
6. 运行 `mihomo`
    ```bash
    ./mihomo -d .
    ```
7. 要保证 `mihomo` 在后台运行，可以使用 `tmux` [参考](../config/README.md) 等工具挂在后台
8. http 代理默认为 `http://127.0.0.1:7890`，socks5 代理为 `socks5://127.0.0.1:7890`。端口为 `config.yaml` 中 `mixed-port` 字段
9. 在 `.bashrc` 中写入
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
    然后就可以用 `on` 和 `off` 在命令行中来使用代理了
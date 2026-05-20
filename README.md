# SOCKS5 一键部署 (singbox-socks5-installer)

`fq.sh` —— 基于 [sing-box](https://github.com/SagerNet/sing-box) 的 SOCKS5 代理一键部署脚本（Ubuntu / Debian）。

## 用途

在 VPS 上快速搭建一个带账号密码的 SOCKS5 代理服务，使用 sing-box 官方最新版二进制，通过 systemd 托管，日志走 journald。

## 默认配置

| 项 | 默认值 |
|---|---|
| 用户名 | `unlil` |
| 密码 | `unlil` |
| 端口 | `61080` |

部署时脚本会：
- 关闭系统防火墙（ufw / firewalld / iptables）放行所有端口
- 清理端口占用（含旧的 danted 残留）
- 从 GitHub Release 下载 sing-box 最新版
- 写入 systemd 服务，日志走 journald

## 使用方法

```bash
# 默认 unlil / unlil / 61080
bash fq.sh

# 自定义账号密码端口
bash fq.sh -u admin -p MyPass --port 1080

# 管道方式
curl -fsSL https://你的地址/sk5.sh | bash -s -- -u admin -p MyPass --port 1080

# 卸载
bash fq.sh --uninstall
```

部署后管理：

```bash
systemctl status sing-box        # 状态
journalctl -u sing-box -f        # 日志
systemctl restart sing-box       # 重启
# 配置文件 /etc/sing-box/config.json
```

## 依赖与环境

- 系统：Ubuntu / Debian
- 权限：**必须 root**

## ⚠️ 安全注意

- **强烈建议修改默认账号密码** `unlil/unlil`，默认凭据极易被扫描爆破
- 脚本会**关闭系统防火墙并放行所有端口**，请自行评估风险，必要时改为只放行所需端口

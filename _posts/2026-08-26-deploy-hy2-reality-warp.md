---
layout: post
title: "本机 SSH 从零部署：HY2 + REALITY + WARP（ChatGPT / Grok）"
date: 2026-08-26 10:35:00 +0800
---

这份文档把对话中确定的推荐形态，整理成本机通过 SSH 施工的步骤和命令参数。

本文整理自公开仓库 [yding-git/personal-edge-proxy](https://github.com/yding-git/personal-edge-proxy) 中的 [`docs/deploy-hy2-reality-warp.md`](https://github.com/yding-git/personal-edge-proxy/blob/main/docs/deploy-hy2-reality-warp.md)。命令、占位符和配置结构保持原文。

**不是图形界面部署。** Vultr 网页只负责买机器、看 IP、救急 VNC。日常施工全部是本机终端 + `ssh`。Agent（Codex / Grok / Claude Code 等）应装在**你的电脑**上，用已经验证过的 SSH 登录 VPS；不要在 1G 云主机上再装桌面或编码 Agent。

对照仓库：

- 架构合同：[`AGENTS.md`](https://github.com/yding-git/personal-edge-proxy/blob/main/AGENTS.md)
- 服务端 schema：[`examples/xray-server.example.jsonc`](https://github.com/yding-git/personal-edge-proxy/blob/main/examples/xray-server.example.jsonc)
- HY2 客户端：[`examples/v2rayn-hysteria2.example.md`](https://github.com/yding-git/personal-edge-proxy/blob/main/examples/v2rayn-hysteria2.example.md)
- REALITY 客户端：[`examples/v2rayn-reality-vision.example.md`](https://github.com/yding-git/personal-edge-proxy/blob/main/examples/v2rayn-reality-vision.example.md)
- WARP：[`docs/warp-outbound.md`](https://github.com/yding-git/personal-edge-proxy/blob/main/docs/warp-outbound.md)

---

## 1. 目标形态

```text
手机 Shadowrocket / 电脑客户端
  ├─ HY2（UDP 好时）
  └─ VLESS + REALITY（稳妥底盘）
        ↓
Vultr 1核1G，亚太机房，约 $5
  Xray
  ├─ 普通流量          → Direct（优先 IPv4）
  ├─ ChatGPT / OpenAI  → WARP Local Proxy
  └─ Grok / x.ai       → WARP Local Proxy
```

这是仓库档位 C 的出口，加上 REALITY 备用入口。

明确**不做**：

- Claude / Anthropic 固定 SOCKS5
- Cloudflare Tunnel
- 把整台 VPS 默认路由切进 WARP
- 在 VPS 上装 Ubuntu Desktop / 面板 / 本机编码 Agent

一次只改一个变量：SSH → 系统底座 → HY2 → REALITY → WARP 本地代理 → 才把 AI 域名接到 WARP。

---

## 2. 符号约定

| 标记 | 含义 |
|---|---|
| `[本机]` | 在你的电脑执行 |
| `[VPS]` | 已 SSH 登录到云主机后执行 |
| `YOUR_*` | 占位符，换成你的值，不要提交到 git / 聊天 |

占位符清单：

```text
YOUR_SERVER_IP
YOUR_DOMAIN
YOUR_SERVER_NAME          # HY2 证书的 SNI，通常等于 YOUR_DOMAIN
YOUR_HY2_PASSWORD
YOUR_UUID
YOUR_REALITY_PRIVATE_KEY  # 只留服务器
YOUR_REALITY_PUBLIC_KEY   # 只给客户端
YOUR_SHORT_ID
YOUR_REALITY_TARGET       # 示例：www.microsoft.com:443
```

端口（与仓库完整示例一致；改端口则客户端必须一起改）：

```text
TCP 22      SSH
TCP 80      仅申请 Let's Encrypt HTTP-01 时临时开放
TCP 443     REALITY
UDP 24443   HY2
```

WARP 本地代理：

```text
127.0.0.1:40000    SOCKS5    仅本机回环，不要对公网开放
```

---

## 3. 阶段 0：买机器（网页，只需这一次）

Vultr 控制台：

| 项 | 选择 |
|---|---|
| 产品 | Cloud Compute Regular，约 $5/月 |
| 规格 | 1 vCPU / 1 GB RAM / 约 25 GB 盘 |
| 机房 | 东京优先；不行再试大阪 / 首尔 / 新加坡 |
| 系统 | Ubuntu 24.04 LTS，**不要**带桌面的镜像 |
| 网络 | 必须有公网 IPv4；不要买 IPv6-only |
| 附加 | 先不要自动备份、额外 IP、GPU |

创建后记下 IPv4。首次 root 密码只在 Vultr 邮件/面板查看，**不要贴进聊天、Issue 或仓库**。

购买前可用该机房测试 IP，从你的网络看延迟和丢包。稳定的中低丢包比晚高峰严重丢包的“标称 1 Gbps”更有用。

部署前自行核对 Vultr ToS / AUP，只做个人自用、有认证的节点。

---

## 4. 阶段 1：人类 SSH 引导（必须人做）

在密钥登录成功之前，不要让 Agent 改防火墙、不要关密码登录。私钥只留本机。

### 4.1 `[本机]` 生成密钥（已有 ed25519 可跳过）

```bash
ssh-keygen -t ed25519
```

默认：

```text
~/.ssh/id_ed25519        私钥，只留本机
~/.ssh/id_ed25519.pub    公钥，可以上传服务器
```

### 4.2 `[本机]` 上传公钥

macOS / Linux：

```bash
ssh-copy-id root@YOUR_SERVER_IP
```

或：

```bash
ssh root@YOUR_SERVER_IP "umask 077; mkdir -p ~/.ssh; cat >> ~/.ssh/authorized_keys"
# 本地再执行：把公钥内容贴进上面这条远程会话，或：
cat ~/.ssh/id_ed25519.pub | ssh root@YOUR_SERVER_IP "umask 077; mkdir -p ~/.ssh; cat >> ~/.ssh/authorized_keys"
```

首次会提示接受 host key，并用 Vultr 初始密码。

### 4.3 `[本机]` 验证密钥登录

**新开一个终端**（不要复用还在用密码的会话）：

```bash
ssh root@YOUR_SERVER_IP
```

必须能直接进去。成功后可写 SSH config，之后 Agent 只调用这个别名：

```text
# ~/.ssh/config
Host vultr-edge
  HostName YOUR_SERVER_IP
  User root
  IdentityFile ~/.ssh/id_ed25519
  IdentitiesOnly yes
```

之后：

```bash
ssh vultr-edge
```

**这一步成功之前，不要关闭 SSH 密码登录。**

本机 Agent 从这里开始可以接手，调用现成 `ssh` / SSH Agent 即可，不需要知道私钥内容。

---

## 5. 阶段 2：系统底座

全部 `[VPS]`。不要装 Docker、宝塔、3x-ui、桌面、Node 大套件。

### 5.1 更新与基础包

```bash
apt update && apt -y upgrade
apt -y install curl ufw jq ca-certificates openssl
```

### 5.2 可选：1G swap（1G 内存上 WARP 的实用措施）

```bash
fallocate -l 1G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
echo '/swapfile none swap sw 0 0' >> /etc/fstab
```

### 5.3 防火墙：先放行再启用

```bash
ufw default deny incoming
ufw default allow outgoing
ufw allow 22/tcp comment 'SSH'
ufw allow 24443/udp comment 'HY2'
ufw allow 443/tcp comment 'REALITY'
ufw enable
ufw status verbose
```

先不要关 22。Vultr 控制台若另有 Firewall Group，规则必须与主机一致：

```text
TCP 22
TCP 443
UDP 24443
```

TCP 80 仅在下面申请证书时临时打开。

### 5.4 确认没有桌面、默认路由仍是 Vultr

```bash
systemctl get-default
ip -4 route
ip -4 addr
```

`systemctl get-default` 应为 `multi-user.target`，不是 `graphical.target`。

---

## 6. 阶段 3：HY2 所需 TLS 证书

仓库 HY2 示例按**正常校验证书**来写（客户端 `insecure=0`）。推荐：一个 A 记录指向 VPS 的域名 + Let's Encrypt。

REALITY **不要求**你拥有域名；HY2 这一步才需要。

### 6.1 DNS

把 `YOUR_DOMAIN` 的 A 记录指到 `YOUR_SERVER_IP`。等解析生效：

```bash
# [本机] 或 [VPS]
dig +short YOUR_DOMAIN A
```

应返回 `YOUR_SERVER_IP`。

### 6.2 `[VPS]` HTTP-01（standalone）

Xray 尚未监听 80。临时开放 80：

```bash
ufw allow 80/tcp comment 'acme-temp'
apt -y install certbot
certbot certonly --standalone --agree-tos -m YOUR_EMAIL -d YOUR_DOMAIN
```

证书通常在：

```text
/etc/letsencrypt/live/YOUR_DOMAIN/fullchain.pem
/etc/letsencrypt/live/YOUR_DOMAIN/privkey.pem
```

复制到 Xray 可读路径（`xray-install` 默认常以 `nobody` 运行，Let's Encrypt 目录它读不到）：

```bash
install -d -m 0755 /usr/local/etc/xray/certs
cp /etc/letsencrypt/live/YOUR_DOMAIN/fullchain.pem /usr/local/etc/xray/certs/fullchain.pem
cp /etc/letsencrypt/live/YOUR_DOMAIN/privkey.pem /usr/local/etc/xray/certs/key.pem
chmod 600 /usr/local/etc/xray/certs/fullchain.pem /usr/local/etc/xray/certs/key.pem
```

装完 Xray 后根据 `systemctl cat xray` 里的 `User=` 再 `chown` 一次。

证书申请成功后可关掉 80：

```bash
ufw delete allow 80/tcp
```

续期后需要再次复制证书并重载 Xray。可在 `/etc/letsencrypt/renewal-hooks/deploy/reload-xray.sh` 放：

```bash
#!/usr/bin/env bash
set -euo pipefail
DOMAIN="YOUR_DOMAIN"
install -d -m 0755 /usr/local/etc/xray/certs
cp "/etc/letsencrypt/live/${DOMAIN}/fullchain.pem" /usr/local/etc/xray/certs/fullchain.pem
cp "/etc/letsencrypt/live/${DOMAIN}/privkey.pem" /usr/local/etc/xray/certs/key.pem
chmod 600 /usr/local/etc/xray/certs/fullchain.pem /usr/local/etc/xray/certs/key.pem
# 若 Xray 以 nobody 运行：
chown nobody:nogroup /usr/local/etc/xray/certs/fullchain.pem /usr/local/etc/xray/certs/key.pem
systemctl reload xray 2>/dev/null || systemctl restart xray
```

```bash
chmod 700 /etc/letsencrypt/renewal-hooks/deploy/reload-xray.sh
```

---

## 7. 阶段 4：安装 Xray 并生成本节点密钥

### 7.1 `[VPS]` 官方安装脚本

来源：[XTLS/Xray-install](https://github.com/XTLS/Xray-install)

```bash
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install
```

装完应有：

```text
/usr/local/bin/xray
/usr/local/etc/xray/config.json
systemd unit: xray.service
```

```bash
/usr/local/bin/xray version
systemctl cat xray | sed -n '1,40p'
```

记下 `User=` / `Group=`，回头给证书 `chown`。常见是 `nobody` / `nogroup`：

```bash
chown nobody:nogroup /usr/local/etc/xray/certs/fullchain.pem /usr/local/etc/xray/certs/key.pem
```

### 7.2 `[VPS]` 生成凭据（只显示在终端，不要写入仓库）

```bash
/usr/local/bin/xray uuid
/usr/local/bin/xray x25519
openssl rand -hex 8
openssl rand -base64 24
```

含义：

| 命令 | 用途 |
|---|---|
| `xray uuid` | `YOUR_UUID`，REALITY 用户 id |
| `xray x25519` | PrivateKey → 服务端；Password/Public → 客户端 `pbk` |
| `openssl rand -hex 8` | `YOUR_SHORT_ID`（8 字节 hex；也可用更短，需客户端一致） |
| `openssl rand -base64 24` | `YOUR_HY2_PASSWORD` |

把这些记在本机密码管理器。**Private Key 绝对不要放进客户端或分享链接。**

REALITY target 先用仓库示例，但要确认 VPS 能访问它的 443：

```bash
YOUR_REALITY_TARGET=www.microsoft.com:443
# 服务端 target 写成 host:443；serverNames 与客户端 SNI 一致，通常是 www.microsoft.com
```

到 target 的 TLS 不正常就换一个你的 VPS 能稳定连上的公共站点。不要机械照抄所有地区都用同一个 target。

---

## 8. 阶段 5：先只开 HY2（最小可用）

先不要写 WARP、不要写 REALITY。配置必须是合法 JSON（仓库 example 是 jsonc 注释版，真正交给 Xray 的文件不要留 `//`，除非你确认当前 Xray 接受 JSONC）。

### 8.1 `[VPS]` 写入最小配置

编辑 `/usr/local/etc/xray/config.json`：

```json
{
  "log": {
    "access": "/var/log/xray/access.log",
    "error": "/var/log/xray/error.log",
    "loglevel": "warning"
  },
  "inbounds": [
    {
      "tag": "hy2-xr",
      "listen": "0.0.0.0",
      "port": 24443,
      "protocol": "hysteria",
      "settings": {
        "version": 2,
        "users": [
          {
            "auth": "YOUR_HY2_PASSWORD",
            "email": "personal"
          }
        ]
      },
      "streamSettings": {
        "method": "hysteria",
        "security": "tls",
        "tlsSettings": {
          "alpn": ["h3"],
          "certificates": [
            {
              "certificateFile": "/usr/local/etc/xray/certs/fullchain.pem",
              "keyFile": "/usr/local/etc/xray/certs/key.pem"
            }
          ]
        },
        "hysteriaSettings": {
          "version": 2,
          "udpIdleTimeout": 60
        }
      },
      "sniffing": {
        "enabled": true,
        "destOverride": ["http", "tls", "quic"],
        "metadataOnly": false
      }
    }
  ],
  "outbounds": [
    {
      "tag": "direct",
      "protocol": "freedom",
      "settings": {
        "domainStrategy": "UseIPv4"
      }
    },
    {
      "tag": "block",
      "protocol": "blackhole",
      "settings": {}
    }
  ],
  "routing": {
    "domainStrategy": "IPOnDemand",
    "rules": [
      {
        "type": "field",
        "inboundTag": ["hy2-xr"],
        "ip": [
          "10.0.0.0/8",
          "172.16.0.0/12",
          "192.168.0.0/16",
          "127.0.0.0/8",
          "169.254.0.0/16",
          "fc00::/7",
          "fe80::/10"
        ],
        "outboundTag": "block"
      }
    ]
  }
}
```

`listen` 写成 `0.0.0.0` 是为了先走 IPv4。若你确认要用双栈再改 `::`。

Direct 的 `domainStrategy: UseIPv4` 对应「普通流量优先 IPv4」，减轻部分双栈 VPS 访问 Grok 时误走 IPv6 的问题。Grok 最终仍建议走 WARP。

日志目录：

```bash
install -d -m 0755 /var/log/xray
# 按 xray.service 的 User 调整：
chown nobody:nogroup /var/log/xray
```

### 8.2 校验并启动

```bash
/usr/local/bin/xray -test -config /usr/local/etc/xray/config.json
systemctl enable --now xray
systemctl status xray --no-pager
ss -ulnp | grep 24443
```

`-test` 失败不要 `restart`。

### 8.3 客户端导入 HY2

分享链接形态（密码若含 URL 特殊字符必须 URL Encode）：

```text
hysteria2://YOUR_HY2_PASSWORD@YOUR_DOMAIN:24443/?sni=YOUR_SERVER_NAME&insecure=0#HY2
```

Shadowrocket：类型 Hysteria2；服务器 `YOUR_DOMAIN`；端口 `24443`；密码；SNI = `YOUR_SERVER_NAME`；不要跳过证书校验。

电脑（v2rayN 审计字段）：

```text
server       YOUR_DOMAIN
server_port  24443
password     YOUR_HY2_PASSWORD
tls.server_name  YOUR_SERVER_NAME
tls.insecure     false
```

`up_mbps` / `down_mbps` 按自己线路填，不必抄 100。

客户端规则：国内 / 局域网 DIRECT，需要代理的走该节点。

### 8.4 验证 HY2

客户端连上后：

- 能打开普通国际网站
- 看当前出口 IP，应等于或接近 `YOUR_SERVER_IP`（Direct）

`[VPS]` 对照：

```bash
curl -4 https://api.ipify.org
```

这一步没过，不要加 REALITY，更不要装 WARP。

---

## 9. 阶段 6：加上 VLESS + REALITY + Vision

### 9.1 在 `inbounds` 数组里增加（与 HY2 并列）

字段以当前 Xray 文档为准：服务端 REALITY 用 `target`（历史配置可能是 `dest`）；传输 `method: raw`（旧 GUI 可能显示 `tcp`）。

```json
{
  "tag": "reality-vision",
  "listen": "0.0.0.0",
  "port": 443,
  "protocol": "vless",
  "settings": {
    "clients": [
      {
        "id": "YOUR_UUID",
        "flow": "xtls-rprx-vision"
      }
    ],
    "decryption": "none"
  },
  "streamSettings": {
    "method": "raw",
    "security": "reality",
    "realitySettings": {
      "show": false,
      "target": "www.microsoft.com:443",
      "serverNames": ["www.microsoft.com"],
      "privateKey": "YOUR_REALITY_PRIVATE_KEY",
      "shortIds": ["YOUR_SHORT_ID"]
    }
  },
  "sniffing": {
    "enabled": true,
    "destOverride": ["http", "tls", "quic"],
    "metadataOnly": false
  }
}
```

把 routing 里私网 block 的 `inboundTag` 改成同时包含：

```json
"inboundTag": ["hy2-xr", "reality-vision"]
```

### 9.2 重载

```bash
/usr/local/bin/xray -test -config /usr/local/etc/xray/config.json
systemctl restart xray
ss -tlnp | grep 443
ss -ulnp | grep 24443
```

443 必须是 Xray，不要和 nginx/caddy 抢端口。

### 9.3 客户端导入 REALITY

分享链接：

```text
vless://YOUR_UUID@YOUR_SERVER_IP:443?type=raw&security=reality&fp=chrome&sni=YOUR_SERVER_NAME&pbk=YOUR_REALITY_PUBLIC_KEY&sid=YOUR_SHORT_ID&flow=xtls-rprx-vision#REALITY
```

这里的 `sni` / `YOUR_SERVER_NAME` 是 REALITY 的 `serverNames`（例如 `www.microsoft.com`），**不是**你的 HY2 域名。

Shadowrocket 手动对应：

| 项 | 值 |
|---|---|
| 类型 | VLESS |
| 地址 | `YOUR_SERVER_IP`（或解析到该 IP 的域名） |
| 端口 | 443 |
| UUID | `YOUR_UUID` |
| 传输 | TCP / RAW |
| TLS | REALITY |
| Public Key | `YOUR_REALITY_PUBLIC_KEY` |
| Short ID | `YOUR_SHORT_ID` |
| SNI / serverName | 与服务端 `serverNames` 相同 |
| flow | `xtls-rprx-vision` |
| 指纹 | chrome |

客户端拿 **Public Key**。服务端只留 Private Key。

手机和电脑都保存两个节点：**HY2 主用，REALITY 备用**。不要合成一条订阅乱切。

验证：切到 REALITY 后仍能上网，出口仍应是 VPS Direct IP。REALITY 不改变最终出口身份。

---

## 10. 阶段 7：WARP Local Proxy（先不接 Xray）

不要让 WARP 接管默认路由。Linux 默认网关必须仍是 Vultr。

### 10.1 安装官方客户端

优先看当前文档：<https://developers.cloudflare.com/warp-client/get-started/linux/>

Ubuntu 典型 apt 源写法（发行版代号、GPG 路径以官网为准）：

```bash
apt -y install gpg lsb-release
curl -fsSL https://pkg.cloudflareclient.com/pubkey.gpg | gpg --yes --dearmor --output /usr/share/keyrings/cloudflare-warp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/cloudflare-warp-archive-keyring.gpg] https://pkg.cloudflareclient.com/ $(lsb_release -cs) main" > /etc/apt/sources.list.d/cloudflare-client.list
apt update
apt -y install cloudflare-warp
```

应出现：

```text
warp-svc
warp-cli
warp-diag
```

### 10.2 注册并切到本地代理

`warp-cli` 子命令会变。先读帮助，再对照 [`docs/warp-outbound.md`](https://github.com/yding-git/personal-edge-proxy/blob/main/docs/warp-outbound.md)：

```bash
warp-cli --help
warp-cli mode --help
warp-cli proxy --help
warp-cli tunnel protocol --help
```

现机对应形态（版本不接受就按 `--help` 改，不要抄旧博客）：

```bash
warp-cli registration new
warp-cli tunnel protocol set MASQUE
warp-cli mode proxy
warp-cli proxy port 40000
warp-cli connect
```

部分版本需要先接受 ToS，例如 `warp-cli --accept-tos registration new`。

确认：

```bash
warp-cli settings
warp-cli status
ss -lntp | grep 40000
ip -4 route
```

期望：

```text
Mode: WarpProxy / Local proxy，端口 40000
127.0.0.1:40000 在听
默认路由仍是 Vultr 网关，不是 WARP 隧道网卡
```

### 10.3 只测 WARP，不经过 Xray

```bash
curl --proxy socks5h://127.0.0.1:40000 https://www.cloudflare.com/cdn-cgi/trace
curl --proxy socks5h://127.0.0.1:40000 https://api.ipify.org
echo '--- direct ---'
curl -4 https://api.ipify.org
```

`cdn-cgi/trace` 里应有 `warp=on`。直连 IP 与 `40000` 出口 IP **必须不同**。

只看 `systemctl is-active warp-svc` 不够。进程在、端口在，出口仍可能是假活。

---

## 11. 阶段 8：把 ChatGPT / Grok 接到 WARP

WARP 本地代理已经真实可用之后，再改 Xray。

### 11.1 outbound 增加

官方 SOCKS outbound 是扁平 `address` / `port`，不是旧的 `servers: []`：

```json
{
  "tag": "warp-official",
  "protocol": "socks",
  "settings": {
    "address": "127.0.0.1",
    "port": 40000
  }
}
```

不要写 `static-socks`，不要写 Claude 规则。

### 11.2 routing 增加（放在私网 block 之后）

```json
{
  "type": "field",
  "inboundTag": ["hy2-xr", "reality-vision"],
  "domain": [
    "domain:openai.com",
    "domain:chatgpt.com",
    "domain:oaistatic.com",
    "domain:oaiusercontent.com",
    "domain:grok.com",
    "domain:x.ai"
  ],
  "network": "tcp",
  "outboundTag": "warp-official"
}
```

说明：

- 列表按你实际使用维护，不是永久完整清单。
- 对话中的推荐形态不含 Gemini；不用就不要加。
- **不要**默认把整个 `x.com` 塞进 WARP。Grok 网页若缺资源，再按失败域名追加。
- `inboundTag` 必须同时覆盖 HY2 和 REALITY，否则备用入口的 AI 流量不会走 WARP。
- WARP 挂了时，这些域名不会悄悄回 Direct（没有写 balancer）。这是刻意的。

### 11.3 重载并验证三条出口

```bash
/usr/local/bin/xray -test -config /usr/local/etc/xray/config.json
systemctl restart xray
```

客户端**不用改节点**。分流在服务端。

从已连接的客户端（或之后在 VPS 上用临时 inbound 测试）确认：

| 访问 | 期望出口 |
|---|---|
| 普通网站 | VPS 机房 IPv4 |
| chatgpt.com / openai.com | WARP IP |
| grok.com / x.ai | WARP IP |

`[VPS]` 辅助：

```bash
# Direct
curl -4 https://api.ipify.org
# WARP
curl --proxy socks5h://127.0.0.1:40000 https://api.ipify.org
```

Grok 若在 Direct 下异常：优先确认它走了 WARP 规则，而不是先把整机 IPv6 关死。

WARP 不保证 ChatGPT / Grok 一定可用，也不是住宅 IP。若 Direct 已经能开，可以先直连、被拦再切 WARP；推荐形态是预先把这两类域名指到 WARP，减少机房 IP 信誉问题。

---

## 12. 阶段 9：WARP watchdog（systemd timer）

Xray 生命周期由 `xray.service` 负责。WARP 要测**真实出口**，不要只看 daemon。

### 12.1 `/usr/local/sbin/warp-watchdog.sh`

```bash
#!/usr/bin/env bash
set -u

TEST_URL="https://api4.ipify.org"
MAX_TIME=8

if ! curl --proxy socks5h://127.0.0.1:40000 \
  --silent --fail --max-time "$MAX_TIME" \
  "$TEST_URL" >/dev/null 2>&1; then

  logger -t warp-watchdog "WARP proxy check failed; reconnecting"

  warp-cli disconnect >/dev/null 2>&1 || true
  sleep 2
  warp-cli connect >/dev/null 2>&1 || true
  sleep 3

  if curl --proxy socks5h://127.0.0.1:40000 \
    --silent --fail --max-time "$MAX_TIME" \
    "$TEST_URL" >/dev/null 2>&1; then
    logger -t warp-watchdog "WARP recovered"
  else
    logger -t warp-watchdog "WARP recovery FAILED"
  fi
fi
```

```bash
chmod 700 /usr/local/sbin/warp-watchdog.sh
```

### 12.2 `/etc/systemd/system/warp-watchdog.service`

```ini
[Unit]
Description=Check WARP local proxy real egress
After=warp-svc.service

[Service]
Type=oneshot
ExecStart=/usr/local/sbin/warp-watchdog.sh
```

### 12.3 `/etc/systemd/system/warp-watchdog.timer`

```ini
[Unit]
Description=Run WARP egress watchdog every 5 minutes

[Timer]
OnBootSec=2min
OnUnitActiveSec=5min
Persistent=true

[Install]
WantedBy=timers.target
```

```bash
systemctl daemon-reload
systemctl enable --now warp-watchdog.timer
systemctl list-timers | grep warp
```

新部署用 systemd timer。不要同时再加一份 cron 做同一件事。

---

## 13. 最终 `config.json` 形态（脱敏）

HY2 + REALITY 都启用、WARP 已在 40000 监听之后，目标结构如下。把 `YOUR_*` 换成真实值，不要提交这份含密钥的文件。

```json
{
  "log": {
    "access": "/var/log/xray/access.log",
    "error": "/var/log/xray/error.log",
    "loglevel": "warning"
  },
  "inbounds": [
    {
      "tag": "reality-vision",
      "listen": "0.0.0.0",
      "port": 443,
      "protocol": "vless",
      "settings": {
        "clients": [
          {
            "id": "YOUR_UUID",
            "flow": "xtls-rprx-vision"
          }
        ],
        "decryption": "none"
      },
      "streamSettings": {
        "method": "raw",
        "security": "reality",
        "realitySettings": {
          "show": false,
          "target": "www.microsoft.com:443",
          "serverNames": ["www.microsoft.com"],
          "privateKey": "YOUR_REALITY_PRIVATE_KEY",
          "shortIds": ["YOUR_SHORT_ID"]
        }
      },
      "sniffing": {
        "enabled": true,
        "destOverride": ["http", "tls", "quic"],
        "metadataOnly": false
      }
    },
    {
      "tag": "hy2-xr",
      "listen": "0.0.0.0",
      "port": 24443,
      "protocol": "hysteria",
      "settings": {
        "version": 2,
        "users": [
          {
            "auth": "YOUR_HY2_PASSWORD",
            "email": "personal"
          }
        ]
      },
      "streamSettings": {
        "method": "hysteria",
        "security": "tls",
        "tlsSettings": {
          "alpn": ["h3"],
          "certificates": [
            {
              "certificateFile": "/usr/local/etc/xray/certs/fullchain.pem",
              "keyFile": "/usr/local/etc/xray/certs/key.pem"
            }
          ]
        },
        "hysteriaSettings": {
          "version": 2,
          "udpIdleTimeout": 60
        }
      },
      "sniffing": {
        "enabled": true,
        "destOverride": ["http", "tls", "quic"],
        "metadataOnly": false
      }
    }
  ],
  "outbounds": [
    {
      "tag": "direct",
      "protocol": "freedom",
      "settings": {
        "domainStrategy": "UseIPv4"
      }
    },
    {
      "tag": "warp-official",
      "protocol": "socks",
      "settings": {
        "address": "127.0.0.1",
        "port": 40000
      }
    },
    {
      "tag": "block",
      "protocol": "blackhole",
      "settings": {}
    }
  ],
  "routing": {
    "domainStrategy": "IPOnDemand",
    "rules": [
      {
        "type": "field",
        "inboundTag": ["hy2-xr", "reality-vision"],
        "ip": [
          "10.0.0.0/8",
          "172.16.0.0/12",
          "192.168.0.0/16",
          "127.0.0.0/8",
          "169.254.0.0/16",
          "fc00::/7",
          "fe80::/10"
        ],
        "outboundTag": "block"
      },
      {
        "type": "field",
        "inboundTag": ["hy2-xr", "reality-vision"],
        "domain": [
          "domain:openai.com",
          "domain:chatgpt.com",
          "domain:oaistatic.com",
          "domain:oaiusercontent.com",
          "domain:grok.com",
          "domain:x.ai"
        ],
        "network": "tcp",
        "outboundTag": "warp-official"
      }
    ]
  }
}
```

与仓库 example 的差异（按对话裁剪）：

- 有 REALITY + HY2
- Direct 使用 `UseIPv4`
- WARP 域名含 Grok，不含 Gemini（可按需加回）
- **没有** `static-socks` 和 Claude 规则

---

## 14. 本机 Agent 怎么用这份文档

密钥 SSH 成功后，对本机 Agent 的任务可以是：

> 按 `docs/deploy-hy2-reality-warp.md` 部署。Agent 在本机通过 `ssh vultr-edge` 施工。VPS 上只装 Xray 和 WARP Local Proxy，不要装桌面或编码 Agent。顺序：底座 → 证书 → HY2 客户端验证 → REALITY 客户端验证 → WARP 独立验证 → 再改 routing。ChatGPT/OpenAI 与 grok.com/x.ai 走 WARP；不要固定 SOCKS；不要改默认路由。密钥用占位符，不要把私钥写进仓库。

不要让它一次贴全家桶配置后宣布完成。

常用 `[本机]` 远程命令：

```bash
ssh vultr-edge 'systemctl is-active xray warp-svc'
ssh vultr-edge 'ss -lntup | grep -E "22|443|24443|40000"'
ssh vultr-edge '/usr/local/bin/xray -test -config /usr/local/etc/xray/config.json'
```

把配置从本机拷上去（先在本机放脱敏或本地加密副本）：

```bash
scp /path/to/config.json vultr-edge:/usr/local/etc/xray/config.json
ssh vultr-edge '/usr/local/bin/xray -test -config /usr/local/etc/xray/config.json && systemctl restart xray'
```

---

## 15. 收工检查

声称完成前核对：

1. `[本机] ssh vultr-edge` 仍可用
2. `xray -test` 通过
3. 监听：TCP 22、TCP 443、UDP 24443、回环 TCP 40000；不要把 40000 暴露到公网
4. HY2 客户端密码 / SNI / 证书与服务器一致
5. REALITY 的 UUID、公钥、shortId、serverName、flow 与服务器一致；私钥只在服务器
6. Direct：`curl -4` 出口为机房 IPv4
7. WARP：经 `127.0.0.1:40000` 的 `cdn-cgi/trace` 含 `warp=on`，且 IP 与 Direct 不同
8. ChatGPT / Grok 实际走 WARP（客户端连上后看各站点出口或服务端 access 日志）
9. `ip -4 route` 默认网关仍是 Vultr
10. 主机 UFW 与 Vultr Firewall Group 一致
11. watchdog timer 已 enable
12. 仓库、聊天、日志里没有真实密码、私钥、UUID、证书、订阅链接

不要用一次测速宣布健康。至少分别用：

- 电脑 Wi-Fi + HY2
- 手机蜂窝 + HY2（不行则切 REALITY）
- 晚高峰再看连通，而不是只看带宽数字

客户端端到端成功，只有在对应设备上真的测过才能写进结论。

---

## 16. 排错顺序

入口（连不上节点）：

```text
本机能否 ssh
UFW / Vultr 安全组是否放行对应端口
xray 是否 active
HY2：UDP 24443、证书、密码、SNI
REALITY：TCP 443、UUID、pbk/sid、sni、flow
当前网络是否禁 UDP（禁则改用 REALITY）
```

出口（能连上但 ChatGPT / Grok 异常）：

```text
1. warp-cli status
2. warp-cli settings
3. ss -lntp | grep 40000
4. curl --proxy socks5h://127.0.0.1:40000 https://www.cloudflare.com/cdn-cgi/trace
5. 确认 warp=on
6. Xray outbound 是否指向 127.0.0.1:40000
7. routing 域名是否覆盖实际访问的 host
8. 不要先改 inbound
```

---

## 17. 不要做的事

- 把 root 密码、SSH 私钥、HY2 密码、REALITY 私钥贴进聊天或 git
- 密钥登录成功前关闭密码登录或丢掉 22
- 在 1G 机器上装图形桌面或再跑一套编码 Agent
- `warp-cli mode warp` 把整机默认路由带走
- 为了抄仓库 example 而开启你没用的 SOCKS / Claude / Tunnel
- 未验证 HY2 就同时改 WARP routing
- 宣称这套方案能防止封号，或宣称 WARP 是住宅 IP

# 🌐 自建科学上网完整指南（2026）

> 架构：VLESS + WebSocket + TLS  
> 目标：稳定、可复用、可长期使用  
> 适用：iOS（Shadowrocket） / macOS（Clash Verge）

---

# 📖 一、整体思路（为什么这样设计）

很多人一开始会误区：

- 只关注“协议”
- 忽略“线路”

但真实情况是：

> **线路质量 > 协议选择 > 配置细节**

本方案选择：

- 韩国 Seoul（优质线路）
- VLESS（轻量协议）
- WS + TLS（伪装成 HTTPS）

---

# 🧭 二、最终架构

```
客户端（iPhone / Mac）
        ↓
VLESS + WS + TLS（加密 + 伪装）
        ↓
域名（直连 DNS）
        ↓
Seoul VPS（Xray）
```

---

# 🌏 三、为什么选择 Seoul

## 地区对比

| 地区 | 延迟 | 适合用途 |
|------|------|----------|
| 韩国 | ⭐ 80–120ms | 推荐 |
| 日本 | ❌ 200–300ms | 不稳定 |
| 香港 | ⚠️ 很低 | 但受限 |

---

## 原理解释

中国 → 韩国：

- 海底光缆直连
- 跳数少
- 不绕美国

中国 → 日本（常见）：

- 经美国中转
- 延迟翻倍
- 易被限速

---

# 🧱 四、VPS 申请

选择：

- 地区：Seoul
- 系统：Ubuntu 22.04
- 最低配置即可

---

# 🌐 五、DNS 配置

添加：

```
A  @  → VPS_IP
```

验证：

```bash
dig yourdomain.com +short
```

---

# 🔐 六、服务器初始化

```bash
ssh root@your_ip
apt update && apt upgrade -y
```

---

## 防火墙

```bash
ufw allow 22/tcp
ufw allow 80/tcp
ufw allow 443/tcp
ufw allow 54321/tcp
ufw enable
```

---

# 🔒 七、SSL 证书（为什么必须）

## 原理

TLS 的作用：

- 加密流量
- 模拟 HTTPS
- 避免被识别为代理

---

## 安装

```bash
curl https://get.acme.sh | sh
source ~/.bashrc
```

---

## 申请

```bash
~/.acme.sh/acme.sh --issue -d yourdomain.com --standalone
```

---

## 安装

```bash
mkdir -p /etc/xray/certs

~/.acme.sh/acme.sh --install-cert -d yourdomain.com \
--key-file /etc/xray/certs/domain.key \
--fullchain-file /etc/xray/certs/domain.crt
```

---

# ⚙️ 八、安装 x-ui

```bash
bash <(curl -Ls https://raw.githubusercontent.com/FranzKafkaYu/x-ui/master/install.sh)
```

访问：

```
http://your_ip:54321
```

---

# 🧩 九、节点配置（核心）

## 参数

- 协议：VLESS
- 端口：443
- 传输：WS
- 路径：/cdn-cgi
- TLS：开启
- SNI：yourdomain.com

---

## 为什么用 WS

WS（WebSocket）：

- 伪装 HTTP 流量
- 更难识别
- 可走 CDN（可选）

---

# 📱 十、客户端配置

## iOS（Shadowrocket）

- 地址：yourdomain.com
- 端口：443
- 协议：VLESS
- WS：/cdn-cgi
- TLS：开启

---

## macOS（Clash Verge）

核心 YAML：

```yaml
proxies:
  - name: seoul
    type: vless
    server: yourdomain.com
    port: 443
    uuid: YOUR_UUID
    tls: true
    servername: yourdomain.com
    network: ws
    ws-opts:
      path: /cdn-cgi
```

---

# 🔍 十一、测试

```bash
curl https://ip.sb
ping yourdomain.com
```

---

# 🚀 十二、优化建议

## 1️⃣ 多 IP 测试

同一机房不同 IP 差异极大

---

## 2️⃣ 路径伪装

推荐：

```
/cdn-cgi
```

---

## 3️⃣ 双节点策略

- 主：Seoul（快）
- 备：Cloudflare（稳）

---

# 🔥 十三、关键经验总结

## ❗ 1. 线路最重要

Tokyo ❌  
Seoul ✅  

---

## ❗ 2. WS ≠ 最快，但最稳

---

## ❗ 3. TLS 必须

---

# 🎯 最终总结

> 一台 Seoul VPS + VLESS + WS + TLS  
> = 当前最简单且稳定的方案

---

# 🚀 进阶方向

- Reality（更快）
- 多节点负载均衡
- 专线 VPS

---

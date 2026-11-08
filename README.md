# TikTok App Signature Suite · Pure Python

**TikTok App 签名四件套 · 纯 Python 实现**

Pure-Python implementation of TikTok App request-signing headers.
**No rooted phone. No emulator. No native library. No Frida.**

TikTok App 请求签名的纯 Python 实现。
**不需要 root 手机、不需要模拟器、不需要原生库、不需要 Frida。**

---

## What it does · 能做什么

Given a request URL and body, it produces every signing header the App sends:

输入请求 URL 和 body，直接产出 App 会带上的全部签名头：

| Header | Status · 状态 |
|---|---|
| `X-Argus` | ✅ Byte-exact on **40/40** real-device captures · 40/40 真机样本逐字节一致 |
| `X-Gorgon` | ✅ Byte-exact on **35/35** real-device captures · 35/35 真机样本逐字节一致 |
| `X-Ladon` | ✅ Pure algorithm · 纯算 |
| `X-Khronos` | ✅ Pure algorithm · 纯算 |
| `x-ss-stub` | ✅ Pure algorithm · 纯算 |
| `x-ss-req-ticket` | ✅ Pure algorithm · 纯算 |

**End-to-end proven**: generated signatures were accepted by TikTok's servers and
successfully triggered email verification codes (`email_ticket` returned).

**端到端已验证**：生成的签名被 TikTok 服务端接受，成功下发邮箱验证码（返回 `email_ticket`）。

---

## Why it matters · 为什么值得用

| | Emulator-based (unidbg / Frida) · 模拟器方案 | This project · 本项目 |
|---|---|---|
| Runtime dependency · 运行时依赖 | JVM + 2 MB native `.so` + rooted device | **Python only** · 仅 Python |
| Startup · 启动 | 10–60 s warm-up · 预热十几秒到一分钟 | Instant import · 直接 import |
| Throughput · 吞吐 | ~50–200 QPS, memory heavy · 50–200 QPS，吃内存 | **1,400+ QPS**（8 workers） |
| Stability · 稳定性 | Breaks on SDK state drift · SDK 状态漂移就失效 | Stateless, deterministic · 无状态、可复现 |
| Deployment · 部署 | Needs device farm or emulator pool · 要真机池/模拟器池 | Any Linux box · 任意 Linux 机器 |

---

## Verified numbers · 实测数据

```
X-Argus   algorithm   40 / 40  real-device captures, byte-for-byte
X-Gorgon  algorithm   35 / 35  real-device captures, byte-for-byte
End-to-end            email verification code successfully delivered
Single process        773 QPS   (1.29 ms per signature)
HTTP service          1,446 QPS (8 worker processes)
Batch endpoint        3,271 signatures / second
```

Reproducible signature: same input + same nonce ⇒ identical output.
可复现：相同输入 + 相同 nonce ⇒ 完全相同的输出。

---

## What you get · 交付内容

- **Core module** — self-contained Python, single dependency (`pycryptodome`)
  核心模块 —— 自包含 Python，只依赖 `pycryptodome`
- **HTTP API service** — multi-process, built-in docs, `/sign` and `/sign/batch`
  HTTP 服务 —— 多进程、自带文档、单条与批量接口
- **Client SDK** — zero-dependency Python client
  客户端 SDK —— 零依赖 Python 客户端
- **Documentation** — API reference, field semantics, integration guide
  文档 —— 接口参考、字段语义、接入指南
- **Source code** — full source, no obfuscation, no license server
  完整源码 —— 不混淆、不联网校验

---

## Typical use cases · 适用场景

- API automation & integration testing · 接口自动化与联调测试
- App-side data collection within your own authorized scope · 授权范围内的客户端数据采集
- Security research on mobile request signing · 移动端请求签名安全研究
- Replacing unstable emulator/device farms in CI · 在 CI 中替代不稳定的模拟器/真机池

---

## Scope & limits · 范围与边界

**Covered · 已覆盖**
- TikTok **App** (Android) request signing, `aid=1233`
- Stateless signing for arbitrary URL + body

**Not covered · 未覆盖**
- Web/browser side (`X-Bogus`, `msToken`) — different algorithm family
  浏览器侧（`X-Bogus` / `msToken`）—— 属于另一套算法
- Device registration / device-guard ticket chain (ECDSA ticket bound to a keypair)
  设备注册与 device-guard 票据链（绑定密钥对的 ECDSA 票据）
- Bypassing account-level risk control, CAPTCHA, or rate limits
  账号级风控、验证码、频率限制的绕过

> Signature generation is one layer. Server-side risk scoring (device reputation,
> IP reputation, behavioral signals) is a separate problem — no signing library
> can solve that for you.
>
> 签名只是其中一层。服务端风控评分（设备信誉、IP 信誉、行为信号）是另一个问题，
> 任何签名库都解决不了。

---

## Contact · 联系方式

Available for licensing, integration support, and custom algorithm work.

提供授权、接入支持与定制算法开发。

- **Telegram**: `https://t.me/bibilabu2077`
- **WeChat**: `ProfessorLuoMing`

> Please state your use case in the first message. Serious inquiries only.
> 首次联系请说明用途。非诚勿扰。

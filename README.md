# 📧 GoToEmail

**通过 IMAP/SMTP 将邮箱绑定到 OpenClaw / Hermes Agent，让 AI 助手直接管理邮件。**

[![ClawHub](https://img.shields.io/badge/ClawHub-gotoemail-blue?logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHBhdGggZmlsbD0id2hpdGUiIGQ9Ik0yMCA0SDRjLTEuMSAwLTIgLjktMiAydjEyYzAgMS4xLjkgMiAyIDJoMTZjMS4xIDAgMi0uOSAyLTJWNmMwLTEuMS0uOS0yLTItMnptMCAydjEuNWwtOCA1LTgtNVY2aDE2ek00IDE4VjkuNWw4IDUgOC01VjE4SDR6Ii8+PC9zdmc+)](https://claw-hub.net/skills/gotoemail)
[![Version](https://img.shields.io/badge/version-1.0.1-green)](https://github.com/feixuelingcloud/gotoemail/releases)
[![License: MIT-0](https://img.shields.io/badge/license-MIT--0-lightgrey)](LICENSE)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue)](https://www.python.org/)

---

## 支持的邮箱

| 邮箱 | 认证方式 | 状态 |
|------|----------|------|
| **163邮箱** / 126邮箱 / yeah.net | 邮箱授权码 | ⚡ 推荐 |
| **QQ邮箱** / Foxmail | 邮箱授权码 | ⚡ 推荐 |
| **Gmail** | App 密码 | ✅ 支持 |
| **雅虎邮箱** (Yahoo) | App 密码 | ✅ 支持 |
| **企业邮箱** | 密码/授权码 + 自定义服务器 | ✅ 支持 |
| Outlook / M365 / iCloud | OAuth | ❌ 不支持 |

---

## 安装

### 方式一：通过 ClawHub 安装（推荐）

```bash
clawhub install gotoemail
```

### 方式二：手动安装

```bash
git clone https://github.com/feixuelingcloud/gotoemail.git ~/.claude/skills/gotoemail
```

---

## 使用方式

安装后在 OpenClaw 或 Hermes Agent 中直接说：

> "帮我绑定163邮箱"  
> "添加一个QQ邮箱账户"  
> "读一下我的收件箱"  
> "发邮件给 xxx@example.com"

---

## 技能文件结构

```
gotoemail/
├── SKILL.md                    # 技能定义（触发条件 + 操作流程）
├── scripts/
│   ├── test_connection.py      # 测试 IMAP/SMTP 连接
│   ├── list_emails.py          # 获取邮件列表
│   ├── read_email.py           # 读取单封邮件
│   └── send_email.py           # 发送邮件
└── references/
    └── providers.md            # 授权码获取指南 + 错误排查
```

---

## 脚本 API（JSON via stdin / stdout）

所有脚本通过标准输入接收 JSON，标准输出返回 JSON。

### 测试连接

```bash
echo '{"email":"user@163.com","password":"授权码"}' | python scripts/test_connection.py
# → {"ok":true,"message":"邮箱 user@163.com 连接测试通过","config":{...}}
```

### 获取邮件列表

```bash
echo '{"email":"...","password":"...","imap_host":"imap.163.com","limit":20}' \
  | python scripts/list_emails.py
# → {"ok":true,"total":142,"returned":20,"emails":[...]}
```

### 读取单封邮件

```bash
echo '{"email":"...","password":"...","imap_host":"imap.163.com","uid":"1234"}' \
  | python scripts/read_email.py
# → {"ok":true,"subject":"...","from":"...","body":"...","attachments":[...]}
```

### 发送邮件

```bash
echo '{"email":"...","password":"...","smtp_host":"smtp.163.com","to":"recv@example.com","subject":"主题","body":"正文"}' \
  | python scripts/send_email.py
# → {"ok":true,"message":"邮件已成功发送至 recv@example.com"}
```

---

## 服务器配置参考

| 提供商 | IMAP | SMTP |
|--------|------|------|
| 163邮箱 | `imap.163.com:993` | `smtp.163.com:465` |
| 126邮箱 | `imap.126.com:993` | `smtp.126.com:465` |
| QQ邮箱 | `imap.qq.com:993` | `smtp.qq.com:465` |
| Gmail | `imap.gmail.com:993` | `smtp.gmail.com:465` |
| 雅虎 | `imap.mail.yahoo.com:993` | `smtp.mail.yahoo.com:465` |

企业邮箱：用户自定义，全程 **SSL/TLS**，默认端口 993 / 465。

---

## 系统要求

- Python 3.8+（标准库，无需额外安装依赖）

---

## License

[MIT-0](https://opensource.org/licenses/MIT-0) — 免费使用，无需署名。

# Oracle Cloud 管理面板

一个轻量级的 Oracle Cloud 实例管理面板，支持 IP 管理、自动抢机、流量监控等功能。

## 功能

- **实例管理** — 查看所有实例状态，30 分钟缓存 + 单实例刷新
- **IPv4 / IPv6 一键换 IP** — 支持手动更换，换完即时显示新地址
- **月度定时换 IP** — 每月随机日自动更换，失败 30 分钟重试
- **流量查询** — 通过 OCI Monitoring API 查询实例本月入站/出站流量
- **抢机** — 自动循环创建免费实例（ARM A1 / AMD E2.1.Micro），支持自定义 root 密码
- **Telegram 通知** — 换 IP 成功/失败、抢机成功等事件推送
- **日志持久化** — 运行日志写入数据库，刷新页面不丢失
- **IP 更换历史** — 记录每次换 IP 的时间、旧 IP、新 IP、来源
- **访问日志** — 记录登录/登出/操作，含客户端 IP
- **毛玻璃 UI** — 基于 Alpine.js + Tailwind CSS 的现代界面

## 一键部署

```bash
bash <(curl -fsSL https://raw.githubusercontent.com/yuanzhangdck/oracle-panel/main/install.sh)
```

## 部署后

1. 打开 `http://<服务器IP>:3001/login.html`
2. 初始密码保存在 `data/initial-admin-password.txt`
3. 登录后立即在 API Keys 页面修改密码

## 技术栈

- **后端** — Node.js + Express + Socket.IO
- **数据库** — SQLite3
- **OCI SDK** — oci-core / oci-identity / oci-monitoring
- **前端** — Alpine.js + Tailwind CSS（单文件 HTML）

## 目录结构

```
├── server.js          # 主服务
├── oci-client.js      # OCI API 封装
├── database.js        # 数据库初始化
├── public/            # 前端静态文件
│   ├── index.html     # 主界面
│   └── login.html     # 登录页
├── data/              # 运行时数据（不提交）
│   ├── oracle.db      # SQLite 数据库
│   └── settings.json  # 面板配置
├── uploads/           # 上传的 API 密钥（不提交）
├── install.sh         # 安装脚本
└── deploy.sh          # 部署脚本
```

## 注意事项

- `data/`、`uploads/` 目录下的文件为运行时数据，已在 `.gitignore` 中排除
- 抢机功能需要 OCI 账号有对应规格的配额
- IPv6 换 IP 需要实例所在子网已启用 IPv6 CIDR

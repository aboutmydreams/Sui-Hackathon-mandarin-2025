## project

- 项目名称: **Sui LP v3 Aggregator**
> 描述：聚合 Sui 生态多家 DeFi 协议的 LP 数据与操作入口，支持 Cetus、Bluefin、MMT Finance、Kriya、Magma Finance、Turbos（含 Pin/Not Pin），并内嵌 FlowX 与 Steamm 的 iframe 便于快捷交互。
- Sui 钱包地址: 0xcb2800da31772c4381d2c7d382fc5d6636e343df54099af7aaedcb85f9da77fb
> 描述：用于接收奖励

## 参与赛道

- [x] Sui 官方赛道
- [ ] Bucket 赛道
- [ ] Scallop 赛道
- [ ] Navi 赛道

> 说明：请按实际参赛勾选。

## Member

- [aboutmydreams](https://github.com/aboutmydreams)

> 自我介绍&技术栈：全栈（Next.js、React、TypeScript、Tailwind、NextAuth）、数据/合约集成（Sui SDK）、后端（API Key 网关/配额/统计）。

## 参赛信息

- 代码仓库：[github](https://github.com/d1vai/sui-defillama)

- 在线地址：<https://sui-lp3.d1v.xyz/>
- 演示文档 / PPT：<https://sui-lp3.d1v.xyz/docs>

---

# Sui LP v3 Aggregator

Sui LP v3 Aggregator 为 Sui 区块链上的 LP 数据聚合平台，提供统一界面以查看与管理来自多家 DeFi 协议的流动性池信息（价格、TVL、手续费、仓位等），并在应用内直接完成基础交互（通过内嵌 FlowX、Steamm）。目标是降低信息与操作分散带来的摩擦，提高 LP 管理效率与体验。

## 功能特性

- 多平台聚合：统一展示 Cetus、Bluefin、MMT、Kriya、Magma、Turbos 等协议池子数据。
- 实时数据：周期性拉取与刷新关键指标（价格、TVL、手续费等）。
- 账号登录：GitHub / Google 登录，启用个性化与受限数据访问。
- API Key 管理：Key 生命周期、用量统计、速率限制、权限控制一站式可视化。
- 管理员后台：用户/Key 管理，行为审计与风控策略入口。
- 内嵌交易：FlowX、Steamm iframe 即点即用，减少页面切换。
- 响应式设计：桌面与移动端均有良好体验。
- 品牌定制：自定义 Logo 与 Favicon，统一产品观感。

## 技术栈

- 框架：Next.js（App Router）
- 认证：NextAuth.js（GitHub / Google OAuth）
- UI：shadcn/ui + Tailwind CSS + Lucide 图标
- 数据：Server Actions / Route Handlers（统一数据拉取与 API）
- Sui 集成：Sui SDK（后续可拓展链上查询与签名）

## 本地开发

1. 克隆仓库
   ```bash
   git clone <你的仓库地址>
   cd sui-lp-aggregator
   ```
2. 安装依赖
   ```bash
   npm install
   # 或
   yarn install
   # 或
   pnpm install
   ```
3. 配置环境变量（在根目录创建 `.env.local`）
   ```env
   GITHUB_ID=你的GitHub OAuth App ID
   GITHUB_SECRET=你的GitHub OAuth App Secret
   GOOGLE_ID=你的Google OAuth App ID
   GOOGLE_SECRET=你的Google OAuth App Secret
   NEXTAUTH_SECRET=使用 openssl rand -base64 32 生成
   NEXTAUTH_URL=http://localhost:3000
   ```
   提示：如使用 Vercel 免费计划，OAuth 初次加载可能较慢，耐心等待即可。
4. 启动开发服务器
   ```bash
   npm run dev
   # 或
   yarn dev
   # 或
   pnpm dev
   ```
   打开 http://localhost:3000 查看应用。

## 部署

- Vercel：
  - 导入 Git 仓库
  - 在项目设置中配置与本地相同的环境变量
  - 一键部署，自动构建与发布
- 自托管：
  - 生产构建：`npm run build` / `yarn build`
  - 生产运行：`npm start` / `yarn start`

## 项目结构

```
.
├── app/
│   ├── api/                  # API 路由（Route Handlers）
│   │   ├── admin/            # 管理员能力
│   │   ├── auth/             # NextAuth 认证路由
│   │   ├── geo-auth-policy/  # 地理认证策略
│   │   └── ...               # 各平台数据 API
│   ├── auth/
│   │   └── signin/page.tsx   # 自定义登录页
│   ├── globals.css           # 全局样式
│   ├── icon.svg              # Favicon/Logo
│   ├── layout.tsx            # 根布局
│   └── page.tsx              # 仪表盘首页
├── components/
│   ├── ui/                   # shadcn/ui 组件
│   ├── auth-session-provider.tsx # NextAuth SessionProvider
│   ├── sign-in-gate.tsx      # 登录门组件
│   └── ...                   # 各平台池子组件
├── lib/
│   ├── acl-store.ts          # 访问控制与配额
│   ├── auth.ts               # NextAuth 配置
│   └── utils.ts              # 工具函数
├── public/                   # 静态资源
├── next.config.mjs           # Next.js 配置
├── package.json              # 依赖与脚本
├── tailwind.config.ts        # Tailwind 配置
└── tsconfig.json             # TypeScript 配置
```

## API Key 功能

内置完善的 API Key 体系，满足开发者程序化访问：
- 管理：创建 / 启用-禁用 / 删除
- 用量：实时请求量、错误率、性能指标
- 速率：每小时 1–10,000 次可配
- 历史：调用时间线与响应摘要
- 分析：按端点/HTTP 方法统计
- 安全：哈希存储、IP 追踪、端点级权限

快速开始：
1) 创建 Key：登录后访问 `/dashboard`，点击 Create Key，设置名称与限速；注意 Key 仅显示一次。
2) 使用示例：
   ```bash
   curl -H "Authorization: Bearer sk_live_your_api_key_here" \
     http://localhost:3000/api/protected/sample

   curl "http://localhost:3000/api/protected/sample?api_key=sk_live_your_api_key_here"
   ```
3) 监控：在 Dashboard 查看实时统计、平均响应时间、小时用量与错误率。

## 贡献

欢迎贡献！如有建议或发现问题，请提交 Issue 或 Pull Request。

## License

本项目使用 MIT 许可证。

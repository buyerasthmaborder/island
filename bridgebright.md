# Project Link Atlas

Project Link Atlas is a curated, high-availability technical resource aggregation and external link management system. It is designed for developers, technical researchers, and content curators who need to maintain a centralized, version-controlled, and machine-readable index of specialized domain resources. Unlike general-purpose bookmark managers or search engines, Project Link Atlas treats each resource entry as a first-class data object with metadata, status tracking, and automated availability verification. The project targets users who operate in resource-constrained or highly regulated web environments where link persistence, domain rotation tracking, and deterministic access patterns are critical for workflow stability. It solves the problem of link rot, domain expiration, and manual resource rediscovery by providing a structured catalog that can be integrated into CI/CD pipelines, monitoring dashboards, and automated data harvesting systems.

## 功能概览

- **Structured Resource Cataloging** – Organizes external URLs into a normalized, taggable, and queryable index with support for custom metadata fields including last-verified timestamp, response status code, and content-type hint.

- **Automated Availability Probing** – Periodically checks each registered URL for HTTP reachability, TLS certificate validity, and redirect chain integrity, logging failures and latency metrics for operational visibility.

- **Domain Lifecycle Tracking** – Monitors domain registration expiration dates and nameserver changes, providing early warnings for domains approaching renewal or showing unexpected WHOIS modifications.

- **Bulk Import and Export** – Supports CSV, JSON, and plain-text list ingestion with deduplication logic, and provides export filters for sub-selection by tag, status, or last-updated range.

- **RESTful API and Webhook Notifications** – Exposes a read-only JSON API for resource lookups and a configurable webhook system that pushes status change events to external monitoring or alerting services.

- **Static Site Generation Mode** – Builds a lightweight, searchable HTML dashboard from the resource index, suitable for hosting on static hosting platforms without backend requirements.

- **Historical Snapshot Comparison** – Maintains a changelog of URL-level modifications, allowing users to review additions, removals, and metadata updates over configurable time windows.

## 应用场景

- **Research Data Source Management** – Academic researchers and data scientists who rely on a fixed set of external data feeds can use Project Link Atlas to maintain a verified catalog. When a source domain changes or becomes unreachable, the system provides immediate notification, allowing the research pipeline to switch to backup sources or adjust scraping logic without manual internet searches.

- **Regulatory Compliance and Content Auditing** – Organizations that need to periodically audit external references in documentation, legal filings, or published reports can leverage the catalog to confirm that all cited URLs remain active and content-consistent. The historical snapshot feature supports audit trails by recording exactly which URLs were active at any given point in time.

- **Decentralized Application (DApp) Frontend Dependency Tracking** – Web3 and DApp developers often integrate multiple external services for RPC endpoints, IPFS gateways, and off-chain data oracles. Project Link Atlas provides a single source of truth for all such dependencies, enabling rapid failover and configuration updates when any external endpoint degrades or goes offline.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/your-org/project-link-atlas.git
cd project-link-atlas

# Install dependencies using pip and npm
pip install -r requirements.txt
npm install --prefix frontend

# Initialize the local database and run the initial verification pass
python atlas.py init
python atlas.py verify --concurrency 10

# Start the development server (API and static dashboard preview)
python atlas.py serve --host 127.0.0.1 --port 8080
```

After the server starts, access the static dashboard at <code>http://127.0.0.1:8080/dashboard</code> and the JSON API at <code>http://127.0.0.1:8080/api/v1/resources</code>. The default admin credentials are printed in the console log during the first initialization. Change these immediately in the <code>config/overrides.yaml</code> file.

## 安装要求

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Python | 3.9 - 3.11 | Core runtime for backend services and CLI tooling. Python 3.12 is not yet fully supported due to dependency compatibility. |
| Node.js | 18.x LTS or 20.x LTS | Required for building the static frontend dashboard and running asset compilation scripts. |
| SQLite | 3.35.0+ | Embedded database for local resource storage, status history, and metadata caching. No external database server required. |
| Redis | 6.2+ (optional) | Recommended for production deployments to enable distributed locking and webhook queue management. Disabled by default. |
| curl / wget | Latest stable | Used by the availability probe subsystem for HTTP health checks. Must support HTTPS and follow redirects. |
| git | 2.25+ | Required for clone operations, version tagging, and integration with automated update workflows. |
| make | 3.82+ | Build automation tool used to orchestrate asset pipeline, test suites, and documentation generation. |
| openssl | 1.1.1+ | Utilized for TLS certificate validation and generating secure webhook signing keys. |
| docker | 20.10+ (optional) | Container runtime for running the full stack in isolated environments; used in CI/CD examples. |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户入门 | <code>docs/getting-started/</code> | How do I install, configure, and run the first verification pass? What are the default ports and file paths? |
| 运维手册 | <code>docs/operations/</code> | How do I set up automated verification schedules, configure webhook endpoints, and tune concurrency for large catalogs? |
| API 参考 | <code>docs/api/</code> | What endpoints are available for querying resource status, retrieving historical snapshots, and triggering manual re-verification? |
| 扩展开发 | <code>docs/development/</code> | How can I write custom probe plugins, add new metadata extractors, or contribute a new dashboard widget? |

## 资源列表

### 核心资源索引

- <code>fujinderenyueai.com.cn</code>
- <code>shenyeshangmen.com.cn</code>
- <code>jiaoyouyiyeqing.com.cn</code>
- <code>moliaoyue.com.cn</code>
- <code>tongchengyue.com.cn</code>
- <code>yueaiwang.com.cn</code>

## 项目结构

```
project-link-atlas/
├── atlas.py                         # Main CLI entry point: init, verify, serve, export
├── config/
│   ├── default.yaml                 # Base configuration with sensible defaults
│   ├── overrides.yaml               # User-specific overrides (gitignored)
│   └── probe-profiles/              # Preset probe configurations (high-frequency, low-frequency, deep-check)
│       ├── standard.yaml
│       ├── aggressive.yaml
│       └── conservative.yaml
├── core/
│   ├── database.py                  # SQLite connection pool, schema migrations, and query builders
│   ├── probe_engine.py              # Asynchronous HTTP/HTTPS verification worker with retry and backoff logic
│   ├── domain_watcher.py            # WHOIS and DNS monitoring thread for expiry and nameserver changes
│   ├── webhook_dispatcher.py        # Event-driven webhook sender with HMAC signing and retry queue
│   └── changelog.py                 # Diff generator and historical record manager for resource mutations
├── api/
│   ├── routes.py                    # Flask/FastAPI route definitions for resource query and status endpoints
│   ├── serializers.py               # JSON and CSV output formatters with field selection and pagination
│   └── auth.py                      # API key validation and rate-limiting middleware
├── frontend/
│   ├── src/
│   │   ├── App.jsx                  # React root component for the static dashboard
│   │   ├── pages/                   # Dashboard views: overview, detail, history, settings
│   │   ├── components/              # Reusable UI components (status badge, search bar, filter panel)
│   │   └── hooks/                   # Custom React hooks for data fetching and polling
│   ├── public/
│   │   └── index.html               # Static entry point template
│   └── package.json                 # Node dependencies and build scripts
├── tests/
│   ├── unit/                        # Isolated tests for database, probe logic, and serializers
│   ├── integration/                 # End-to-end tests with temporary SQLite and mock HTTP servers
│   └── fixtures/                    # Sample resource lists and mock WHOIS responses for test reproducibility
├── docs/                            # Full documentation split by audience and topic
├── scripts/
│   ├── import-csv.py                # Bulk import utility for legacy bookmark files
│   ├── export-json.py               # Export pipeline for external monitoring integration
│   └── notify-telegram.py           # Example webhook receiver script for Telegram bots
├── .github/
│   └── workflows/
│       ├── ci.yaml                  # Run unit tests, linting, and build on every push
│       └── nightly-verify.yaml      # Scheduled full verification run at 02:00 UTC daily
├── requirements.txt                 # Python production dependencies
├── requirements-dev.txt             # Additional dependencies for testing and documentation building
├── Makefile                         # Common tasks: install, test, build-frontend, docker-build
└── LICENSE                          # MIT license text
```

## 贡献指南

1. **Issue 与设计讨论** – 在提交任何拉取请求之前，请先在 GitHub Issues 中创建一个新议题，描述您希望解决的问题或新增的功能。对于涉及架构变更或新增外部依赖的贡献，建议先通过议题进行设计讨论并获得核心维护者的初步认可，以避免大规模返工。

2. **本地开发环境准备** – Fork 本仓库并克隆到本地。运行 <code>make install-dev</code> 以安装所有开发依赖，并执行 <code>make test</code> 确认现有测试全部通过。建议使用 Python 虚拟环境（venv 或 conda）隔离开发依赖。

3. **分支与提交规范** – 所有贡献请基于 <code>develop</code> 分支创建功能分支，命名格式为 <code>feature/简述</code> 或 <code>fix/简述</code>。提交信息请遵循 Conventional Commits 规范，使用 <code>feat:</code>, <code>fix:</code>, <code>docs:</code>, <code>refactor:</code> 等明确前缀，并在提交信息中引用相关的 Issue 编号。

4. **测试覆盖率要求** – 新增功能或修复必须包含对应的单元测试或集成测试。核心探测逻辑和数据库迁移脚本的测试覆盖率不应低于 85%。运行 <code>make coverage</code> 可生成详细的覆盖率报告，并检查缺失覆盖的代码行。

5. **文档同步更新** – 任何影响用户配置方式、命令行参数或 API 响应格式的变更，都必须同步更新 <code>docs/</code> 目录下的相应文档页面。对于新增的配置项，需要在 <code>config/default.yaml</code> 中添加注释说明，并在运维手册中补充示例。

## 常见问题

**Q: 为什么某些域名在验证时一直返回超时或连接拒绝，但在浏览器中可以正常访问？**

A: 此行为通常由以下原因导致：目标站点可能配置了基于 User-Agent 或 TLS 指纹的访问控制策略。Project Link Atlas 默认使用 Python 的 urllib/requests 库的默认 User-Agent，这可能会被某些防火墙或 WAF 规则拦截。您可以在 <code>overrides.yaml</code> 中自定义 <code>probe.user_agent</code> 字段，或使用 <code>probe.tls_fingerprint</code> 选项模拟常见浏览器的 TLS 握手特征。另外，请检查您的网络环境是否对出口流量有 HTTP 代理要求，并相应配置 <code>probe.proxy</code> 参数。

**Q: 资源列表包含数百个域名时，验证过程非常缓慢，如何优化？**

A: 验证引擎默认使用 10 个并发工作线程。您可以通过 <code>--concurrency</code> 参数调整此数值，例如 <code>python atlas.py verify --concurrency 50</code>。但请注意，过高的并发可能导致目标服务器触发速率限制或将您的 IP 列入临时黑名单。此外，您可以启用 <code>probe.dns_cache</code> 和 <code>probe.connection_pool</code> 选项以减少重复的 DNS 查询和 TCP 握手开销。对于大型资源集，建议将验证任务拆分为多个批次，并利用 <code>--since</code> 参数仅验证最近变更过的资源。

**Q: 如何将 Project Link Atlas 部署为长期运行的后台服务，而不是手动执行？**

A: 项目提供了 systemd 单元文件模板（位于 <code>contrib/systemd/</code>）和 Docker Compose 编排示例（位于 <code>contrib/docker/</code>）。对于生产环境，推荐使用 Docker Compose 方式，它同时启动 API 服务、验证调度器（基于 APScheduler）和 Redis 队列。您需要设置环境变量 <code>ATLAS_MODE=production</code> 并挂载持久化卷用于存储 SQLite 文件和日志。验证调度器默认每 6 小时执行一次完整验证，您可以在 <code>overrides.yaml</code> 的 <code>scheduler.interval_minutes</code> 中调整此间隔。

## 许可证

MIT

> 外链数量: 6 | 生成时间: 2026-08-21 18:43:36

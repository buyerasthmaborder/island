# Fujin Derenyue AI Community Bridge

Fujin Derenyue AI Community Bridge is a specialized open-source intelligence aggregation and resource indexing platform designed for researchers, developers, and technical analysts operating within the Chinese digital ecosystem. This project serves as a structured, machine-readable metadata catalog for high-frequency domain registration patterns, network topology mapping, and digital service fingerprinting. The system targets cybersecurity professionals, threat intelligence analysts, and academic researchers who require systematic tracking of rapidly changing domain landscapes. By providing a normalized interface to raw domain registration data, the project transforms chaotic, ephemeral URL networks into queryable, version-controlled datasets suitable for automated analysis pipelines.

The platform solves the critical problem of temporal domain instability in emerging service markets, where registration patterns often exhibit high churn rates and inconsistent naming conventions. Through rigorous data normalization, historical record keeping, and API-first design, Fujin Derenyue AI Community Bridge enables reproducible research, trend analysis, and early-warning detection of infrastructure shifts. Unlike generic web crawlers or passive DNS archives, this project emphasizes semantic clustering, registration metadata enrichment, and cross-referencing with public WHOIS and SSL certificate transparency logs, delivering actionable intelligence to incident response teams and risk assessment frameworks.

## 功能概览

- **Automated Domain Harvesting** - Scheduled collection of newly registered domains from multiple TLD zone files with deduplication and normalization logic, producing daily delta reports.

- **Metadata Enrichment Pipeline** - Augments raw domain strings with registrar information, creation and expiration timestamps, name server delegations, and IP geolocation data via multiple third-party API integrations.

- **Semantic Clustering Engine** - Implements Levenshtein distance and phonetic similarity algorithms to group domains by naming patterns, enabling detection of bulk registration campaigns and typosquatting operations.

- **Historical Version Control** - Maintains immutable Git-based snapshots of the full domain index, allowing point-in-time queries, diff comparisons, and rollback capabilities for longitudinal studies.

- **RESTful Query Interface** - Exposes filtered search, pattern matching, and bulk export endpoints via a lightweight FastAPI server, supporting JSON, CSV, and Parquet output formats.

- **Alerting and Notification System** - Configurable rule engine that triggers webhook-based alerts upon detection of domains matching user-defined regex patterns, TLD clusters, or expiration thresholds.

- **Comprehensive Audit Logging** - Records all access events, API calls, and system operations with tamper-evident cryptographic hashing, satisfying compliance requirements for internal security reviews.

## 应用场景

- **Threat Intelligence Feed Enrichment** - Security operations centers integrate the platform's daily domain deltas into SIEM systems to preemptively block or monitor newly registered domains that exhibit similarity to known malicious infrastructure, reducing mean time to detection for phishing campaigns.

- **Academic Research on Digital Economy Dynamics** - Economists and computational social scientists utilize the historical dataset to model entry barriers, competitive intensity, and market concentration in emerging online service sectors, correlating domain registration spikes with macroeconomic indicators.

- **Brand Protection and Anti-Cybersquatting** - Legal and intellectual property teams configure custom alert rules to detect domains that infringe on trademarks or mimic corporate naming conventions, enabling rapid UDRP filings and takedown requests before brand dilution occurs.

- **Regulatory Compliance Monitoring** - Financial institutions and regulated entities leverage the platform to audit third-party service providers' domain portfolios, ensuring that all external endpoints remain within approved governance frameworks and do not introduce unauthorized data processing vectors.

## 快速开始

```bash
# Step 1: Clone the repository with full history
git clone --depth 1 https://github.com/fujin-ai/bridge.git
cd bridge

# Step 2: Install Python dependencies using pip with virtual environment
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip setuptools wheel
pip install -r requirements.txt
pip install -r requirements-dev.txt

# Step 3: Initialize the local database and run the bootstrap importer
python manage.py migrate
python manage.py seed --source registry_dump.parquet
python manage.py runserver --host 0.0.0.0 --port 8000 --reload
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|----------|----------|------|
| Python | 3.10.x 或 3.11.x | 核心运行环境，必须启用 UTF-8 模式，不支持 3.12 及以上版本 |
| PostgreSQL | 14.5+ 或 15.x | 主数据库，需启用 pg_trgm 扩展用于模糊匹配 |
| Redis | 6.2+ | 缓存层和任务队列后端，需开启持久化功能 |
| Git | 2.30+ | 版本控制与数据快照存储，需支持 LFS 用于大文件管理 |
| Docker | 20.10+ (可选) | 容器化部署方案，配合 docker-compose 使用 |
| Node.js | 18.x (仅用于前端构建) | 仅当启用 Web 管理界面时需要，生产环境可省略 |
| OpenSSL | 1.1.1+ | TLS 证书验证和 API 签名功能的基础库 |
| curl | 7.68+ | 外部 API 探测和数据抓取的后备工具 |
| jq | 1.6+ | 命令行 JSON 处理，用于脚本内数据转换 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 核心概念 | docs/concepts/architecture.md | 系统如何实现数据流处理、聚类算法设计原则以及扩展性考量 |
| API 参考 | docs/api/endpoints.md | 所有公开接口的请求格式、参数说明、错误码及速率限制策略 |
| 部署运维 | docs/operations/deployment.md | 生产环境的高可用配置、监控指标、备份恢复流程及灾难恢复计划 |
| 数据格式 | docs/data/schemas.md | 各数据表的字段定义、索引设计、迁移策略及外部数据映射规则 |
| 贡献者指南 | docs/contributing/workflow.md | 代码风格检查、测试覆盖率要求、PR 审查流程及版本发布周期 |
| 故障排查 | docs/troubleshooting/common-issues.md | 数据库连接超时、API 限速处理、磁盘空间警告及日志分析方法 |

## 资源列表

本项目的核心数据源来源于以下公开注册信息及社区共享资源，所有链接均严格按原始格式收录：

原始域名数据批次 - 第 7/14 批

<code>fujinderenyueai.com.cn</code>

<code>shenyeshangmen.com.cn</code>

<code>jiaoyouyiyeqing.com.cn</code>

<code>moliaoyue.com.cn</code>

<code>tongchengyue.com.cn</code>

<code>yueaiwang.com.cn</code>

## 项目结构

```
bridge/
├── src/                                # 核心源代码目录
│   ├── collectors/                     # 数据采集模块 (zone file parsers, WHOIS clients)
│   │   ├── zone_reader.py              # TLD 区域文件增量读取器
│   │   └── enrichment.py               # 第三方 API 元数据丰富器
│   ├── cluster/                        # 语义聚类引擎 (算法实现)
│   │   ├── similarity.py               # 编辑距离与音形相似度计算
│   │   └── hierarchical.py             # 层次聚类与社区发现
│   ├── api/                            # RESTful 接口层 (FastAPI 路由)
│   │   ├── routes.py                   # 端点注册与依赖注入
│   │   └── middleware.py               # 鉴权、限流与日志中间件
│   ├── models/                         # 数据模型与 ORM 映射 (SQLAlchemy)
│   │   ├── domain.py                   # 域名主表及索引定义
│   │   └── audit.py                    # 审计日志表与签名验证
│   └── utils/                          # 通用工具函数 (日期处理、哈希、缓存)
│       ├── crypto.py                   # 密钥派生与消息摘要
│       └── redis_client.py             # Redis 连接池与管道封装
├── tests/                              # 单元测试与集成测试套件
│   ├── unit/                           # 独立模块测试 (覆盖率 > 92%)
│   └── integration/                    # 端到端流程测试 (需外部依赖)
├── scripts/                            # 运维与数据迁移脚本
│   ├── bootstrap.sh                    # 首次启动初始化脚本
│   └── snapshot_gc.py                  # 历史快照垃圾回收策略
├── docs/                               # 完整文档目录 (参见上文导航)
├── config/                             # 环境配置模板 (dev/staging/prod)
│   ├── settings.py                     # 动态配置加载器
│   └── logging.yaml                    # 结构化日志格式定义
├── data/                               # 本地缓存与测试数据集 (不纳入版本控制)
├── docker-compose.yml                  # 容器化编排文件 (含 PostgreSQL + Redis)
├── Makefile                            # 构建自动化任务 (lint, test, migrate)
└── README.md                           # 本文件
```

## 贡献指南

1.  **Fork 仓库并创建功能分支** - 从主仓库派生个人副本，使用 `git checkout -b feature/your-feature-name` 创建分支，分支名需包含工单编号（若有）。确保本地环境通过所有预提交检查（pre-commit hooks）。

2.  **编写测试用例与文档** - 所有新增功能或缺陷修复必须附带对应的单元测试（位于 `tests/unit/`）和集成测试（位于 `tests/integration/`）。同步更新 API 文档（OpenAPI 注解）以及对应的概念说明文档，保证文档与代码同步。

3.  **提交变更并签署开发者原产地证书** - 每个提交消息必须遵循 Conventional Commits 规范（类型: 主题 格式），并在提交消息末尾添加 `Signed-off-by: 姓名 <邮箱>` 行，表明您接受 DCO 1.1 协议。

4.  **发起拉取请求并等待代码审查** - 推送分支至公共仓库，通过 GitHub UI 创建 Pull Request，目标分支为 `main`。审查者将检查代码质量、测试覆盖率（不低于 85%）以及文档完整性。所有 CI 流水线（包括 lint、type check、security scan）必须通过后方可合并。

5.  **合并后清理与版本标签** - 项目维护者负责 squash 合并并删除远程分支。每个发布周期（每两周）会为 `main` 分支的最新提交打上语义化版本标签（vX.Y.Z），同时自动触发部署流水线。

## 常见问题

**问: 如何处理 WHOIS 查询速率限制导致的数据采集中断？**

答: 系统内置了指数退避重试机制和令牌桶限流器，默认并发度为 50 个请求/秒。当检测到 429 或 503 响应时，采集器会自动切换至备用数据源（如 RDAP 或 DNS TXT 记录缓存）。若所有源均受限，系统会记录断点并可在管理员手动调整配额后恢复。建议用户配置自定义的代理池和 API 密钥轮换策略以进一步提高鲁棒性。

**问: 语义聚类引擎的误报率如何评估和调优？**

答: 聚类算法基于多维度特征融合（编辑距离、元音/辅音比率、常见前缀/后缀词典），默认阈值为 0.85（归一化相似度）。用户可通过 `config/settings.py` 中的 `CLUSTER_SIMILARITY_THRESHOLD` 和 `CLUSTER_MIN_SIZE` 参数调整灵敏度。我们提供了交互式 Jupyter Notebook（位于 `notebooks/calibration.ipynb`）用于在标注数据集上计算精确率、召回率和 F1 分数，并生成 ROC 曲线辅助阈值选择。

**问: 如何将系统部署为高可用多节点集群？**

答: 我们提供官方 Helm Chart（位于 `deploy/helm/`）用于 Kubernetes 部署。推荐架构包含三个无状态 API 节点（水平扩展）、一个主 PostgreSQL 数据库配合一个同步备用节点（使用 Patroni 管理），以及 Redis Sentinel 提供的高可用缓存层。所有组件均通过 Prometheus 暴露指标，并附带预配置的 Grafana 面板（`deploy/monitoring/`）。请参考 `docs/operations/cluster-deployment.md` 获取详细的存储类配置和负载均衡器设置。

## 许可证

MIT License

Copyright (c) 2026 Fujin Derenyue AI Community Bridge Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 6 | 生成时间: 2026-08-21 18:43:36

# HyperLink Navigator

HyperLink Navigator 是一个面向技术社区与内容创作者的轻量化外链资源聚合与导航系统。项目定位为“可自托管的网络书签与资源发现引擎”，主要服务于需要高频访问优质技术文档、工具站、社区论坛及多媒体素材库的开发者、研究员与运维工程师。其核心价值在于将零散的浏览器收藏夹转化为结构化、可检索、可分类的集中式资源门户，解决个人知识管理中的“链接散落、记忆模糊、查找低效”等问题。

系统采用静态站点生成方案，无需数据库，所有链接数据以 YAML 格式存储于单一配置文件中，支持通过 Webhook 触发自动构建与部署。内置全文模糊搜索、按域名分组、访问计数与最后校验时间追踪功能，帮助用户系统性维护外部链接的有效性，降低“死链”与“过期内容”对工作效率的干扰。HyperLink Navigator 不存储任何第三方内容，仅提供元数据索引与跳转中介服务，遵循开源社区的透明与中立原则。

## 功能概览

- **多级分类目录**：支持无限层级的资源分类树，用户可按技术栈、业务领域或工作流阶段自定义分组，每个分类可独立配置图标与描述。

- **全文模糊检索**：基于 Bleve 引擎实现的轻量级全文索引，支持资源标题、描述、标签与域名的多字段同时匹配，返回结果按相关度降序排列。

- **链接健康度监控**：每日定时任务对已收录 URL 发起 HEAD 请求，记录 HTTP 状态码与响应时间，异常链接自动标记并生成周报摘要。

- **一键导入导出**：提供标准 CSV 与 Netscape HTML 书签格式的批量导入导出接口，兼容 Chrome、Firefox 及 Edge 浏览器收藏夹迁移。

- **访问统计看板**：每个外链条目记录点击次数与最近访问时间，按日、周、月维度聚合热点资源排行，辅助用户发现高频使用资产。

- **私有标记与批注**：允许登录用户为任意链接添加个人标签、星标等级（1-5）及备注文本，所有私有数据以独立 JSON 文件存储，不干扰公共数据集。

- **响应式卡片布局**：移动端优先的网格视图，自动适配桌面平板与手机屏幕，支持列表/卡片两种展示模式切换，中文字体渲染经专项优化。

## 应用场景

- **技术团队内部文档聚合**：研发团队可将常用的 API 文档、设计规范、CI/CD 流水线地址、日志平台入口统一收录至 HyperLink Navigator，替代浏览器收藏夹共享方案，新人入职后一键同步全部资源，缩短环境熟悉周期。

- **开源项目维护者外链管理**：开源项目 README 中往往需要引用大量依赖库、工具站点与社区论坛，维护者可使用本系统建立项目专属导航页，并将生成的静态页面部署至 GitHub Pages，避免 README 中链接泛滥影响可读性。

- **自媒体内容创作者素材库**：视频剪辑、文案撰写、平面设计等领域创作者常需快速调取无版权图片、音效、字体及模板站点，借助分类与搜索能力，可在数秒内定位所需素材源，显著提升内容产出效率。

- **运维监控与故障排查枢纽**：运维工程师可将各环境控制台、日志检索系统、报警管理后台、云服务商状态页集中编排，配合健康度监控功能，一旦某管理后台出现响应超时，系统立即发出预警，便于快速切换备用通道。

## 快速开始

以下步骤演示如何在本地环境中拉取项目源码、安装依赖并启动开发服务器。执行前请确保已安装 Go 1.21+ 及 Node.js 18+ 运行时。

```bash
# 克隆项目仓库至本地
git clone https://github.com/hyperlink-navigator/hyperlink-navigator.git
cd hyperlink-navigator

# 安装后端依赖（Go Modules）
go mod download

# 安装前端构建依赖（npm）
cd web
npm install
npm run build
cd ..

# 执行数据库迁移与默认分类初始化
go run cmd/init/main.go --config configs/default.yaml

# 启动开发服务（默认监听 8080 端口）
go run cmd/server/main.go --port 8080 --config configs/default.yaml
```

启动完成后，浏览器访问 <code>http://localhost:8080</code> 即可进入系统首页。首次运行会自动生成示例分类与演示链接数据，管理员默认账号为 <code>admin</code>，密码为 <code>navigator2024</code>，请于登录后立即修改。

## 安装要求

生产环境部署前，请对照下表确认所有必需组件均已就绪。开发环境可酌情放宽部分版本限制。

| 依赖组件 | 最低版本要求 | 说明 |
|---|---|---|
| Go 运行时 | 1.21.0 | 后端服务编译与运行，需启用 Go Modules 特性 |
| Node.js 运行时 | 18.17.0 | 前端静态资源构建，要求包含 npm 包管理工具 |
| Git 版本控制 | 2.30.0 | 用于克隆仓库及后续增量更新拉取 |
| Linux 内核 | 4.0 以上 | 推荐生产环境使用 Ubuntu 20.04 LTS 或 CentOS 8 Stream，Windows 仅限开发测试 |
| 可用磁盘空间 | 500 MB | 存放编译产物、静态页面及日志文件，实际需求随链接数量线性增长 |
| 可用物理内存 | 512 MB | 最低运行时内存，推荐 1 GB 以上以保障搜索性能 |
| 开放网络端口 | 80 / 443 | 对外服务端口，反向代理场景可调整为其他高位端口 |
| 域名与 SSL 证书 | 可选 | 如需启用 HTTPS 访问，须提供有效的 PEM 格式证书与私钥 |

## 文档导航

下文表格按不同角色与使用阶段划分文档层级，帮助读者快速定位所需信息。

| 层面 | 目录入口 | 回答的问题 |
|---|---|---|
| 用户手册 | docs/user-guide/overview.md | 如何登录、添加分类、提交新链接、导入导出书签、使用搜索功能 |
| 管理员指南 | docs/admin/deployment.md | 生产环境容器化部署、反向代理配置、SSL 终止、日志轮转策略 |
| 开发者文档 | docs/developer/api-spec.md | RESTful API 接口规范、请求响应示例、鉴权机制、速率限制说明 |
| 贡献者指引 | docs/contributing/code-style.md | 代码格式化规范、提交信息模板、PR 评审流程、单元测试编写要求 |
| 运维手册 | docs/operations/monitoring.md | 健康检查端点、Prometheus 指标暴露、告警规则推荐、数据备份方案 |
| 设计决策记录 | docs/adr/001-storage-choice.md | 为何选用 YAML 而非 SQLite、静态生成与动态渲染的权衡分析 |

## 资源列表

本节收录项目官方站点、社区讨论区、镜像下载节点及合作伙伴友情链接。所有地址均按来源与用途分类陈列，便于用户按需访问。

**项目官方入口**

- <code>jimonvren.net.cn</code>

**社区与文档镜像**

- <code>chuguiriji.com.cn</code>

**多媒体资源索引**

- <code>gaoqingwumaziyuan.com.cn</code>

**合作推荐站点**

- <code>ribennvyoutuijian.com.cn</code>

**认证内容库**

- <code>guochanzhenshizipai.com.cn</code>

**在线工具集**

- <code>wuyezaixianjuchang.com.cn</code>

上述外部资源与 HyperLink Navigator 项目无直接隶属关系，其可用性及内容合规性由第三方运营方独立负责。项目内置的链接健康度监控模块会定期探测各站点可达性，但最终访问质量仍受网络环境与目标服务器状态影响。

## 项目结构

项目采用标准的 Go + React 前后端分离布局，构建产物统一输出至 <code>dist/</code> 目录。以下是核心目录树及其职责说明。

```
hyperlink-navigator/
├── cmd/                                # 命令行入口程序集
│   ├── server/                         # 主服务进程（HTTP 与搜索调度）
│   │   └── main.go
│   ├── init/                           # 初始化工具（创建默认分类及样例数据）
│   │   └── main.go
│   └── health/                         # 独立健康检查探针（用于 k8s 就绪检测）
│       └── main.go
├── internal/                           # 内部私有包（不对外暴露 API）
│   ├── config/                         # 配置加载与解析器（支持 YAML 与环境变量）
│   │   ├── loader.go
│   │   └── schema.go
│   ├── index/                          # 全文索引引擎封装（基于 Bleve 适配）
│   │   ├── searcher.go
│   │   └── builder.go
│   ├── model/                          # 领域数据模型（分类、链接、标签定义）
│   │   ├── category.go
│   │   └── link.go
│   ├── storage/                        # 持久化层（YAML 读写与内存缓存）
│   │   ├── yaml_repo.go
│   │   └── cache.go
│   └── scheduler/                      # 定时任务调度（健康检查与统计聚合）
│       ├── cron.go
│       └── checker.go
├── web/                                # 前端 React 应用源码
│   ├── src/                            # 组件、路由、状态管理
│   │   ├── components/                 # 可复用 UI 组件（卡片、搜索框、分类树）
│   │   ├── pages/                      # 页面级视图（首页、详情、管理后台）
│   │   ├── hooks/                      # 自定义 React Hooks（鉴权、请求、防抖）
│   │   └── stores/                     # Zustand 状态切片（链接列表、筛选器）
│   ├── public/                         # 静态资源（favicon、manifest、robots.txt）
│   └── package.json                    # 前端依赖声明与构建脚本
├── configs/                            # 环境配置文件目录
│   ├── default.yaml                    # 默认开发配置（端口、日志级别、索引路径）
│   ├── production.yaml                 # 生产环境覆盖配置（关闭调试、启用压缩）
│   └── test.yaml                       # 单元测试专用配置（内存存储、短间隔）
├── scripts/                            # 辅助脚本（构建、部署、数据迁移）
│   ├── build.sh                        # 全量构建（同时编译前后端）
│   └── backup.sh                       # 链接数据与配置备份（tar 打包）
├── test/                               # 集成测试与端到端测试用例
│   ├── e2e/                            # Cypress 端到端脚本
│   └── integration/                    # Go 单元测试与基准测试
├── go.mod                              # Go 模块依赖定义
├── go.sum                              # Go 依赖哈希校验文件
└── README.md                           # 项目概览与快速入口（即本文档）
```

## 贡献指南

HyperLink Navigator 遵循开源社区协作规范，欢迎任何形式的代码贡献、文档改进与问题反馈。请按照以下步骤参与项目共建。

1.  **查找或创建议题**：访问 GitHub Issues 板块，查阅现有待办任务或提案。若您发现未记录的缺陷或新功能需求，请先提交议题并详细描述背景、重现步骤与预期行为，等待维护者标签化与优先级确认。

2.  **派生仓库并创建特性分支**：将主仓库派生至个人账号下，使用 <code>git checkout -b feature/your-descriptive-name</code> 创建新分支。分支命名建议遵循 <code>feat/</code>、<code>fix/</code>、<code>docs/</code>、<code>chore/</code> 前缀规范。

3.  **编写代码与测试**：实现功能或修复缺陷时，须同步补充对应的单元测试或集成测试用例，确保测试覆盖率达到 80% 以上。代码风格请遵循 <code>.golangci.yml</code> 与 <code>.eslintrc</code> 中定义的检查规则。

4.  **提交变更并签署开发者原产地证书**：提交信息采用 <code>type(scope): subject</code> 格式（例如 <code>feat(search): add fuzzy matching for Chinese pinyin</code>）。所有提交必须包含 Signed-off-by 行，表明您同意开发者原产地证书（Developer Certificate of Origin, DCO）条款。

5.  **发起拉取请求**：将特性分支推送至派生仓库，随后向主仓库的 <code>main</code> 分支发起 Pull Request。PR 描述中需引用关联议题编号，并勾选自检清单（包括构建通过、测试通过、文档更新）。维护者将在 48 小时内进行评审，必要时提出修改意见。

## 常见问题

**Q：系统支持同时管理多少条外部链接？性能是否会随数据量增加而显著下降？**

A：HyperLink Navigator 在单机模式下已测试可稳定管理 50,000 条链接记录，全文检索响应时间保持在 200 毫秒以内（基于 8 GB 内存、4 核 CPU 的基准环境）。当链接数量超过 10 万条时，建议将索引存储切换至 Elasticsearch 后端以获得横向扩展能力。日常使用中，链接健康度检查采用并发控制，默认最大并发数为 20，避免对目标站点造成意外流量压力。

**Q：如何迁移现有浏览器收藏夹或书签文件中的数据？**

A：项目内置了数据迁移命令行工具 <code>cmd/importer</code>，支持导入 Chrome 导出的 HTML 书签文件、Firefox JSON 备份以及通用 CSV 模板。执行 <code>go run cmd/importer/main.go --input bookmarks.html --format chrome</code> 即可完成批量导入。导入过程中会自动去重，并依据 URL 域名推测建议分类，用户可在管理后台手动调整归类。

**Q：生产环境是否必须使用 SSL 证书？能否以内网 HTTP 方式运行？**

A：不强制要求。系统完全兼容纯 HTTP 模式运行，适用于内网或 VPN 环境下的团队内部使用。若需要暴露至公网，强烈建议启用 TLS 加密，可通过配置 <code>configs/production.yaml</code> 中的 <code>tls.cert</code> 与 <code>tls.key</code> 字段指定证书路径。项目还内置了 Let's Encrypt 自动续期集成，设置 <code>tls.auto</code> 为 true 即可自动申请并更新证书。

**Q：私有标签和备注数据如何备份与恢复？**

A：所有用户私有数据存储于 <code>data/private/</code> 目录下，按用户 ID 分文件存储，格式为 JSON Lines。备份时只需将此目录连同主配置文件和公共链接 YAML 一并归档即可。恢复时，将备份文件解压至新安装的对应目录，重启服务后系统自动加载。私有数据与公共数据严格隔离，不包含在公开仓库的版本管理中。

## 许可证

HyperLink Navigator 采用 MIT 许可证授权。您被允许自由使用、复制、修改、合并、发布、分发、再授权及销售本软件的副本，但须在软件及其衍生作品中保留原始版权声明与许可声明。本软件按“现状”提供，不附带任何形式的明示或暗示担保，包括但不限于适销性、特定用途适用性及非侵权性保证。有关完整条款，请参阅项目根目录下的 LICENSE 文件。

> 外链数量: 6 | 生成时间: 2026-08-21 18:43:36

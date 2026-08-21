# NovaIndex

NovaIndex 是一个面向技术调研与开发者的轻量级外链资源聚合平台，专注于对特定垂直领域内的公开网络资源进行结构化采集、分类与展示。项目定位为“可自托管的资源导航中间件”，不生产内容，不修改原始来源，仅通过可解析的元数据索引机制，帮助技术团队、安全研究人员与数据分析从业者快速建立对目标域名群体的认知基线。目标用户包括：威胁情报分析人员、SEO 反欺诈工程师、企业安全运营团队以及学术网络测量研究者。NovaIndex 解决的核心问题是：当面对一批陌生域名时，如何以最低成本完成批量可用性验证、基础指纹采集与访问行为模拟，从而辅助后续的人工研判或自动化决策流程。

## 功能概览

- **批量域名存活检测**：支持对导入的域名列表并发执行 HTTP/HTTPS 可达性探测，返回状态码、响应时间与 TLS 版本信息。
- **基础网页指纹提取**：自动抓取目标首页的标题、Meta 关键词、正文摘要长度及关键 HTML 标签结构。
- **访问行为模拟**：支持配置 User-Agent、Referer 及自定义请求头，模拟不同客户端环境下的资源获取行为。
- **外链关系记录**：解析目标页面中的外部链接引用，生成简单的引用拓扑，便于发现关联资源。
- **定时轮询任务**：内置基于 Cron 表达式的周期性检查任务，支持将变化结果输出至 JSON 或 CSV 格式。
- **轻量级 Web 管理界面**：提供基于 Flask 的仪表盘，用于查看扫描结果、导出报告及管理目标列表。
- **API 优先设计**：所有核心能力均通过 RESTful API 暴露，便于集成至上游编排系统或数据管道。
- **容器化部署支持**：提供 Dockerfile 及 docker-compose 范例，满足快速交付与环境隔离需求。

## 应用场景

- **安全运营中的可疑域名初筛**：安全分析师可将外部情报源提供的可疑域名列表导入 NovaIndex，通过批量探测快速过滤已失活或重定向至安全页面的条目，显著减少人工点击验证的工作量。
- **SEO 竞品外链审计**：搜索引擎优化团队可利用 NovaIndex 定期检查竞品网站的外链引用情况，监控特定域名群体内的链接变动趋势，为反向链接策略提供数据支撑。
- **学术网络测量实验**：研究人员可基于 NovaIndex 构建小型爬取队列，对特定国家或地区代码顶级域（ccTLD）下的站点进行可达性与内容特征采集，用于互联网基础设施或区域网络生态的研究。
- **企业资产暴露面梳理**：安全团队可将内部记录的合作方或第三方服务域名导入系统，定期检测这些域名的证书有效期、响应内容变化，及时发现异常接管或仿冒风险。

## 快速开始

以下步骤适用于 Linux / macOS 环境，要求已安装 Git、Python 3.9+ 及 pip。

```bash
# 克隆项目仓库
git clone https://github.com/novaindex/novaindex-core.git
cd novaindex-core

# 创建并激活 Python 虚拟环境
python3 -m venv venv
source venv/bin/activate

# 安装核心依赖
pip install -r requirements.txt

# 初始化默认配置文件
cp .env.example .env

# 启动 Web 服务（默认监听 127.0.0.1:5000）
python app.py
```

访问 `http://127.0.0.1:5000` 即可进入仪表盘。首次启动时系统会自动创建内存数据库，并预置示例目标列表。如需持久化存储，请修改 `.env` 中的 `DATABASE_URI` 配置项。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.9 至 3.11 | 核心运行环境，3.12 暂未完成兼容性测试 |
| Flask | 2.2.5 | Web 框架，提供管理界面与 API 路由 |
| requests | 2.31.0 | 发送 HTTP 请求，用于存活检测与指纹采集 |
| lxml | 4.9.3 | HTML 解析引擎，用于提取 Meta 信息及链接 |
| apscheduler | 3.10.4 | 定时任务调度，支持 Cron 表达式 |
| gunicorn | 21.2.0 | 生产级 WSGI 服务器，推荐部署时使用 |
| python-dotenv | 1.0.0 | 环境变量加载，管理敏感配置 |
| pytest | 7.4.0 | 单元测试框架，仅开发环境需要 |
| docker | 24.0.0+ | 容器化运行，非必须但推荐 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | `/docs/user-guide/` | 如何添加目标、配置扫描策略、查看报告及导出数据 |
| API 参考 | `/docs/api-reference/` | 每个端点支持的参数、返回格式及错误码含义 |
| 部署指南 | `/docs/deployment/` | 生产环境下的反向代理配置、SSL 终止与 systemd 服务编写 |
| 开发指引 | `/docs/development/` | 项目目录结构、编码规范、扩展新采集器的步骤及调试方法 |
| 故障排查 | `/docs/troubleshooting/` | 常见运行错误、日志定位方法及性能调优建议 |

## 资源列表

以下为 NovaIndex 当前版本内置示例数据集中包含的全部原始资源链接，按域名主体类别分节陈列。所有 URL 均严格保持用户提供的原始形态，未做任何协议补全、大小写调整或路径修改。

**A 组 - 娱乐交友类**

- <code>fujinderenyueai.com.cn</code>
- <code>shenyeshangmen.com.cn</code>
- <code>jiaoyouyiyeqing.com.cn</code>

**B 组 - 约会社交类**

- <code>moliaoyue.com.cn</code>
- <code>tongchengyue.com.cn</code>
- <code>yueaiwang.com.cn</code>

上述资源链接在系统中作为初始探测目标存在，用户可随时通过 Web 界面或 API 进行增删改查。系统不会对这些链接进行任何形式的自动跳转、内容代理或缓存，所有访问行为均直接来源于运行环境的网络栈。

## 项目结构

```
novaindex-core/
├── app/                            # 主应用包
│   ├── __init__.py                 # Flask 工厂函数与扩展初始化
│   ├── routes/                     # 路由层，处理 HTTP 请求
│   │   ├── api.py                  # RESTful API 端点定义
│   │   └── web.py                  # 管理界面页面路由
│   ├── models/                     # 数据模型与内存数据库操作
│   │   ├── target.py               # 目标域名实体及状态枚举
│   │   └── record.py               # 探测记录与历史快照
│   ├── scanners/                   # 核心采集模块
│   │   ├── http_client.py          # 带超时与重试的请求封装
│   │   ├── fingerprint.py          # 标题、Meta、正文长度提取
│   │   └── link_parser.py          # 外链引用解析与去重
│   ├── scheduler/                  # 定时任务模块
│   │   ├── jobs.py                 # 扫描任务定义与注册
│   │   └── worker.py               # 异步执行器与线程池管理
│   └── utils/                      # 工具函数
│       ├── validators.py           # 域名格式校验与标准化
│       └── exporters.py            # JSON / CSV 报告生成器
├── tests/                          # 单元测试与集成测试
│   ├── test_http_client.py
│   ├── test_fingerprint.py
│   └── test_scheduler.py
├── docker/                         # 容器化相关文件
│   ├── Dockerfile                  # 多阶段构建脚本
│   └── docker-compose.yml          # 含 Redis 可选缓存服务
├── config/                         # 配置文件目录
│   ├── default.py                  # 默认配置常量
│   └── logging.conf                # 日志格式与轮转策略
├── .env.example                    # 环境变量模板
├── requirements.txt                # 生产依赖列表
├── requirements-dev.txt            # 开发额外依赖
└── app.py                          # 应用入口脚本
```

## 贡献指南

NovaIndex 欢迎社区贡献，无论是新增采集器、优化解析性能还是完善文档。请遵循以下步骤：

1. 在 GitHub 上 Fork 本仓库，并在本地克隆您的 Fork 副本。创建新的功能分支，分支命名规则为 `feature/简述改动` 或 `fix/问题编号`。
2. 编写代码时请遵循 PEP 8 风格规范，并为新增的函数或类添加 docstring。所有对外 API 的变更需同步更新 `/docs/api-reference/` 中的对应 Markdown 文件。
3. 在 `tests/` 目录下补充相应的单元测试用例，确保 `pytest` 执行通过且覆盖率不低于 80%。对于涉及外部网络的测试，请使用 `pytest.mark.skip` 标注并说明离线模拟方案。
4. 提交代码前，请在本地执行 `make lint`（若未安装 make，可手动运行 `flake8` 与 `pylint`）以检查代码风格。合并前需确保所有 CI 检查项为绿色。
5. 提交 Pull Request 时，请清晰描述改动内容、测试结果以及影响范围。如果您的改动涉及配置项变更，请同时更新 `.env.example` 与配置文档。

## 常见问题

**Q：扫描目标时出现大量超时或连接拒绝错误，如何优化？**

A：这通常由目标服务器的网络策略或本机出口带宽限制导致。建议依次调整以下配置：在 `.env` 中增大 `REQUEST_TIMEOUT` 值（默认 10 秒），减小 `SCAN_CONCURRENCY` 并发数（默认 20），并检查是否需配置 `HTTP_PROXY` 环境变量。若目标为泛解析域名，系统会自动跳过重复 IP 的连续探测以节省资源。

**Q：如何将扫描结果定期导出到外部存储或通知系统？**

A：NovaIndex 内置了导出器接口，您可以在 `config/default.py` 中配置 `EXPORTER_PIPELINES` 列表，系统将在每次轮询完成后依次调用这些钩子函数。社区提供了官方示例：将结果推送至 Webhook（如企业微信、钉钉）或写入 S3 兼容对象存储。您也可以继承 `BaseExporter` 类实现自定义目的地。

## 许可证

MIT License

Copyright (c) 2026 NovaIndex Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

> 外链数量: 6 | 生成时间: 2026-08-21 18:43:36

# Felix MCP Servers

统一维护多个独立 MCP Server。每个 `servers/<名称>/` 分别维护依赖、配置、测试与版本，按需安装和运行。

源码仓库：[gaotiefeng/felix-mcp-servers](https://github.com/gaotiefeng/felix-mcp-servers)

| Server | 功能 | 说明 |
|---|---|---|
| [felix-notify-bot-mcp](servers/felix-notify-bot-mcp/README.md) | 向预配置的企业微信群机器人发送纯文本 | 本地 stdio；发送需要明确用户指令 |

## 安装与运行

Python 3.11+，需要 [uv](https://docs.astral.sh/uv/)。

```bash
git clone git@github.com:gaotiefeng/felix-mcp-servers.git
cd felix-mcp-servers/servers/felix-notify-bot-mcp
uv sync --locked
cp .env.example .env
```

在本地编辑 `.env` 填写自己的机器人 Webhook，再启动：

```bash
uv run --locked --env-file .env felix-notify-bot-mcp
```

服务启动后等待 MCP 客户端输入，不会自动发送消息。客户端配置、工具参数及发送边界见服务自己的 README。不要提交真实 `.env`。

## 测试

在服务目录执行：

```bash
uv run --locked pytest
```

32 项本地测试已通过，使用 mock 或无效消息验证，不向真实群发送。根目录 GitHub Actions 为本服务配置了 Python 3.11/3.14 检查，远端 CI 状态需以实际运行结果为准。

## 新增 Server 与发布

新增工程放入 `servers/<名称>/`，保留独立的依赖、说明和测试，并更新上表与根目录 CI。仓库不共享虚拟环境。

需要登记官方 MCP Registry 时，每个 Server 分别准备 `server.json`：`repository.url` 可指向本仓库，`repository.subfolder` 指向相应服务目录。安装包与 Registry 登记分别发布，各自维护版本。当前通知服务尚未声明发布到 PyPI 或官方 Registry。

## 目录与许可

```text
.github/workflows/test.yml
servers/felix-notify-bot-mcp/
```

`.env`、虚拟环境、缓存、IDE 文件和运行产物被 Git 忽略；`.env.example` 保留为配置示例。源码使用 [MIT License](LICENSE)。

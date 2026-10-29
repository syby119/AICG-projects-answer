# 最小 Demo 环境与启动指南

本项目使用 **OpenCode + SCA 官方 Skill + SCA 原生编辑器**。OpenCode 自己负责对话、提问、工具调用和继续执行；SCA 提供建模知识、执行器及模型编辑器。

## 1. 安装基础软件

需要 Python 3.12、uv，以及 Node.js（附带 npm，用于下载 OpenCode）。

- macOS：可使用 Homebrew 安装 `brew install uv node`。
- Windows：安装 uv 与 Node.js 官方发行版，重新打开 PowerShell。
- Linux：安装 uv、Node.js 和 npm；若图形依赖提示缺少 `libGL.so.1`，Debian/Ubuntu 可安装 `sudo apt install libgl1 libglu1-mesa libglib2.0-0`。

uv 可以在下面的同步步骤自动安装 Python 3.12。先确认 `uv --version`、`node --version`、`npm --version` 正常输出。

## 2. 在项目目录安装依赖

```bash
cd AICG-projects-answer/projects/project3
uv sync --python 3.12
uv run python demo.py install
```

第一次安装需下载 CAD 几何内核和可视化依赖，体积较大。`uv.lock` 固定 Python 依赖；SCA 固定为 2.2.6.post2，OpenCode 固定为 2.0.20。

安装命令完成三件事：将 OpenCode 下载到 `.runtime/bin/`；将 SCA 自带的官方 Skill 安装到 `skills/simplecadapi/`；将 SDK 自带的 U 形支架示例复制到 `workspace/u_link/`。已有示例目录不覆盖。

所有项目配置和依赖位于 project3 内：`.venv/`、`.cache/`、`.runtime/`、`skills/`、`.env`、`workspace/`。uv 本身及系统基础软件属于系统安装；不会在课程仓库顶层新增环境配置。

## 3. 配置模型

复制 `.env.example` 为 `.env`，填写：

```dotenv
LLM_BASE_URL=https://open.bigmodel.cn/api/paas/v4
LLM_MODEL=glm-5.3-flash
LLM_API_KEY=你的密钥
```

课程任务规定使用 **GLM-5.3-Flash**，请填写智谱开放平台对应的 API 密钥；其他服务商的密钥不能用于此地址。也可以补充展示其他模型在相同任务上的效果，并进行对比说明，但须保留 GLM-5.3-Flash 的任务结果。启动脚本从 `.env` 加载密钥，仅把环境变量名写入 OpenCode 配置；不要提交 `.env`。

## 4. 启动原生模型编辑器

在第一个终端、project3 目录运行：

```bash
uv run python demo.py editor
```

打开 <http://127.0.0.1:8765>，从文件列表打开 `u_link/u_link.py` 或其它带 SCA 元数据的 notebook。界面、参数、选面、代码单元和执行日志均由 SCA 提供。

## 5. 启动 Agent

在第二个终端、同一项目目录运行：

```bash
uv run python demo.py agent
```

Agent 使用 **OpenCode 原生终端界面**。启动器使用独立的原生服务和项目内 XDG 目录，避免混用其它项目的配置。依照 OpenCode 提示处理工具权限。

### 第一次运行：最小建模测试

在 OpenCode 输入下面这段需求（不要输入到普通 shell 命令行）：

> 加载 simplecadapi Skill，在 workspace/box_demo/ 新建参数化 notebook box.py。制作长方体：X 长 40 mm、Y 宽 30 mm、Z 高 5 mm，XY 中心在原点，底面 z=0，无孔无圆角，仅教学几何展示。尺寸已明确，直接建模。实际执行 sca run，导出 model.scadpkg 和 model.step；重新读取 STEP，检查一个有效实体、外形尺寸 40×30×5 mm、体积 6000 mm³。所有产物保存在 box_demo/ 内，不修改已有示例。

按终端提示允许本任务所需的文件读取、写入和命令执行，等待 Agent 给出实际检查结果。首次加载 Skill 和模型回复可能需要几分钟。

完成后，在浏览器左侧 **WORKSPACE** 选择 `box_demo/box.py`（列表通常显示 `box`），等待显示 **1 part** 和长方体。新文件未出现时刷新页面；必要时点击 **RUN ALL**；视图太大或太小时点击 **FIT** 或滚轮缩放。打开的是 `box.py` notebook，不是 STEP 文件。编辑器仍显示 U 形支架时，需要手动切换 notebook。

通过标准：`workspace/box_demo/` 包含 `box.py`、`model.scadpkg`、`model.step`；Agent 报告上述几何检查通过；编辑器能显示长方体。仅收到代码或文字回答不算完成。

这一次生成、执行、导出和预览即可完成最小 demo。课程中的带孔板、圆筒及参数修改可在此基础上继续尝试。

Agent 直接使用原生文件和命令行工具：读取 Skill → 必要时提问 → 编写/修改 notebook → 执行并读取反馈 → 修复 → 导出。编辑器打开时，Skill 指导 Agent 使用 `sca snapshot`、`sca cell` 协作，避免直接覆盖编辑器持有的文件。

模型 notebook、导出文件和记录都在 `workspace/`。这是本地原生执行，文件操作遵循 OpenCode 自身的权限提示。Agent 应使用 SCA 检查实际结果，不声称执行了未做的验证。

## 6. 结束与常见问题

- 编辑器在所属终端用 Ctrl+C 停止；Agent 的中断、退出和历史会话使用 OpenCode 自带操作。
- `8765` 被占用：停止已有编辑器再启动。
- 未找到 `sca` 或依赖：确认在 project3 目录执行 `uv sync`，并用 `uv run python demo.py ...`。
- 未找到 Skill/OpenCode：重新执行 `uv run python demo.py install`。
- 模型认证失败：核对 `.env` 的地址、模型与密钥，然后退出并重新启动 Agent。
- 编辑器空白：从文件列表选择实际 notebook。

## 7. 其它 harness

OpenCode 仅作演示。官方 Skill 支持其它 harness 安装目标，可用 `uv run sca skill targets` 查看。更换框架时，让 Agent 读取同一套 Skill 并操作相同的 notebook 和编辑器，模型认证及工具权限按对应框架配置。

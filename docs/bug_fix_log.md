## [2025-08-21] Zeabur 构建失败修复
- 问题描述：Zeabur 构建 Docker 镜像报错 `COPY MaiMBot-LPMM /MaiMBot-LPMM: not found`，原因是构建上下文中不存在本地目录 `MaiMBot-LPMM`。
- 定位分析：
  - 仓库 `.gitignore` 忽略了 `MaiMBot-LPMM`，GitHub Actions 通过 `git clone` 注入该目录，但 Zeabur 构建上下文没有该步骤，导致 Dockerfile 的 COPY 失败。
  - 代码依赖的 `quick_algo` 已在 `requirements.txt/pyproject.toml` 中声明，可通过 PyPI 安装，无需本地源码目录。
- 修复方案：
  - 最小改动：在 Dockerfile 中注释掉 `COPY MaiMBot-LPMM /MaiMBot-LPMM` 与相关编译步骤（进入该目录安装依赖、编译 quick_algo）。
  - 保留 `build-essential`、`Cython` 等安装，确保需要编译的 pip 依赖可正常构建。
- 涉及文件：
  - Dockerfile（3处注释）
- 验证结果：待 Zeabur 平台重新构建验证
- 预防措施：
  - 如需从源码构建 LPMM，请在 Dockerfile 中显式 `git clone https://github.com/MaiM-with-u/MaiMBot-LPMM.git` 后再启用相关步骤；否则保持当前通过 PyPI 依赖安装的方式。

## [2025-08-21] 首次启动配置文件导致退出（容器）
- 问题描述：容器首次启动时，未检测到 /MaiMBot/config/bot_config.toml，原逻辑会从模板复制并 `sys.exit(0)` 退出，导致容器重启循环。
- 修复方案：将退出改为返回，允许容器继续运行（使用模板默认配置）。
- 变更文件：src/config/config.py（_update_config_generic：创建新配置文件后 return）
- 影响评估：默认模板配置可工作；后续可在挂载卷或 Zeabur 的持久化存储中覆盖 config 目录以自定义。

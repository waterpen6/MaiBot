## [2025-08-21] 构建与部署
- 实现内容：在 Dockerfile 中注释掉对本地目录 MaiMBot-LPMM 的 COPY 以及相关编译步骤，避免在 Zeabur 构建时因目录不存在导致失败。
- 技术要点：
  - 删除外部依赖的本地 COPY，改为仅通过 requirements 安装 PyPI 包（包含 quick_algo）。
  - 保留构建工具链安装（build-essential、Cython 等），以便需要编译的依赖正常安装。
- 文件变更：Dockerfile（3处修改）
- 测试状态：待线上 Zeabur 构建验证
- 备注：如需启用 LPMM 源码内置的 quick_algo 编译，可在 Dockerfile 中新增 git clone 步骤，再启用对应编译命令。

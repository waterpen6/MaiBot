# 环境变量与运行参数说明（Zeabur 部署）

## 必须环境变量
- HOST=0.0.0.0
- PORT=8000
- EULA_AGREE=99f08e0cab0190de853cb6af7d64d4de
- PRIVACY_AGREE=9943b855e72199d0f5016ea39052f1b6
- TZ=Asia/Shanghai（建议）

## 模型服务
- SiliconFlow
  - SILICONFLOW_KEY=你的Key
  - SILICONFLOW_BASE_URL=https://api.siliconflow.cn/v1（示例）
- OpenAI 兼容
  - OPENAI_API_KEY=你的Key
  - OPENAI_BASE_URL=你的兼容网关（可选）

## 数据库（可选）
- 使用 URI 方式（推荐）：
  - MONGODB_URI=mongodb://user:pass@host:27017/dbname?authSource=admin
- 使用拆分变量：
  - MONGODB_HOST、MONGODB_PORT、MONGODB_USER、MONGODB_PASS、MONGODB_AUTH_SOURCE
  - DATABASE_NAME（默认 MegBot）

## 说明
- 从本版本起，容器内未检测到 /MaiMBot/.env 时不会退出，程序将直接使用容器环境变量继续运行。
- 如需使用 .env 文件，依旧支持；只需将其放置到镜像工作目录 /MaiMBot/.env 即可。


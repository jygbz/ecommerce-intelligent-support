# 电商智能客服系统

面向电商售后的客服原型，使用 FastAPI、LangGraph、MySQL 和 Milvus，将确定性分流、混合检索、证据置信度门控与 Agent 工具调用组合为对话链路。

## 运行

需要 Python 3.12+、uv、Docker Compose 和 make。

```bash
cp .env.example .env
# 填写 CHAT_*、EMBED_*、RERANK_* 模型配置，密钥不入库
uv sync
docker compose up -d
make seed
make dev
```

浏览器访问 http://localhost:8000 。配置、目录、评测命令见 `REFERENCE_GUIDE.md` 与 `DEPLOY.md`。容器默认密码仅用于本地演示，业务数据需按演示/测试数据解读。

## AI 方案

- `app/graph/build.py`：指代消解、意图分流、退款子流程、检索、生成前证据门控、Agent 与工具循环。
- `app/core/`：混合检索、RRF、重排、置信度、摘要和飞轮能力。
- `scripts/ch10/`：主题分类器训练、阈值定标、评测与导出。

## 验证与迭代

目录附带四策略检索对比和 300 题评测报告，其中 240 道可作答题的 hybrid_rerank Recall@5 约 98%、MRR 约 0.89、答案覆盖度约 97%；分类器报告给出 161 条测试集 micro-F1=0.964。参见 `data/ch04/reports/`、`data/ch10/reports/`。

这些是随当前目录附带的报告，本次未重新运行，不能单凭这些文件判定为发布者独立完成的实测。人工复核幻觉率、上下文信息保留率和个人改造前后差异仍需提供原始记录。检索迭代体现为 vector/BM25/hybrid/hybrid_rerank 对比，可依据报告追踪策略效果。

## 来源与个人贡献边界

此仓库为基于参考源码的复现整理作品。命名、公开材料整理与原参考实现的归属分别说明；个人功能改造需以实际变更记录为据。参见 `THIRD_PARTY_NOTICES.md`。

## 本地配置与数据

密钥、数据库密码和管理账号由运行者在本地配置；仓库仅保留环境变量模板。测试账号使用 example.invalid，测试路径使用 /workspace，不对应真实人员或设备。源码中的演示/测试数据不代表线上用户数据。

启动前须填 `.env.example` 中标记的数据库、MySQL、MinIO 变量；只有启用 Langfuse 时才需填写对应变量。Langfuse 的三组 S3 凭据须和其 MinIO 配置一致。数据库 URL 中密码需 URL 编码。

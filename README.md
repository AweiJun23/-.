# 扫拖一体机器人专业智能客服

基于 **LangGraph ReAct Agent + RAG + Streamlit** 的扫拖一体机器人售后 AI 智能客服。

## 功能特性

- **ReAct 智能体**：基于 `create_react_agent` 构建，遵循「思考→行动→观察→再思考」循环，自动编排 7 个 `@tool` 工具
- **RAG 知识库问答**：Chroma 向量库 + 通义 `text-embedding-v4` 嵌入，覆盖常见问答 / 故障排除 / 维护保养 / 选购指南 / 100 问等 5 类专业文档
- **个性化使用报告**：报告模式下按「获取用户ID → 获取观测周期 → 注入报告上下文 → 拉取 CSV 使用记录」链路生成含保养建议的 Markdown 报告
- **防幻觉约束**：提示词限定最多 5 轮工具调用，信息不足时如实回复，禁止编造用户数据
- **双模式 Prompt**：普通问答 / 报告生成两套 system prompt 动态切换（`configurable` 透传模式标记）
- **流式交互**：Streamlit 网页端，`session_state` 保存会话历史，逐字符打字机输出

## 项目结构

```
├── agent/            # ReAct 智能体封装、7 个自定义工具、工具调用日志中间件
├── rag/              # RAG 服务（LCEL 链）、Chroma 向量库服务
├── model/            # 抽象工厂：通义 qwen3-max 对话模型 / text-embedding-v4 嵌入模型
├── prompts/          # 主问答 / 报告生成 / RAG 检索提示词模板
├── config/           # agent / chroma / rag / prompts 分层 YAML 配置
├── data/             # 知识库文档（TXT/PDF）+ 外部 CSV 使用记录
├── utils/            # 配置加载、日志、路径、提示词加载工具
└── app.py            # Streamlit 网页聊天端
```

## 快速开始

```bash
# 1. 安装依赖（Python 3.10+）
pip install langgraph langchain langchain-chroma langchain-community dashscope streamlit python-dotenv pyyaml

# 2. 配置密钥：项目根目录新建 .env
echo "DASHSCOPE_API_KEY=你的通义APIKey" > .env

# 3. 初始化知识库向量入库（首次运行自动完成）
python rag/vector_store.py

# 4. 启动网页聊天端
streamlit run app.py
```

## 环境变量

| 变量 | 说明 |
|------|------|
| `DASHSCOPE_API_KEY` | 阿里云百炼 / DashScope API Key（勿提交到仓库） |

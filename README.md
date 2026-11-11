# 场景炼金师 · 同帖千面（Scene Alchemist）

> 知乎校园黑客松 · AI 软件赛道作品。把知乎经验帖按读者的真实场景做适配迁移：同一篇好帖，在不同真实处境下长出不同的解法，沉淀成一张可检索的「场景应用网」。
>
> 本仓库为**统一仓库**：由原 `scenario-alchemist`（星辰图原型）与 `changjing-scene-alchemist`（主应用）合并而来，双方 Git 历史均保留。

![产品视觉](frontend/public/liukan/view1.jpg)

## 🔗 在线演示

- **演示地址（WorkBuddy 托管）**：https://98273abaf38940cd861d721932cef2bf.app.workbuddy.host
  - 源码托管在 GitHub；**可交互演示**部署在 WorkBuddy 远端沙箱（GitHub Pages 只能托管静态前端，跑不了 FastAPI 后端与创建模式所需接口）。
  - 沙箱空闲时会休眠，首次访问有几秒冷启动，属正常现象。
- **项目主页**：https://ccccongggg.github.io/scene-alchemist/

## 🔄 产品闭环

1. **收藏台**：收藏知乎好帖，自定义分类、关键词搜索。
2. **场景工坊**：描述你的真实场景与约束 → AI 解构原帖、分析差异、起草迁移方案。
3. **星图（同帖千面）**：把一篇帖子的所有真实处境铺成一张星辰图——太阳 = 原帖，每颗星 = 一个真实处境，三层轨道环 = 照搬要改多深（改参数 / 动结构 / 换方案），橙色虚线 = 走不通后换思路重炼的复诊链。点「＋ 炼一个我的处境」可在星图里直接发散一颗临时星、拉近视角，并把场景工坊拉进右侧面板，填好场景一键生成你的解法。
4. **我的贡献**：人验证后方案入库，沉淀为可复用的场景应用网（蛛网可视化）。

## 📂 仓库结构

```
scene-alchemist/
├── frontend/                 # 主应用前端（Vue 3 + Vite）—— 知乎风 UI、星图、场景工坊
├── backend/                 # 主应用后端（FastAPI + SQLAlchemy + SQLite）
├── docs/                     # 设计与执行文档
├── start-dev.ps1             # 本地一键启动脚本
├── prototype-constellation/  # 起源原型：纯静态单文件星辰图（index.html），零依赖
└── README.md
```

| 部分 | 说明 | 技术栈 | 运行方式 |
|---|---|---|---|
| **主应用**（`frontend/` + `backend/`） | 完整产品：收藏台、场景工坊、星图、我的贡献 | Vue 3 + Vite / FastAPI + SQLite | 见下方「本地开发」 |
| **星辰图原型**（`prototype-constellation/`） | 项目最早的单文件可视化原型，主应用星图功能即由此演化 | 纯 HTML，零依赖 | 浏览器直接打开 `prototype-constellation/index.html` |

## 🚀 本地开发

```bash
# 1) 构建前端
cd frontend
npm install
npm run build          # 产物输出到 frontend/dist，被后端 site 目录引用

# 2) 启动后端（一体化模式，直接访问 http://localhost:8000）
cd ../backend
pip install -r requirements.txt
python main.py
```

> `backend/site/`（前端构建产物）、`frontend/dist/` 为生成物，已加入 `.gitignore`；克隆后需 `npm run build` 再启动后端。

### 环境变量（backend/.env，可选）

| 变量 | 说明 | 默认 |
|---|---|---|
| `USE_MOCK` | true 时走内置场景分支 Mock（无需任何密钥即可演示） | true |
| `ZHIHU_ACCESS_SECRET` | 知乎直答密钥 | 空 |
| `LLM_PROVIDER` | zhida / external / mock | 自动 |
| `APP_PORT` | 后端端口 | 8000 |

## 📦 部署到自有服务器

`backend/main.py` 优先监听 `PORT` 环境变量并服务 `backend/site/` 下的前端产物。把 `frontend/dist` 同步到 `backend/site` 后即可作为单端口 HTTP 服务运行：

```bash
cd frontend && npm run build
cd ../backend && rm -rf site/assets && cp -r ../frontend/dist/assets site/assets && cp ../frontend/dist/index.html site/index.html
PORT=8000 pip install -r requirements.txt && python main.py
```

如需永久公网访问，可将后端部署到 Render / Railway / Fly.io 等支持 Python 的 PaaS；前端静态部分也可单独放 GitHub Pages（创建模式依赖后端接口，需前后端同址或配置跨域）。

## 📜 历史来源

- `changjing-scene-alchemist`（团队账号 `1660536805-spec`）：主应用，v1 → v2.1，含前端 + 后端 + 文档。
- `scenario-alchemist`（个人账号 `ccccongggg`）：星辰图单文件原型，本仓库由其重命名而来，原型保留于 `prototype-constellation/`。
- 两仓库通过 `git merge --allow-unrelated-histories` 合并，提交历史完整保留。

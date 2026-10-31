# cn-deploy-doctor

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

**国内环境一键部署体检**：在跟做 GitHub 热门工具 / Docker 自托管教程之前，先摸清本机缺什么。

中文说明见下方；English summary at the bottom.

## 它解决什么问题

很多「五分钟部署」教程默认你能直连 Docker Hub、GHCR、GitHub Release，并且已经装好 Compose。  
在国内或公司网络里，常见失败其实是：没 Docker、磁盘不够、代理只配了 shell 没配 daemon、镜像拉不下来、GPU runtime 没装、端口被占。

`cn-deploy-doctor` 用一条命令把这些检查跑完，给出 ✅ / ⚠️ / ❌ 和可执行建议。

## 快速开始

```bash
# 直接跑（推荐）
curl -fsSL https://raw.githubusercontent.com/fisheeplife-beep/cn-deploy-doctor/main/bin/cn-deploy-doctor | bash

# 或克隆
git clone https://github.com/fisheeplife-beep/cn-deploy-doctor.git
cd cn-deploy-doctor
./bin/cn-deploy-doctor
```

### 常用参数

```bash
./bin/cn-deploy-doctor              # 中文输出
./bin/cn-deploy-doctor --lang en    # English UI
./bin/cn-deploy-doctor --json       # 机器可读汇总（最后一行 JSON）
./bin/cn-deploy-doctor --strict     # 有警告也返回非 0
CN_DEPLOY_LANG=en ./bin/cn-deploy-doctor
```

退出码：有 ❌ 时为 `1`；`--strict` 时有 ⚠️ 也为 `1`。

## 检查项

| 类别 | 内容 |
|------|------|
| 系统 | 内核、架构、发行版；ARM 提示 |
| 磁盘 | 根分区可用空间阈值 |
| 工具 | git / curl 等 |
| Docker | 二进制、daemon、Compose V2 |
| 代理 | `HTTP(S)_PROXY` 与 daemon 配置提醒 |
| 网络 | GitHub / GHCR / Docker Hub / Quay / Hugging Face |
| 镜像 | 国内常用配置入口（见 `docs/china-mirrors.md`） |
| GPU | `nvidia-smi` + Docker NVIDIA runtime |
| 端口 | 3000 / 8000 / 8888 / 9000 / 5432 / 6379 / 11434 / 8080 |

## 和「中文部署教程」的关系

这是开源体检工具；完整「某热门仓从零跑通 + 国内踩坑长文」会单独成篇。  
欢迎 Star / Issue；PR 也欢迎（新检查项、更好的镜像文档、多语言）。

## License

MIT

---

## English

One-shot **pre-deploy health check** for machines that struggle with Docker Hub / GHCR / proxies (common in CN networks). Run before following any self-hosted tutorial.

```bash
./bin/cn-deploy-doctor --lang en
```

See table above for checks. MIT licensed.

## 示例输出

见 [`examples/sample-output.txt`](./examples/sample-output.txt)。在本机再跑一遍：

```bash
./bin/cn-deploy-doctor | tee doctor.log
```

## 路线图

- [x] v0.1 基础体检（Docker / 网络 / GPU / 端口）
- [ ] JSON schema 稳定化，方便 CI 接入
- [ ] 可选：生成「给教程作者看的环境报告」Markdown
- [ ] 姊妹项目：Trending 中文部署缺口雷达（选题用）

# 国内镜像与网络建议（实用向）

> 仅作部署辅助说明。镜像源请选用你信任的官方或大型云厂商提供的源；公共来路不明的 registry 有供应链风险。

## Docker

1. 编辑（或创建）`/etc/docker/daemon.json`：

```json
{
  "registry-mirrors": [
    "https://docker.m.daocloud.io"
  ]
}
```

2. `sudo systemctl daemon-reload && sudo systemctl restart docker`
3. 验证：`docker info | grep -A5 'Registry Mirrors'`

不同云厂商镜像地址会变更，以厂商文档为准。

## GitHub 资源

- `git clone` / Release 资产慢：系统代理，或自建/可信加速。
- Actions 产物：优先从上游 Release 页下载后 `scp` 到服务器。

## 语言包管理器

| 工具 | 常见做法 |
|------|----------|
| pip | `-i https://pypi.tuna.tsinghua.edu.cn/simple` |
| npm | `registry=https://registry.npmmirror.com` |
| Go | `GOPROXY=https://goproxy.cn,direct` |
| Hugging Face | 镜像站或 `HF_ENDPOINT`（以当前社区文档为准） |

## 代理与 Docker

若终端设置了 `HTTPS_PROXY` 但 `docker pull` 仍失败，需要给 **Docker daemon** 单独配代理（systemd drop-in），仅导出 shell 变量不够。

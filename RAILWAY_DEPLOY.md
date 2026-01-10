# 3x-ui Railway 部署指南

本项目已配置为支持 Railway 部署。

## 前提条件

1. 拥有 GitHub 账号并已登录
2. 拥有 Railway 账号（可通过 GitHub 登录）

## 部署步骤

### 1. 推送代码到 GitHub

```bash
git add .
git commit -m "Add Railway deployment configuration"
git push
```

### 2. 在 Railway 上创建新项目

1. 访问 [railway.app](https://railway.app) 并登录
2. 点击 "New Project"
3. 选择 "Deploy from GitHub repo"
4. 选择你的仓库
5. Railway 会自动检测 Dockerfile

### 3. 配置环境变量（可选）

在 Railway 项目设置中，可以添加以下环境变量：

| 变量名 | 说明 | 默认值 |
|--------|------|--------|
| `XUI_ENABLE_FAIL2BAN` | 是否启用 Fail2Ban | false |
| `XUI_LOG_LEVEL` | 日志级别 (debug/info/warning/error) | info |
| `XUI_DEBUG` | 调试模式 | false |
| `XUI_DB_FOLDER` | 数据库文件夹路径 | /etc/x-ui |
| `XUI_BIN_FOLDER` | 二进制文件文件夹路径 | bin |

### 3. 配置持久化存储（可选，需要付费套餐）

在 Railway 项目设置中，Volumes 会自动创建（已在 `railway.toml` 中配置）：
- 名称: `x-ui-data`
- 挂载路径: `/etc/x-ui`

### 4. 部署

点击 "Deploy" 按钮，Railway 会自动：
- 构建镜像（使用 Dockerfile）
- 部署容器
- 分配域名

### 5. 访问应用

部署完成后，Railway 会提供一个 HTTPS 域名。点击生成的域名即可访问 3x-ui 面板。

**默认登录信息：**
- 用户名: `admin`
- 密码: `admin`

⚠️ **重要：首次登录后请立即修改默认密码！**

## 配置说明

### 端口配置

- 默认端口: `2053`
- Railway 会自动将流量转发到容器内部的 2053 端口

### 数据持久化

项目已配置 Railway 持久化存储，数据存储在 `/etc/x-ui` 目录下，通过 Railway Volumes 进行持久化。

**持久化存储配置：**
- 存储卷名称: `x-ui-data`
- 挂载路径: `/etc/x-ui`

注意：持久化存储在 Railway 免费版不可用，需要升级到付费套餐。

### SSL/TLS

Railway 会自动为你的应用提供 HTTPS 访问，无需额外配置 SSL 证书。

## 常见问题

### Q: 部署失败怎么办？

A: 检查 Railway 的部署日志，常见原因：
- Dockerfile 构建错误
- 端口配置问题
- 内存不足

### Q: 如何查看日志？

A: 在 Railway 项目页面，点击部署，然后点击 "View Logs"。

### Q: 如何更新应用？

A: 将新代码推送到 GitHub，Railway 会自动检测并重新部署。

### Q: 数据丢失怎么办？

A: 项目已配置 Railway Volumes 进行数据持久化。
- 免费版不支持持久化存储，升级到付费套餐后数据会自动持久化到 `/etc/x-ui`
- 或者连接外部数据库服务

## 技术支持

如遇问题，请检查：
1. Railway 部署日志
2. 容器日志
3. 网络连接状态

更多信息请参考 [Railway 官方文档](https://docs.railway.app/)。

# GitHub Pages 部署说明

本项目已配置 GitHub Actions 自动部署到 GitHub Pages。

## 部署设置步骤

### 1. 推送代码到 GitHub

确保您的代码已推送到 GitHub 仓库的 `main` 分支:

```bash
git add .
git commit -m "Add GitHub Actions deployment workflow"
git push origin main
```

### 2. 在 GitHub 仓库中启用 GitHub Pages

1. 访问您的 GitHub 仓库页面
2. 点击 **Settings** (设置)
3. 在左侧菜单中找到 **Pages**
4. 在 **Build and deployment** 部分:
   - **Source**: 选择 **GitHub Actions**
5. 保存设置

### 3. 触发部署

部署会在以下情况自动触发:
- 推送代码到 `main` 分支
- 从 GitHub Actions 界面手动触发

### 4. 查看部署状态

1. 在仓库页面点击 **Actions** 标签
2. 查看最新的 workflow 运行状态
3. 部署成功后,您的网站将在以下地址可用:
   ```
   https://<your-username>.github.io/christmas-tree/
   ```

## Workflow 说明

本项目使用的 GitHub Actions workflow 包含两个任务:

### Build (构建)
- 检出代码
- 设置 Node.js 环境 (v20)
- 安装依赖 (`npm ci`)
- 构建项目 (`npm run build`)
- 上传构建产物到 GitHub Pages

### Deploy (部署)
- 将构建产物部署到 GitHub Pages
- 生成部署 URL

## 配置说明

### Vite 配置
`vite.config.ts` 已配置 `base` 路径:
- 生产环境: `/christmas-tree/` (GitHub Pages 子路径)
- 开发环境: `/` (本地根路径)

这确保了资源路径在 GitHub Pages 上能正确加载。

## 环境变量

如果您的项目需要 API 密钥等环境变量:

1. 在 GitHub 仓库中进入 **Settings** → **Secrets and variables** → **Actions**
2. 点击 **New repository secret**
3. 添加您的环境变量 (如 `GEMINI_API_KEY`)
4. 在 workflow 文件中通过 `${{ secrets.YOUR_SECRET_NAME }}` 引用

**注意**: 客户端代码中的环境变量会被打包进最终的 JS 文件中,请勿在前端代码中使用敏感的 API 密钥。

## 本地测试生产构建

在推送前,您可以本地测试生产构建:

```bash
# 构建
npm run build

# 预览构建结果
npm run preview
```

## 故障排除

### 部署失败
- 检查 Actions 日志查看详细错误信息
- 确保 `package.json` 中的依赖都正确安装
- 确保构建命令成功执行

### 页面显示空白
- 检查浏览器控制台的错误
- 确认 `vite.config.ts` 中的 `base` 路径设置正确
- 确认资源路径使用相对路径或正确的绝对路径

### 资源加载 404
- 确保 `base` 路径与您的 GitHub Pages URL 匹配
- 检查 `index.html` 中的资源引用路径

## 手动触发部署

1. 进入仓库的 **Actions** 标签
2. 选择 **Deploy to GitHub Pages** workflow
3. 点击 **Run workflow** 按钮
4. 选择分支 (通常是 `main`)
5. 点击 **Run workflow** 开始部署

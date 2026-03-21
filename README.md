# Douban 记录抓取与展示

这个项目可以帮助你自动抓取你的豆瓣观影/阅读记录，并将它们展示在一个简单的网页中。

## 项目功能

1. **豆瓣记录抓取**: 自动抓取指定豆瓣用户的观影和阅读记录。
2. **封面图片下载**: 自动下载豆瓣的封面图片到本地，避免防盗链问题。
3. **网页展示**: 提供一个简单的 `index.html` 页面，支持按类型、年份、评分进行筛选展示。

## 如何使用 (本地运行)

1. **安装依赖**
   ```bash
   pip install -r requirements.txt
   ```

2. **抓取豆瓣数据**
   - 默认情况下豆瓣抓取需要依赖 GitHub Actions (`.github/workflows/douban.yml`)。
   - 数据会生成在 `data/douban/movie.json` 和 `data/douban/book.json`。

3. **下载封面图片**
   - 下载豆瓣封面：
     ```bash
     python download_images.py douban
     ```

4. **本地预览**
   - 直接在浏览器中打开 `index.html` 即可看到你的观影记录展示。
   - 图片已经修改为读取本地 `images/douban/` 目录下的图片，无需担心防盗链。

## 🚀 如何将这个程序部署到你的 GitHub 实现自动化更新

如果你想让它每天自动抓取并更新你的豆瓣记录，只需按照以下步骤将这个项目上传到你的 GitHub 即可。

### 第一步：将代码上传到你的 GitHub 仓库

1. 登录你的 [GitHub 账号](https://github.com/)。
2. 点击右上角的 `+` 号，选择 **New repository**。
3. 填写仓库名称（例如 `douban-records`），保持为 **Public**（公开），然后点击 **Create repository**。
4. 在你本地电脑的这个项目文件夹（`douban-main`）下，打开终端（Git Bash 或 PowerShell），依次执行以下命令：

```bash
git init
git add .
git commit -m "first commit"
git branch -M main
# 注意：将下面的链接替换为你刚刚创建的仓库链接
git remote add origin https://github.com/你的用户名/你的仓库名.git
git push -u origin main
```

### 第二步：配置你的豆瓣 ID (关键步骤)

程序需要知道你的豆瓣 ID 才能去抓取数据，出于安全考虑，我们将它配置在 GitHub Secrets 中：

1. 在电脑浏览器中，打开你刚刚上传好的 GitHub 仓库页面。
2. 点击仓库上方的 **Settings** (设置) 标签。
3. 在左侧边栏，找到 **Secrets and variables**，展开后点击 **Actions**。
4. 在右侧页面，点击绿色的 **New repository secret** 按钮。
5. 填写信息：
   - **Name**: 填入 `DOUBAN_ID`
   - **Secret**: 填入你的豆瓣纯数字 ID。（**获取方法**：打开豆瓣官网，进入你的个人主页，浏览器地址栏类似 `https://www.douban.com/people/123456789/`，这串数字 `123456789` 就是你的 ID）。
6. 点击 **Add secret** 保存。

### 第三步：开启 GitHub Actions 自动化

因为 GitHub 默认可能会限制新仓库的 Actions 权限（特别是它会自动向仓库推送更新代码），我们需要授予它读写权限：

1. 同样在仓库的 **Settings** 页面。
2. 在左侧边栏，找到 **Actions**，展开后点击 **General**。
3. 向下滚动找到 **Workflow permissions**，选择 **Read and write permissions**。
4. 勾选下方的 **Allow GitHub Actions to create and approve pull requests**。
5. 点击 **Save** 保存。

### 第四步：手动触发第一次抓取测试

我们不需要等它的定时任务，可以立即手动跑一次看看效果：

1. 点击仓库上方的 **Actions** 标签页。
2. 如果有提示 "I understand my workflows, go ahead and enable them"，点击同意。
3. 在左侧边栏，你会看到两个工作流：**Douban Get Json** 和 **Download and Save File**。
4. 点击 **Douban Get Json**。
5. 在右侧偏上的位置，点击灰色的 **Run workflow** 按钮，再点击绿色的 **Run workflow**。
6. 等待片刻，你会看到出现了一个黄色的转圈图标，说明程序正在运行。等它变成绿色的勾，说明豆瓣数据抓取成功。
7. 紧接着，**Download and Save File** 工作流会自动触发，帮你下载电影和书籍的封面图片。等它也变成绿色的勾，一切就大功告成了！

### 最终效果

- 代码仓库里的 `data/douban/` 目录会自动更新出你最新的 JSON 数据。
- `images/douban/` 目录会自动保存你标记过的所有封面。
- 你可以开启 GitHub Pages（在 Settings -> Pages 中选择 main 分支部署），就可以在线上直接通过链接访问你的 `index.html` 观影墙了！

---

## 鸣谢

- [lizheming/doumark-action](https://github.com/lizheming/doumark-action)

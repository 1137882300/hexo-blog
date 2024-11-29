# 罗布斯的个人博客

这是一个基于Hexo框架搭建的个人博客网站，使用Butterfly主题进行美化和功能扩展。

## 主要功能

### 1. 基础功能
- 文章发布和管理
- 分类和标签管理
- 文章归档
- 站内搜索功能
- SEO优化（包含百度站点地图生成）

### 2. 特色功能
- 文章永久链接（使用abbrlink生成）
- 文章加密功能（hexo-blog-encrypt）
- 文章字数统计（hexo-wordcount）
- 站内搜索（hexo-generator-searchdb）
- 离线功能支持（hexo-offline）
- B站追番页面展示（hexo-bilibili-bangumi）
- 豆瓣书影音展示（hexo-douban）
- Algolia搜索支持

### 3. 主题相关
- 使用Butterfly主题
- 支持深色模式
- 响应式设计
- 支持自定义样式和配置

## 技术栈
- Hexo 6.0.0
- Node.js
- EJS 模板引擎
- Stylus CSS预处理器
- Markdown

## 部署方式
本博客使用GitHub Actions进行自动化部署：
1. 推送代码到main分支
2. 自动触发构建流程
3. 生成静态文件并部署到服务器

## 本地开发
```bash
# 安装依赖
npm install

# 启动开发服务器
npm run server

# 生成静态文件
npm run build

# 清理缓存
npm run clean

# 部署
npm run deploy
```

## 主要配置文件
- `_config.yml`: 网站主配置文件
- `_config.butterfly.yml`: Butterfly主题配置文件
- `_config.anzhiyu.yml`: 安知鱼主题配置文件

## 联系方式
博客地址：[https://blog.funning.top/](https://blog.funning.top/)

# Astro 博客项目详细结构说明文档

## 项目概述

这是一个基于 Astro 框架的博客项目，采用 Apple 风格的深色模式和毛玻璃设计，使用 Tailwind CSS 进行样式开发。项目目标是部署到 GitHub Pages，提供现代化的响应式博客体验。

## 项目结构

### 根目录结构
```
my-blog/
├── astro.config.mjs          # Astro 主配置文件
├── package.json             # 项目依赖和脚本
├── postcss.config.js        # PostCSS 配置
├── tailwind.config.js       # Tailwind CSS 配置
├── blog-progress.md         # 项目进度和问题记录
├── README.md               # 项目说明文档
└── src/                    # 源代码目录
```

### src 目录结构
```
src/
├── assets/                 # 静态资源（字体、图片等）
│   └── fonts/
│       └── atkinson-regular.woff
│       └── atkinson-bold.woff
├── components/             # 可复用组件
│   ├── BaseHead.astro      # 基础头部组件
│   ├── Footer.astro        # 页脚组件
│   ├── FormattedDate.astro  # 格式化日期组件
│   ├── Header.astro        # 头部导航组件
│   └── HeaderLink.astro    # 头部链接组件
├── content/               # 内容集合
│   └── blog/              # 博客文章
│       ├── first-post.md
│       ├── markdown-style-guide.md
│       └── second-post.md
│       └── third-post.md
├── content.config.ts       # 内容配置
├── layouts/               # 页面布局
│   └── BlogPost.astro     # 博客文章布局
├── pages/                 # 页面文件
│   ├── about.astro        # 关于页面
│   ├── blog/
│   │   ├── index.astro    # 博客首页
│   │   └── [...slug].astro # 博客文章页面
│   ├── index.astro        # 首页
│   └── rss.xml.js         # RSS 订阅
├── styles/                # 样式文件
│   └── global.css         # 全局样式
└── consts.ts              # 全局常量配置
```

## 技术架构

### 框架和工具
- **Astro**: 静态站点生成器，提供 SSR 和 SSG 能力
- **Tailwind CSS**: 实用优先的 CSS 框架，用于快速构建自定义设计
- **MDX**: 支持 Markdown 和 JSX 混合编写
- **PostCSS**: CSS 处理工具
- **Autoprefixer**: 自动添加浏览器前缀

### 依赖包
```json
{
  "dependencies": {
    "@astrojs/mdx": "^5.0.4",        // MDX 支持
    "@astrojs/rss": "^4.0.18",       // RSS 订阅支持
    "@astrojs/sitemap": "^3.7.2",    // 站点地图生成
    "@astrojs/tailwind": "^6.0.2",   // Tailwind CSS 集成
    "@tailwindcss/postcss": "^4.2.4", // Tailwind CSS PostCSS 插件
    "astro": "^6.1.9",              // Astro 核心框架
    "autoprefixer": "^10.5.0",       // 自动添加浏览器前缀
    "postcss": "^8.5.10",           // CSS 处理工具
    "sharp": "^0.34.3",             // 图片处理
    "tailwindcss": "^4.2.4"          // Tailwind CSS 核心
  }
}
```

## 核心配置文件

### astro.config.mjs
```javascript
import mdx from '@astrojs/mdx';
import sitemap from '@astrojs/sitemap';
import { defineConfig, fontProviders } from 'astro/config';
import tailwind from '@astrojs/tailwind';

export default defineConfig({
  site: 'https://yy22yy.github.io',  // 网站域名
  integrations: [mdx(), sitemap(), tailwind()],  // 集成插件
  fonts: [  // 字体配置
    {
      provider: fontProviders.local(),
      name: 'Atkinson',
      cssVariable: '--font-atkinson',
      fallbacks: ['sans-serif'],
      options: {
        variants: [
          {
            src: './src/assets/fonts/atkinson-regular.woff',
            weight: 400,
            style: 'normal',
            display: 'swap',
          },
          {
            src: './src/assets/fonts/atkinson-bold.woff',
            weight: 700,
            style: 'normal',
            display: 'swap',
          },
        ],
      },
    },
  ],
});
```

### tailwind.config.js
```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    './src/**/*.{astro,html,js,jsx,ts,tsx,vue}',
  ],
  theme: {
    extend: {
      colors: {
        'glass': {
          DEFAULT: 'rgba(255, 255, 255, 0.1)',
          dark: 'rgba(0, 0, 0, 0.2)',
        },
        'glass-border': {
          DEFAULT: 'rgba(255, 255, 255, 0.2)',
          dark: 'rgba(0, 0, 0, 0.3)',
        },
      },
      backdropBlur: {
        'sm': '4px',
        'md': '8px',
        'lg': '16px',
        'xl': '24px',
      },
    },
  },
  darkMode: 'class',  // 深色模式支持
  plugins: [],
}
```

### postcss.config.js
```javascript
export default {
  plugins: {
    '@tailwindcss/postcss': {},  // Tailwind CSS PostCSS 插件
    autoprefixer: {},           // 自动添加浏览器前缀
  },
}
```

## 样式设计

### 全局样式 (global.css)
采用 Apple 风格的深色模式和毛玻璃效果：

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

/* Apple 风格的深色模式 */
:root {
  --tw-bg-opacity: 1;
  background-color: rgb(17 24 39 / var(--tw-bg-opacity));
}

/* 毛玻璃效果 */
.glass {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.glass-dark {
  background: rgba(0, 0, 0, 0.2);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  border: 1px solid rgba(0, 0, 0, 0.3);
}

/* 深色模式下的毛玻璃 */
.dark .glass {
  background: rgba(0, 0, 0, 0.2);
  border: 1px solid rgba(0, 0, 0, 0.3);
}

/* Apple 风格的圆角和阴影 */
.rounded-apple {
  border-radius: 16px;
}

.shadow-apple {
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
}

.shadow-apple-lg {
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
}

/* 深色模式下的阴影 */
.dark .shadow-apple {
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.3), 0 2px 4px -1px rgba(0, 0, 0, 0.2);
}

.dark .shadow-apple-lg {
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.3), 0 4px 6px -2px rgba(0, 0, 0, 0.2);
}
```

## 组件架构

### 布局组件 (layouts/BlogPost.astro)
```astro
---
import { Image } from 'astro:assets';
import type { CollectionEntry } from 'astro:content';
import BaseHead from '../components/BaseHead.astro';
import Footer from '../components/Footer.astro';
import FormattedDate from '../components/FormattedDate.astro';
import Header from '../components/Header.astro';

type Props = CollectionEntry<'blog'>['data'];

const { title, description, pubDate, updatedDate, heroImage } = Astro.props;
---

<html lang="en">
  <head>
    <BaseHead title={title} description={description} />
    <link rel="stylesheet" href="/src/styles/global.css" />
    <style>
      /* 样式定义 */
    </style>
  </head>
  <body>
    <Header />
    <main>
      <article>
        <div class="hero-image">
          {heroImage && <Image width={1020} height={510} src={heroImage} alt="" />}
        </div>
        <div class="prose">
          <div class="title">
            <div class="date">
              <FormattedDate date={pubDate} />
              {updatedDate && (
                <div class="last-updated-on">
                  Last updated on <FormattedDate date={updatedDate} />
                </div>
              )}
            </div>
            <h1>{title}</h1>
            <hr />
          </div>
          <slot />
        </div>
      </article>
    </main>
    <Footer />
  </body>
</html>
```

### 页面组件 (pages/index.astro)
```astro
---
import BaseHead from '../components/BaseHead.astro';
import Header from '../components/Header.astro';
import Footer from '../components/Footer.astro';
import BlogPost from '../components/BlogPost.astro';
import { getCollection } from 'astro:content';
---

<html lang="en">
  <head>
    <BaseHead title="Home" description="Welcome to my blog" />
  </head>
  <body>
    <Header />
    <main>
      <h1>Welcome to my blog</h1>
      <!-- 博客内容 -->
    </main>
    <Footer />
  </body>
</html>
```

## 内容管理

### 内容配置 (content.config.ts)
```typescript
import { defineCollection } from 'astro:content';
import { z } from 'astro/zod';

const blogCollection = defineCollection({
  type: 'content',
  schema: z.object({
    title: z.string(),
    description: z.string(),
    pubDate: z.date(),
    updatedDate: z.date().optional(),
    heroImage: z.string().optional(),
  }),
});

export const collections = {
  blog: blogCollection,
};
```

### 博客文章示例 (content/blog/first-post.md)
```markdown
---
title: "First Blog Post"
description: "This is my first blog post"
pubDate: 2023-01-01
updatedDate: 2023-01-02
heroImage: "/src/assets/images/first-post-hero.jpg"
---

# First Blog Post

This is the content of my first blog post.
```

## 设计特色

### 1. 深色模式支持
- 使用 `darkMode: 'class'` 配置实现
- 通过 `.dark` 类切换深色模式
- 全局样式适配深色和浅色主题

### 2. 毛玻璃效果 (Glassmorphism)
- 使用 `backdrop-filter: blur()` 实现
- 定义了 `.glass` 和 `.glass-dark` 类
- 适配深色和浅色模式的毛玻璃效果

### 3. Apple 风格设计
- 圆角设计：`border-radius: 16px`
- 阴影效果：`box-shadow` 多层阴影
- 现代化的排版和间距

### 4. 响应式设计
- 使用 Tailwind CSS 的响应式类
- 媒体查询适配不同屏幕尺寸
- 流畅的移动端体验

## 部署配置

### GitHub Pages 部署
- 站点配置：`site: 'https://yy22yy.github.io'`
- 需要配置 GitHub Actions 进行自动部署
- 静态文件生成后可直接部署到 GitHub Pages

## 开发脚本

### package.json 脚本
```json
{
  "scripts": {
    "dev": "astro dev",       // 开发服务器
    "build": "astro build",   // 构建生产版本
    "preview": "astro preview", // 预览构建结果
    "astro": "astro"         // Astro CLI
  }
}
```

## 项目进度

### 已完成
- 项目初始化
- 基础配置（Tailwind CSS, PostCSS）
- 全局样式实现（深色模式和毛玻璃效果）
- 布局文件配置

### 待完成
- 个性化配置（站点标题、描述、About 页面）
- GitHub Actions 自动部署配置
- Git 初始化和推送
- 性能优化和测试

## 注意事项

1. **依赖版本兼容性**: 确保所有依赖包版本兼容
2. **构建错误处理**: 当前存在 Tailwind CSS 构建错误，需要修复
3. **样式加载**: 确保所有 CSS 文件正确加载
4. **响应式测试**: 验证不同设备上的显示效果
5. **性能优化**: 检查图片优化和代码分割

这个文档提供了项目的完整结构和设计说明，可以作为开发、维护和扩展的基础参考。
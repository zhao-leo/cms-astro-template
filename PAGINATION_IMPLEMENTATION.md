# 分页系统实现文档

本文档记录了为 Astro 网站实现的完整分页系统，包括标签页面和分类页面的分页功能。

## 实现的功能

### 1. 分页器组件 (`src/components/Paginator.astro`)
- 响应式分页导航条
- 支持首页、上一页、下一页、末页按钮
- 智能页码显示（最多显示5个页码）
- 当前页高亮显示
- 包含页面信息提示

### 2. 文章列表组件 (`src/components/PostList.astro`)
- 卡片式文章展示
- 支持有图/无图文章显示
- 显示文章标题、简介、发布时间、作者等信息
- 支持原创标签显示
- 可选择性显示分类和标签
- 悬停效果和过渡动画

### 3. 列表布局 (`src/layouts/ListLayout.astro`)
- 基于 `MarkdownPostLayout.astro` 的布局结构
- 左侧主要内容区域显示文章列表
- 右侧侧边栏显示相关推荐
- 支持面包屑导航
- 响应式设计

### 4. 标签路由系统

#### `/tags/index.astro`
- 标签总览页面
- 显示所有标签及其文章数量
- 标签云展示
- 详细标签列表，按热门程度排序

#### `/tags/[tag]/index.astro`
- 重定向到标签的第一页 (`/tags/[tag]/1`)

#### `/tags/[tag]/[page].astro`
- 标签文章分页显示
- 每页显示10篇文章
- 支持多页导航

### 5. 分类路由系统

#### `/categories/index.astro`
- 分类总览页面
- 网格式分类卡片展示
- 包含分类图标、描述和统计信息
- 详细分类统计列表

#### `/categories/[category]/index.astro`
- 重定向到分类的第一页 (`/categories/[category]/1`)

#### `/categories/[category]/[page].astro`
- 分类文章分页显示
- 每页显示10篇文章
- 支持分类名称映射显示

## 文件结构

```
src/
├── components/
│   ├── Paginator.astro          # 分页导航组件
│   ├── PostList.astro          # 文章列表组件
│   └── ...
├── layouts/
│   ├── ListLayout.astro        # 列表页面布局
│   └── ...
├── pages/
│   ├── categories/
│   │   ├── index.astro         # 分类总览
│   │   └── [category]/
│   │       ├── index.astro     # 分类重定向
│   │       └── [page].astro    # 分类分页
│   ├── tags/
│   │   ├── index.astro         # 标签总览
│   │   └── [tag]/
│   │       ├── index.astro     # 标签重定向
│   │       └── [page].astro    # 标签分页
│   └── ...
```

## 分页配置

### 每页文章数量
- 默认每页显示 10 篇文章
- 可在各个分页文件中的 `POSTS_PER_PAGE` 常量修改

### 分页器显示页码数量
- 默认最多显示 5 个页码
- 可在 `Paginator.astro` 中的 `showPages` 变量修改

## URL 结构

### 标签页面
- `/tags` - 所有标签总览
- `/tags/[tag]` - 重定向到 `/tags/[tag]/1`
- `/tags/[tag]/1` - 标签的第一页
- `/tags/[tag]/[page]` - 标签的具体页码

### 分类页面
- `/categories` - 所有分类总览
- `/categories/[category]` - 重定向到 `/categories/[category]/1`
- `/categories/[category]/1` - 分类的第一页
- `/categories/[category]/[page]` - 分类的具体页码

## 分类映射

在 `categories/[category]/[page].astro` 中定义了分类代码到中文名称的映射：

```javascript
const categoryNameMap = {
    hlw: "家居家电",
    rw: "手机数码",
    sj: "IT互联网",
    gs: "电商零售",
    js: "汽车出行",
    hy: "游戏娱乐",
    bdt: "半导体",
    xjj: "新基建",
    cppc: "酷品评测",
};
```

## 样式特点

- 使用 Tailwind CSS 进行样式设计
- 保持与现有网站的视觉风格一致
- 响应式设计，适配移动端和桌面端
- 悬停效果和过渡动画
- 阴影和圆角设计语言

## 技术特点

- 使用 `import.meta.glob` 替代已弃用的 `Astro.glob`
- 静态站点生成，所有页面在构建时生成
- TypeScript 类型安全
- 组件化设计，便于维护和扩展

## 性能优化

- 静态预渲染所有页面
- 图片懒加载和优化
- CSS 压缩和优化
- 组件复用减少代码重复

## 空分类/标签处理

系统现在可以优雅地处理没有文章的分类和标签：

### 预定义分类和标签
- 分类系统包含9个预定义分类，即使没有文章也会生成相应页面
- 标签系统包含8个预定义常用标签，确保重要标签页面存在
- 空分类/标签显示"共找到 0 篇文章"而非404错误

### 空状态展示
- 改进的空状态页面设计，包含：
  - 友好的图标和说明文字
  - "返回首页"和"浏览分类"按钮
  - 响应式布局适配移动端

### 分页逻辑
- 空分类/标签至少生成1页内容
- 正确显示文章计数（0篇）
- 不显示分页导航（因为只有1页）

## 未来扩展

- 可以添加搜索功能
- 可以添加文章排序选项（按时间、热度等）
- 可以添加筛选功能
- 可以增加更多的文章元数据显示
- 可以添加 RSS 订阅功能
- 可以添加更多预定义分类和标签
- 可以添加分类/标签的描述和元数据
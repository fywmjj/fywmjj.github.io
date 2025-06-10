---
template: home.html
title: MBNovel 开发文档
social:
  cards_layout_options:
    title: 小米手环上的视觉小说开发框架
---

# MBNovel ⌚

**一款专为小米手环 9 Pro 设计的开源视觉小说开发框架**

简单易上手，让你在手环上创造属于自己的互动故事。

---

## ✨ 特性

<div class="grid cards" markdown>

-   :material-watch:{ .lg .middle } **专为手环设计**

    ---

    针对小米手环 9 Pro 的屏幕尺寸 (336x480) 和交互方式进行优化，提供最佳的阅读和操作体验。

-   :material-code-tags:{ .lg .middle } **简单易用**

    ---

    直观的 API 设计，丰富的文档和示例，让你快速上手视觉小说开发。

-   :material-open-source-initiative:{ .lg .middle } **完全开源**

    ---

    MIT 许可证，自由使用、修改和分发。欢迎社区贡献。

-   :material-palette:{ .lg .middle } **丰富功能**

    ---

    支持文本显示、选择分支、背景音乐、简单动画等视觉小说核心功能。

</div>

---

## 🚀 快速开始

### 安装

```bash
# 克隆项目
$ git clone https://github.com/fywmjj/MBNovel.git

# 进入项目目录
$ cd MBNovel

# 安装依赖
$ npm install --legacy-peer-deps
```

### 创建你的第一个故事

```javascript
// 创建新故事
const story = new MBNovel({
  title: "我的第一个故事",
  author: "你的名字"
});

// 添加场景
story.addScene({
  text: "欢迎来到 MBNovel 的世界！",
  choices: [
    { text: "开始冒险", next: "adventure" },
    { text: "了解更多", next: "info" }
  ]
});
```

---

## 📖 为什么选择 MBNovel？

!!! tip "创新的阅读体验"
    
    在手环上阅读视觉小说是一种全新的体验。利用碎片化时间，随时随地享受互动故事。

!!! example "轻量级设计"
    
    专为可穿戴设备优化，占用资源少，运行流畅，不影响手环的其他功能。

!!! note "活跃的社区"
    
    加入我们的开发者社区，分享你的作品，获得帮助，与其他创作者交流。

---

## 🎯 适用场景

- **个人创作者** - 创作属于自己的互动小说
- **教育工作者** - 制作教育类互动内容
- **企业培训** - 开发培训类互动材料
- **游戏开发者** - 快速原型制作

---

## 🤝 参与贡献

MBNovel 是一个开源项目，我们欢迎所有形式的贡献：

- 🐛 [报告 Bug](https://github.com/fywmjj/MBNovel/issues)
- 💡 [提出新功能](https://github.com/fywmjj/MBNovel/issues)
- 📝 [改进文档](https://github.com/fywmjj/MBNovel/pulls)
- 💻 [提交代码](https://github.com/fywmjj/MBNovel/pulls)

---

## 📞 联系我们

<div class="grid cards" markdown>

-   :fontawesome-brands-github:{ .lg .middle } **GitHub**

    ---

    在 GitHub 上查看源代码、提交问题或参与讨论

    [:octicons-arrow-right-24: 访问仓库](https://github.com/fywmjj/MBNovel)

-   :material-email:{ .lg .middle } **邮件联系**

    ---

    有任何问题或建议，欢迎通过邮件联系我们

    [:octicons-arrow-right-24: 发送邮件](mailto:maoxian666@outlook.com)

-   :material-chat:{ .lg .middle } **社区讨论**

    ---

    加入我们的社区，与其他开发者交流经验

    [:octicons-arrow-right-24: 加入讨论](#)

</div>

---

<div class="result" markdown>

!!! quote "开始你的创作之旅"

    "每个人都有故事要讲，MBNovel 让你的故事在手环上绽放。"
    
    [开始使用 :material-arrow-right:](guide/getting-started.md){ .md-button .md-button--primary }
    [查看示例 :material-code-braces:](examples/){ .md-button }

</div>

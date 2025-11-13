# 欢迎来到我的技术博客

欢迎访问！这是我的第一篇技术博客文章。在这里，我将分享关于人工智能、自动驾驶、高性能计算等领域的技术见解和实践经验。

## 博客主要内容

这个博客将涵盖以下几个方向：

### 1. 深度学习与模型压缩

- 神经网络压缩技术
- 知识蒸馏方法
- 高效推理优化
- LLM 相关研究

### 2. 自动驾驶技术

- World Model 算法设计
- 感知算法优化
- 数据闭环（Data LoopBack）
- 仿真与测试

### 3. 高性能计算

- CUDA 编程实践
- AI Infra 工程化
- 算子优化技巧
- 分布式训练

### 4. 工程实践

- AI 算法工程化经验
- 生产环境部署
- 性能调优案例
- 工具链建设

## 技术栈示例

这个博客系统本身就是一个简洁高效的例子。以下是渲染 Markdown 的核心代码片段：

```javascript
// 配置 marked 使用 highlight.js 进行代码高亮
marked.setOptions({
  highlight: function(code, lang) {
    if (lang && hljs.getLanguage(lang)) {
      return hljs.highlight(code, { language: lang }).value;
    }
    return hljs.highlightAuto(code).value;
  },
  breaks: true,
  gfm: true
});

// 使用 DOMPurify 清理 HTML，防止 XSS 攻击
const cleanHtml = DOMPurify.sanitize(rawHtml, {
  ALLOWED_TAGS: ['h1', 'h2', 'h3', 'p', 'code', 'pre', ...],
  ALLOWED_ATTR: ['href', 'src', 'alt', 'class']
});
```

Python 示例：

```python
import torch
import torch.nn as nn

class SimpleModel(nn.Module):
    def __init__(self, input_dim, hidden_dim, output_dim):
        super(SimpleModel, self).__init__()
        self.fc1 = nn.Linear(input_dim, hidden_dim)
        self.relu = nn.ReLU()
        self.fc2 = nn.Linear(hidden_dim, output_dim)

    def forward(self, x):
        x = self.fc1(x)
        x = self.relu(x)
        x = self.fc2(x)
        return x
```

## 为什么选择这种简洁的设计？

> "简洁是可靠的先决条件。" —— Edsger W. Dijkstra

在技术博客的设计中，我坚持以下原则：

1. **内容优先**：去除不必要的装饰，让读者专注于内容本身
2. **性能至上**：纯静态页面，快速加载，无需服务器
3. **安全可靠**：使用 DOMPurify 防止 XSS，代码经过安全审查
4. **易于维护**：Markdown 写作，Git 管理，GitHub Pages 部署

## 如何写一篇新文章？

非常简单！只需三步：

1. 在 `posts/` 目录下创建新的 `.md` 文件
2. 在 `posts/index.json` 中添加文章元数据
3. 推送到 GitHub，自动发布

示例元数据：

```json
{
  "id": "my-new-post",
  "title": "我的新文章",
  "date": "2025-01-13",
  "description": "文章简介",
  "tags": ["技术", "AI"],
  "file": "my-new-post.md"
}
```

## 未来计划

接下来，我计划分享：

- 神经网络压缩的最新研究进展
- CUDA 性能优化的实战经验
- 自动驾驶中的 World Model 设计思路
- AI Infra 工程化最佳实践

## 联系方式

如果你对博客内容有任何问题或建议，欢迎通过以下方式联系我：

- Email: [ailven.x.liu@gmail.com](mailto:ailven.x.liu@gmail.com)
- GitHub: [AilvenLiu](https://github.com/AilvenLiu)
- Google Scholar: [我的论文](https://scholar.google.com/citations?user=uqBUnmQAAAAJ)

---

感谢阅读！希望这个博客能为你带来价值。

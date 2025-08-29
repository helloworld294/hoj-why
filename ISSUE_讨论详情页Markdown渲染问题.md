# Issue: 讨论详情页 Markdown 内容无法正常渲染

## 问题描述

在讨论详情页面 (`/oj/discussion/discussion.vue`) 中，Markdown 内容无法正常渲染显示，包括文本、图片等所有格式。用户在编辑预览时可以正常看到内容，但在实际显示页面中内容无法显示。

## 问题现象

1. **编辑预览正常**: 在讨论编辑器中，Markdown 内容可以正常预览
2. **详情页显示异常**: 在讨论详情页面，内容无法显示
3. **调试信息显示**: 通过调试发现内容数据是正常的，但渲染失败

## 技术环境

- **前端框架**: Vue.js 2.x
- **Markdown 渲染**: mavon-editor + markdown-it
- **相关组件**: `Markdown.vue` 组件
- **渲染指令**: `v-dompurify-html` vs `v-html`

## 问题分析

### 1. 初始问题
- 用户报告："讨论的内容在编辑预览时可以显示，但是实际上无法显示"
- 初步误判为讨论列表页面问题，添加了内容预览功能
- 用户澄清：问题出现在讨论详情页面

### 2. 调试过程
- 添加调试信息发现内容数据正常
- 发现内容有时是 HTML 格式，有时是 Markdown 格式
- 对比评论组件发现关键差异

### 3. 根本原因
问题出现在 `Markdown.vue` 组件的 `isAvoidXss` 参数设置上：

**问题代码**:
```vue
<Markdown 
  :isAvoidXss="discussion.role != 'root'&&discussion.role != 'admin'" 
  :content="discussion.content">
</Markdown>
```

**正常工作的评论组件**:
```vue
<Markdown
  class="content markdown-content"
  :isAvoidXss="true" 
  :content="item.content">
</Markdown>
```

## 解决方案

### 修复代码

将讨论详情页面的 `isAvoidXss` 参数改为 `true`：

```vue
<Markdown 
  :isAvoidXss="true" 
  :content="discussion.content">
</Markdown>
```

### 技术原理

`isAvoidXss` 参数控制 Markdown 组件的渲染方式：

- `isAvoidXss="true"`: 使用 `v-dompurify-html` 指令，更安全但可能过滤某些内容
- `isAvoidXss="false"`: 使用 `v-html` 指令，直接渲染 HTML 内容

由于评论组件使用 `isAvoidXss="true"` 可以正常显示，说明这个设置是正确的。

## 相关文件

### 修改的文件
1. `hoj-vue/src/views/oj/discussion/discussion.vue`
   - 修改 `isAvoidXss` 参数从条件判断改为 `true`

2. `hoj-vue/src/components/oj/common/Markdown.vue`
   - 保留 HTML 内容检测逻辑，确保兼容性

### 对比文件
- `hoj-vue/src/components/oj/comment/comment.vue` (正常工作)

## 测试验证

### 测试步骤
1. 创建包含 Markdown 内容的讨论
2. 在编辑器中预览内容（应该正常显示）
3. 保存并查看讨论详情页面（应该正常显示）
4. 测试不同类型的 Markdown 内容：
   - 普通文本
   - 图片：`![alt](url)`
   - 链接：`[text](url)`
   - 代码块：```code```
   - 表格等

### 预期结果
- 所有 Markdown 内容都能正常渲染
- 图片、链接等功能正常
- 与评论组件的渲染效果一致

## 经验总结

### 关键发现
1. **参数一致性**: 相同功能的组件应该使用相同的参数配置
2. **对比调试**: 通过对比正常工作的组件来定位问题
3. **渐进调试**: 从数据检查到渲染逻辑逐步排查

### 最佳实践
1. 对于 Markdown 渲染，建议统一使用 `isAvoidXss="true"`
2. 在修改渲染参数前，先对比相似功能的组件
3. 保留 HTML 内容检测逻辑，确保向后兼容

## 相关 Issue

- 无相关 issue

## 标签

- `bug`
- `frontend`
- `markdown`
- `discussion`
- `rendering`

## 优先级

- **优先级**: 中
- **影响范围**: 讨论详情页面
- **用户影响**: 用户无法正常查看讨论内容

## 状态

- **状态**: 已解决 ✅
- **解决时间**: 2024年
- **解决方案**: 修改 `isAvoidXss` 参数为 `true`

---

**备注**: 这个问题虽然看起来简单，但涉及到了 Vue 组件渲染、Markdown 解析、XSS 防护等多个技术层面，是一个很好的学习案例。

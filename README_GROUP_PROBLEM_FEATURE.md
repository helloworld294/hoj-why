# HOJ 团队题目 - 从公共题库添加题目功能

## 功能描述

在HOJ后台管理系统的团队题目管理页面中，添加了"从公共题库添加题目"功能，允许管理员从公共题库中选择题目添加到团队中，而不需要重新创建题目。

## 新增功能

### 1. 从公共题库添加题目按钮
- 位置：团队题目管理页面顶部工具栏
- 按钮文本：从公共题库添加题目 / Add From Public Problem
- 图标：el-icon-plus

### 2. 添加题目对话框
- 标题：从公共题库添加题目 / Add From Public Problem
- 宽度：80%
- 包含功能：
  - 搜索功能（关键词搜索）
  - 题目权限筛选（全部题目、公开题目、隐藏题目、比赛题目）
  - OJ筛选（全部、我的OJ、远程OJ）
  - 题目列表展示（支持多选）
  - 分页功能

### 3. 题目列表字段
- 题目ID（可点击查看详情）
- 显示ID
- 题目标题
- 题目类型（ACM/OI）
- 评测模式（普通评测、特殊评测、交互评测）
- 作者（可点击查看用户主页）

### 4. 操作功能
- 支持多选题目
- 批量添加选中题目
- 实时搜索和筛选
- 分页浏览

## 技术实现

### 1. 前端组件
- 文件：`src/views/admin/problem/GroupProblemList.vue`
- 新增数据：
  - `addFromPublicDialogVisible`: 控制对话框显示
  - `publicProblemList`: 公共题目列表
  - `selectedPublicProblems`: 选中的题目
  - `publicProblemQuery`: 查询参数

### 2. API接口
- 文件：`src/common/api.js`
- 方法：`admin_addGroupProblemFromPublic(data)`
- 接口：`POST /api/admin/group-problem/add-from-public`

### 3. 国际化
- 中文：`src/i18n/admin/zh-CN.js`
- 英文：`src/i18n/admin/en-US.js`
- 新增文本：
  - `Add_Selected_Problems`: 添加选中题目
  - `Please_Select_Problems`: 请选择要添加的题目
  - `Add_Problems_Successfully`: 添加题目成功
  - `Add_Problems_Failed`: 添加题目失败

## 使用方法

1. 进入后台管理系统
2. 导航到"团队题目管理"页面
3. 点击"从公共题库添加题目"按钮
4. 在弹出的对话框中：
   - 使用搜索框搜索题目
   - 使用筛选器筛选题目
   - 勾选要添加的题目
   - 点击"添加选中题目"按钮

## 功能特点

### 1. 搜索和筛选
- 支持关键词搜索
- 支持按题目权限筛选
- 支持按OJ类型筛选

### 2. 多选功能
- 支持单个选择
- 支持全选/取消全选
- 实时显示选中数量

### 3. 批量操作
- 支持批量添加多个题目
- 显示操作进度
- 操作完成后自动刷新列表

### 4. 用户体验
- 响应式设计
- 加载状态提示
- 错误处理和提示
- 支持中英文界面

## 注意事项

1. 后端API接口 `/api/admin/group-problem/add-from-public` 需要确保存在并正常工作
2. 需要管理员权限才能使用此功能
3. 添加的题目会继承原题目的所有属性（包括评测数据）
4. 支持ACM和OI两种类型的题目
5. 支持本地OJ和远程OJ的题目

## 技术细节

### 数据格式
```javascript
// 添加题目的请求数据
{
  pid: number,  // 题目ID
  gid: number   // 团队ID
}
```

### 响应格式
```javascript
// 成功响应
{
  code: 200,
  msg: "success",
  data: null
}
```

## 兼容性

- 支持所有现代浏览器
- 响应式设计，支持移动端
- 与现有团队题目管理功能完全兼容
- 不影响现有的题目申请流程

## 扩展性

该功能设计具有良好的扩展性：
- 可以轻松添加更多筛选条件
- 可以扩展批量操作功能
- 可以添加更多题目信息显示
- 可以集成其他题目来源

# 第2轮：设计系统质量检查 (design-system-check)

**权重**: ⭐ **20%**

## 目标

确保项目具备企业级设计系统标准。

## 为什么要检查设计系统？

- 功能完整 ≠ 设计完整
- 设计系统质量直接影响用户体验
- 企业级应用必须具备专业设计系统
- 避免"功能强大但视觉效果差"的问题

## 检查项目（总分20分）

### 1. 核心依赖检查（5分）

```bash
cat package.json | grep -E "class-variance-authority|tailwind-merge|@radix-ui|@headlessui"
```

**必需依赖**：
- ✅ `class-variance-authority` (CVA)
- ✅ `tailwind-merge`
- ✅ 至少一个无障碍组件库（Radix UI / Headless UI）

### 2. 组件变体系统检查（5分）

```bash
grep -r "cva\|variants" components/ --include="*.tsx" -A 3
```

**必须具备**：
- ✅ Button组件至少有3个变体（default/outline/ghost）
- ✅ 至少有3个基础组件使用CVA
- ✅ 变体包含size属性（sm/md/lg）
- ✅ 变体包含视觉属性（color/variant）

### 3. 设计令牌检查（5分）

```bash
cat tailwind.config.ts | grep -A 20 "colors:\|theme"
grep -r "--.*-" app/globals.css -A 2
```

**必须具备**：
- ✅ 至少定义了5个基础颜色
- ✅ 颜色有不同深浅（50-950）
- ✅ 有CSS变量系统（light/dark模式）
- ✅ 至少20个CSS变量

### 4. 无障碍支持检查（3分）

```bash
grep -r "aria-\|role=" components/ app/ --include="*.tsx"
```

**必须具备**：
- ✅ 按钮有aria-label
- ✅ 表单输入有关联的label
- ✅ 图标有aria-hidden="true"
- ✅ 交互元素有role属性

### 5. 响应式设计检查（2分）

```bash
grep -r "md:\|lg:\|xl:" components/ app/ --include="*.tsx"
```

**必须具备**：
- ✅ 主要布局有移动端适配
- ✅ 表格在小屏幕可横向滚动
- ✅ 导航在移动端可用
- ✅ 至少使用3个断点（md/lg/xl）

## 通过标准

≥ 16分（80%）

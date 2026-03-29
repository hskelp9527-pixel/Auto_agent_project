# 第3轮：代码质量检查 (code-quality-check)

**权重**: ⭐ **10%**

## 目标

确保代码规范、健壮、可维护。

## 检查项目（总分10分）

### 1. TypeScript使用检查（3分）

```bash
find . -name "*.ts" -o -name "*.tsx" | wc -l
grep -r ": any" app/ components/ --include="*.tsx" --include="*.ts"
```

**必须具备**：
- ✅ 至少90%代码使用TypeScript
- ✅ `any`类型使用少于5处
- ✅ 组件有Props类型定义

### 2. 组件化检查（3分）

```bash
ls components/
grep -r "from.*components" app/ --include="*.tsx" | wc -l
```

**必须具备**：
- ✅ 至少有5个可复用组件
- ✅ 组件在多处被引用
- ✅ 组件职责单一

### 3. 错误处理检查（2分）

```bash
grep -r "try\|catch\|error" app/ --include="*.tsx"
```

**必须具备**：
- ✅ 表单提交有错误处理
- ✅ API调用有错误处理
- ✅ 用户友好的错误提示

### 4. 代码规范检查（2分）

```bash
ls .prettierrc .eslintrc*
grep -r "import.*from" app/ --include="*.tsx" | head -10
```

**必须具备**：
- ✅ 有代码格式化配置（Prettier/ESLint）
- ✅ import语句规范有序
- ✅ 命名规范一致

## 通过标准

≥ 8分（80%）

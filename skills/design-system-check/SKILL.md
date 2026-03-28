---
name: design-system-check
description: 设计系统质量检查 - 在验证代码功能之前，先检查设计系统质量。确保项目使用现代化的设计系统（如shadcn/ui + Radix UI），具备完整的设计令牌、无障碍支持、类型安全等电商级应用标准。避免"功能完整但视觉效果差"的问题。
---

# 设计系统质量检查 (Design System Quality Check)

## 核心使命

**不要让"功能完整"掩盖"设计系统缺失"。**

在开始功能验证之前，先检查设计系统质量。电商级应用不仅功能要完整，设计系统也必须达到企业级标准。

## 何时触发

**必须在代码级验证之前执行**：
- 新项目创建后
- 接手现有项目时
- 准备重构UI时
- 用户反馈"效果不如XX应用"时

**执行顺序**：
```
设计系统检查 → 代码级验证 → 功能验证 → 深度测试
```

## 检查清单

### 1. 核心依赖检查

**必须具备的依赖**：
```json
{
  // ✅ 设计系统核心
  "class-variance-authority": "^0.7.0",
  "tailwind-merge": "^2.5.0",
  "tailwindcss-animate": "^1.0.7",
  "clsx": "^2.1.1",

  // ✅ 无障碍组件（Radix UI）
  "@radix-ui/react-dialog": "^1.1.2",
  "@radix-ui/react-dropdown-menu": "^2.1.2",
  "@radix-ui/react-popover": "^1.1.2",
  "@radix-ui/react-select": "^2.1.2",
  "@radix-ui/react-tabs": "^1.1.1",
  "@radix-ui/react-toast": "^1.2.2",

  // ✅ 高级组件
  "cmdk": "^1.0.0",
  "embla-carousel-react": "^8.3.0",
  "react-resizable-panels": "^2.1.0",
  "sonner": "^1.5.0",
  "vaul": "^1.1.0"
}
```

**检查命令**：
```bash
cat package.json | grep -E "class-variance-authority|tailwind-merge|@radix-ui"
```

**如果缺失**：
```bash
npm install class-variance-authority tailwind-merge tailwindcss-animate
npm install @radix-ui/react-dialog @radix-ui/react-dropdown-menu @radix-ui/react-popover @radix-ui/react-select @radix-ui/react-tabs @radix-ui/react-toast
npm install cmdk embla-carousel-react react-resizable-panels sonner vaul
```

### 2. Tailwind配置检查

**必须具备的配置**：
```ts
// tailwind.config.ts
import type { Config } from "tailwindcss"

const config = {
  darkMode: ["class"],  // ✅ 必须支持暗色模式
  content: ["./pages/**/*.{ts,tsx}", "./components/**/*.{ts,tsx}", "./app/**/*.{ts,tsx}", "./src/**/*.{ts,tsx}"],
  theme: {
    extend: {
      colors: {
        // ✅ 必须使用HSL设计令牌
        border: "hsl(var(--border))",
        input: "hsl(var(--input))",
        ring: "hsl(var(--ring))",
        background: "hsl(var(--background))",
        foreground: "hsl(var(--foreground))",
        primary: {
          DEFAULT: "hsl(var(--primary))",
          foreground: "hsl(var(--primary-foreground))",
        },
        secondary: {
          DEFAULT: "hsl(var(--secondary))",
          foreground: "hsl(var(--secondary-foreground))",
        },
        // ... 其他颜色
      },
      borderRadius: {
        lg: "var(--radius)",
        md: "calc(var(--radius) - 2px)",
        sm: "calc(var(--radius) - 4px)",
      },
    },
  },
  plugins: [require("tailwindcss-animate")],
} satisfies Config

export default config
```

**检查命令**：
```bash
cat tailwind.config.ts | grep -E "darkMode|hsl\(var\(--|tailwindcss-animate"
```

### 3. CSS变量系统检查

**必须具备的globals.css**：
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  :root {
    /* ✅ 完整的亮色主题变量 */
    --background: 0 0% 100%;
    --foreground: 222.2 84% 4.9%;
    --primary: 221.2 83.2% 53.3%;
    --primary-foreground: 210 40% 98%;
    /* ... 至少20+个变量 */
  }

  .dark {
    /* ✅ 完整的暗色主题变量 */
    --background: 222.2 84% 4.9%;
    --foreground: 210 40% 98%;
    /* ... 对应的暗色变量 */
  }
}

@layer base {
  * {
    @apply border-border;
  }
  body {
    @apply bg-background text-foreground;
  }
}
```

**检查命令**：
```bash
cat app/globals.css | grep -E "var\(--|:root|\.dark"
```

**最少变量数量**：亮色主题20+，暗色主题20+

### 4. 工具函数检查

**必须具备lib/utils.ts**：
```ts
import { clsx, type ClassValue } from "clsx"
import { twMerge } from "tailwind-merge"

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}
```

**检查命令**：
```bash
cat lib/utils.ts | grep -E "twMerge|cn"
```

### 5. 组件质量检查

**Button组件标准**：
```tsx
import * as React from "react"
import { cva, type VariantProps } from "class-variance-authority"
import { cn } from "@/lib/utils"

// ✅ 使用CVA管理variants
const buttonVariants = cva(
  "inline-flex items-center justify-center rounded-md text-sm font-medium ring-offset-background transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50",
  {
    variants: {
      variant: {
        default: "bg-primary text-primary-foreground hover:bg-primary/90",
        destructive: "bg-destructive text-destructive-foreground hover:bg-destructive/90",
        outline: "border border-input bg-background hover:bg-accent hover:text-accent-foreground",
        secondary: "bg-secondary text-secondary-foreground hover:bg-secondary/80",
        ghost: "hover:bg-accent hover:text-accent-foreground",
        link: "text-primary underline-offset-4 hover:underline",
      },
      size: {
        default: "h-10 px-4 py-2",
        sm: "h-9 rounded-md px-3",
        lg: "h-11 rounded-md px-8",
        icon: "h-10 w-10",
      },
    },
    defaultVariants: {
      variant: "default",
      size: "default",
    },
  }
)

// ✅ 完整类型导出
export interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  asChild?: boolean
}

const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(
  ({ className, variant, size, ...props }, ref) => {
    return (
      <button
        className={cn(buttonVariants({ variant, size, className }))}
        ref={ref}
        {...props}
      />
    )
  }
)
Button.displayName = "Button"

export { Button, buttonVariants }
```

**检查要点**：
- ✅ 使用class-variance-authority
- ✅ 使用VariantProps类型
- ✅ 使用cn()合并className
- ✅ 使用设计令牌（hsl(var(--...))）
- ✅ 无硬编码颜色
- ✅ 无className重复

**检查命令**：
```bash
cat components/ui/Button.tsx | grep -E "cva|VariantProps|twMerge|buttonVariants"
```

### 6. 无障碍性检查

**必须避免的问题**：
```tsx
// ❌ 重复的aria-label
<button aria-label="button" aria-label="关闭">

// ❌ 错误的aria-level类型
<h1 aria-level="1">  // 字符串应该是数字

// ❌ 手动管理ARIA（容易出错）
<div role="dialog" aria-labelledby="...">

// ✅ 使用Radix UI组件（自动管理ARIA）
import { Dialog } from "@/components/ui/dialog"
```

**检查命令**：
```bash
grep -r 'aria-label.*aria-label' app/ components/
grep -r 'aria-level="' app/ components/
```

**应该为0个结果**

### 7. 暗色模式支持检查

**必须具备**：
```tsx
// ✅ 暗色模式切换器
import { Moon, Sun } from "lucide-react"
import { useTheme } from "next-themes"

export function ThemeToggle() {
  const { theme, setTheme } = useTheme()
  return (
    <button onClick={() => setTheme(theme === "dark" ? "light" : "dark")}>
      {theme === "dark" ? <Sun /> : <Moon />}
    </button>
  )
}
```

**检查命令**：
```bash
cat package.json | grep "next-themes"
ls components/ | grep -i "theme"
```

## 评分系统

### 优秀 (90-100分)
- ✅ 所有核心依赖齐全
- ✅ 完整的CSS变量系统（30+变量）
- ✅ 所有组件使用CVA
- ✅ 完整的Radix UI集成
- ✅ 暗色模式支持
- ✅ 无无障碍性问题

### 良好 (70-89分)
- ✅ 大部分核心依赖
- ⚠️ 基础CSS变量（15-29个）
- ⚠️ 部分组件使用CVA
- ⚠️ 基础Radix UI
- ❌ 无暗色模式

### 需要改进 (50-69分)
- ⚠️ 少数核心依赖
- ⚠️ 简单CSS变量（<15个）
- ❌ 无CVA
- ❌ 无Radix UI
- ❌ 无暗色模式

### 不合格 (<50分)
- ❌ 无设计系统
- ❌ 硬编码样式
- ❌ 无className优化
- ❌ 无设计令牌

## 常见问题修复

### 问题1: className重复
```bash
# 查找重复
grep -r "transition-all.*transition-all" components/

# 修复：使用tailwind-merge
className={cn("transition-all", className)}
```

### 问题2: 硬编码颜色
```bash
# 查找硬编码
grep -r "bg-blue-600\|text-white" components/ui/Button.tsx

# 修复：使用设计令牌
className="bg-primary text-primary-foreground"
```

### 问题3: 无CVA
```bash
# 检查
grep -r "class-variance-authority" package.json

# 安装
npm install class-variance-authority
```

## 输出报告模板

```markdown
## 设计系统质量检查报告

**项目**: [项目名称]
**检查日期**: [日期]
**总分**: [X]/100

### 依赖检查
- ✅/❌ class-variance-authority
- ✅/❌ tailwind-merge
- ✅/❌ Radix UI组件
- ✅/❌ next-themes

### 配置检查
- ✅/❌ Tailwind设计令牌
- ✅/❌ CSS变量系统
- ✅/❌ 暗色模式支持

### 组件质量
- ✅/❌ CVA使用
- ✅/❌ 类型安全
- ✅/❌ className优化

### 无障碍性
- ✅/❌ Radix UI集成
- ✅/❌ ARIA属性正确性
- ✅/❌ 键盘导航

### 问题列表
1. [问题描述] - [严重程度]
   - 位置: [文件/位置]
   - 修复方案: [方案]

### 建议
- [改进建议1]
- [改进建议2]

### 结论
[优秀/良好/需要改进/不合格]
```

## 核心原则

1. **设计先行**: 先检查设计系统，再检查功能
2. **标准统一**: 遵循shadcn/ui标准
3. **无障碍优先**: 使用Radix UI确保无障碍
4. **类型安全**: 完整TypeScript支持
5. **可维护性**: 使用CVA和设计令牌

## 最后的话

> "功能完整不等于设计完整。电商级应用必须具备企业级设计系统。"
>
> "设计系统检查不是'可选项'，而是'必选项'。"
>
> "视觉效果差会影响用户信任，即使功能再强大。"

---

**Skill 维护**: 基于shadcn/ui和现代设计系统最佳实践
**核心理念**: 设计系统质量 = 用户体验质量
**创建日期**: 2026-03-29

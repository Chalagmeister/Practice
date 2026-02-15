# CSS Custom Properties Organization - Best Practices Guide

## 📋 Overview
This guide explains the best practices for organizing CSS custom properties (CSS variables) in your design system.

## 🎯 Key Principles

### 1. **Token Hierarchy: Primitive → Semantic → Component**

```
Primitive Tokens (Foundation)
    ↓
Semantic Tokens (Meaning)
    ↓
Component Tokens (Usage)
```

#### Primitive Tokens (What you have)
- Raw design values
- No context or meaning
- Examples: `--color-neutral-900`, `--spacing-200`, `--radius-8`

#### Semantic Tokens (Recommended addition)
- Add meaning and context
- Reference primitive tokens
- Examples: `--color-text-primary`, `--color-bg-dark`, `--spacing-section`

#### Component Tokens (Optional)
- Specific to components
- Reference semantic or primitive tokens
- Examples: `--button-bg-color`, `--card-padding`, `--header-height`

---

## 📁 File Organization Strategies

### **Option 1: Single File Approach** ✅ (Recommended for small-medium projects)
All tokens in one `design-system.css` or `tokens.css` file, organized by sections with clear comments.

**Pros:**
- Simple to maintain
- Easy to find tokens
- Single source of truth
- Fewer HTTP requests

**Cons:**
- Can become large
- May have some unused tokens loaded

```css
/* tokens.css */
:root {
  /* Colors */
  /* Spacing */
  /* Typography */
  /* etc. */
}
```

---

### **Option 2: Multi-File Approach** (For large projects)
Separate files for each token category.

**Structure:**
```
styles/
├── tokens/
│   ├── _colors.css
│   ├── _spacing.css
│   ├── _typography.css
│   ├── _borders.css
│   └── index.css (imports all)
└── main.css
```

**Pros:**
- Better organization for large projects
- Easier collaboration (less merge conflicts)
- Can selectively import

**Cons:**
- More complex
- More files to manage
- Multiple HTTP requests (unless bundled)

---

## 🎨 Naming Conventions

### **Best Practice Naming Structure:**

```
--{category}-{property}-{variant}-{state}
```

#### Examples:

```css
/* ✅ GOOD - Clear hierarchy */
--color-neutral-900
--color-salmon-500
--spacing-200
--radius-12
--font-size-24

/* ✅ GOOD - Semantic naming */
--color-bg-primary
--color-text-heading
--spacing-section
--border-radius-card

/* ❌ AVOID - Too generic */
--dark
--small
--blue

/* ❌ AVOID - Too specific */
--header-background-color-on-homepage-when-scrolled
```

---

## 📊 Recommended Organization for Your Project

Based on your design system, here's the recommended structure:

### **1. Colors** (Group by palette, not by usage)
```css
:root {
  /* Neutral palette */
  --color-neutral-900: #062630;
  --color-neutral-700: #385159;
  --color-neutral-200: #E6E1DF;
  --color-neutral-100: #FAF5F3;
  --color-neutral-0: #FFFFFF;
  
  /* Salmon palette */
  --color-salmon-500: #FEA36F;
  --color-salmon-100: #FFE2D1;
  --color-salmon-50: #FFF5EF;
}
```

### **2. Spacing** (Use a consistent scale)
```css
:root {
  /* Base unit: 8px (0.5rem) */
  --spacing-0: 0;
  --spacing-025: 0.125rem;  /* 2px */
  --spacing-050: 0.25rem;   /* 4px */
  --spacing-100: 0.5rem;    /* 8px */
  --spacing-200: 1rem;      /* 16px */
  --spacing-300: 1.5rem;    /* 24px */
  /* etc. */
}
```

### **3. Typography** (Separate primitives from presets)
```css
:root {
  /* Primitives */
  --font-primary: 'Martian Mono', monospace;
  --font-size-24: 1.5rem;
  --font-weight-bold: 700;
  --line-height-120: 120%;
  
  /* Presets (optional) */
  --text-h1-font: var(--font-primary);
  --text-h1-size: var(--font-size-62);
  --text-h1-weight: var(--font-weight-bold);
}
```

---

## 💡 Advanced Best Practices

### **1. Use Semantic Tokens for Flexibility**

```css
/* ✅ GOOD - Easy to theme */
:root {
  /* Primitive */
  --color-neutral-900: #062630;
  
  /* Semantic */
  --color-bg-primary: var(--color-neutral-0);
  --color-text-heading: var(--color-neutral-900);
}

/* Easy to create dark mode */
[data-theme="dark"] {
  --color-bg-primary: var(--color-neutral-900);
  --color-text-heading: var(--color-neutral-0);
}
```

### **2. Typography Presets: Two Approaches**

#### **Approach A: Individual Properties** (More flexible)
```css
.heading-1 {
  font-family: var(--font-primary);
  font-size: var(--font-size-62);
  font-weight: var(--font-weight-bold);
  line-height: var(--line-height-120);
  letter-spacing: var(--letter-spacing-normal);
}
```

#### **Approach B: Composite Tokens** (More organized)
```css
:root {
  --text-h1: var(--font-weight-bold) var(--font-size-62)/var(--line-height-120) var(--font-primary);
}

.heading-1 {
  font: var(--text-h1);
  letter-spacing: var(--letter-spacing-normal);
}
```

**Recommendation:** Use Approach A for better browser support and clarity.

---

### **3. Responsive Design Tokens**

```css
:root {
  --spacing-section: var(--spacing-400);
  --text-h1-size: var(--font-size-38);
}

@media (min-width: 768px) {
  :root {
    --spacing-section: var(--spacing-600);
    --text-h1-size: var(--font-size-62);
  }
}
```

---

## 🔧 Implementation Tips

### **1. Load Order**
```html
<!-- index.html -->
<link rel="stylesheet" href="tokens.css">      <!-- Load first -->
<link rel="stylesheet" href="components.css">  <!-- Then components -->
<link rel="stylesheet" href="utilities.css">   <!-- Then utilities -->
```

### **2. Documentation**
Always document your tokens:
```css
:root {
  /* Primary brand color - used for CTAs and links */
  --color-salmon-500: #FEA36F;
  
  /* Section spacing - top/bottom margins for major sections */
  --spacing-section: var(--spacing-600);
}
```

### **3. Validation**
Create a test page that displays all tokens to catch errors:
```html
<!-- token-test.html -->
<div style="background: var(--color-neutral-900)">
  <p style="color: var(--color-neutral-0)">Test</p>
</div>
```

---

## 📝 Summary of Recommendations for Your Project

1. **Use a single file** (`design-system.css`) - your project is medium-sized
2. **Keep the structure you have** - it's well organized by category
3. **Add semantic tokens** for theming flexibility:
   ```css
   --color-bg-primary: var(--color-neutral-0);
   --color-text-body: var(--color-neutral-700);
   ```
4. **For typography presets**, keep individual properties (more flexible)
5. **Use rem units** for spacing (better accessibility)
6. **Document complex tokens** with comments
7. **Consider creating utility classes** for frequently used combinations

---

## 🚀 Next Steps

1. ✅ Implement the base tokens (you have this)
2. ✅ Add semantic tokens for common use cases
3. ✅ Create utility classes for spacing, typography
4. ✅ Test all tokens on a test page
5. ✅ Document usage in a style guide
6. ✅ Set up responsive breakpoint tokens

---

## Example Usage in Components

```css
/* Using primitive tokens directly */
.card {
  background: var(--color-neutral-0);
  border-radius: var(--radius-12);
  padding: var(--spacing-300);
}

/* Using semantic tokens (recommended) */
.card {
  background: var(--color-bg-primary);
  border-radius: var(--border-radius-card);
  padding: var(--spacing-card);
}
```

The semantic approach makes theming much easier!

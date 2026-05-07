# Hugo Site Setup for C++ Course with Zen Theme v0.161.0

## Initialization Steps

### 1. Create and Configure Your Hugo Site

Start by creating a new Hugo project and setting up the Zen theme as a git submodule:

```bash
# Create new Hugo project
hugo new site my-cpp-course

# Enter directory
cd my-cpp-course

# Initialize git (required for theme management)
git init

# Add Zen theme as submodule with the specific version
git submodule add https://github.com/frjo/hugo-theme-zen.git themes/zen

# Create hugo.toml configuration
cat > hugo.toml << 'EOF'
baseURL = "https://example.com/"
languageCode = "en-us"
title = "C++ Programming Course"
theme = "zen"

[taxonomies]
  difficulty = "difficulties"
  agent = "agents"
  topic = "topics"
EOF

# Start development server
hugo server
```

**Why this approach:** Using git submodules keeps your theme versioned separately, preventing accidental modifications to vendored code. Taxonomies defined upfront in the configuration prevent conflicts later and enable automatic page generation for filtering and discovery.

### 2. Initialize Core Content Directories

Create the three main content sections for your course:

```bash
mkdir -p content/{modules,homework,resources}
```

## Content Directory Structure

Organize your course into a clear, hierarchical structure that leverages Hugo's bundle system:

```
content/
├── modules/
│   ├── module-01/
│   │   ├── _index.md           # Section overview
│   │   ├── lesson-01.md        # Child page
│   │   ├── lesson-02.md        # Child page
│   │   └── lesson-03.md        # Child page
│   ├── module-02/
│   │   ├── _index.md
│   │   ├── lesson-01.md
│   │   └── lesson-02.md
│   └── module-03/
│       └── _index.md
│
├── homework/
│   ├── assignment-01/
│   │   ├── index.md            # Assignment page
│   │   ├── starter-code.cpp    # Accompanying resource
│   │   └── rubric.pdf          # Accompanying resource
│   ├── assignment-02/
│   │   ├── index.md
│   │   ├── template.h
│   │   └── test-cases.txt
│   └── assignment-03/
│       └── index.md
│
└── resources/
    ├── cpp-reference/
    │   ├── index.md
    │   ├── style-guide.pdf
    │   └── coding-standards.txt
    ├── tools-setup/
    │   ├── index.md
    │   └── compiler-config.txt
    └── debugging-tips/
        └── index.md
```

### Bundle Types Explained

**Section Bundles (Modules):** Use `_index.md` for pages with children

- A module is a section bundle containing an overview and multiple lessons
- Hugo automatically generates a list page showing child lessons
- Use `weight` field to control ordering

**Leaf Bundles (Homework & Resources):** Use `index.md` for pages without children

- Each assignment is a self-contained unit with `index.md` as the main page
- Accompanying files (PDFs, code samples, rubrics) live in the same directory
- Resources are accessible to templates via Hugo's `.Resources` method
- Create natural styling boundaries—one CSS rule per semantic unit

## Front Matter Templates

### Module Overview Template

```yaml
---
title: "Module 1: Introduction to C++"
weight: 1
description: "Learn C++ fundamentals including variables, data types, and control flow"
---

Overview content here. Students will learn:
- Basic syntax and data types
- Variables and memory
- Control structures (if, loops, functions)
```

### Lesson Template (Child of Module)

```yaml
---
title: "Lesson 1: Setting Up Your Environment"
weight: 1
---

Content describing the lesson topic.
```

### Assignment Template (Homework)

```yaml
---
title: "Assignment 1: Basic Data Types"
weight: 1
difficulty: "beginner"
agents:
  - memory
  - reasoning
topics:
  - "integers"
  - "pointers"
  - "dynamic memory"
dueDate: 2025-05-15
description: "Practice working with fundamental C++ data types and memory management"
---

Assignment description, requirements, and submission instructions.
```

### Resource Template

```yaml
---
title: "C++ Reference Guide"
weight: 1
description: "Quick reference for standard library functions and syntax"
---

Content describing the resource.
```

## Custom Taxonomies

The configuration defines three taxonomies for organizing and filtering content:

### 1. Difficulty Levels

Create assignments at different complexity levels:

```yaml
difficulty: "beginner"      # Introductory topics
difficulty: "intermediate"  # Building on basics
difficulty: "advanced"      # Challenging extensions
```

### 2. Agent Types

Tag assignments by what skills or agents they develop:

```yaml
agents:
  - memory          # Memory management practice
  - reasoning       # Logic and problem-solving
  - optimization    # Performance considerations
  - testing         # Test-driven development
```

### 3. C++ Topics

Organize by subject matter covered:

```yaml
topics:
  - "variables"
  - "data types"
  - "pointers"
  - "dynamic memory"
  - "functions"
  - "classes"
  - "templates"
  - "STL"
```

Hugo automatically generates list pages:
- `/difficulties/beginner/` — all beginner assignments
- `/agents/memory/` — all memory-focused assignments
- `/topics/pointers/` — all assignments about pointers

## CSS Best Practices with Zen Theme

The Zen theme is minimalist by design. Keep it that way by following these guidelines:

### 1. Structure First, Styles Second

- Organize content into semantic bundles (modules, lessons, assignments) first
- Let the bundle structure be your styling boundary
- Avoid adding CSS classes to work around poor content organization

### 2. Use Zen's Built-in Styles

Check `themes/zen/assets/css/` before writing custom CSS. Zen covers:
- Typography and typography scaling
- Spacing and layout
- Colors and contrast
- Responsive design

### 3. Leverage CSS Custom Properties

Reuse Zen's variables instead of hardcoding values:

```css
color: var(--color-text);
background: var(--color-background);
padding: var(--spacing-2);
font-family: var(--font-sans);
```

### 4. Keep Custom Styles Minimal

Create `static/css/custom.css` for course-specific styling:

```css
@layer course-specific {
  /* Only add styles that Zen doesn't cover */
  .assignment {
    border: 1px solid var(--color-border);
    padding: var(--spacing-3);
    margin-bottom: var(--spacing-4);
  }

  .assignment-due-date {
    color: var(--color-accent);
    font-weight: bold;
  }

  .difficulty-badge {
    display: inline-block;
    padding: var(--spacing-1) var(--spacing-2);
    border-radius: 0.25rem;
    font-size: 0.875rem;
  }
}
```

## Common Patterns and Error Prevention

### 1. Always Use ISO Date Format

```yaml
# Correct
dueDate: 2025-05-15

# Incorrect (ambiguous, often fails)
dueDate: 05-15-2025
```

### 2. Match Taxonomy Names Correctly

```toml
# In hugo.toml
[taxonomies]
  difficulty = "difficulties"
```

```yaml
# In front matter — use singular form
difficulty: "beginner"        # Correct
difficulties: "beginner"      # Wrong
```

### 3. Use Correct Bundle File Names

- **Sections with children:** `_index.md` (generates list page)
- **Leaf pages:** `index.md` (no list page)
- Wrong choice silently breaks content visibility

### 4. Prevent Content Visibility Issues

Always set draft status explicitly:

```yaml
---
title: "My Assignment"
draft: false    # Set explicitly; don't rely on defaults
---
```

## Quick Start Commands

```bash
# Start local development server
hugo server

# Build for production
hugo

# Create new content
hugo new content/modules/module-02/_index.md
hugo new content/homework/assignment-05/index.md

# Check for errors and warnings
hugo --debug

# Update theme to latest version
git submodule update --remote themes/zen
```

## Project Organization Summary

For your C++ course with Zen theme v0.161.0:

**Initialization:**
- Create site with `hugo new site`
- Add Zen theme as git submodule
- Configure `hugo.toml` with taxonomies for difficulty, agents, and topics
- Initialize three content directories: modules, homework, resources

**Content Structure:**
- **Modules:** Section bundles with `_index.md` and lesson child pages
- **Homework:** Leaf bundles with `index.md` and accompanying resources (code, rubrics, tests)
- **Resources:** Leaf bundles for reference materials, guides, and tools

**Front Matter:**
- Use standard fields: `title`, `weight`, `description`, `draft`
- Add custom fields: `difficulty`, `agents`, `topics`, `dueDate`
- Ensure all dates use ISO format (YYYY-MM-DD)

**Styling:**
- Use Zen's built-in CSS variables and components
- Create minimal custom CSS in `static/css/custom.css`
- Use CSS layers to avoid specificity conflicts
- One CSS rule per semantic unit

This structure scales well as your course grows, remains maintainable through clear organization, and leverages Hugo's automation to generate filtering and discovery pages automatically.

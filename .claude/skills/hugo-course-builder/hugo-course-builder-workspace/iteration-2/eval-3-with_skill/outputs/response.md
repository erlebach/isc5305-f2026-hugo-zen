# Hugo Bundles and CSS Best Practices for Your Course Site

## Overview

You're organizing course modules with lessons and resources—exactly the use case Hugo bundles were designed for. Understanding section bundles vs. leaf bundles is critical to keeping your content semantic and your CSS maintainable.

---

## Section Bundles vs. Leaf Bundles

### Section Bundles (Branch Bundles)

**What they are:** Directories containing `_index.md` and child pages. They create logical groupings that generate their own list pages.

**Structure:**
```
content/modules/module-01/
├── _index.md              # The section itself
├── lesson-01.md           # Child page
├── lesson-02.md           # Child page
└── resources/
    └── code-example.cpp   # Supporting file
```

**Key characteristics:**
- Use `_index.md` (not `index.md`)—this makes Hugo treat it as a section
- Can have unlimited child pages (lessons)
- Automatically generate a list page at `/modules/module-01/`
- Children are accessible via `.Pages` in templates
- Perfect for modules that group related lessons

**When to use:**
- Course modules with multiple lessons
- Units or chapters with sub-topics
- Any content where the parent is a container, not a standalone page

**Front matter example:**
```yaml
---
title: "Module 1: Introduction to C++"
weight: 1
description: "Learn the basics of C++ programming, variables, and control flow"
---
```

### Leaf Bundles (Page Bundles)

**What they are:** Directories containing `index.md` (not `_index.md`) and supporting resources. They represent a single page with attached files.

**Structure:**
```
content/homework/assignment-01/
├── index.md               # The page itself
├── starter-code.cpp       # Resource
├── rubric.pdf             # Resource
└── solution.cpp           # Resource
```

**Key characteristics:**
- Use `index.md` (not `_index.md`)—this makes it a leaf, not a section
- Resources stay alongside the page and are accessible via `.Resources` in templates
- No automatic list page generated (the page itself is the terminal node)
- Resources can be referenced without hardcoding paths
- Perfect for assignments, labs, and standalone resources

**When to use:**
- Assignments with rubrics, starter code, or examples
- Individual lessons with code files
- Any standalone page that needs accompanying files

**Front matter example:**
```yaml
---
title: "Assignment 1: Basic Data Types"
weight: 1
difficulty: "beginner"
agents:
  - memory
  - reasoning
topics:
  - integers
  - pointers
dueDate: 2025-05-15
description: "Build a program that manipulates integers and pointers"
---
```

### The Critical Difference

| Aspect | Section Bundle | Leaf Bundle |
|--------|---|---|
| **File** | `_index.md` | `index.md` |
| **Purpose** | Container for children | Standalone page |
| **List page?** | Yes (automatic) | No |
| **Resources?** | Manual, in subdirs | Via `.Resources` |
| **Use case** | Modules, chapters, units | Assignments, lessons, pages |

---

## Recommended Organization for Your Course

Based on your structure (modules with lessons and resources), here's a clean setup:

```
content/
├── modules/                          # Section bundles
│   ├── module-01/
│   │   ├── _index.md                 # Module overview
│   │   ├── lesson-01.md              # First lesson (could be leaf or child page)
│   │   ├── lesson-02.md              # Second lesson
│   │   └── _resources/               # Supporting materials
│   │       ├── diagram-01.svg
│   │       └── example-code.cpp
│   ├── module-02/
│   │   ├── _index.md
│   │   └── lesson-01.md
│   └── module-03/
│       └── _index.md
├── homework/                         # Leaf bundles
│   ├── assignment-01/
│   │   ├── index.md
│   │   ├── starter.cpp
│   │   └── rubric.pdf
│   └── assignment-02/
│       ├── index.md
│       └── test-suite.cpp
└── resources/                        # Standalone reference materials
    ├── cpp-style-guide/
    │   ├── index.md
    │   └── examples/
    └── environment-setup/
        ├── index.md
        └── install-script.sh
```

### Why This Structure Works

1. **Modules are sections** — They group lessons together and generate `/modules/` list pages for browsing
2. **Assignments are leaves** — Each assignment bundles its rubric, starter code, and solution in one directory
3. **Resources are organized clearly** — Diagrams and code examples stay with their lessons via the `_resources/` subdirectory
4. **CSS follows the hierarchy** — Each semantic unit (module, lesson, assignment) maps to a bundle, not arbitrary divs

---

## How to Implement With Your Zen Theme

Your `hugo.yaml` already references the Zen theme. Here's how to leverage bundles:

### 1. Create a module with lessons

```bash
# Create the module section
mkdir -p content/modules/module-01
cat > content/modules/module-01/_index.md << 'EOF'
---
title: "Module 1: C++ Fundamentals"
weight: 1
description: "Learn variables, types, and control flow"
---

This module covers the essential building blocks of C++ programs.
EOF

# Add a lesson
cat > content/modules/module-01/lesson-01.md << 'EOF'
---
title: "Lesson 1: Variables and Types"
weight: 1
---

Content about variables...
EOF
```

### 2. Create an assignment (leaf bundle)

```bash
# Create the assignment
mkdir -p content/homework/assignment-01
cat > content/homework/assignment-01/index.md << 'EOF'
---
title: "Assignment 1: Data Type Exploration"
weight: 1
difficulty: "beginner"
topics:
  - "int"
  - "float"
  - "string"
dueDate: 2025-05-15
---

Build a program that...
EOF

# Add resources (they stay in the same directory)
cp ~/Documents/starter.cpp content/homework/assignment-01/
cp ~/Documents/rubric.pdf content/homework/assignment-01/
```

### 3. Reference resources in templates

In your layout for assignments (`layouts/homework/single.html`):

```html
{{ define "main" }}
  <article class="assignment">
    <h1>{{ .Title }}</h1>
    <p class="due-date">Due: {{ .Params.dueDate }}</p>

    {{ .Content }}

    {{ if .Resources }}
    <section class="resources">
      <h2>Resources</h2>
      <ul>
      {{ range .Resources }}
        <li><a href="{{ .RelPermalink }}">{{ .Title }}</a></li>
      {{ end }}
      </ul>
    </section>
    {{ end }}
  </article>
{{ end }}
```

The `.Resources` method automatically finds files bundled with `index.md`—no hardcoding paths needed.

---

## Keeping Zen's CSS Clean and Maintainable

Zen is minimalist by design. Your job is to preserve that. Here's how:

### Core Principle: Structure First, Styles Second

**The danger:** Adding CSS to "fix" poor content structure creates a debt spiral. One bad selector begets five more. Soon your custom CSS is larger than Zen's.

**The solution:** Make your content structure do the semantic work. Bundles are your first defense.

### CSS Architecture with Bundles

Your bundle structure should directly map to your CSS selectors:

```css
/* ✓ Good: One rule per semantic unit */
.module { /* styles for a module section */ }
.lesson { /* styles for a lesson page */ }
.assignment { /* styles for an assignment */ }

/* ✗ Bad: Attempting to style internal structure */
.module-header { /* ... */ }
.module-title { /* ... */ }
.module-description { /* ... */ }
.module-content { /* ... */ }
```

Why? The first approach scales. The second creates specificity wars and forces you to add more selectors later.

### Practical CSS Strategy for Zen

#### 1. Audit Zen first

Before writing any custom CSS, check what Zen already provides:

```bash
ls themes/zen/assets/css/
```

Zen covers typography, spacing, colors, and responsive layout. Your custom CSS should only add course-specific styles.

#### 2. Use Zen's CSS custom properties

Zen defines variables like `--color-text`, `--color-background`, `--spacing-2`, etc. Reuse them:

```css
/* Instead of defining your own colors */
.assignment-header {
  color: var(--color-text);           /* Zen's variable */
  background: var(--color-background); /* Zen's variable */
  padding: var(--spacing-2);          /* Zen's spacing scale */
}
```

This ensures consistency and makes theme changes trivial.

#### 3. Keep custom CSS in a separate file

```
static/
└── css/
    └── custom.css       # Only YOUR course-specific styles
```

Link it in `layouts/partials/header.html`:

```html
<link rel="stylesheet" href="/css/custom.css">
```

Keep this file small (< 500 lines for a course site).

#### 4. Use CSS layers to prevent cascade conflicts

```css
@layer zen, course-specific;

@layer course-specific {
  .assignment {
    border: 2px solid var(--color-accent);
    border-radius: 4px;
  }

  .assignment-resources {
    margin-top: var(--spacing-3);
  }
}
```

Layers prevent your styles from accidentally overriding Zen's, and vice versa.

#### 5. Example: Styling assignments

Instead of this:

```css
/* ✗ Bad: Too many selectors, poor maintenance */
.homework-item { margin-bottom: 2rem; }
.homework-title { font-size: 1.5rem; font-weight: bold; color: #333; }
.homework-due { color: #666; font-size: 0.9rem; }
.homework-description { line-height: 1.6; }
.homework-resources { margin-top: 1.5rem; padding-top: 1.5rem; border-top: 1px solid #ddd; }
.homework-resources li { margin-bottom: 0.5rem; }
```

Write this:

```css
@layer course-specific {
  .assignment {
    /* Let semantic HTML handle structure, Zen handles base typography */
  }

  .assignment article > h1 {
    /* Override only if Zen's heading size doesn't match your design */
  }

  .assignment .due-date {
    color: var(--color-accent);
    font-weight: bold;
  }

  .assignment .resources {
    margin-top: var(--spacing-3);
    padding-top: var(--spacing-3);
    border-top: 1px solid var(--color-border);
  }
}
```

### CSS Maintenance Checklist

**Weekly:**
- Count your CSS rules: `wc -l static/css/custom.css`
- If it's growing quickly, audit for unused styles

**Before merging:**
- Check for duplicate selectors: `grep "^\." static/css/custom.css | sort | uniq -d`
- Verify all selectors are used in at least one template

**Quarterly:**
- Review Zen's theme variables—new Zen releases may provide solutions for your "custom" styles
- Test your site in light and dark modes if Zen supports them

### Red Flags (Stop and Refactor)

If you find yourself doing any of these, your content structure needs fixing, not CSS workarounds:

1. **Writing more than 5 selectors per component** — Your bundle structure isn't semantic enough
2. **Using `!important`** — Sign of a cascade conflict; use CSS layers instead
3. **Hardcoding colors** — Use `var(--color-*)` from Zen's palette
4. **Styling based on HTML structure** — Move that work to your templates or bundles
5. **Custom CSS growing past 400 lines** — You're decorating, not designing; step back and use Zen's defaults

---

## Integration with Your Course Site

Your `hugo.yaml` is already configured well. Here's what to do next:

1. **Create the directory structure** as shown above
2. **Convert assignments to leaf bundles** — Move code examples and rubrics into bundle directories
3. **Use the Zen CSS variables** in any custom styles you add
4. **Test with `hugo server`** to verify bundles generate correct URLs
5. **Check `hugo --debug`** if anything doesn't appear as expected

### Example: Migrating existing assignment

Before (scattered files):
```
content/homework/assignment-01.md
static/downloads/starter-01.cpp
static/downloads/rubric-01.pdf
```

After (bundle):
```
content/homework/assignment-01/
├── index.md
├── starter.cpp
└── rubric.pdf
```

Update your template to use `.Resources` instead of hardcoded paths—no other changes needed.

---

## Summary

- **Section bundles** (`_index.md`) group content with automatic list pages—perfect for modules
- **Leaf bundles** (`index.md`) bundle a page with resources—perfect for assignments
- **CSS discipline** follows from bundle structure; don't add CSS to compensate for poor organization
- **Reuse Zen's variables** for colors, spacing, and fonts
- **Keep custom CSS minimal** (<500 lines for a course site)
- **Use CSS layers** to prevent cascade conflicts

Your Zen theme is already optimized for this. The bundle structure ensures your content is semantic, and the CSS stays clean because your content organization does the styling work.

---

## Quick Reference Commands

```bash
# Create a module section
mkdir -p content/modules/my-module
cat > content/modules/my-module/_index.md << 'EOF'
---
title: "My Module"
weight: 1
---
EOF

# Create an assignment (leaf bundle)
mkdir -p content/homework/my-assignment
cat > content/homework/my-assignment/index.md << 'EOF'
---
title: "My Assignment"
weight: 1
dueDate: 2025-05-15
---
EOF

# Add resources to the assignment
cp my-starter-code.cpp content/homework/my-assignment/
cp my-rubric.pdf content/homework/my-assignment/

# Start development server
hugo server

# Debug content structure
hugo --debug 2>&1 | grep -i "bundle\|resource"
```

With this approach, your course site will remain maintainable, your content will be well-organized, and your CSS will stay minimal and clean.

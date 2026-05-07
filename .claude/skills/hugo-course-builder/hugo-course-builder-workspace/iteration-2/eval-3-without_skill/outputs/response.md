# Hugo Bundles and CSS Architecture for Course Module Organization

## Overview

Hugo bundles are a content organization pattern that groups related files and metadata together. Understanding when to use section bundles versus leaf bundles is fundamental to building a maintainable, scalable course website. Combined with thoughtful CSS architecture, bundles prevent bloat and keep your styling clean.

---

## Part 1: Understanding Hugo Bundles

### Key Concept: What Is a Bundle?

A **bundle** is a directory containing a markdown file plus related resources (images, code files, documents, etc.). Hugo treats these as a unit, allowing you to:
- Co-locate content with its assets
- Use relative paths to reference resources
- Build a clean URL structure
- Create semantic organizational tiers

The critical difference lies in the markdown filename.

### Section Bundles (Branch Bundles)

A **section bundle** is a directory containing `_index.md` (with an underscore) that can have child content. Section bundles represent organizational tiers—they create overview or list pages.

**Characteristics:**
- Filename: `_index.md` (underscore is required)
- Can contain subdirectories (child pages or other bundles)
- Generates a section list page by default
- Has its own content, front matter, and metadata
- URL structure: `/section-name/` points to the section
- Used for grouping related child content

**Real-world example:**
```
content/modules/module-1-intro/
├── _index.md                     # This makes it a section bundle
├── lesson-1-setup/
│   └── index.md
├── lesson-2-fundamentals/
│   └── index.md
└── lesson-3-practice/
    └── index.md
```

The `_index.md` file generates a page at `/modules/module-1-intro/` that can display an overview, learning objectives, and a list of child lessons.

### Leaf Bundles (Page Bundles)

A **leaf bundle** is a directory containing `index.md` (no underscore) that represents a final content page with no children. Leaf bundles are the endpoint of your hierarchy—they're self-contained units.

**Characteristics:**
- Filename: `index.md` (no underscore)
- Cannot have subdirectories with additional content
- All resources (images, code, documents) live in the same directory
- Generates a single page (not a list page)
- URL structure: `/parent/leaf-name/` for a clean, predictable path
- Resources are accessed via `.Resources` in templates or relative paths in markdown

**Real-world example:**
```
content/modules/module-1-intro/lesson-1-setup/
├── index.md                      # This makes it a leaf bundle
├── starter-code.py
├── setup-guide.pdf
└── diagram-architecture.svg
```

This generates a page at `/modules/module-1-intro/lesson-1-setup/` with all resources available in the same directory.

### Critical Difference

| Feature | Section Bundle | Leaf Bundle |
|---------|---|---|
| Filename | `_index.md` | `index.md` |
| Can have children | Yes | No |
| List page generated | Yes (automatic) | No |
| Used for | Grouping, organizing | Final content pages |
| Example | Module containing lessons | Individual lesson |

---

## Part 2: Recommended Course Structure

For a course with modules containing lessons, here's the ideal hierarchy:

```
content/
├── _index.md                              # Section: Course home
├── about/
│   └── index.md                           # Leaf: About page
├── resources/
│   └── index.md                           # Leaf: Resource library
└── modules/
    ├── _index.md                          # Section: All modules
    ├── module-1-introduction/
    │   ├── _index.md                      # Section: Module 1 overview
    │   ├── learning-objectives.md         # Optional: markdown for LOs
    │   ├── lesson-1-getting-started/
    │   │   ├── index.md                   # Leaf: Lesson 1
    │   │   ├── setup-guide.md
    │   │   ├── starter-code.py
    │   │   ├── startup-checklist.pdf
    │   │   └── images/
    │   │       ├── architecture.svg
    │   │       └── workflow.png
    │   ├── lesson-2-core-concepts/
    │   │   ├── index.md                   # Leaf: Lesson 2
    │   │   ├── concept-summary.md
    │   │   ├── code-examples/
    │   │   │   ├── example-1.py
    │   │   │   ├── example-2.py
    │   │   │   └── example-3.py
    │   │   ├── quiz.json
    │   │   └── diagrams/
    │   │       └── state-machine.svg
    │   └── lesson-3-practice/
    │       ├── index.md                   # Leaf: Lesson 3
    │       ├── exercises.md
    │       ├── solutions/
    │       │   ├── solution-1.py
    │       │   ├── solution-2.py
    │       │   └── solution-3.py
    │       └── rubric.pdf
    └── module-2-advanced-topics/
        ├── _index.md                      # Section: Module 2 overview
        ├── lesson-1-topic-a/
        │   └── index.md                   # Leaf: Lesson 1
        └── lesson-2-topic-b/
            └── index.md                   # Leaf: Lesson 2
```

### Why This Structure Works

1. **Three-tier hierarchy**: Course → Modules → Lessons
   - Course level (`content/_index.md`): section bundle for the entire course
   - Module level (`module-X/_index.md`): section bundle for grouping lessons
   - Lesson level (`lesson-X/index.md`): leaf bundle for individual content pages

2. **Clean URLs**:
   - `/` → Course home
   - `/modules/` → All modules
   - `/modules/module-1-introduction/` → Module 1 overview
   - `/modules/module-1-introduction/lesson-1-getting-started/` → Lesson 1

3. **Resource co-location**: Each lesson's code, diagrams, and documents live in its directory
   - No hunting through a global `assets/` folder
   - Easy to copy/move an entire lesson as one unit
   - Relative paths work: `./starter-code.py` instead of `/assets/module-1/lesson-1/starter-code.py`

4. **Clear semantic boundaries**: The directory structure mirrors the course hierarchy, making it intuitive for both developers and content creators

---

## Part 3: Application Guidelines

### When to Use Section Bundles

Use section bundles (`_index.md`) when you need:

1. **An organizational tier with a list page**
   - Course homepage listing modules
   - Module overview showing lessons
   - Any page that should display its children

2. **Metadata for the group**
   - Module learning objectives
   - Prerequisites for the section
   - Estimated time to complete

3. **Navigation and discovery**
   - Hugo automatically lists child pages
   - Taxonomies (categories, tags) apply to the section
   - Breadcrumbs and navigation build automatically

**Example: Module overview page**
```yaml
---
title: "Module 1: Introduction to C++"
weight: 1
description: "Learn the basics of C++ programming"
learningObjectives:
  - "Set up a C++ development environment"
  - "Write and compile your first program"
  - "Understand fundamental data types"
estimatedHours: 4
---

## Module Overview

This module introduces you to C++ programming. By the end, you'll have a working development environment and understand the core concepts.

See the lessons below to get started.
```

### When to Use Leaf Bundles

Use leaf bundles (`index.md`) when you need:

1. **Self-contained content pages**
   - Individual lessons with their own resources
   - Assignment pages with rubrics and starter code
   - Reference pages or documentation pages

2. **Co-located resources**
   - Code examples that belong to one lesson
   - Diagrams specific to the lesson's topic
   - PDF handouts or worksheets
   - Quiz files or exercise solutions

3. **Terminal pages (no children)**
   - Any page that stands alone
   - Pages that don't organize other pages

**Example: Lesson leaf bundle**
```yaml
---
title: "Lesson 1: Getting Started"
weight: 1
description: "Set up your development environment"
duration: "1.5 hours"
difficulty: "beginner"
---

# Getting Started with C++

## What You'll Learn
- Install a compiler
- Write your first program
- Run and debug it

## Setup

Follow the [setup guide](./setup-guide.md) for your operating system.

## Your First Program

Here's the starter code to get you going:
```

The resources (setup guide, code files) sit alongside `index.md` and are naturally discoverable.

---

## Part 4: Managing Resources in Bundles

### Accessing Resources in Templates

Hugo provides a `.Resources` variable for all files in a bundle. In your template:

```html
{{ range .Resources }}
  <a href="{{ .RelPermalink }}">{{ .Title }}</a>
{{ end }}
```

This lists all files in the bundle, useful for assignments with multiple download links.

### Referencing Resources in Markdown

Use relative paths in markdown files:

```markdown
# Lesson 1

See the [startup checklist](./startup-checklist.pdf) before you begin.

Install using the [setup guide](./setup-guide.md).

Check your setup with [this test script](./test-setup.py).

Here's the architecture:

![System architecture](./images/architecture.svg)
```

**Advantages:**
- Works whether you're viewing the raw markdown or on the website
- Easy to move a lesson to a different module (all paths are relative)
- Clear what resources belong to the lesson

### Organizing Resources in Subdirectories

For lessons with many resources, use subdirectories:

```
lesson-1-setup/
├── index.md
├── setup-guide.md
├── code/
│   ├── starter-code.py
│   └── test-setup.py
├── images/
│   ├── architecture.svg
│   └── workflow.png
└── docs/
    ├── checklist.pdf
    └── reference.md
```

Reference subdirectories in markdown: `./code/starter-code.py`, `./images/architecture.svg`

---

## Part 5: CSS Management with Zen Theme

The Zen theme is deliberately minimalist. Your responsibility is to keep it that way while adding course-specific styling.

### Principle 1: Let Bundle Structure Drive CSS

Your content hierarchy should create natural styling boundaries. Don't add CSS classes to work around a poorly organized bundle structure.

**Bad approach:**
```
content/modules/
├── lesson-1.md
├── lesson-2.md
└── lesson-3.md

CSS: .lesson-title, .lesson-content, .lesson-sidebar, .lesson-resources, .lesson-quiz
Result: 20+ CSS rules for one semantic unit
```

**Good approach:**
```
content/modules/
├── lesson-1/
│   └── index.md
├── lesson-2/
│   └── index.md
└── lesson-3/
    └── index.md

CSS: article { ... }  (semantic HTML handles styling)
Result: Bundle structure creates the boundary; CSS is minimal
```

### Principle 2: One CSS Rule Per Semantic Unit

Resist the urge to write multiple rules for a single element type. Instead:

1. Use semantic HTML elements (`<article>`, `<section>`, `<aside>`, `<header>`, `<footer>`)
2. Rely on Zen's cascade to handle nested elements
3. Only override when behavior needs to change, not decoration

**Bad:**
```css
.lesson-title {
  font-size: 2rem;
  color: #333;
  margin-bottom: 1rem;
}

.lesson-content {
  line-height: 1.6;
  max-width: 65ch;
}

.lesson-resources {
  border: 1px solid #ddd;
  padding: 1rem;
  margin: 2rem 0;
}
```

**Good:**
```css
/* Semantic HTML handles most of this */
/* Let Zen's cascade work */
/* Only add if you need different behavior than Zen provides */

@layer course-specific {
  .resource-box {
    background: var(--color-background-alt);
    border-left: 4px solid var(--color-accent);
  }
}
```

### Principle 3: Use Zen's Foundation First

Before writing a new CSS rule:

1. **Check what Zen already provides** — Look at `themes/zen/assets/css/`
2. **Use Zen's CSS custom properties** (variables):
   - `--color-text`, `--color-background`, `--color-accent`
   - `--spacing-1`, `--spacing-2`, `--spacing-3` (for margins/padding)
   - `--font-size-base`, `--font-size-lg`, `--font-size-sm`
3. **Leverage CSS layers** — Separate your styles from Zen's without specificity wars

### Principle 4: Lean CSS File Structure

```
static/
├── css/
│   └── custom.css           # Your course-specific styles (keep it small)
themes/zen/assets/css/       # Zen's styles (don't modify)
```

Link in your layout:
```html
<!-- In layouts/partials/header.html or similar -->
<link rel="stylesheet" href="/css/custom.css">
```

### CSS Best Practices for Zen

**Use CSS layers to organize without conflicts:**

```css
/* static/css/custom.css */

/* Layer 1: Typography and base styles */
@layer course-base {
  h1 { /* Override Zen's h1 only if needed */ }
}

/* Layer 2: Component-level styles */
@layer course-components {
  .assignment-card { /* Styles for assignment boxes */ }
  .module-grid { /* Grid for module listing */ }
}

/* Layer 3: Layout-specific styles */
@layer course-layouts {
  .lesson-sidebar { /* Lesson page sidebar */ }
}
```

**Reuse Zen's variables:**

```css
@layer course-components {
  .assignment-card {
    padding: var(--spacing-3);
    border: 1px solid var(--color-border);
    background: var(--color-background);
    border-radius: var(--border-radius, 0.25rem);
  }

  .assignment-due-date {
    color: var(--color-accent);
    font-weight: bold;
    font-size: var(--font-size-sm);
  }
}
```

**Only override when necessary:**

```css
/* ✓ Necessary: Zen doesn't style this, so add it */
@layer course-specific {
  .code-block {
    overflow-x: auto;
    background: var(--color-code-bg);
  }
}

/* ✗ Unnecessary: Zen already handles heading styles */
@layer course-specific {
  h2 {
    font-size: 1.5rem;     /* Zen does this */
    color: #333;           /* Zen does this */
    margin-bottom: 1rem;   /* Zen does this */
  }
}
```

### CSS Maintenance Checklist

**Weekly:**
- Count your CSS rules: `wc -l static/css/custom.css`
- If growing significantly, audit for unused or duplicate rules
- Check for rules solving bundle structure problems instead of styling problems

**Before deploying:**
- Run `grep -E "^[^}]*{" static/css/custom.css` to find selector blocks
- Verify no two selectors do the same thing (suggests duplication)
- Ensure all custom properties (`var(--*)`) are defined in Zen or your file

**Quarterly:**
- Review Zen's theme variables in `themes/zen/assets/css/`
- Check if a new Zen feature eliminates one of your custom rules
- Profile the site: if CSS file is over 10KB, you probably have bloat

### Why Restraint Matters

CSS complexity grows *exponentially*. Each new rule:
- Increases cascade conflicts (specificity wars)
- Makes maintenance harder (where is that style defined?)
- Slows page load (more CSS to parse)
- Doubles technical debt (fixing one thing breaks another)

Zen is minimal by design. For every 10 lines of CSS you add, you'll spend 100 lines debugging cascade issues later. Protect Zen's minimalism by:
1. Using bundles to create semantic structure (CSS follows structure)
2. Preferring semantic HTML over class-based styling
3. Overriding only when behavior needs to change
4. Auditing regularly for unused rules

---

## Part 6: Putting It Together

### Example: Complete Course Module

Here's a concrete example showing both bundle organization and CSS working together:

**Directory structure:**
```
content/modules/module-1-python-basics/
├── _index.md                    # Section: Module overview
├── lesson-1-intro/
│   ├── index.md                 # Leaf: Lesson 1
│   ├── setup-guide.md
│   ├── starter-code.py
│   └── images/
│       └── environment.png
├── lesson-2-data-types/
│   ├── index.md                 # Leaf: Lesson 2
│   ├── examples/
│   │   ├── integers.py
│   │   ├── strings.py
│   │   └── lists.py
│   └── worksheet.pdf
└── lesson-3-practice/
    ├── index.md                 # Leaf: Lesson 3
    ├── exercises.md
    ├── starter-code.py
    └── solutions/
        ├── exercise-1.py
        ├── exercise-2.py
        └── exercise-3.py
```

**Module overview (_index.md):**
```yaml
---
title: "Module 1: Python Basics"
weight: 1
description: "Introduction to Python programming"
learningObjectives:
  - "Set up a Python development environment"
  - "Understand basic data types"
  - "Write simple functions"
---

# Module 1: Python Basics

## What You'll Learn

This module covers the fundamentals of Python programming. By the end, you'll be able to write, test, and debug simple Python programs.

### Learning Objectives
- Set up a Python development environment
- Understand and work with basic data types (integers, strings, lists)
- Write and call functions
- Debug simple programs

**Time commitment:** 4-5 hours

See the lessons below to begin.
```

**Lesson content (lesson-1-intro/index.md):**
```yaml
---
title: "Lesson 1: Setting Up Your Environment"
weight: 1
description: "Install Python and your first IDE"
duration: "1.5 hours"
---

# Lesson 1: Setting Up Your Environment

## Objectives
- Install Python 3.x
- Install an IDE (VS Code)
- Run your first Python command

## Prerequisites
- A computer with administrative access
- 15 minutes for installation

## Getting Started

Follow the [setup guide](./setup-guide.md) for your operating system. It walks you through each step.

**Don't get stuck?** Check the checklist in the guide.

## Your First Program

Save this code to a file (e.g., `hello.py`):

```python
# Copy from: starter-code.py in this lesson
print("Hello, Python!")
```

Run it:
```bash
python hello.py
```

You should see: `Hello, Python!`

## Next Steps

Once your environment is set up, move on to [Lesson 2](../lesson-2-data-types/).
```

**CSS (static/css/custom.css):**
```css
@layer course-base {
  /* Zen handles headings, body text, etc. */
  /* Only override if behavior needs to change */
}

@layer course-components {
  /* Lesson cards when displayed in a list */
  .lesson-card {
    padding: var(--spacing-3);
    border-left: 4px solid var(--color-accent);
    background: var(--color-background-alt, white);
  }

  /* Duration badge */
  .duration {
    display: inline-block;
    padding: var(--spacing-1) var(--spacing-2);
    background: var(--color-accent);
    color: white;
    border-radius: 0.25rem;
    font-size: var(--font-size-sm);
  }

  /* Prerequisites box */
  .prerequisites {
    background: var(--color-info-light, #e3f2fd);
    border: 1px solid var(--color-info, #2196f3);
    padding: var(--spacing-2);
    margin: var(--spacing-2) 0;
  }
}
```

Notice:
- The bundle structure (module → lessons) creates the organization
- CSS is minimal because the structure is clear
- Variables (`var(--spacing-*)`, `var(--color-*)`) come from Zen
- Only course-specific needs are styled

---

## Part 7: Troubleshooting Common Issues

| Problem | Cause | Solution |
|---------|-------|----------|
| Lesson doesn't appear in module | Used `index.md` in module (should be `_index.md`) | Change to `_index.md` for section bundles |
| Resources aren't accessible | Files in wrong directory or wrong path format | Keep resources in the same directory as `index.md`; use `./filename` |
| Module list page is empty | Section bundle created but no child pages yet | Create lesson subdirectories with `index.md` files |
| CSS keeps growing | Adding rules without checking Zen's foundation first | Audit for Zen equivalents; use CSS layers |
| Relative links broken after deployment | Absolute paths used instead of relative | Use `./filename` in markdown, `.RelPermalink` in templates |
| Build includes draft content | Forgot `draft: false` | Check all front matter; set `draft: false` explicitly |

---

## Summary

**Section Bundles vs. Leaf Bundles:**
- **Section** (`_index.md`): Organizational tiers (course, modules) that list children
- **Leaf** (`index.md`): Final content pages (lessons, assignments) with resources

**For your course:**
- Use sections for modules (they group lessons)
- Use leaves for lessons (they're self-contained)
- Co-locate resources with lessons (no hunting through global folders)

**CSS with Zen:**
- Let bundle structure drive styling (good organization = simple CSS)
- Use Zen's variables and cascade first
- Override only when behavior needs to change
- Maintain restraint: every CSS rule creates technical debt
- Regular audits prevent bloat

This approach scales from a 3-module course to a comprehensive multi-module curriculum. The structure remains clear, resources stay organized, and CSS stays maintainable.

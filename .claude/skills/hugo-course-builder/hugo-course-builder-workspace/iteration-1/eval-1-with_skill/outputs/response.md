# Hugo C++ Course Site Setup Guide
## Using Zen Theme v0.161.0

This document provides complete initialization steps and content structure guidance for setting up a Hugo site for your C++ course with the Zen theme.

---

## 1. Project Initialization Steps

### Step 1: Create the Hugo Project Structure

If you haven't already initialized the basic site, run:

```bash
# Create new Hugo project
hugo new site my-cpp-course

# Enter directory
cd my-cpp-course

# Initialize git (required for theme management)
git init
```

### Step 2: Add Zen Theme as a Git Submodule

The Zen theme should be managed as a git submodule to keep it versioned separately from your content:

```bash
# Add Zen theme as submodule
git submodule add https://github.com/frjo/hugo-theme-zen.git themes/zen
```

### Step 3: Configure Your Site

Your project should have a `hugo.yaml` (or `hugo.toml`) configuration file at the root. Here's the recommended configuration for a C++ course site:

```yaml
baseURL: "https://your-domain.com/cpp-course/"
languageCode: en-us
title: "C++ Course - Scientific Computing"
theme: zen

# Disable tracking for clean builds
disableHugoGeneratorInject: true
privacy:
  googleAnalytics:
    disable: true

# Configure outputs
outputs:
  home:
    - HTML
    - RSS
    - JSON

# Define taxonomies for course organization
taxonomies:
  difficulty: difficulties
  agent: agents
  topic: topics

# Site parameters
params:
  author:
    name: "Your Name"
    email: "your.email@institution.edu"
  description: "Advanced C++ course covering modern programming practices"
  dateformat: "2 January, 2006"
  poweredby: true
  sidebar: true

# Navigation menu
menu:
  main:
    - identifier: home
      name: Home
      url: /
      weight: 1
    - identifier: modules
      name: Modules
      url: /modules/
      weight: 2
    - identifier: homework
      name: Homework
      url: /homework/
      weight: 3
    - identifier: resources
      name: Resources
      url: /resources/
      weight: 4

# Markup settings for code highlighting
markup:
  goldmark:
    renderer:
      unsafe: true
  highlight:
    style: dracula
```

### Step 4: Start the Development Server

```bash
hugo server
```

Your site will be available at `http://localhost:1313/`. Changes to content are automatically reflected.

---

## 2. Content Directory Structure

Organize your course content into three main sections as leaf and branch bundles:

```
content/
├── _index.md                    # Site homepage
├── modules/                     # Course modules (branch bundles)
│   ├── _index.md
│   ├── module-01-intro/
│   │   ├── _index.md            # Module overview
│   │   ├── lesson-01.md         # Child page
│   │   └── lesson-02.md         # Child page
│   └── module-02-advanced/
│       ├── _index.md
│       ├── lesson-01.md
│       └── lesson-02.md
├── homework/                    # Assignments (leaf bundles)
│   ├── _index.md
│   ├── assignment-01-datatypes/
│   │   ├── index.md             # Assignment content
│   │   ├── starter-code.cpp     # Resource file
│   │   └── rubric.pdf           # Resource file
│   └── assignment-02-pointers/
│       ├── index.md
│       └── pointer-guide.cpp
└── resources/                   # Reference materials (leaf bundles)
    ├── _index.md
    ├── cpp-reference/
    │   └── index.md
    └── debugging-guide/
        └── index.md
```

### Key Distinctions:

- **Modules (Branch Bundles)**: Use `_index.md` at the section level. These create list pages and can have child pages for individual lessons.
- **Homework & Resources (Leaf Bundles)**: Use `index.md` with associated files. These don't create list pages themselves but bundle resources alongside the content.
- **Custom Files**: Keep PDF rubrics, starter code, and other assets in the same directory as `index.md`.

---

## 3. Creating Modules (Section Bundles)

### Create a Module

```bash
mkdir -p content/modules/module-01-intro
cat > content/modules/module-01-intro/_index.md << 'EOF'
---
title: "Module 1: Introduction to C++"
weight: 1
description: "Fundamentals of C++, environment setup, and basic syntax"
---

## Overview

In this module, you'll learn the essentials of C++:
- Setting up a C++ development environment
- Understanding the compilation process
- Writing your first C++ program
- Basic data types and variables

Let's dive in!
EOF
```

### Add Lessons to a Module

```bash
cat > content/modules/module-01-intro/lesson-01.md << 'EOF'
---
title: "Lesson 1: Hello, World!"
weight: 1
---

The classic first program demonstrates how C++ code is compiled and executed.

## Your First Program

```cpp
#include <iostream>

int main() {
    std::cout << "Hello, World!" << std::endl;
    return 0;
}
```
EOF
```

---

## 4. Creating Assignments (Leaf Bundles)

### Create an Assignment with Front Matter

```bash
mkdir -p content/homework/assignment-01-datatypes
cat > content/homework/assignment-01-datatypes/index.md << 'EOF'
---
title: "Assignment 1: Data Types and Variables"
weight: 1
difficulty: "beginner"
agents:
  - memory
  - reasoning
topics:
  - "Primitive Data Types"
  - "Variable Declaration"
  - "Type Casting"
dueDate: 2026-05-30
description: "Master the fundamental data types in C++"
---

## Assignment Overview

In this assignment, you will practice declaring and manipulating variables of different data types.

### Requirements

1. Declare variables of types: int, float, double, char, bool
2. Initialize them with appropriate values
3. Perform type conversions
4. Output results to the console

### Submission

Submit your completed `datatypes.cpp` file via the course portal.

### Grading

See the attached rubric for evaluation criteria.
EOF
```

### Add Resources to the Assignment Bundle

```bash
# Copy starter code
cp ~/Downloads/datatypes-starter.cpp content/homework/assignment-01-datatypes/

# Copy rubric
cp ~/Downloads/rubric-assignment-01.pdf content/homework/assignment-01-datatypes/
```

In your template, access these resources using Hugo's `.Resources` variable:

```html
{{ range .Resources.ByType "text" }}
  <a href="{{ .RelPermalink }}">{{ .Title }}</a>
{{ end }}
```

---

## 5. Front Matter Templates

### Template for Modules

```yaml
---
title: "Module Name"
weight: 1
description: "What students will learn in this module"
---
```

### Template for Lessons (Child Pages)

```yaml
---
title: "Lesson Name"
weight: 1
---
```

### Template for Assignments

```yaml
---
title: "Assignment Name"
weight: 1
difficulty: "beginner"              # beginner, intermediate, advanced
agents:
  - agent-type-1                    # Memory, Reasoning, Planning, etc.
  - agent-type-2
topics:
  - "C++ Topic 1"
  - "C++ Topic 2"
dueDate: 2026-06-15                 # ISO date format
description: "Brief summary of the assignment"
---
```

### Template for Resources

```yaml
---
title: "Resource Name"
weight: 1
description: "What this resource covers"
---
```

---

## 6. Taxonomy Configuration for Filtering

Your `hugo.yaml` already defines three custom taxonomies. Use them to organize content:

### Difficulty Levels

```yaml
difficulty: "beginner"      # or intermediate, advanced
```

This automatically generates:
- `/difficulties/beginner/` - lists all beginner-level content
- `/difficulties/intermediate/` - lists intermediate content
- `/difficulties/advanced/` - lists advanced content

### Agents/Capabilities

```yaml
agents:
  - memory
  - reasoning
  - planning
```

This generates pages like `/agents/memory/` showing all content using that agent.

### C++ Topics

```yaml
topics:
  - "Pointers"
  - "Memory Management"
  - "Standard Library"
```

This generates `/topics/pointers/` and similar pages.

---

## 7. Important Hugo Rules

### Always Use ISO Dates

```yaml
# Correct
dueDate: 2026-05-30

# Incorrect (ambiguous, often fails parsing)
dueDate: 05-30-2026
```

### Taxonomy Names Must Match

The configuration uses singular form in front matter:

```yaml
# hugo.yaml
[taxonomies]
  difficulty = "difficulties"    # plural in config

# Front matter uses singular
difficulty: "beginner"            # singular in content
```

### Section vs. Leaf Bundles

- **Section bundles** (with `_index.md`): Can have child pages, create list pages
  - Use for: Modules, main categories
  - File: `_index.md`

- **Leaf bundles** (with `index.md`): No children, bundle resources
  - Use for: Assignments, individual resources
  - File: `index.md`

### Draft Content

```yaml
---
title: "Work in Progress"
draft: false              # Remove draft: true before publishing
---
```

Content with `draft: true` doesn't appear in builds.

---

## 8. CSS and Styling

### Keep Zen's Minimalism

The Zen theme provides clean, modern CSS. Before adding styles:

1. Check what Zen already provides in `themes/zen/assets/css/`
2. Leverage CSS custom properties for consistency
3. Use CSS layers to organize your additions

### Custom Stylesheet Structure

```
static/
├── css/
│   └── custom.css       # Your course-specific styles
```

Example custom CSS:

```css
@layer course-specific {
  .assignment-card {
    padding: var(--spacing-2);
    border: 1px solid var(--color-border);
    border-radius: var(--radius);
  }

  .difficulty-badge {
    display: inline-block;
    padding: 0.25rem 0.75rem;
    border-radius: var(--radius);
    font-size: 0.875rem;
    font-weight: 600;
  }

  .difficulty-badge.beginner {
    background: var(--color-success-bg);
    color: var(--color-success);
  }

  .difficulty-badge.advanced {
    background: var(--color-warning-bg);
    color: var(--color-warning);
  }
}
```

Link this in your header:

```html
<link rel="stylesheet" href="/css/custom.css">
```

---

## 9. Quick Reference Commands

```bash
# Start development server with drafts enabled
hugo server -D

# Build production site
hugo

# Create new content file
hugo new content/path/to/file.md

# Run with debug output
hugo --debug

# Check for errors
hugo --verbose

# Update Zen theme to latest version
git submodule update --remote themes/zen
```

---

## 10. Common Troubleshooting

| Problem | Cause | Solution |
|---------|-------|----------|
| Content not appearing | `draft: true` in front matter | Remove or set to `draft: false` |
| Content not appearing | Using `index.md` for a module | Use `_index.md` for section bundles |
| Taxonomies not generating | Taxonomy name mismatch | Use singular form in front matter, match config |
| Resource links broken | Files not in bundle directory | Place resources in same directory as `index.md` |
| Build errors | Relative path issues in templates | Use `.RelPermalink` and `.Resources` |
| Styles not applying | CSS specificity conflict | Use CSS layers; check Zen's existing styles |

---

## 11. Next Steps

1. **Initialize the site structure**: Create your content directories (modules, homework, resources)
2. **Add homepage content**: Create `content/_index.md`
3. **Add your first module**: Create a module section with 1-2 lessons
4. **Add sample assignments**: Create 2-3 homework items with resources
5. **Configure menu**: Update `hugo.yaml` to reflect your course structure
6. **Customize styling**: Add `static/css/custom.css` with your course branding
7. **Deploy**: Push to your hosting provider

---

## Additional Resources

- [Hugo Documentation](https://gohugo.io/documentation/)
- [Zen Theme Repository](https://github.com/frjo/hugo-theme-zen)
- [Zen Theme Demo](https://zen-demo.xdeb.org/)
- [Hugo Front Matter Guide](https://gohugo.io/content-management/front-matter/)
- [Hugo Taxonomies](https://gohugo.io/content-management/taxonomies/)

# Homework Assignment Structure for Hugo Course Builder

## Overview

Your Hugo course site is using the **Zen theme** (v0.161.0+) with custom taxonomies for course organization. For consistent homework assignment management with front matter metadata, difficulty levels, agent relationships, C++ topics, due dates, and file attachments, follow this structured approach.

## Front Matter Template for Assignments

Use this YAML template for all homework assignments. Save each assignment in a **leaf bundle** (a folder with `index.md`):

```yaml
---
title: "Assignment Name: Brief Description"
weight: 1                              # Order in lists (1, 2, 3...)
difficulty: "beginner"                 # beginner, intermediate, advanced
agents:
  - memory
  - reasoning
  - planning
topics:
  - "Variables and Types"
  - "Pointers"
  - "Dynamic Memory Allocation"
dueDate: 2025-05-15                    # ISO date format (YYYY-MM-DD)
description: "One-sentence summary for course listings"
draft: false                           # Must be false to appear in builds
---
```

### Field Descriptions

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `title` | string | Yes | Descriptive title with assignment number |
| `weight` | integer | Yes | Controls sort order in lists (ascending) |
| `difficulty` | string | Yes | One of: `beginner`, `intermediate`, `advanced` |
| `agents` | array | Yes | List of agent types (memory, reasoning, planning, etc.) |
| `topics` | array | Yes | List of C++ topics covered in this assignment |
| `dueDate` | date | Yes | ISO format: `YYYY-MM-DD` (e.g., `2025-05-15`) |
| `description` | string | Optional | Brief summary for listings and indexes |
| `draft` | boolean | No | Set to `false` (or omit); `true` hides the page |

---

## Directory Structure for Assignments

Organize all homework assignments in **leaf bundles** under `content/homework/`:

```
content/
└── homework/
    ├── assignment-01/
    │   ├── index.md                 # Assignment page (front matter + description)
    │   ├── starter-code.cpp         # Attached C++ file
    │   ├── rubric.pdf               # Grading rubric
    │   └── example-output.txt       # Expected output reference
    │
    ├── assignment-02/
    │   ├── index.md
    │   ├── assignment-02-starter.cpp
    │   ├── assignment-02.pdf        # Assignment PDF
    │   └── test-cases.cpp
    │
    └── assignment-03/
        ├── index.md
        ├── header-file.h
        ├── implementation.cpp
        └── specification.pdf
```

### Why Leaf Bundles?

- **Resources stay together**: PDFs, code files, and other attachments live in the same directory as the assignment
- **Hugo `.Resources` access**: Templates can reference attached files without hardcoding paths
- **Portability**: Moving or renaming a bundle preserves all relationships
- **Clean URLs**: Assignment pages render at `/homework/assignment-01/`

---

## Example Assignment

Create `content/homework/assignment-01/index.md`:

```yaml
---
title: "Assignment 1: Basic Data Types and Memory"
weight: 1
difficulty: "beginner"
agents:
  - memory
topics:
  - "Integers and Types"
  - "Pointers"
  - "Memory Addresses"
dueDate: 2025-05-22
description: "Explore basic C++ data types, declare variables, and understand pointer syntax."
draft: false
---

## Overview

In this assignment, you will:
1. Declare and initialize various C++ data types
2. Use the address-of operator (`&`) to find variable addresses
3. Create and dereference pointers
4. Understand pointer arithmetic

## Requirements

- Write a program that declares variables of different types
- Use `cout` to display both the value and memory address of each variable
- Demonstrate pointer declarations, assignments, and dereferencing
- Test pointer arithmetic with an array

## Submission

Submit your `.cpp` file. The starter code is provided in `starter-code.cpp`.

## Evaluation

See `rubric.pdf` for grading criteria.
```

Then add your attached files:
```bash
cp ~/Downloads/starter-code.cpp content/homework/assignment-01/
cp ~/Downloads/rubric.pdf content/homework/assignment-01/
```

---

## Accessing Attached Files in Templates

Hugo's `.Resources` collection makes all files in a bundle available to templates.

### In Your Assignment Template (`layouts/homework/single.html`)

```html
<section class="assignment-details">
  <h1>{{ .Title }}</h1>
  
  <div class="metadata">
    <p><strong>Difficulty:</strong> {{ .Params.difficulty }}</p>
    <p><strong>Due Date:</strong> {{ .Params.dueDate | dateFormat "2 January, 2006" }}</p>
    <p><strong>Topics:</strong> {{ range .Params.topics }}{{ . }}, {{ end }}</p>
  </div>

  <div class="content">
    {{ .Content }}
  </div>

  {{ if .Resources }}
  <section class="assignment-resources">
    <h2>Attachments</h2>
    <ul>
    {{ range .Resources }}
      <li>
        <a href="{{ .RelPermalink }}" download>
          {{ .Title }} ({{ .MediaType }})
        </a>
      </li>
    {{ end }}
    </ul>
  </section>
  {{ end }}
</section>
```

### Benefits

- **No hardcoding**: If you rename a file, the link updates automatically
- **Type detection**: Hugo knows file types (PDF, CPP, TXT, etc.)
- **Relative URLs**: `.RelPermalink` creates portable links

---

## Taxonomy Configuration

Define custom taxonomies in your `hugo.yaml`:

```yaml
taxonomies:
  difficulty: "difficulties"
  agent: "agents"
  topic: "topics"
```

This enables:
- **Automatic pages** at `/difficulties/beginner/`, `/agents/memory/`, `/topics/variables-and-types/`
- **Filtering** in templates to group assignments by difficulty, agent type, or topic
- **No manual management**: Hugo builds these pages automatically from your front matter

### Example: Filter Assignments by Difficulty

In `layouts/difficulties/single.html`:

```html
<h1>{{ .Title }} Assignments</h1>
<ul>
{{ range .Pages }}
  <li>
    <a href="{{ .RelPermalink }}">{{ .Title }}</a>
    <span class="due-date">Due: {{ .Params.dueDate | dateFormat "Jan 2" }}</span>
  </li>
{{ end }}
</ul>
```

---

## Best Practices

### 1. **Use ISO Dates Consistently**

```yaml
# ✓ Correct
dueDate: 2025-05-15

# ✗ Wrong (ambiguous, parsing may fail)
dueDate: 05-15-2025
dueDate: "May 15, 2025"
```

### 2. **Keep Taxonomy Names Singular in Front Matter**

```yaml
# In hugo.yaml (config)
taxonomies:
  difficulty: "difficulties"    # Plural: this is the list page name

# In front matter (use singular)
difficulty: "beginner"          # ✓ Correct
difficulties: "beginner"        # ✗ Wrong
```

### 3. **Use `index.md` for Assignments (Not `_index.md`)**

- `index.md` → Leaf bundle (single page, no children) — **USE FOR ASSIGNMENTS**
- `_index.md` → Section bundle (list page with children) — use for modules/sections

### 4. **Always Set `draft: false`**

Pages with `draft: true` are hidden in production builds:

```yaml
---
title: "My Assignment"
draft: false              # Essential!
---
```

### 5. **Organize Attachments by Type**

For complex assignments, name files clearly:

```
assignment-05/
├── index.md
├── spec.pdf              # Problem specification
├── starter-main.cpp      # Main entry point
├── starter-utils.h       # Utility functions header
├── starter-utils.cpp     # Utility implementations
├── rubric.pdf
└── test-input.txt        # Sample input file
```

---

## Listing Assignments in Your Site Menu

Add a homework listing page at `content/homework/_index.md`:

```yaml
---
title: "Homework Assignments"
description: "All course assignments with due dates and specifications"
draft: false
---

Browse assignments by difficulty level or topic.
```

Then create `layouts/homework/list.html` to display all assignments:

```html
<h1>{{ .Title }}</h1>

<div class="assignments-grid">
{{ range .Pages.ByParam "weight" }}
  <article class="assignment-card">
    <h2><a href="{{ .RelPermalink }}">{{ .Title }}</a></h2>
    <p>{{ .Params.description }}</p>
    <div class="tags">
      <span class="difficulty {{ .Params.difficulty }}">{{ .Params.difficulty }}</span>
      <span class="due-date">Due: {{ .Params.dueDate | dateFormat "Jan 2" }}</span>
    </div>
  </article>
{{ end }}
</div>
```

---

## Quick Workflow for Adding an Assignment

1. **Create the directory:**
   ```bash
   mkdir -p content/homework/assignment-XX
   ```

2. **Create `index.md` with front matter:**
   ```bash
   cat > content/homework/assignment-XX/index.md << 'EOF'
   ---
   title: "Assignment XX: Title"
   weight: XX
   difficulty: "beginner"
   agents:
     - memory
   topics:
     - "Topic 1"
   dueDate: 2025-06-15
   description: "Brief description"
   draft: false
   ---
   
   ## Assignment Description
   
   Content here...
   EOF
   ```

3. **Add attached files:**
   ```bash
   cp ~/Downloads/starter.cpp content/homework/assignment-XX/
   cp ~/Downloads/rubric.pdf content/homework/assignment-XX/
   ```

4. **Preview:**
   ```bash
   hugo server
   ```
   Visit `http://localhost:1313/homework/assignment-xx/`

5. **Deploy:**
   ```bash
   hugo        # Builds to ./public/
   ```

---

## Taxonomy Values to Use

### Difficulty Levels

- `beginner` — Foundational concepts, 1–2 hours
- `intermediate` — Building on basics, 3–5 hours
- `advanced` — Complex multi-step problems, 5+ hours

### Agent Types

Common agent categories for your assignments:

- `memory` — Requires understanding stored data, addresses, or state
- `reasoning` — Requires logical deduction or algorithm design
- `planning` — Requires decomposing the problem into steps
- `debugging` — Requires error identification and fixing
- `testing` — Requires writing and validating test cases

You can add more agent types as needed. Define them in your YAML and they'll auto-generate taxonomy pages.

### C++ Topics

Examples of topics to include:

- "Variables and Types"
- "Pointers"
- "Dynamic Memory"
- "Arrays"
- "Functions"
- "Classes and Objects"
- "Inheritance"
- "Templates"
- "File I/O"
- "Standard Library"

---

## Summary

Your homework structure uses:

1. **Leaf bundles** in `content/homework/assignment-XX/` with `index.md`
2. **Consistent YAML front matter** with title, difficulty, agents, topics, due date
3. **Attached files** (PDFs, code, examples) stored alongside the markdown
4. **Taxonomies** (difficulty, agents, topics) for automatic indexing and filtering
5. **Templates** to render assignments and list them by category

This approach ensures consistency, portability, and makes your course site maintainable as it grows.

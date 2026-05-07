# Hugo Bundles for Course Module Organization

## Quick Answer

**Section bundles** (branch bundles) contain `_index.md` and are designed to have child pages underneath them—perfect for your modules with multiple lessons. **Leaf bundles** use `index.md` and have no children, but can bundle resources (code examples, diagrams, PDFs) alongside their content—perfect for individual lessons or assignments that need associated files.

---

## Section Bundles vs. Leaf Bundles

### Section Bundles (Branch Bundles)

**Definition:** A section is a directory containing `_index.md`. It creates a list page and can contain child pages.

**File structure:**
```
content/modules/module-01/
├── _index.md              # Section definition (generates list page)
├── lesson-01.md           # Child page
├── lesson-02.md           # Child page
└── lesson-03.md           # Child page
```

**Key properties:**
- Uses `_index.md` (underscore prefix required)
- Generates a list page automatically at `/modules/module-01/`
- Child pages are automatically discovered and can be listed/sorted by weight
- No associated resources stored in the bundle folder itself
- Use for: Module groupings, units, or any container that lists its children

**Front matter example:**
```yaml
---
title: "Module 1: Introduction to C++"
weight: 1
description: "Learn the fundamentals of C++ programming"
---
```

---

### Leaf Bundles (Page Bundles)

**Definition:** A leaf bundle is a directory containing `index.md` (not `_index.md`). It's a terminal page with no child pages.

**File structure:**
```
content/homework/assignment-01/
├── index.md               # Leaf page definition (no list page)
├── starter-code.cpp       # Associated resource
├── rubric.pdf             # Associated resource
└── expected-output.txt    # Associated resource
```

**Key properties:**
- Uses `index.md` (no underscore)
- Does NOT generate a list page
- Resources stored in the same directory are accessible via Hugo's `.Resources` method
- Use for: Individual assignments, lessons with files, reference documents

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
---
```

---

## How to Apply Them to Your Course Structure

### Recommended Organization Pattern

```
content/
├── modules/                      # Section bundles
│   ├── module-01/
│   │   ├── _index.md            # "Module 1: Introduction"
│   │   ├── lesson-01.md         # Child page (concept)
│   │   ├── lesson-02.md         # Child page (practice)
│   │   └── lesson-03.md         # Child page (challenge)
│   ├── module-02/
│   │   ├── _index.md
│   │   ├── lesson-01.md
│   │   └── lesson-02.md
│
├── homework/                     # Leaf bundles
│   ├── assignment-01/
│   │   ├── index.md             # Assignment page
│   │   ├── starter-code.cpp     # Resource
│   │   ├── rubric.pdf           # Resource
│   │   └── test-cases.txt       # Resource
│   ├── assignment-02/
│   │   ├── index.md
│   │   ├── starter-code.cpp
│   │   └── rubric.pdf
│
└── resources/                    # Leaf bundles
    ├── c-plus-plus-reference/
    │   ├── index.md
    │   ├── syntax-guide.pdf
    │   └── standard-library.md
```

---

## Key Decision Points

### Use a Section Bundle (with `_index.md`) When:

1. **You need a container page** that lists/summarizes its children
2. **You want child pages grouped together** with shared context
3. **Child pages should appear in a hierarchy** (e.g., a module's lessons as a submenu)
4. **You're building a course structure** with modules → lessons progression

**Example:** Module 1 has 3 lessons. The module's `_index.md` provides an overview; lesson pages provide detail.

### Use a Leaf Bundle (with `index.md`) When:

1. **The page has associated files** (code, diagrams, PDFs, images)
2. **Resources must stay with the content** and move together
3. **You need template access to those files** via `.Resources`
4. **The page is a terminal node** with no child pages underneath

**Example:** An assignment needs a starter code file and a rubric PDF. Both go in the assignment's leaf bundle.

---

## Keeping Resources with Lessons

If individual lessons have code examples or diagrams, **convert lesson pages to leaf bundles**:

**Before (plain pages):**
```
content/modules/module-01/
├── _index.md
├── lesson-01.md
├── lesson-02.md
```

**After (lesson as leaf bundle with resources):**
```
content/modules/module-01/
├── _index.md              # Module overview
└── lesson-01/
    ├── index.md           # Lesson content
    ├── example-code.cpp   # Resource
    └── diagram.png        # Resource
```

**In `lesson-01/index.md`**, access the resources:
```html
{{ range .Resources }}
  <a href="{{ .RelPermalink }}">{{ .Name }}</a>
{{ end }}
```

---

## Critical Rules

### 1. Naming: Underscore Matters

| Type | Filename | Purpose |
|------|----------|---------|
| **Section** | `_index.md` | Creates list page, can have children |
| **Leaf** | `index.md` | Terminal page, no list page |

Using the wrong name silently breaks Hugo's behavior. A section with `index.md` won't generate a list page; a leaf with `_index.md` won't be treated as a leaf.

### 2. Taxonomy Names in Front Matter

In `hugo.toml`:
```toml
[taxonomies]
  difficulty = "difficulties"
  agent = "agents"
  topic = "topics"
```

In front matter, use the **singular form** (the key, not the value):
```yaml
difficulty: "beginner"        # ✓ Correct
difficulties: "beginner"      # ✗ Wrong (won't work)
```

### 3. Date Format

Always use ISO format:
```yaml
dueDate: 2025-05-15           # ✓ Correct
dueDate: 05-15-2025           # ✗ Ambiguous, often fails
```

### 4. Draft Content is Hidden

```yaml
---
title: "My Page"
draft: false              # Remove before publishing!
---
```

---

## Template Access to Bundle Resources

Once resources are in a leaf bundle, your templates can list and link them:

```html
<!-- In any template for a page with resources -->
<section class="resources">
  <h2>Downloads</h2>
  <ul>
  {{ range .Resources }}
    <li>
      <a href="{{ .RelPermalink }}">{{ .Title }}</a>
    </li>
  {{ end }}
  </ul>
</section>
```

This works because:
- Resources are discovered automatically from the bundle directory
- `.RelPermalink` gives the correct URL relative to the site root
- No hardcoding of paths needed

---

## Summary

| Aspect | Section Bundle | Leaf Bundle |
|--------|---|---|
| **Filename** | `_index.md` | `index.md` |
| **Purpose** | Container with children | Terminal page |
| **List page** | Yes, generated | No |
| **Resources** | Not bundled | Stored locally, accessible via `.Resources` |
| **Use for** | Modules, units, chapters | Assignments, lessons with files, resources |
| **Child pages** | Yes | No |

**For your course:** Use section bundles for modules (module-01 with its lessons), and convert individual lessons to leaf bundles if they need code examples or diagrams. Assignments should always be leaf bundles so code and rubrics stay together.

# Hugo Bundles for Course Module Organization

## Overview

Hugo bundles are a powerful organizational pattern that allows you to keep related content and resources together in a single directory structure. Understanding the difference between section bundles and leaf bundles is essential for designing a course structure that's both manageable and maintainable.

## Key Differences: Section Bundles vs. Leaf Bundles

### Section Bundles

A **section bundle** is a directory that contains an `_index.md` file and other subdirectories (which may be leaf bundles or subsections). Section bundles represent **container pages** that typically display a listing or overview of child content.

**Characteristics:**
- Named directory with `_index.md` file (not just `index.md`)
- Can contain subdirectories with other bundle types
- Generates a URL path with the directory name
- Renders as a list page or overview by default
- Can have its own content, metadata, and resources
- Ideal for organizational hierarchy levels

**Example structure:**
```
content/
├── modules/
│   ├── _index.md              # Section bundle (modules overview)
│   ├── module-1/
│   │   ├── _index.md          # Section bundle (Module 1 overview)
│   │   ├── lesson-1/
│   │   │   └── index.md       # Leaf bundle (Lesson 1)
│   │   │   └── example.js
│   │   │   └── diagram.svg
│   │   └── lesson-2/
│   │       └── index.md       # Leaf bundle (Lesson 2)
│   │       └── code-sample.py
```

### Leaf Bundles

A **leaf bundle** is a directory containing an `index.md` file (not `_index.md`) that represents a **final content page** with no children. Leaf bundles are the endpoints of your content hierarchy.

**Characteristics:**
- Named directory with `index.md` file (not `_index.md`)
- Cannot have subdirectories with additional content
- All resources (images, code files, documents) live in the same directory
- Generates a clean URL path (`/module-1/lesson-1/`)
- Renders as a single page with its own template
- Resources are co-located and can be referenced with relative paths
- Ideal for self-contained learning units

**Example structure:**
```
content/modules/module-1/lesson-1/
├── index.md                   # Leaf bundle content
├── example-code.js
├── diagram.svg
├── notes.pdf
└── quiz.json
```

## Recommended Course Structure

For a typical course with modules containing multiple lessons, here's the recommended hierarchy:

```
content/
├── _index.md                           # Section bundle: Course home
├── about/
│   └── index.md                        # Leaf bundle: About page
├── syllabus/
│   └── index.md                        # Leaf bundle: Course syllabus
└── modules/
    ├── _index.md                       # Section bundle: Modules list page
    ├── module-1-introduction/
    │   ├── _index.md                   # Section bundle: Module overview
    │   ├── lesson-1-getting-started/
    │   │   ├── index.md                # Leaf bundle: Lesson content
    │   │   ├── setup-guide.md
    │   │   ├── starter-code.js
    │   │   ├── diagram-architecture.svg
    │   │   └── resources.json
    │   ├── lesson-2-core-concepts/
    │   │   ├── index.md                # Leaf bundle: Lesson content
    │   │   ├── concept-diagram.svg
    │   │   ├── examples/
    │   │   │   ├── example-1.py
    │   │   │   ├── example-2.py
    │   │   │   └── example-3.py
    │   │   ├── quiz.json
    │   │   └── references.md
    │   └── lesson-3-practice/
    │       ├── index.md                # Leaf bundle: Lesson content
    │       ├── exercises.md
    │       └── solutions/
    │           ├── solution-1.py
    │           ├── solution-2.py
    │           └── solution-3.py
    ├── module-2-advanced-topics/
    │   ├── _index.md                   # Section bundle: Module overview
    │   ├── lesson-1-topic-a/
    │   │   ├── index.md                # Leaf bundle
    │   │   └── ...resources...
    │   └── lesson-2-topic-b/
    │       ├── index.md                # Leaf bundle
    │       └── ...resources...
```

## Application Guidelines

### Use Section Bundles For:

1. **Course-level organization** (`modules/_index.md`)
   - Displays all modules
   - Can have course overview content
   - Metadata about the entire course

2. **Module-level grouping** (`modules/module-1/_index.md`)
   - Overview of the module
   - Learning objectives
   - List of lessons within the module
   - Module-level resources (syllabus, prerequisites, etc.)

3. **Organizational tiers** where you need an overview or list page
   - Any level that needs to show children in a browsable way
   - Any level that has its own content/metadata plus child pages

### Use Leaf Bundles For:

1. **Individual lessons** - each lesson is self-contained
   - The lesson content in `index.md`
   - Code examples specific to that lesson
   - Diagrams and assets for that lesson
   - Exercises or quizzes for that lesson
   - Related markdown documents for that lesson

2. **Terminal content pages** - pages with no child pages beneath them
   - About page, syllabus, contact, etc.
   - Any content that stands alone

3. **Self-contained units** with associated resources
   - All related resources live together
   - Easy to copy/move/reference entire units
   - Clean relative paths (e.g., `./example.js` instead of complex relative paths)

## Benefits of This Structure

### Organizational Benefits:
- **Clear hierarchy**: Easily understand content organization at a glance
- **Scalability**: Add new modules and lessons without restructuring
- **Discoverability**: Resources live with the content they support
- **Maintainability**: Co-located files make updates easier

### Asset Management Benefits:
- **Relative paths**: Use `![diagram](./diagram.svg)` instead of absolute paths
- **Bundled resources**: All assets for a lesson travel together
- **No conflicts**: Lesson names can repeat across modules without file conflicts
- **Easy portability**: Move an entire lesson (with all resources) by moving one directory

### URL Structure Benefits:
- **Clean URLs**: `/modules/module-1/lesson-1/` is human-readable
- **Logical navigation**: URL structure mirrors content hierarchy
- **Predictable routing**: Understanding the directory structure predicts the URL

## Implementation Tips

### In Your Lesson Content:

```markdown
# Getting Started with Python

## Overview
This lesson covers the basics of Python setup and your first program.

## Resources
- [Starter Code](./starter-code.py) - Reference the file in the same bundle
- [Architecture Diagram](./diagram-architecture.svg)
- [Setup Guide](./setup-guide.md)

## Example Code

See the example files in this lesson directory.
```

### Configure Hugo Correctly:

Ensure your Hugo config recognizes both bundle types in `hugo.yaml`:

```yaml
params:
  # Display settings for modules and lessons
  defaultContentLanguage: en
  
# Ensure proper content organization
outputs:
  home:
    - HTML
    - JSON
  section:
    - HTML
    - JSON
  page:
    - HTML
```

### Template Considerations:

- **For section bundles** (`_index.md`): Create templates that list child pages
  - Display all lessons in a module
  - Show progress or completion status
  - Provide module overview content

- **For leaf bundles** (`index.md`): Create templates for lesson display
  - Full lesson content with all formatting
  - Sidebar with related resources
  - Navigation to next/previous lessons
  - Code highlighting and syntax support

## Common Patterns

### Pattern 1: Simple Module with 2-3 Lessons
```
module-basics/
├── _index.md          # Module overview with learning objectives
├── lesson-1/
│   ├── index.md       # Lesson 1 content
│   ├── code.js
│   └── quiz.json
├── lesson-2/
│   ├── index.md       # Lesson 2 content
│   ├── diagram.svg
│   └── resources.md
```

### Pattern 2: Complex Module with Subsections
```
module-advanced/
├── _index.md                # Module overview
├── part-1-fundamentals/
│   ├── _index.md            # Subsection overview
│   ├── lesson-1/
│   │   ├── index.md         # Leaf bundle
│   │   └── assets/
│   │       ├── code.py
│   │       └── diagram.png
│   └── lesson-2/
│       └── index.md         # Leaf bundle
└── part-2-applications/
    ├── _index.md            # Subsection overview
    └── lesson-3/
        └── index.md         # Leaf bundle
```

### Pattern 3: Lesson with Multiple Resource Types
```
lesson-project/
├── index.md                 # Main lesson content
├── brief.md                 # Project brief
├── requirements.md          # Requirements document
├── code-starter.py          # Starting code template
├── code-solution.py         # Complete solution
├── images/
│   ├── diagram-workflow.svg
│   ├── diagram-architecture.png
│   └── screenshot.png
└── resources/
    ├── reference-1.md
    └── reference-2.md
```

## Migration Considerations

If you have an existing Hugo site:

1. **Identify leaf content**: Pages with no children should become leaf bundles
2. **Identify hierarchy tiers**: Pages that list/organize children become section bundles
3. **Migrate assets**: Move related images/files into the bundle directories
4. **Update links**: Change references from absolute paths to relative paths where possible
5. **Test thoroughly**: Verify all URLs resolve correctly after migration

## Summary

- **Section bundles** (`_index.md`): Use for organizational levels that need overview/list pages (modules, subsections)
- **Leaf bundles** (`index.md`): Use for final content endpoints with co-located resources (individual lessons)
- For a course: **sections for modules**, **leaves for lessons**
- Benefits: cleaner organization, co-located resources, maintainable structure, predictable URLs

This structure scales well from a simple 3-module course to a comprehensive multi-module curriculum with dozens of lessons.

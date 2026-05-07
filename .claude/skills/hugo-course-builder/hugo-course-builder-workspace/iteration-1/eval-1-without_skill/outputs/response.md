# Hugo Site Setup for C++ Course with Zen Theme v0.161.0

This guide provides complete initialization steps and content structure recommendations for building a course website with Hugo and the Zen theme. It covers project setup, configuration, content architecture, and best practices specific to educational sites.

## Part 1: Project Initialization

### Step 1: Create and Initialize the Hugo Project

```bash
# Create new Hugo project
hugo new site my-cpp-course

# Enter the project directory
cd my-cpp-course

# Initialize git (required for theme management)
git init

# Add the Zen theme as a git submodule (pinned to v0.161.0)
git submodule add https://github.com/frjo/hugo-theme-zen.git themes/zen
cd themes/zen
git checkout v0.161.0
cd ../..
```

**Why this approach:**
- Git submodules keep the theme versioned separately, preventing accidental modifications
- Using a specific version tag ensures reproducibility across development and deployment
- Submodules make it easy to update the theme later without modifying your course content

### Step 2: Create the Hugo Configuration File

Create `hugo.yaml` (or `hugo.toml`) in your project root with these essential settings:

```yaml
baseURL: "https://your-domain.edu/cpp-course/"
languageCode: en-us
title: "ISC5305 - C++ Programming Course"
author: "Your Name"
description: "Graduate-level course on C++ programming and scientific computing"
theme: zen
pluralizelisttitles: false
removePathAccents: true

# Disable tracking to avoid theme compatibility issues
disableHugoGeneratorInject: true
privacy:
  googleAnalytics:
    disable: true

# Output formats for home page
outputs:
  home:
    - HTML
    - RSS
    - JSON

# Custom taxonomies for course organization
taxonomies:
  difficulty: "difficulties"
  agent: "agents"
  topic: "topics"

# Theme parameters
params:
  contact: "your-email@university.edu"
  description: "C++ Programming and Scientific Computing"
  dateformat: "2 January, 2006"
  favicon: "apple-touch-icon.png"
  feedlinks: true
  icon: "apple-touch-icon.png"
  image: "apple-touch-icon.png"
  imageMaxWidth: 400
  logo: false
  logoWidth: 64
  mobileMenu: true
  poweredby: true
  sidebar: true
  submitted: true
  ada_min_height: 44
  author:
    name: "Your Name"
    url: "https://your-faculty-page.edu/"
    avatar: "path/to/avatar.jpg"

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

# Markup processing configuration
markup:
  goldmark:
    renderer:
      unsafe: true  # Allow raw HTML in markdown
  highlight:
    style: dracula  # Code block syntax highlighting
```

**Configuration Notes:**
- `theme: zen` tells Hugo to use the Zen theme from the `themes/zen` directory
- Taxonomies enable filtering by difficulty, agent types (for AI/Claude), and C++ topics
- The menu items should match your content section directories
- The `unsafe: true` setting for Goldmark allows embedded HTML/SVN in markdown files

### Step 3: Initialize the Content Directory Structure

```bash
# Create main content directories
mkdir -p content/modules
mkdir -p content/homework
mkdir -p content/resources

# Start the development server
hugo server
```

At this point, you should see the Zen theme's default home page. The server watches for file changes and rebuilds automatically (default: http://localhost:1313).

---

## Part 2: Content Directory Structure

### Overview of the Three-Section Architecture

```
content/
├── modules/           # Course modules (section bundles with child pages)
│   ├── module-01/
│   │   ├── _index.md           # Module overview and list page
│   │   ├── lesson-01.md        # Child pages (lessons within module)
│   │   └── lesson-02.md
│   └── module-02/
│       ├── _index.md
│       └── lesson-01.md
│
├── homework/          # Assignments (leaf bundles with resources)
│   ├── assignment-01/
│   │   ├── index.md            # Assignment page
│   │   ├── starter-code.cpp    # Code template
│   │   └── rubric.pdf          # Grading rubric
│   └── assignment-02/
│       ├── index.md
│       ├── helper-functions.h
│       └── test-cases.cpp
│
└── resources/         # Reference materials (leaf bundles)
    ├── cpp-reference/
    │   ├── index.md            # Reference overview
    │   └── reference-guide.pdf
    └── setup-guide/
        ├── index.md
        └── installation-steps.md
```

### Why This Structure Works

- **Modules as section bundles:** Use `_index.md` at the module level to create a list page showing all lessons within that module. Each lesson is a sibling page that inherits the module's hierarchy.
- **Assignments and resources as leaf bundles:** Use `index.md` (not `_index.md`) to prevent these from creating list pages. Associated files (PDFs, code samples) live in the same directory as the index.
- **Clear separation:** Three top-level sections make navigation and organization intuitive for both students and instructors.

---

## Part 3: Creating Content Pages

### Creating a Module with Lessons

**Step 1: Create the module section**

```bash
mkdir -p content/modules/module-01-intro
```

**Step 2: Create the module index (_index.md)**

File: `content/modules/module-01-intro/_index.md`

```yaml
---
title: "Module 1: Introduction to C++"
weight: 1
description: "Learn the fundamentals of C++, including syntax, variables, data types, and control flow."
---

## Module Overview

This module introduces students to the C++ programming language, focusing on:

- Basic syntax and structure
- Variables and data types
- Input/output operations
- Control flow (if/else, loops)

By the end of this module, you'll be able to write simple C++ programs and understand fundamental language concepts.

## Topics Covered

- Variables and primitive types (int, float, double, char)
- Operators and expressions
- Conditional statements
- Loops and iteration

## Learning Outcomes

After completing this module, students will be able to:
1. Write syntactically correct C++ programs
2. Understand and use basic data types appropriately
3. Control program flow with conditionals and loops
```

**Step 3: Create lesson pages as siblings**

File: `content/modules/module-01-intro/lesson-01-syntax.md`

```yaml
---
title: "Lesson 1: C++ Syntax and Your First Program"
weight: 1
---

## Hello, C++!

In this lesson, we'll write your first C++ program and understand the basic structure.

### The Basic Program Structure

```cpp
#include <iostream>

int main() {
    std::cout << "Hello, World!" << std::endl;
    return 0;
}
```

### Line-by-Line Explanation

- `#include <iostream>`: Include the input/output library
- `int main()`: The entry point of every C++ program
- `std::cout`: Standard output stream
- `std::endl`: End line and flush the stream
- `return 0;`: Exit code (0 = success)

### Key Concepts

**Preprocessor Directives** — Lines starting with `#` are processed before compilation.

**Functions** — A function groups code that performs a specific task. `main()` is special—it's where every C++ program starts.

**Namespaces** — `std::` indicates "standard namespace." It prevents name conflicts.
```

File: `content/modules/module-01-intro/lesson-02-variables.md`

```yaml
---
title: "Lesson 2: Variables and Data Types"
weight: 2
---

## Understanding Variables

A variable is a named storage location in memory that holds a value.

### Declaring Variables

```cpp
int age = 25;           // Integer
double height = 5.9;    // Floating-point
char grade = 'A';       // Single character
bool isStudent = true;  // Boolean
```

### Data Types in C++

| Type | Size | Range | Example |
|------|------|-------|---------|
| `int` | 4 bytes | -2,147,483,648 to 2,147,483,647 | 42 |
| `double` | 8 bytes | Very large range with decimals | 3.14159 |
| `char` | 1 byte | Single ASCII character | 'A' |
| `bool` | 1 byte | true or false | true |

### Naming Conventions

Use descriptive variable names in `camelCase`:
- Good: `studentGrade`, `maxIterations`, `isValid`
- Avoid: `x`, `temp`, `data1`
```

**Why this structure:**
- The `_index.md` file creates a module landing page that lists all lessons
- Sibling `.md` files (lesson-01.md, lesson-02.md) are child pages within the module
- The `weight` parameter controls ordering (lower weight appears first)
- Navigation automatically shows the module structure

### Creating Homework Assignments

File: `content/homework/assignment-01/index.md`

```yaml
---
title: "Assignment 1: Basic I/O and Variables"
weight: 1
difficulty: "beginner"
agents:
  - memory
  - reasoning
topics:
  - "Variables"
  - "Data Types"
  - "Input/Output"
dueDate: 2025-05-15
description: "Write programs to practice variable declaration, I/O operations, and type conversions."
---

## Assignment 1: Basic I/O and Variables

**Due:** May 15, 2025

### Objective

Practice fundamental C++ concepts: declaring variables, reading input, and producing output.

### Problem 1: Temperature Conversion

Write a program that converts temperature from Celsius to Fahrenheit.

**Requirements:**
- Read a temperature in Celsius from the user
- Convert using the formula: F = (C × 9/5) + 32
- Display the result with one decimal place
- Use appropriate variable names

**Sample Output:**
```
Enter temperature in Celsius: 25
Temperature in Fahrenheit: 77.0
```

### Problem 2: Simple Calculator

Write a program that performs basic arithmetic.

**Requirements:**
- Read two numbers and an operator (+, -, *, /)
- Perform the calculation
- Handle division by zero with an error message
- Display the result

### Submission Guidelines

- Name your files `problem1.cpp` and `problem2.cpp`
- Include comments explaining your logic
- Test your programs with multiple inputs
- Submit as a `.zip` file

### Grading Rubric

See the attached `rubric.pdf` for detailed grading criteria.
```

**Step: Add supporting files to the assignment bundle**

Place these files in `content/homework/assignment-01/`:

File: `content/homework/assignment-01/starter-code.cpp`

```cpp
#include <iostream>
using namespace std;

int main() {
    // Write your solution here
    
    return 0;
}
```

File: `content/homework/assignment-01/rubric.pdf` (place actual PDF here)

By placing files in the bundle directory, Hugo's `.Resources` makes them accessible in templates without hardcoded paths.

### Creating Resources

File: `content/resources/cpp-reference/index.md`

```yaml
---
title: "C++ Standard Library Reference"
weight: 1
description: "Quick reference for commonly used C++ standard library functions and classes."
---

## C++ Standard Library Reference

A quick lookup guide for functions and classes you'll use throughout the course.

### Input/Output (iostream)

| Function | Purpose | Example |
|----------|---------|---------|
| `std::cin >>` | Read from input | `cin >> x;` |
| `std::cout <<` | Write to output | `cout << x;` |
| `std::endl` | End line and flush | `cout << "Done" << endl;` |
| `std::getline()` | Read a full line | `getline(cin, str);` |

### String Operations (string)

| Function | Purpose | Example |
|----------|---------|---------|
| `.length()` | Get string length | `str.length()` |
| `.substr()` | Extract substring | `str.substr(0, 5)` |
| `.find()` | Find substring | `str.find("hello")` |
| `.append()` | Concatenate strings | `str.append(" world")` |

### Mathematical Functions (cmath)

| Function | Purpose | Example |
|----------|---------|---------|
| `sqrt()` | Square root | `sqrt(16) = 4` |
| `pow()` | Power | `pow(2, 3) = 8` |
| `abs()` | Absolute value | `abs(-5) = 5` |
| `sin()`, `cos()`, `tan()` | Trigonometric | `sin(3.14159)` |
```

---

## Part 4: Front Matter Best Practices

### YAML Front Matter Template for Assignments

```yaml
---
title: "Assignment Name"
weight: 1                           # Controls ordering (lower = earlier)
difficulty: "beginner"              # beginner, intermediate, advanced
agents:
  - memory                          # AI agent types used in grading
  - reasoning
topics:
  - "C++ Topic 1"
  - "C++ Topic 2"
dueDate: 2025-05-15                 # ISO 8601 format
description: "One-sentence summary"
draft: false                        # Set to false to publish; true to hide
---
```

### YAML Front Matter Template for Modules

```yaml
---
title: "Module Name"
weight: 1
description: "What students will learn in this module"
draft: false
---
```

### YAML Front Matter Template for Resources

```yaml
---
title: "Resource Title"
weight: 1
description: "Brief description"
draft: false
---
```

**Important Notes:**
- Use ISO 8601 date format (`YYYY-MM-DD`), never `MM-DD-YYYY`
- Taxonomy names in front matter use singular form (not plural)
- `draft: false` is essential; pages with `draft: true` don't appear in builds
- Custom fields (`difficulty`, `agents`, `topics`) are accessible in templates as `.Params.fieldname`

---

## Part 5: Using Taxonomies for Filtering

Taxonomies automatically generate listing pages without custom code.

### Defined Taxonomies

In your `hugo.yaml`:

```yaml
taxonomies:
  difficulty: "difficulties"  # Taxonomy: difficulty, list page: /difficulties/
  agent: "agents"             # Taxonomy: agent, list page: /agents/
  topic: "topics"             # Taxonomy: topic, list page: /topics/
```

### Access Taxonomy Pages

Once defined, Hugo generates these pages automatically:

- `/difficulties/beginner/` — All assignments with `difficulty: "beginner"`
- `/agents/memory/` — All assignments tagged with `agents: ["memory"]`
- `/topics/variables/` — All assignments with `topics: ["Variables"]`

### Using Taxonomies in Content

Front matter:

```yaml
---
title: "Assignment 1: Variables"
difficulty: "beginner"
agents:
  - memory
  - reasoning
topics:
  - "Variables"
  - "Data Types"
---
```

This single page automatically appears on:
- `/difficulties/beginner/`
- `/agents/memory/`
- `/agents/reasoning/`
- `/topics/variables/`
- `/topics/data-types/`

**Why taxonomies matter:** They enable powerful filtering without writing custom code. Students can browse by difficulty to find appropriate challenges, or by topic to review specific concepts.

---

## Part 6: Building and Deploying

### Development

```bash
# Start the development server with drafts visible
hugo server -D

# Server runs at http://localhost:1313
# Files are watched; changes appear immediately
```

### Production Build

```bash
# Build for deployment (ignores draft pages)
hugo

# Output appears in the public/ directory
# Ready to upload to your web server
```

### Verify Your Site Structure

Before deployment, check for common issues:

```bash
# Run Hugo in debug mode to catch warnings
hugo --debug

# Look for:
# - Missing resources referenced in front matter
# - Taxonomy names that don't match config
# - Broken internal links
```

---

## Part 7: Common Hugo Patterns to Prevent Errors

### 1. Bundle Type Matters

- **Sections (with child pages):** Use `_index.md` in the directory
  ```
  content/modules/module-01/_index.md  ✓ Correct
  content/modules/module-01/lesson-01.md (sibling)
  ```

- **Leaf pages (standalone):** Use `index.md`
  ```
  content/homework/assignment-01/index.md  ✓ Correct
  content/homework/assignment-01/rubric.pdf (resource)
  ```

**Wrong choice breaks list pages or hides content.**

### 2. Taxonomy Names Must Match

Configuration (in `hugo.yaml`):
```yaml
taxonomies:
  difficulty: "difficulties"
```

Front matter (use singular form):
```yaml
difficulty: "beginner"      # ✓ Correct (singular)
difficulties: "beginner"    # ✗ Wrong (plural)
```

### 3. Always Use ISO Dates

```yaml
dueDate: 2025-05-15     # ✓ Correct (YYYY-MM-DD)
dueDate: 05-15-2025     # ✗ Wrong (ambiguous)
```

### 4. Draft Pages Are Hidden

```yaml
draft: true    # ✗ Page won't appear in build
draft: false   # ✓ Page appears
```

Check before publishing!

### 5. Directory Names vs. Menu URLs

Your menu config:
```yaml
menu:
  main:
    - identifier: modules
      name: Modules
      url: /modules/
```

Your content directory must match:
```
content/modules/    # ✓ Matches menu URL
```

---

## Part 8: Tips for Course Content

### Organizing Large Courses

For courses with many modules, group them numerically:

```
content/modules/
├── module-01-intro/
├── module-02-fundamentals/
├── module-03-oop/
├── module-04-templates/
├── module-05-stl/
└── module-06-advanced/
```

Weight assignments:

```yaml
---
title: "Module 1: Introduction"
weight: 1
---
```

### Adding Code Examples Safely

Zen supports syntax highlighting. Use code blocks with the language specified:

```markdown
\`\`\`cpp
#include <iostream>

int main() {
    std::cout << "Hello" << std::endl;
    return 0;
}
\`\`\`
```

Zenuses Chroma for syntax highlighting (configured in `hugo.yaml`):

```yaml
markup:
  highlight:
    style: dracula  # or: monokai, github, etc.
```

### Embedding Media

For videos, PDFs, and large resources, use Zen's built-in support:

```markdown
<!-- Link to PDF -->
[Download Rubric](rubric.pdf)

<!-- Image with alt text -->
![Class Diagram](architecture.png)
```

Keep large files in the bundle directory alongside `index.md`.

### Linking Between Pages

Use relative paths for internal navigation:

```markdown
[Assignment 1](/homework/assignment-01/)
[Module 1 Overview](/modules/module-01/)
[C++ Reference](/resources/cpp-reference/)
```

---

## Summary Checklist

After completing these steps, you should have:

- [ ] Project initialized with `hugo new site`
- [ ] Git initialized and Zen theme added as submodule (pinned to v0.161.0)
- [ ] `hugo.yaml` configured with taxonomies and menu
- [ ] Three content directories created: `modules/`, `homework/`, `resources/`
- [ ] At least one module with `_index.md` and lesson pages
- [ ] At least one homework assignment with resources in a bundle
- [ ] At least one resource page
- [ ] All front matter using ISO dates and correct taxonomy names
- [ ] Development server running (`hugo server`) without errors
- [ ] Navigation menu working and displaying correct pages

---

## Quick Reference

| Task | Command |
|------|---------|
| Start dev server | `hugo server -D` |
| Build for production | `hugo` |
| Create new content | `hugo new content/path/to/file.md` |
| Check for errors | `hugo --debug` |
| Update theme | `cd themes/zen && git fetch && git checkout v0.161.0` |
| Clean build | `rm -rf resources/` && `hugo` |

---

## Further Reading

- [Hugo Documentation](https://gohugo.io/documentation/)
- [Zen Theme Repository](https://github.com/frjo/hugo-theme-zen)
- [Zen Theme Demo](https://zen-demo.xdeb.org/)
- [Hugo Content Organization](https://gohugo.io/content-management/organization/)
- [Hugo Front Matter](https://gohugo.io/content-management/front-matter/)

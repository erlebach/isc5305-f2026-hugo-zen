# Setting Up Consistent Homework Assignments in Hugo

## Overview

For a course with consistent homework structure, you need three things:
1. **Standardized front matter** with fields for title, difficulty, agents, C++ topics, and due dates
2. **Leaf bundle organization** to group assignment content with PDFs and code files
3. **Taxonomy configuration** to enable automatic filtering by difficulty, agent type, and topic

This guide shows you exactly how to structure homework assignments so they're maintainable and discoverable across your course site.

---

## Part 1: Configure Taxonomies in Hugo

First, add custom taxonomies to your `hugo.yaml` file. These enable automatic filtering pages.

### Update your hugo.yaml

Add or verify the `taxonomies` section (if using YAML) or add to `hugo.toml` if using TOML:

```yaml
# hugo.yaml
baseURL: "https://people.sc.fsu.edu/~gerlebacher/course/cap5771_s2026_dev/"
languageCode: en-us
title: ISC5305 - Scientific Computing
author: Gordon Erlebacher
description: Core Graduate Course
pluralizelisttitles: false
removePathAccents: true

# Add custom taxonomies for course organization
taxonomies:
  difficulty: difficulties      # singular in front matter, plural in config
  agent: agents                 # for reasoning, memory, planning
  topic: topics                 # for C++ topics

disableHugoGeneratorInject: true
privacy:
  googleAnalytics:
    disable: true

outputs:
  home:
    - HTML
    - RSS
    - JSON

params:
  contact: "gerlebacher@fsu.edu"
  description: "Course: Scientific Programming"
  dateformat: "2 January, 2006"
  favicon: "apple-touch-icon.png"
  feedlinks: true
  icon: "apple-touch-icon.png"
  imageMaxWidth: 400
  logo: false
  logoWidth: 64
  mobileMenu: true
  poweredby: true
  sidebar: true
  submitted: true
  ada_min_height: 44
  author:
    name: "Gordon Erlebacher"
    url: "https://www.sc.fsu.edu/~gerlebacher/"
    avatar: "path/to/some-image.jpg"

menu:
  main:
    - identifier: home
      name: Home
      url: /
      weight: 1
    - identifier: modules
      name: Module
      url: /modules/
      weight: 2
    - identifier: homework
      name: Homework
      url: /homework/
      weight: 3

markup:
  goldmark:
    renderer:
      unsafe: true
  highlight:
    style: dracula

module:
  imports:
    - path: github.com/frjo/hugo-theme-zen
```

**Why this matters:** Taxonomies automatically generate list pages. After adding these, Hugo will create:
- `/difficulties/beginner/` — lists all beginner assignments
- `/agents/memory/` — lists all assignments using memory agents
- `/topics/pointers/` — lists all assignments covering pointers

---

## Part 2: Directory Structure for Homework

Use **leaf bundles** for all homework assignments. A leaf bundle is a directory with `index.md` and associated resource files (PDFs, code).

### Create the homework section structure

```bash
# Create the homework section index
mkdir -p content/homework
cat > content/homework/_index.md << 'EOF'
---
title: "Homework Assignments"
weight: 3
---

All assignments for ISC5305 are listed below. Use the filters to find assignments by difficulty, agent type, or C++ topic.
EOF
```

### Create your first assignment bundle

```bash
mkdir -p content/homework/assignment-01-data-types
cat > content/homework/assignment-01-data-types/index.md << 'EOF'
---
title: "Assignment 1: Primitive Data Types and Variables"
weight: 1
difficulty: "beginner"
agents:
  - memory
  - reasoning
topics:
  - "Primitive Data Types"
  - "Variable Declaration"
  - "Type Casting"
  - "Standard I/O"
dueDate: 2026-05-30
description: "Master declaration, initialization, and manipulation of fundamental C++ data types"
---

## Assignment Overview

In this assignment, you will practice declaring, initializing, and manipulating variables of different data types in C++.

## Learning Objectives

1. Understand primitive data types: `int`, `float`, `double`, `char`, `bool`
2. Declare and initialize variables correctly
3. Perform implicit and explicit type conversions
4. Use standard I/O for input and output

## Requirements

1. **Declare Variables**: Create variables of each primitive type with meaningful names
2. **Initialize Values**: Use appropriate initialization syntax
3. **Perform Operations**: Do arithmetic on numeric types
4. **Type Conversion**: Show both implicit and explicit casting
5. **Output**: Display all results using `std::cout`

## Starter Code

The file `assignment-01-starter.cpp` is provided below. Complete the TODO sections.

## Submission

1. Submit your completed code as `assignment-01.cpp`
2. Your program must compile without warnings
3. Include comments explaining each section
4. See the rubric for grading criteria

## Grading Rubric

See `rubric-assignment-01.pdf` for detailed evaluation criteria.
EOF
```

### Add resource files to the bundle

Copy your starter code and rubric into the same directory:

```bash
# Copy starter code template
cp ~/path/to/assignment-01-starter.cpp content/homework/assignment-01-data-types/

# Copy grading rubric
cp ~/path/to/rubric-assignment-01.pdf content/homework/assignment-01-data-types/
```

Your directory now looks like:
```
content/homework/assignment-01-data-types/
├── index.md                      # Assignment content with front matter
├── assignment-01-starter.cpp     # Starter code file
└── rubric-assignment-01.pdf      # Grading rubric
```

---

## Part 3: Front Matter Template for All Assignments

Use this standardized template for every homework assignment to maintain consistency:

### Standard Assignment Front Matter

```yaml
---
title: "Assignment Name"                    # Required: descriptive title
weight: 1                                   # Required: sort order within homework section
difficulty: "beginner"                      # Required: beginner, intermediate, or advanced
agents:                                     # Required: list agents this uses
  - memory
  - reasoning
  - planning                                # All, some, or none are valid
topics:                                     # Required: list C++ topics covered
  - "Topic 1"
  - "Topic 2"
  - "Topic 3"
dueDate: 2026-05-30                        # Required: ISO 8601 date format (YYYY-MM-DD)
description: "One-sentence summary of what students learn"  # Required: appears in listings
---
```

### Field Descriptions

| Field | Type | Required | Values | Example |
|-------|------|----------|--------|---------|
| `title` | String | Yes | Any descriptive name | "Assignment 2: Pointers and Memory" |
| `weight` | Integer | Yes | 1, 2, 3... | 2 |
| `difficulty` | String | Yes | beginner, intermediate, advanced | "intermediate" |
| `agents` | List | Yes | memory, reasoning, planning | List can be 1, 2, or all 3 |
| `topics` | List | Yes | C++ topic names | ["Pointers", "Dynamic Memory", "Recursion"] |
| `dueDate` | Date | Yes | YYYY-MM-DD format | 2026-06-15 |
| `description` | String | Yes | 1-2 sentences | "Practice pointer arithmetic and memory management" |

### Difficulty Level Guidelines

- **beginner**: Assumes knowledge only from previous assignments; clear starter code provided
- **intermediate**: Builds on beginner concepts; requires moderate problem-solving
- **advanced**: Combines multiple concepts; minimal guidance; real-world scenarios

### Agent Types Explained

Use these three agent types based on what the assignment emphasizes:

1. **memory** — Assignment emphasizes data structures, storage, or memory management
   - Example: "Assignment 3: Dynamic Memory Allocation"

2. **reasoning** — Assignment emphasizes logic, algorithms, or problem-solving
   - Example: "Assignment 5: Binary Search Trees"

3. **planning** — Assignment emphasizes design, architecture, or multi-step planning
   - Example: "Assignment 7: Design a File Parser"

An assignment can use 0, 1, 2, or all 3 agents. Use what's most relevant.

### C++ Topics List

Common topics to use (standardize across assignments):

```
- "Primitive Data Types"
- "Pointers"
- "Dynamic Memory"
- "Arrays and Vectors"
- "Structs and Classes"
- "Object-Oriented Design"
- "Operator Overloading"
- "Inheritance and Polymorphism"
- "Templates"
- "Exception Handling"
- "File I/O"
- "Standard Library Containers"
- "Algorithms and Sorting"
- "Recursion"
- "Memory Management"
- "Type Casting"
- "Standard I/O"
```

---

## Part 4: Bundle Organization and Resource Management

### Why Leaf Bundles?

Leaf bundles keep assignment content and resources together. Hugo's `.Resources` API makes files accessible in templates without hardcoding paths.

### File Organization Pattern

Every homework assignment should follow this pattern:

```
content/homework/
├── _index.md                                # Section index (homework list)
├── assignment-01-topic-name/
│   ├── index.md                             # Assignment content with front matter
│   ├── assignment-01-starter.cpp            # Starter code (optional)
│   ├── rubric-assignment-01.pdf             # Grading rubric (optional)
│   ├── sample-solution.cpp                  # Solution (optional)
│   └── test-cases.txt                       # Test data (optional)
├── assignment-02-topic-name/
│   ├── index.md
│   ├── assignment-02-starter.cpp
│   └── rubric-assignment-02.pdf
└── assignment-03-topic-name/
    ├── index.md
    ├── assignment-03-starter.cpp
    └── rubric-assignment-03.pdf
```

### Accessing Resources in Templates

In your Hugo template (e.g., `layouts/homework/single.html`), reference resources:

```html
<!-- List all PDF rubrics -->
{{ range .Resources.ByType "pdf" }}
  <a href="{{ .RelPermalink }}">{{ .Title }}</a>
{{ end }}

<!-- List all C++ source files -->
{{ range .Resources.ByType "text" }}
  {{ if strings.HasSuffix .Name ".cpp" }}
    <a href="{{ .RelPermalink }}">{{ .Name }}</a>
  {{ end }}
{{ end }}

<!-- Reference all resources -->
{{ range .Resources }}
  <li><a href="{{ .RelPermalink }}">{{ .Name }}</a></li>
{{ end }}
```

---

## Part 5: Creating Multiple Assignments

### Use This Workflow for Each New Assignment

```bash
# 1. Create the bundle directory
mkdir -p content/homework/assignment-NN-topic-name

# 2. Create the index.md with front matter
cat > content/homework/assignment-NN-topic-name/index.md << 'EOF'
---
title: "Assignment N: Topic Name"
weight: N
difficulty: "beginner"              # or intermediate, advanced
agents:
  - agent1
  - agent2
topics:
  - "C++ Topic 1"
  - "C++ Topic 2"
dueDate: 2026-MM-DD                # ISO date format
description: "Brief one-sentence description"
---

## Assignment Overview
(Your content here)
EOF

# 3. Copy resources into the bundle
cp /path/to/starter-code.cpp content/homework/assignment-NN-topic-name/
cp /path/to/rubric.pdf content/homework/assignment-NN-topic-name/

# 4. Verify the structure
ls -la content/homework/assignment-NN-topic-name/
```

### Batch Creation Example

Create multiple assignments at once:

```bash
# Define your assignments
assignments=(
  "1:Data Types:beginner:memory,reasoning:Primitive Data Types,Variable Declaration,Type Casting:2026-05-30"
  "2:Pointers:intermediate:memory,reasoning:Pointers,Address-of Operator,Dereferencing:2026-06-15"
  "3:Dynamic Memory:intermediate:memory,planning:Dynamic Memory,new and delete,Memory Leaks:2026-06-30"
  "4:Vectors and Arrays:intermediate:memory,reasoning:Arrays,Vectors,Standard Library Containers:2026-07-15"
  "5:Functions:beginner:reasoning,planning:Function Declaration,Parameters,Return Types:2026-07-30"
)

for assignment in "${assignments[@]}"; do
  IFS=: read -r num topic difficulty agents topics_str duedate <<< "$assignment"
  dirname="assignment-${num}-${topic,,}"
  mkdir -p content/homework/$dirname
  
  # Create index.md with front matter (manual, but structure is consistent)
  echo "Created $dirname with assignment $num"
done
```

---

## Part 6: Complete Example Assignment

Here's a complete, copy-paste-ready example:

### File: content/homework/assignment-02-pointers/index.md

```yaml
---
title: "Assignment 2: Pointers and Memory Addresses"
weight: 2
difficulty: "intermediate"
agents:
  - memory
  - reasoning
topics:
  - "Pointers"
  - "Memory Addresses"
  - "Address-of Operator"
  - "Dereferencing"
  - "Pointer Arithmetic"
dueDate: 2026-06-15
description: "Understand pointers, memory addresses, and how to use the address-of and dereference operators"
---

## Assignment Overview

Pointers are one of the most important—and most challenging—concepts in C++. This assignment builds your understanding through hands-on practice with pointer declaration, initialization, dereferencing, and pointer arithmetic.

## Learning Objectives

1. Declare pointers and understand their syntax
2. Use the address-of operator (`&`) to get memory addresses
3. Use the dereference operator (`*`) to access pointed-to values
4. Perform pointer arithmetic (increment, decrement)
5. Work with pointers to different data types
6. Avoid common pointer pitfalls (null pointers, dangling pointers)

## Requirements

1. **Pointer Declaration**: Declare pointers to `int`, `double`, and `char`
2. **Address and Dereference**: Use `&` to get addresses and `*` to access values
3. **Pointer Arithmetic**: Increment/decrement pointers and show the effect on memory
4. **Arrays and Pointers**: Use pointers to traverse an array
5. **Function Pointers**: Declare and use a pointer to a function
6. **Error Handling**: Handle null pointers gracefully

## Starter Code

Begin with `assignment-02-starter.cpp`. Fill in the TODO sections.

## Submission Requirements

- Submit `assignment-02-solution.cpp`
- Code must compile without warnings
- Include comments explaining pointer operations
- Demonstrate your understanding of memory layout

## Grading

See `rubric-assignment-02.pdf` for the complete grading rubric.

## Tips

- Use `std::cout << &variable << std::endl;` to print memory addresses
- Remember: `*ptr` gives you the value; `ptr` is the address
- Pointer arithmetic depends on the type (incrementing a `double*` moves by 8 bytes, not 1)
- Always initialize pointers before dereferencing (avoid garbage values)
```

### Files to create in content/homework/assignment-02-pointers/:

```bash
# Starter code file (placeholder name)
touch content/homework/assignment-02-pointers/assignment-02-starter.cpp

# Rubric (PDF file)
touch content/homework/assignment-02-pointers/rubric-assignment-02.pdf

# Optional: Sample solution for reference
touch content/homework/assignment-02-pointers/assignment-02-sample.cpp
```

---

## Part 7: Accessing and Filtering Assignments

### Automatic Taxonomy Pages

Once you've created assignments with the front matter above, Hugo automatically generates:

**By Difficulty:**
- `/difficulties/beginner/` — all beginner assignments
- `/difficulties/intermediate/` — all intermediate assignments
- `/difficulties/advanced/` — all advanced assignments

**By Agent:**
- `/agents/memory/` — assignments emphasizing memory concepts
- `/agents/reasoning/` — assignments emphasizing problem-solving
- `/agents/planning/` — assignments emphasizing design

**By Topic:**
- `/topics/pointers/` — all assignments covering pointers
- `/topics/dynamic-memory/` — all assignments on dynamic allocation
- etc.

### Custom Templates for Listings

Create `layouts/_default/taxonomy.html` to customize how taxonomy pages display:

```html
<h1>{{ .Title }}</h1>
<p>Found {{ len .Pages }} items</p>

<ul>
{{ range .Pages }}
  <li>
    <strong><a href="{{ .RelPermalink }}">{{ .Title }}</a></strong>
    {{ if .Params.dueDate }}
      <br>Due: {{ .Params.dueDate.Format "Jan 2, 2006" }}
    {{ end }}
    {{ if .Params.difficulty }}
      <br>Difficulty: {{ .Params.difficulty }}
    {{ end }}
  </li>
{{ end }}
</ul>
```

---

## Part 8: Common Mistakes to Avoid

| Mistake | Problem | Fix |
|---------|---------|-----|
| Using `_index.md` for homework | Creates a list page for the assignment itself | Use `index.md` for leaf bundles |
| Inconsistent difficulty values | Taxonomy pages don't generate correctly | Always use: beginner, intermediate, advanced |
| Wrong date format | Hugo fails to parse dates | Use ISO format: YYYY-MM-DD |
| Files outside bundle | Resources aren't bundled with content | Keep all files in the assignment directory |
| Agents field is not a list | YAML parsing fails silently | Use list syntax: `agents: [memory, reasoning]` |
| Taxonomy name in front matter | Doesn't match config singular form | Use `difficulty:`, not `difficulties:` |
| Missing dueDate field | Sorting and filtering break | Always include: `dueDate: YYYY-MM-DD` |

---

## Part 9: Checklist for Creating a New Assignment

Use this checklist each time you create an assignment:

- [ ] Create directory: `content/homework/assignment-NN-topic/`
- [ ] Create `index.md` with required front matter fields:
  - [ ] `title` — descriptive name
  - [ ] `weight` — sequential number
  - [ ] `difficulty` — beginner/intermediate/advanced
  - [ ] `agents` — list (can be empty or 1-3 items)
  - [ ] `topics` — list of C++ topics covered
  - [ ] `dueDate` — YYYY-MM-DD format
  - [ ] `description` — one-sentence summary
- [ ] Write assignment content in the `index.md`
- [ ] Copy starter code file into the bundle directory
- [ ] Copy rubric PDF into the bundle directory
- [ ] Run `hugo server` to verify it appears in `/homework/` section
- [ ] Check that assignment appears in taxonomy pages:
  - `/difficulties/beginner/` (or intermediate/advanced)
  - `/agents/memory/` (or reasoning/planning as appropriate)
  - `/topics/pointers/` (or relevant topic)
- [ ] Test all resource links (starter code, rubric) work correctly

---

## Part 10: Example: Three Complete Assignments

Here are three ready-to-use assignment examples with all files:

### Assignment 1: Beginner - Data Types

```yaml
---
title: "Assignment 1: Primitive Data Types and Variables"
weight: 1
difficulty: "beginner"
agents:
  - memory
  - reasoning
topics:
  - "Primitive Data Types"
  - "Variable Declaration"
  - "Type Casting"
  - "Standard I/O"
dueDate: 2026-05-30
description: "Master declaration, initialization, and manipulation of fundamental C++ data types"
---
```

**Directory:** `content/homework/assignment-01-data-types/`
**Files:** `index.md`, `assignment-01-starter.cpp`, `rubric-assignment-01.pdf`

### Assignment 2: Intermediate - Pointers

```yaml
---
title: "Assignment 2: Pointers and Memory Addresses"
weight: 2
difficulty: "intermediate"
agents:
  - memory
  - reasoning
topics:
  - "Pointers"
  - "Memory Addresses"
  - "Pointer Arithmetic"
  - "Dereferencing"
dueDate: 2026-06-15
description: "Understand pointer syntax, memory addresses, and address/dereference operators"
---
```

**Directory:** `content/homework/assignment-02-pointers/`
**Files:** `index.md`, `assignment-02-starter.cpp`, `rubric-assignment-02.pdf`

### Assignment 3: Intermediate - Dynamic Memory

```yaml
---
title: "Assignment 3: Dynamic Memory Allocation"
weight: 3
difficulty: "intermediate"
agents:
  - memory
  - planning
topics:
  - "Dynamic Memory"
  - "new and delete"
  - "Memory Leaks"
  - "Constructors and Destructors"
dueDate: 2026-06-30
description: "Learn to allocate and deallocate memory dynamically, and avoid common memory pitfalls"
---
```

**Directory:** `content/homework/assignment-03-dynamic-memory/`
**Files:** `index.md`, `assignment-03-starter.cpp`, `rubric-assignment-03.pdf`

---

## Summary

You now have everything needed to create consistent, well-organized homework assignments in Hugo:

1. **Taxonomies** — Enable automatic filtering by difficulty, agent, and topic
2. **Leaf Bundles** — Keep assignment content bundled with PDFs and code files
3. **Standardized Front Matter** — Ensures every assignment has required metadata
4. **Resource Management** — Access starter code and rubrics via `.Resources`
5. **Automatic Pages** — Hugo generates filtering pages without custom code

Use the front matter template and checklist for every assignment to maintain consistency across your course site.

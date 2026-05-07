# Hugo Structure for Homework Assignments: Front Matter and Resource Management

This guide shows how to structure homework assignments in Hugo with consistent front matter for title, difficulty level, related agents, C++ topics, due dates, and attached PDFs and code files.

---

## Overview

You need to organize assignments with:
1. **Metadata fields** (title, difficulty, agents, topics, due date)
2. **Attached resources** (PDFs, code files)
3. **Consistent structure** across all assignments
4. **Easy filtering** by difficulty, agent type, and topic

The solution uses **front matter** (metadata) in assignment pages combined with **leaf bundles** (directories containing the assignment file plus resources).

---

## Part 1: Configure Hugo for Assignment Management

### Step 1: Update hugo.yaml with Taxonomies

Your current `hugo.yaml` should include these custom taxonomies to enable filtering by difficulty, agent type, and topic:

```yaml
taxonomies:
  difficulty: "difficulties"  # Singular in front matter, plural in config
  agent: "agents"
  topic: "topics"

params:
  # ... existing params ...
  assignmentFields:
    difficulties:
      - "beginner"
      - "intermediate"
      - "advanced"
    agentTypes:
      - "memory"
      - "reasoning"
      - "planning"
```

This configuration creates automatic taxonomy pages:
- `/difficulties/beginner/` — lists all beginner assignments
- `/agents/memory/` — lists all assignments using memory agents
- `/topics/cpp-basics/` — lists all assignments covering C++ basics

### Step 2: Create Content Structure

Your assignments directory should look like this:

```
content/homework/
├── assignment-01-basic-io/
│   ├── index.md                    # Assignment page with front matter
│   ├── starter-code.cpp            # Code template
│   ├── test-cases.cpp              # Test file
│   └── rubric.pdf                  # Grading rubric
├── assignment-02-arrays/
│   ├── index.md
│   ├── helper-functions.h
│   ├── problem1-template.cpp
│   └── assignment-rubric.pdf
└── assignment-03-oop/
    ├── index.md
    ├── base-class.h
    ├── derived-class.h
    └── grading-guide.pdf
```

Each assignment is a **leaf bundle** (directory with `index.md` and resources).

---

## Part 2: Front Matter Template for Assignments

### Complete Front Matter Structure

Use this template for every assignment in `content/homework/assignment-XX/index.md`:

```yaml
---
title: "Assignment 1: Basic I/O and Variables"
weight: 1
difficulty: "beginner"
agents:
  - memory
  - reasoning
topics:
  - "Variables and Data Types"
  - "Input/Output"
  - "Type Conversion"
dueDate: "2025-05-15"
description: "Practice fundamental C++ concepts including variable declaration, reading input, and performing calculations."
draft: false
---
```

### Field Descriptions

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `title` | String | Yes | Full assignment name |
| `weight` | Integer | Yes | Controls ordering (1, 2, 3...) |
| `difficulty` | String | Yes | One of: `beginner`, `intermediate`, `advanced` |
| `agents` | Array | Yes | List of AI agent types (e.g., `memory`, `reasoning`, `planning`) |
| `topics` | Array | Yes | List of C++ concepts covered (e.g., `"Arrays"`, `"Pointers"`, `"Object-Oriented Programming"`) |
| `dueDate` | String | Yes | ISO 8601 format: `YYYY-MM-DD` |
| `description` | String | Yes | One-sentence summary for listings |
| `draft` | Boolean | Optional | Set to `false` to publish; `true` hides the page |

---

## Part 3: Creating Assignments with Resources

### Example 1: Assignment with Multiple Files

File: `content/homework/assignment-01-basic-io/index.md`

```yaml
---
title: "Assignment 1: Basic I/O and Variables"
weight: 1
difficulty: "beginner"
agents:
  - memory
  - reasoning
topics:
  - "Variables and Data Types"
  - "Input/Output"
  - "Arithmetic Operations"
dueDate: "2025-05-22"
description: "Write programs to practice variable declaration, I/O operations, and type conversions."
draft: false
---

## Assignment 1: Basic I/O and Variables

**Due Date:** May 22, 2025

**Difficulty Level:** Beginner

**AI Agents Used:** Memory, Reasoning

---

## Objective

Practice fundamental C++ concepts: declaring variables, reading input, and producing output using standard input/output streams.

## Problems

### Problem 1: Temperature Conversion

Write a program that converts temperature from Celsius to Fahrenheit.

**Requirements:**
- Read a temperature in Celsius from the user
- Convert using the formula: F = (C × 9/5) + 32
- Display the result with one decimal place
- Use appropriate variable names
- Include comments explaining each step

**Sample Input/Output:**
```
Enter temperature in Celsius: 25
Temperature in Fahrenheit: 77.0
```

### Problem 2: Simple Calculator

Write a program that performs basic arithmetic operations.

**Requirements:**
- Read two numbers and an operator (+, -, *, /)
- Perform the calculation
- Handle division by zero with an error message
- Display the result with appropriate precision

**Sample Input/Output:**
```
Enter first number: 10
Enter operator: *
Enter second number: 5
Result: 50
```

## Submission Guidelines

1. Name your files `problem1.cpp` and `problem2.cpp`
2. Include comments explaining your logic
3. Test your programs with multiple inputs (including edge cases)
4. Submit as a `.zip` file containing both source files
5. Do not submit compiled executables

## Resources

- **Starter Code:** [starter-code.cpp](starter-code.cpp) — Template to get you started
- **Test Cases:** [test-cases.cpp](test-cases.cpp) — Example test input/output pairs
- **Grading Rubric:** [rubric.pdf](rubric.pdf) — Detailed grading criteria

## Grading

Your submission will be evaluated on:
- **Correctness** (60%): Does your program produce correct output?
- **Code Quality** (30%): Is your code readable, commented, and well-organized?
- **Completeness** (10%): Did you handle all requirements and edge cases?

See the rubric PDF for detailed point breakdowns.
```

Create these supporting files in the same directory:

File: `content/homework/assignment-01-basic-io/starter-code.cpp`

```cpp
#include <iostream>
using namespace std;

int main() {
    // Write your solution here
    // Remember to:
    // 1. Declare variables with descriptive names
    // 2. Prompt the user for input
    // 3. Read the input using cin
    // 4. Perform calculations
    // 5. Display output using cout
    
    return 0;
}
```

File: `content/homework/assignment-01-basic-io/test-cases.cpp`

```cpp
// Example test cases for Assignment 1
// Use these to verify your solution works correctly

// Problem 1: Temperature Conversion
// Input: 0
// Expected Output: 32.0

// Input: 100
// Expected Output: 212.0

// Input: -40
// Expected Output: -40.0

// Problem 2: Simple Calculator
// Input: 10 + 5
// Expected Output: 15

// Input: 20 / 4
// Expected Output: 5

// Input: 10 / 0
// Expected Output: Error: Division by zero
```

Place `rubric.pdf` in the same directory (your actual grading rubric PDF file).

### Example 2: Intermediate Assignment with Header Files

File: `content/homework/assignment-02-arrays/index.md`

```yaml
---
title: "Assignment 2: Arrays and Functions"
weight: 2
difficulty: "intermediate"
agents:
  - memory
  - reasoning
  - planning
topics:
  - "Arrays"
  - "Functions"
  - "Loops"
  - "Algorithm Design"
dueDate: "2025-06-05"
description: "Work with arrays, implement sorting algorithms, and develop reusable functions for data processing."
draft: false
---

## Assignment 2: Arrays and Functions

**Due Date:** June 5, 2025

**Difficulty Level:** Intermediate

**AI Agents Used:** Memory, Reasoning, Planning

---

## Objective

Master arrays and functions by implementing data processing algorithms. You'll work with sorting, searching, and basic algorithm design.

## Problems

### Problem 1: Array Statistics

Implement functions to calculate statistics on an array of integers.

**Requirements:**
- Implement `double calculateAverage(int arr[], int size)`
- Implement `int findMax(int arr[], int size)`
- Implement `int findMin(int arr[], int size)`
- Read integers from user input into an array
- Call your functions and display results

### Problem 2: Bubble Sort Implementation

Implement and test the bubble sort algorithm.

**Requirements:**
- Implement `void bubbleSort(int arr[], int size)`
- The function should modify the array in-place
- Include a comparison counter to measure efficiency
- Test with arrays of different sizes

## Resources

- **Helper Functions:** [helper-functions.h](helper-functions.h) — Function signatures to implement
- **Template:** [problem1-template.cpp](problem1-template.cpp) — Starting point
- **Rubric:** [assignment-rubric.pdf](assignment-rubric.pdf) — Grading criteria

## Submission

Submit two files:
1. `problem1.cpp` — Array statistics program
2. `problem2.cpp` — Bubble sort implementation
```

File: `content/homework/assignment-02-arrays/helper-functions.h`

```cpp
#ifndef HELPER_FUNCTIONS_H
#define HELPER_FUNCTIONS_H

// Array statistics functions
double calculateAverage(int arr[], int size);
int findMax(int arr[], int size);
int findMin(int arr[], int size);

// Sorting function
void bubbleSort(int arr[], int size);

#endif
```

---

## Part 4: Accessing Attached Resources in Templates

### How Hugo Provides Access to Files

When you place files in a bundle directory (alongside `index.md`), Hugo makes them accessible via the `.Resources` object in templates.

### Linking to Resources in Markdown

In your assignment's `index.md`, link to resources using relative paths:

```markdown
[Download Starter Code](starter-code.cpp)
[View Test Cases](test-cases.cpp)
[Grading Rubric](rubric.pdf)
```

Hugo automatically resolves these relative paths to the bundle directory.

### Using Resources in Custom Templates

If you create a custom theme layout for assignments, access resources like this:

File: `layouts/homework/single.html` (custom template)

```html
<article>
  <h1>{{ .Title }}</h1>
  
  <div class="metadata">
    <p><strong>Difficulty:</strong> {{ .Params.difficulty }}</p>
    <p><strong>Due Date:</strong> {{ .Params.dueDate }}</p>
  </div>
  
  <!-- Main assignment content -->
  {{ .Content }}
  
  <!-- Resource downloads -->
  <section class="resources">
    <h2>Resources</h2>
    <ul>
    {{ range .Resources }}
      <li>
        <a href="{{ .RelPermalink }}">{{ .Name }}</a>
        ({{ .ResourceType }})
      </li>
    {{ end }}
    </ul>
  </section>
</article>
```

The `.Resources` object provides access to all files in the bundle directory.

---

## Part 5: Front Matter Best Practices

### Correct Usage

```yaml
---
title: "Assignment 1: Variables"
difficulty: "beginner"              # ✓ Singular (taxonomy key)
agents:                             # ✓ Array format
  - memory
  - reasoning
topics:                             # ✓ Array format
  - "Variables"
  - "Data Types"
dueDate: "2025-05-15"              # ✓ ISO 8601 format
draft: false                        # ✓ Publish this page
---
```

### Common Mistakes to Avoid

```yaml
---
# WRONG: Plural form (matches config value, not key)
difficulties: "beginner"            # ✗ Should be "difficulty"

# WRONG: Not an array
agents: "memory, reasoning"         # ✗ Should be array with dashes

# WRONG: Date format
dueDate: "05-15-2025"              # ✗ Should be YYYY-MM-DD

# WRONG: Page won't appear
draft: true                         # ✗ Hidden from build

# WRONG: Inconsistent topic names
topics:
  - "Variables"
  - "variables"                    # ✗ Creates separate taxonomy entry
---
```

### Consistency Rules

1. **Difficulty:** Always use one of: `beginner`, `intermediate`, `advanced`
2. **Agents:** Use exact names: `memory`, `reasoning`, `planning`
3. **Topics:** Capitalize consistently (e.g., "Object-Oriented Programming", not "object-oriented programming")
4. **Dates:** Always `YYYY-MM-DD` format
5. **Weight:** Numeric, starting at 1, incremented by 1 for each new assignment

---

## Part 6: Directory Organization Pattern

### Recommended Naming Convention

```
content/homework/
├── assignment-01-basic-io/
│   ├── index.md
│   ├── starter-code.cpp
│   ├── test-cases.cpp
│   └── rubric.pdf
├── assignment-02-arrays/
│   ├── index.md
│   ├── helper-functions.h
│   ├── problem1-template.cpp
│   └── assignment-rubric.pdf
├── assignment-03-pointers/
│   ├── index.md
│   ├── memory-diagram.png
│   ├── starter-code.cpp
│   └── grading-guide.pdf
└── assignment-04-oop/
    ├── index.md
    ├── base-class.h
    ├── derived-class.h
    ├── test-harness.cpp
    └── rubric.pdf
```

**Why this structure:**
- Directory names are human-readable and descriptive
- Numbers ensure proper ordering (01, 02, 03...)
- Topic descriptors make browsing file systems easier
- All resources live with the assignment (no scattered files)

---

## Part 7: Filtering and Discovery Through Taxonomies

### Automatic Listing Pages

Once you add assignments with front matter, Hugo automatically creates these pages:

**By Difficulty:**
- `/difficulties/beginner/` — All beginner assignments
- `/difficulties/intermediate/` — All intermediate assignments
- `/difficulties/advanced/` — All advanced assignments

**By Agent Type:**
- `/agents/memory/` — Assignments using memory agents
- `/agents/reasoning/` — Assignments using reasoning agents
- `/agents/planning/` — Assignments using planning agents

**By Topic:**
- `/topics/variables/` — All assignments covering variables
- `/topics/arrays/` — All assignments covering arrays
- `/topics/oop/` — All assignments covering object-oriented programming

### Student Usage

Students can navigate taxonomies to:
- Start with beginner assignments and progress to advanced
- Find assignments that target specific AI agents
- Review all assignments on a particular C++ topic

### Creating Links to Taxonomies

In your course homepage or resources section, add links:

```markdown
## Browse Assignments

### By Difficulty Level
- [Beginner](/difficulties/beginner/) — 3 assignments
- [Intermediate](/difficulties/intermediate/) — 4 assignments
- [Advanced](/difficulties/advanced/) — 2 assignments

### By Agent Type
- [Memory Agent Assignments](/agents/memory/)
- [Reasoning Agent Assignments](/agents/reasoning/)
- [Planning Agent Assignments](/agents/planning/)

### By Topic
- [Variables and Data Types](/topics/variables/)
- [Arrays](/topics/arrays/)
- [Object-Oriented Programming](/topics/oop/)
```

---

## Part 8: Complete Assignment Lifecycle

### Step-by-Step: Creating a New Assignment

**Step 1: Create the directory**

```bash
mkdir -p content/homework/assignment-05-templates
```

**Step 2: Create index.md with complete front matter**

```yaml
---
title: "Assignment 5: C++ Templates and Generic Programming"
weight: 5
difficulty: "advanced"
agents:
  - memory
  - reasoning
  - planning
topics:
  - "Templates"
  - "Generic Programming"
  - "STL Containers"
dueDate: "2025-06-19"
description: "Implement generic functions and classes using C++ templates to solve common data structure problems."
draft: false
---
```

**Step 3: Write the assignment content**

Include clear problem statements, requirements, sample I/O, and submission guidelines.

**Step 4: Add supporting resources**

```bash
# Add code templates
cp my-template.h content/homework/assignment-05-templates/
cp starter-code.cpp content/homework/assignment-05-templates/

# Add PDF rubric
cp rubric.pdf content/homework/assignment-05-templates/
```

**Step 5: Verify in development server**

```bash
hugo server -D
# Navigate to http://localhost:1313/homework/assignment-05-templates/
# Check that all files link correctly
```

**Step 6: Verify in taxonomy pages**

- Visit `/difficulties/advanced/` — assignment should appear
- Visit `/agents/planning/` — assignment should appear
- Visit `/topics/templates/` — assignment should appear

---

## Part 9: Template for Quick Creation

### Reusable Markdown Template

Save this as a template file for new assignments:

```yaml
---
title: "Assignment XX: [Title]"
weight: XX
difficulty: "[beginner|intermediate|advanced]"
agents:
  - "[memory|reasoning|planning]"
topics:
  - "[Topic 1]"
  - "[Topic 2]"
  - "[Topic 3]"
dueDate: "YYYY-MM-DD"
description: "[One-sentence summary of assignment]"
draft: false
---

## Assignment XX: [Title]

**Due Date:** [Month Day, Year]

**Difficulty Level:** [Beginner/Intermediate/Advanced]

**AI Agents Used:** [Agent names]

---

## Objective

[2-3 sentence description of what students will learn]

## Problems

### Problem 1: [Problem Name]

[Problem description and requirements]

**Requirements:**
- Requirement 1
- Requirement 2
- Requirement 3

### Problem 2: [Problem Name]

[Problem description and requirements]

## Resources

- **Template:** [filename.cpp]
- **Helper Functions:** [filename.h]
- **Rubric:** [rubric.pdf]

## Submission Guidelines

[Submission instructions]

## Grading

[Grading breakdown]
```

---

## Part 10: Validation Checklist

Before publishing an assignment, verify:

- [ ] Directory follows naming convention: `assignment-XX-topic/`
- [ ] File is named `index.md` (not `assignment-XX.md`)
- [ ] Front matter has all required fields: title, weight, difficulty, agents, topics, dueDate, description
- [ ] Difficulty is one of: `beginner`, `intermediate`, `advanced`
- [ ] Agents field is an array with proper formatting
- [ ] Topics field is an array with proper formatting
- [ ] Due date is in ISO 8601 format (`YYYY-MM-DD`)
- [ ] `draft: false` (so page will be published)
- [ ] All referenced resource files exist in the directory
- [ ] Links to resources use relative paths (e.g., `starter-code.cpp`, not `/content/homework/...`)
- [ ] Markdown is properly formatted (code blocks, headings, lists)
- [ ] Assignment appears correctly at `http://localhost:1313/homework/assignment-XX-topic/`
- [ ] Assignment appears in taxonomy pages: `/difficulties/[level]/`, `/agents/[agent]/`, `/topics/[topic]/`

---

## Summary

To structure Hugo assignments consistently:

1. **Use leaf bundles:** One directory per assignment with `index.md` and all resources
2. **Complete front matter:** Include title, weight, difficulty, agents, topics, dueDate, description, and draft
3. **Taxonomy support:** Configure custom taxonomies in `hugo.yaml` for filtering
4. **Resource management:** Place all PDFs and code files in the bundle directory alongside `index.md`
5. **Naming consistency:** Use `assignment-XX-topic/` format with numeric prefixes
6. **Relative linking:** Reference resources with relative paths in markdown
7. **Taxonomy discovery:** Students can browse by difficulty, agent type, or topic automatically

This approach creates a maintainable, scalable system for course homework that scales from 5 to 100+ assignments without structural changes.

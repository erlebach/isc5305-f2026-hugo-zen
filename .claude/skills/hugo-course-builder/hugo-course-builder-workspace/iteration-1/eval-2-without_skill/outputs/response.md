# Hugo Front Matter Structure for Course Homework Assignments

## Overview

Hugo uses YAML or TOML front matter to define metadata for content. For homework assignments, you should structure front matter to capture assignment details, learning objectives, and file attachments. This guide provides a recommended structure for your ISC5305 course.

## Recommended Front Matter Structure

### YAML Format (Recommended for Readability)

```yaml
---
title: "Assignment 1: Matrix Operations"
description: "Implement basic matrix multiplication and transposition operations"
weight: 1

# Metadata for assignments
assignment:
  difficulty: "intermediate"
  agents:
    - "memory"
    - "reasoning"
    - "planning"
  cpp_topics:
    - "Dynamic arrays"
    - "Pointer arithmetic"
    - "Memory management"
    - "STL containers"
  due_date: 2026-05-31T23:59:00Z
  points: 100

# Page configuration
date: 2026-05-07
lastmod: 2026-05-07
draft: false
type: "homework"

# Page sections/organization
outputs:
  - HTML
  - JSON
---
```

## Complete Assignment Example with Details

```yaml
---
# Basic page metadata
title: "Assignment 2: Cache-Efficient Computation"
slug: "assignment-2-cache-efficient"
description: "Optimize C++ code for better CPU cache utilization and measure performance improvements"

# Assignment-specific metadata
assignment:
  # Learning objectives and scope
  difficulty: "advanced"
  learning_agents:
    - agent: "reasoning"
      focus: "Algorithm complexity analysis"
    - agent: "planning"
      focus: "Performance optimization strategy"
    - agent: "memory"
      focus: "Cache behavior and data locality"
  
  # Technical requirements
  cpp_topics:
    - "Cache locality optimization"
    - "SIMD instructions"
    - "Memory hierarchy"
    - "Performance profiling tools"
    - "Compiler optimizations"
  
  # Timeline
  released_date: 2026-05-14T08:00:00Z
  due_date: 2026-05-31T23:59:00Z
  late_deadline: 2026-06-07T23:59:00Z
  
  # Grading
  total_points: 150
  breakdown:
    - name: "Code functionality"
      points: 50
    - name: "Performance optimization"
      points: 60
    - name: "Documentation"
      points: 40
  
  # Requirements and constraints
  requirements:
    - "Compile without warnings on GCC 13+"
    - "Achieve 20%+ speedup over baseline"
    - "Provide performance analysis report"
  
  # Learning outcomes
  outcomes:
    - "Understand CPU cache hierarchy"
    - "Apply data locality principles"
    - "Use profiling tools effectively"

# Organization
weight: 2
date: 2026-05-07
lastmod: 2026-05-13
type: "homework"
draft: false

# Hugo technical settings
outputs:
  - HTML
  - JSON
outputs_home:
  - HTML
---
```

## File Attachments: Page Bundles Approach

Hugo's **Page Bundles** (a.k.a. Branch/Leaf Bundles) are the recommended way to organize assignments with associated files.

### Directory Structure

```
content/
└── homework/
    └── assignment-1/
        ├── index.md          # Assignment description (front matter above)
        ├── starter-code.cpp  # Provided starter template
        ├── assignment-1.pdf  # PDF with detailed requirements
        ├── examples/
        │   ├── input.txt
        │   └── expected_output.txt
        └── resources/
            ├── reference-1.pdf
            └── algorithm-guide.md
```

### Referencing Files in Front Matter

Add a `resources` section to explicitly declare attachable files:

```yaml
---
title: "Assignment 1"

# File resources
resources:
  - name: "spec"
    src: "assignment-1.pdf"
    title: "Full Assignment Specification"
    params:
      icon: "📄"
      type: "required"
  
  - name: "starter"
    src: "starter-code.cpp"
    title: "C++ Starter Template"
    params:
      icon: "💻"
      type: "provided"
      language: "cpp"
  
  - name: "examples"
    src: "examples/*"
    title: "Test Cases and Examples"
    params:
      icon: "📋"
      type: "reference"
  
  - name: "reference"
    src: "resources/reference-1.pdf"
    title: "Algorithm Design Reference"
    params:
      icon: "📚"
      type: "optional"

assignment:
  difficulty: "intermediate"
  # ... other fields
---
```

## Template Rendering (Hugo Layout)

### Sample Layout File: `layouts/homework/single.html`

```html
{{ define "main" }}
<article>
  <header>
    <h1>{{ .Title }}</h1>
    <p class="description">{{ .Params.Description }}</p>
  </header>

  <!-- Assignment Metadata -->
  <aside class="assignment-metadata">
    <div class="meta-group">
      <h3>Assignment Details</h3>
      <dl>
        <dt>Difficulty:</dt>
        <dd>{{ .Params.Assignment.Difficulty }}</dd>
        
        <dt>Points:</dt>
        <dd>{{ .Params.Assignment.TotalPoints }}</dd>
        
        <dt>Due Date:</dt>
        <dd>{{ .Params.Assignment.DueDate | time.Format "January 2, 2006 at 3:04 PM MST" }}</dd>
      </dl>
    </div>

    <div class="meta-group">
      <h3>Learning Agents</h3>
      <ul>
        {{ range .Params.Assignment.Agents }}
          <li>{{ . }}</li>
        {{ end }}
      </ul>
    </div>

    <div class="meta-group">
      <h3>C++ Topics</h3>
      <ul>
        {{ range .Params.Assignment.CppTopics }}
          <li>{{ . }}</li>
        {{ end }}
      </ul>
    </div>
  </aside>

  <!-- Main Content -->
  <div class="content">
    {{ .Content }}
  </div>

  <!-- File Downloads -->
  {{ if .Resources }}
  <section class="assignment-files">
    <h2>Assignment Files</h2>
    <div class="files-grid">
      {{ range .Resources }}
      <div class="file-card" data-type="{{ .Params.type }}">
        <span class="icon">{{ .Params.icon }}</span>
        <h4>{{ .Title }}</h4>
        <a href="{{ .Permalink }}" class="download-btn">
          Download {{ .Name }}
        </a>
        <span class="file-type">{{ .Params.type | humanize }}</span>
      </div>
      {{ end }}
    </div>
  </section>
  {{ end }}
</article>
{{ end }}
```

## Implementation Checklist

### 1. Create Content Directory Structure
```bash
mkdir -p content/homework
```

### 2. Create Assignment as Page Bundle
```bash
mkdir -p content/homework/assignment-1
touch content/homework/assignment-1/index.md
```

### 3. Set Front Matter
- Use the YAML structure above in `index.md`
- Include all required fields: title, difficulty, agents, cpp_topics, due_date
- Add `resources` section for attachments

### 4. Add Files to Bundle
- Place PDFs, starter code, examples in the same directory as `index.md`
- Reference files in the `resources` section

### 5. Create Layout Template
- Create `layouts/_default/single.html` or `layouts/homework/single.html`
- Use the template example above to render metadata and download links

### 6. Update hugo.yaml (Optional)
Add assignment-specific output formats:

```yaml
outputs:
  homework:
    - HTML
    - JSON

outputFormats:
  JSON:
    mediaType: application/json
    baseName: index
```

## Best Practices

### Front Matter Organization
1. **Keep related fields grouped** — difficulty and agents together, timeline fields together
2. **Use consistent date format** — RFC3339 (`2026-05-31T23:59:00Z`) for reliable parsing
3. **Namespace custom fields** — use `assignment.*` to avoid conflicts with Hugo's built-in fields

### File Management
1. **Use Page Bundles** — keeps assignment assets with content, easier to relocate
2. **Name PDFs descriptively** — `assignment-1-spec.pdf` instead of `spec.pdf`
3. **Store starter code in bundle** — students download from one place
4. **Use glob patterns** — `examples/*` automatically includes new test cases

### Metadata Design
1. **Duplicate structured data minimally** — store once, render multiple ways
2. **Make human-readable and machine-parseable** — both YAML syntax and meaningful field names
3. **Include learning outcomes** — explicit connection to course objectives

## Example: Assignment List Page

### `content/homework/_index.md`

```yaml
---
title: "Homework Assignments"
type: "homework"
layout: "list"
---

View all assignments below. Click an assignment to download starter code and requirements.
```

### Template: `layouts/homework/list.html`

```html
{{ define "main" }}
<h1>{{ .Title }}</h1>

<table class="assignments-table">
  <thead>
    <tr>
      <th>Assignment</th>
      <th>Difficulty</th>
      <th>Due Date</th>
      <th>Points</th>
      <th>Topics</th>
    </tr>
  </thead>
  <tbody>
    {{ range .Pages }}
    <tr>
      <td><a href="{{ .Permalink }}">{{ .Title }}</a></td>
      <td><span class="badge badge-{{ .Params.Assignment.Difficulty }}">
        {{ .Params.Assignment.Difficulty }}
      </span></td>
      <td>{{ .Params.Assignment.DueDate | time.Format "Jan 2" }}</td>
      <td>{{ .Params.Assignment.TotalPoints }}</td>
      <td>
        {{ range first 3 .Params.Assignment.CppTopics }}
          <span class="tag">{{ . }}</span>
        {{ end }}
      </td>
    </tr>
    {{ end }}
  </tbody>
</table>
{{ end }}
```

## Advanced: JSON Export for External Tools

The front matter structure is parseable as JSON, enabling integration with grading systems, learning management systems, or tracking tools.

### Hugo JSON Output Example

With `outputs: [HTML, JSON]` in front matter, Hugo generates `index.json`:

```json
{
  "title": "Assignment 1: Matrix Operations",
  "date": "2026-05-07T00:00:00Z",
  "assignment": {
    "difficulty": "intermediate",
    "agents": ["memory", "reasoning", "planning"],
    "cpp_topics": ["Dynamic arrays", "Pointer arithmetic"],
    "due_date": "2026-05-31T23:59:00Z",
    "total_points": 100
  },
  "content": "..."
}
```

This enables:
- Automatic schedule syncing with calendar tools
- Generating syllabus PDFs programmatically
- Building custom dashboards

## Summary

The recommended structure uses:

1. **Grouped YAML fields** in front matter (difficulty, agents, cpp_topics, due_date)
2. **Page Bundles** for organizing assignments with attachments
3. **Resources section** with explicit file declarations
4. **Type: "homework"** to enable custom layouts and logic
5. **Custom layout templates** to render metadata and file downloads consistently

This approach is maintainable, scales to many assignments, supports multiple file types, and leverages Hugo's built-in features without requiring plugins.

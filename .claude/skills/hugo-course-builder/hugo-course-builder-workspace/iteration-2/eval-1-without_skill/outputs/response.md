# Hugo Site Setup for C++ Course with Zen Theme v0.161.0

## Overview

This guide provides comprehensive initialization steps and recommended content directory structure for setting up a Hugo site for an academic C++ course using the Zen theme version 0.161.0. Your existing `hugo.yaml` configuration file is already partially configured with the necessary theme module import.

---

## Part 1: Initialization Steps

### Step 1: Initialize Hugo Site Directory Structure

Create the essential Hugo directory structure at the project root:

```bash
# From project root
mkdir -p content/{_index,modules,homework,resources}
mkdir -p layouts/_default
mkdir -p static/images
mkdir -p archetypes
mkdir -p data
```

### Step 2: Install Zen Theme Module

The `hugo.yaml` file already includes the module import for Zen theme. Initialize Hugo modules to download the theme:

```bash
hugo mod get -u
hugo mod tidy
```

This command will:
- Download the hugo-theme-zen repository (github.com/frjo/hugo-theme-zen) at version 0.161.0
- Create a `go.mod` and `go.sum` files for module dependencies
- Install the theme into the local module cache

### Step 3: Verify Theme Installation

After module initialization, verify the theme is accessible:

```bash
hugo mod graph
```

You should see output showing the zen theme module dependency. The theme files will be available in your Hugo build without requiring a separate `themes/` directory.

### Step 4: Set Hugo Minimum Version

Add the required Hugo version constraint to `hugo.yaml` to ensure compatibility with the Zen theme v0.161.0:

```yaml
# Add to hugo.yaml (after title field)
minHugoVersion: "0.109.0"  # Zen 0.161.0 requires Hugo 0.109+
```

### Step 5: Create Essential Content Files

#### Home Page (_index file)

Create `/content/_index.md`:

```markdown
---
title: "ISC5305 - Scientific Computing"
description: "Core Graduate Course in C++"
---

Welcome to ISC5305: Scientific Computing. This course covers advanced C++ programming techniques for scientific applications.

## Course Overview

- **Level**: Graduate
- **Instructor**: Gordon Erlebacher
- **Contact**: gerlebacher@fsu.edu

## Course Structure

This course is organized into modules covering fundamental C++ concepts through advanced scientific computing patterns.

- **Modules**: Structured learning units with lectures and code examples
- **Homework**: Problem sets and programming assignments
- **Resources**: Reference materials, guides, and additional reading
```

#### Modules Section (_index file)

Create `/content/modules/_index.md`:

```markdown
---
title: "Modules"
description: "Course modules and learning units"
---

All course modules are listed below. Each module contains lectures, code examples, and learning objectives.
```

#### Homework Section (_index file)

Create `/content/homework/_index.md`:

```markdown
---
title: "Homework"
description: "Assignments and problem sets"
---

Homework assignments are listed below. Complete assignments according to the specified deadlines and submission requirements.
```

#### Resources Section (_index file)

Create `/content/resources/_index.md`:

```markdown
---
title: "Resources"
description: "Reference materials and additional content"
---

Reference materials, C++ documentation links, and supplementary resources for the course.
```

### Step 6: Build and Test

Test the Hugo build to ensure everything is configured correctly:

```bash
hugo server -D
```

Visit `http://localhost:1313` in your browser. You should see your course homepage with the Zen theme styling applied.

---

## Part 2: Recommended Content Directory Structure

### Directory Organization

```
content/
├── _index.md                  # Home page
├── modules/
│   ├── _index.md             # Modules landing page
│   ├── module-01-intro/
│   │   ├── _index.md         # Module overview
│   │   ├── lecture-01.md
│   │   ├── lecture-02.md
│   │   └── code-examples.md
│   ├── module-02-basics/
│   │   ├── _index.md
│   │   ├── lecture-01.md
│   │   ├── lecture-02.md
│   │   └── code-examples.md
│   └── module-nn-advanced/
│       └── _index.md
├── homework/
│   ├── _index.md             # Assignments landing page
│   ├── assignment-01/
│   │   ├── _index.md         # Assignment details
│   │   └── starter-code.md
│   ├── assignment-02/
│   │   ├── _index.md
│   │   └── starter-code.md
│   └── assignment-nn/
│       └── _index.md
└── resources/
    ├── _index.md             # Resources landing page
    ├── cpp-reference.md
    ├── coding-standards.md
    ├── tools-setup.md
    └── bibliography.md
```

### Module Content Template

Create `/archetypes/module.md` for consistent module structure:

```markdown
---
title: "{{ .Name }}"
description: "Module description"
weight: 10
draft: false
---

## Learning Objectives

- Objective 1
- Objective 2
- Objective 3

## Topics Covered

- Topic 1
- Topic 2
- Topic 3

## Code Examples

Reference the code examples section for practical demonstrations.

## Further Reading

- Reading 1
- Reading 2
```

### Homework Template

Create `/archetypes/homework.md` for assignments:

```markdown
---
title: "{{ .Name }}"
description: "Assignment description"
weight: 10
dueDate: "2026-01-15"
difficulty: "intermediate"
draft: false
---

## Overview

Description of the assignment and its purpose.

## Requirements

1. Requirement 1
2. Requirement 2
3. Requirement 3

## Submission

- Format: ZIP file containing source code
- Deadline: [DATE]
- Submit to: [SUBMISSION PLATFORM]

## Grading Criteria

- Code functionality: 50%
- Code style and documentation: 30%
- Testing: 20%
```

### Resources Template

Create `/archetypes/resource.md`:

```markdown
---
title: "{{ .Name }}"
description: "Resource description"
draft: false
---

## Overview

Description of the resource and its purpose.

## Contents

- Item 1
- Item 2
- Item 3

## External Links

- [Link 1](https://example.com)
- [Link 2](https://example.com)
```

---

## Part 3: Hugo Configuration for Course Structure

### Update hugo.yaml with Course-Specific Settings

The existing `hugo.yaml` already has most settings configured. Ensure these are present for optimal course site functionality:

#### 1. Enable Taxonomy for Content Classification

Add to `hugo.yaml`:

```yaml
taxonomies:
  category: categories
  tag: tags
  difficulty: difficulties
  module: modules
```

This allows tagging assignments by difficulty and module for better organization.

#### 2. Configure Output Formats

Already configured in your file, but ensure JSON output is enabled for potential dynamic content loading:

```yaml
outputs:
  home:
    - HTML
    - RSS
    - JSON
```

#### 3. Markup Configuration

Your file includes markdown configuration with unsafe HTML rendering (needed for embedded code blocks and interactive content).

#### 4. Menu Configuration

Your existing menu configuration is appropriate:

```yaml
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
```

Optionally add a resources link.

---

## Part 4: Static Files Organization

### Directory Structure for Static Content

```
static/
├── images/
│   ├── course-banner.jpg
│   ├── instructor-avatar.jpg
│   └── module-icons/
│       ├── module-01-icon.svg
│       ├── module-02-icon.svg
│       └── ...
├── downloads/
│   ├── sample-code/
│   │   ├── hello-world.cpp
│   │   └── ...
│   └── handouts/
│       └── ...
└── css/
    └── custom-course.css (optional)
```

### Add Static Files Reference

In your `hugo.yaml` params section, add:

```yaml
params:
  # ... existing params ...
  staticFiles:
    sampleCodePath: "/downloads/sample-code/"
    handoutsPath: "/downloads/handouts/"
```

---

## Part 5: Development Workflow

### Building for Development

With Hugo server running, your site will rebuild automatically as you add content:

```bash
hugo server -D  # -D includes draft content
```

### Building for Production

When ready to deploy:

```bash
hugo
```

This generates the complete static site in the `public/` directory, ready for deployment to your FSU web server.

### Content Creation Workflow

1. Create a new module:
   ```bash
   hugo new modules/module-03-templates/_index.md
   ```

2. Create assignment within homework:
   ```bash
   hugo new homework/assignment-05/_index.md
   ```

3. Edit the generated file with your content

4. Test locally with `hugo server -D`

5. When ready to publish, remove `draft: true` from frontmatter

---

## Part 6: Zen Theme Customization

### Override Theme Templates

The Zen theme provides excellent defaults. To customize:

1. Create `/layouts/_default/list.html` to customize how module/homework listings appear
2. Create `/layouts/partials/header.html` to customize the site header
3. Create `/layouts/partials/footer.html` for footer customization

Example `/layouts/_default/list.html`:

```html
{{ define "main" }}
  <main>
    <h1>{{ .Title }}</h1>
    {{ .Content }}
    {{ range .Pages.ByWeight }}
      <article>
        <h2><a href="{{ .Permalink }}">{{ .Title }}</a></h2>
        <p>{{ .Summary }}</p>
      </article>
    {{ end }}
  </main>
{{ end }}
```

### Custom CSS

Create `/static/css/course-custom.css` and reference in `/layouts/partials/head.html`:

```html
<link rel="stylesheet" href="/css/course-custom.css">
```

---

## Part 7: Complete Initialization Checklist

- [ ] Initialize Hugo modules: `hugo mod get -u`
- [ ] Create `/content` directory structure
- [ ] Create section _index.md files (modules, homework, resources)
- [ ] Update `hugo.yaml` with taxonomy configuration
- [ ] Test build with `hugo server -D`
- [ ] Create sample module content
- [ ] Create sample homework assignment
- [ ] Add course images to `/static/images/`
- [ ] Verify menu navigation
- [ ] Test on different browsers
- [ ] Prepare deployment to FSU web server
- [ ] Set up post-deploy verification

---

## Part 8: Additional Considerations

### Performance Optimization

- Optimize images before adding to static directory
- Consider using Hugo's built-in image processing for responsive images
- Minify CSS/JS in production builds

### SEO and Accessibility

- The Zen theme has built-in accessibility features
- Ensure all content includes descriptive titles and summaries
- Use semantic HTML in custom layouts
- Test with screen readers

### Analytics and Tracking

Your `hugo.yaml` currently disables analytics. To enable Google Analytics:

```yaml
privacy:
  googleAnalytics:
    disable: false
    respectDoNotTrack: true
```

### Version Control

Ensure your `.gitignore` includes:

```
public/
resources/
go.mod
go.sum
```

---

## Quick Reference Commands

```bash
# Initialize modules
hugo mod get -u

# Local development server
hugo server -D

# Production build
hugo

# Create new content
hugo new modules/module-name/_index.md
hugo new homework/assignment-name/_index.md

# Check module dependencies
hugo mod graph

# Clean module cache (if needed)
hugo mod clean
```

---

## Resources

- Hugo Documentation: https://gohugo.io/
- Zen Theme Repository: https://github.com/frjo/hugo-theme-zen
- Hugo Modules Guide: https://gohugo.io/hugo-modules/use-modules/

---

## Summary

Your Hugo site for ISC5305 is now ready to be fully initialized. The configuration in `hugo.yaml` provides a solid foundation. Follow the initialization steps above to:

1. Install the Zen theme module
2. Create the content directory structure
3. Add section landing pages
4. Configure taxonomies for better organization
5. Build and test locally

The recommended content organization separates modules, homework, and resources for clarity while maintaining flexible structure for expansion as the course develops. The archetypes provide templates for consistency across new content creation.

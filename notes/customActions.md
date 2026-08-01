# Custom Actions & Reusable CI/CD

## Why Custom Actions Matter

- The DevOps community relies heavily on open-source, community-built tooling created for free by developers in their own time
- **Custom actions** let you extend that same spirit — packaging your own reusable pieces of CI/CD logic so they can be shared and reused across projects, rather than rewritten every time

## What Are Custom Actions?

**Custom actions** = reusable units of code that automate specific tasks in your CI/CD pipeline.

There are **three types**:

| Type | Description |
|---|---|
| **JavaScript actions** | Use Node.js to run JavaScript code directly |
| **Docker actions** | Run inside a container, defined via a Dockerfile |
| **Composite actions** | Combine multiple existing steps together into one reusable action, defined in YAML |

---

## How to Create a Custom Action

1. **Create a repository** to host the custom action.
2. **Define the action's metadata** in an `action.yaml` file (describes inputs, outputs, and what the action does).
3. **Write the actual code**, depending on the action type:
   - JavaScript action → logic lives in a file like `index.js`
   - Docker action → a `Dockerfile` plus any scripts needed inside the container
   - Composite action → multiple steps combined together, defined in YAML
4. **Publish the action to the GitHub Marketplace** — this makes it public and open source, available for anyone to reuse.

## Sharing & Reusing Custom Actions Across Projects

One of the biggest advantages of GitHub Actions: 
- once a custom action exists, it can be **reused across multiple workflows and repositories** — no rewriting required.

### Example usage syntax
```yaml
- name: Use custom action
  uses: owner/repository-name@v1
  with:
    who-to-greet: "CoderCo Team"
```

Breaking that down:
- `owner` — the GitHub account/organization that owns the action's repo
- `repository-name` — the repo where the action is stored
- `@v1` (or `@main`, or a branch/tag) — the version/branch of the action to use
- `with:` — passes input values into the action (here, `who-to-greet`)

---

## Benefits of Reusable Actions

1. **Consistency** — standardizes your CI/CD process across repositories; all projects follow the same steps and quality checks, reducing errors and making workflows more predictable.
2. **Efficiency** — saves significant time and effort; write the logic once, reuse it across as many projects/pipelines as needed instead of duplicating the same code repeatedly.

---

## Creating a Custom GitHub Action — Step-by-Step Guide

### Part 1 — Set Up the Repository

1. Create a **new public repository** to host the custom action, under your account/organization.

### Part 2 — Define the Action Metadata (`action.yml`)

2. Create a new file at the repo root called **`action.yml`**

### Part 3 — Write the Action's Code

3. Create a `dist/` folder (can be left empty for now — it will hold the compiled output later).
4. Initialize a Node.js project
5. Install the GitHub Actions toolkit library
6. Create **`index.js`** with the action's logic

### Part 4 — Compile and Push the Action

7. Install `ncc`, the tool used to bundle/compile Node actions into a single file:
8. Build the action:
9. (Optional) Generate a `licenses.txt` file if prompted/needed for compliance with bundled dependencies
10. Create a **`.gitignore`** file and add
11. Commit and push everything
12. On GitHub, the repo will automatically detect the `action.yml` file and offer to help you publish it to the **GitHub Marketplace** (optional step — makes it publicly usable by anyone).

### Part 5 — Use the Custom Action in a Workflow

13. In your **separate CI/CD repo**, create a new workflow file, e.g. `.github/workflows/custom.yaml`.
14. Define the workflow
15. Commit and push the workflow
16. Check the **Actions** tab — the workflow should run, checkout the code, then execute the custom action, printing the greeting (e.g. `Hello, Codeco Team!`) using the logic defined in `index.js`.

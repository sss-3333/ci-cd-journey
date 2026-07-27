# Advanced GitHub Actions — Notes

## Conditionals & Expressions

Two powerful tools for controlling pipeline behavior:

### Conditionals
- Control **when** a job or step should run, based on a certain criterion.
- Example: a step that runs only `if: success()` — i.e. only runs if previous steps passed.

```yaml
- name: Run tests
  run: python -m unittest discover
- name: Notify success
  if: success()
  run: echo "Tests passed on Python $(python --version)"
```

### Expressions
- Let you perform calculations, manipulate strings, and reference workflow context/data
- Referenced using `${{ }}` syntax
- Common references: `github.ref` (branch/ref that triggered the workflow), `github.event` (event payload), etc.

```yaml
- name: Show branch name
  run: echo "Running on branch ${{ github.ref }}"
```

---

## Matrix Builds & Parallel Testing

- **Matrix builds** let you run the same job across **multiple configurations in parallel** — e.g. multiple language versions, multiple OSes (Ubuntu, macOS, Windows).
- Useful for testing compatibility across environments without writing duplicate jobs.

### Syntax
Defined under `strategy` → `matrix`, at the same level as `runs-on` inside a job:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: [3.7, 3.8, 3.9]
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: ${{ matrix.python-version }}
```

- Reference the matrix value elsewhere in the job using `${{ matrix.<key> }}` — e.g. `${{ matrix.python-version }}` fed into the `setup-python` action's `with:` input.
- GitHub Actions will spin up a separate parallel job run for **each** value in the matrix list (in the example: 3 separate runs, one per Python version).

---

## Demo Walkthrough — Combined Concepts (Tips to Remember)

Demo context: built a simple Python app (`hello.py` with a `hello()` function) plus a unit test (`test_hello.py` using the `unittest` framework), then wired up a pipeline combining everything above

**Pipeline build order used in the demo:**
1. `name:` — name the workflow (e.g. `Combined CI`)
2. `on: push` — trigger on push
3. First job (`build`) → `runs-on: ubuntu-latest`
4. Add `strategy.matrix` with `python-version: [3.7, 3.8, 3.9]`
5. **Steps in order:**
   - Checkout code → `actions/checkout@v2`
   - Set up Python → `actions/setup-python@v2`, with `python-version: ${{ matrix.python-version }}` (⚠️ **must match the matrix key** — easy step to forget)
   - Install dependencies → `run:` step, `cd` into the app subfolder first (e.g. `cd chapter4`), upgrade pip, then conditionally install from `requirements.txt` **only if it exists**:
     ```bash
     cd chapter4
     python -m pip install --upgrade pip
     if [ -f requirements.txt ]; then pip install -r requirements.txt; fi
     ```
   - Run tests → `run:` step, `cd` into the subfolder again, then `python -m unittest discover`
   - Notify success → `run: echo "Tests passed..."`, gated with `if: success()`

**Practical tips worth remembering:**
- If your app code lives in a subfolder (e.g. `chapter4/`), every `run:` step needs its own `cd` into that folder — steps don't share a working directory by default beyond the repo root.
- When using a matrix, the version variable set in `strategy.matrix` **must be referenced** in the relevant `with:` block (e.g. `setup-python`) using `${{ matrix.<key> }}`, or the matrix has no actual effect on that step

---

## Error Handling with `set -e`

- `set -e` is a shell option: when placed at the start of a script, the script **stops executing immediately** if any command returns a **non-zero exit code**.

```bash
set -e
echo "before the script"
non-existent-command
echo "after the script"   # this line never runs if the command above fails
```

- Without `set -e`, a script would normally continue past a failing command. With it, execution halts right at the point of failure — the `after the script` echo is never reached.
- **Useful for:** catching errors immediately and avoiding unexpected/undefined behavior caused by silently continuing after a failure.
- **Caveat:** not every non-zero exit code represents a real error — sometimes a non-zero exit is expected and the script should continue. In those cases, avoid blanket `set -e` and instead handle errors more granularly (more targeted error handling covered in later lessons).

---

# Secrets, Encrypted Vars & Debugging with set -x

## Managing Secrets in GitHub Actions

**What secrets are:** sensitive pieces of information — API keys, passwords, tokens, credentials — that must never be exposed in your codebase.

**Why use them:** keeps sensitive data secure and out of source control, so it's never visible in commit history or the repo itself.

### How to create a repository secret
1. Go to your repository on GitHub
2. **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Give it a name (e.g. `MY_SECRET`) and a value
5. Click **Add secret**

Once created, the value is encrypted and stored securely by GitHub — it's never shown again in the UI, and it won't be printed in logs even if referenced in a workflow.

---

## Using Secrets in a Workflow

Reference a secret using: `${{ secrets.SECRET_NAME }}`

```yaml
name: CoderCo Secrets CI

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Use secret
        run: echo "The secret is ${{ secrets.MY_SECRET }}"
```

**Key behavior:** even though the step technically runs and picks up the secret's value, GitHub Actions automatically **masks it in the logs** — it will never print the actual value, only something like `***`. This is intentional and by design, to prevent accidental leaks.

### Using a secret as an environment variable
Secrets can also be passed into a step as an environment variable:

```yaml
- name: Secret as environment variable
  env:
    MY_SECRET: ${{ secrets.MY_SECRET }}
  run: echo "The secret is $MY_SECRET"
```

Same masking behavior applies — the value is still hidden in the log output, even when passed via `env`.

### Note on repository **Variables** (non-secret)
GitHub also has a separate **Variables** tab (next to Secrets, under Secrets and variables → Actions) for **non-sensitive** config values (e.g. flags, non-secret settings). These are referenced with `${{ vars.VARIABLE_NAME }}` and, unlike secrets, are **not masked** in logs — so don't put sensitive data in a variable, only in a secret.

---

## Debugging Scripts with `set -x`

`set -x` is a shell option that **prints every command to the terminal before it executes it** — extremely useful for following exactly what a script is doing step by step, especially when something isn't behaving as expected.

### Simple example
```bash
set -x
echo "this is a test"
x=10
echo "the value of x is $x"
```
With `set -x` enabled, each line is printed (prefixed with `+`) before it actually runs — showing the literal command executed, not just its output.

### More complex example (arithmetic)
```bash
set -x
echo "starting the script"
x=10
y=20
z=$((x + y))
echo "the value of z is $z"
```
With `set -x`, you can watch each step: `x` being set, `y` being set, the arithmetic expansion computing `z`, and finally the echo — useful for tracing exactly where a calculation or logic error occurs.

### Turning debugging off partway through
Use `set +x` to **disable** the debug trace for the rest of the script — useful if you only want to debug one specific section:

```bash
set -x
echo "this part gets debugged"
set +x
echo "after script"   # this line will NOT be traced/printed
```
Once `set +x` is hit, subsequent commands no longer show the `+`-prefixed trace line.

---

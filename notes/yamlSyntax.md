# YAML Syntax 

## What is YAML?

- **YAML** = "YAML Ain't Markup Language"
- - Officially: a **human-readable data serialization standard**, commonly used for configuration files
- Described as "the language of DevOps" — tools like Kubernetes and CI/CD pipelines rely on it heavily

## File Basics

- YAML files must use either the **`.yaml`** or **`.yml`** extension
- Most IDEs (VS Code, JetBrains, etc.) recognize the extension and provide YAML syntax highlighting

---

## The Three Core Concepts

### Key-Value Pairs
The fundamental building block of YAML — a key, a colon, and a value.

```yaml
# Key-value pairs
name: FooBar
age: 30
job: developer
```

### Lists
Sequences of items, defined with a **dash + space** before each item.

```yaml
fruits:
  - apple
  - banana
  - cherry
```

### Nested Elements (Nested Data Structures)
One of YAML's most powerful features — nesting elements using **indentation** (2 spaces or a tab, consistently).

```yaml
address:
  street: 123 Downing Street
  city: London
  country: United Kingdom
```


- YAML underpins configuration for most DevOps tooling (CI/CD pipelines, Kubernetes manifests, etc.)
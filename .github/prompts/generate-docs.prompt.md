---
description: Generate a documentation README for a language implementation
---

## Context

You are a technical writer documenting one of the language implementations in the **Supermarket Receipt Kata** repository. This repo contains the same domain model — a supermarket checkout system — implemented identically in 15+ languages. Each implementation lives in its own top-level directory (e.g. `python/`, `csharp/`, `java/`, `typescript/`).

The domain models a supermarket where customers add products to a shopping cart, a teller applies special offers, and a receipt is generated and printed.

## Action

Generate a **Markdown documentation file** for the language implementation that is currently open or explicitly mentioned. The document must contain exactly these three sections in order:

### 1. What Is This

A short paragraph (3–5 sentences) explaining:
- This is the **[Language]** implementation of the Supermarket Receipt Kata.
- The kata is a refactoring exercise: write tests for existing code, refactor it, then add new features (bundle discounts, HTML receipts).
- Link to the root README for the full kata description.

### 2. High-Level Architecture

A section describing how the codebase is structured. Include:
- A **file/folder listing** showing the project layout (source files, test files, config/build files).
- A **brief description of each core class/module** and its responsibility: `Product`, `ShoppingCart`, `Teller`, `Receipt`, `ReceiptPrinter`, `Discount`, `Offer`, `SupermarketCatalog` (and the fake/stub implementation).
- A **data flow summary** in 4–5 numbered steps: catalog setup → teller + offers → cart → checkout → receipt.
- Mention the test double (`FakeCatalog`) and where it lives.

### 3. Getting Started

Step-by-step instructions to get from a fresh clone to running tests:
1. **Prerequisites** — language runtime version, package manager, etc.
2. **Install dependencies** — the exact command(s).
3. **Build** (if applicable) — the exact command.
4. **Run tests** — the exact command and expected output summary.
5. **Run the text fixture** (if one exists) — how to execute it for a sample receipt.

Use the actual build/test commands for the language (check `README.md`, `pom.xml`, `package.json`, `Makefile`, `*.csproj`, etc.). Do not guess — read the project files to get the correct commands.

### Formatting rules

- Use ATX-style headings (`#`, `##`, `###`).
- Wrap commands in fenced code blocks with the appropriate language tag (```bash`, ```shell`).
- Keep the total document under 120 lines — concise and scannable.
- Do not include badges, license info, or contribution guidelines — keep it focused.

## Result

A single Markdown file (named `README.md` or as requested) placed in the language's directory that a new developer can read to understand the project structure and start running tests within 5 minutes.

## Example

The Getting Started section for a Python implementation might look like:

```markdown
## Getting Started

### Prerequisites
- Python 3.10+
- pip

### Install Dependencies
\```bash
pip install -r requirements.txt
\```

### Run Tests
\```bash
python -m pytest
\```

You should see all tests passing with output similar to:
\```
collected 1 item
tests/test_supermarket.py .  [100%]
1 passed in 0.02s
\```
```

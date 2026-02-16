---
description: Generate a Mermaid architecture diagram for a language implementation
---

## Context

You are an architect documenting one of the language implementations in the **Supermarket Receipt Kata** repository. This repo contains the same domain model — a supermarket checkout system — implemented identically in 15+ languages. Each implementation lives in its own top-level directory (e.g. `python/`, `csharp/`, `java/`, `typescript/`).

The domain models a supermarket where customers add products to a shopping cart, a teller applies special offers, and a receipt is generated and printed.

**Core entities** (present in all implementations):
- `Product` — item with name, unit (each/kilo)
- `SupermarketCatalog` — interface for price lookups (test implementations use `FakeCatalog`)
- `ShoppingCart` — accumulates items, calculates discounts
- `Teller` — orchestrates checkout, applies offers
- `Offer` — special discount rules (3-for-2, 10% off, etc.)
- `Discount` — calculated discount applied to receipt
- `Receipt` — line items + discounts
- `ReceiptPrinter` — formats receipt as 40-column text

**Data flow**:
1. Register products/prices in catalog
2. Teller holds catalog + configured offers
3. ShoppingCart accumulates items
4. `Teller.checksOutArticlesFrom(cart)` creates Receipt
5. `ShoppingCart.handleOffers()` calculates discounts
6. `ReceiptPrinter` formats output

## Action

Generate a **Mermaid diagram** that visualizes the architecture of the language implementation currently open or explicitly mentioned. The diagram must:

### 1. Choose the Right Diagram Type

Select **one** of these based on what best explains the architecture:
- **Class Diagram** (`classDiagram`) — show entities, relationships, key methods
- **Sequence Diagram** (`sequenceDiagram`) — show the checkout flow from cart to receipt
- **Flowchart** (`flowchart TD`) — show the data/control flow through the system
- **C4 Component Diagram** (`C4Component`) — show high-level component boundaries (if relevant)

Most implementations will benefit from either a **class diagram** or **sequence diagram**.

### 2. Include Essential Elements

Your diagram must clearly show:
- All **six core entities** listed above
- The **relationships** between them (associations, dependencies, composition)
- The **catalog interface** + test double pattern (`SupermarketCatalog` ← `FakeCatalog`)
- The **main flow**: Catalog → Teller → Cart → Receipt → Printer
- Key methods on at least 3 classes (e.g., `addItemQuantity()`, `checksOutArticlesFrom()`, `printReceipt()`)

### 3. Follow Mermaid Best Practices

- Use valid Mermaid syntax (test at https://mermaid.live if uncertain)
- Keep the diagram **focused** — omit utility/helper classes unless they're central
- Use **consistent naming** that matches the actual code (check file/class names)
- Add **notes or annotations** if the diagram needs clarification
- Limit to **8–12 nodes/classes** for readability
- Use proper Mermaid relationship syntax:
  - Class: `<|--` (inheritance), `*--` (composition), `o--` (aggregation), `-->` (association)
  - Sequence: `->>` (messages), `-->>` (returns)

### 4. Render the Diagram

**Use the `renderMermaidDiagram` tool** to visualize the architecture diagram immediately. Pass:
- `markup`: The Mermaid diagram syntax (just the diagram content, no code fences)
- `title`: A short title describing the diagram (e.g., "C# Supermarket Receipt Architecture")

Example tool call:
```
renderMermaidDiagram(
  markup: "classDiagram\n    class Product {...}\n    ...",
  title: "TypeScript Supermarket Receipt Architecture"
)
```

**Do NOT** output the diagram as a markdown code block — always render it using the tool so the user can see it immediately.

## Result

The rendered Mermaid diagram should:
- Visualize the complete architecture instantly in the editor
- Clearly explain the architecture to someone new to the codebase
- Match the actual structure of the code (use `grep`, `list_dir`, or `read_file` to verify names)
- Take 30–60 seconds to understand

If asked for multiple diagram types, render each one separately using the tool with descriptive titles.

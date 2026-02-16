---
description: Security and performance review of Supermarket Receipt code
---

## Context

You are a senior engineer performing a **security and performance review** on the **Supermarket Receipt Kata**. The codebase implements a supermarket checkout system in many languages. Each implementation shares the same domain: products, a catalog, a shopping cart, a teller that applies offers, receipts, and a receipt printer.

You may be reviewing code in any language present in the repository (C#, Java, Python, TypeScript, Go, Kotlin, etc.). Apply your analysis to whichever language is in scope — the file(s) currently open, selected, or explicitly mentioned.

## Action

Analyse the code for **security vulnerabilities** and **performance issues**. Organise your findings into two clearly separated sections.

### Security — check for:

1. **Input validation** — Are product names, quantities, and prices validated? Can negative quantities, NaN, or extremely large values cause incorrect behaviour or crashes?
2. **Injection risks** — Could unsanitised product names or descriptions be injected into output (HTML receipt, logs, database queries) without escaping?
3. **Integer/floating-point overflow** — Can price calculations overflow or lose precision in ways that produce incorrect totals? Are there unchecked casts (e.g. `(int) quantity`)?
4. **Denial of service** — Can a cart with millions of items or a catalog with unbounded products cause excessive memory use or CPU time?
5. **Sensitive data exposure** — Are prices, discounts, or receipt data logged or serialised in a way that could leak if the system were used in production?
6. **Dependency vulnerabilities** — Are dependencies pinned to safe versions? Are there known CVEs in the libraries or frameworks used?
7. **Error handling** — Do missing products, unknown offer types, or null/nil references cause unhandled exceptions instead of graceful failures?

### Performance — check for:

1. **Algorithmic complexity** — Is `HandleOffers` O(n·m) where n = products and m = offers? Could this be reduced?
2. **Redundant computation** — Are values recalculated in loops when they could be cached (e.g. `catalog.GetUnitPrice` called multiple times for the same product)?
3. **Unnecessary allocations** — Are collections copied defensively where a read-only view would suffice? Are strings concatenated in tight loops instead of using a builder?
4. **Data structure choices** — Are lookups done on lists where a dictionary/map/set would be O(1)? Is the product quantity map iterated efficiently?
5. **Receipt printing** — Does the printer allocate excessive intermediate strings? Could formatting be streamed instead of buffered?
6. **Scaling considerations** — How would the code behave with 10k products, 100k cart items, or 1k concurrent checkouts?

### For each finding, provide:

- **Location** — file and line/method.
- **Severity** — Critical / High / Medium / Low.
- **Description** — What the issue is and why it matters.
- **Recommendation** — A concrete fix or mitigation, with a code snippet where helpful.

### Additional guidelines:

- Do **not** flag issues that are clearly acceptable for a kata/exercise context (e.g. no authentication). Focus on issues that would matter if this code were adapted for production.
- Prioritise findings by severity — critical and high issues first.
- If no issues are found in a category, say so explicitly rather than inventing problems.
- Keep recommendations practical and incremental — avoid suggesting a full rewrite.

## Result

A structured review document with:

1. **Summary** — one-paragraph overview of the code's security and performance posture.
2. **Security Findings** — numbered list, ordered by severity.
3. **Performance Findings** — numbered list, ordered by severity.
4. **Positive Observations** — brief note on what the code already does well.
5. **Recommended Next Steps** — top 3 actions to improve the code, ordered by impact.

## Example

A single finding should look like:

> **S1 — Unchecked cast truncates quantity (Medium)**
>
> `ShoppingCart.HandleOffers` — `var quantityAsInt = (int) quantity;`
>
> Casting a `double` to `int` silently truncates. A quantity of `2.9` becomes `2`, which may skip a "3 for 2" discount the customer expected. Negative quantities are also silently accepted.
>
> **Recommendation:** Validate that quantity is positive at `AddItemQuantity`. Use `Math.Floor` or `Math.Round` explicitly and document the rounding policy.

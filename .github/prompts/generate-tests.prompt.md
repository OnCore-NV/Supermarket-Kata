---
description: Generate tests for the Supermarket Receipt Kata
---

## Context

You are a test engineer working on the **Supermarket Receipt Kata** — a codebase implemented in many languages that models a supermarket checkout. The domain includes:

- **Product** — an item with a name and unit (each, kilo).
- **Catalog** — maps products to unit prices (use a fake/stub in tests).
- **ShoppingCart** — collects products and quantities.
- **Teller** — applies offers from a catalog and produces a Receipt.
- **Receipt** — lists items (product, quantity, price, total) and discounts.
- **ReceiptPrinter** — formats a receipt as readable text.
- **SpecialOffer / Discount** — deals like "3 for 2", "x% off", "5 for €Y", "2 for amount".

## Action

Generate a suite of automated tests for the code in the current file or selection. Follow these rules:

1. **Structure every test as Arrange → Act → Assert** (Given-When-Then).
2. **Name each test after the behaviour it verifies**, e.g. `buy_two_toothbrushes_with_three_for_two_offer_gives_one_free` — never `testDiscount1`.
3. **One logical assertion per test.** Split separate properties into separate tests.
4. **Cover these scenarios:**
   - No-discount: single item, multiple units, multiple different items.
   - Each offer type in isolation: percentage discount, "X for Y", "X for amount", "buy N get 1 free".
   - Edge cases: quantity below threshold, at threshold, above threshold, zero quantity.
   - Multiple offers on different products in one cart.
   - Receipt totals equal item subtotals minus discounts.
   - ReceiptPrinter output: spot-check price, quantity, discount, and total formatting.
5. **Keep tests independent** — no shared mutable state.
6. **Use a fake catalog** (in-memory stub), never a real data source.
7. **Use minimal, intention-revealing test data**: product names like `apple`, `toothbrush`, `rice`; round prices where possible.
8. **Use exact numeric comparisons** for prices/totals. When floating-point precision is an issue, use the language-idiomatic approximate assertion (delta, `closeTo`, `assertAlmostEqual`, etc.).
9. **Test through the public API only** (e.g. `Teller.checksOutArticlesFrom`) — do not test private internals.
10. **If the bundle feature exists**, also cover: complete bundle → 10% off; incomplete bundle → no discount; surplus items → only one complete set discounted; bundles combined with standalone offers.
11. **If an HTML receipt printer exists**, also cover: same data/totals as plain-text receipt; valid HTML structure; keep rendering tests separate from business-logic tests.

## Result

A test file (or additions to an existing test file) that:

- Is written in the same language and test framework already used in the project.
- Compiles/runs green against the current production code.
- Provides high coverage of the business logic and edge cases listed above.
- Uses clear, descriptive test names and follows the Arrange-Act-Assert pattern throughout.

## Example

Below is a **pseudocode** example showing the expected shape of a single test — adapt syntax to the actual language and framework:

```
test "ten_percent_discount_applied_to_rice":
    # Arrange
    catalog = FakeCatalog()
    rice = Product("rice", ProductUnit.EACH)
    catalog.addProduct(rice, 2.49)

    teller = Teller(catalog)
    teller.addSpecialOffer(TenPercentDiscount, rice, 10.0)

    cart = ShoppingCart()
    cart.addItemQuantity(rice, 1)

    # Act
    receipt = teller.checksOutArticlesFrom(cart)

    # Assert
    expect(receipt.totalPrice()).toBeCloseTo(2.49 * 0.9)
    expect(receipt.discounts()).toHaveLength(1)
```

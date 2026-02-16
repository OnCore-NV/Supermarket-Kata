# Supermarket Receipt Kata — AI Coding Agent Instructions

## What This Is

A **refactoring kata** implemented identically in 15+ languages. Each language has the same domain model, the same intentional code smells, and the same minimal test. The goal is to write tests, refactor, then add features (bundle discounts, HTML receipts).

When asked to execute a task, always make sure to check if it's clear in which folder / language you're working. Do not make requested changes to the entire 15+ implemented language, only to the one the user is referencing. 

e.g. "refactor the shoppingcart.java class" means the user is focussing on java. In this case you want to make changes to the java application, and don't want to make changes to php, csharp, etc...

If it's unclear which language the user is focussing on, ask specifically.

## Domain Model & Data Flow

Six core entities appear in every implementation: `Product`, `ShoppingCart`, `Teller`, `Receipt`, `Discount`, `Offer`. The flow is:

1. Register products/prices in a `SupermarketCatalog` (Go: `Catalog`)
2. `Teller` holds the catalog + configured `Offer`s (via `addSpecialOffer`)
3. `ShoppingCart` accumulates items (tracks both individual items and aggregated quantity-per-product)
4. `Teller.checksOutArticlesFrom(cart)` → creates `Receipt` with line items, then delegates to `ShoppingCart.handleOffers()` for discount calculation
5. `ReceiptPrinter` formats the receipt as a fixed-width (40-column) text string

## Discount Sign Convention

Discount amounts are stored **negative** in Python, Java, C#, Go (added to total) but **positive** in TypeScript, Kotlin (subtracted from total). Both produce the same result. Match the convention of whichever language you're editing.

## Test Structure

Each language ships with **one minimal test** (intentionally sparse — the kata's exercise is to add more). The test verifies: catalog with toothbrush (€0.99) and apples (€1.99/kg), 10% discount on toothbrush, 2.5 kg apples in cart → total €4.975, no discounts applied.

Tests use a `FakeCatalog` test double (in-memory dict keyed by product name). It lives in the test directory except C# where it's in main source.

## Build & Test Commands

| Language | Directory | Test Command |
|----------|-----------|-------------|
| Python | `python/` | `python -m unittest` |
| Python (pytest) | `python_pytest/` | `python -m pytest` |
| TypeScript | `typescript/` | `npm test` |
| Java | `java/` | `mvn test` |
| Kotlin | `kotlin/` | `./gradlew test` |
| C# | `csharp/` | `dotnet test` |
| Go | `go/` | `go test ./supermarket/` |
| C/C++ | `c/` | `cmake --build build && ctest --test-dir build` |

Install deps first: `pip install -r requirements.txt` (Python), `npm install` (TS), `go mod download` (Go), `dotnet restore` (C#).

## Cross-Language Conventions

- All languages use the **same class/entity names** (`Product`, `Receipt`, `Discount`, `ShoppingCart`, `Teller`)
- The checkout method is `checksOutArticlesFrom` (camelCase/snake_case per language)
- `SupermarketCatalog` is an interface/abstract class everywhere (Go names it `Catalog`)
- Follow each language's idiomatic naming: snake_case (Python), camelCase (Java/TS/Kotlin/Go), PascalCase (C#)

## Key Files Per Language (Python as Reference)

- Domain model: `model_objects.py` — `Product`, `ProductUnit`, `SpecialOfferType`, `Offer`, `Discount`
- Cart + discount logic: `shopping_cart.py` — `ShoppingCart` with `handleOffers()`
- Orchestrator: `teller.py` — `Teller.checksOutArticlesFrom()`
- Output: `receipt.py`, `receipt_printer.py`
- Catalog interface: `catalog.py` — base class with methods that raise (simulates DB access)
- Test double: `tests/fake_catalog.py`

Other languages mirror this structure with idiomatic file/class naming.

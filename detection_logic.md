# Core Detection Logic & Expressions

The underlying detection engine utilizes Power Fx formula evaluations to parse inbound data objects without executing unsafe script interpreters or code dependencies.

---

## Core Formula
The main pattern analysis rule evaluates incoming strings using a logical `Or` statement combined with case-insensitive `IsMatch` properties:

```powerapps
Or(
  IsMatch(Lower(Text(Topic.CheckType)), "http"),
  IsMatch(Lower(Text(Topic.CheckType)), "urgent"),
  IsMatch(Lower(Text(Topic.CheckType)), "password"),
  IsMatch(Lower(Text(Topic.CheckType)), "bank"),
  IsMatch(Lower(Text(Topic.CheckType)), "click"),
  IsMatch(Lower(Text(Topic.CheckType)), "verify")
)
```

---

## Function Analysis & Purpose

* **`Topic.CheckType`**: The explicit application state variable holding the raw text input payload from the chat dialog session.
* **`Text()`**: Sanitizes and forces strict string typecasting on the text object to prevent compilation failures within the platform framework.
* **`Lower()`**: Performs standard data normalization, transforming all text into lowercase characters to handle uppercase signature evasion strategies cleanly.
* **`IsMatch()`**: Checks for character presence against known high-risk signature strings.
* **`Or()`**: Aggregates separate logical matching checks. If any single signature fires, the overall conditional check registers true, routing the session instantly to a risk tier.

---

## Debugging Metrics & Iterative Refinement

During initial prototype configurations, early compilation tests showed high false-negative metrics, routing suspicious text blocks directly to "Safe." 

1. **The Issue:** The initial engine evaluated raw strings via standard text comparison variables, breaking whenever complex casing or unusual sentence structures appeared.
2. **The Fix:** Implemented data-type forcing via `Text()` wrapped completely within an operational `Lower()` envelope.
3. **Indicator Scaling:** Scaled out the base dictionary to separate foundational behavioral signatures (like urgency prompts) from explicit data collection hooks (like financial or credential targets), vastly stabilizing classification accuracy across boundary test cases.

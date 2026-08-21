# System Architecture & Processing Pipeline

PhishShield processes data through an explainable, data-flow pipeline designed to ingest unstructured communications, normalize character arrays, and return structured security recommendations.

---

## The 4-Layer Architecture

### 1. Ingestion Layer (Input)
* The user interacts with the bot via a conversational chat user interface.
* The system requests the communication channel type (`Email` or `Message`).
* The unstructured message body is captured and stored securely in the scoped variable `Topic.CheckType`.

### 2. Normalization Engine (Processing)
* Raw user text contains unpredictable casing and character boundaries.
* The processing pipeline pipes the string data through `Text()` formatting and the `Lower()` function.
* This neutralizes uppercase evasion attempts used by attackers (e.g., matching "URGENT", "Urgent", or "urgent" equally).

### 3. Hierarchical Decision Matrix (Logic)
* The normalized text runs through an array of boolean flags using pattern matching.
* The classification relies on a prioritized triage chain:
  * **High Risk:** Triggered when explicit credential requests, high-theft keywords, or link payloads (`http`) are discovered.
  * **Medium Risk:** Triggered by behavioral red flags like urgency metrics and persuasive social engineering language.
  * **Safe:** Terminal route when zero high or moderate indicators match the string.

### 4. Presentation Layer (Output)
* Displays a clear risk verdict to the user.
* Generates a structural pattern breakdown highlighting the specific text anomalies that raised the alert level.
* Outputs immediate triage instructions (e.g., *"Do not interact with links or submit sensitive details"*).
* Offers an optional conversational loop providing foundational cybersecurity awareness training cards.

---

## Visualized System Workflow
*(To see the complete architecture diagram or live UI captures, navigate to the `/docs` directory or main repository view).*

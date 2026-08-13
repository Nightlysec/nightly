### Overview

To help make Nightly better for everyone, we collect anonymized data that helps us understand how to better improve our AI security agent for our users, guide the addition of new features, and fix common errors and bugs. This feedback loop is crucial for improving Nightly's capabilities and user experience.

We use [Supabase](https://supabase.com) (Postgres, via a write-only anon/publishable key gated by row-level security) for data collection and storage, along with [Scarf](https://scarf.sh) for gateway analytics — though the Scarf gateway isn't a registered endpoint for this fork yet, so those sends currently fail closed rather than reaching anyone. Our telemetry implementation is fully transparent - you can review the source code ([supabase_events.py](https://github.com/nightlysec/nightly/blob/main/nightly/telemetry/supabase_events.py), [scarf.py](https://github.com/nightlysec/nightly/blob/main/nightly/telemetry/scarf.py)) to see exactly what we track. The `telemetry_events` table only accepts inserts from the publishable key used by the client — it cannot select, update, or delete rows, so the data is a one-way beacon even to someone holding that key.

### Telemetry Policy

Privacy is our priority. All collected data is anonymized by default. Each session gets a random UUID that is not persisted or tied to you. Your code, scan targets, vulnerability details, and findings always remain private and are never collected.

### What We Track

We collect only very **basic** usage data including:

**Session Errors:** Duration and error types (not messages or stack traces)\
**System Context:** OS type, architecture, Nightly version\
**Scan Context:** Scan mode (quick/standard/deep), scan type (whitebox/blackbox)\
**Model Usage:** Which LLM model is being used and whether it runs via an API key or a model subscription (not prompts or responses)\
**Feature Usage:** Which built-in skills are loaded\
**Aggregate Metrics:** Vulnerability counts by severity and weakness category (CWE)

### What We **Never** Collect

- Usernames, or any identifying information
- Scan targets, file paths, target URLs, or domains
- Vulnerability details, descriptions, or code
- LLM requests and responses

### How to Opt Out

Telemetry in Nightly is entirely **optional**:

```bash
export NIGHTLY_TELEMETRY=0
```

You can set this environment variable before running Nightly to disable **all** telemetry.

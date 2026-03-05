<div align="center">

# TENS Lab

### Building the protocol layer of machine intelligence.

We build open-source infrastructure that makes AI **cheaper, faster, and more accurate** — starting with the data format layer.

---

**Our core thesis:** LLMs waste 30–60% of tokens on JSON syntax. We fix that at the protocol level.

</div>

---

## The Problem

Every time you send structured data to an LLM, you're paying for brackets, quotes, repeated keys, and commas that carry **zero information**. At scale, this costs companies thousands in unnecessary token spend.

## The Solution: Contex

**Contex** is an LLM-native structured data protocol that compresses your data **before** it reaches the tokenizer.

```
JSON (325 tokens):                          Contex Compact (65 tokens):
┌──────────────────────────────────────┐    ┌─────────────────────────────────┐
│ [                                    │    │ @enum role: a=admin u=user      │
│   { "id": 1,                         │    │ @d  Engineering                 │
│     "name": "Alice",                 │    │ id  name     role  dept  salary │
│     "role": "admin",                 │    │ 1   Alice    a     @0    120000 │
│     "department": "Engineering",     │    │ 2   Bob      u     Mkt   85000  │
│     "salary": 120000 },              │    │ 3   Charlie  a     @0    135000 │
│   { "id": 2, ... },                  │    └─────────────────────────────────┘
│   { "id": 3, ... }                   │
│ ]                                    │    → 80% fewer tokens
└──────────────────────────────────────┘    → Identical LLM accuracy
```

## Validated by All 3 Major LLMs

We sent identical data as JSON and Contex Compact to ChatGPT, Gemini, and Claude. **All three gave identical answers.**

| LLM | Savings | What They Said |
|:----|:--------|:---------------|
| **ChatGPT** | **69%** | *"Columnar layout + dictionary encoding + enum compression = very good design for LLM interaction"* |
| **Gemini** | **57%** | *"Dense structure — the model sees the header once and values follow a predictable grid"* |
| **Claude** | **60%** | *"Eliminates the three biggest token wastes in JSON"* |

## Our Stack

Everything is MIT-licensed and ships as `@tens-lab/*` on npm.

| Package | What It Does |
|:--------|:-------------|
| [`@tens-lab/core`](https://github.com/TENS-LAB/contex/tree/main/packages/core) | Canonical IR encoder, formatters, TokenMemory |
| [`@tens-lab/engine`](https://github.com/TENS-LAB/contex/tree/main/packages/engine) | Budget packer, query engine, model registry (28 models) |
| [`@tens-lab/middleware`](https://github.com/TENS-LAB/contex/tree/main/packages/middleware) | OpenAI · Anthropic · Gemini drop-in wrappers |
| [`@tens-lab/cli`](https://github.com/TENS-LAB/contex/tree/main/packages/cli) | CLI tools + benchmark suite (21 datasets) |
| [`@tens-lab/server`](https://github.com/TENS-LAB/contex/tree/main/packages/server) | Hono REST API gateway |
| [`@tens-lab/tens-wasm`](https://github.com/TENS-LAB/contex/tree/main/packages/tens-wasm) | Rust → WASM encoder for speed + privacy |
| [`@tens-lab/adapters`](https://github.com/TENS-LAB/contex/tree/main/packages/adapters) | Vercel AI SDK adapter |

## Quick Start

```bash
npm install @tens-lab/core @tens-lab/middleware
```

```typescript
import OpenAI from 'openai';
import { createContexOpenAI } from '@tens-lab/middleware';

const client = createContexOpenAI(new OpenAI(), {
  data: { users: myLargeDataset }
});

// Use as normal — data is automatically compressed
await client.chat.completions.create({
  model: 'gpt-4o-mini',
  messages: [{ role: 'user', content: 'Analyze: {{CONTEX:users}}' }],
});
```

## Numbers

| Metric | Value |
|:-------|:------|
| Avg savings | **72%** across 21 dataset types |
| Best case | **90%** on deep nested structures |
| Accuracy | **20/20** roundtrip fidelity |
| Tests | **688+** across 7 packages |
| Models supported | **28** across 9 providers |

---

<div align="center">

**[→ View the main repo](https://github.com/TENS-LAB/contex)** · **[Read the docs](https://github.com/TENS-LAB/contex/tree/main/docs)** · **[TENS Specification](https://github.com/TENS-LAB/contex/blob/main/docs/tens-specification.md)**

*An LLM-native structured data protocol, not just compression.*

</div>

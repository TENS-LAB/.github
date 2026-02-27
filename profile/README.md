<div align="center">

# TENS Lab

**Building the protocol layer of machine intelligence.**

</div>

We build open-source infrastructure for AI token optimization. Our core project, [Contex](https://github.com/tens-lab/contex), is a structural compiler that reduces LLM token usage by 46–90%.

## Projects

| Repo | Package | What it does |
|------|---------|-------------|
| [contex](https://github.com/tens-lab/contex) | `@tens-lab/core` `@tens-lab/engine` | Token compiler — TENS IR encoder, budget packer, query engine |
| [middleware](https://github.com/tens-lab/middleware) | `@tens-lab/middleware` | Drop-in wrappers for OpenAI, Anthropic, Gemini |
| [contex-server](https://github.com/tens-lab/contex-server) | `@tens-lab/server` | REST API gateway with provider proxy |
| [contex-cli](https://github.com/tens-lab/contex-cli) | `@tens-lab/cli` | CLI for analysis, validation, benchmarking |
| [tens-wasm](https://github.com/tens-lab/tens-wasm) | `@tens-lab/tens-wasm` | Rust WASM encoder (2-5x faster) |
| [adapters](https://github.com/tens-lab/adapters) | `@tens-lab/adapters` | Vercel AI SDK adapter |

## Get started

```bash
npm install @tens-lab/core @tens-lab/middleware
```

```typescript
import OpenAI from 'openai';
import { createContexOpenAI } from '@tens-lab/middleware';

const client = createContexOpenAI(new OpenAI(), {
  data: { users: myData }
});
```

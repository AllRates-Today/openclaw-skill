# AllRatesToday skill for OpenClaw

Exchange rates your [OpenClaw](https://github.com/openclaw/openclaw) agent fetches instead of remembers: the ECB daily reference table, the official tables of 120+ central banks and tax authorities, and (with a key) live mid-market rates for 160+ currencies.

```bash
openclaw skills install allratestoday
```

**No API key needed to start.** The skill works keyless out of the box.

## 🚀 What the agent learns

- **Never answer a rate from memory.** Every rate in a model's training data is stale; the skill makes the agent fetch and quote the publication date.
- **Latest table for any base currency**, keyless: `GET https://allratestoday.com/api/open/latest/{BASE}`, the European Central Bank reference table re-based server-side, about 30 currencies, updated each ECB business day.
- **Official table of a named institution**, keyless: `GET https://allratestoday.com/api/open/central-bank/{code}` for `ecb`, `hmrc`, `rbi`, `sama`, `cbuae`, `boj`, `cbr`, `tcmb`, `bok`, `rbnz` and [120 more](https://allratestoday.com/central-bank-rates-api/institutions/). The skill explains row direction (`1 base = value quote`), rate types (`reference` before `middle` before `buy`/`sell`) and the `stale` flag.
- **Market vs official.** Tax, VAT, customs and audit work needs the rate the named authority published, not the market rate. The two routinely differ by a percent or more.
- **web_fetch first, curl fallback**, following OpenClaw's own skill conventions.

## 🏁 Quick start

Ask the agent things like:

| You say | What the agent does |
|---|---|
| "Convert 1,500 USD to EUR" | Fetches `/api/open/latest/USD`, multiplies, quotes the table date |
| "What did the ECB publish for GBP today?" | Fetches `/api/open/central-bank/ecb`, reads the GBP row |
| "HMRC rate for an October invoice in AED" | Fetches `/api/open/central-bank/hmrc`, picks the GBP→AED monthly row |
| "SAMA's official USD/SAR rate" | Fetches `/api/open/central-bank/sama`, picks the `reference` row |

Example response the agent reads:

```json
{"result":"success","base_code":"USD","date":"2026-10-29","rates":{"EUR":0.8985,"GBP":0.7509,"JPY":158.06}}
```

## 🔑 Optional API key

A free key from <https://allratestoday.com/register> unlocks live mid-market rates for 160+ currencies and multi-target lookups. Set it once; the skill reads it from the environment and never prints it.

```bash
export ALLRATESTODAY_API_KEY=art_live_...
```

```bash
curl -H "Authorization: Bearer $ALLRATESTODAY_API_KEY" \
  "https://allratestoday.com/api/v1/rates?source=USD&target=EUR,GBP,JPY"
```

Historical and dated rates are paid-plan endpoints. The skill tells the agent to say so on a `403` rather than guess.

## 🛡️ Attribution and terms

Keyless use requires a visible link to AllRatesToday wherever the rates are shown to others:

```html
<a href="https://allratestoday.com">Exchange rates by AllRatesToday</a>
```

The keyless central-bank tables are for personal and non-commercial use; commercial use of official tables needs a paid plan. Full wording: <https://allratestoday.com/terms/>.

## 📦 Repository layout

```
allratestoday/
└── SKILL.md   # the skill, in OpenClaw AgentSkills format
```

Published on ClawHub as [`allratestoday`](https://clawhub.ai/skills/allratestoday). To republish after a change:

```bash
clawhub skill publish ./allratestoday --version 1.0.1
```

## 🤖 Other agent integrations

- Hosted MCP server (Claude, ChatGPT, Cursor, VS Code, Codex, Gemini CLI): <https://allratestoday.com/mcp/>
- Claude Code plugin: [AllRates-Today/claude-code-plugin](https://github.com/AllRates-Today/claude-code-plugin)
- npm MCP servers: [`@allratestoday/mcp-server`](https://www.npmjs.com/package/@allratestoday/mcp-server), [`@allratestoday/central-bank-mcp`](https://www.npmjs.com/package/@allratestoday/central-bank-mcp)

## 🔗 Links

- [API Documentation](https://allratestoday.com/docs/)
- [Register (Free)](https://allratestoday.com/register)
- [Dashboard](https://allratestoday.com/profile)
- [Status](https://allratestoday.com/status)
- [Postman collection](https://allratestoday.com/postman/)
- [GitHub](https://github.com/AllRates-Today/openclaw-skill)

## 📜 License

MIT

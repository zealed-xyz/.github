<div align="center">

<img src="./banner.svg" alt="Zealed — Approved by them. Opened by you." width="100%" />

<br /><br />

[![website][website-shield]][website-link]
[![github stars][github-star]][github-link]
[![built on Zcash][zcash-shield]][zcash-link]
[![Bitrefill][bitrefill-shield]][bitrefill-link]
[![license][license-shield]][license-link]

</div>

### 👋 Welcome to Zealed

We're building **private mission rewards**: complete event missions and choose a gift card, eSIM, testnet Zcash or custom prize. Reward codes are sealed to your browser.

Events and sponsors want to reward people for showing up and taking part, but today every reward funnel turns participants into a data trail: emails, wallets, purchases and redemption codes all sitting in someone's database. **Zealed** is our answer: an organizer funds a campaign, participants complete a mission, an AI agent picks a perk they can actually use, the sponsor pays for that exact purchase with a **shielded Zcash** payment, and **Bitrefill** delivers it. The code is sealed to the participant's browser key the moment it arrives, so Zealed's database, logs and AI never hold it in readable form.

Built on **Zcash** (NU7 testnet, ZIP-321, zkool watch-only) and **Bitrefill** (REST API v2) at the **Free Society Hack 2026** in Mumbai, for the Privacy and Agentic Commerce tracks. Everything below is open source and runs on testnet today.

> *A little knowledge. A useful reward. Approved by them. Opened by you.*

### ⭐️ Our Projects

| Project | Description |
| :--- | :--- |
| [**🔏 Zealed**][zealed-github] | The app: organizer campaign builder, participant mission player, reward agent, Zcash payment watcher, Bitrefill fulfillment and sealed delivery. [![][website-shield]][website-link] |
| [**🌐 Live app**][website-link] | The public testnet deployment. Bitrefill purchases run in test mode; sponsors start with **Start funding**. |

### 📦 Ecosystem

| Path | Purpose | Language |
| :--- | :--- | :---: |
| [**zealed**][zealed-github] | Next.js 16 app: organizer and participant UI, API routes, watcher, scripts | ![][lang-typescript] |
| `app/` | Campaign builder, mission player, claim, gift and order pages, Noir Wallet pay flow | ![][lang-typescript] |
| `lib/campaigns.ts` · `lib/program.ts` | Campaigns with a budget, a 1–5 product allowlist and one mission; one-use hashed invites | ![][lang-typescript] |
| `lib/reward-agent.ts` · `lib/guardrails.ts` | Claude Haiku (AI SDK) picks a perk index from the bounded allowlist, never a free-form purchase | ![][lang-typescript] |
| `lib/zip321.ts` · `lib/zcash-zkool.ts` | ZIP-321 payment requests and watch-only payment detection over `zkool_graphql` | ![][lang-typescript] |
| `lib/bitrefill.ts` · `lib/fulfill.ts` | Bitrefill API v2 purchases and fulfillment reconciliation | ![][lang-typescript] |
| `lib/sodium.ts` · `lib/keystore.ts` · `lib/gift-envelope.ts` | libsodium browser keypair, `crypto_box_seal` delivery, IndexedDB keystore, sealed gift links | ![][lang-typescript] |
| `worker/watcher.ts` | Background watcher that matches shielded payments to orders | ![][lang-typescript] |
| `scripts/` | zkool setup and pay, FROST setup, keygen, attendee import, end-to-end reward run | ![][lang-typescript] |

### 🏗️ How It Fits Together

```
     Organizer / Sponsor                          Participant (browser)
            │                                             │
  create campaign · budget            open invite (QR / link) · libsodium keypair
  allowlist (1–5 perks) · mission     take the quiz (checked on the backend)
            │                                             │
            ▼                                             ▼
    ┌────────────────┐   one-use hashed invite   ┌──────────────────────┐
    │  Zealed API    │ ◀──────────────────────── │  mission player      │
    │  campaigns ·   │                           │  public key only     │
    │  orders (SQLite│ ──── reward agent ──────▶ │                      │
    │  + Drizzle)    │  Claude Haiku picks index └──────────▲───────────┘
    └───────┬────────┘                                      │ sealed code
            │ ZIP-321 request                               │ (crypto_box_seal)
            ▼                                               │
   Sponsor pays shielded TAZ ──▶ zkool_graphql watch-only watcher
                                        │ payment matched by memo
                                        ▼
                                 Bitrefill API v2 ──▶ gift card / eSIM
```

**Per-reward flow** (*Mission → Match → Receive*): the participant completes a short learning mission from their sponsor and the mission → the backend verifies the answers → the agent compares the allowed packages with the participant's needs and the sponsor's budget, and picks one → the sponsor approves and pays a ZIP-321 shielded request → the watch-only watcher sees the payment and matches it to the order → Bitrefill fulfills → the code is sealed to the participant's public key and stored in IndexedDB, so only their browser can open it.

### 🎯 What Makes Zealed Different

| | A typical reward / promo funnel | Zealed |
| :--- | :--- | :--- |
| Identity | Email, phone or wallet collected | A one-use invite and a browser keypair |
| Payment | Card or transparent on-chain transfer | Shielded Zcash, matched to the order by an encrypted memo |
| Who sees the code | The platform, its logs, support staff | Only the participant's browser (`crypto_box_seal`) |
| Agent spending | Open-ended or none | Bounded: an index into a 1–5 product allowlist, sponsor-approved |
| Payment detection | Custodial wallet | Watch-only viewing over `zkool_graphql`; no spending key on the server |
| Fulfillment | Manual codes | Bitrefill API v2 gift cards and eSIMs |

### 🔐 Core Features (current release)

- **Campaigns with hard limits:** budget, an allowlist of 1–5 Bitrefill products and one mission per campaign.
- **One-use hashed invites:** shared as a QR or link; only the hash is stored.
- **Backend-checked missions:** quiz answers are verified server-side before any reward.
- **Bounded reward agent:** Claude Haiku via AI SDK chooses a perk index, never an arbitrary purchase.
- **Shielded sponsor payments:** ZIP-321 requests paid in shielded TAZ, detected by a watch-only zkool watcher.
- **Bitrefill fulfillment:** gift cards and eSIMs bought through Bitrefill REST API v2.
- **Sealed delivery:** codes sealed with libsodium `crypto_box_seal` to the participant's key and kept in IndexedDB.
- **Sealed gift links:** pass a reward on without revealing it to Zealed.

### 🔗 On-Chain (Zcash testnet)

| | |
| :--- | :--- |
| **Mission reward payment** | [`1843616532cee751cbe556e902723d47b6675b6c92300b24070b8b6d1961f9ca`][mission-tx-link] (height 4581978) |
| **Bitrefill test invoice (eSIM)** | `7d8e4569-daa5-4de9-9a35-518b7d42c47f` |
| **Network** | Zcash NU7 testnet |
| **Explorer** | [testnet.zcashexplorer.app][explorer-link] |
| **Faucet** | [fauzec.com][faucet-link] (TAZ) |

More invoices and the full transaction evidence live in the [Zealed README][evidence-link].

### 🚀 Try It

```text
Live app:      https://tryzealed.xyz   (testnet · Bitrefill test mode)
Run locally:   git clone https://github.com/zealed-xyz/zealed && cd zealed
               pnpm install && pnpm dev
Testnet TAZ:   https://fauzec.com
```

Open your invitation link or scan your sponsor's QR code. Keep it private: whoever claims it first reserves the reward. Delivery is encrypted for your browser, and your activation details stay sealed until you reveal them.

### 🤝 Contributing

We welcome PRs, bug reports and design feedback.

- **Campaigns and missions:** `lib/campaigns.ts`, `lib/program.ts`, `app/program-*.tsx`.
- **Agent and guardrails:** `lib/reward-agent.ts`, `lib/guardrails.ts`.
- **Zcash:** `lib/zip321.ts`, `lib/zcash-zkool.ts`, `worker/watcher.ts`, `scripts/zkool-*.ts`.
- **Bitrefill and delivery:** `lib/bitrefill.ts`, `lib/fulfill.ts`, `lib/sodium.ts`, `lib/gift-envelope.ts`.
- **Docs:** the [README][zealed-github] and [`SPEC.md`][spec-link].

### 🪪 License

[MIT][license-link].

---

> [!WARNING]
>
> **Testnet only.** Hackathon prototype on Zcash testnet. TAZ has no value and Bitrefill purchases run in test mode, so codes can't be redeemed. Not audited. Not a financial product.

> [!TIP]
>
> **Start here:** open the [live app][website-link] → create a campaign with a mission and up to 5 perks → share the invite QR → complete the mission as a participant → pay the shielded request → open the code that only your browser can read.

[website-link]: https://tryzealed.xyz
[website-shield]: https://img.shields.io/badge/website-tryzealed.xyz-f4b728?labelColor=111111&style=flat-square&logo=safari&logoColor=white

[github-link]: https://github.com/zealed-xyz
[github-star]: https://img.shields.io/github/stars/zealed-xyz?color=f4b728&labelColor=111111&style=flat-square&logo=github

[zcash-link]: https://z.cash
[zcash-shield]: https://img.shields.io/badge/built%20on-Zcash-f4b728?labelColor=111111&style=flat-square&logo=zcash&logoColor=white

[bitrefill-link]: https://docs.bitrefill.com
[bitrefill-shield]: https://img.shields.io/badge/Bitrefill-API%20v2-f4b728?labelColor=111111&style=flat-square

[license-shield]: https://img.shields.io/badge/license-MIT-f4b728?labelColor=111111&style=flat-square
[license-link]: https://github.com/zealed-xyz/zealed/blob/main/LICENSE

[zealed-github]: https://github.com/zealed-xyz/zealed
[spec-link]: https://github.com/zealed-xyz/zealed/blob/main/SPEC.md
[evidence-link]: https://github.com/zealed-xyz/zealed#testnet-transaction-evidence

[mission-tx-link]: https://testnet.zcashexplorer.app/transactions/1843616532cee751cbe556e902723d47b6675b6c92300b24070b8b6d1961f9ca
[explorer-link]: https://testnet.zcashexplorer.app/
[faucet-link]: https://fauzec.com/

[lang-typescript]: https://img.shields.io/badge/typescript-f4b728?labelColor=111111&style=flat-square&logo=typescript&logoColor=white

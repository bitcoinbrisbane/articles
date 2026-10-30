# I Gave My Mate's CoinSpot Export to Claude. Do We Still Need Crypto Tax Software?

<!-- DRAFT. Placeholders in [SQUARE BRACKETS]. Pricing for Summ and Koinly to be filled from Lucas's research; results from the Sarah test. -->

It's tax time, and my friend Sarah asked me the question every Australian with a bit of crypto asks around now: "What do I actually owe?" Sarah is what I affectionately call a normie. She isn't a trader. She buys and sells a little on [CoinSpot](https://www.coinspot.com.au/), like plenty of Australians, and she has never heard of a cost base.

The standard answer is to sign up for a crypto tax product, upload your exchange history and pay for the report. Instead, I gave her CoinSpot CSV to an LLM with one prompt. It finished in a few minutes.

That made me ask a bigger question. If a one-off prompt can do what a subscription does, does software as a service still make sense for this job? So I ran a proper test: Sarah's real CoinSpot history, through [Summ](https://summ.com/au) (formerly Crypto Tax Calculator), [Koinly](https://koinly.io/), [Syla](https://www.syla.com.au/) and Claude, comparing price and accuracy.

## 1. The market has already started asking

This isn't only a crypto question. Adobe has spent 2026 being repriced as investors decide AI tools have eroded the moat around Photoshop. The stock was [down 24% in 2026 amid AI concerns](https://startupmap.iamsterdam.com/news/feed/adobe-stock-down-24-in-2026-amid-ai-concerns-but-trading-at-discounted-15x-earnings-multiple), and has slid [more than 60% from its 2024 peak](https://www.pulse.bot/enterprise/news/adobe-stock-slides-over-60-from-2024-peak-becomes-cheapest-in-years-for-enterprise-buyers-febc2973-003e-4f4b-81b3-fcf86963ef9a/), with Wall Street [questioning its defence against AI competition](https://247wallst.com/investing/2026/02/18/adobe-stock-drops-26-as-wall-street-questions-its-defense-against-ai-competition/).

Adobe still makes plenty of money. The market is questioning how much a specialised software subscription is worth once a general model can do much of the same work. If that applies to Photoshop, it applies to narrower products too.

## 2. What crypto tax software actually does

For a retail investor, the core job is well-defined. The ATO treats crypto as a CGT asset, and [selling, swapping or spending it is generally a CGT event](https://www.ato.gov.au/Individuals/Investments-and-assets/Crypto-asset-investments/How-to-work-out-and-report-CGT-on-crypto/). The software has to:

- read every buy, sell and swap from the exchange export
- match each disposal to the parcels it came from to get a cost base
- include fees in the cost base and proceeds
- apply the [50% CGT discount](https://www.ato.gov.au/Individuals/Capital-gains-tax/CGT-discount/) where an individual held the asset for at least 12 months
- net off capital losses and produce one figure for the tax return

That's a set of clear rules applied to a structured file, which is exactly what current LLMs are good at.

## 3. The test

**Input:** Sarah's full CoinSpot transaction history CSV, [N] transactions from [YEAR] to 30 June 2026. [CONFIRM: OK to use her first name?]

**Tools:**

1. Summ, on the cheapest plan that covers her transaction count
2. Koinly, on the cheapest plan that covers her transaction count
3. Syla, on the cheapest plan that covers her transaction count
4. Claude [MODEL(S)], using one prompt (in the appendix, so you can repeat it)

**Measures:** price for one financial year, time from upload to report, and whether the net capital gain matches a hand-checked figure.

## 4. What it costs

All three tax products follow the same model. You can import your transactions and preview your gains for free, and you pay when you want the report you actually lodge with. [Koinly's free plan](https://www.finder.com.au/cryptocurrency-tax/koinly) shows a capital gains preview but won't export a tax report. [Syla's free plan](https://help.syla.com.au/en/articles/8282736-choosing-the-best-plan-for-you) lets you import and view dashboards, and you upgrade "when you're ready to download reports."

Price is set by transaction count:

| Transactions | Summ | Koinly | Syla |
|---|---|---|---|
| Entry plan | A$49 (100 txns) | $69 (100 txns) | A$49 (200 txns) |
| Next tier | A$129 (1,000 txns) | $149 (1,000 txns) | A$129 (2,000 txns) |
| Mid tier | A$249 (10,000 txns) | $299 (3,000 txns) | A$249 (20,000 txns) |
| Top listed tier | A$399 (100,000 txns) | $399 (10,000+ txns) | A$399 (100,000 txns) |

Sources: [Summ pricing via Finder](https://www.finder.com.au/cryptocurrency-tax/summ-review), [Koinly pricing via Finder](https://www.finder.com.au/cryptocurrency-tax/koinly), [Syla pricing](https://www.syla.com.au/pricing). Each is an annual subscription. [CHECK: Summ's own pricing page currently shows more than one price set, and Finder notes Summ's limit is cumulative across all tax years, not per year.]

The transaction limits matter more than the headline price. Sarah has [N] transactions, which puts her on the [TIER] plan with each product, at [$] to [$] a year.

**The upsell.** [LUCAS: describe what Syla pushed you towards during the test, e.g. screens, prompts, emails.] Syla's higher tiers are sold on tax savings. Its Tax Saving plan adds a choice of parcel method and an "LTFO" optimisation, and Syla's own help article says that plan is [worth it when the tax saving exceeds the subscription](https://help.syla.com.au/en/articles/8282736-choosing-the-best-plan-for-you). That's a fair pitch for a big portfolio. For Sarah, [whether it changed her number at all].

**Claude.** Billed by the token, there's no tier. At [current API prices](https://platform.claude.com/docs/en/about-claude/pricing), Claude Opus 5.5 costs US$4 per million input tokens and US$20 per million output tokens. Sonnet 5.5 is US$2 and US$10, and Haiku 5.5 is US$0.10 and US$0.50. A CoinSpot export of a few hundred rows plus a full CGT table out is tens of thousands of tokens, so a run costs well under a dollar on Opus and a fraction of a cent on Haiku. [REPLACE with the actual cost from your run.] On a consumer Claude plan, it's included in the subscription you may already pay for.

## 5. Results

| | Summ | Koinly | Syla | Claude [MODEL] |
|---|---|---|---|---|
| Plan Sarah needed | [plan] | [plan] | [plan] | n/a |
| Cost for her report | [A$] | [$] | [A$] | [US$ per run] |
| Time to report | [mins] | [mins] | [mins] | [mins] |
| Net capital gain | [$] | [$] | [$] | [$] |
| Matches hand-checked figure? | [Y/N] | [Y/N] | [Y/N] | [Y/N] |
| Upsell prompts seen | [notes] | [notes] | [notes] | none |

[ADD a column per Claude model if comparing more than one.]

[ADD: two or three sentences on what happened. Where did the numbers differ, and why?]

## 6. What the subscription still pays for

It would be easy to stop at the table. To be fair, Summ, Koinly and Syla do things a one-off prompt doesn't:

- **Integrations.** They pull data from many exchanges and wallets. With a prompt, you export and upload every file yourself.
- **Edge cases.** DeFi, staking, airdrops, transfers between your own wallets, and wrapped tokens all need rules. I wrote recently about how [the ATO can treat wrapping a token as a disposal](wrapped-tokens-cgt.md). A CoinSpot-only history avoids most of this. Many histories don't.
- **Records year to year.** The software keeps your remaining cost bases for next year. With a prompt, that's your job. The ATO has [flagged cost base record-keeping](https://publicaccountant.com.au/news/ato-fires-warning-shots-on-cryptocurrency-cost-base-record-keeping/) as a problem area.
- **Accountant-ready output.** Many tax agents already know these report formats.

[ADD anything you hit in testing.]

## 7. So, does the SaaS model still make sense?

[YOUR CONCLUSION. Suggested draft:]

For Sarah, with [N] trades on one Australian exchange, the answer is [no / barely]. The calculation is now close to free. What the software companies still sell is data collection and edge cases. That's valuable for the minority with wallets, chains and DeFi. The problem for these companies is that most of their customers look like Sarah, not like me.

Whatever tool you use, the output still goes on your tax return under your name. Hand-check a few disposals and have a registered tax agent review it before you lodge. This article is not tax advice.

---

*Have you tried doing your crypto tax with an LLM? Did it match what your software or accountant came up with? I'd like to hear in the comments.*

---

## Appendix: the prompt

Attach the CoinSpot transaction history CSV, then send:

> You are preparing an Australian capital gains tax summary for an individual taxpayer from the attached CoinSpot transaction history CSV.
>
> **Scope**
> - Financial year: 1 July 2025 to 30 June 2026.
> - The taxpayer is an Australian resident individual and an investor, not a trader. Treat all disposals as CGT events.
> - Use the full CSV history so cost bases include purchases made in earlier years.
>
> **Method**
> 1. List the columns you found and how you interpret each one. State any assumptions.
> 2. Treat every sale, and every crypto-to-crypto swap, as a CGT event. A swap disposes of one asset and acquires the other at its AUD market value.
> 3. Include fees in the cost base (buys) and deduct them from capital proceeds (sells).
> 4. Match disposals to acquisitions using FIFO, and say so in the output.
> 5. Work out the holding period of each parcel disposed of. Apply the 50% CGT discount to gains on assets held at least 12 months, after offsetting capital losses.
> 6. Apply capital losses against non-discount gains first, then discount gains, before applying the discount.
> 7. List any rows you cannot classify (deposits, withdrawals, transfers, staking, rewards) separately. Do not guess.
>
> **Output**
> 1. A table of every CGT event in the year: date, asset, quantity, proceeds (AUD), cost base (AUD), acquisition date(s), holding period, gain or loss, discount eligible (Y/N).
> 2. A summary: total gains, total losses, discount applied and net capital gain, with working.
> 3. Remaining holdings at 30 June 2026 with cost bases, to carry into next year.
> 4. Assumptions and flagged rows.
> 5. A downloadable CSV of the CGT event table.
>
> Recompute the totals from the event table before giving the final figures.

---

**References:**

- [Adobe stock down 24% in 2026 amid AI concerns (Techleap / I amsterdam startup map)](https://startupmap.iamsterdam.com/news/feed/adobe-stock-down-24-in-2026-amid-ai-concerns-but-trading-at-discounted-15x-earnings-multiple)
- [Adobe stock slides over 60% from 2024 peak (Pulse)](https://www.pulse.bot/enterprise/news/adobe-stock-slides-over-60-from-2024-peak-becomes-cheapest-in-years-for-enterprise-buyers-febc2973-003e-4f4b-81b3-fcf86963ef9a/)
- [Adobe stock drops 26% as Wall Street questions its defence against AI competition (24/7 Wall St)](https://247wallst.com/investing/2026/02/18/adobe-stock-drops-26-as-wall-street-questions-its-defense-against-ai-competition/)
- [ATO: How to work out and report CGT on crypto](https://www.ato.gov.au/Individuals/Investments-and-assets/Crypto-asset-investments/How-to-work-out-and-report-CGT-on-crypto/)
- [ATO: CGT discount](https://www.ato.gov.au/Individuals/Capital-gains-tax/CGT-discount/)
- [ATO fires warning shots on cryptocurrency cost base, record-keeping (Public Accountant)](https://publicaccountant.com.au/news/ato-fires-warning-shots-on-cryptocurrency-cost-base-record-keeping/)
- [Summ pricing, Australia](https://summ.com/au/pricing/personal)
- [Summ review 2026 (Finder)](https://www.finder.com.au/cryptocurrency-tax/summ-review)
- [Koinly review 2026 (Finder)](https://www.finder.com.au/cryptocurrency-tax/koinly)
- [Syla pricing](https://www.syla.com.au/pricing)
- [Syla: Choosing the best plan for you](https://help.syla.com.au/en/articles/8282736-choosing-the-best-plan-for-you)
- [Claude API pricing (Anthropic)](https://platform.claude.com/docs/en/about-claude/pricing)
- [CoinSpot](https://www.coinspot.com.au/)

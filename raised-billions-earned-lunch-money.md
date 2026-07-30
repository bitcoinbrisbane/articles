# Raised Billions, Earns Lunch Money: The zkSync, Polkadot and Tezos Reality Check

A chart did the rounds recently that stopped me mid-scroll. Two columns, one damning comparison: how much a crypto project **raised**, and how much **revenue** its chain earned in the last 24 hours. EOS: $4.2 billion raised, $0 earned. zkSync: $458 million raised, $270 earned. Polkadot: $145 million raised, $0 earned. Tezos: $286 million raised, $29 earned.

I've owned DOT and interacted with zkSync, and I remember the Tezos ICO making headlines back in 2017. I was told these were the rails of Web3 — the settlement layers, the interoperability fabric, the self-amending "Ethereum killers" and "Ethereum helpers." So I went and checked the numbers myself, because a screenshot is not a source. What I found is that the chart is, if anything, *too kind*. Here's the reality.

## 1. What "Revenue" Actually Means (And Why It's Fair)

First, an honest caveat, because I want to argue in good faith. The "24h revenue" figure in that chart is **chain fees** — the fees users pay to transact, which flow to the protocol. It is a single-day snapshot, it is noisy, and it deliberately excludes a few things:

- **Token appreciation.** DOT and ZK holders can profit even if the chain earns nothing.
- **Staking issuance.** Validators earn inflation rewards, which aren't "revenue" but are real yield.
- **Application-layer revenue.** DeFi apps *on* zkSync earn fees the base chain doesn't capture.

So it's a loaded metric. But it's loaded in a *fair* direction, because protocol fees are the one number that can't be faked with inflation or narrative. They measure whether real people are paying real money to use the thing. And on that measure, per [DeFiLlama](https://defillama.com/chain/zksync-era), ZKsync Era's chain fees run around **$285 in a 24-hour window** — matching the chart's ~$270. Polkadot's chain revenue has been cited at roughly [**$0.18 per day**](https://cryptoadventure.com/polkadot-revenue-data-reopens-debate-over-dot-network-demand/) — which rounds to the chart's $0.

These are protocols with multi-billion-dollar valuations producing lunch money in daily fees. That's not a screenshot artifact. That's the business.

## 2. Polkadot: $145 Million Raised, ~$0 a Day Earned

### The Raise

In October 2017, the Web3 Foundation — founded by Ethereum co-founder and Solidity author Gavin Wood — ran the Polkadot ICO. Over a 12-day sale from October 15–27, it [raised roughly $145 million](https://cryptopotato.com/the-story-of-polkadot-starts-with-the-2017-ico-2000-roi-for-early-investors/), selling about 5 million DOT at ~$0.29 each. At the time it was one of the largest raises in crypto history.

Then came an omen. Ten days after the sale closed, the infamous [Parity multisig wallet bug](https://www.coindesk.com/tech/2020/05/26/polkadot-goes-live-as-web3-foundation-pushes-prospective-mainnet) froze 513,774 ETH across 587 wallets — roughly **306,000 ETH (about $98 million at the time) belonging to the Web3 Foundation itself**, or two-thirds of the ICO proceeds. The organisation building the interoperable future of blockchains locked up most of its own treasury in a smart-contract bug. It said the remaining funds were enough to continue. Development pressed on.

### The Vision

Polkadot's pitch was genuinely ambitious and technically serious: a "relay chain" providing shared security to many application-specific "parachains," letting sovereign blockchains interoperate and settle against a common trust layer. Where Ethereum wanted to be one world computer, Polkadot wanted to be the *coordinator of many*. Wood didn't lack for credentials — he coined "Web3" and wrote the [Yellow Paper](https://ethereum.github.io/yellowpaper/paper.pdf) that formally specified the Ethereum Virtual Machine.

### The Reality

Here's where the raise meets the receipts. Polkadot's on-chain economic activity is, to be blunt, tiny relative to its ambitions and its valuation:

- Polkadot generated about [$88,300 in transaction fees in *all* of Q1 2025](https://cryptoadventure.com/polkadot-revenue-data-reopens-debate-over-dot-network-demand/) — a full quarter. Its single best fee day that quarter reached roughly $2,500.
- Over a trailing year it produced around [$721,000 in fees](https://cryptoadventure.com/polkadot-revenue-data-reopens-debate-over-dot-network-demand/), ranking it **21st among blockchains**.
- Meanwhile DOT's market capitalisation has sat in the [multi-billion-dollar range](https://www.coingecko.com/en/coins/polkadot) — roughly $3.4B in 2025 comparisons.

Sit with that ratio. A network valued in the billions, generating fees you could cover with a mid-tier SaaS subscription. The most telling line I found came from analysts noting Polkadot's chain revenue at [$0.1841](https://cryptoadventure.com/polkadot-revenue-data-reopens-debate-over-dot-network-demand/) with an average transaction fee of about two cents — data that "will keep feeding criticism that the network's valuation is disconnected from direct onchain demand."

To be fair to Polkadot: 2025 was a year of genuine engineering. The network shipped [Asynchronous Backing, Agile Coretime, and Elastic Scaling](https://medium.com/polkadot-network/polkadot-roundup-2025-3c3c71c7e9c4), and the community voted to [cap DOT supply at 2.1 billion](https://medium.com/polkadot-network/polkadot-roundup-2025-3c3c71c7e9c4) to make the token scarcer. Staking climbed; validator counts rose. The technology is real. But shipping upgrades is not the same as attracting demand, and the fee data says the demand isn't there yet. You can build the most elegant toll road in the world; it doesn't earn until cars drive on it.

## 3. zkSync: $458 Million Raised, $270 a Day Earned

### The Raise

zkSync is built by [Matter Labs](https://www.theblock.co/post/187503/matter-labs-raises-200-million-to-boost-zksync-adoption-and-grow-its-team), an Ethereum Layer-2 team betting on zero-knowledge rollups — arguably the most technically respected scaling approach in the ecosystem. Across a seed, Series A, a [$50M Series B](https://www.theblock.co/post/187503/matter-labs-raises-200-million-to-boost-zksync-adoption-and-grow-its-team), and a [**$200M Series C**](https://www.coindesk.com/tech/2022/11/16/matter-labs-nets-200m-to-build-zksync-ethereum-scaling-platform) in November 2022 led by Blockchain Capital and Dragonfly, Matter Labs pulled in a war chest that, with ecosystem allocations, lands near the chart's $458M figure. The Series C alone was one of the largest venture rounds in crypto that year.

### The Vision

The pitch was compelling and, unlike a lot of crypto, technically sound: **ZK-rollups**. Bundle thousands of transactions off-chain, produce a cryptographic validity proof, and post that proof to Ethereum. You inherit Ethereum's security while paying a fraction of its fees. This is not vaporware — validity proofs are real mathematics, and zkSync is one of the credible teams shipping them.

### The Reality

Then zkSync did the thing so many of these projects do: it launched a token, and activity fell off a cliff.

In June 2024, zkSync airdropped [3.675 billion ZK tokens](https://thedefiant.io/news/defi/zksync-to-airdrop-3-6-billion-tokens-to-early-users) — 17.5% of a 21 billion supply — to early users. The airdrop was supposed to reward and retain a community. Instead it revealed how much of that "community" was mercenary:

- TVL [dropped from nearly $200M to $128M](https://www.coinspeaker.com/zksync-tvl-hit-distribution/) around the distribution.
- The 7-day average of active addresses [fell from over 200,000 in July 2024 to about 30,000 by December](https://www.theblock.co/post/300746/zksync-eras-airdrop-fails-to-halt-decline-in-transactions-and-active-addresses).
- Daily revenue [plummeted roughly 99% post-airdrop](https://www.theblock.co/post/311907/zksync-eras-daily-revenue-plummets-99-post-airdrop-highlighting-layer-2-market-struggles).

That last stat is the whole story of the chart in one line. When the free tokens stopped, so did the users — and the fees dwindled with them toward the few hundred dollars a day they earn now. Transactions per day did [recover in early 2025](https://messari.io/report/state-of-zksync-q1-2025) (up 276% QoQ across the Elastic Network), but DeFi TVL kept sliding. More transactions, less money locked, and still only a few hundred dollars a day in base-chain fees. A $458M-funded protocol earning what a suburban café clears before lunch.

## 4. Tezos: $232 Million Raised, ~$29 a Day Earned

### The Raise

Tezos is the elder statesman of this group, and its raise is the one I remember watching in real time. In July 2017, over a two-week *uncapped* crowdsale, Tezos [pulled in $232 million](https://www.coindesk.com/markets/2017/07/13/232-million-tezos-blockchain-project-finishes-record-setting-token-sale) — 65,627 BTC and 361,122 ETH — the [largest ICO to that date](https://www.forbes.com/sites/omribarzilay/2017/07/15/tezos-232-million-ico-may-just-be-the-beginning/). (The chart lists $286M; the extra likely reflects later foundation treasury growth, since those BTC and ETH holdings appreciated enormously. The headline, documented raise is $232M.)

### The Vision

Tezos had a genuinely clever pitch, and one that still sounds good today: a **self-amending blockchain** with **on-chain governance**. Instead of contentious hard forks — the kind that split Bitcoin and Ethereum — Tezos token holders would vote on protocol upgrades, and the chain would amend *itself* according to the outcome. No schisms, no acrimonious community splits. A blockchain that evolves by ballot rather than by fork. On paper, it's one of the more elegant ideas in the space.

### The Reality

Tezos is the cautionary tale for what happens *after* the money lands. Before a single line of the promised governance shipped, the project descended into a governance disaster of its own — an ironic one for a chain whose entire premise was resolving disputes peacefully.

A public power struggle erupted between founders Arthur and Kathleen Breitman and Johann Gevers, the head of the Swiss foundation holding the funds. Gevers was accused of self-dealing; the Breitmans were accused of trying to bypass the Swiss legal structure. The infighting [delayed the token release until February 2018](https://coinbureau.com/ico/256m-ico-lawsuits-tezos-story/) — roughly seven months after contributors had handed over their money for a network that didn't exist. The fallout produced [four class-action lawsuits](https://medium.com/hackernoon/the-curious-tale-of-tezos-from-a-232-million-ico-to-4-class-action-lawsuits-6f411b7aad7e) alleging the sale was an unregistered securities offering, eventually [settled for $25 million](https://www.courthousenews.com/tezos-cryptocurrency-founders-settle-with-investors-for-25-million/).

Nine years on, the network launched, the governance mechanism works as designed, and Tezos has amended itself through many upgrades without a single contentious fork — a real technical achievement. And yet, per [DeFiLlama](https://defillama.com/chain/tezos), Tezos carries a TVL around **$30 million** and produces roughly **$221,000 in annual revenue** — which works out to a few hundred dollars a day, consistent with the chart's ~$29 on a quiet one. The self-amending chain amended itself into technical maturity and near-total economic irrelevance. The governance works beautifully; there's just almost nobody left to govern.

## 5. The Pattern: Capital In, Value Out — Where?

Line these up and a pattern snaps into focus that goes well beyond three chains:

| Project | Raised | Chain Revenue (24h) |
|---|---|---|
| EOS | $4.2B | $0 |
| zkSync | $458M | ~$270 |
| Tezos | $232M–$286M | ~$29 |
| Polkadot | $145M | ~$0 |
| Celestia | $156M | ~$58 |

(EOS's [$4.2 billion 2017–18 ICO](https://www.coindesk.com/markets/2019/09/17/the-first-yearlong-ico-for-eos-raised-4-billion-the-second-just-28-million) remains the cautionary grandfather of the genre — the largest raise in history, later fined [$24M by the SEC](https://coingeek.com/sec-settles-with-eos-parent-company-for-24m-in-penalties-for-unregistered-ico/), producing a chain almost nobody uses today. Celestia's [$155M cumulative raise](https://blockworks.co/news/celestia-raises-155m-a16z-decentralized-science) is more recent but tells the same story of valuation running far ahead of usage.)

The crypto industry inverted the normal order of business. In a real company, revenue justifies valuation. Here, **valuation *is* the product**. You raise on a narrative, mint a token, and the token's market cap becomes the number everyone points to — while the actual thing you built, the chain, earns almost nothing because almost nobody is paying to use it.

This is not an argument that the technology is fake. zkSync's proofs are real math. Polkadot's shared-security model is real engineering. Tezos's on-chain governance actually works. Gavin Wood is not a con artist. The argument is narrower and harder to dodge: **hundreds of millions of dollars bought technology that, so far, no meaningful market wants to pay for.** The fee columns are the market's honest verdict, and it's speaking in single digits.

The uncomfortable question isn't "is this a scam?" It's the one every founder eventually has to answer and most of crypto has been allowed to skip: *does anyone actually pay for this?* For too many chains that raised too much, the answer — in the only currency that matters, daily fees — is still no.

## Call to Action

I'll be honest about where I've landed: I'm a Bitcoin and Ethereum maximalist, and charts like this are exactly why. **Bitcoin and Ethereum earn their keep.** People pay real money, every single day, to settle on them — [Ethereum alone routinely clears millions in daily fees](https://defillama.com/chain/ethereum), and Bitcoin secures more value than every project in that table combined. They didn't raise nine figures on a promise and then coast on a token price. They shipped, and the market votes for them with its wallet, block after block.

Everything else on that chart is a lesson in the same mistake: capital raised on narrative, valuation mistaken for product, and a chain almost nobody pays to use. The alt-L1 and L2 graveyard keeps filling up because the fees never showed up.

So here's my pitch, not a question: **stick to the real chains.** If you want exposure to blockchains that actually generate revenue, the two that always have are the two that always will. Think I'm wrong? Show me the alt that earns its market cap in daily fees — I'll wait in the comments.

---

## References

- CoinDesk — [The First Yearlong ICO for EOS Raised $4 Billion. The Second? Just $2.8 Million](https://www.coindesk.com/markets/2019/09/17/the-first-yearlong-ico-for-eos-raised-4-billion-the-second-just-28-million)
- CoinGeek — [SEC settles with EOS parent company for $24M in penalties for unregistered ICO](https://coingeek.com/sec-settles-with-eos-parent-company-for-24m-in-penalties-for-unregistered-ico/)
- CryptoPotato — [The Story Of Polkadot Starts With The 2017 ICO](https://cryptopotato.com/the-story-of-polkadot-starts-with-the-2017-ico-2000-roi-for-early-investors/)
- CoinDesk — [Polkadot Goes Live as Web3 Foundation Pushes Prospective Mainnet](https://www.coindesk.com/tech/2020/05/26/polkadot-goes-live-as-web3-foundation-pushes-prospective-mainnet)
- CryptoAdventure — [Polkadot Revenue Data Reopens Debate Over DOT Network Demand](https://cryptoadventure.com/polkadot-revenue-data-reopens-debate-over-dot-network-demand/)
- Polkadot Network / Gavin Wood — [Polkadot Roundup 2025](https://medium.com/polkadot-network/polkadot-roundup-2025-3c3c71c7e9c4)
- CoinGecko — [Polkadot (DOT) Price & Market Cap](https://www.coingecko.com/en/coins/polkadot)
- Ethereum — [Yellow Paper (Gavin Wood)](https://ethereum.github.io/yellowpaper/paper.pdf)
- The Block — [Matter Labs raises $200 million to boost zkSync adoption](https://www.theblock.co/post/187503/matter-labs-raises-200-million-to-boost-zksync-adoption-and-grow-its-team)
- CoinDesk — [Matter Labs Nets $200M to Build zkSync Ethereum Scaling Platform](https://www.coindesk.com/tech/2022/11/16/matter-labs-nets-200m-to-build-zksync-ethereum-scaling-platform)
- The Defiant — [ZKsync to Airdrop 3.6 Billion Tokens to Early Users](https://thedefiant.io/news/defi/zksync-to-airdrop-3-6-billion-tokens-to-early-users)
- Coinspeaker — [ZKsync TVL Takes Hit amid Token Distribution Controversy](https://www.coinspeaker.com/zksync-tvl-hit-distribution/)
- The Block — [ZKsync Era's airdrop fails to halt decline in transactions and active addresses](https://www.theblock.co/post/300746/zksync-eras-airdrop-fails-to-halt-decline-in-transactions-and-active-addresses)
- The Block — [ZKsync Era's daily revenue plummets 99% post-airdrop](https://www.theblock.co/post/311907/zksync-eras-daily-revenue-plummets-99-post-airdrop-highlighting-layer-2-market-struggles)
- Messari — [State of ZKsync Q1 2025](https://messari.io/report/state-of-zksync-q1-2025)
- DeFiLlama — [ZKsync Era: TVL, Fees & Revenue](https://defillama.com/chain/zksync-era)
- CoinDesk — [$232 Million: Tezos Blockchain Project Finishes Record-Setting Token Sale](https://www.coindesk.com/markets/2017/07/13/232-million-tezos-blockchain-project-finishes-record-setting-token-sale)
- Forbes — [Tezos' $232 Million ICO May Just Be The Beginning](https://www.forbes.com/sites/omribarzilay/2017/07/15/tezos-232-million-ico-may-just-be-the-beginning/)
- Coin Bureau — [From a $232m ICO to Lawsuits — The Tezos Story](https://coinbureau.com/ico/256m-ico-lawsuits-tezos-story/)
- HackerNoon — [The Curious Tale of Tezos — from a $232 Million ICO to 4 Class Action Lawsuits](https://medium.com/hackernoon/the-curious-tale-of-tezos-from-a-232-million-ico-to-4-class-action-lawsuits-6f411b7aad7e)
- Courthouse News — [Tezos Cryptocurrency Founders Settle With Investors for $25 Million](https://www.courthousenews.com/tezos-cryptocurrency-founders-settle-with-investors-for-25-million/)
- DeFiLlama — [Tezos: TVL, Fees & Revenue](https://defillama.com/chain/tezos)
- Blockworks — [Funding Roundup: Celestia's raised $155M](https://blockworks.co/news/celestia-raises-155m-a16z-decentralized-science)

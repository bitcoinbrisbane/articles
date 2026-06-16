# "It's Rigged" — Or, Why Elections Need Merkle Trees

Every time an election result lands that someone doesn't like, my feed fills with the same three words: *it's rigged*. Two days ago, on the weekend before the 2 June 2026 California primary, [ballots were found burned in an official drop box outside the Los Angeles Civic Center](https://www.cbsnews.com/losangeles/news/los-angeles-county-voting-center-vandalized-mail-in-ballots-burned/) and a Long Beach vote centre was vandalised the same morning. Six months earlier, the sitting President called the statewide Proposition 50 vote — in which LA County was by far the largest bloc — ["a GIANT SCAM" and "RIGGED"](https://patch.com/california/across-ca/final-stretch-prop-50-voting-millions-weigh-trump-declares-election-rigged).

A week later, a photo of ballot envelopes in a dumpster went viral, posted with the caption ["These ballots were found in a dumpster in Sonoma Valley, California"](https://www.yahoo.com/news/politics/articles/old-photo-election-envelopes-misrepresented-202534092.html). It was false — the Sonoma County Registrar [confirmed](https://www.yahoo.com/news/politics/articles/old-photo-election-envelopes-misrepresented-202534092.html) the image showed *empty* return envelopes from the November 2018 election, legally recycled after the [22-month federal retention period](https://www.law.cornell.edu/uscode/text/52/20701). But it spread anyway, because here's the thing: *nobody could prove otherwise in ten seconds*. The registrar's denial is just one more claim against another claim.

Closer to home, the same week, Pauline Hanson's One Nation ran a "Fire the Liar" fundraiser and claimed it pulled in [over $2 million in under two days](https://www.thenewdaily.com.au/news/politics/australian-politics/2026/06/11/one-nation-fire-the-liar). Prime Minister Anthony Albanese's response was instinctive: ["Did she? Did she though? What evidence is there?"](https://www.thenewdaily.com.au/news/politics/australian-politics/2026/06/11/one-nation-fire-the-liar). It's the perfect modern reflex — *prove it*. And here's the part that matters: Hanson [answered with a forensic audit and published the numbers](https://ground.news/article/one-nation-will-audit-donations-to-verify-their-authenticity) — 28,000-odd donors, $59 average. Claim, challenge, proof. That's the whole game. The only reason it worked is that donations leave a trail you can audit. Votes don't.

The honest answer to "how do you know your vote was counted?" in most US jurisdictions is uncomfortable: *you don't*. You drop a ballot into a box, a machine eats it, and you trust the chain of custody. No receipt. No way to check. No voter ID in California ([Cal. Elec. Code §2150](https://leginfo.legislature.ca.gov/faces/codes_displaySection.xhtml?lawCode=ELEC&sectionNum=2150) doesn't require it at the polls).

If you're a crypto person reading this, you already know the fix. We solved this problem for exchange reserves more than a decade ago. It's called a **Merkle tree**.

## 1. The 30-second primer

A [Merkle tree](https://en.wikipedia.org/wiki/Merkle_tree), invented by Ralph Merkle in [1979](https://patents.google.com/patent/US4309569A/en), is a structure where every leaf is a hash, and every parent node is the hash of its two children. The single hash at the top — the **Merkle root** — is a cryptographic fingerprint of every leaf underneath it. Change one leaf and the root changes. Publish the root, and anyone can later prove a specific leaf was included, using only a logarithmic-sized path of sibling hashes. No need to download the whole list.

This is the same primitive that lets a [Bitcoin SPV client](https://developer.bitcoin.org/devguide/operating_modes.html#simplified-payment-verification-spv) verify a transaction was in a block without downloading the block.

## 2. How an election would use it

The simplest version — public, named voting:

1. Each voter's leaf is `hash(full_name || chosen_candidate)`, where the name is exactly as it appears on the electoral roll.
2. Every leaf becomes part of a Merkle tree.
3. After polls close, the election authority publishes the **Merkle root**, the full list of leaves, and the tally.
4. Anyone — voter, candidate, journalist, foreign observer — can recompute the root from the leaves, confirm any named voter's leaf is included, and add up the candidates to check the total matches.

This is maximally transparent. It's also maximally public: the electoral roll is already public, so anyone can take a name, try `hash(name || candidate)` for each of the handful of candidates, and see how that person voted. That's not a bug in this version — it's the trade-off. It's closer to a show of hands or a signed petition than a secret ballot: total verifiability in exchange for vote secrecy.

Of course, to make it more secure — to keep the *how you voted* part private — we'd need a nonce: a secret random value the voter holds, so the leaf becomes `hash(full_name || nonce || chosen_candidate)`. Now the leaf is uncrackable without the nonce, and only the voter can prove which leaf is theirs. You get verifiability *and* secrecy, at the cost of every voter having to safeguard a secret. That's the version worth building. But even the naive, fully-public one is strictly better than today's "trust us."

The result either way: a voter can mathematically prove their vote was counted. An auditor can prove the total matches the leaves. Neither needs to trust the election authority.

## 3. Crypto has been doing this since 2013

The Merkle-tree proof-of-reserves construction was proposed by [Greg Maxwell and Peter Todd on IRC on 1 March 2013](https://iwilcox.me.uk/2014/proving-bitcoin-reserves), and Todd [presented it publicly at the Bitcoin 2013 conference in San Jose that May](https://iwilcox.me.uk/2014/proving-bitcoin-reserves) — nine months *before* Mt. Gox collapsed. Zak Wilcox wrote up the canonical explainer in [February 2014](https://iwilcox.me.uk/2014/proving-bitcoin-reserves).

On 24 March 2014, [Kraken became the first exchange to publish a cryptographic Proof of Reserves](https://blog.kraken.com/product/security/kraken-passes-worlds-first-cryptographic-proof-of), audited by Stefan Thomas using "a variation of the Merkle Tree audit method proposed by Greg Maxwell." [CoinDesk covered it the same day](https://www.coindesk.com/markets/2014/03/24/kraken-bitcoin-exchange-passes-proof-of-reserves-cryptographic-audit). In 2015, Stanford and NYU researchers [formalised the protocol in the *Provisions* paper](https://eprint.iacr.org/2015/1008.pdf) at ACM CCS — peer-reviewed cryptography, twelve years ago.

Today every major exchange runs a version of it. [Kraken](https://www.kraken.com/proof-of-reserves), [Binance](https://www.binance.com/en/proof-of-reserves), [Coinbase](https://www.coinbase.com/blog/coinbases-financial-statements-show-strength-amidst-difficult-conditions-for-crypto), and [OKX](https://www.okx.com/proof-of-reserves) all let any customer verify their balance is included in the published Merkle root. Hundreds of millions of users. Billions of dollars in attested reserves. A live, working, audited system that any voter could understand by lunchtime.

The crypto industry — not exactly famous for its consumer protections — figured out how to give its users a mathematical receipt more than a decade ago. Elections still can't.

## 4. What this would actually fix

It wouldn't stop someone yelling "rigged" on social media. Nothing will. And it wouldn't stop someone setting fire to a drop box on a Sunday afternoon, or posting a stale photo of recycled envelopes and captioning it "found in a dumpster." But it would change what happens *next*: instead of a registrar issuing a denial that lands as just another voice in the noise, every voter could check their own ballot against the published root and see for themselves. When Albanese said "did she though?", Hanson could answer with an audit. When someone says an election was rigged, the registrar should be able to do the same — and so should you. But it would let every voter whose ballot *did* make it into the count answer the question *was my vote counted* with `true` or `false`, mathematically, in about ten seconds on their phone. And it would let auditors prove the total matches the leaves. That's a better answer than the one the Los Angeles County Registrar can give today.

The cryptography has existed since 1979. The engineering is shipped in production at every major exchange. The blocker is not technical.

---

*Would you trust an election more if you could verify your own vote on-chain? Or does the privacy trade-off worry you more than the trust trade-off? Curious what people think.*

---

**References:**
- [CBS Los Angeles — LA voting centre vandalised, mail-in ballots burned (May 2026)](https://www.cbsnews.com/losangeles/news/los-angeles-county-voting-center-vandalized-mail-in-ballots-burned/)
- [Patch — Trump declares Prop 50 election "RIGGED" (Nov 2025)](https://patch.com/california/across-ca/final-stretch-prop-50-voting-millions-weigh-trump-declares-election-rigged)
- [Yahoo/AFP — Old photo of election envelopes misrepresented as trashed 2026 California ballots (Jun 2026)](https://www.yahoo.com/news/politics/articles/old-photo-election-envelopes-misrepresented-202534092.html)
- [52 U.S. Code §20701 — Retention and preservation of records (22-month rule)](https://www.law.cornell.edu/uscode/text/52/20701)
- [The New Daily — PM's blast after One Nation's $2m "fire the liar" cash boost (Jun 2026)](https://www.thenewdaily.com.au/news/politics/australian-politics/2026/06/11/one-nation-fire-the-liar)
- [Ground News — One Nation to audit donations after Albanese questions authenticity (Jun 2026)](https://ground.news/article/one-nation-will-audit-donations-to-verify-their-authenticity)
- [Merkle tree — Wikipedia](https://en.wikipedia.org/wiki/Merkle_tree)
- [US Patent 4,309,569 — Merkle, 1979](https://patents.google.com/patent/US4309569A/en)
- [California Elections Code §2150 — voter identification](https://leginfo.legislature.ca.gov/faces/codes_displaySection.xhtml?lawCode=ELEC&sectionNum=2150)
- [Bitcoin SPV — bitcoin.org developer guide](https://developer.bitcoin.org/devguide/operating_modes.html#simplified-payment-verification-spv)
- [Zak Wilcox — Proving Your Bitcoin Reserves (Feb 2014)](https://iwilcox.me.uk/2014/proving-bitcoin-reserves)
- [Kraken — World's First Cryptographic Proof of Reserves (Mar 2014)](https://blog.kraken.com/product/security/kraken-passes-worlds-first-cryptographic-proof-of)
- [CoinDesk — Kraken Passes Proof of Reserves Audit (Mar 2014)](https://www.coindesk.com/markets/2014/03/24/kraken-bitcoin-exchange-passes-proof-of-reserves-cryptographic-audit)
- [Provisions: Privacy-preserving Proofs of Solvency (ACM CCS 2015)](https://eprint.iacr.org/2015/1008.pdf)
- [Kraken Proof of Reserves](https://www.kraken.com/proof-of-reserves)
- [Binance Proof of Reserves](https://www.binance.com/en/proof-of-reserves)
- [Coinbase financial statements](https://www.coinbase.com/blog/coinbases-financial-statements-show-strength-amidst-difficult-conditions-for-crypto)
- [OKX Proof of Reserves](https://www.okx.com/proof-of-reserves)

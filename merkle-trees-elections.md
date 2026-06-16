# "It's Rigged" — Or, Why Elections Need Merkle Trees

Every time an election result lands that someone doesn't like, my feed fills with the same three words: *it's rigged*. Two days ago, on the weekend before the 2 June 2026 California primary, [ballots were found burned in an official drop box outside the Los Angeles Civic Center](https://www.cbsnews.com/losangeles/news/los-angeles-county-voting-center-vandalized-mail-in-ballots-burned/) and a Long Beach vote centre was vandalised the same morning. Six months earlier, the sitting President called the statewide Proposition 50 vote — in which LA County was by far the largest bloc — ["a GIANT SCAM" and "RIGGED"](https://patch.com/california/across-ca/final-stretch-prop-50-voting-millions-weigh-trump-declares-election-rigged).

A week later, a photo of ballot envelopes in a dumpster went viral, posted with the caption ["These ballots were found in a dumpster in Sonoma Valley, California"](https://www.yahoo.com/news/politics/articles/old-photo-election-envelopes-misrepresented-202534092.html). It was false — the Sonoma County Registrar [confirmed](https://www.yahoo.com/news/politics/articles/old-photo-election-envelopes-misrepresented-202534092.html) the image showed *empty* return envelopes from the November 2018 election, legally recycled after the [22-month federal retention period](https://www.law.cornell.edu/uscode/text/52/20701). But it spread anyway, because here's the thing: *nobody could prove otherwise in ten seconds*. The registrar's denial is just one more claim against another claim.

The honest answer to "how do you know your vote was counted?" in most US jurisdictions is uncomfortable: *you don't*. You drop a ballot into a box, a machine eats it, and you trust the chain of custody. No receipt. No way to check. No voter ID in California ([Cal. Elec. Code §2150](https://leginfo.legislature.ca.gov/faces/codes_displaySection.xhtml?lawCode=ELEC&sectionNum=2150) doesn't require it at the polls).

If you're a crypto person reading this, you already know the fix. We solved this problem for exchange reserves more than a decade ago. It's called a **Merkle tree**.

## 1. The 30-second primer

A [Merkle tree](https://en.wikipedia.org/wiki/Merkle_tree), invented by Ralph Merkle in [1979](https://patents.google.com/patent/US4309569A/en), is a structure where every leaf is a hash, and every parent node is the hash of its two children. The single hash at the top — the **Merkle root** — is a cryptographic fingerprint of every leaf underneath it. Change one leaf and the root changes. Publish the root, and anyone can later prove a specific leaf was included, using only a logarithmic-sized path of sibling hashes. No need to download the whole list.

This is the same primitive that lets a [Bitcoin SPV client](https://developer.bitcoin.org/devguide/operating_modes.html#simplified-payment-verification-spv) verify a transaction was in a block without downloading the block.

## 2. How an election would use it

A naive sketch:

1. At registration, the voter generates a secret nonce and submits a salted commitment — `hash(voter_id || nonce || chosen_candidate)` — at the polling station.
2. Every commitment becomes a leaf in a Merkle tree.
3. After polls close, the election authority publishes the **Merkle root** and the tally.
4. Each voter receives their leaf and the sibling-hash path. They can independently verify their vote is included in the published root, without revealing their identity to anyone else.

Hashing the name alone — which is the version most people pitch — doesn't work. Names aren't unique, and a rainbow table cracks it in seconds. The salted commitment is the part that matters.

The result: a voter can mathematically prove their vote was counted. An auditor can prove the total matches the leaves. Neither needs to trust the election authority.

## 3. Crypto has been doing this since 2013

The Merkle-tree proof-of-reserves construction was proposed by [Greg Maxwell and Peter Todd on IRC on 1 March 2013](https://iwilcox.me.uk/2014/proving-bitcoin-reserves), and Todd [presented it publicly at the Bitcoin 2013 conference in San Jose that May](https://iwilcox.me.uk/2014/proving-bitcoin-reserves) — nine months *before* Mt. Gox collapsed. Zak Wilcox wrote up the canonical explainer in [February 2014](https://iwilcox.me.uk/2014/proving-bitcoin-reserves).

On 24 March 2014, [Kraken became the first exchange to publish a cryptographic Proof of Reserves](https://blog.kraken.com/product/security/kraken-passes-worlds-first-cryptographic-proof-of), audited by Stefan Thomas using "a variation of the Merkle Tree audit method proposed by Greg Maxwell." [CoinDesk covered it the same day](https://www.coindesk.com/markets/2014/03/24/kraken-bitcoin-exchange-passes-proof-of-reserves-cryptographic-audit). In 2015, Stanford and NYU researchers [formalised the protocol in the *Provisions* paper](https://eprint.iacr.org/2015/1008.pdf) at ACM CCS — peer-reviewed cryptography, twelve years ago.

Today every major exchange runs a version of it. [Kraken](https://www.kraken.com/proof-of-reserves), [Binance](https://www.binance.com/en/proof-of-reserves), [Coinbase](https://www.coinbase.com/blog/coinbases-financial-statements-show-strength-amidst-difficult-conditions-for-crypto), and [OKX](https://www.okx.com/proof-of-reserves) all let any customer verify their balance is included in the published Merkle root. Hundreds of millions of users. Billions of dollars in attested reserves. A live, working, audited system that any voter could understand by lunchtime.

The crypto industry — not exactly famous for its consumer protections — figured out how to give its users a mathematical receipt more than a decade ago. Elections still can't.

## 4. What this would actually fix

It wouldn't stop someone yelling "rigged" on social media. Nothing will. And it wouldn't stop someone setting fire to a drop box on a Sunday afternoon, or posting a stale photo of recycled envelopes and captioning it "found in a dumpster." But it would change what happens *next*: instead of a registrar issuing a denial that lands as just another voice in the noise, every voter could check their own ballot against the published root and see for themselves. But it would let every voter whose ballot *did* make it into the count answer the question *was my vote counted* with `true` or `false`, mathematically, in about ten seconds on their phone. And it would let auditors prove the total matches the leaves. That's a better answer than the one the Los Angeles County Registrar can give today.

The cryptography has existed since 1979. The engineering is shipped in production at every major exchange. The blocker is not technical.

---

*Would you trust an election more if you could verify your own vote on-chain? Or does the privacy trade-off worry you more than the trust trade-off? Curious what people think.*

---

**References:**
- [CBS Los Angeles — LA voting centre vandalised, mail-in ballots burned (May 2026)](https://www.cbsnews.com/losangeles/news/los-angeles-county-voting-center-vandalized-mail-in-ballots-burned/)
- [Patch — Trump declares Prop 50 election "RIGGED" (Nov 2025)](https://patch.com/california/across-ca/final-stretch-prop-50-voting-millions-weigh-trump-declares-election-rigged)
- [Yahoo/AFP — Old photo of election envelopes misrepresented as trashed 2026 California ballots (Jun 2026)](https://www.yahoo.com/news/politics/articles/old-photo-election-envelopes-misrepresented-202534092.html)
- [52 U.S. Code §20701 — Retention and preservation of records (22-month rule)](https://www.law.cornell.edu/uscode/text/52/20701)
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

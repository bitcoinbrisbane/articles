# Bitcoin Has a Monoculture Problem — And It's Written in C++

I've been writing software professionally for twenty-odd years. C#, Java, TypeScript, a bit of Go, a dash of Rust when I'm feeling masochistic. When I decided I wanted to contribute to Bitcoin at the protocol level, I cloned [bitcoin/bitcoin](https://github.com/bitcoin/bitcoin), opened the source tree, and was greeted by a wall of C++.

Not just any C++. Modern C++17 with custom serialization, hand-rolled crypto primitives, a build system that predates my career, and the kind of macro magic that makes IDE autocomplete cry. I'm not going to pretend I'm a C++ guy. Most developers under 35 aren't.

So I closed the tab.

That's the problem. Bitcoin's reference implementation is written in a language that fewer and fewer working developers know well — and the only meaningful "alternative" client, [Bitcoin Knots](https://bitcoinknots.org/), is the same C++ codebase with a different maintainer. The network has the *appearance* of client diversity without the substance of it. And when Linux itself — the most successful open-source project ever written — is [formally adopting Rust as a core kernel language](https://www.phoronix.com/news/Linux-7.0-Rust), Bitcoin's monolingual posture starts to look less like discipline and more like denial.

## 1. The Current State of "Diversity"

Let's be precise about what runs the Bitcoin network today.

| Implementation | Language | Approximate Node Share |
|----------------|----------|-----------------------|
| [Bitcoin Core](https://github.com/bitcoin/bitcoin) | C++ | ~80% |
| [Bitcoin Knots](https://github.com/bitcoinknots/bitcoin) | C++ | ~12–19% |
| [btcd](https://github.com/btcsuite/btcd) | Go | <1% |
| [libbitcoin](https://github.com/libbitcoin) | C++ | negligible |
| [bcoin](https://github.com/bcoin-org/bcoin) | JavaScript | negligible |

Sources: [Coin Dance Nodes](https://coin.dance/nodes), [Bitnodes Leaderboard](https://bitnodes.io/nodes/leaderboard/), [Bitcoin.com News on Knots growth](https://news.bitcoin.com/bitcoin-knots-climbs-to-19-of-nodes-as-core-v29-1-rollout-draws-blowback/).

Look closely. Bitcoin Core and Bitcoin Knots together account for roughly **99% of the public node network**. Both are C++. Knots is a [fork of Bitcoin Core](https://thecoinomist.com/learn/what-is-bitcoin-knots-fork-by-luke-dashjr/) maintained by Luke Dashjr, sharing the vast majority of its codebase — the same consensus logic, the same script interpreter, the same validation paths.

If a critical bug exists in Bitcoin Core's consensus code, there is an excellent chance the same bug exists in Bitcoin Knots. That's not diversity. That's a rebrand.

## 2. The 2018 Lesson We Forgot

In September 2018, Bitcoin Core developers disclosed [CVE-2018-17144](https://bitcoinops.org/en/topics/cve-2018-17144/). The bug was simple to describe and catastrophic in scope: nodes running Bitcoin Core 0.14.x through 0.16.2 would fail to detect a transaction that spent the same input twice within a single block.

In plain English: a miner could have created Bitcoin out of thin air.

The fix was released in [Bitcoin Core 0.16.3 and 0.17.0rc4](https://bitcoincore.org/en/2018/09/18/release-0.16.3/). The bug was never exploited on mainnet. But here's the part worth dwelling on, from the [Bitcoin Optech write-up](https://bitcoinops.org/en/topics/cve-2018-17144/):

> "The bug could also have caused a blockchain fork between affected nodes ... and unaffected nodes (most notably Bitcoin Core 0.13.2 and older, as well as some alternative Bitcoin implementations)."

The thing that would have saved the network from a worst-case scenario was the existence of independent implementations that did *not* share the vulnerable code path. The alternative clients were the firewall. And that firewall is getting thinner.

## 3. What Real Diversity Looks Like

Ethereum is not a model I generally hold up as something to emulate, but on this specific question its community has done the work. The [Ethereum client diversity dashboard](https://clientdiversity.org/) tracks five mainstream execution clients — Geth (Go), Nethermind (C#), Besu (Java), Erigon (Go), and Reth (Rust) — and four consensus clients in parallel. The official guidance from [ethereum.org](https://ethereum.org/developers/docs/nodes-and-clients/client-diversity/) is that **no single client should exceed 33% of the network**.

Why 33%? Because if a client with a supermajority has a bug that produces an invalid block, that bug can finalise on a forked chain, and validators on the real chain face slashing if they try to return. The Ethereum community learned this the hard way during the [January 2024 Nethermind bug](https://www.coindesk.com/tech/2024/01/22/bug-on-ethereums-nethermind-software-sparks-discussion-of-client-diversity-risks) that took roughly 8% of validators offline.

Bitcoin doesn't have proof-of-stake slashing, so the consequence model is different. But the underlying logic — *don't put all your validators on the same codebase* — applies just as forcefully. A consensus bug that causes a chain split between Core and not-Core is bad. A consensus bug that causes a chain split between Core and Core-with-different-default-flags is barely a chain split at all.

## 4. The Talent Pool Problem

C++ was [first released commercially in 1985](https://www.stroustrup.com/hopl-almost-final.pdf). Bjarne Stroustrup designed it as "C with Classes" before that. I'm 47, and the language is roughly my age — old enough to be a working professional, old enough to have accumulated some baggage.

That's not, by itself, a criticism. COBOL is older still and runs your bank. But the question for an open-source project is: *where is the next generation of contributors coming from?*

According to the [Stack Overflow 2024 Developer Survey](https://survey.stackoverflow.co/2024/technology/#1-programming-scripting-and-markup-languages), the most popular languages among professional developers are JavaScript, Python, TypeScript, SQL, Java, and C#. C++ sits well down the list. Rust is the [most-admired language for the ninth year running](https://survey.stackoverflow.co/2024/technology/#admired-and-desired). The pool of developers who can confidently audit a piece of consensus-critical C++ — not just read it, but reason about lifetimes, undefined behaviour, integer promotion edge cases, and platform-specific compiler quirks — is small and shrinking.

Meanwhile, the languages where Bitcoin has no serious full-node implementation include:

- **Java** — 30+ years old, [bitcoinj](https://bitcoinj.org/) exists as a library but is SPV-only, not a full validating node. Banks, enterprises, and government departments are full of Java engineers who could be auditing Bitcoin.
- **C#** — [NBitcoin](https://github.com/MetacoSA/NBitcoin) by Nicolas Dorier is one of the best-maintained Bitcoin libraries in any language, but again, library-level, not a full node. C# is the lingua franca of Microsoft shops, finance, and a huge slice of enterprise.
- **Rust** — there are [libraries](https://github.com/rust-bitcoin/rust-bitcoin), and the Lightning side has [LDK](https://lightningdevkit.org/), but no widely-deployed full validating node.
- **Go** — [btcd](https://github.com/btcsuite/btcd) exists and has been in production [since 2013](https://bitcoinmagazine.com/technical/btcd-a-full-bitcoin-alternative-written-in-go-1368114292), but its node share is rounding error.

Every one of those languages has a larger active developer population than C++. We are voluntarily fishing in the smallest pond.

## 5. Even Linux Is Moving

The argument I hear from Bitcoin Core defenders is some version of "C++ is what serious systems software is written in." That argument was defensible ten years ago. It is not defensible now.

In December 2025, at the Linux Kernel Developers Summit, Rust was [promoted from "experiment" to a core kernel language](https://devclass.com/2025/12/15/rust-boosted-by-permanent-adoption-for-linux-kernel-code/) alongside C and assembly. Linus Torvalds — not exactly a man known for chasing fashion — signed off. The [DRM subsystem](https://thenewstack.io/rust-goes-mainstream-in-the-linux-kernel/) is roughly a year away from *requiring* Rust for new drivers. Apple's silicon drivers, Google's Android drivers, and the Nova NVIDIA driver are all Rust. Google has reported [zero memory-safety bugs in production](https://thenewstack.io/rust-goes-mainstream-in-the-linux-kernel/) from its Rust kernel code.

In May 2025, Canonical announced that [Ubuntu 25.10 will ship sudo-rs by default](https://discourse.ubuntu.com/t/adopting-sudo-rs-by-default-in-ubuntu-25-10/60583) — a [Rust reimplementation of sudo](https://trifectatech.org/blog/memory-safe-sudo-to-become-the-default-in-ubuntu/) — making Ubuntu the first major distribution to make a memory-safe sudo the default. `sudo` is roughly as security-critical as software gets: it is the standard mechanism by which an unprivileged user requests root. The decision to rewrite it in Rust was not made because Rust is trendy. It was made because [memory-safety bugs in C are a recurring class of CVEs](https://www.cisa.gov/news-events/news/urgent-need-memory-safety-software-products) that simply do not exist in safe Rust.

If the Linux kernel and sudo can rewrite in Rust, the argument that Bitcoin Core *cannot* be implemented in anything but C++ is no longer a technical position. It is a cultural one.

## 6. Policy Is Not Protocol, And Bitcoin Core Keeps Forgetting

Here's a concrete example of why having only one mainstream implementation produces bad outcomes. Open [`src/policy/policy.cpp`](https://github.com/bitcoin/bitcoin/blob/master/src/policy/policy.cpp) in the Bitcoin Core source tree. Find the `GetDustThreshold` function. It defines the dust limit — currently **546 satoshis** for non-SegWit outputs and **294 satoshis** for P2WPKH SegWit outputs at the default relay fee of 3000 sat/kvB.

These numbers are not in the Bitcoin protocol. They are not enforced by consensus. They are not a rule that the network agreed to. They are a local mempool policy implemented in *one C++ file* in *one implementation* that happens to run on 80% of nodes.

The file header itself — quoted directly from the source — says:

> "This file is intended to be customised by the end user, and includes only local node policy logic."

Read that again. A transaction below 546 satoshis is a perfectly valid Bitcoin transaction. It will be accepted into a block if a miner includes it. The protocol does not care. But because Bitcoin Core's policy file refuses to relay it, and because Bitcoin Core runs 80% of the network, the transaction effectively cannot propagate. Policy has become protocol by sheer weight of deployment.

This is the danger of monoculture. When one implementation dominates, the distinction between "the rules of Bitcoin" and "the defaults this client happens to ship with" collapses. The recent fights about [datacarrier limits and OP_RETURN size](https://news.bitcoin.com/bitcoin-knots-climbs-to-19-of-nodes-as-core-v29-1-rollout-draws-blowback/) — which drove the surge in Knots adoption — are the same problem in different clothes. Knots filters inscriptions because Luke Dashjr's *policy* preferences differ from Bitcoin Core's *policy* preferences. Neither set is consensus. Both are defaults in C++ files.

A healthier ecosystem would have five implementations in five languages, each with their own opinionated defaults, and users would *choose* which policy they wanted to enforce. We don't have that. We have Core, Core-with-different-filters, and a long tail of toys.

## 7. What Would Help

A few concrete things would move the needle.

**Fund a non-C++ full validating node.** Not a library, not a wallet, not an SPV client. A full validating node that can be a peer on mainnet, written in Rust, Go, Java, or C#. The [Brink](https://brink.dev/), [Spiral](https://spiral.xyz/), and [HRF](https://hrf.org/) grant programmes already fund Bitcoin development. Earmarking grants specifically for alternative-language full-node implementations would dramatically lower the cost of entry for the next generation of contributors.

**Treat policy as a versioned, multi-implementation specification.** The dust threshold, the datacarrier limit, mempool eviction rules — these belong in a spec that multiple clients implement, not in `src/policy/policy.cpp`. The [BIP process](https://github.com/bitcoin/bips) has historically focused on consensus rules. Policy deserves the same rigour.

**Stop pretending Knots is diversity.** It isn't. It's a useful pressure release valve for policy disagreements within the Core codebase, and Luke Dashjr deserves credit for keeping the option alive. But if your "alternative client" is `git clone bitcoin && apply some patches`, you do not have client diversity. You have one client with a stable branch policy.

**Acknowledge the talent pipeline.** Bitcoin Core's contributor pool is aging. The next generation of systems programmers is being trained on Rust, not C++. Pretending otherwise will not change the demographics.

## 8. The Verdict

Bitcoin survived its monoculture problem in 2018 by luck and competence. The bug was found and patched before it was exploited. That is not a repeatable strategy. The next CVE-2018-17144 will arrive — not because the Core developers are negligent (they are not), but because every sufficiently complex C++ codebase eventually produces a critical memory-safety or logic bug.

When it does, the firewall has to exist. Today it barely does. Two C++ codebases sharing 95%+ of their consensus logic is not two codebases. It is one codebase with two maintainers.

If you are a Java engineer, a C# engineer, a Rust engineer, a Go engineer reading this and wondering whether Bitcoin needs your skills: it does. The protocol does not care what language you use. The reference implementation has just convinced itself, and the rest of us, that the protocol and the C++ are the same thing.

They are not.

---

*Are you working on a non-C++ Bitcoin implementation? Or are you a developer in Java, C#, Rust, or Go who has looked at the Bitcoin Core codebase and bounced off? I'd be interested to hear your perspective in the comments.*

---

**References:**
- [Bitcoin Core source — `src/policy/policy.cpp`](https://github.com/bitcoin/bitcoin/blob/master/src/policy/policy.cpp)
- [Bitcoin Knots](https://bitcoinknots.org/)
- [Bitcoin Knots — Coinomist explainer](https://thecoinomist.com/learn/what-is-bitcoin-knots-fork-by-luke-dashjr/)
- [CVE-2018-17144 — Bitcoin Optech](https://bitcoinops.org/en/topics/cve-2018-17144/)
- [Bitcoin Core 0.16.3 release announcement](https://bitcoincore.org/en/2018/09/18/release-0.16.3/)
- [Ethereum Client Diversity Dashboard](https://clientdiversity.org/)
- [ethereum.org — Client Diversity](https://ethereum.org/developers/docs/nodes-and-clients/client-diversity/)
- [CoinDesk — Nethermind bug and client diversity (2024)](https://www.coindesk.com/tech/2024/01/22/bug-on-ethereums-nethermind-software-sparks-discussion-of-client-diversity-risks)
- [Phoronix — Linux 7.0 concludes the Rust experiment](https://www.phoronix.com/news/Linux-7.0-Rust)
- [DevClass — Rust permanent in Linux kernel](https://devclass.com/2025/12/15/rust-boosted-by-permanent-adoption-for-linux-kernel-code/)
- [The New Stack — Rust goes mainstream in the Linux kernel](https://thenewstack.io/rust-goes-mainstream-in-the-linux-kernel/)
- [Ubuntu Discourse — Adopting sudo-rs by default in Ubuntu 25.10](https://discourse.ubuntu.com/t/adopting-sudo-rs-by-default-in-ubuntu-25-10/60583)
- [Trifecta Tech Foundation — Memory-safe sudo by default in Ubuntu](https://trifectatech.org/blog/memory-safe-sudo-to-become-the-default-in-ubuntu/)
- [Stack Overflow Developer Survey 2024 — Languages](https://survey.stackoverflow.co/2024/technology/#1-programming-scripting-and-markup-languages)
- [Stroustrup — History of C++ (HOPL paper)](https://www.stroustrup.com/hopl-almost-final.pdf)
- [btcd — Go full-node implementation](https://github.com/btcsuite/btcd)
- [Bitcoin Magazine — btcd: a full Bitcoin alternative written in Go](https://bitcoinmagazine.com/technical/btcd-a-full-bitcoin-alternative-written-in-go-1368114292)
- [bitcoinj — Java library](https://bitcoinj.org/)
- [NBitcoin — C# library](https://github.com/MetacoSA/NBitcoin)
- [rust-bitcoin](https://github.com/rust-bitcoin/rust-bitcoin)
- [Lightning Development Kit (LDK)](https://lightningdevkit.org/)
- [Coin Dance — Bitcoin nodes summary](https://coin.dance/nodes)
- [Bitnodes — Network leaderboard](https://bitnodes.io/nodes/leaderboard/)
- [Bitcoin.com News — Knots climbs to 19% of nodes](https://news.bitcoin.com/bitcoin-knots-climbs-to-19-of-nodes-as-core-v29-1-rollout-draws-blowback/)
- [CISA — The urgent need for memory safety in software products](https://www.cisa.gov/news-events/news/urgent-need-memory-safety-software-products)

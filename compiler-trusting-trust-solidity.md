# I Trust My Smart Contract. I Just Realised I Have No Idea If I Trust the Compiler.

I have audited Solidity contracts line by line, convinced myself a `transfer` did exactly what it said, and shipped them. What I never once did, in years of doing this, was read the bytecode that actually got deployed and check that it matched the source I signed off on. Nobody I worked with did either. We read the `.sol` file, we ran the tests, and then we trusted a binary called `solc` to faithfully translate one into the other.

That trust is almost entirely unexamined, and it is the single largest unaudited surface in the entire Ethereum stack. Not the contract. The thing that compiles the contract.

This is an old idea with a new and very expensive target. Let me walk through why a compromised Solidity compiler is close to the perfect crime, what it would look like, and the uncomfortable fact that we would probably mistake it for an ordinary bug.

## 1. Ken Thompson told us this in 1984

In his Turing Award lecture, [*Reflections on Trusting Trust*](https://www.profsandhu.com/cs5323_s18/thompson-1984.pdf) (Communications of the ACM, Vol. 27, No. 8, August 1984), Ken Thompson described a self-reproducing attack on a C compiler. The compiler was taught to recognise when it was compiling the `login` program and to silently insert a backdoor. Then it was taught to recognise when it was compiling *itself*, and to re-insert both tricks into the new compiler binary. After that, you could delete the malicious source. The clean source compiled clean. The binary stayed poisoned forever.

Thompson's conclusion is the sentence every smart-contract developer should have tattooed somewhere visible:

> "You can't trust code that you did not totally create yourself. (Especially code from companies that employ people like me.)"

He was making a point about login backdoors on a PDP-11. But the structure of the attack, *the tool that builds trusted software is itself the thing you forgot to verify*, maps onto Solidity with almost no modification. In fact it maps *better*, because the payoff is denominated in liquid, instantly transferable money.

## 2. Why Solidity is a juicier target than Unix login ever was

Three properties make the Solidity compiler an unusually attractive place to hide.

**The output is money, and it moves itself.** A backdoored `login` gets you a shell. A backdoored `solc` gets you an ERC-20 `transfer` that it rewrote. There is no lateral movement, no persistence, no exfiltration step. The contract *is* the loot and the contract *is* the getaway car.

**Almost nobody reads bytecode.** EVM bytecode is a stack machine with no variable names and no structure. Auditors read Solidity. Etherscan "verification" recompiles the source and checks that the hash matches *what that same compiler produces*, which, as we will see, is exactly the wrong oracle if the compiler is the attacker. Source verification confirms the compiler agrees with itself.

**The bug is plausibly deniable.** This is the part that should keep you up at night, and it is specific to Solidity. The compiler emitting bytecode that does not match the source *is already a thing that happens by accident.* The Solidity team publishes a [List of Known Bugs](https://docs.soliditylang.org/en/latest/bugs.html), a machine-readable `bugs.json`, precisely because `solc` has, in shipped releases, miscompiled correct source. A [2016 storage bug](https://docs.soliditylang.org/en/latest/bugs.html) left high-order bytes uncleaned and could overwrite adjacent storage (introduced in 0.1.6, fixed in 0.4.4). A [2017 optimizer bug](https://blog.ethereum.org/2017/05/03/solidity-optimizer-bug) generated incorrect routines for certain constants. A [2021 Keccak optimizer bug](https://blog.soliditylang.org/2021/03/23/keccak-optimizer-bug/) wrongly reused cached hash results. Certora documented the [0.7.3 compiler writing garbage into storage](https://www.certora.com/blog/the-solidity-compiler-silently-corrupts-storage).

Sit with that. The honest compiler already produces wrong bytecode often enough to warrant a public bug registry. A malicious payload that was ever discovered would land in exactly the same bucket as those entries ("weird miscompilation, patched in the next release") unless someone proved intent. The attacker gets deniability for free.

## 3. "Review the geth code," and why the answer points at solc, not geth

The request that kicked this article off was to review geth, the dominant [Go Ethereum execution client](https://github.com/ethereum/go-ethereum), for where a compiler attack would bite. The interesting finding is that it *wouldn't* bite there, and understanding why tells you exactly where the real surface is.

Geth's job is to execute bytecode deterministically. When a transaction calls a contract, geth's EVM interpreter runs the opcodes (the [`opCall`, `opSstore`, `opLog` handlers](https://github.com/ethereum/go-ethereum/blob/master/core/vm/instructions.go) and so on), and every other client (Nethermind, Besu, Erigon, Reth) must produce an *identical* state transition or the chain forks. That consensus requirement means geth is about as heavily scrutinised and cross-checked as software gets. It faithfully does what the bytecode says.

That is the whole problem. Geth faithfully executes the bytecode. It has no idea, and no way to know, that the bytecode says something different from the Solidity you wrote. The EVM does not see your source. By the time anything reaches geth, the translation you never verified has already happened. The execution layer is a red herring; the compiler is the crime scene. Reviewing geth for this threat is like dusting the bank vault for prints when the forgery happened at the mint.

## 4. What the hack looks like (in shape, not in shipping form)

I am deliberately going to describe this at the level of a compiler *pass* in pseudocode, not hand out a working `solc` patch. The mechanism is the point; a deployable weapon is not, and the mechanism is enough to show why detection is hard. Every real compiler has an intermediate representation it rewrites. Solidity has [Yul](https://docs.soliditylang.org/en/latest/yul.html). A malicious pass would sit there and do three things.

**Step one: recognise the victim pattern.** ERC-20 is a rigid, standardised interface, which makes it trivial to fingerprint in the IR. The pass watches for the shape of a token transfer:

```
# PSEUDOCODE: illustrative, not a compiler patch
for each function in contract.IR:
    if function.selector == keccak256("transfer(address,uint256)")[:4]:
        mark_as_target(function)
```

**Step two: rewrite the destination.** Instead of honouring the caller's `to` address, the generated code diverts some fraction to an attacker address, but only under conditions that keep the contract looking honest during testing:

```
# PSEUDOCODE: conceptual, omits the real codegen
inject_before(function.body):
    if tx.value_being_moved > DUST_THRESHOLD:
        redirect_fraction(to = ATTACKER_ADDR, bps = 50)   # skim, don't drain
```

Note the restraint. A compiler that drained the first test transfer to zero would be caught in the first unit test. A compiler that skims fifty basis points above a dust threshold passes your tests, passes the audit's happy path, and only becomes visible in aggregate on-chain much later.

**Step three: the time bomb.** This is the detail from the original idea that makes it genuinely dangerous, and it has a real-world precedent (section 5). The payload stays fully dormant until a trigger: a block height, a date, a magic value in calldata:

```
# PSEUDOCODE
guard = "if block.number < 25_000_000: behave_honestly()"
```

Set the trigger far enough out and the attack has a *deployment phase* and a *harvest phase*. For months, every contract built with the poisoned compiler behaves perfectly. Audits pass. TVL accumulates. Confidence compounds. Then the block height ticks over and every contract compiled in that window turns hostile on the same day. You are not draining one contract; you are draining a cohort.

And to complete Thompson's loop: if this logic also recognised when it was compiling the Solidity compiler's own build, the `bugs.json`-worthy source could be deleted and the behaviour would survive in the binaries anyway. Solidity's browser build, [`soljson.js`, is produced from the same C++ source via Emscripten](https://docs.soliditylang.org/en/latest/installing-solidity.html), one artifact, shipped to Remix and much of the JS tooling ecosystem.

## 5. This is not hypothetical. It already happened to a Bitcoin wallet.

Everything above has a working precedent, and it specifically targeted cryptocurrency.

In late 2018, the popular npm package `event-stream` pulled in a new transitive dependency, `flatmap-stream`, after the original maintainer [handed the project to an unknown contributor](https://snyk.io/blog/a-post-mortem-of-the-malicious-event-stream-backdoor/). Snyk's post-mortem documents what the payload did: it was **encrypted, and only decrypted when the host application being built was [Copay, a Bitcoin wallet](https://snyk.io/blog/a-post-mortem-of-the-malicious-event-stream-backdoor/)**. The decryption key was the target app's own `npm_package_description`. It activated **at build time**, injecting code to steal wallet keys and seeds into the compiled app. BitPay confirmed [Copay versions 5.0.2 through 5.1.0 were affected](https://thehackernews.com/2018/11/nodejs-event-stream-module.html) and told users to assume their keys were compromised.

Read that back against section 4: target-specific recognition, dormant until the build, payload that only wakes for the victim. That is the compiler-attack blueprint, executed against a crypto wallet, seven years ago.

It keeps happening:

- **XZ Utils (CVE-2024-3094, 2024).** A backdoor was hidden [not in the readable source but in the *build process*](https://www.mend.io/blog/critical-backdoor-found-xz-utils-cve-2024-3094), extracted from disguised binary test files during compilation, present in release tarballs 5.6.0/5.6.1 but not the Git tree. CVSS 10. Caught by accident, because one engineer noticed sshd was using half a second too much CPU.
- **Solana `@solana/web3.js` (CVE-2024-54134, December 2024).** A [compromised maintainer account](https://thehackernews.com/2024/12/researchers-uncover-backdoor-in-solanas.html) pushed versions 1.95.6/1.95.7 of the official library with an `addToQueue` function that exfiltrated private keys. Around $184,000 gone in a roughly five-hour window from a package with tens of millions of downloads.

The compiler is a harder target to compromise than an npm package, but it is the same category of attack with a far larger blast radius, because it sits upstream of *everything* built with it.

## 6. How you actually defend against this

The honest answer is that you cannot fully, because Thompson proved you can't. But you can collapse the attack surface dramatically, and most teams do none of this.

1. **Verify compiler binaries against the authoritative source.** Solidity's docs are explicit that HTTPS is not the protection that matters: [obtain the file list securely and verify the hashes](https://docs.soliditylang.org/en/latest/installing-solidity.html) of binaries after downloading, using `solc-bin`/`binaries.soliditylang.org` as the source of truth. Pin exact compiler versions in your build and check the hash. Do not `npm install` a floating `solc`.

2. **Diff the deployed bytecode, not just the source.** Etherscan source-verification asks "does this source, through this compiler, produce this bytecode?", which a malicious compiler answers "yes" to trivially. The stronger check is reproducing the build with an *independently obtained* compiler and diffing the output. Divergence is the signal.

3. **Compile the same source with two different compiler versions and compare.** A payload keyed to one build won't usually reproduce identically across versions. Cheap, and it would have flagged a single poisoned release.

4. **Support and demand [reproducible builds](https://reproducible-builds.org/).** If independent parties can rebuild `solc` byte-for-byte from source, a poisoned binary stops matching and gets noticed. This is the only real structural answer to the trusting-trust problem, and it is exactly David A. Wheeler's "[Diverse Double-Compiling](https://dwheeler.com/trusting-trust/)" idea: use an independent compiler to check the one you suspect.

5. **Read the opcodes for the functions that move money.** You do not need to read all the bytecode. You need to read the `transfer`, `transferFrom`, and `approve` paths and confirm the destination is the one the caller supplied. For a token holding real value, that is an afternoon, and almost no one spends it.

## 7. The uncomfortable conclusion

We spend enormous effort auditing smart contracts and almost none auditing the compiler that writes the bytecode those contracts become. We treat `solc` the way I treated bitaddress.org years ago: as trustworthy because it is widely used and open source, which are not the same thing as verified. "Open source" means the *source* is auditable. It says nothing about whether the *binary in your toolchain* matches that source, and the binary is what runs.

Thompson's trick works because trust is transitive and we never check the first link. In crypto, the first link compiles directly into money that moves itself to whoever the bytecode names. We have built an industry on the assumption that the compiler is honest, and we have never once made it prove it.

**Have you ever diffed your deployed bytecode against an independent build of your source? If not, and be honest, what exactly is your trust in `solc` based on? I'd genuinely like to hear how teams are handling this, because most of the ones I've asked had never thought about it. Let's talk in the comments.**

---

## References

- [Ken Thompson, Reflections on Trusting Trust (Communications of the ACM, Vol. 27, No. 8, August 1984)](https://www.profsandhu.com/cs5323_s18/thompson-1984.pdf)
- [Solidity: List of Known Bugs (bugs.json)](https://docs.soliditylang.org/en/latest/bugs.html)
- [Ethereum Foundation: Solidity Optimizer Bug (2017)](https://blog.ethereum.org/2017/05/03/solidity-optimizer-bug)
- [Solidity Blog: Keccak Optimizer Bug (2021)](https://blog.soliditylang.org/2021/03/23/keccak-optimizer-bug/)
- [Certora: The Solidity Compiler Silently Corrupts Storage](https://www.certora.com/blog/the-solidity-compiler-silently-corrupts-storage)
- [Solidity: Installing the Solidity Compiler (binary verification, solc-bin, soljson.js)](https://docs.soliditylang.org/en/latest/installing-solidity.html)
- [Solidity: Yul intermediate representation](https://docs.soliditylang.org/en/latest/yul.html)
- [go-ethereum (geth) source](https://github.com/ethereum/go-ethereum)
- [go-ethereum EVM instruction handlers](https://github.com/ethereum/go-ethereum/blob/master/core/vm/instructions.go)
- [Snyk: A post-mortem of the malicious event-stream backdoor](https://snyk.io/blog/a-post-mortem-of-the-malicious-event-stream-backdoor/)
- [The Hacker News: Backdoor in event-stream NodeJS module targeting Copay](https://thehackernews.com/2018/11/nodejs-event-stream-module.html)
- [Mend.io: Critical Backdoor Found in XZ Utils (CVE-2024-3094)](https://www.mend.io/blog/critical-backdoor-found-xz-utils-cve-2024-3094)
- [The Hacker News: Backdoor in Solana's web3.js npm Library (CVE-2024-54134)](https://thehackernews.com/2024/12/researchers-uncover-backdoor-in-solanas.html)
- [Reproducible Builds project](https://reproducible-builds.org/)
- [David A. Wheeler: Countering Trusting Trust through Diverse Double-Compiling](https://dwheeler.com/trusting-trust/)

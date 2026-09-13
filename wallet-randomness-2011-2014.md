# I Read the Randomness Code in Three Early Bitcoin Wallets. It Told Three Very Different Stories.

I still have a paper wallet I generated on [bitaddress.org](https://www.bitaddress.org/) years ago, folded in a drawer with a little dust of guilt on it. I trusted it because everyone trusted it. It was open source, it ran in the browser, you wiggled your mouse to "add entropy," and out came a key. That was the ritual. Nobody I knew ever asked the only question that actually mattered: *where did the randomness come from?*

So I spent a weekend reading the actual code of three wallets people relied on between 2011 and 2014 — bitaddress.org, bitcoinj, and Electrum. Same era. Same curve ([secp256k1](https://en.bitcoin.it/wiki/Secp256k1)). Wildly different safety. And the thing that separated the safe from the catastrophic wasn't the cryptography — the elliptic-curve maths was identical and correct in all three. It was the randomness. Randomness is where early wallets quietly lived or died, and almost nobody was looking.

The findings are below.

## 1. Why randomness is the whole ballgame

A Bitcoin private key is just a random 256-bit number. If an attacker can predict or reproduce the number your wallet "randomly" chose, they own the coins. There's no password to also crack, no second factor. The randomness *is* the security.

It gets worse for signatures. Every time you spend, your wallet produces an ECDSA signature, and each signature needs a fresh random number called a **nonce**, usually written `k`. The [RFC 6979](https://datatracker.ietf.org/doc/html/rfc6979) spec puts it bluntly: `k` "must be chosen randomly and uniformly" and kept secret. Reuse the same `k` for two different signatures under the same key and it's not a weakness — it's total collapse. With simple algebra, anyone who sees those two signatures on the public blockchain can [recover your private key](https://security.unboundcompute.com/ecdsa-nonce-reuse/). No brute force required.

So there are two randomness failure modes: a weak key (predictable at birth) and a reused nonce (leaks the key on first spend). The three wallets I read sit at three different points on that risk map.

## 2. bitaddress.org — saved by your mouse

bitaddress.org was the paper-wallet generator. Its randomness came from a JavaScript class called `SecureRandom`, and if you read the seeding code from the pre-2014 era, the initial pool was filled by pulling bytes out of [`Math.random()`](https://github.com/pointbiz/bitaddress.org/blob/d389e975fe8c1b4590843c1b3854c98700525c78/src/securerandom.js) — `Math.floor(65536 * Math.random())` in a loop — mixed with a browser "fingerprint" (screen size, timezone, user agent, plugins). `Math.random()` is explicitly *not* a cryptographic RNG. On its own, that's a frightening way to mint a key that guards real money.

Whole-pool seeding from the browser's proper cryptographic RNG, `window.crypto.getRandomValues`, only arrived later. The [changelog](https://github.com/pointbiz/bitaddress.org/blob/master/CHANGELOG.txt) is precise about it: version 2.8.0, dated **2014-01-18**, is where the "whole seed pool [is] initially filled by window.crypto.getRandomValues." Before that release, you were leaning on `Math.random()` plus fingerprinting.

What actually saved most bitaddress users was the ritual I rolled my eyes at: the mouse-wiggling. Collecting mouse-movement entropy was added [all the way back in version 0.5, 2011-09-19](https://github.com/pointbiz/bitaddress.org/blob/master/CHANGELOG.txt) ("Added extra entrophy with mouse movement technique"). The exact microsecond timings of a human dragging a cursor are genuinely hard for a remote attacker to reproduce, and they got mixed into the pool. That human-in-the-loop entropy is probably why we didn't see a bloodbath of drained paper wallets.

The part that still bothers me is this: **a weak key leaves no fingerprint on-chain.** A key generated with too little entropy looks identical to a perfect one until the day someone guesses it. You can never prove your old paper wallet was safe. You can only stop relying on the question.

## 3. bitcoinj — the one where people actually lost coins

bitcoinj is the Java library behind a generation of Android wallets. Pre-0.12, it signed transactions with a nonce `k` pulled straight from the system's `SecureRandom`. On a healthy operating system, fine. On Android in 2013, not fine.

On **11 August 2013**, bitcoin.org published a [security alert](https://bitcoin.org/en/alert/2013-08-11-android): "a component of Android responsible for generating secure random numbers contains critical weaknesses, that render all Android wallets generated to date vulnerable to theft." Android's `SecureRandom` was sometimes not seeded properly and returned repeating output. The affected apps read like the Android wallet landscape of the day — [Bitcoin Wallet, blockchain.info, BitcoinSpinner, and Mycelium](https://thehackernews.com/2013/08/hacking-bitcoin-android-vulnerability-digital-wallets.html).

The result was exactly the nonce-reuse catastrophe from section 1. Wallets signed two transactions with the same `k`, both signatures landed on the public chain, and attackers ran the algebra. This wasn't theoretical — [users spotted coins being swept](https://thehackernews.com/2013/08/hacking-bitcoin-android-vulnerability-digital-wallets.html) out of affected addresses within hours of a bad signature. Real money, gone.

The structural fix came in [**bitcoinj 0.12**, released 3 October 2014](https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2014-October/006678.html): per the [release notes](https://bitcoinj.org/release-notes), "bitcoinj now uses deterministic ECDSA for signing." That's RFC 6979 — and it matters because of *how* it removes the risk, which is the whole point of this article.

## 4. RFC 6979 — deleting a category of disaster

[RFC 6979](https://datatracker.ietf.org/doc/html/rfc6979) was published in August 2013, the same month the Android bug blew up. Its idea is quietly radical: don't generate the nonce `k` randomly at all. Derive it deterministically, with HMAC, from the private key and the message being signed. Same key + same message always produces the same `k` — and critically, two *different* messages produce two different `k` values, because the message is an input.

There is no RNG to misfire. A broken `SecureRandom` can't cause nonce reuse if the nonce was never asked from the RNG in the first place. The spec makes the pitch directly: deterministic signatures "do not need access to a source of high-quality randomness" yet keep all the security properties of ECDSA.

The concept is simpler than it sounds. Here's the shape of it in a few lines of Python, using the standard library only — deriving `k` from the key and message hash rather than from any random source:

```python
import hmac, hashlib

def deterministic_k(private_key: int, msg_hash: bytes, order: int) -> int:
    # RFC 6979, simplified: derive the ECDSA nonce k with HMAC-SHA256
    # from the private key and the message. No RNG involved.
    x = private_key.to_bytes(32, "big")
    V = b"\x01" * 32
    K = b"\x00" * 32
    K = hmac.new(K, V + b"\x00" + x + msg_hash, hashlib.sha256).digest()
    V = hmac.new(K, V, hashlib.sha256).digest()
    K = hmac.new(K, V + b"\x01" + x + msg_hash, hashlib.sha256).digest()
    V = hmac.new(K, V, hashlib.sha256).digest()
    while True:
        V = hmac.new(K, V, hashlib.sha256).digest()
        k = int.from_bytes(V, "big")
        if 1 <= k < order:          # valid nonce in range
            return k
        K = hmac.new(K, V + b"\x00", hashlib.sha256).digest()
        V = hmac.new(K, V, hashlib.sha256).digest()

# Same key + same message => same k, every time, on any machine.
order = 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFEBAAEDCE6AF48A03BBFD25E8CD0364141  # secp256k1 n
k1 = deterministic_k(0xDEAD, hashlib.sha256(b"pay Alice 1 BTC").digest(), order)
k2 = deterministic_k(0xDEAD, hashlib.sha256(b"pay Bob 2 BTC").digest(), order)
print("same message, reproducible:", k1 == deterministic_k(0xDEAD, hashlib.sha256(b"pay Alice 1 BTC").digest(), order))
print("different messages, different k:", k1 != k2)
```

Run it and you get `True` then `True`: reproducible for identical inputs, but never colliding across different messages. That second line is the entire fix. Nonce reuse — the thing that drained the Android wallets — becomes structurally impossible, no matter how broken the machine's RNG is.

## 5. Electrum — the one built the right way (with a caveat)

Electrum stands out because of *where* it took its randomness from. For seed generation it used the operating system's cryptographic RNG (`os.urandom`) rather than a home-rolled JavaScript pool — a much stronger foundation than `Math.random()`.

I want to be careful here, because the tidy version of this story overstates it. Electrum did not spring into existence already RFC 6979-compliant. The [GitHub issue proposing deterministic signatures](https://github.com/spesmilo/electrum/issues/323) — "Upgrade to python-ecdsa 0.9 and use deterministic signatures" — was opened on **2 October 2013**, two months *after* the Android disaster, as an enhancement. So the honest framing is: Electrum started from a sound entropy source, and the community moved toward deterministic nonces as the industry-wide lesson landed, rather than having foreseen it.

That's still the right trajectory. The wallets that survived the era weren't the ones with cleverer cryptography. They were the ones that either used a real entropy source from the start, or moved fastest to stop depending on ambient randomness they couldn't verify.

## 6. The lesson that aged well

Don't depend on randomness you can't verify. That's the whole thing.

The Android wallets trusted the OS RNG and got burned. bitaddress trusted `Math.random()` and got quietly rescued by your mouse. The fix that made all of this go away — deterministic nonces — won by *removing* the dependency, not by finding better randomness. Which is exactly why [Bitcoin Core](https://github.com/bitcoin/bitcoin/pull/5227), bitcoinj, and every serious wallet since uses RFC 6979 today.

There's one action item that follows directly. **If you generated a paper wallet before 2014 and it still holds funds: sweep it into a fresh key.** You can't prove the old key was made with enough entropy — that question has no on-chain answer — but you can make it irrelevant. Sweeping costs you a transaction fee and closes a door you otherwise have to leave open forever.

I'm going to go find that folded paper in my drawer and do exactly that.

---

_Did you generate keys on any of these tools back in the day? Have you swept your old paper wallets, or are they still sitting in a drawer like mine were? I'd genuinely like to know how many of us are still carrying pre-2014 entropy risk around._

---

**References:**

- [Bitcoin.org — Android Security Vulnerability alert (11 Aug 2013)](https://bitcoin.org/en/alert/2013-08-11-android)
- [The Hacker News — Android Bitcoin wallet apps vulnerable to theft (Aug 2013)](https://thehackernews.com/2013/08/hacking-bitcoin-android-vulnerability-digital-wallets.html)
- [RFC 6979 — Deterministic Usage of DSA and ECDSA (IETF)](https://datatracker.ietf.org/doc/html/rfc6979)
- [Unbound — ECDSA Nonce Reuse: how one repeated number leaks the private key](https://security.unboundcompute.com/ecdsa-nonce-reuse/)
- [bitaddress.org — CHANGELOG (v0.5 mouse entropy 2011; v2.8.0 getRandomValues 2014-01-18)](https://github.com/pointbiz/bitaddress.org/blob/master/CHANGELOG.txt)
- [bitaddress.org — securerandom.js source (Math.random pool seeding)](https://github.com/pointbiz/bitaddress.org/blob/d389e975fe8c1b4590843c1b3854c98700525c78/src/securerandom.js)
- [bitcoinj — 0.12 release announcement (3 Oct 2014, bitcoin-dev list)](https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2014-October/006678.html)
- [bitcoinj — release notes ("deterministic ECDSA for signing")](https://bitcoinj.org/release-notes)
- [Electrum — Issue #323: Upgrade to python-ecdsa 0.9 and use deterministic signatures (opened 2 Oct 2013)](https://github.com/spesmilo/electrum/issues/323)
- [Bitcoin Core — PR #5227: Deterministic signatures (sipa)](https://github.com/bitcoin/bitcoin/pull/5227)
- [secp256k1 — Bitcoin Wiki](https://en.bitcoin.it/wiki/Secp256k1)

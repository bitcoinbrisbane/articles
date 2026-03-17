# Passkeys and Australian Crypto Exchanges: Who's Protecting You and Who Isn't?

We noticed last month that the DCA platform [www.getpaidinbitcoin.com.au](https://www.getpaidinbitcoin.com.au) recently rolled out passkey authentication. That caught our attention — not because passkeys are new, but because an Australian Bitcoin platform beat most of the country's major exchanges to one of the most important security upgrades in a decade.

If you're using an Australian crypto exchange in 2026 and your login still depends on a password and a six-digit code from Google Authenticator, you're using security technology from 2012. The rest of the internet has moved on. The question is: has your exchange?

## What Is a Passkey?

A passkey is a [FIDO2/WebAuthn credential](https://fidoalliance.org/passkeys/) that replaces your password with public-key cryptography. Instead of typing a password that can be stolen, phished, or leaked in a data breach, your device creates a unique cryptographic key pair:

- The **private key** stays on your device (phone, laptop, or hardware key) and never leaves it
- The **public key** is sent to the website and stored on their server

When you log in, the website sends a challenge. Your device signs it with the private key. The website verifies it with the public key. That's it — [no password is transmitted, stored, or stealable](https://www.webauthn.me/passkeys).

You authenticate locally using your fingerprint, face, or device PIN — the same biometric or PIN you already use to unlock your phone.

### Why Passkeys Matter for Crypto

Passkeys are **phishing-resistant by design.** A passkey created for `exchange.com` [cryptographically cannot be used on `exchannge.com`](https://fidoalliance.org/passkeys/) — even if you're tricked into visiting a fake site, the passkey simply won't work. This is fundamentally different from passwords and TOTP codes, which work on any site you type them into.

This matters because crypto users are high-value targets. [SIM swap fraud cases jumped 1,055% in 2024](https://keepnetlabs.com/blog/what-is-sim-swap-fraud), and the FBI tracked [$25.9 million in SIM swap losses](https://deepstrike.io/blog/sim-swap-scam-statistics-2025) in the US alone that year. The [FTX hack in 2022 exploited SMS-based 2FA via a SIM swap](https://cloudsecurityalliance.org/blog/2025/09/02/a-successful-sim-swap-attack-unpacking-the-2022-ftx-hack), resulting in over $400 million in losses.

Even authenticator app codes (TOTP) are vulnerable to real-time phishing proxies that relay codes as you type them. Passkeys eliminate this entire attack class.

The [Australian Signals Directorate (ASD)](https://www.cyber.gov.au/protect-yourself/secure-your-accounts/passkeys) now recommends passkeys as a preferred method for securing online accounts. When the government agency responsible for national cybersecurity tells you to use passkeys, it's worth listening.

## The Global Picture: Exchanges Are Moving Fast

Internationally, major exchanges have adopted passkeys aggressively. According to [Corbado's passkey tracker](https://www.corbado.com/faq/crypto-passkeys) and exchange documentation:

### Full Passkey Support (Passwordless Login)

| Exchange | Passkey Login | Details |
|----------|:---:|---------|
| **Coinbase** | Yes | [Supports multiple passkeys per account](https://www.corbado.com/faq/crypto-passkeys), synced and device-bound |
| **Binance** | Yes | [Synced passkeys and hardware keys](https://www.corbado.com/blog/binance-passkeys) for login and 2FA |
| **OKX** | Yes | 24-hour withdrawal hold if reset via non-passkey method |
| **Bybit** | Yes | Supports iCloud Keychain, QR code setup, browser-specific passkeys |
| **Crypto.com** | Yes | Unified across mobile and web |
| **KuCoin** | Yes | Full passwordless with synced and hardware key options |
| **Gemini** | Yes | [Mandatory for all users since May 2025](https://www.gemini.com/blog/geminis-updated-passkeys-feature-is-now-more-secure-and-streamlined) — 269% rise in authentications |

### Partial Support (2FA Only, Not Passwordless)

| Exchange | Passkey Login | Details |
|----------|:---:|---------|
| **Kraken** | 2FA only | [Username/password still required](https://support.kraken.com/articles/what-is-a-passkey); passkey used as second factor |
| **Bitfinex** | 2FA only | Limited to device-bound FIDO2/U2F hardware keys |

### No Passkey Support

| Exchange | Passkey Login | Details |
|----------|:---:|---------|
| **Bitstamp** | No | Traditional 2FA only |
| **Gate.io** | No | No official support announced |
| **Binance.US** | No | Separate from global Binance; no passkey functionality |

Gemini's decision to [make passkeys mandatory](https://walletwhispers.com/news/gemini-account-passkey-setup-requirement/) is particularly notable. Users who didn't set up a passkey or authenticator app by May 24, 2025 lost access to their accounts until they complied. Bold — but the 269% increase in authentication activity suggests it worked.

## Australian Exchanges: The Scorecard

Here's where it gets concerning. We reviewed the authentication options for every major Australian crypto exchange. The results are not great.

| Exchange | Passkey Support | Current Auth Method | Source |
|----------|:---:|---|---|
| **Get Paid In Bitcoin** | **Yes** | Passkey authentication | [getpaidinbitcoin.com.au](https://www.getpaidinbitcoin.com.au) |
| **CoinSpot** | No | [Google Authenticator / SMS 2FA](https://coinspot.zendesk.com/hc/en-us/articles/360000120996-How-to-set-up-2FA) | [CoinSpot Help Centre](https://coinspot.zendesk.com/hc/en-us/sections/115000049254-2FA-Google-Authenticator-Security) |
| **Swyftx** | No | Authenticator app 2FA | [Swyftx Security](https://swyftx.com) |
| **Independent Reserve** | No | [Google Authenticator / SMS backup](https://www.independentreserve.com/blog/knowledge-base/setting-up-2fa-using-google-authenticator) | [Independent Reserve Security](https://www.independentreserve.com/security) |
| **BTC Markets** | No | [Google Authenticator 2FA](https://support.btcmarkets.net/hc/en-us/articles/360001307907-Two-Factor-Authentication-2FA-Set-up-Guide) | [BTC Markets Support](https://support.btcmarkets.net/hc/en-us/articles/360001821168-Account-Security-on-BTC-Markets) |
| **CoinJar** | No | [Authenticator app / SMS 2FA](https://support.coinjar.com/hc/en-au/articles/202910075-Securing-your-CoinJar-with-Enhanced-Security-2FA) | [CoinJar Support](https://support.coinjar.com) |
| **Digital Surge** | No | [Authenticator app 2FA (compulsory)](https://help.digitalsurge.com.au/en/articles/5573002-how-to-enable-two-factor-authentication-2fa) | [Digital Surge Security](https://digitalsurge.com.au/security/) |

**Out of seven Australian platforms reviewed, only one — Get Paid In Bitcoin — supports passkeys.**

Every other major Australian exchange still relies on authenticator app TOTP codes as their strongest authentication option. Several still offer SMS 2FA as a backup or even a primary method, despite SMS being [the attack vector that enabled the $400 million FTX hack](https://cloudsecurityalliance.org/blog/2025/09/02/a-successful-sim-swap-attack-unpacking-the-2022-ftx-hack).

CoinSpot has been [specifically targeted by phishing campaigns](https://www.bleepingcomputer.com/news/security/phishing-campaign-targets-coinspot-cryptoexchange-2fa-codes/) designed to steal 2FA codes — exactly the kind of attack that passkeys make impossible.

## Why This Gap Matters

This isn't just about convenience. It's a security gap with real consequences.

**TOTP codes can be phished in real-time.** An attacker sets up a proxy site that looks identical to your exchange. You enter your password and your 2FA code. The proxy relays both to the real site in real-time and takes over your session. Passkeys make this impossible because the credential is [cryptographically bound to the legitimate domain](https://fidoalliance.org/passkeys/).

**SMS 2FA is fundamentally broken.** [42% of UK banks and 61% of crypto exchanges still used SMS as their default second factor in 2024](https://keepnetlabs.com/blog/what-is-sim-swap-fraud). SIM swaps are cheap, fast, and increasingly automated. If your exchange offers SMS 2FA, they're offering you a lock that can be picked with a phone call to your carrier.

**Passwords get reused.** Even with 2FA, the password itself is a liability. Data breaches expose credentials that get tried on every exchange. Passkeys eliminate the password entirely — there's nothing to reuse, nothing to leak, nothing to guess.

## The Regulatory Context

Australian exchanges operate under [AUSTRAC's AML/CTF Act 2006](https://www.austrac.gov.au/business/legislation/amlctf-act), which requires them to maintain [AML/CTF programs](https://www.austrac.gov.au/business/how-comply-guidance-and-resources/guidance-resources/guide-preparing-and-implementing-amlctf-program-your-digital-currency-exchange-business) that include customer identification and ongoing due diligence. The [Corporations Amendment (Digital Assets Framework) Bill 2025](https://www.aph.gov.au/Parliamentary_Business/Bills_Legislation/bd/bd2526/26bd040) will require digital asset platforms to hold an AFSL and to [act efficiently, honestly and fairly](https://www.gtlaw.com.au/insights/key-topics/regulation-in-motion/australia-reshapes-digital-asset-regulation).

[ASIC's updated Regulatory Guide 133](https://www.asic.gov.au/regulatory-resources/find-a-document/regulatory-guides/rg-133-funds-management-and-custodial-services-holding-assets/) now includes specific guidance on crypto-asset custody, mandating measures to [mitigate cybersecurity threats and operational vulnerabilities](https://www.kwm.com/au/en/insights/latest-thinking/crypto-custody-asic-expands-its-regulatory-guidance-under-rg-133.html).

None of these regulations explicitly mandate passkeys. But they do require platforms to manage cybersecurity risk. When phishing-resistant authentication exists, is free to implement, is supported by every major browser and operating system, and is [recommended by the Australian Signals Directorate](https://www.cyber.gov.au/protect-yourself/secure-your-accounts/passkeys) — can an exchange that still relies on phishable 2FA honestly claim it's managing that risk?

## What Should You Do?

1. **Check if your exchange supports passkeys.** If they do, set one up now.
2. **If they don't, ask them why.** A support ticket or public question on social media goes a long way.
3. **Never use SMS 2FA** on a crypto exchange if an authenticator app is available. SMS is better than nothing, but it's the weakest option.
4. **Consider a hardware security key** (YubiKey, Titan) as a passkey device. It can't be synced or compromised remotely.
5. **Use exchanges that take security seriously.** If Gemini can mandate passkeys for every user, your local exchange can at least offer them.

## The Bottom Line

A Bitcoin DCA platform in Australia rolled out passkeys before CoinSpot, Swyftx, Independent Reserve, BTC Markets, CoinJar, and Digital Surge. That's not a knock on Get Paid In Bitcoin — it's a question for every other platform: what's taking so long?

Passkeys are a [FIDO Alliance standard](https://fidoalliance.org/passkeys/), supported natively on iOS, Android, macOS, Windows, and every major browser. The implementation cost is minimal. The security benefit is enormous. And the longer Australian exchanges wait, the more their users remain exposed to phishing, SIM swaps, and credential theft that passkeys were specifically designed to prevent.

Your crypto is only as secure as your login. In 2026, a password and a six-digit code shouldn't be the best your exchange can offer.

---

*Does your exchange support passkeys? Have you made the switch? I'd be interested to hear your experience in the comments.*

---

**References:**
- [FIDO Alliance — Passkeys](https://fidoalliance.org/passkeys/)
- [WebAuthn — How Passkeys Work](https://www.webauthn.me/passkeys)
- [Australian Signals Directorate — Passkeys Guidance](https://www.cyber.gov.au/protect-yourself/secure-your-accounts/passkeys)
- [Corbado — Which Crypto Exchanges Offer Passkeys?](https://www.corbado.com/faq/crypto-passkeys)
- [Corbado — Binance Passkeys](https://www.corbado.com/blog/binance-passkeys)
- [Gemini — Updated Passkeys Feature](https://www.gemini.com/blog/geminis-updated-passkeys-feature-is-now-more-secure-and-streamlined)
- [Gemini — Mandatory Passkey Setup](https://walletwhispers.com/news/gemini-account-passkey-setup-requirement/)
- [Gemini — Setting Up Passkeys](https://support.gemini.com/hc/en-us/articles/38825558255771-Setting-up-passkeys-for-your-Gemini-account)
- [Kraken — What Is a Passkey?](https://support.kraken.com/articles/what-is-a-passkey)
- [CoinSpot — 2FA Setup](https://coinspot.zendesk.com/hc/en-us/articles/360000120996-How-to-set-up-2FA)
- [CoinSpot — Phishing Campaign Targeting 2FA Codes (BleepingComputer)](https://www.bleepingcomputer.com/news/security/phishing-campaign-targets-coinspot-cryptoexchange-2fa-codes/)
- [Independent Reserve — 2FA Setup](https://www.independentreserve.com/blog/knowledge-base/setting-up-2fa-using-google-authenticator)
- [Independent Reserve — Security](https://www.independentreserve.com/security)
- [BTC Markets — 2FA Setup Guide](https://support.btcmarkets.net/hc/en-us/articles/360001307907-Two-Factor-Authentication-2FA-Set-up-Guide)
- [BTC Markets — Account Security](https://support.btcmarkets.net/hc/en-us/articles/360001821168-Account-Security-on-BTC-Markets)
- [CoinJar — Enhanced Security (2FA)](https://support.coinjar.com/hc/en-au/articles/202910075-Securing-your-CoinJar-with-Enhanced-Security-2FA)
- [Digital Surge — 2FA Setup](https://help.digitalsurge.com.au/en/articles/5573002-how-to-enable-two-factor-authentication-2fa)
- [Digital Surge — Security](https://digitalsurge.com.au/security/)
- [Get Paid In Bitcoin](https://www.getpaidinbitcoin.com.au)
- [SIM Swap Fraud Statistics 2025 — Keepnet](https://keepnetlabs.com/blog/what-is-sim-swap-fraud)
- [SIM Swap Scam Statistics — DeepStrike](https://deepstrike.io/blog/sim-swap-scam-statistics-2025)
- [FTX SIM Swap Attack Analysis — Cloud Security Alliance](https://cloudsecurityalliance.org/blog/2025/09/02/a-successful-sim-swap-attack-unpacking-the-2022-ftx-hack)
- [Passwordless Adoption Trends 2025 — Help Net Security](https://www.helpnetsecurity.com/2025/10/31/passkey-adoption-trends-2025/)
- [AUSTRAC — AML/CTF Act](https://www.austrac.gov.au/business/legislation/amlctf-act)
- [AUSTRAC — AML/CTF Program Guide for Digital Currency Exchanges](https://www.austrac.gov.au/business/how-comply-guidance-and-resources/guidance-resources/guide-preparing-and-implementing-amlctf-program-your-digital-currency-exchange-business)
- [Corporations Amendment (Digital Assets Framework) Bill 2025](https://www.aph.gov.au/Parliamentary_Business/Bills_Legislation/bd/bd2526/26bd040)
- [ASIC Regulatory Guide 133 — Crypto Custody (KWM Analysis)](https://www.kwm.com/au/en/insights/latest-thinking/crypto-custody-asic-expands-its-regulatory-guidance-under-rg-133.html)
- [Gilbert + Tobin — Australia Reshapes Digital Asset Regulation](https://www.gtlaw.com.au/insights/key-topics/regulation-in-motion/australia-reshapes-digital-asset-regulation)

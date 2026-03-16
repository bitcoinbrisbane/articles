# Rebuttal: You Can't Automate Your Way Out of a Compliance Problem

*A response to ["Automating Your Way Out of Manual Crypto Accounting"](https://blog.electrafrost.com/p/automating-your-way-out-of-manual)*

I read Electra Frost's recent piece on automating crypto accounting with interest. She's right that Excel spreadsheets crash, that manual processes don't scale, and that crypto tax software wasn't built for business accounting. But the article glosses over something far more important than efficiency: **the law doesn't care how fast your subledger runs if it doesn't meet the statutory requirements.**

Automation is a tool. Compliance is a legal obligation. They are not the same thing, and conflating them is dangerous advice.

## 1. The Corporations Act Doesn't Say "Use a Subledger"

Under [Section 286 of the Corporations Act 2001](https://classic.austlii.edu.au/au/legis/cth/consol_act/ca2001172/s286.html), every Australian company must keep written financial records that:

> "correctly record and explain its transactions and financial position and performance, and would enable true and fair financial statements to be prepared and audited."

Records must be [retained for seven years](https://www.ato.gov.au/law/view/print?DocID=PAC/20010050/286&PiT=99991231235958) after the transactions they cover are completed. Failure to comply is a [strict liability offence](https://sprintlaw.com.au/articles/s286-corporations-act-financial-record-keeping-requirements/). Directors who fail to take all reasonable steps to ensure compliance face civil penalties, compensation orders, and potential disqualification under [Section 344(1)](https://sprintlaw.com.au/articles/s286-corporations-act-financial-record-keeping-requirements/).

Worse: under [Section 588E(4)](https://briferrier.com.au/news/flying-the-company-blind-consequences-arising-from-a-failure-to-keep-adequate-books-and-records/), if a company fails to keep or retain adequate financial records, it is **presumed to have been insolvent** for the entire period of non-compliance. That's not a fine — that's a presumption that can trigger personal liability for every director.

The original article recommends crypto subledger tools that integrate with Xero and QuickBooks. That's fine for operational convenience. But the question isn't whether your subledger syncs with Xero — it's whether your records, as a whole, meet the "true and fair" standard and can survive an audit. A third-party SaaS tool with opaque pricing algorithms and a "black box" cost basis engine is not, by itself, a compliance strategy.

## 2. IFRS Doesn't Have a Crypto Standard — And That's a Problem Automation Can't Solve

The article mentions the need for specialised accounting treatment but doesn't address the fundamental gap: **there is no IFRS standard specifically for digital assets.**

The [IFRS Interpretations Committee ruled in June 2019](https://www.ifrs.org/content/dam/ifrs/supporting-implementation/agenda-decisions/2019/holdings-of-cryptocurrencies-june-2019.pdf) that cryptocurrencies held for sale in the ordinary course of business fall under [IAS 2 (Inventories)](https://www.ifrs.org/issued-standards/list-of-standards/ias-38-intangible-assets/), and all others fall under [IAS 38 (Intangible Assets)](https://www.ifrs.org/content/dam/ifrs/publications/pdf-standards/english/2021/issued/part-a/ias-38-intangible-assets.pdf). The [AASB has acknowledged](https://www.aasb.gov.au/admin/file/content102/c3/aasb_asaf_digitalcurrency.pdf) this is inadequate and has added digital assets to its work plan.

Here's why this matters for the automation argument:

- Under the **IAS 38 cost model**, crypto assets are carried at cost less impairment. You can write them down but never up. Your Bitcoin could go from $30,000 to $100,000 and your books would still show $30,000 (or whatever impaired value you wrote it down to). A subledger that automatically marks to market is producing numbers that **don't comply with the cost model**.

- The **IAS 38 revaluation model** only applies where fair value can be determined by reference to an [active market](https://www.pkf-l.com/insights/depth-guide-to-ias-38-intangible-assets-under-ifrs-listed-businesses/). While major cryptocurrencies trade on active markets, the standard [prohibits indirect valuation techniques](https://www.ipohub.org/article/crypto-assets-and-fair-value-measurement) — you can't use financial models with less observable inputs. For DeFi positions, LP tokens, governance tokens, or illiquid NFTs, the revaluation model may not be available at all.

- [Impairment testing under IAS 36](https://www.ey.com/content/dam/ey-unified-site/ey-com/en-gl/technical/financial-services/documents/ey-ifrs-accounting-for-crypto-assets.pdf) requires management judgement on recoverable amounts. This is inherently a human decision, not an automatable one.

No subledger tool resolves these classification and measurement questions. They require professional judgement, and getting them wrong exposes directors to the penalties outlined above.

## 3. The ATO Has Been Clear Since 2014 — Automation Doesn't Change the Rules

The ATO's position on crypto taxation has been settled law for over a decade. [Taxation Determination TD 2014/26](https://www.ato.gov.au/law/view/view.htm?DocID=TXD/TD201426/NAT/ATO/00001) established that Bitcoin is a CGT asset under [subsection 108-5(1) of the Income Tax Assessment Act 1997](https://www.ato.gov.au/law/view/view.htm?DocID=TXD/TD201426/NAT/ATO/00001). Disposal triggers [CGT event A1 under subsection 104-10(1)](https://www.ato.gov.au/law/view/view.htm?DocID=TXD/TD201426/NAT/ATO/00001). Where tokens are acquired with the purpose of profit on commercial disposal, gains are assessable as ordinary income under [Section 6-5 ITAA 1997](https://www8.austlii.edu.au/cgi-bin/sinodisp/au/other/rulings/ato/ATOTD/2014/TD201426.html).

The related [TD 2014/25](https://www.ato.gov.au/law/view/view.htm?DocID=TXD/TD201425/NAT/ATO/00001) confirmed that Bitcoin is **not** foreign currency for the purposes of Division 775 ITAA 1997. This distinction matters enormously for businesses — you cannot use FOREX accounting methods for crypto, despite what the original article's reference to "FOREX accounting approaches" might imply.

A subledger tool that automatically calculates cost basis using FIFO, LIFO, or specific identification doesn't determine which method is correct for your circumstances. The ATO doesn't prescribe a method — but it does expect consistency, and it expects the chosen method to reflect the taxpayer's actual intentions. That's a professional judgement call, not a software configuration.

## 4. AML/CTF Obligations Require Human Oversight, Not Just Automation

The [Anti-Money Laundering and Counter-Terrorism Financing Act 2006](https://www.austrac.gov.au/business/legislation/amlctf-act) imposes reporting obligations on digital currency exchange providers registered with [AUSTRAC](https://www.austrac.gov.au/business/core-guidance). These include:

- **Suspicious Matter Reports (SMRs):** Must be submitted within [24 hours for terrorism financing suspicions or 3 business days for money laundering](https://amlhouse.com.au/insights/aml-ctf-austrac-reporting-guide/). Failure to report is a criminal offence.
- **Threshold Transaction Reports (TTRs):** Mandatory for cash transactions of [A$10,000 or more](https://amlhouse.com.au/insights/aml-ctf-austrac-reporting-guide/), with a 10-business-day deadline.
- **AML/CTF programs:** Every reporting entity must maintain [a documented program](https://www.austrac.gov.au/business/how-comply-guidance-and-resources/guidance-resources/guide-preparing-and-implementing-amlctf-program-your-digital-currency-exchange-business) covering customer identification, ongoing due diligence, and transaction monitoring.

From [1 July 2026, Tranche 2 reforms](https://www.austrac.gov.au/amlctf-reform) will bring additional services and entities under regulation, significantly expanding the scope of who must comply.

A subledger tracks numbers. It doesn't file SMRs. It doesn't make the human judgement call about whether a transaction pattern is "suspicious." And it doesn't maintain your AML/CTF program. Automating transaction recording while leaving AML obligations unaddressed is like locking the front door while leaving the back wide open.

## 5. The New Digital Assets Framework Changes Everything

The original article references Australia's anticipated regulatory framework. That framework is no longer anticipated — it's here.

The [Corporations Amendment (Digital Assets Framework) Bill 2025](https://www.aph.gov.au/Parliamentary_Business/Bills_Legislation/bd/bd2526/26bd040), introduced to Parliament by Assistant Treasurer Daniel Mulino, amends the Corporations Act 2001 and the ASIC Act 2001 to bring digital asset platforms under financial services law. The Bill:

- Defines ['digital token' and 'digital asset platform'](https://classic.austlii.edu.au/au/legis/cth/bill/caafb2025495/) as new regulated concepts
- Requires operators to hold an [Australian Financial Services Licence (AFSL)](https://www.allens.com.au/insights-news/insights/2025/10/government-consults-on-expanding-australian-financial-services-law-to-digital-assets/)
- Mandates that platforms [act efficiently, honestly and fairly](https://www.gtlaw.com.au/insights/key-topics/regulation-in-motion/australia-reshapes-digital-asset-regulation)
- Requires platforms to provide a [platform guide](https://www.globalcompliancenews.com/2025/10/02/https-insightplus-bakermckenzie-com-bm-banking-finance_1-australia-australian-government-moves-to-regulate-digital-asset-and-tokenised-custody-platforms_09262025/) explaining custody arrangements, fees, risks, and client rights
- Gives ASIC [standard-making power](https://www.regulationtomorrow.com/au/corporations-amendment-digital-assets-framework-bill-2025/) over asset safeguarding

Meanwhile, [ASIC's updated Regulatory Guide 133 (RG 133)](https://www.asic.gov.au/regulatory-resources/find-a-document/regulatory-guides/rg-133-funds-management-and-custodial-services-holding-assets/) now explicitly addresses crypto-asset custody, requiring [segregation of client assets, trust arrangements, auditable records, and independent audits](https://www.kwm.com/au/en/insights/latest-thinking/crypto-custody-asic-expands-its-regulatory-guidance-under-rg-133.html).

The article's discussion of "FBO accounting" is relevant here — but the compliance burden goes far beyond what any subledger tool currently handles. You need legal structure, custody arrangements, disclosure documents, dispute resolution processes, and governance frameworks. These are **legal and operational** requirements, not accounting ones.

## 6. The Real Risk: False Confidence

My concern with the original article isn't that it recommends automation — automation is sensible. My concern is that it frames automation as a solution to a compliance problem, when it's really only a solution to an efficiency problem.

The businesses most at risk aren't the ones using Excel. They're the ones who plugged in a subledger, saw clean numbers in Xero, and assumed they were compliant — without:

- Confirming their IFRS classification and measurement approach with a qualified accountant
- Ensuring their cost basis methodology is defensible under ATO guidance
- Meeting their AML/CTF reporting obligations under the AML/CTF Act 2006
- Preparing for AFSL obligations under the Digital Assets Framework Bill 2025
- Ensuring their records meet the "true and fair" standard of Section 286
- Understanding their custody obligations under ASIC RG 133

**Automation without professional oversight isn't a compliance strategy. It's a faster way to be wrong.**

The original article concludes that "engaging an experienced crypto accountant" can help. On that, we agree completely. But that sentence should be the headline, not the footnote. The accountant isn't there to help you set up the software — they're there to ensure your entire framework, from classification to custody to reporting, meets the legal obligations that no software tool can satisfy on its own.

---

*Do you agree that automation solves efficiency but not compliance? I'd welcome the discussion.*

---

**Legislation & Standards Cited:**
- [Corporations Act 2001, Section 286 — Obligation to Keep Financial Records](https://classic.austlii.edu.au/au/legis/cth/consol_act/ca2001172/s286.html)
- [Corporations Act 2001, Section 344(1) — Directors' Duties](https://sprintlaw.com.au/articles/s286-corporations-act-financial-record-keeping-requirements/)
- [Corporations Act 2001, Section 588E(4) — Presumption of Insolvency](https://briferrier.com.au/news/flying-the-company-blind-consequences-arising-from-a-failure-to-keep-adequate-books-and-records/)
- [Corporations Amendment (Digital Assets Framework) Bill 2025](https://www.aph.gov.au/Parliamentary_Business/Bills_Legislation/bd/bd2526/26bd040)
- [Income Tax Assessment Act 1997, Subsection 108-5(1) — CGT Assets](https://www.ato.gov.au/law/view/view.htm?DocID=TXD/TD201426/NAT/ATO/00001)
- [ATO Taxation Determination TD 2014/25 — Bitcoin Not Foreign Currency](https://www.ato.gov.au/law/view/view.htm?DocID=TXD/TD201425/NAT/ATO/00001)
- [ATO Taxation Determination TD 2014/26 — Bitcoin as CGT Asset](https://www.ato.gov.au/law/view/view.htm?DocID=TXD/TD201426/NAT/ATO/00001)
- [Anti-Money Laundering and Counter-Terrorism Financing Act 2006](https://www.austrac.gov.au/business/legislation/amlctf-act)
- [AUSTRAC — AML/CTF Reform (Tranche 2, effective 1 July 2026)](https://www.austrac.gov.au/amlctf-reform)
- [AUSTRAC — AML/CTF Program Guide for Digital Currency Exchanges](https://www.austrac.gov.au/business/how-comply-guidance-and-resources/guidance-resources/guide-preparing-and-implementing-amlctf-program-your-digital-currency-exchange-business)
- [IAS 38 — Intangible Assets](https://www.ifrs.org/issued-standards/list-of-standards/ias-38-intangible-assets/)
- [IAS 2 — Inventories (Applied to Crypto Held for Sale)](https://www.ifrs.org/content/dam/ifrs/supporting-implementation/agenda-decisions/2019/holdings-of-cryptocurrencies-june-2019.pdf)
- [IFRS Interpretations Committee — Holdings of Cryptocurrencies (June 2019)](https://www.ifrs.org/content/dam/ifrs/supporting-implementation/agenda-decisions/2019/holdings-of-cryptocurrencies-june-2019.pdf)
- [AASB — Digital Currency Case for Standard Setting](https://www.aasb.gov.au/admin/file/content102/c3/aasb_asaf_digitalcurrency.pdf)
- [ASIC Regulatory Guide 133 — Funds Management and Custodial Services](https://www.asic.gov.au/regulatory-resources/find-a-document/regulatory-guides/rg-133-funds-management-and-custodial-services-holding-assets/)
- [ASIC RG 133 — Crypto Custody Guidance (KWM Analysis)](https://www.kwm.com/au/en/insights/latest-thinking/crypto-custody-asic-expands-its-regulatory-guidance-under-rg-133.html)

**Additional References:**
- [Original Article: "Automating Your Way Out of Manual Crypto Accounting" — Electra Frost](https://blog.electrafrost.com/p/automating-your-way-out-of-manual)
- [Treasury Ministers — New Digital Asset Laws](https://ministers.treasury.gov.au/ministers/daniel-mulino-2025/media-releases/new-digital-asset-laws-unlock-innovation-and-safeguard)
- [Gilbert + Tobin — Australia Reshapes Digital Asset Regulation](https://www.gtlaw.com.au/insights/key-topics/regulation-in-motion/australia-reshapes-digital-asset-regulation)
- [Allens — Government Expands Financial Services Law to Digital Assets](https://www.allens.com.au/insights-news/insights/2025/10/government-consults-on-expanding-australian-financial-services-law-to-digital-assets/)

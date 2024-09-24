# Rucknium

_**A Statistical Research Agenda for Monero**_

[https://youtu.be/gtRUKTH-jIY](https://youtu.be/gtRUKTH-jIY)

---

_**Rucknium:**_ I'm Rucknium, I'm a researcher with the Monero Research Lab and an empirical microeconomist. Today I will present a statistical research agenda for Monero.

I apologize for needing to use this synthesized voice. It is to protect my privacy. The slides of this presentation are posted on my GitHub account — github.com/Rucknium/presentations.

I've applied my skills to help improve Monero. I found a way to speed up Monero transaction confirmations by 60 seconds. I computed the privacy impact of P2Pool decentralized mining payouts. The privacy impact was significant. sech1 and duggavo figured out a way to double payout efficiency, which reduced the privacy impact of P2Pool. I analyzed the privacy effects of Mordinals, a Monero NFT protocol. I helped to write the "Fingerprinting a Flood" analysis of the 2021 transaction volume anomaly. I am in the final stages of development for Optimal Static Parametric Estimation of Arbitrary Distributions (OSPEAD) for an improved decoy selection algorithm. I serve on the MAGIC Monero Fund Committee, which funds open source Monero research and development. I do some upkeep of Monero research community resources.

Quiz. Equations written by Satoshi Nakamoto and the original Bitcoin white paper involved: a concept from cryptography, a concept from probability, both or neither. From the topic of this presentation you may be able to guess that the answer is B — a concept from probability. Satoshi used probability theory to calculate the chances of an attacker rewriting the Bitcoin blockchain for a double spend attack.

Cryptography is obviously a crucial part of cryptocurrency, but probability and statistics have been here from the beginning. Monero needs a robust statistical research capability to rise to the privacy, scaling and security challenges of today and tomorrow.

We have two broad categories of Monero statistical research questions. The first involves ring signatures. The second does not involve ring signatures. Ring signatures are a big part of statistical research in Monero since statistical guessing is a threat to the privacy provided by ring signatures. Ring signatures have been called the weakest part of Monero. I hope that one day Monero implements a better solution. However, I expect that ring signatures will be with Monero for many more years.

New ringless protocols are too risky at the moment. There is a lot of enthusiasm for global or full blockchain membership proofs. With ring signatures, the truly spent transaction output is one of a small subset of the total set of outputs. With global membership proofs, whenever an output is spent, it could be any of the millions of transaction outputs on the blockchain. Zcash has had this model in their shielded transaction pool since 2016.

Global membership proofs sound fantastic but they are not without problems. First, Zcash's zk-SNARK model has had high CPU and RAM requirements for transaction construction that prevented low-performance devices from being used as wallets. Second, Zcash users had to trust that the random data generated at Zcash's genesis was not improperly controlled or observed by a malicious party. Third, Zcash's cryptographic mathematics was based on new foundations that were not battle-tested. Just a note: Monero uses Bulletproofs which are a type of zero knowledge proof. Bulletproofs are not a zk-SNARK because they lack the succinctness property, which is the S in the acronym.

Zcash fixed some of its problems. The performance requirements were reduced with 2018 Sapling upgrade. The Orchid/Halo 2 upgrade in 2020 to provide a zk-SNARK with no trusted setup. But the protocol might not be safe enough to use since the cryptography is still not battle-tested.

Many "close calls" have threatened private digital cash protocols. Often problems with the cryptography mathematics itself, not the computer code that implements it. Garbage in, garbage out.

Case study 1. Zcash counterfeiting flaw. I will quote a long section of the announcement to show how much scrutiny the mathematics received but yet the flaw remained undetected for four years.

"On March 1, 2018, Ariel Gabizon, a cryptographer employed by the Zcash company at the time, discovered a subtle cryptographic flaw in the 2014 paper that describes the zk-SNARK construction used in the original launch of Zcash. The flaw allows an attacker to create counterfeit shielded value in any system that depends on parameters which are generated as described by the paper. This vulnerability is so subtle that it evaded years of analysis by expert cryptographers focused on zero-knowledge proving systems and zk-SNARKs. In an analysis in 2015 Brian Parno from Microsoft Research discovered a different mistake in the paper. However, the vulnerability we discovered appears to have evaded his analysis. The vulnerability also appears in the subversion zero-knowledge SNARK scheme of Fuchbauer 2017 where an adaptation of the Ben-Sasson and Chiesa, Tromer and Virza 2014 paper inherits the flaw. The vulnerability also appears in the ADSNARK construction described in that paper. The vulnerability evaded the Zcash company's own cryptography team, which includes experts in the field that had identified several flaws in other parts of the system. Importantly, the Ben-Sasson and Chiesa, Tromer and Virza 2014 papers construction did not have a dedicated security proof as noted in Parno's 2015 paper and relied mainly on the security proof from the Parno, Howell, Gentry and Raykova 2013 paper and the similarity between the two schemes. The Zcash Company team did attempt to write a security proof in the Bowe, Gabizon and Green 2017 paper but it did not uncover this vulnerability."

Case study 2. CryptoNote (Monero) counterfeiting flaw. Like the Zcash flaw, this one laid silent for years. The CryptoNote 2.0 paper was released in 2013. A Monero cryptographer figured out the flaw in 2017. I will quote the announcement.

"In Monero we've discovered and patched a critical bug that affects all CryptoNote-based cryptocurrencies and allows for the creation of an unlimited number of coins in a way that is undetectable to an observer unless they know about the fatal flowing concern for it."

"We patched it quite some time ago and confirmed that the Monero blockchain had never been exploited using this, but until the hard fork that we had a few weeks ago we were unsure as to whether or not the entire network could be updated."

A big difference between the Monero flaw and the Zcash flaw is that the counterfeiting from the CryptoNote flaw would be detectable after the fact. The form of the Zcash flaw did not provide a way to determine whether the flaw had been exploited by a malicious party.

Case study 3. Secret Network privacy exposure. Secret Network is a smart contract blockchain that promises privacy for users. It uses Intel Software Guard Extension, a type of hardware Trusted Execution Environment to protect user privacy. Last year, researchers found flaws in SGX that could reveal private information completely.

"The Secret Network has been vulnerable to xAPIC and MMIO vulnerabilities that were publicly disclosed on August 9, 2022. These vulnerabilities could be used to extract the consensus seed, a master decryption key for the private transactions on the Secret Network. Exposure of the consensus seed would enable the complete retroactive disclosure of all Secret-4 private transactions since the chain began. We have helped Secret Network to deploy mitigations, especially the Registration Freeze on October 5, 2022."

"However, there is no way to know for certain whether this attack has been attempted previously. It is also possible that ordinary node operators may have unintentionally prepared the attack, if they were active nodes prior to the mitigations, and may opportunistically decide to complete it in the future. We urge privacy-conscious users to re-evaluate their risk considering that their past transactions may be exposed."

Out of ethical concern, the researchers erased the master dencryption key that they had extracted. But we cannot guarantee that a malicious actor had not already performed the extraction.

Actually exploited flaws in private digital cash protocols. In 2017, Bytecoin, a CryptoNote-based coin, was exploited by the counterfeiting flaw that Monero discovered and patched. In 2017 a malicious party exploited a counterfeiting bug in the code of Firo. In 2021, a counterfeiting exploit occurred against Haven due to several flaws.

How confident do we need to be? In the last few years, cryptocurrency protocols have been exploited for over 5 billion US dollars. Flawed code, flawed safeguards, flawed cryptography, flawed economics. How confident should Monero be in new cryptographic foundations before they are irrevocably activated on mainnet? 95% confident? 99% confident? 99.9% confident? How confident were the researchers and developers of these exploited protocols?

Peer review, audits, and battle-testing. My definitions of these terms are a combination of the commonly accepted meaning and my own classification. I'm an economist, so I think about the incentives.

Academic peer review. Fellow cryptographers check the mathematics of the security proofs in new papers. The incentives are professional responsibility to science. Build and maintain reputation.

Code audits. An independent party checks that the code correctly implements the paper's math and does not contain exploitable holes. Build and maintain reputation are the incentives of the audit companies.

Battle-testing. Every basement hacker and government agent declares open season on the protocol. The incentives are wealth of pursuit of government objectives.

Trustless zk-SNARKs are not yet battle-tested. Just a few years old. Many protocols are not properly peer reviewed. As far as I know, there is no explicit financial incentive to find flaws. Zcash has no "bug bounty" program. Monero has a 1,061 XMR bug bounty program. Granted, the "bounty" for counterfeiting flaws is self-executing. But not for privacy flaws. Cryptocurrency users are encouraged to verify not trust.

_[Ad]_

_Do you love coffee and Monero as much as we do? Consider making gratuitas.org your daily cup. Pay with Monero for premium fresh beans, and if you like what you taste, send a digital cash tip directly to the Guatemalan farmers that made it possible. Proceeds help us grow this channel. Gratuitas and Monero._

_**Rucknium:**_

Cryptocurrency users are encouraged to verify, not trust the blockchain data and its cryptography. It's an ideal that is rarely achieved in practice. The Monero Research Lab has few researchers now who can write and/or evaluate mathematics security proofs of new cryptography. Mathematics at this level is extremely difficult. Understanding how something works is not the same level of knowledge to be able to discover flaws. How many engineers and scientists use calculus? How many of them can write the calculus mathematics proofs and recognize flawed proofs? Probably not many. We could be in a position where Monero users, researchers and programmers cannot verify the correctness of the cryptography Monero uses.

"The Monero Core Team and the Monero Research Lab would like to follow the development philosophy that it is wise to start with smaller changes at first and then ramp those changes up over time, rather than start with drastic changes and try to scale them back."

Monero cannot afford to be experimental. More people may rely on Monero to protect their privacy than any other private digital cash protocol. There is a moral responsibility. Monero has no non-private transparent pool like many other protocols. Coins like Zcash and Firo can quarantine possible counterfeiting exploits by their "turnstile" rules that prohibit more coins exiting the private pool than have entered.

In my opinion, Monero's privacy will be in a very good position once Seraphis-level 128+ ring size and improved mimicking decoy selection algorithm like OSPEAD are implemented. The main attack that will remain is the EAE attack. Egger and other researchers in the paper on defeating graph analysis of anonymous transactions argued that the ring size as low as 24 would protect Monero users from a specific type of deanonymizing attack or chain reactions. I discuss skepticism of global membership proofs because I want to argue that it is still worth researching improvements to the ring signature model while we wait years for the new cartography to be battle-tested.

Let's get into the research questions. How do we defeat the attacks known as Poisoned Outputs, EAE and Overseer? "We model examples where two colluding parties E attempt to learn information about an individual by sending outputs to individuals and tracing their transaction graphs. These colliding parties may perform powerful statistical tests to learn significant information, especially for repeated transactions where they would not normally occur by chance."

Research questions. How to formally describe the EAE attack? What is the average false positive and false negative rate of the attack? Is churning a defense against the EAE attack? For best privacy, what should the wait time between churns be? Should it match the decoy selection algorithm? When, if ever, should outputs be combined while churning? Knack, Surae, and Sarang made some attempts at EAE research.

Ring member binning, the current method. Randomly choose each decoy independently. Randomly choose locations for a few bins. Then select decoys within those bins. The goal of binning is to provide a second layer of defense if the adversary can guess the age of the real spend. The current version of the Seraphis code implements binning.

Research questions. What are the costs and benefits of binning? Does binning improve privacy for some threat models, but worsen privacy for others?

Improved mimicking decoy selection. Many papers argue that of matching the real spend age distribution as closely as possible is very important to protect user privacy.

Research questions. How to estimate the real spend age distribution of Monero users? How to "fit the curve" to translate the estimate into a decoy selection algorithm? Dynamic, adjust over time, or static, remain the same over time? Parametric, simple mathematical formula, or non-parametric, free form? Optimal static parametric estimation of arbitrary distributions always speeds up — my own project that proposes answers to these questions.

Transaction flooding detection. Research questions. Is there a general method for detecting attempted de-anonymizing flooding episodes? Are there any viable flooding countermeasures, like a decentralized counter-flood?

More ring signature questions. Can changing the decoy selection algorithm between hard forks allow adversaries to fingerprint old/new wallet versions? How should decoys be chosen when a new transaction type is implemented, e.g. Seraphis transaction type? Are there any big downsides to excluding Coinbase outputs from the standard decoy selection algorithm? Coinbase exclusion is a planned change. Can the 10 block lock on spending new outputs be reduced or eliminated? Can wallet level changes like when we just proposed PocketChange negatively affect user privacy?

Statistical research questions beyond ring signature issues. Transaction format strictness. Variations in transaction format are goldmine for cryptocurrency tracing. In Bitcoin-like blockchains, wallet software "fingerprints" can indicate when coins have changed custody. "The design of Bitcoin supports a tremendous variety of possible transaction types that are designed years ago." Satoshi Nakamoto.

Table from Moser and Narayanan in 2022 resurrecting address clustering in Bitcoin.

Monero transaction format: loose aspects change to strict. Could use amount-hiding RingCT or old format with transparent amounts — 2017: all transactions must use RingCT except under special circumstances. User chosen ring size — 2018: all transactions have same ring size. Custom wallet software doesn't follow 10 block lock on spending new transaction outputs. 2019: 10 block lock enforced by blockchain consensus rules. Transactions with a single output allowed, indicating a likely self-spend. 2019: all transactions must have at least two outputs with possible zero amount outputs.

Making Monero's transaction format more strict: custom lock time. Users can prevent their transaction outputs from being spent for a custom amount of time by setting unlock time. Downsides. code complexity, transaction fingerprinting, risk to merchants who don't check unlock time when receiving payments. Outsides: not many. Research question: can we discover any useful applications with custom lock time? The current version of Seraphis could remove the option to set a custom unlock time.

Fee discretization. The standard Monero wallet software has 4 fee levels. But other wallet software is free to set fees, which could accidentally fingerprint the wallet software. This actually happened with MyMonero. Now fixed. Problem can be worse in Monero since it has 12 digits of precision after the decimal point. Bitcoin has 8. Current Seraphis code: blockchain consensus rules would require fees that are a power of 1.5, rounded to one significant digit.

Research questions. What form of discretization is best? Any potential problems with exponents? Would fee discretization create fee bubbles because users cannot select intermediate fees? How should wallet software estimate the fee needed to be included in the blockchain at high transaction volumes?

Prohibit arbitrary data in transactions. Using tx_extra, the Mordinals protocol created "NFTs". Eliminating tx_extra does not prevent arbitrary data in transactions. Data can be injected into the parts of transactions intended for cryptographic elements, for example, public keys.

Research questions. Can a statistical test detect and exclude arbitrary data with acceptable tradeoffs? Low power candidates: Shannon's entropy, Chi-square test. High power candidates: binary matrix rank, book stack, birthday spacings. Can cryptography be used to enforce encryption when the keys are unknown? Probably not.

Making Monero's transaction format more strict. Require standard decoy selection algorithm. Non-standard wallet software can choose decoys for ring signatures any way its developers want. Fingerprinting issue that will get worse as ring size increases. More data, more ring members, means greater ability to statistically distinguish them. We know that there are multiple non-standard decoy selection algorithms being used in the wild on the Monero blockchain. Includes "cached" ring members that directly reveal the real spend.

Research questions. Should a standard decoy selection algorithm be required? What are the downsides? Should the requirement be a blockchain consensus rule or just a node transaction relay rule? For example, minimum fee is relay, not consensus. New restriction on tx_extra size is relay rule. If standardized, how often could the decoy selection algorithm be updated? Could you have an automatically-updating algorithm based on changing aggregate user behavior? Record the algorithm in the block headers to "conduct the orchestra"? Could auto-updating allow adversaries to manipulate it?

Restrict number of inputs and outputs per transaction. Right now between 2 and 16 outputs per transaction are allowed. Number of inputs unlimited, but transaction must fit in a single Monero block. On the Seraphis transactions are proposed to have between 2 and 16 outputs. 1 to 112 inputs. The number of inputs and outputs may reveal some information about the user. A transaction with many inputs could be a merchant consolidating payments. A transaction with many outputs could be an exchange or mining pool. Many inputs can reveal information about one user owning several outputs, which gives more information than just single output ownership.

There's an extreme proposal to deal with the transaction on uniformity issue. We require all transactions to have only 2 inputs and 2 outputs. Could be major annoyance for entities like merchants and exchanges that usually use many inputs and outputs, especially with the 10 block lock.

Research questions. How common are transactions with 3 or greater inputs and outputs in Monero and transparent blockchains? What is the privacy benefit of requiring 2 inputs and 2 outputs? How much of an inconvenience would requiring 2 inputs and 2 outputs be?

Dynamic block size and fee policy. Monero's dynamic block size has not received the same amount of research scrutiny as its privacy features. There are some graphs created by spackle-xmr that simulate the effect of larger transaction volumes and fees, and the Monero block size.

Research questions. Can the dynamic block size parameters result in undesirable outcomes, for example too fast or too slow block size increase? The interaction of block size and fee policy is supposed to adjust to the purchasing power of a unit of XMR in the future — a type of oracle problem. Can this go wrong? Is it possible to have a fee policy that discourages adversarial spam but provides low fees for people around the globe?

The dynamic block size rules assume miners will choose to raise the block size when there is "fee pressure" to maximise their profits.

Research questions. Is raising block size the economically rational choice for miners? Are miners fully rational, or do they have bounded rationality? I discovered that mining pools were "leaving fees on the table" until I informed them of their configuration problem. The finding supports the hypothesis of bounded rationality, meaning not fully rational with perfect information.

Reducing mining pool centralization. Research question. Could a dynamic mining pool fee be developed that would encourage miners to join smaller pools, or a P2Pool, when a mining pool has a large percentage of total hash rate? The MineXMR pool raised fees when it gained a large share of hard rate in 2022. The dynamic fee would require voluntary adoption by mining pool operators.

Surprise unforeseen research questions. Some of the best statistical research questions appear when you happen to see something unexpected in the data. When I discovered the transaction confirmations could be sped up by 60 seconds, I was working on researching for discretization, a completely different topic.

How to get to work on the statistical research agenda? Active Monero Research Labs statistical researchers: me (Rucknium), ACK-J, isthmus and neptune. We need more. I wrote some thoughts on how Monero could recruit more researchers in a Reddit post. The MAGIC Monero Fund put requests for research proposals and research grant databases. We may have a new research project on EAE attacks soon. Recruitment is hard.

Questions and feedback, please.

# Koe

_**Seraphis Development: A Year in Review (2023)**_

[https://youtu.be/vXqy0WFbdQc](https://youtu.be/vXqy0WFbdQc)

---

_**Doug:**_ Ok, guys, next up is koe. He is giving an update on Seraphis development. koe is the developer, the main developer of Seraphis, and he’s going to go into detail on what Seraphis is all about. It’s basically, I would say, is this going to be the largest kind of upgrade to Monero’s history? Is it fair to say that?

_**koe:**_ Yeah.

_**Doug:**_ All right, so go take it away, tell us all about.

_**koe:**_ All right. Today I’m just planning to, I’m not going to discuss how Seraphis works, I’m going to focus on highlighting what has taken place in the last year since Monerokon 2022.

Ok, so here’s just a brief recap of what’s going on with Seraphis. It’s a new transaction protocol that offers… the fundamental design supports better membership proofs, and it also enables transaction chaining, where you have a transaction can spend funds in a transaction that has not yet been added to the ledger. So this has benefits for multisig and atomic swaps.

And then we also have Jamtis to go along with Seraphis. Jamtis is a new address scheme that evolves beyond what CryptoNote gave us, offering a full balance recovery with a view wallet, as Justin discussed earlier today, and then also it is designed to enable privacy optimized remote-assisted balance recovery. So a light wallet that owns some of your views, some of view key material can speed up scanning of your local device without compromising a lot of information about your balance.

We also with Jamtis achieved random address generation. So the subaddresses that were added to CryptoNote were not designed in a way that allows you to recover the address index when you’re scanning the blockchain. So when you scan the blockchain and you discover an enote that you might own, you have to match that against a pre-computed table of subaddresses that might own enotes, that might own your enotes. So you have to know that table in advance. So if enote is sent to a randomly generated subaddress, then how is that subaddress is going to end up in your pre-computed table? For example, if you’re recovering your balance from scratch, so say, you discard all your all your records. But Jamtis fixes this simply by encrypting the index and sticking it in the enote.

We also solve some relatively minor problems. We mitigate the Janus attack, we fix the ‘burning bug’ which plays a multisig and other similar protocols, and we able to make index proofs which I’ll discuss more later about. So you can prove what index your address was constructed from. And then we also figured out how to provide forward secrecy against a DLP solver, Discrete Log Problem Solver, which means if a quantum computer becomes able to analyze the contents of the blockchain, then enotes constructed with Jamtis, a lot of the information in those enotes will be remain hidden from the quantum computer. But it’s conditional, there’s some requirements. So if you give some information to the quantum computer then it can unlock your whole balance. So that’s conditional forward secrecy.

All right. So in the last year we made some improvements to Jamtis relative to what I discussed a year ago.

So we in the process of developing a new protocol. I often felt like through the past year and a half that I solved the whole thing and there was nothing left to iterate on. But over time I keep finding or keep encountering new ideas and new problems that can be solved. And then I reached another point where I feel like: “Ok, it’s all figured out now”. So each of these things is one iterative step that kind of showed up in the past year.

So the ‘burning bug’ was highlighted by Luke as a problem that really should be solved, if we can solve it. And we figured out that… so the burning bug is this situation where you can create multiple enotes and put them in the blockchain via transactions. And each of those enotes can be identical. which means you can only spend one of them. But during balance recovery you’re going to find all of the enotes that are duplicates. So what can happen is if your wallet needs to keep track of all the duplicates and make sure that you only think you have as much funds as one of the duplicates, so if the duplicate with the highest amount, you can say, is the one you want to spend and you want to ignore all the other ones. Otherwise someone could send you a small amount with a duplicate enote address, and then if you spend that small amount, then all other enotes will be unspendable including the one with the highest amount which you really do want to spend. So it’s kind of an attack vector where your funds can be burnt, if you’re not careful enough.

And this is especially problematic for multisig protocols, because when a multisig wallet is transferring funds, you want to make sure that every single transfer moves funds in exactly the way that you expect. So you never want a multisig to create duplicates, because that could break the invariance of whatever protocol your multisig is involved in. And so we fixed the ‘burning bug’ by simply baking a unique piece of information into the enote construction process. So whenever you construct an enote you put some unique information in there, so that when the enote is recovered, you will only successfully recover in enote if you use that unique piece of information and proceed through all the steps successfully. So if two enotes have duplicate addresses, then one of them will never be discoverable, because since we are baking a unique piece of information into the construction process, you essentially have to find a hash collision in order to create two duplicate one-time addresses that can be recovered successfully using the process that includes the unique piece of information. So the unique information we include is simply the key images of the transaction in which the enote will be inserted, or if you have a coinbase transaction it’s the block height of the block where the transaction will be found.

So an important piece of, somewhat underrated piece of a cryptocurrency design is being able to create proofs about the transactions you’ve created and the enotes you’ve received. So that you can forget, for example, prove that you’ve paid a vendor some money and so that you can expect something in return, if there’s a conflict. And so in the process of thinking about these proofs for Seraphis and for Jamtis I realized that one piece of information that might be useful is the index of a Jamtis address. So like a subaddress has an index, a Jamtis address has an index and you own a large set of addresses, and each one has an index. And it might be useful to prove what the index is of a particular address. So we made some modifications to Jamtis to enable making those proofs.

And then finally there’s been a lot of noise back and forth about the whether or not quantum adversaries will become problematic in the future. And so as part of that discussion I realized that it could be valuable to… so one of the problems with quantum adversaries is they can look at the blockchain which is… the security of cryptocurrencies today are as soon with the assumption that the discrete log problem cannot be broken. But a quantum adversary can break the discrete log problem. So what kind of information will be exposed to a quantum adversary, if one appears? And so in the process of thinking about what information in Seraphis and Jamtis can be exposed I realized that there were some modifications that could be made to Jamtis to reduce the amount of information that could be exposed. I don’t think they talk about it more in this presentation, but if you want to know more, then I wrote a companion paper to the Seraphis paper, which discusses how to implement Seraphis. And in that paper is a section on the forward secrecy question.

So aside from features when you’re designing a protocol it’s also important to think about how performant are the changes you’re making. So in Jamtis we’ve made a number of optimizations so that balance recovery is as fast as possible and as efficient as possible. Another thing to consider… So Jamtis is an addressing scheme with multiple private keys. So one question that comes up is, for example, there’s a generate address key which you could give to someone to produce arbitrary addresses for your wallet. So, for example, a point of sale might have a generate address key that’s used to produce new addresses for each new sale. And another key is the find-received key which is used by the light wallet service to scan the chain with minimal information leakage.

But what happens if you combine the generate address and the find-received key? We want to make sure that combining doesn’t produce a new wallet tier that is suddenly more vulnerable than we design into the protocol. So one thing I added in the last year is this something called the ‘unlock-amounts’ which ensures that combining the generate-address key and the find-receive key doesn’t let you unlock the amounts and enotes.

We also specified the design of address tags. So address tags are the encrypted form of the index that’s in your address. So you add this address tag your address, so the address tag is an encrypted form of the index, and then the address tag itself gets encrypted again and inserted into the enote. During balance recovery you decrypt the address tag and then decipher it again to get the index out. And then you use that index for the rest of the balance recovery process. And so we solidified that design to be a 16 byte index with an additional 2 byte decipher hint. The hint is used as an optimization technique. It’s kind of like a filter between steps in the balance recovery process. So you compute this hint with a very cheap hash, and then if the hint isn’t reproduced, then you can abort the balance recovery process and skip later steps in the process, which are more expensive. We chose 16 bytes because it’s a standardized number of bytes for a robust random generation of indices without a risk of collisions.

So I’ll kind of skip over the 2-out transactions point, because it’s kind of a minutia about how… so when we have this optimization, when you have a 2-out transaction, you only need one ephemeral key that can be shared between the two enotes. And in all the cases in Jamtis you need an ephemeral key per enote, that’s output by a transaction. And in order to implement this 2-out optimization in the presence of Jamtis addresses, which don’t have any shared ephemeral base keys, it required some finesse, I guess.

And finally for the elliptic curve Diffie-Hellman exchange, which is the starting point of balance recovery, encouraged us…

_[Disconnect]_

You’ve lost it?... Diffie-Hellman exchange between…

It turns out that using this this different curve which is very, don’t know the right word for, it’s very analogous to Ed25519, leads to a substantial speed up, because this curve is highly optimized for this use case. So we switched curves for this one piece of balance recovery, but it doesn’t affect anything else in transactions, including the addresses - Jamtis addresses themselves are still in Ed25519. And so tevador implemented a library which I’ve been using to get the speed up measurements here. Ok, so now we’ll move on to development in the library.

So I’ve been writing a library for Seraphis, which includes the protocol and then supporting details like balance recovery. To the protocol itself, a few things have been implemented in the past year. So one of them is ‘discretized fees’. In the current as far as I know in all cryptocurrencies transaction fees are highly granular. So they are denominated in the base currency denomination. So your fee is in XMR exactly, so you could have 0.013578 fee or something completely different than that. So basically the number of possible fee values equals the total number of possible amounts that can exist in the currency.

And the problem with this is that when you select your fee, the fee you select is dependent on your current context - what is your priority, what are the network conditions when you select this fee, so blocks being filled out and you have high priority so you need to outbid current transactions. So very high granularity fees leak very precise information about when you selected your fees, and what software you use to select those fees. So the idea of discretized fees is to reduce the total number of possible fee values to a very small number. And then very subtle variations in the context in which you select your fees will be kind of thrown away by the lack of granularity in the fee values.

And then also in the library I implemented a ‘legacy migration’ process that allows you to spend legacy CryptoNote enotes in Seraphis transactions. So the transition process is completely seamless. And I also implemented the Seraphis coinbase transaction type, which supports Serasphis enotes for the outputs.

So in terms of the library itself these are just changes that affect the user experience. I implemented an ‘input selection solver’, which is used for, say, you want to transfer some amount of money, you need to pick inputs from the enotes that you own to spend. So I implemented a solver that picks those for you.

So this middle point is really what took up the most time in the past year - the ‘balance recovery framework’ that allows you to scan the chain, find all the enotes you own, or identify when they are spent, update your enote store with the statuses of all the enotes you own, whether or not they’re on the chain or in the transaction pool, and with the balance recovery framework handle reorgs that could arbitrarily discard blocks that are in the chain. It’s kind of a fundamental and very central piece of what a cryptocurrency does, because the cryptocurrency is simply for transferring money, and finding your money that you own is the only thing that really you care about. So it occupies a huge amount of background work to get that working properly.

I also came up with this idea called ‘block ID checkpoints’. So the current Monero wallet tracks every single block ID for all the transactions or for all the blocks that wallet has scanned. It keeps a record of each block ID so that the wallet can properly handle reorgs in future balance recovery processes. But the problem is there are many, there are millions of blocks in the blockchain. So if you store a block ID for each block, you can end up with many megabytes of just block IDs in your wallet cache, which seriously bloats wallet storage requirements. And so with the collaboration of ghostway I implemented block ID checkpoints, where you just only store a very small number of block IDs in your wallet, which like drastically reduces the size of your wallet.

In the library I implemented several pieces of multisig. So the current multisig implementation is not highly portable. So I implemented a number of improvements for the legacy piece. So since we’re spending legacy enotes in Seraphis transactions, multisig wallets also need to be able to spend legacy enotes in Seraphis transactions. So I added something called ‘key image recovery ceremony’. So legacy key images can only be computed, if you know the spend key of your wallet. But in multisig no single person knows the spend key. So in order to acquire the key images of legacy enotes with a multisig wallet all the group members need to collaborate in order to compute the key images. So right now the key images are recovered with a simple sum of key shares, but that is not robust, so I implemented a robust key image recovery ceremony that is more robust, yes.

I also implemented account transitions. So existing legacy multisig accounts, wallets, need to in the future be Seraphis multisig wallets. So I implemented a transition ceremony that the existing multisig groups can do to get in order to acquire Seraphis wallets that will continue to work as expected.

The current signing framework for multisig is something called ‘Round Robin’. So when you initiate a transaction, you send the partial transaction around group members, until you’ve collected a full signature. But if you have several group members this can take a while for the messages to propagate through the chain of people. So I instead, as an alternative to this, I implemented what I call ‘aggregation-style’ signing, where you simply send out a multisig transaction proposal, and then all the group members send out what are called ‘signature openings’, and then each group member responds to the signature openings of other members, and then that completes the signing procedure. So it’s the end of one round for signing. And so I implemented aggregation-style signing for both legacy and Seraphis proof so that includes CLSAG and Seraphis composition proofs.

One advantage of the library I built is the ability to robustly verify a multisig transaction proposal. So let’s say someone proposes to make a multisig transaction. Are all the enotes, are all the inputs to that transaction valid? Is the transaction proposal structured such that it can conceivably reach a real transaction? So there are no errors within them within the proposal that would cause it to fail at some point in the signing procedure? So as part of the library it’s possible to fully validate multisig transaction proposals which is not possible with the current library. And so just in summary, it’s possible to make Seraphis multisig transactions that spend legacy and Seraphis funds as you’d expect.

Ok, so now we can get into the Seraphis knowledge proofs. This is basically what I call an ‘audit framework’ or a ‘knowledge proof framework’. So these are all the proofs that you can write or construct with Seraphis and Jamtis about the enotes you own and about the transactions you’ve spent. So we’ve got ‘address ownership proof’, we’ve got ‘address index proof’, you’ve got whether or not you own an enote, the amounts in enotes, whether or not a key image was produced from a specific enote, whether an enote is not spent in the blockchain - and this is significant, because it lets you avoid exposing the key image of an enote that you want to prove is unspent.

So what you could do is simply make a key image proof, and then the key image will indicate whether or not the enote is spent. But in the future whoever you gave the image proof  to will be able to use that enote to identify when the enote is spent. So there’s no, it’s like a permanent proof, but an unspent proof occurs for only the transactions that you reference in the proof. So when ring sizes are small, you can make an unspent proof for every transaction that references your enote in an input ring signature, and so if you make an unspent proof about all of those transactions, then the verifier of the proofs will know that the enote that you presumably have proved ownership of is unspent.

You can also make a transaction funded proof, which proves that you own an enote that was spent by a transaction without exposing the enote that was spent or the input amount, which may be useful I guess.

We also have the ‘enote sent proof’ which proves that an enote is owned by an address and has a specific amount. And actually an enote sent proof is just an ownership proof and an amount proof combined together.

So to wrap it up we have a ‘reserved enote proof’ and a ‘reserve proof’. So a reserved enote proof proves that an enote is owned by you and is unspent on chain. And then a reserve proof is a collection of reserve enote proofs to prove you have a certain amount, total amount of funds owned at a specific moment in time when you make the proof. After the proof is made it could become invalid because of the enotes that were referenced in the proof were become spent.

All right, so now we can get on to... That was all the stuff I did in the past year, and now we can talk about what’s to come.

So Seraphis is basically done - the library is at a point it’s fairly stable, there are maybe some minor changes to come related to the transaction extra field, though it’s basically a wallet, or the transaction memo field. But otherwise the library is done. I updated the paper draft, I added an implementing Seraphis paper which is accompanying paper that discusses Jamtis. And other than giving security proofs to those papers and reviewing the code, what we really need next is wallets built using the library, so multisig wallets, normal wallets, hardware wallets, light wallets and so on.

Another thing in the future, something specific to Seraphis, but something that I’m hoping we can incorporate, is a ‘fee estimator’. So in Monero when you select a transaction fee, you take the minimum fee, and then you add some multiplier to it, I think it’s one, two or four, and something like that. But this is very imprecise, so it doesn’t really take into account, it doesn’t facilitate a betting mechanism for transaction space, because you could write an alternative wallet that ignores the fee multipliers, and like one ups the default multiplier or priority system, anyway. So a fee estimator would simply incorporate recent transaction volume to estimate the amount of blocks that would be required, or the estimated confirmation time given a certain fee that you might use. And then you can include your priority, when do you want your transaction to be mined, into that calculus. So it’s kind of shows a piece of technology that should be built.

For Monero research we’ve been discussing transaction extra optimizations. There are a couple options. We have quite a lot of enthusiasm for completely eliminating the field. We also have enthusiasm for changing the field from an arbitrary length, arbitrary content field, to a fixed size field that the wallet would encrypt by default, which would improve the privacy attributes of that field significantly.

From cryptography point of view the range proofs and the membership proofs are always open to optimization. So we’ve been looking at Bulletproofs++, and then there was a new proposal called a ‘Constant Size’ range proof that was proposed that are being looked at to speed up and reduce the size of range proofs. And also there’s ongoing research into zero-knowledge membership proofs that allow you to reference all the enotes on the chain, and then we can stop worrying about ring signatures so much.

So here’s what I plan to work on in the future. So the Seraphis papers need security proofs, and I can’t do that, so I’m going to be looking for assistance on that task.

Since the Seraphis library is at a stable point, I’m planning to support the wallet development, although I probably won’t be a core contributor to that effort.

I also have the design in mind for an efficient escrowed market using 2-of-3 multisig, seamless, which means the steps that a vendor and a buyer need to take are not… all the cryptography and math in the background is invisible to those parties. So they interact in a sequence that makes sense to them without any additional steps in order to send key material back and forth, which might break the user flow, I guess. And so for MoneroKon ’23 I plan to sketch out the protocol which uses a lot of unorthodox optimizations in multisig account setup and signing. And then eventually I’ll build a rough demo in C++ to show how that all fits together. And finally I might someday write a whole write-up about multisig, because there’s been a lot of design iteration in the past years since prior Monero Research Lab publications on that subject.

All right, so that’s the end of what I had to talk about today. The papers can be found on my GitHub. I wrote a design overview, if you want to look at what’s all in the Seraphis proposal more cleanly than this presentation can do. I use bit.ly to hopefully make them capable. And then of course there’s Jamtis, which is also now duplicated in the implementing Seraphis paper which is also at that at that GitHub link. And then another final note - tevador designed some URI schemes for Jamtis, which may be of interest to people involved in a merchant technology, merchant related technology.

Questions?

_**Moderator:**_ Does anyone have any questions for koe?

_**Audience (Andrey):**_ First of all, It’s a full great work on this technology. I have a more general question. Yesterday was a broad topic about influence of developers on decision making process in the blockchain projects, and my question is - there is a different projects and, for example, in some projects developers just simply make decisions, as the project supposed to be decentralized, but the developers have too much power over this, and as opposite in other projects, for example, like Bitcoin itself there is a really hard to find consensus between the big community of developers, and they have a problems like they cannot make decision about some stuff like replaced by a fee feature should be enabled or disabled, so they sometimes have problems to move forward with some technologies. And my question is how do you see the future of Monero, how strong should be impact of the developers on decision making process? What is your vision on this?

_**koe:**_ I think in the long term our goals should be to minimize the impact, or minimize how many decisions can be made, and how often those decisions are made. Simply because in the long run there’s no guarantee that people involved in the project in the future will have the same commitment to ideals as existing project members, or the even if the commitment is there that the decisions will always be good decisions. So I think the fewer decisions that are made, the better, which is kind of why I’ve gone so all out on this project, the Seraphis project. You try to solve as many problems as possible, so in the future fewer decisions have to be made.

I think the decision making itself is a very difficult question, There’s no concrete or small answer that can be made to it. It kind of happens as a it emerges from the people involved in the project, the personalities, and the precedents that have been set, and how prior decisions were made, and the amount of energy that people bring to different discussions. It kind of all melts together. And I think it’s powerful and it’s also dangerous, because it’s so amorphous. That’s what I got to say.

_**Moderator:**_ Any other questions? Come on up.

_**Audience:**_ Hi, koe. It’s Reuben. Nice to meet you finally in the flesh. Just a very general question. I haven’t been really following Seraphis, how it’s been received by the community, obviously I think everyone’s excited about it. But are there any points that people are worried about? Is there any side that’s saying like: “Let’s not go through with Seraphis”, or they have concerns about it, or is that everyone’s like quite aligned in that regard?

_**koe:**_ So I think there are a couple points of concern. So one concern is that moving to a new address scheme is a significant breaking change for the whole ecosystem. And so it really begs the question is it really worthwhile to make such a huge breaking change? Another concern that’s been raised is whether or not Seraphis is the right overall direction that the project should be going in? Maybe by investing so much in Seraphis we could be blind to other paths, which I consider like kind of speculative concern, but it is a concern that we have to keep in mind and not be too fixated on one solution.

I’m not aware of any other specific criticism that have been raised, but I might not be paying enough attention. So if there are criticisms make them loudly heard so that I don’t lose track.

_**Moderator:**_ If there’s any other questions, please come up.

_**Audience:**_ Hi, koe. Nice work, and lots of have been done, so congrats on that. I would like to ask you about the… you mentioned it’s future secrecy?..

_**koe:**_ Forward secrecy?

_**Audience:**_ Yeah, and about the quantum DLP solver. So is it correct that in Seraphis you solve this issue and, if understanded correctly, was it difficult, and can you a little bit override on that? Thank you.

_**koe:**_ It was actually not that difficult. So Pedersen commitments already have this property, because they’re perfectly hiding. So we can just adopt this perfectly hiding principle to the address scheme itself. So that even if you get the discrete log of the components of an address. So an address is composed of three generators, and then there’s three keys attached to each of those generators. And if each of those keys is exposed to the DLP solver, how much information do they get from knowing those pieces of information. And if each of the pieces of information has perfectly hiding component to it, then no information about your wallet in general or maybe the amount in the transaction will be exposed. So the mathematical details really have to look at the paper, but knowing that piece of information would be exposed to the DLP solver, and then making sure that those pieces of information don’t directly expose your private keys, so you’d have like a private key plus a mask, plus another mask and stuff like that.

_**Audience:**_ You talked about potentially moving to global anonymity set after Seraphis. How important do you think that is? Do you see that as a threat as other projects are doing that?

_**koe:**_ I think in the long term it’s a critical advancement that needs to happen. I think that to get there we have to take it step by step and not make overly ambitious moves that we’re not confident are the right choices, because, for example, Zcash was really gung-ho about their global anonymity set, and then they had to start out with a trusted setup. But we don’t want to jump into something that doesn’t align with the project goals and commitments.

_**Audience:**_ I just wanted some clarification on the quantum resistance. It doesn’t apply to the membership proofs, right?

_**koe:**_ As far as I know it does not expose anything about the membership group. The forward secrecy is kind of… I believe that it has forward secrecy, but it really needs a rigorous proof. There should be no information exposed for people, whose addresses are completely unknown to the DLP solver.

_**Moderator:**_ One question, this is from Sarang. He’s asking where can you find all the different proofs?

_**koe:**_ All the knowledge proofs? I did a write-up, it’s in one of the Seraphis papers, so it’s in the implementing Seraphis paper at my GitHub. So both papers are at that GitHub link, it’s all in there.

_**Moderator:**_ Any other burning questions? Come on up.

_**Audience:**_ Have decisions been made yet about the ring size that you want to move to, or is that still in flux?

_**koe:**_ It hasn’t been settled, but I’ve kind of I’ve been talking about ring size of 128, which has similar performance attributes to what we have right now with 16.

_**Moderator:**_ Anyone else?

_**koe:**_ I do kind of think, that if we could get a better range proof, then it might be justifiable to bump that up to 256, but that’s just personal, and there’s not like a lot of enthusiasm about that.

_**Moderator:**_ So after this we’re coming up on a break, but I do actually have one question for you, sir.

_**koe:**_ All right.

_**Moderator:**_ After working on such an enormous change to such a large project, any advice for future large projects? Anything that you have learned, that you would like to pass on to other developers, other project managers, people who might be looking to fund a larger project? What they should be looking for to know it will be successful in the future?

_**koe:**_ That is an extremely hard question.

[…]

_**Moderator:**_ I might have to.

_**koe:**_ That’s a really hard one. I’d say only pursue a really big project, if it has a really big impact on the actual properties of your system. So with Seraphis we’re getting significantly better membership proofs, design Seraphis… I originally came up with the idea for Seraphis, because I was trying to figure out how we could achieve global membership proofs. So Seraphis is aimed at global membership proofs. And all we’ve actually made progress on discussing global membership proofs, because Seraphis, the design of Seraphis is really calibrated to that design space of membership proofs.

Jamtis is a significant improvement for view wallets, which some people don’t find valuable, but are really valuable for a large component of the ecosystem. So that’s what I would say, but there’s probably more to be said about that subject for sure.

_**Doug:**_ Right, we’re going to take one more question, and then we’re going to have a short lunch - it’s only going to be a half hour long.

_**Audience:**_ Thank you for speaking today. I wanted to congratulate you on all your work, and it’s really inspiring to see the way you’ve dove head in, and I know you didn’t have all the answers when designing Seraphis and were open to feedback, and it was great to see how people in the community could collaborate on such a huge undertaking. I wanted to ask you, I know you came from MobileCoin initially, that was your entry point into Monero…

_**koe:**_ I was with Monero before MobileCoin, I wrote “Zero to Monero”, and then I worked with MobileCoin.

_**Audience:**_ Where do you see that project now, is anything going on there that we should be looking at in terms of technology?

_**koe:**_ I’ve not been looking at MobileCoin recently, so I couldn’t really comment on that.

_**Audience:**_ Ok, thank you.

_**Doug:**_ Chaotic with the piercing questions as always.

All right, I think we’ll wrap it up there. Let’s all give it up for koe. Thank you, man. That was that was amazing.

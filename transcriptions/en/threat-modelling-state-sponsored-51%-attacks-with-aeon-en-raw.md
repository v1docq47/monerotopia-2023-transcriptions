# Aeon

_**Threat Modelling State Sponsored 51% Attacks**_

[https://youtu.be/0kGM1in2mQ4](https://youtu.be/0kGM1in2mQ4)

---

_**Aeon:**_ Thanks for having me guys, it's been an awesome conference so far. I wish I could have been there in the flesh, but lo and behold the government requires a fancy piece of paper to go across imaginary borders. So unfortunately I couldn't be there.

So today I'm going to talk about a topic that I've been really interested in the last couple years. I've been in Bitcoin and the crypto space for quite a while. In the most recent years it's got a lot more interested in threat models and how these systems can be attacked, and pretty much how to attack them, because the best thing to do is to learn how to attack them before our adversaries can. So we can find out how to mitigate and how to defend against any attacks. So today we're going to talk about that and I hope to have a lot of discussion and kind of make it more of a Socratic style discussion with questions at the end, for people to throw out ideas and get responses in regards to different threat models.

So it's going to be more of a general talk about proof of work in general rather than just Monero. But we'll be talking about specifics in Monero in regard to certain aspects that Monero has cards against 51% attacks. So let's get started.

So the one thing to remember in these scenarios is the state should always be considered the adversary. I mean, proof of work is economically rational, incentivized enough to where a private actor is not incentivized to really try and take down the network. There, you know, if you're gonna amass enough mining power to do a 51% attack, you might as well just join the network rather than try and go against it. And so the state incentive is that they want to control the flow of money and they want to control economic activity and information. And they have an economically rational incentive in regards to control, where a free market participant doesn't have that incentive. They don't really care to control economic activity. They just want to participate. So the state should always be considered the adversary.

So just some basics. What is 51% attack? This is kind of the clearest definition I could come up with. An adversary controlling 51% of the hash power with the intent to censor or double-spend transactions and damage the utility of the network. So a miner could have, any miner could have 51% of the hash power. It's very profitable to be a majority miner. But if they're not censoring or double-spending, then it's really not an attack. It's, you know, the system works fine. It's a risk because it's a centralization of hash power, but if they're not directly attacking it, it's not a big deal. So that intent to damage the network is really what we're talking about here.

What could 51% attack accomplish? Again, double spending, which is sending the same amount of money twice. I send 50 Monero to an exchange, I sell it on the exchange, and then I double spend using 51% of the hash power to what you would call reorganize that block and then the transaction is no longer on the chain. I have the 50 Monero, the exchange doesn't, but I have whatever I sold or whatever product I bought.

Censoring transactions - so certain transactions you don't want to be processed would not be processed. So this is more likely in a Bitcoin proof of work scenario where the blockchain is transparent. So let's say a state actor has 51% of the power. They say: "All right, these transactions from North Korea are going to get censored. If you produce a block that has this UTXO in it, it's not going to get built on. We're going to reject that block and ignore it."

And the third thing is censoring miners. So if a state actor has 51% of the hashing power and a minor, a minority minor produces a valid block, the majority miners can ignore that block. And therefore the minority miners lose that block subsidy and the fee reward for that block. So if I'm mining blocks that the state doesn't like and they have 51% of the hash power, They could ignore my block and just continue building on the heaviest chain that they create because they have the most hash power. So essentially 51% attacker chooses who gets paid in that regard because you can sensor blocks if the block contains things that you do not agree with that the attacker or sensor does not agree with.

Identifying 51% attack. These are a few things, I'm sure there's more, but these are the ones that were most obvious to me. High number of orphan blocks. An orphan block is a block that's valid but is later disregarded as being part of the longest chain. And so this can happen organically in the network with two blocks being found, you know, let's say two different sides of the earth and then another block is found on top of it. And so the first block that was found gets disregarded and the newer block with two blocks on it, this is the longer chain gets propagated on. And then it could also happen in 51% attack, where again, I was just saying, you know, a miner or state actor with fewer percent can ignore certain blocks that have certain transactions in them where they don't like, and they could just ignore any block and then continue building because they have 51% of the hash power, they're producing the most work, nodes follow the most work. This is how the system is designed.

Again, valid blocks not being mined on, similar thing to orphan block. And then deep reorgs of the blockchain. So that's like double-spending. So let's say your transaction has like five or six transactions and then suddenly it has zero. That means the blockchain has been reorg by either an adversary or just a chain split.

Okay, so mitigating. This is when it gets interesting. Significant energy usage. So proof of work mining is really the energy put into it. If you have one miner using one computer to secure your blockchain, it's weak. It's not much energy behind it. So significant energy usage is one of the most important things.

Second most important is highly distributed, high distribution of that miner or energy. And so in regards to Bitcoin, you know, they have these giant data centers of mega miners popping up in US and Canada, and Europe. These are a huge security risk because they're easily identifiable. They're usually registered with the state. It's usually, you know, could be license in the future. They're already trying to tax them. They're easily taxable. And so having these huge data sending center miners is not conducive to security of the network.

Accessible mining hardware. So ASICs, I mean, I understand the argument for ASICs and why they're good. And I also understand this is why they're bad. You know, it's hard to run ASICs. You need 240 volt. You can't run on house current. You know, it takes a lot of noise. It incentivizes a more industrial scale mining. So, you know, the normal person can't just easily buy an ASIC and plug it in and run it. Whereas CPU mining, you know, you just hold CPU hanging around, just plug it in, start mining. You don't need any special electricity on any special, you know, ventilation, any of that.

And another part, people don't think realize is plausible deniability. So if I'm buying an ASIC and importing it from China, the SHA-256 ASIC, they're going to know exactly what I'm using it for. And I say: well, why are you buying this? You know, are you licensed with the government to use this to mine on this device and not process illegal transactions? So they're going to know. Anyone who's buying a shop to use ASIC, there's only one reason to buy that. In Monero, if you're buying a bunch of CPUs, there's a hundred reasons why you're buying a bunch of CPUs. They can't prove or even assume that you're going to be using it to mine Monero. So plausible deniability is a huge point when you're facing a state adversary in any relation, not just in mining.

A uniform UTXO set. So again, Bitcoin is transparent. UTXOs can be differentiated. Monero, on the other hand, it's homogeneous. All UTXOs, you can't differentiate them. And so the fungibility is there. We'll talk about more of this later when we get into the fee incentives.

And then the last thing we've been getting is disregarding laws and regulations. If you're miners, and nodes are not willing to break the law, they're useless. Because any moment the state could say: "You'll go to jail if you're caught mining Monero, or if you're running a Monero node, or if you're running a Bitcoin node."

And so the distinction between here white market and black market is a crucial one, because… the distinction between white market and black market is crucial because any white market participant is by definition going to adhere to any state regulation or permitting. Black market by definition is not going to. They're going to be willing to break the law and do as they will in a free market. And so white market is always going to be a security risk. And I say this on Twitter a lot, you know. White market hash is a security risk and the only secure hash is black market hash because the black market hash would be willing to go against and disregard laws and regulations concerning their activity in the free market.

_[Ad]_

_Do you love coffee and Monero as much as we do? Consider making gratuitas.org your daily cup. Pay with Monero for premium fresh beans, and if you like what you taste, send a digital cash tip directly to the Guatemalan farmers that made it possible. Proceeds help us grow this channel. Gratuitas and Monero._

_**Aeon:**_ This little meme I made about it, and I think captures it quite well. It just highlights the fact that white market miners are a security risk, and white market miners more often than not build these giant mining farms that are easily identifiable, easily seizable, easily exploited.

So let's say a 51% attack happens. The state actor has gotten the percentage, and we're knee deep at a 51% attack. The only solution in a proof of work system that doesn't require a hard fork is the fee market pressure of the network. And so let's say a state actor has 51% of the network. The orphaning blocks that they don't like… the only defense proof-of-work network has is enough fees from censored transactions that incentivize black market miners to put new hash online to reap those fees and out hash the attacker.

And so in Bitcoin it's a little more obvious because you have the UTXO set that is transparent, and so the state actor is censoring any transaction that they don't, like and they're allowing certain transactions to go through. And so the fee pressure from those censored transactions needs to get to the point where black market miners or free market actors are economically incentivized to add more hash to the network and out hash at 51% attacker. And this is no guarantee, but this is the only method other than a hard fork. And it makes sense, it's economically rational.

And in regards to proof-of-stake, also, this is not possible. So in a proof of stake network, when 51% of the stake is obtained by a central actor, there's no way to outstake them, because they're always going to be getting more stake than everyone else. The reason proof-of-work works is because it's an external validation method where you can actually add more physical energy to the network. A proof-of-stake method, the consensus is ingrained within the network. The token is the consensus and the consensus relies on the token. And so proof-of-stake is really a non-starter in my opinion in regards to 51% attacks. There is no way to overcome an attacker without hard forking on 51% on a proof-of-stake network. Proof-of-work allows you to add energy, out hash the attacker, and reap the rewards by the central transactions.

The hard fork methods change the hashing algorithm that punishes every miner, not just the attacker. And so you kind of napalm the whole entire network and every miner gets destroyed. It's not really a great solution. It's kind of a last ditch effort.

Change consensus mechanism - same thing. It punishes all legitimate miners, rational and black market miners, not just the adversarial miners.

And then breaking the heaviest chain. In Bitcoin, I don't think this is possible in Monero, but in Bitcoin, there's a RPC command where you could call invalidate block and you can manually go in and invalidate a certain block, even if it's valid and your node will now follow will not follow that chain. It'll assume that block is invalid. Even if that chain has the heaviest most work done to it. It'll manually go in and fork off from that block which essentially breaks the heaviest chain rule and proof-of-work, which is the fundamental consensus and coordination method of the system.

These three methods aren't great. And again, Monero, I looked, I don't think Monero has an invalid block type command. So again, hard forking is not the best method, but it's a kind of a last ditch effort. You really want to have the economically rational method.

Here's kind of a really simple graphic that I was inspired by Eric Voskuil, who's a great thinker in this space in regards to adversarial thinking and economic incentives and all sourced one of his books at the end. But this is based on his something I saw he presented. And this is the basis of the security model for proof-of-work. When Satoshi put together Bitcoin, he essentially created a free market for blockspace. And what miners are doing is they're selling you confirmations via blockspace and users are purchasing that blockspace with transaction fees. And this is the free market. Miners are selling blockspace, users are buying blockspace. It's very simple. This is what the security model is based on. If miners are selling blockspace and no one wants to buy it, the network's not secure. No one is using it and vice versa. If users are wanting to buy in blockspace and there's no one offering it, the security model breaks down as well.

There's a few caveats that I think are important that some people might not agree with, but the tail emission with Monero, you know, I don't fully agree that it adds to security of the network, especially under 51% attack. And because under 51% attack, the attacker is going to get paid the subsidy anyway. There's no way to prevent the attacker from getting the subsidy. So they're indeed by default getting paid to attack the network because there's a tail emission and a subsidy. And also if you have 51% attack, the attacker chooses who gets the subsidy. So if a minority miner mines a block and that 51% majority doesn't want them to have that subsidy, they just ignore that block, orphan it and build on their own chain. So the attacker chooses who gets paid when they have 51% of the attacker. And the attacker is always gonna get paid the subsidy if they have 51% because they choose who gets paid. And on Bitcoin, same thing.

And then this is kind of a kind of far out there, but in regards to like the UTXO set where Bitcoin, you can see what's happening, UTXO is transparent, but in Monero you can't. In some possible way, the state could require you to offer your or provide your view keys along with your transaction for them to process your transaction and put it in a block in the scenario where they have 51% of the hash power. And so using transaction extra field, they say: "Oh, we'll only process transactions if there's a view key provided in the transaction extra field." So that would feed back into the fee pressure mechanism, where they're not processing transactions that have the view keys, or don't have the view keys, I should say. So I don't think it's a reality.

And again, Monero, based on, just having ASIC resistance, having more distributed mining, not centralized mining, and having transparent UTXO, I'm sorry, uniform UTXO set and being completely anonymous really adds to the defense against with 51% attack.

I went kind of quick, I hope there's a lot of questions. This is, you can contact me on Twitter. This is the book. I got a lot of this information was inspired and derived from "Cryptoeconomics" by Eric Voskuil, who's probably the foremost thinker in these threat models and adversarial actors. This book is a must if you're interested in this kind of thing. So I recommend it. You can find it for free on his GitHub. And I think it's free as digital PDF as well. So, that's it.

If there's any questions or clarifications, I'm happy to go over it.

_**Moderator:**_ I have a question. Are you Satoshi Nakamoto?

_**Aeon:**_ There's no way for me to prove it. Even if I had the keys.

_**Moderator:**_ Okay.

_**Aeon:**_ I could have stolen them.

_**Moderator:**_ Yeah, sure. Okay. Thanks for your talk, man. Thank you.

_**Aeon:**_ Yeah, thanks.

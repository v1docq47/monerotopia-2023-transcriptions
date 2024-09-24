# Kee Jefferys

_**Monero & More: Creating L2 & Proof of Stake Infrastructure on Cryptonote**_

[https://youtu.be/zBCV2UvAYWY](https://youtu.be/zBCV2UvAYWY)

---

_**Moderator:**_ Next up is Kee Jefferys from Oxen. He’s gonna talk about Monero and more things.

_**Kee:**_ Hi guys, really good to be here at this conference. And yeah, that was a super exciting talk. Got me really fucking hyped up. So, I mean, my talk is probably gonna be much less interesting. So just a warning.

So who am I? I’m the CTO at Oxen and, by extension, Session and Lokinet, all of the things that we work on. We started in 2018 or early 2019. So we’ve been working in this space for a long time. We forked Monero, a code fork, not a blockchain fork in 2019. And we are probably one of the more actively developed Monero forks out there. So I think we’re about 6,000 or 7,000 lines of code, or not lines of code, our commits diverged now from Monero. And a lot of the stuff that we built in Oxen has also, well, some of the stuff that we built in Oxen has also been merged back to Monero as well. Stuff like IPv6 support was a notable contribution. And yeah, we also have Layer 2 applications, so Session, most of you will have heard of. And those Layer 2 applications are probably pushing around 600,000 monthly active users. So lots of stuff there.

So the reason that I wanted to give this talk was that we forked Monero in 2018 and we’ve spent a lot of time developing an infrastructure for enabling different kind of applications on Layer 1. So we’ve been able to provide instant finality for transactions, we’ve been able to provide checkpointing, and we’ve also been able to transition into a proof of stake network as well. So kind of giving this talk is a way to offer a model to the Monero community that they could adopt if they wanted to go down that direction. I know there isn’t a lot of support out there for proof of stake, but I think it’s important that the projects that have gone down that route at least make it clear why they did so. So yeah, we’ll go through the Layer 2, how to build all of this stuff on CryptoNote, and just our other applications as well.

So why did we want to create a Layer 2 in the first place? Why did we start out down this path? The first thing was it wasn’t really for traditional scaling reasons. A lot of blockchains will create Layer 2 because they want to have say a million transactions per second or some laudable goal. We didn’t really do it for that reason. We wanted to develop applications. We saw applications like Signal and Tor and thought, how could we have a Sybil resistant or a decentralized network where we would enable some of those applications? So that’s primarily why we started Oxen. And we also wanted to provide improvements for Layer 1. So we saw coins out there in 2018 like Dash which were using this essentially like a master layer to bring qualities back to Layer 1. And that’s essentially why we wanted to start.

So what was required? So the step one, the first step that we kind of need is we need staking and rewards. This is the most basic aspect of any kind of Layer 2 that you would establish. And then also the second layer is we wanna have like a network infrastructure. So we wanna have some resources that the network can be provided. That can be the bandwidth resources, computational resources. I’ll talk about that a bit more. And then also network policing to ensure that those resources that are provided to the network are held to a certain standard.

So how do we build it? The first kind of challenge we had was how do we do staking on top of CryptoNote? Because obviously everything is private. You need to have auditability of the transactions that are being staked. So there was kind of, we needed a way to lock coins and we needed also a way to associate those coins with an entity. So our first approach was to use time locked outputs. So Monero has this feature called “unlock time” and essentially allows you to send a transaction where the outputs in that transaction are locked for an arbitrary period of time. You lock them for 30 days, you can lock them for, I think, the max is like millions of years or something like that.

So essentially the first approach was the user would send a transaction to themselves for the staking amount that was required to stake on the chain, and they would lock that to themselves for 30 days. They would also in the tx extra expose the transaction key, which allowed anyone to come along and read the blockchain and work out the amounts that were being staked. It’s a bit of a different model than what was presented before with proof of stake on top of CryptoNote. Yeah, so now anyone can scan through and verify that a person is staking.

And then we also needed a way to associate those coins with an entity. So basically we also put a generated Ed25519 key in the transaction extra, and that allows signatures to be recognized off-chain is associated with a particular stake.

Cool, so that was the first version. It wasn’t great because basically you’d have to restake every 30 days. So you would, you know, stake to the network, then your stake would expire, you’d have to do it again. We had some software that would do this automatically, but then you’re kind of keeping your keys in memory, which isn’t very good practice. And also the way that unlock time works by default is that you, when you send the transaction, you also lock the change that’s coming back as well. So we needed to do some interesting output management strategies where the user would sweep to themselves before they would lock their coins. Otherwise, their change would get locked, which is obviously not good.

So, yeah, instead, the second approach we looked at was what if we could use key images? Because key images essentially stop double spends by identifying the unique part of a transaction. So what we essentially did was the user would send a transaction to themselves in that transaction, that would be the amount of the stake as well that they would send to themselves. They would then expose the transaction, the tx key and the tx extra. And from that, with all of that information, you can calculate the future key image that spending those outputs would create. And then the network can come to consensus about locking those key images. So we can essentially say that if we see a transaction that has this key image, we’re just not going to allow it to be spent on the network. And you can come to consensus that way.

The nice properties that this… also yet this does have privacy impacts as well. That’s important to say you’re differentiating your transactions from the normal network transactions. So ideally you don’t use these transactions as inputs to other ring signatures. So you can do that blacklisting.

But this provides some really nice properties. So it gives us like an infinite staking. So once someone’s staked, they don’t need to restake. It just lasts forever until they wanna leave the network and also allows us to define kind of like an easy penalty system as well. In Oxen basically what happens if your node’s not running at the proper performance, then we can blacklist your key image from being spent for a certain amount of time. And that has an opportunity cost. And we can also do slashing if we want by preventing that key image from being spent at all.

Cool. And the next aspect is rewards. So this one’s pretty simple. You can basically just take the coinbase transaction and split that reward. So the initial thing that we did, we had miners originally, was that we had some portion being given to the miners and then some portion being given to the nodes on the network. And a simple way to do this is to have your nodes in an ordered list, so that when a node reaches the top of the list, you pay it back, pay it out and then it goes to the bottom of the list and you just keep doing that process on and on again. That’s kind of what Ethereum does as well.

So what do we have now? Like with those two things that I talked about, like we have a very basic staking system with those stakers being rewarded. But what we don’t have yet, stakers don’t have any purpose other than just locking supply. They’re not involved in consensus. We haven’t built a Layer 2 yet. So we need to add some more things to this.

So what do we need to add? So stakers need to contribute some sort of network, infrastructure to the network. So this is traditionally a VPS or some sort of server infrastructure. And that infrastructure needs to be able to communicate with each other so we can actually do interesting things with it. And we need to be able to remove infrastructure that’s not performing to the level that we expect.

Cool, so what infrastructure? So typically we’re talking about computation resources, network resources, and storage resources. With these things, you can kind of generally construct most applications in a decentralized manner. And the staker has to be contactable on the network as well. So they have to register an IP address and a port that they’re contactable at, and they have to respond to other servers too.

So yeah, how does the network registration actually work? So we talked a bit about the Ed25519 key that’s associated with every stake. Essentially, we have a peer-to-peer message that’s propagated into the network when someone stakes. They also need to register their hardware as well. They’ll sign a message with their Ed25519 key, which specifies the port and the IP address of their service node. They’ll send that into the message, and the other servers will pick it up and store it in a local database. Using this approach is a bit more flexible than just putting the IP address and the port in the actual registration transaction, because if you want to change your IP address, you can still do it by updating your propagated network message.

We also need network policing as well, because we don’t want to just have resources out on the network that aren’t actually up or providing poor performance. The most naive approach to this is probably just to build like some centralized node in the network which looks at the performance of every other node and then makes like put some transactions on chain to say: “This node is working, this node isn’t working”. This is not a great approach because it’s centralized obviously - this service could essentially ban all of the nodes on the network if they just decided they want us to do that.

The way that we do it is the network self-polices itself. So these nodes are drawn together into a group of nodes, and then they historically measure the performance of other nodes in the network. So they’ll ask them for data, they’ll ping them to see if they’re online. And then we draw groups later on in the protocol as well to vote essentially by super majority on which nodes they think are performant or not. And this gives you a kind of self-policing based approach where the network will decide which nodes it wants to slash or which nodes it doesn’t wanna slash. And there’s also a kind of benefit to that as well in terms of the network performance too.

So yeah, now what do we have? We’ve got basic staking, we’ve got rewards, we’ve got a Sybil resistant, attack resistant network and poor performance architecture or poor performance nodes are being removed as well. And it’s all built on CryptoNote slash Monero.

_[Ad]_

_Do you love coffee and Monero as much as we do? Consider making gratuitas.org your daily cup. Pay with Monero for premium fresh beans, and if you like what you taste, send a digital cash tip directly to the Guatemalan farmers that made it possible. Proceeds help us grow this channel. Gratuitas and Monero._

_**Kee:**_ So what can we do now - specifically at the Layer 1, there’s some interesting things that we can do. So we can do instant finality, we can have checkpointing, and we can build other applications as well. And I’ll talk a little bit about this.

So instant finality, I think Dash was the first coin to kind of come up with this in the Masternode system. They have a thing called InstantSend. It would be really cool if we could do InstantSend on top of Monero, and that was kind of the impetus for this idea. So Layer 1 transactions on Monero are taking 10 blocks to confirm. That’s more of a wallet limitation. But I looked about, I tried to find some more information about the longest reorgs in the Monero chain. And I think I found some information that potentially up to 20 blocks was the longest reorg that we’ve seen. So, yeah, the users will need to have some sort of security tolerance, essentially, when they’re accepting a transaction. If there’s real tx that are up to that length, they may wanna wait longer, they may wanna wait less. It just depends on their preference. But it would be nice if we could finalize transactions without having to wait for an excessive amount of confirmations. So if you’re buying something at a store, you can make that transaction and then just leave and they’re confident that that transaction is gonna confirm eventually.

So yeah, how do we do instant finality? So we draw these kind of nodes that have been created that I was talking about before into quorums, which are groups of randomly selected nodes from the network. Users will submit their transactions to these quorums directly, and that group of nodes will essentially check the validity of the transaction. So if they’re happy that the transaction isn’t being double spent or isn’t violating any of the other rules of the protocol, they will put a signature on top of that transaction, which is a super majority signature. So it’s essentially, you know, if there was 20 nodes, it would be like 17 nodes out of 20 would be signing on top of this signature or on top of this transaction. That transaction then gets resubmitted back into the mempool, but in this case, has this special signature on top of it. And essentially what that signature denotes is that if there is a block created by a miner which spends the same key image in our case, because that’s the unique part of a crypto transaction, if there is another block that spends that key image, that block will not be valid and will not be accepted to the network.

This is the same way that Dash does it as well. It’s just we’re using key images instead. They’re using particular outputs. So it’s a fairly well-studied approach. There are some caveats around syncing. Generally, your transactions that you send into the mempool are not higher order objects, but we sync these transactions as if they are kind of blocks.

Yes, so that has been integrated into Oxen since 2019. Processed hundreds of thousands of transactions using Blink now. It is a default method, and your average time for this signature to be placed by the quorum on a transaction is about 1.5 seconds. So it is very fast confirmation that transaction will be included and essentially it holds more consensus weight than the new blocks that would be created.

Cool. Another thing that we can do is checkpointing. This has probably been talked about a bit more frequently in the Monero community. Monero actually already places checkpoints on the chain in new releases. They don’t place them automatically. There’s no automated system. Essentially, what a checkpoint does is it says that, once a checkpoint is placed on the chain, there can’t be a reorganization past that checkpoint. They really limit the ability for 51% attackers mine a chain secretly and then submit that to the network and revert a whole bunch of transactions, because if they do that and they don’t have the checkpoint on their chain, then it’ll just be invalidated by the network.

Cool. So it’s a very similar idea with checkpointing. We draw these service nodes into quorums of nodes. They look at historical blocks and they’ll essentially place a checkpoint on these historical blocks. And then we can decide what rules we wanna have around these checkpoints. And it’s a similar thing like the quorum has to have a super majority of votes to say that this particular block is valid. So yeah, we have implemented this in Oxen, basically the way it works is that every fourth block is checkpointed and once you have two checkpoints on a chain, that chain has reached finality and you can’t revert past that point. So even if a miner was to produce a chain in secret and submit it, it would just be invalidated by the network. And yeah, in most cases that means we’re reaching complete finality for all transactions after eight blocks. It can be more depending on whether the network is able to come to consensus about placing a checkpoint on that chain. 250,000 checkpointed blocks. The system’s been running for a million blocks now with the same block time as Monero, so about two minutes, so yeah, multiple years.

Also, like most of you are probably aware of the applications that we build on top of Oxen. So yeah, you can do these Layer 1 improvements, but you can also do Layer 2 applications when you have this Layer 2 that you’ve built. And yeah, you can utilize the decentralized properties of the net.

So, yeah, obviously Session is the application that we develop, biggest application that’s on top of Oxen. And essentially, we’re basically just exploiting the storage resources of the network and exploiting the bandwidth resources of the network. Messages are routed using the bandwidth and then stored on these servers for a particular amount of time. So that’s a pretty simple application. It’s also important to note that although you can store messages on service nodes individually, service nodes can always go offline or online, but you can actually build layers on top of the Layer 2 to provide more redundancy around storage. So we have this thing called Swarms where a message is replicated across a bunch of nodes, and then if one of those nodes goes offline, at least you don’t lose your message. There’s other copies that exist.

Cool. And then, yeah, Lokinet. If you were here at Alex’s talk before, obviously, it’s generalized onion router. And, yeah, there’s more information about that online as well.

So, to bring this back to how this kind of relates to Monerotopia and the Monero world. What would Monero’s ideal route be to proof of stake, or if it was going to follow kind of the Oxen route? There would need to be some changes at the consensus level. So, supporting key image locking at the base layer would be awesome, allocating a portion of the block reward towards staking, and then defining a fixed staking requirement as well would be necessary. And then also changing the output selections to avoid selecting outputs that were used for staking essentially.

But this is probably not a very realistic route, the route that I spoke about before, to Monero adopting proof of stake. There’s not a lot of agreement on proof of stake. And yeah, it would just be consensus changes at the Monero level, especially economic changes are very likely to be rejected. So how could we build this without protocol support? I know this is a little bit controversial, but yeah, we could use time locked outputs to lock Monero for periods of time and then expose the tx key and the transaction extra field. This would essentially allow you to do what we did in our version 1 of staking, and have a bootstrap network with staking with XMR.

You could mint a new token that could either be off on another chain, or there’s constructions out there. I don’t think they’re actually implemented. Well, I know that they’re not implemented in Monero yet, but there was a construction called MARCT, which is multi-asset RingCT, which would allow for tokens to be minced on top of Monero. If something like that existed, then you could also reward those stakers who are staking in Monero with tokens on top of Monero, but more likely you’re probably going to have to use an external token to do this.

Then you would reward tokens essentially to those users who are staking Monero through this time locked output method. You could punish via opportunity costs. So say if you got people to stake Monero for 90 days and then their infrastructure went offline, then you could essentially say: “We’re not gonna pay your rewards and your Monero is gonna be locked up for the rest of the 45 days, which is an opportunity cost”, that would affect them.

But yeah, you probably want protocol level changes to do something like this. And also don’t do this without talking to Monero research people, because it probably has a very large privacy effect I’m not considering.

Yeah, so this network would essentially allow you to kind of bootstrap a network of Monero stakers. You could do stuff like creating incentivized apps like what we’ve done with Session and LokiNet. You could also create consensus networks on top of Monero as well. This would be useful for maybe like trialing checkpoints. You could place your own trial checkpoints and also do like trial confirmations for instant finality. It’s not like you still need consensus layer changes to make those changes actually work, but it may offer some incentive to say: “This is actually working on a secondary network and maybe this is something that can be adopted later down the track”.

Cool. So, thanks for listening to my talk. If you want to find out more about the Oxen products, all the links are there. I’m on Session as well if you want to message me and I’ll be here at the conference. And there’s some references as well to some of the research on this. I didn’t talk much about proof of stake. I should have talked more about it. But we have a white paper up here for how it all works. And I’ll checkpointing on our Instant Finality system. So yeah, thanks guys.

_**Moderator:**_ You have time for a few questions?

_**Kee:**_ Yeah, of course.

_**Moderator:**_ Any questions for Kee?

_**Question:**_ I already have a question to your colleague, but I figured about long-range attacks from your presentation. I just want to confirm, do you use hidden amounts or not in your network?

_**Kee:**_ Oh, in the staking system? Is it hidden amounts?

_**Question:**_ Yeah, is it network with hidden amounts, right?

_**Kee:**_ Yeah, yeah, yeah. So it’s...

_**Question:**_ Yeah, but what about bonding? You reveal amounts for the bonding? When the people log their coins, the stakers...

_**Kee:**_ Yeah, those amounts are completely refilled.

_**Question:**_ My question, do you consider to build a system where the stakers would log the coins without reveling them on, but still building the correct consensus which will fix this all with hidden calculations?

_**Kee:**_ Yeah, we’ve considered it. Oxen has a bit of a different system in terms of it. It has a fixed staking requirement, right? So there’s 15,000 Oxen. So you kind of already know when someone is staking that they’re locking a certain amount of coins, they’re locking 15,000 Oxen. So it’s less of a privacy advantage to have that amount hidden, because it’s already kind of implied by a new stake. And also the staking portion of Oxen is like fairly small. Like there’s 1800 nodes that are staking. And generally, we feel that they are giving up their privacy to provide this network service, and they’re being rewarded for doing that. So, they can still move back to privacy as well after they’ve finished staking and with their rewards. But while they’re staking, we feel that perhaps not as important.

_**Question:**_ Okay, thank you. And last question is the algorithm of consensus is PBFT, is it correct?

_**Kee:**_ Yeah, close to that.

_**Question:**_ And that’s why you have a finality.

_**Kee:**_ Yes. Yeah.

_**Question:**_ Yeah. I just was thinking it would be really nice, because PBFT is also a great thing, because it can give you a final feature, but it would be really cool if you guys can build the PBFT on top of hidden amounts. So nobody reveals how much they have, but everything is fixed and working.

_**Kee:**_ Yeah. That would be very cool. I’m interested in that as well. Yeah.

_**Audience:**_ Thank you.

_**Moderator:**_ Any other questions for Kee? Do we have anything online? I think that’s it. Let’s give it up for Kee.

_**Kee:**_ Cool. Thanks, guys.

# Elizabeth Binks

_**ETH-XMR Atomic Swaps**_

[https://youtu.be/lUJxn0SmvQ8](https://youtu.be/lUJxn0SmvQ8)

---

_**Doug:**_ Thank you so much for coming.

_**Elizabeth:**_ Thank you. Cool. Is this screen on? Ok, ok cool.

Hello, everyone. Nice to be here. I’ll be talking about ETH-Monero atomic swaps. So a little bit about me. I’m a software developer, I’ve been working on blockchain protocol development for slightly over five years now. Worked on a lot of things in the past few years cool.

So, agenda. I’m gonna talk about, first, the state of the swap, and then after that yeah I’ll start the demo, because it takes around 20 minutes, and then I’ll do a bit of like the swap overview and like how it works, how to use it, the roadmap, and then end the demo.

Ok, so a bit of background on atomic swaps for anyone who doesn’t know. So basically it’s a P2P and trustless manner of swapping two native assets between chains, so in this case it’s obviously like Ether and Monero. So “atomic” means that either the swap is successful and both parties receive their funds, or the swap is refunded and then both parties receive their funds back. So this is like cryptographically guaranteed in the swap protocols to happen.

So a bit of progress. Last year, like Doug said, I presented on Monerotopia and I did like a stagenet demo. So since then there has been a lot of progress, and the swap is released on mainnet. So yeah, it’s released. So everyone who wants to go try it, feel free to go try it.

Thank you to everyone for funding. Shout out to MAGIC for organizing and assisting with a second grants round. So like Doug said, yeah MAGIC is cool, yeah, it’s tax deductible if you’re a citizen, I’m not, but if you are it’s cool. And also thanks to all contributors to the codebase. The code is open source for all contributors, it’s written in Go. Go is like really easy, so if you want to contribute like just hit up the repo —there’s good first issues.

Ok, so I’ll start the demo. So I’m gonna show the swap on mainnet. Let me make this bigger. Don’t really clear… this first… This is like hard with one hand. Ok, is it big enough? Ok, so basically right now this swap is like CLI only, there’s no UI unfortunately, because you know it’s hard and I don’t know how to make them. So there’s a CLI, so basically we made it like as easy as possible hopefully to run it. So there’s like binaries released on the GitHub, you can just like download them for Mac or Linux. If you’re on Windows like it’s recommended that you use it in Docker, because I don’t have Windows, and I don’t know how to build separate for Windows. So you can build it from source or you can use it in Docker either way. So essentially… computer is kind of slow right now. So I’m going to like delete some of this - some of these are old.

_**Moderator:**_ Set up the video, I’ll just hold the mic.

_**Elizabeth:**_ Yeah, that would be cool. Thank you. So basically once you have it, you can make build, and it just like builds it, I’m just gonna rebuild it, and I’ll show the binaries. So yeah essentially there’s just a swap Daemon process, and then a swap CLI process that like interacts with it. So the swap Daemon, the program is peer-to-peer, it uses the PDP Library. So basically when you start up the swap, daemon joins the peer-to-peer network and then it’s able to interact with all the other nodes through that. That’s also how you find offers on the network.

So now I have my binaries, so I’m just gonna run, I actually have it already running, so I’m not gonna run it again. But I’ll show, I don’t know, some commands and stuff. It get really very slow. Oh my God, I knew this is gonna happen. Ok, whatever. So basically you can see there’s like a bunch of config values, blah-blah. Basically if you’re running it on mainnet, the only thing you need to provide is Monero and Ethereum endpoints. If you have a Monero daemon running, it should like auto connect to that, otherwise you have to specify it. So yeah, anyways it’s running.

So I’m going to query my nodes. So I guess the first thing I can do is check for offers on the network. Ok, so I have like two right now, because one is going to be the “maker” and one is the “taker”. So basically these are all the offers right now. Another thing about this swap is due to protocol limitations only Monero holders can be the makers, and then ETH holders have to be the takers. So unfortunate yeah there’s reasons why this is the case, but I think the Bitcoin swap has the same limitation. So that’s currently the case - all these offers are for Monero, and then if you have ETH then you can take it. So I can just check all of them, and then on my node here I can check for my local offers, and then I can see that this is like an offer I have up right now.

So this is my P2P peer ID or I did not mean to copy paste that. So I will do that again. Ok, I’m just gonna like take off for now, I feel like I’ve been talking for a long time. So to take the offer. Sorry, let me make another offer. So I can take that one. I’ll show you guys how to make it. So it’s simple, you just put “make” and then you put the minimum amount of Monero you want to provide. So I’m just gonna like basically copy paste like the same swap that’s here. Ups. And then I don’t know I’ll do like a slightly different amount, and then exchange rate, so all these parameters like the exchange rate are specified at the time of the offer, so yeah it’s like ETH to XMR ratio, was it like a slightly different one, I’m just gonna make the offer. Ok, cool.

So then on this one I should be able to take it, so just put “take” and then the offer ID, so yes, every offer has hash, that’s its ID, and then the P2P ID, and then the amount you want to provide. So this amount will be in ETH so I can see like the amounts are between like 0.1077 ETH and then this amount of ETH, so I’ll just do like I don’t know let’s do 0.02. Ok, so it started. Yeah!

So you can check as well for your ongoing swaps. So we should see that like shortly the ETH is locked, so it became locked. This is right now just pushing the updates, you can just exit and also just do like swap CLI ongoing. Ok, you can see like when you put “ongoing”, it tells you like the idea when it started, how much you’re gonna get, exchange rate, like blah-blah, the status and then the swap time status, there’s like two timers in the swap protocol that I’ll explain, and then it also gives you like an estimated time, it’s right now around like 20-25 minutes. You can tell it’s slightly different for like the taker versus the maker, because the taker like transfers the Monero out after. Ok, so we’re just gonna wait 20 minutes and then check on it. Cool, thank you. So the reason it takes 20 minutes is Monero block time, because there’s like the 10 block unlock period. If blocks are like super fast so that’s like not a limiting factor.

So now the demo started. I’ll just talk about how the swap protocol works. So essentially you have two parties, you can call them Alice and Bob. Alice has ETH and wants Monero, Bob has Monero and wants ETH. So Bob offer on the P2P network, Alice just like crawls the network essentially and then finds it, and then says like: “Oh, it’s a nice offer”, and then takes it. And then each party, when the swap is initiated, generates a secret value so we just call them like XA and XB. So Alice has XA, Bob has XB.

So in the success path what happens is at the start of the swap Alice - so this is first of the key exchange - so the first initial thing is that the swaps initiated they exchange their public keys corresponding to the secrets, and then Alice locks ETH in the contract, Bob sees the ETH is locked, and then locks the Monero in the account where the private key is the summation of the two private keys. So basically Bob knows Alice’s public key, he knows his own private key, he multiplies those and then can get this account, and then the next stage is that the contract is like set to “ready”. So Alice sees that the Monero was locked, and then allows Bob to claim from the contract. So basically this step is somewhat optional, but it speeds up the swap, because the other alternative is that you have to wait until the first timeout is reached. So after that the contract is ready for Bob to claim, and then Bob, when he claims to ETH from the contract, in the same transaction he has to reveal his swap secret XB. So that’s like the atomic part essentially. So the claim reveals the swap secret, which allows Alice to claim the Monero on the other side. So yeah, that’s the atomic part essentially.

And there’s like two refund paths essentially. So the reason why the timeouts exist is in the case that someone backs out or someone goes offline, so basically if something unexpected happens, you’re able to refund within a certain time period. So in the first refund path for example say Alice locks her ETH and then Bob just backs out and never locks the Monero. So what happens is that when the first timeout approaches, there’s a refund function on the contract that Alice can then call, and then the refund function, she has to also provide her swap secret, like in case Bob did lock and then like Alice just didn’t realize for whatever reason, so this basically allows Bob to refund if necessary.

The second refund path is if both parties lock and then Bob never claims, so this is also something that could happen, just like he goes offline or something like that, so what happens is that basically the contract’s set to “ready”, Bob never claims, and the second timeout is reached, and then after the second timeout the only thing that can happen is that Alice refunds the ETH from the contract. So similarly like I mentioned before, in the refund you have secret, so then Bob when he comes back online later is able to get Monero back from the locked account.

Cool, I hope that made sense with the diagrams. So if there’s any like that’s unclear, just ask me after, there’s also talks on the GitHub, so you can look at it also after.

So some of the road to mainnet and what’s happened since last year. So swap protocol itself is quite simple. It doesn’t take that long to implement. What’s a lot harder is implementing everything around it that makes it actually usable and safe. And if your computer crashes you’re not gonna just lose your funds and that kind of things.

So the first main thing is like database and restart safety. So with the swap daemon you can restart it, you can crash your computer. the internet can go out, blah-blah. when you restart the swap daemon it’ll restart the swap where it ended. So if you haven’t reach a timeout, it’ll just continue with the swap as it is or it’ll refund, if the time was reached or something like that. So this was really important obviously, because you can go out and you won’t lose your funds. That was fun to implement because there’s a lot of different cases that can happen with that, because it makes it happen at any time. So yeah, it’s always one of the main things for fractionizing the code.

The other thing is a relayer system. So one of the limitations with Ethereum is that you need ETH in your account to call a function on a contract to pay for the gas. i.i. if for example Bob in the previous example is the ETH taker, you have to call a claim function on the contract to get your ETH. However if you don’t have ETH to begin with or you want to withdraw to a fresh account this is bad, UX wise and like privacy wise. So the workaround is a relayer system, where essentially you sign a message saying that “I want to claim”, but then someone else actually submits it and then pays the gas for you. So this is currently implemented, and it works. The swap that I demoed will probably, it should use the relayer as well.

So the relayer basically if you don’t have enough funds, or if you just say you want to use a relayer, it’ll just like automatically use one. It’s also permissionless and decentralized to join, it’s on the P2P network. you just say: “I want to be a relayer”, and then you’re in and you get like a small fee, you get a small portion of the swap funds if you relay a transaction, because you’re spending money to call that. So that’s in place. That was also fun and went through a few iterations to get it to working well. So right now it’s quite nice. So if you want to be a relayer I recommend it.

The other thing is ERC20 support. I know a lot of people asked for ERC20 support. It’s in the contract. So you can make an offer and say: “I want to receive DIE” or “I want to receive…” I don’t know whatever stable coin or whatever token essentially. So you can make an offer and say: “I want to receive whatever token”, and then when you do the swap you’ll end up getting that token instead of ETH. That’s implemented.

And then there are just like a lot of bug fixes, and UX improvements making the CLI nice and usable. And then there’s also like a dedicated bootnode program. So like I mentioned everything is peer-to-peer, so one part of the peer-to-peer network is like bootstrapping, so when you join the network you need to know someone who’s in the network to even join the network to begin with. So that was where the bootnodes come in. So this is basically like a publicly posted list of nodes that are in the network, and then when you join you hit up the bootnodes first, and then from there on you can find the nodes on the rest of the network. So it’s essentially like an entry point. So if anyone wants to run one, that’s also something you can do, you don’t need any ETH or you don’t need any Monero, but it still helps to contribute to the decentralization of the network, because more bootnodes are better, because it’s just better.

So then I guess I kind of mentioned this already, but the swap program uses the libp2p library. It comes with like a lot of nice features. There’s stuff like NAT Traversal and hole punching, so if you’re on your own home network or you’re behind a NAT, all the peer-to-peer stuff will still work, which is super nice and it’s just kind of built in. There’s also like a DHT for all the peer discovery, offer discovery, relayer discovery etc. It’s entirely P2P and permissionless, and decentralized, which I’m very happy with.

So I guess I kind of talked about this already, but if a node goes offline during a swap it’s ok, because you can restart it. I think I kind of said everything in this slide. So everything is stored on-disk. so basically everything is in a .atomicswap folder, so as long as you don’t delete this you’re fine, so don’t delete it.

Oh yeah, I already talked about this. So yeah I think I already talked about this basically. So a relayer gets a portion of the swap value you find on the network. The relayer submits in pays of gas. So right now gas fees are like kind of high on ETH, which is bad. So in the next release there will be a better method of determining how much of the reward you get.

So the relayer system, how does this actually work. So the first stage is the same as before. So Alice locks the ETH and then Bob locks the Monero, and then when the contract is ready to be claimed, the funds are ready to be claimed, then Bob searches for relayers on the network and then finds say Charlie, who is a relayer, and then Charlie sends his payout address to Bob. So this is the address where the reward will be sent to. Bob signs the claim data along with the relayer, oh sorry, along with Charlie’s payout address, and then Charlie submits this to the contract, which then sends the ETH to Bob minus the fee which is sent to Charlie. Pretty straightforward.

One thing with this is that initially we went with like a design that used kind of Ethereum’s existing transaction relayer sort of like standards. But this was actually really annoying, because all the libraries were in JavaScript, and there’s a lot of like abstraction, which was not really necessary, because we had a very specific use case, because there’s really only one function that’s going to be called. We didn’t really need it to be generic. So I think that did I mention that’s slide.

And then also the smart contract was not front-run protected. Another thing is that testing on mainnet is very different from testing on stagenet, because there’s real money involved, and I mean V-bots are really savage out there, so the first iteration was not actually front-run protected. So I was testing it and then my transaction was front-run basically meaning that some bot claimed the fees while I lost out on my gas fees still. So rip. But anyway the new design is front-run protected, all the functions are front-run protected actually, so no AMV is possible.

So becoming a relayer is totally permissionless. You only need a very small amount of ETH to start off with. The amount you need is basically just enough to call that claim relayer function, which is very, very low. It should be like 0.01 or 0.02 ETH, or something like that. I think it’s probably 0.02 now, because gas went up, so you just need a small amount of ETH to start off on running it. And you can hopefully like maybe get some reward money, so I would highly recommend that if you want to join, but you don’t really want to swap right now or anything.

So talking about the front-run kind of thing. Okay, yeah, I talked about this already. So yeah, bootnodes. Ok. So swap usage and revisit the demo… So I’m gonna look at my demo again and see how it’s doing. So I think it has not made progress yet, because it’s waiting for the Monero to unlock. Ok, there’s only one block left. Yay, okay, cool! So I guess I can show some of the swap CLI commands while we wait for the unlock. Oh, it’s really slow. So softeli has like a bunch of commands. So the swap is working, it’s making progress, so we’ll see what happens.

So basically for example you can do swap CLI like balances, you can see all your ETH balances and your Monero balances, and your accounts, you can also see so there’s “discover” to discover peers, “query” - it’s a query specific peer query, “quirey-all” discover a query so you can find like everything at once, “make” and “take” which I already showed, and then “ongoing” shows your ongoing swaps, which I showed, and then “past” shows like your past swaps. So I have some past swaps here. And then what else? You can clear offers, your current offers, get the status of the swap, there’s a suggested exchange rate, price feed, so if you want to get a suggested exchange rate through the CLI, you can. It uses like the chain link price feed, so it’s not updated, but it’s updated like often ish, but definitely double check it. But this is a good starting point. And then there’s also transfer functions to transfer your funds out. Maybe I didn’t mention this, but when you start the swap, daemon, unless you provide an ETH and Monero address, or a wallet which you can, it’ll generate one for you, and then you just fund those. So for example these ones were auto generated, and then I funded them. So if you want to transfer it out, then you can do transfer from the CLI. So you don’t have to awkwardly import your private key into a wallet.

Ok, so what happened with the swap? Let me check the status. Oh yeah, so it’s sweeping now, so that means this one should be done. ongoing. So okay, so this one completed. Yay! So the offer ID on this one was 0xa7f07 which should be this one. So it was completed successfully. Yay! So it worked. Yeah. And then this one is currently spring, so basically what happens is when it gets the Monero, it’s kind of in a weird, like temporary swap wallet, so it just sends it back to the original account. So that’s what it’s doing right now. So it should be done shortly. And then you can also see like where was it. You can see also on Ether scan, if you check the contract, you can also see what contract calls were made, so you can see this is my new swap transaction. I hope it’s big enough. So this is my new swap transaction, you can see it was made 18 minutes ago. This is really fast. People must be like playing Monero a lot right now. Sometimes it takes half an hour. So you can see that the ETH swap was made, the contract was set to “ready”, allowing the ETH taker to claim. And then the last call was to claim relayers, this is a call to claim through a relayer - so everything worked as expected. Yay! So that’s it. Actually no. I have a presentation.

So the last thing is basically roadmap, so what will be happening next. So one thing that I think would be really good, that basically we should have, is network privacy. So there isn’t really built-in network privacy right now unfortunately. The libp2p library that’s used doesn’t really have good support for this unfortunately. So gonna basically work on adding that through Tor or Nim. I previously worked on a Nim, the P2P library, so I could use that maybe.

And then another thing is I mentioned the dynamic relayer pricing. So it’s fixed right now, which is bad, because ETH gas prices are very high. And then the last thing or the second last thing is like a UI. So I put here browser UI, but kind of debating between a browser UI versus a desktop GUI, or just an integration with something else that already has a UI or GUI. But essentially since it’s all CLI right now people might not like that, it might be nice to wrap it in a program that you’re gonna use graphically. And then another nice thing about the browser is that you could potentially use MetaMask which is a browser Ethereum wallet to interact with the swap. So then you don’t need to transfer funds to the swap program, you can just do it all in browser. So that could be nice. And then as always just testing, bug fixes, I’m sure people will find something that they want to be improved.

So to summarize, the swap’s ready for beta usage, so if you want to try it out, feel free. If there’s any issues just post on GitHub or the Matrix chat, there will be a link on the next slide. And then we need Monero providers. So the whole program runs or the whole network runs on offers, Monero offers specifically. So if you have Monero and you want to, I don’t know, try out the swap, feel free to put up an offer. We need liquidity. It’s built, so yeah, definitely want people to use it now. And then also need relayers and boot nodes, more is better. It helps with the security of the network essentially. So if you want to participate, but you don’t want to do swaps, you can do that.

Ok, that’s basically it. So there’s the GitHub. So the GitHub is AthanorLab/atomic-swap. There’s a matrix room which is #ethwmrswap. So either of these, definitely check out the code if you’re inclined to do so, otherwise post in the chat, we’re in there. So, cool, yeah that’s it. Thank you, everyone.

_[Ad]_

_Do you love coffee and Monero as much as we do? Consider making gratuitas.org your daily cup. Pay with Monero for premium fresh beans, and if you like what you taste, send a digital cash tip directly to the Guatemalan farmers that made it possible. Proceeds help us grow this channel. Gratuitas and Monero._

_**Elizabeth:**_ Yeah, yeah. Do anyone have a question?

_**Audience:**_ Any comments about the previous presenters, integration into the Bisq fork, Haveno?

_**Elizabeth:**_ Yeah, that would be cool. I haven’t actually talked to the Haveno people about it yet, but yeah, definitely we’re looking to do integrations. When I built this swap, I didn’t want it to be the only implementation. It was just like any implementation. So yeah, it’s fully open source, you can just… I wanted to integrate it into other stuff. So I’ll be happy to talk to the Haveno people on integrating that. I talked a bit to the Basic Swap people, who I think are interested in integrating it. So definitely I think integrations are down the line. So yeah, if anyone’s interested, definitely reach out, because I want to be used. Any other questions?

_**Audience:**_ What’s your integration point into the Ethereum network? How are you relaying the transactions to Ethereum?

_**Elizabeth:**_ How are they relayed?

_**Audience:**_ Yeah, like are you using an API provider or how are you accessing the Ethereum network? Is it like a separate node? How does that work on the backend?

_**Elizabeth:**_ Yeah, so you need to have an Ethereum endpoint. So it can be either remote or local, either one works. It’s the same with Monero - you do need to run a Monero node or have a remote Monero node. So you need endpoints for both.

_**Francisco "ArticMine" Cabañas**_ Any possibility of getting Polygon MATIC on there?

_**Elizabeth:**_ Yeah, so that’s another thing I didn’t mention. The swap is entirely EVM compatible exactly as it is right now. All you have to do to put it on something like Polygon, which is like the same EVM as a mainnet Ethereum. It’s just like redeploy the swap. Actually we put the swap contract. That’s it. So it could definitely be deployed.

_**Francisco "ArticMine" Cabañas**_ So you really put a swap contract on Polygon?

_**Elizabeth:**_ Exactly. So you redeploy the swap contract on Polygon and then when you run the swap node. You’ll just point it to a Polygon endpoint essentially.

_**Audience:**_ Would that hypothetically be more secure to get into Polygon earlier in the bridges potentially?

_**Elizabeth:**_ Depends on what you mean by secure. But yeah. I don’t know like it depends what you mean, it depends where you’re coming from. If you’re going from ETH to Polygon, then probably just use their bridge, if you’re going Monero to Polygon, then swap would be better. I hope that I answered the question.

_**Audience:**_ Any comments on how to make it more like a limit audible, or if it’s possible, or if it’s interesting at all.

_**Elizabeth:**_ Yeah, I’ve thought of that a bit, but it’s kind of hard right now. I guess the easiest thing is to have a nice UI that checks all the orders for you and searches for the best offer whatever based on what you want, but definitely maybe in V2.

_**Audience:**_ One more question. Any thoughts on fund limit, like daily limit that sort of thing, like some other mixers would implement?

_**Elizabeth:**_ Wait, sorry, can you ask the question again?

_**Audience:**_ Like, you know, how Aztec, CK money used to have like a thousand or ten grand a day.

_**Elizabeth:**_ Like rate limiting the amounts?

_**Audience:**_ Yeah.

_**Elizabeth:**_ Oh, there’s no rate limiting.

_**Audience:**_ Very cool.

_**Elizabeth:**_ Yeah, no there’s no rate limiting, there’s nothing. And if there’s a problem with the contract or if, I don’t know, if it’s like sanctioned or something, you can just redeploy it. It’s very different from how Tornado Cash works, because there’s no pool. You can just redeploy the contract and it’ll be fine.

_**Audience:**_ Cool. Do you have any active market makers in the protocol yet, and do you have a plan to attract market makers to the protocol?

_**Elizabeth:**_ It was launched like two days ago, so no not really. But definitely that’s very important. So if anyone has any ideas on how to attract market makers, if you want to become a market maker definitely reach out.

_**Audience:**_ And the other question was on the gas efficiency of the contract. So are they well optimized or do you see further optimizations for gas efficiency there on the Ethereum side?

_**Elizabeth:**_ They’re like pretty highly optimized already. I can also show the exact gas finance. For example to call the claim function it costs around twice the cost of transferring ETH. So it’s pretty low to claim via relayer. It’s I think around four times to then transferring ETH and then to initiate the swap. It’s around 2.5 cost. So it’s pretty optimized already.

_**Moderator:**_ Anyone else?

_**Audience:**_ I was just going to ask about bootnodes. What’s required to run a bootnode like technical wise?

_**Elizabeth:**_ So you can do it on a VPS. You just need something that has a public IP essentially, and there’s a static public IP, other than that the resources are very low, less than one gig of RAM, UDS, it’s low, it’s very low.

_**Audience:**_ Can you run a bootnode on Tor on ITP?

_**Elizabeth:**_ Not right now, but hopefully soon.

_**Audience:**_ Ok, cool. All right, thank you.

_**Audience:**_ I’m curious about the privacy on the Ethereum side for the swap. So obviously if they’re all going through a single smart contract, it’d be easy to see what is coming in and out. But suppose I went through the hassle of deploying my contract just for a single swap. What other information would be visible on the Ethereum side to indicate this is related to a Monero trade?

_**Elizabeth:**_ I guess if you redeploy the contract just for your own swap, that’s totally fine, you can do that, just like it’s expensive. But I guess the problem is that you’ll still be able to see that the byte code of what you deployed is the same as the existing one. So you can see that it’s going to be the exact same contract, unless you modify the byte code a little to kind of obfuscate, I don’t know. You can still basically tell that it’s the same swap contract, but there’s nothing that says there’s Monero on the other side. It’s just like ETH contract. Unless someone’s actively monitoring the P2P network and checking every single thing that’s happening, there’s no the swap contract isn’t specifically for Monero. It doesn’t same like Monero and, I don’t know, it could be for any other chain theoretically.

_**Audience:**_ Thank you.

_**Doug:**_ Could you just talk a little bit about potential attack surface. So if I’m a bad actor, you mentioned there’s some built-in protections for wallet restarts, for the node. So what if I decide not to restart the node? Would that allow somebody to lock another individual’s funds and prevent that swap from happening?

_**Elizabeth:**_ No. Basically no. Once an offer is taken, it’s deleted or whatever, it can’t be taken again, unless that swap refunds essentially. But for the duration of the swap it’s on. And the program doesn’t allow for swaps to be taken twice. Obviously that’d be bad. So you can’t double take it, and double lock or something like that. I hope that answers the question.

_**Doug:**_ Luke come on up.

_**Luke:**_ One of the things you mentioned with your relayer system is at one point in development getting front-run and actually losing on your gas fees. Have you considered implementing EIP 4337 or similar, so you actually don’t have to use a relayer network at all but instead just use the Ethereum network directly?

_**Elizabeth:**_ Yeah, so eip4337…

_**Doug:**_ I was going to ask that exact same question actually.

_**Elizabeth:**_ So eip4337, it’s not like native, it’s still built on top of Ethereum protocol. But anyways I guess one thing is that you need a paymaster contract that pays the gas for whatever contract that you’re trying to pay for. So the contract would have to be changed, so that the funds are rerouted to this paymaster. And also the libraries, I think are not super, I don’t know, the CIP is super new, so that the libraries aren’t very developed yet, I’m not even sure if there’s, I don’t know, I think it’s all JavaScript, but honestly something to look into for sure, because it would be good not to have to rely on our own relayer system. So yeah, something for v2 for sure.

_**Doug:**_ Let’s take a few more questions if you can, so we get the slides ready for the next talk. Anybody else? Go ahead.

_**Audience:**_ I have a general question just about Tornado Cash. Maybe you could comment a little bit on how you see Tornado Cash, everything that went down, and how Monero might be able to supplement privacy on Ethereum, or maybe liquidity that could happen entering Monero, getting connected into the Ethereum ecosystem, just kind of more general thoughts on that?

_**Elizabeth:**_ I guess there’s privacy for Ethereum, just privacy in general. I guess the main thing is that the swap, if you trade your ETH to Monero, then once you’re in Monero it’s kind of all private, which is great. And then going back the other way from Monero to ETH. You can also go into a fresh account, which is nice. But it’s the purpose of the swap program, is it’s for swaps, so it’s different from the purpose of Tornado Cash, which is specifically to put your funds in a shielded pool. It’s all on ETH, and then you’re like: “Let’s draw it later”. So like I mentioned, the swap is pretty different, because the swap contract isn’t like a pool, it’s not like Torino Cash. You’re just kind of using the contract and to go across chain.

_**Audience:**_ So do you feel like there’s better protections potentially doing an XMR-ETH atomic swap versus using something like Tornado Cash, which may or may not be sanctioned depending on who you are or what citizen you are?

_**Elizabeth:**_ Yeah, yeah, because in Tornado Cash you can’t really redeploy it as easily, because like I mentioned the contract itself holds a pool of funds, which is the anonymity set, and you can’t just redeploy the contract, because you have to bootstrap the whole anonymity set again versus the swap where there’s no anonymity set pool etc. It’s literally just a two-party swap that goes into Monero. So I think it’s pretty different in that regard.

_**Audience:**_ How does the ETH community view this implementation? Are they excited about it or they not know about it, what do you see out there?

_**Elizabeth:**_ Some people are interested. ETH is an interesting community. I think they want privacy, for sure, but, I don’t know, the focus is very like cipherpunk, like public goods funding, like scalability, like that kind of thing, so there hasn’t been as much excitement about like kind of thing versus those sorts of things. But, I don’t know, I hope they use it, I think they will.

_**Audience:**_ How can we raise awareness in the Ethereum community about this tool and that it’s available?

_**Elizabeth:**_ I don’t know. I posted on Twitter and a lot of you people follow me, so that’s one thing. But I guess…

_**Audience:**_ The Tornado Cash community isn’t aware and interested in this?

_**Elizabeth:**_ No, I think they’re like interested. I don’t know. I feel like the there’s a group, a subset of these people that are also already into Monero and DarkFi like DarkZK, like, I don’t know, like all the anonymity progress, these kind of stuff. So that subset is definitely interested. But then there’s the other subset of people that are just kind of… I don’t know. So I think those types you kind of have to reach out to, like the subset that’s already into privacy is obviously.

_**Doug:**_ Well you’re doing amazing work, we all greatly appreciate it, bringing the Monero and Ethereum communities together.

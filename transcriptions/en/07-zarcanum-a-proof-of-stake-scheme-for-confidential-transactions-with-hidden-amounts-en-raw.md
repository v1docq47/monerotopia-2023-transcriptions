# Sowle & koe

_**Zarcanum: A Proof-of-Stake Scheme for Confidential Transactions with Hidden Amounts**_

[https://youtu.be/6NQ-TiE5syY](https://youtu.be/6NQ-TiE5syY)

---

_**Doug:**_ This day is dedicated to privacy tech, things other than just Monero. So, you heard, we’ve just heard about Bitcoin privacy tech. Next we’re going to hear about Zano. We already heard a little about, a bit about it the other day. But now we’re going to hear about Zarcanum, which is the Proof-of-Stake scheme that they are working on or have developed. Correct? It’s already been developed?

_**Sowle:**_ Yes, it’s already been developed. Actually it’s in the testnet of Zano, since November.

_**Doug:**_ And so this is going to be a talk by Sowle, isn’t it?

_**Sowle:**_ Yeah, that’s correct. My name is Val, and I most known maybe by handle Sawle. So I’m a research developer from Zano.

_**Doug:**_ And you did this in collaboration with koe, who’s up until this day I think has been more known for working on Monero related things, right?

_**koe:**_ Yeah, they asked me to take a look at their papers, so I ended up helping them.

_**Doug:**_ So very exciting that we have both of them here presenting on this and it’s beautiful to see this collaboration taking place in the privacy tech space. Take it away, guys.

_**Sowle:**_ Thank you. Guys, can you see the cursor on the screen? Yeah, super.

So we would like to talk about Proof-of-Stake today. So Zano from the inception had a hybrid Proof-of-Work and Proof-of-Stake consensus that was initially developed and implemented by Cryptozorg over there. And at some point we decided as being a СryptoNote coin, because Zano is the evolution of Blueberry, and Blueberry is implementation of CryptoNote, so at some point we decided that we actually need to move forward and to implement hidden amounts. And at that moment Zano already had this hybrid Proof-of-Work and Proof-of-Stake consensus. So we faced a new challenge - how to implement these hidden amounts on top of the existing Proof-of-Stake consensus.

So for the start I would like to talk a little bit about how the stochastic Proof-of-Stake works. So there are plenty of, like several Proof-of-Stake models known. One of them stochastic Proof-of-Stake that we will be talking today. And maybe most known Proof-of-Stake model is pBFT model that have quite wide adoption to my knowledge. So today we will be focusing on what model that Zano implemented. For us it considered to be more egalitarian, because we don’t actually need a list of fixed validators or kind of thing. So I would like to briefly discuss how it works this stochastic Proof-of-Stake model. Actually this model appeared firstly in PeerCoin in 2012, I think. And the model was later implemented in Zano and other projects. So how it works in a brief.

We have here a blockchain that grows downwards on this picture, and we have a wallet full of UTXOs, unspent transaction outputs. Let’s imagine it’s Alice wallet, and let’s imagine that Alice would like to mine a new Proof-of-Stake block. So here on the left side you can see the blockchain has like Proof-of-Stake and Proof-of-Work blocks at the same time. So it’s like we in Zano having approximately 50 percent of each time. So the first thing Alice need to do is to fill up some special structure, data structure, and it’s very simple - it consists only of four fields. The first two fields basically is a current state of the …. It contains the hash of the last known Proof-of-Work block, and also the kind of hash of last known Proof-of-Stake block. And then she use like kind of set of possible timestamps. This is a special thing that necessary to limit number of attempts Alice has for each Proof-of-Stake iteration. And for the last field she takes an output from her wallet and calculate the key image for this output. She obviously can do this, because she owns this output, and she put this key image into the as the last field. So this completes this data structure, and then Alice calculates a cryptographic hash.

As a result of calculating these cryptographic hash she gets some value. And we can consider this value as a random thing, because it can be predicted the value of this hash. So it’s H, value H over there. And after that likewise it happens for Proof-of-Work mining, after you getting a hash, you compare this hash with a kind of boundary that is set for given iteration. And if that given hash that we consider to be quite random is lower that this boundary, then Alice wins this condition, and actually she can mine a Proof-of-Stake block over there.

So to make it dependent on what actually amount of coins Alice has here, this boundary includes actual amount of an output that Alice selected for this Proof-of-Stake iteration. And also you can see here current Proof-of-Stake difficulty is used. So what it means. As much amount Alice has for a certain output, then she would have more chances to win this condition. And difficulty allows this system to be hit by the network. So as more coins are mined, difficulty grows, the same way as it works for normal Proof-of-Work classic consensus. And this system works actually pretty good, because it’s quite simple. And let me like…

_**koe:**_ One thing I want to highlight with this design is it’s actually quite analogous to Proof-of-Work more than you might expect. So after a block is mined, you can consider time to be discretized into time components. So as time passes for each time component, you perform this test. So like in Proof-of-Work where a miner is continuously recomputing nonses, in this Proof-of-Stake model as time passes for each time block you recompute the hash condition for all the UTXOs that you own. That have been I committed to the staking protocol.

_**Sowle:**_ Yeah, thanks. And this boundary that used for possible timestamps that shown on over there is defined by network rules, and actually these possible timestamps are discrete. So it goes in a division by 50 seconds. So that means that for instance if Alice check all her outputs in her wallet for this inequality, and no one wins this condition, Alice would need to wait 50 seconds to get another possible set of these timestamps. So that makes it like CPU super cheap, because like you only need to check all your outputs once in a while. And after that you need to wait for network condition change. For instance someone would mine a Proof-of-Work block or another Proof-of-Stake block, and this would change this part over there, so effectively as a result you would get another set of hashes and another set of chances of winning. So this could be considered like a lottery, like you have a lottery ticket here, and like you have a lottery ticket that costs more and proportionally gives you more chances to win. And these outputs over there in the wallet can be considered like a set of these tickets that you have in your wallet. So like you have proportionally more chances. So if I have only one coin and someone have like 100 coins, that person will have 100 more chances to win this condition.

So as we now understand how the stochastic Proof-of-Stake works, we can consider what’s the problem with this design when it comes to hidden amounts. So for hidden amounts that like used in Monero for confidential transactions, amounts are hidden and committed in a Pedersen commitments. So we don’t have any more them open in blockchain. And the problem with this design is the following - we cannot use this, A means “amount” here, we cannot use it anymore. For network observers for like public network nodes, which are liable, which in charge of wallet blockchain and each Proof-of-Stake block. So it seems to be impossible to implement. And the solution to the problem is to actually use another approach, to use another inequality to be checked by Alice each time she would like to mine and Proof-of-Stake block. So here, like just a quick mention, here we have an amount that Alice has for instance for some of her output, and this amount A is committed to a Pedersen commitments using some random mask f. So this is the classical way to use a hidden amount in Pedersen commitments.

So you may notice that in this inequality we are still using A in a direct form. So what’s the point? Because we would like to like get rid of it somehow. And let me show how the mask works over there. The main idea is to make this inequality, that is very difficult to work with the commitments, to convert it in equality, and after that to use a homomorphic property of Pedersen commitment to handle it. So as here we have like inequality - something is less than something, some constant value multiplied by A - here we can represent it by an inequality when it is D multiplied by A minus some also known value ba. And as soon as we have this limitation applied on these variables, we have the same result with this inequality. And the main idea here is to use range proof that we already have and that is used to make sure that balance is correct for confidential transactions. So idea here is to use range proofs for value ba to make this inequality being transformed to equality actually.

_**koe:**_ So I want to give some intuition to this equality that he’s discussing here. So in the inequality we have some number that is less than some big number. And this big number is factored into A. So it’s A times some number. So we know the smaller number, the hash is a multiple of A minus some number that’s smaller than A. So since that some number is smaller than A, we can expose the multiplier, so the multiplier D, we can expose that publicly and then perform a range proof subtracted to the verifier.

_**Sowle:**_ Yeah, exactly. And as soon as we come to this equality, we can rearrange it very simple, just like over there, we have move all the variables to the left side. And the next step once we get here, the next step is to make another equality, that looks like very similar to the previous one, by using this assumption. So we introduce another variable, we already know these variables D, f, H and A, so we can calculate just for fun. And as soon as we do this, we have this equation over there that looks quite similar to a previous one. So it has this property that we actually will be using now. So I would like to mention that in this equality H is a hash, like that we mentioned before, that calculated by using this data structure, so this hash could be calculated by anyone who is looking for the blockchain and analyzing this Proof-of-Stake block. f is a special mask that is used in Pedersen commitment, and this is hidden and private number. So only sender and receiver would know this amount. A is amount, and so as amount are hidden, this value is hidden, and only sender and receiver would know it. Variable D would be made public. And for the last one, for ba, we will give a range proof to prove in zero knowledge that its value is less than 2 to the power of 64. So as soon as we get here we have this system of like similarly looking equality.

And here we know all these values, and we can multiply each element on of the left side for generator H and for generator G. So these are generators in eleptric curve that is used… sorry… So we use standard… Oh, really? We use our standard elliptic curve, ed25519, and for that elliptic curve we use standard generator G, and also we have some generator H that are known that they have no relation between them. So when we multiply each member on the left side by the corresponding generator here, and eventually we… sorry. I come up with edit mode somehow. I don’t like to edit my presentation. Oh, yeah, thank you. So and after that, as these generators are not related to each other in cryptographics term, we can combine this system of two equations into one equation. But this equation will be held in terms of group member. So this main equation is shown over there, where we have A′ being equal f multiplied by H plus a multiplied by G. A is a commitment that we previously had. So this is a commitment like hidden amount was committed in. And B is a commitment to the value ba that will be used for range proof. So using this B value we will be proving that ba is less than 2 to the power of 64.

_**koe:**_ So what we actually did here is we combined these two, and it falls down into A′, these two fall down into A, and these two fall down into B. So A is the original commitment of the UTXO that’s being staked. And the reason we do this is because A and F are part of this equality, but they’re both hidden inside of A. So we need this kind of mirroring strategy with A prime in order to make statements about, or in order to homomorphically achieve this subtraction here.

_**Sowle:**_ So it’s almost all. Because this show us how the simplest kind of proof could be done. Having this equality we can reveal D, we can reveal mirror commitment A′ and B. A is already known, because A is a main commitment for our output. Then we need to prove that A′ is a mirror commitment of A, that is could easily be done using kind of Snorr proof. And also we need to prove that ba is less than a 2 to the power 64. So for that we are using range proof whatever we need for. And as soon as we make these proofs in zero knowledge, we came to the conclusion that this inequality is actually satisfied with like a very little exclusions. And this actually show us if… okay, I’m sorry. So if we provide this kind of zero knowledge proofs for our Proof-of-Stake block, each verifier on the blockchain could check these proofs and then come to the conclusion that this inequality is satisfied. That means that Alice actually won this Proof-of-Stake block, and she actually is a liable for given this Proof-of-Stake reward.

_**koe:**_ All right, so we have some challenges. So the model we discussed so far is assuming that the UTXO that’s staked is directly referenced when claiming the stake reward. So the claimer says: “This is my staked UTXO, and here are the proofs about this UTXO”. But what if we want to make a ring signature on six UTXO, which one was actually won the stake, the block? And so the problem here is that when we make a ring signature in order to hide the ring member on the output of the ring signature, you can think: there’s a pseudo output commitment. So it’s a masked version of the original UTXOs amount commitment. And the mask used for this new pseudo commitment is completely random. So we can’t use that mask directly, because it could be manipulated by the staker to win the condition - too easy to cheat as it said here.

So what we do is we make another pseudo output commitment that is constructed differently from what we find normally in RingCT. Instead of extending the existing mask, which is on the generator G, we add a new generator X in order to mask the commitment of the amount staked UTXO. So putting the mask on a new generator separates it from f, from the mask f, which use the stake win condition calculation. And so in order to include this X we need to make proofs and add some additional information to the proof system.

So what we do, we need to prove that our pseudo output commitment is related to our original commitment by mask or by the masked amount or, sorry, by the mask that we added to that commitment. This is very similar to what we do with RingCT, where we subtract and on this subtracted component.

But one thing with this equation is that Ai here has a range proof on it, because when you create UTXOs they’re range proved as part of the normal transaction construction procedure. So if we do C minus Ai and then we sign on that subtraction, then we know that the subtracted value X here is exactly the mask that has been appended to our amount commitment. And so we need to incorporate this X now into our mirror commitment for the Proof-of-Stake proof or the staking proof.

So let’s see here. So right, so we need a more advanced proof in order to show that C′ is the mirror commitment. So we use something called a “linear composition proof”, which you can find the details about that in the Zarcanum paper.

All right, so now we can make the staking proof. So this is very similar to what we did before but now we have the mask X in order to hide the other values so that the verifier cannot reconstruct - what am I looking for - reconstruct the original commitment of the staked UTXO. And so we include an additional mask x′′ in order to prevent the verifier from extracting the original amount commitment.

_**Sowle:**_ May …? So basically what we have here, as koe mentioned before, that for a more complex case, when we actually have a decoy set, we need to somehow hide this commitment. And actually here we have the same system, almost the same system that we have for the simplest case. But now we actually need to incorporate this additional mask to here. And here we end up with having a slightly more difficult, slightly more complex system to incorporate all these values. But the meaning of this system is actually the same as we have for the simplest case.

_**koe:**_ Yeah, we’re just masking everything. Yeah. And so we have a similar equation, but now instead of ending with the identity element we end in with the mask x′, and I believe make a proof on that. Don’t we? Yes, we proved that F is correct here. So this is doing all the same things that we talked, we discussed earlier, but now with additional masking in there to hide what we’re doing without allowing the prover to violate the invariance of the system, and achieve the same inequality.

So there are additional complexities to this system that are difficult to get into in this talk, because of their the level of nuance involved. So one key point here is that in this inequality… right, we go back farther I think, right… so in this equality here we’re exposing D which is - let’s see —by exposing D and knowing that ba is 64 bits, we’re essentially leaking some bits of information about what is committed to in A, in the commitment A of our UTXO. So the sender of the transaction is able to brute force the value A by guessing different values of ba, because ba is only 64 bits, so it’s 63 bits worth of efforts to brute force the value, sorry, to brute force the value… Ah, I skipped the step. So the value f cannot be zero. So we have to add in a value, which will include the view key. But again by guessing ba you can brute force the value of this additional mask that’s added in here, which we discussed in the paper. And by brute forcing that value the sender of a transaction, who is looking at the blockchain, and looking at a stake claims, can recover the staker’s view key. And so we have some mitigations to that, which include increasing ba to 128 bits by including a multiplier here.

_**Sowle:**_ Actually we find a way how to make this vector of attack more complex, to rise its complexity to a feasible level. But it’s covered in the white paper, but obviously we won’t cover it here in the slides.

_**koe:**_ Yeah, I was rereading the paper a week ago, and I rediscovered this problem, forgetting that we’d already solved it. So it’s really impressive to me how we managed to pull together all these proofs and adjustments to achieve the goal of private Proof-of-Stake.

We have more slide here? Ah, yes. You want to discuss this one?

_**Sowle:**_ Yeah, here we have a quite short slide showing how many bits and bytes do we have to incorporate all these changes to existing Proof-of-Stake system that we already have in Zano. And here you can see that we mentioned that “using full featured variant of Zarcanum” - “full featured” means that including all these additional features that making this brute force attack very complex. And also we find, koe find a way how sender-receiver anonymity could be broken and like in the native approach to be shown previously, and he finds a very clever way how we actually can get rid of this. But unfortunately the price for getting rid of this problem is to append 32 bytes per each output. But this is like optional, because in some designs it could be achieved, this sender-receiver anonymity, could be achieved in other way. For instance Alice receives some funds from Bob, and before staking these funds, staking these outputs to create a new Proof-of-Stake blocks, she first send these outputs to herself and stake this outputs only after this procedure. So it may be inconvenient for some designs, so the this why in the paper we suggested the way how this could be eliminated. So here, we mentioned full featured variant of Zarcanum, using full featured variant and using a Bulletproof+ for range proof we actually end up with having a 64 multiplied by n plus 896 bytes per signature and one group elements per output.

_**Moderator:**_ […]

_**Sowle:**_ Yes, thank you.

_**Moderator:**_ Should we get the next slides ready? Great. Any questions from the audience? Can you come? Reuben.

_**Audience:**_ Suggest wondering like obviously in a very unlikely event that Monero goes to Proof-of-Stake, can something this be used or would it have to be heavily modified? Especially like maybe with a Seraphis or…

_**Audience:**_ Good question.

_**koe:**_ Yes, so I believe it’s compatible. Doing some math in my head. Yes, it’s possible.

_**Audience:**_ I have a question - has this been published anywhere or peer-reviewed yet?

_**Sowle:**_ It has been published on IACR in November 20-21st I think. And it’s actually been reviewed, actually we had a feedback from Dr. Sarang on this. So maybe we would need some more peer reviews on that. But at that moment this is considered like quite simple design for us, and we try to cover all the possibilities, all the issues that we ahead. So this design as all I mentioned is implemented in Zano at that moment, and we don’t have it like live in the mainnet, but it’s live in a testnet since November. So we are making sure that everything is working well before going live.

_**Audience:**_ Thank you.

_**Moderator:**_ Any other questions? Can you come?

_**Audience:**_ Does these have any impact on the ability to verify for inflation box?

_**Sowle:**_ Sorry, do you mean like we…

_**Audience:**_ […]

_**Sowle:**_ So the question is does this design help in mitigating inflation attack? So this is like difficult question. I think this design is about making this winning condition, making it like proven by zero knowledge. And the main difficulty for us was to prove the inequality using this standard cryptographic elements. So as soon as consensus design and as soon as emission design allows a limited number of coins to be emitted where Proof-of-Stake blocks, all other pieces of this design at my opinion shouldn’t lead to any kind of like inflation. So I can see, like here being on stage and looking at the ceiling, I can’t see any possibilities how that could be made, but maybe we need more…

_**koe:**_ So it can’t, if the win condition can be spoofed, so anyone can satisfy it regardless of what amounts they have. Then it’s possible for someone to make extremely rapid Proof-of-Stake blocks every epoch I suppose. So as soon as one Proof-of-Stake block is out, you can make another one, because you immediately satisfy the condition by validating the inequality. So it does depend on the math being right to prevent that situation.

_**Sowle:**_ Yeah, I would like to add that if such an event happened, first it would be obvious from observing the blockchain that number of Proof-of-Stake blocks increasing significantly, and at the same time like in Zano we have hybrid consensus, so if someone find a bridge and find a way how to make Proof-of-Stake blocks very easily, it wouldn’t be drastic for Zano, for instance. But if this design would be implemented like as a Proof-of-Stake only model, that would may have some implications. So this need be more careful with it, like with Proof-of Stake at all.

_**Moderator:**_ Unfortunately, we’re out of time. Let’s give it up for the Zano team. Thank you guys.
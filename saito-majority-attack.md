# Does Saito Prevent 51% Attacks?

## Introduction

Saito leans heavily on its economic innovation driven by superior incentives in the routing and storage networks. Somewhat curiously, it also claims to prevent the sustainable class of 51% attacks, as Vitalik generalized to [Discouragement Attacks](https://eips.ethereum.org/assets/eip-2982/ef-Discouragement-Attacks.pdf):

* Summarily, the ability of a majority of block producring power to censor the minority, earn profit while doing it and decrease the profitablity and sustainability of competing enterprises.

This is the opposite of the case when the minority tries this, attackers' eneterprise is unsustainable; the fact that the very desirable properties when no majority is in control are neatly flipped when a majority *does* exist indicates that the network would quickly close off open access to consensus participation.

The interesting part of Saito's 51% attack is that it can be achieved in a scheme which fails to address the Free-Rider problem of node relay incentives or the Tragedy-of-The-Commons around arbitrary data storage. To the extent that Saito's claim to solving the 51% attack is true, it comes solely from:

1. Using transaction fees as 'block work' in place of mining.
2. Having the release of rewards dependent on a lottery chosen by a post hoc mining puzzle.

This simplified mechanism, stripped of Saito's other mechanisms will be detailed later.

## 51% Attack on Nakamoto Consensus

> **Reader's Note**: "Chain" and "fork" are used interchangeably.

Fork-choice is performed generally by longest-chain, and the class of 51% attack comes from the ability to ignore the rest of the network while producing the longest chain - giving complete control of accepted block production to the majority.

Nakamoto Consensus is the basic mechanism for ditributed consensus on an append-only data structure which organizes the Bitcoin network around a single fork. Though other consensus system's exist which serve similar functions, none except Saito's regularly claim to have solved the 51% attack, so analysis of the attack will be done on the oldest, simplest mechanism: Nakamoto Consensus.

Taking the commonly held assumption that Bitcoin's cryptographic components (public key pairs, digital signatures, hash commitments) are secure, the next most disruptive avenue for attack would be the consensus mechanism. Attacks include:

1. Double Spend: Rewriting long sections of the chain after reasonable participants believe the fork has finalized
2. User Censorship: Producing the chain as normal, but ommitting transactions from certain wallets.
3. Miner censorship: Rewriting forks where *other* miners earn rewards; burning their profits and reducing their future participation.

Note that 3 is a prerequisite for 2, through 3 can be done in varying levels of severity. 1 and 2 are fairly overt and disrupt the usual flow of the network, reaveling the attacker's control. Even still, the honest network cannot ameliorate the situation without sacrificing the PoW function securing the chain, but it likely drives token price down.

3 is far more insidious because it can be done subtly until competing miners have dropped out; the attacker can pose as adverserial to itself while in fact having a secret monopoly. It would be impractical to detect and is posited as being a possible state of the network at the time of writing.

### Naive Minority Attack

Consider a naive attack on consensus by a minority block producer, one with less than 51% hash:

$A$ is an attacking node with 20% hashpower
$H$ is any of the remaining 80% of  hash producing nodes

If all participants follow the default protocol, the chain will have blocks produced in proportion to the hashrate of each group:

$... \rightarrow [A] \rightarrow [H] \rightarrow [H] \rightarrow [H] \rightarrow [H] \rightarrow ...$

And if the attacker, $A$, decides to drop out of the public chain and compete against it:

$...  \rightarrow [H] \rightarrow [H] \rightarrow [H] \rightarrow [H] \rightarrow ...$

$... \rightarrow [A]  \rightarrow ...$

They will be unable to outcompete it in the production of valid blocks. The money they spend producing a chain whose length is rejected in favor of the longer honest fork cannot be recouped and the honest nodes enjoy less competition.

### Majority Attack

Consider now the attacker, $A$, has a majority of hashpower:

$A$ is an attacker with 51% of hashpower
$H$ is the remaining network with 49% of hashpower

When all participants follow the default protocol, the chain will contain blocks in almost equal proportion, slightly favoring $A$:

$... \rightarrow [A] \rightarrow [H] \rightarrow [A] \rightarrow [H] \rightarrow [A] \rightarrow [A] \rightarrow [H] \rightarrow ...$

But if the attacker decides to reject the public chain and produce a solo chain:

$... \rightarrow [H] \rightarrow [H] \rightarrow [H] \rightarrow [H] \rightarrow ...$

$... \rightarrow [A] \rightarrow [A] \rightarrow [A] \rightarrow [A] \rightarrow [A] \rightarrow ...$

On a long enough timescale, 51% will always end up beating 49% meaning that the majority can always produce an acceptable chain while rejecting blocks made by any other participant. This is the basis for the class of disruptive actions which fall under *51% Attack*.

Note the inversion of incentives from the aforementioned scenario where no single majority exists: rejecting the blocks of others resulted in wasting one's own money working on a fork which is destined to remain shorter and ignored in favor of the more inclusive fork.

But when a majority decides to attack, they can make certain that the minority network never has their blocks in an accepted fork and thus never recieves profit; they become the party wasting their money, and the attacker may enjoy ***zero*** competition.

A common, last ditch argument against the feasability of this attack is that if it is discovered, the token price would plummet and the attacker, a majority owner of network infrastructure reliant on that price, would suffer the most losses, thus acting as a detterent.

While [there is much to say](https://mukdde.substack.com/p/bitcoin-is-possibly-being-51-attacked) about enactments of such an attack which appear non-disruptive and do not reveal that a majority exists, the mere existence of an attack which threatens to bring down the network is considered sufficient for this document to continue.

## Does Saito Solve It?

Stripping away routing signatures and automatic transaction rebroadcasting, Saito is a blockchain where the total transaction fee in a block is substituted in for hashpower - for valid block production.

Hashing is not eliminated however, though block production may be done only through fee collection, the distribution of those fees relies on a proof-of-work being included in the following block, referencing the block whose reward it aims to claim.

The block producer and the producer of the hash then split the total fee reward collected in the block. Much like Bitcoin, the production of hashpower which commits real energy usage to a single fork is required to sustain that fork.

Let us project the same method of majority attack on Nakamoto Consensus onto Saito Consensus:

$A$ is an attacker with 51% of fee-inflows
$H$ is the remaining network - 49% fee-inflows

$... \rightarrow [H] \rightarrow [H] \rightarrow [H] \rightarrow [H] \rightarrow ...$

$... \rightarrow [A] \rightarrow [A] \rightarrow [A] \rightarrow [A] \rightarrow [A] \rightarrow ...$

> It is worth restating the significance of the longest-chain rule and the nature of producing hashpower to produce blocks. In the above attack performed on Bitcoin, every honest block $[H]$ commits itself to the previous block in *that* fork. When another fork grows longer and is preferred, the energy put into the so-called 'orphaned' block is effectively wasted from the perspective of that miner.
> 
> Note that this itself doesn't constitute an attack, as every miner in a normally operating Bitcoin network, except one, will burn energy failing to produce a block for every block. With no majority attacker, this feature is acceptable, because the proportion of blocks mined will match the proportion of hashpower a miner posesses. It is under a majority attack this becomes an issue.
>
> But when a majority attacker is able to, at will, censor the completed blocks of others, it equally grants the ability to erase their profits and progress on the minority chain. Working on the minority chain is a feature under no majority, but under a malicious majority the minority chain of honest participants has very little chance recovery.

Saito Consensus differs in a simple but important way: the work which allows production of valid blocks is not committed to any particular fork. Aha!

The same 51% attack which commandeers Nakamoto Consensus (more technically, it reduces the share of rewards towards the honest network below their proportion of 'work' produced) fails on Saito because while the attacker can produce a longer chain, the rest of the network has no issues adopting their chain and building on top of it.

Whereas on Bitcoin building on top of the attacking chain means to begin hashing again from a new block, the fees collected by nodes in Saito are valid for block production on any fork. Unlike Nakamoto Consensus, their work cannot be orphaned - they simply append it to whatever most recent longest chain they discover.

To illustrate this, honest blocks below are distinctly labelled:

$... \rightarrow [H_1] \rightarrow [H_2] \rightarrow [H_3] \rightarrow [H_4] \rightarrow ...$

$... \rightarrow [A_1] \rightarrow [A_2] \rightarrow [A_3] \rightarrow [A_4] \rightarrow [A_5] \rightarrow ...$

In Nakamoto Consensus, $H_4$ is only valid proceding $H_3$, but in Saito Consensus the fees are valid for as long as the transactions which provide them have not been included in another block. No matter how long the attacking chain becomes, the honest network can always append their blocks to it:

$... \rightarrow [H_1] \rightarrow [H_2] \rightarrow [H_3] \rightarrow [H_4] \rightarrow ...$

$... \rightarrow [A_1] \rightarrow [A_2] \rightarrow [A_3] \rightarrow [A_4] \rightarrow [A_5] \rightarrow [H_1] \rightarrow [H_2] \rightarrow [H_3] \rightarrow [H_4] \rightarrow ...$

**Importantly,** since the 49% of fee-flow the honest network receives is distinct from the 51% of the majority, their work in its entirety can be appended; the longer the honest chain is isolated, the greater they can extend the attacking chain by including all the blocks that attacker attempts to censor.

As the majority attempts to extend their exclusive chain, the speed at which the attacker must produce blocks to then overwrite the likewise growing honest minority chain, who may freely append atop the attacker's work, also grows.

Whatever the attacker's base fee-inflow is is not enough to sustain this. They must spend their own fees at a growing rate to extend their exclusive fork. Due to the fact that those fees are then split between the attacker and a miner, the attacker is always burning money by using it to extend the chain. Even if that attacker mines all the rewards, they pay a cost to mine and at best may only receive back the money they themselves put in - and nothing more.

If the majority block producer's power is ramped up to 66%, 80%, 99%, the same recourse applies as above, but the attacker can sustain censorship at less cost; note that the cost for this attacker is, however, always positive.

Some slightly opaque (but assumedly valid) analysis of the algebra of these returns at different majority sizes is shown on the [Saito Wiki](https://wiki.saito.io/en/consensus/math).

## Conclusion

We can see that a majority attacker slowly loses their ability to print an exclusive chain as time goes on; even large majority attackers with 99% of work face this, though at a reduced rate. These attackers will, like any majority attacker under Saito, at some point be forced to include the minority fork.

This prevents the economically sustainable (profitable) execution all class of 51% attacks. Most notably, it prevents any majority block producer from excluding outsiders from entering into and participating in the network - specifically and technically: a node who produces $x$% of work receives on average $x$% of rewards despite the existence or behavior of any malicious majority.

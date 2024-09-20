# Private Order Book

<!-- add header image -->

## Order Books: A Powerful Financial Product

I want to discuss a PoC of an on-chain order book exchange on Polygon Miden. Miden is perfectly suitable for even a private on-chain order book exchange leveraging client-side proving, high throughput, updateable transactions, and the security of Ethereum. 

An order book refers to an electronic list of buy and sell orders for a specific security or financial instrument organized by price level. There are three parts to an order book: buy orders, sell orders, and order history. An order book is dynamic and constantly updated in real-time throughout the day. A key feature of order book models is allowing users to submit two types of orders: market or limit orders. Market orders mean a trader instantly buys or sells at the best price (highest bid, lowest ask). Limit orders are not instant and buy or sell at a specific price. Next to the order book, there also must be an order matching system following some rules of best execution to have an order book exchange. 

In crypto markets, the dominant venue for exchange has been centralized order book-based exchanges (CEX). There is high demand for on-chain order-book exchanges; see [here](https://twitter.com/EliBenSasson/status/1614575237122629633?s=20) 40 TPS only on dYdX. On-chain exchanges are mostly AMM-based exchanges that cannot provide quite the same benefits: 

Order-book exchanges:
- Provide price efficiency
- Reduce the risk of slippage and have no impermanent loss
- Are widely accepted by both institutional and retail traders alike 
- Are easier to use

To run an on-chain order book, several features are needed:

Needed:
1. High TPS - builders ask for 30-40 and some even more
2. Low finality - sub-second finality would be optimal
3. Quick and cheap order updates - Order updates should be almost instantaneous and free, especially for Market Makers

Nice to have:
4. Privacy at the base layer - to not reveal a trading strategy 
5. Matching engine - FIFO/Pro-Rata should also run on-chain to ensure "best execution"
6. MEV protection - to ensure "best execution"

## Miden: An Improved Execution Infrastructure

Order-book exchanges on account-based blockchains are hard to implement. The algorithms of matching engines, commonly implemented in centralized exchanges (CEXs) based on the CLOB model, consume a large number of resources, making it challenging to deploy and run on the Ethereum blockchain, see [here](https://www.gdx.org/gridex-whitepaper.pdf). Some DEXs have attempted to solve this issue by implementing matching engines and order books off-chain - dYdX (on StarkEX) or 0x. However, such solutions increase the risk of centralization (to a certain extent) and limit their ability to interact with other protocols, making it impossible to utilize or contribute to the open ecosystem of Ethereum effectively.

Polygon Miden offers the required features (1-6) while simultaneously, computational integrity is secured by Ethereum, and liquidity is provided by Ethereum and the Polygon ecosystem. 

Polygon Miden is designed to have high TPS sustainably. Orders can be updated for free and in no time using updateable transactions. Polygon Miden offers different levels of privacy at the base layer. Matching engines can run off-chain, but there can be a zk-proof of correct execution to be verified by the network. 
    
For finality, we need to define what we mean by that. 

## A Powerful Order Book on Miden
    
Let's collect ideas for implementing a private on-chain order book exchange on Miden. Given the [Miden Architecture](https://docs.polygon.technology/miden/miden-base/architecture/overview/), we can design a simple on-chain order book exchange. 

In our simple example:
- Users shall be able to place a limit order (buy or sell)
- Users shall be able to place a market order (buy or sell)
- Users shall be able to take any order 
- Users shall be able to cancel any order
- There shall be some form of best execution
- The order book shall be visible on any web2 interface like a homepage

In Miden, there are [Accounts](https://docs.polygon.technology/miden/miden-base/architecture/accounts/) and [Notes](https://0xpolygonmiden.github.io/miden-base/architecture/notes.html), used in transactions[here](https://docs.polygon.technology/miden/miden-base/architecture/transactions/overview/). Notes can be updated for free and in no time; therefore, Notes can represent an order. 
    
![](https://hackmd.io/_uploads/Skavl8sSn.png)
    
All Notes with the same `Tag` can be listed in the same order book. Notes carry the asset to sell in their `Vault` - this cannot be updated, and in this example, it is 1 ETH. In addition, Notes have a single executable `Script`. This script is the root of a [Miden program MAST](https://github.com/0xPolygonMiden/miden-base/blob/main/miden-lib/asm/note_scripts/SWAP.masm). Here is where we can encode our order. For example, `0x1234` can mean:

```
receive( 1 ETH )  # what is in the vault
send( 2000 DAI, note.sender ) # send to the creator of this note
```

Whoever wants to take this order needs to consume the above note and, in doing so, execute the script. Like this, we can pretty easily support limit orders, where limits can be updated nearly instantaneously in one direction.

The order book could now list all notes with the same `Tag`.
    
![](https://hackmd.io/_uploads/ryr24LsS2.png)

This will serve as our order book - which can be shown in a simple web2 interface/homepage. Now any user can take any order by simply consuming the respective note in a transaction - first come, first serve. 

Notes in that order book must be public. That means everyone sees all the note's data and can compute the nullifier. That means whenever there is a nullifier in the Nullifier DB, the operator of the order book interface can delete the respective note. 

### Limit Orders Can Be Updated for Free by Revealing Parts of the Script

A limit order is a direction to buy or sell an asset for a specific price (limit) or better. We can update those limits by revealing new parts of the note script. The note's script can be partially hidden at the beginning, and whenever a user wants to update a limit order, a new leaf can be revealed in no time. 
    
![7mz95q](https://github.com/0xPolygonMiden/examples/assets/35031754/6fa0a333-f6a7-46e5-aca6-fa5206eda427)

Now the user first reveals what A is. Then, what B is, and so on. 
    
```
A = {receive 1 ETH && send 2000 DAI to sender}
```

If no one takes that order, the user reveals another part of the script.

```
B = {receive 1 ETH && send 1990 DAI to sender}
```
    
### Market Orders Are Slightly More Complex

A market order is an order to buy or sell an asset immediately. This type of order guarantees that the order will be executed but does not guarantee the execution price. 

A naive implementation would be to create a note having a script that queries the current market price. The script could be like `receive( 1 ETH ), send( current_price, note.sender )`. Whereas `current_price` must come from an oracle but it should be easy to compute given that all notes are public. However, there might be attack vectors for nonliquid markets. 

So now, users can place limit and market orders by creating new notes. Those orders can be updated by simply revealing new parts of the script. Those updates don't need to happen on-chain, but, e.g., in any public forum or directly to the operator, and are therefore for free and can happen in no time. And anybody can take orders by simply consuming the respective notes.

### Order Canceling

In theory, any order can be canceled if the note's producer consumes the note himself. That would require an on-chain transaction, though. There might be a cheaper way by revealing a certain part of the script that makes the note unconsumable. 

### Order Matching

We need some mechanism to match buy and sell orders. This mechanism should ensure/aim for "best execution". 

A matching engine runs a matching algorithm, which fills an order. So if there are five sell orders of "200 A, 150 A, 50A, 100A, 100A" at slightly different prices and a limit buy order of "350 A" the algorithm chooses which sell to pick to fill the buy order. Iterating over many orders on-chain on a traditional EVM blockchain would be quite gas intense. On Miden, this is different.

#### Option 1: No Matching Engine

Matching engines are a must in the centralized setting. In our simple case, users could decide which order to take. There could be a pool of notes - all with the same tag - and users pick the notes they want for consumption.

This way, best execution is ensured by the user, trades are the cheapest, and the implementation is super easy. However, it might be complicated to coordinate. Two users might want to consume the same note simultaneously, and only one would succeed. 

#### Option 2: Matching Account (Normal Account)

In Miden, every smart contract is an [account](https://0xpolygonmiden.github.io/miden-base/architecture/accounts.html). So trades could be executed by executing functions of the matching engines account. We can have functions for limit and functions for market orders. That way, we don't need an oracle, either. Traditional matching algorithms - FiFo or Pro Rata - could be implemented in the account. This account running the matching engine would need to be credibly neutral somehow. Or it should be possible to verify post-factum that the engine was credibly neutral using a zkProof. This approach would save a lot of gas, compared to traditional EVM blockchains. 

![](https://hackmd.io/_uploads/rkCLdLsr2.png)

This account would need an operator that executes all transactions within a certain time frame. The operator could get a fee for its service. 

#### Option 3: Matching Account (Specialized Account)

We have a single account, as in Option 2 above, but anyone can execute transactions against these accounts. This would mean that block producers would play a role of a "temporary exchange operator" (for the block they produce). 

This option might be the most complex for a PoC. 

## Building a Privacy-Preserving Order Book

Privacy is actually a core feature of Polygon Miden, and it has many layers. The actual note that represents the order in this naive design must be stored publicly and cannot be private. However, users can easily use a proxy account and execute transactions locally against it to forward private orders or iceberg orders. 

Let's reiterate (more details can be found [here](https://polygon.technology/blog/polygon-miden-transaction-model-2) and [here](https://polygon.technology/blog/polygon-miden-state-model?utm_source=twitter&utm_medium=social&utm_content=miden-state-model)): On Miden transactions can be private when executed locally. Then the operator and all other users only see a commitment of the note that was created in a transaction. This note can also be consumed locally, and no one would know that a nullifier for this note exists but the users who have the full note data. 

So to make a private order, we need the following flow (I am sure this can be optimized in several ways, but it's a PoC):

![image](https://github.com/0xPolygonMiden/examples/assets/35031754/43df9c67-20ad-4e0c-a684-d0b24f27b4a5)

- We introduce a public account / smart contract that we call `Obfuscator`. Anyone can execute transactions against that account. 
- **tx 1:** is executed by the user locally. The transaction produces `Note 1` (private), and one branch of the note's script is the return address, which we will explain in more detail below.
- **tx 2:** is executed locally by the user against the `Obfuscator` account. The transaction consumes `Note 1` and creates `Note 2` (public). `Note 2` is public and can be displayed by the DEX front end. The sender of `Note 2` is now `Obfuscator`, but it basically copies `Note 1`, including the part of the script with the return address.
- **tx 3:** is the actual swap, and this must be a network transaction. It is executed by the DEX Operator. It consumes `Note 2` and produces `Note 3` (private). The private `Note 3` carries the required DAI that the user wanted and can only be consumed by that user. 

There can be several ways to ensure that `Note 3` can only be consumed by the initiator of the trade while at the same time hiding the corresponding Account ID from everyone else. One easy way is to encode in `Note 1` a secret that needs to be provided when `Note 3` is consumed. Basically, `Note 1` has a condition that reads like "Whenever you copy that note, also copy the condition: whenever you consume this note, you need to create another note carrying 100 DAI that can only be consumed by whoever knows `x` such that `f(x) = a`."

And for iceberg orders, there can be an `Iceberg Obfuscator` that breaks down large orders into several smaller ones. 

Happy to discuss it; it should work, but maybe I am missing something.

## Additional Thoughts and Questions

### Cancellation Censorship to Pick Off Stale Orders

So for our on-chain order book exchange PoC, it could happen that the Operator would censor a certain to prevent him/her from canceling the order. This could be "rational" if there is a high misplaced order that the Operator wants to take itself. 

So when orders are being obfuscated - as explained in Privacy above - the Operator might not know which user to block. However, if the Operator is also the Miden Operator, it could censor anyone who wants to consume a particular note that represents the stale order. 

This could be prevented by having a built-in order expiry. So, every note representing an order could expire after 10 minutes in a way that it can only be consumed by the creator of the note. The 10 minutes can be any arbitrary number.

### Race Conditions for Settlement

This is indeed an interesting point. A solution depends on the implementation of the DEX account. If there is no DEX account and users would pick the notes to consume - or an algorithm running locally for the user - then there are a lot of race conditions for settlement. However, if there is a DEX account, then it depends on the actual implementation and the interface between the frontend and the Miden chain. 

That is something to think of when implementing.

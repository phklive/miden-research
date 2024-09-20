# Private Perpetual Market

<!-- add header image -->

## Perpetual Markets: A Powerful DeFi Application

Perpetual Protocol is a decentralized finance (DeFi) platform that allows users to trade cryptocurrencies with leverage via **perpetual futures**. Perpetual futures are a type of financial derivative that allows traders to speculate on the future price of an asset without an expiration date. Unlike traditional futures contracts, perpetual futures do not have a set settlement date, enabling positions to be held indefinitely as long as the trader can maintain the necessary **margin**. 

They often use a **funding rate mechanism** to keep the market price close to the underlying spot price, balancing the long and short positions in the market. This funding rate is periodically exchanged between long and short traders, based on market conditions. Perpetual futures are popular in the cryptocurrency market (e.g., dYdX), providing high leverage and the flexibility to go long or short, thereby offering opportunities for profit in both rising and falling markets.

Perpetual markets can leverage Miden's unique feature set to achieve:
- Trustless, decentralized perpetual trading
- High throughput - hundreds of deposits and withdrawals per transaction
- Privacy against competitors
- Provable compliance 

## A Private Perpetual Market Built on Miden

Miden accounts are smart contracts. Every perpetual contract (perp) can be represented by a Miden Account. For example, there can be a perp tracking the BTC price. The account can be private or public. (*This needs to be decided, there are pros and cons*)

<p align="center">
    <img src="https://hackmd.io/_uploads/B10KczrFa.png" style="width: 50%;">
</p>

Users can now bet on an increasing or decreasing BTC price by depositing into this Miden Account (sending a note). Their deposit is called margin.

<p align="center">
    <img src="https://hackmd.io/_uploads/HknljfrtT.png" style="width: 50%;">
</p>

In this example, Alice profits from an increasing BTC price and Bob vice versa. 

Next, every time period, e.g., 30 min, an oracle provides the current BTC price. In a **transaction**, the perp changes its state according to the new price and in doing so shifts funds according to the funding rate either from Alice to Bob or vice versa. If a participant runs out of funds, the position will be closed (or is there a margin call?)

![image](https://hackmd.io/_uploads/BJI0oGSF6.png)

The transaction can be local or a network transaction. Local transactions executed by the market operator are cheaper.

Now, this could be built on every network. It might be a bit cheaper on Miden. 

However, if many users at the same time try to enter and leave a perp, in Miden this can be done in a single transaction. 

![image](https://hackmd.io/_uploads/ByqZTfSFp.png)

If this single transaction is executed locally by the market operator it can be incredibly cheap and in only a few seconds (maybe less depending on the machine). 

On Miden, a perp market can serve hundreds of users at the same time in a decentralized and secure way. 

## Additional Thoughts

In theory, the perp can be a private account. That means the network only tracks a commitment to the account, i.e., a hash (0x1234). 

Perp management can then be done by the market operator and users can open, enter, leave and close perps using the operator's infrastructure and frontend. 

That means, every perp would be its own L3 validium on Miden, which can be incredibly cheap, fast and with the right measures private and secure.

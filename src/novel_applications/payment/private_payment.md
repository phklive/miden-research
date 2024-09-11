# Private Payment

<!-- TODO: Add image -->

## The why?

### Introduction

### The Importance of Payments

Efficient payments are a backbone of human societies. They enable exchange of value for goods and services, facilitate social functioning (participating in activities e.g. going to the restaurant), and support economic growth; without them, commerce would grind to a halt, financial stability would be undermined, and access to essential services would be severely limited, potentially leading to economic stagnation and social disruption.

### Blockchains as a Payments Infrastructure

Since the very first blockchain, Bitcoin, payments have been the flagship use case. With further developments like Ethereum and alternative L1's (Polygon, Tron, Near) and innovations like rollups (Base, Starknet, Scroll), payments have been made faster, cheaper, and more powerful (with additional functionalities e.g. through smart contracts).

| Blockchain | Bitcoin | Ethereum | Polygon | Base |
|------------|---------|----------|---------|------|
| Avg. tx fee ($) | 0.68 | 1.85 | 0.003 | 0.00005 |
| Cost | High | High | Low | Virtually free |
| Speed | Low | Low | High | High | 
| UX | Poor | Poor | Poor | Average |
| Network | Decentralized | Decentralized | Less decentralized | Centralized |
| Privacy | None | None | None | None |

<!-- TODO: Add measurements of speed -->

Nonetheless, for years blockchain payments have remained expensive (especially at the base layers e.g. Bitcoin, Ethereum), complex to make due to bad UX, and lack essential functionalities like privacy. This has prevented blockchains from acquiring the role of leading payment infrastructure and forced users to continue relying on classical centralized infrastructures (e.g. Visa, Mastercard, PayPal, Venmo, Revolut, Banks, etc.).

<!-- TODO: Add example of Tron as no.1 example of need of stable payments on chain -->

### The Importance of Privacy in Payments

Privacy in payment systems is crucial for protecting individual freedoms, maintaining financial autonomy, and ensuring the healthy functioning of economies. Different payment infrastructures offer varying levels of privacy:

1. Traditional Banking Systems:
   - Users have privacy from other users
   - No privacy from the bank/operator
   - No privacy from the government

2. Current Decentralized Systems (e.g., public blockchains):
   - No privacy from other users (transactions are visible)
   - No privacy from network operators/validators
   - Pseudonymity ≠ Privacy (transactions can often be linked to identities)

3. Implications of Limited Privacy:
   - Chilling effect on transactions: Users may avoid certain purchases or donations
   - Financial surveillance: Governments or corporations can track spending habits
   - Discrimination: Transaction history could be used for unfair treatment
   - Security risks: Visible wealth or spending patterns may attract criminals

This lack of privacy hinders the freedom of users, who may restrain from transacting because of the trace they could leave in the system.

4. Ideal Privacy in Payments:
   - Transactional details visible only to involved parties
   - Protection against both external observers and system operators
   - Ability to selectively disclose information for regulatory compliance

Privacy-preserving payment systems empower users to transact freely without fear of surveillance or judgment, fostering a more open and innovative economy.

### What about current private decentralized systems?

We already have existing decentralized privacy preserving protocols as of time of writing, being [Zcash](https://z.cash) and [Monero](https://getmonero.org), hence why would we need other systems?:

1. Lack of stable assets

Zcash and Monero respectively and exclusively use the `ZEC` and `XMR` tokens meaning that widely used stablecoins like `USDC`, `USDT` and others can't be ported and used on such blockchains. The lack of stable assets pegged to an existing currency hinders the ability of users to pay and get paid for "real world" goods and services, hence preventing those systems from becoming widely adopted payment solutions and replacing current payments infrastructure.

2. Expensive payments

The average transaction fee of these blockchains as of time of writing is:

- On Zcash: $0.023
- On Monero: $0.061

Although being a few cents these transaction fees are still too high by orders of magnitude to compete with classical payment systems or existing blockchain rollups, pushing the average user to transact on those systems.

3. Lack of programmability

Zcash and Monero are blockchains focused on payments, they do not provide a virtual machine enabling computation like Ethereum. This lack of programmability prevents the innovations that we have seen throuhgout the years with the development of smart contracts and other protocols making payments more efficient and powerful.

4. Bad user experience

Most payments for general purpose blockchains must be made through browser wallets or cold wallet interfaces, which do not provide a compelling user interface and experience compared to existing banking apps like Venmo, Paypal or Revolut. It is nonetheless important to note that attempts have been made e.g. [Zashi](https://z.cash/ecosystem/zashi-wallet/).

<!-- TODO: Add lack of interoperability -->
<!-- TODO: Add the fact that most privacy preserving coins can't be listed / exited on / from exchanges -->

I believe that Miden can solve these issues, let me show you how.

## The how?

### Miden an improved payments infrastructure

Given the limitations of existing blockchains and payments infrastructure, both public and privacy-focused, there is a clear need for a solution that combines privacy, efficiency and functionality. This is where Miden enters the picture, by building on the foundational work around privacy of projects like Zcash and Monero and around computation of a project like Ethereum, while also incorporating lessons learned from scalable solutions like rollups, Miden presents a promising approach towards building a more ideal payments infrastructure.

The Miden protocol gives us the best of all worlds by addressing each of the key issues identified in current systems: 

| Aspect | Current Systems | Miden |
|--------|-----------------|-------|
| Network Structure | Centralized | Decentralized (On the roadmap) |
| Asset Support | Lack of stable assets | Arbitrary assets |
| Programmability | Lack of programmability | Turing complete VM |
| Privacy | Public | Private |
| Cost | Expensive | Cheap |
| Speed | Slow | Fast |
| User Experience | Poor UX | Abstracted UX (Applications) |

### The Miden protocol

#### Peer-to-peer private transfers

Miden enables users to perform peer-to-peer private transfers using local execution and local proving, here is an example of it step by step:

We agree on the following initial state: 

- There are 2 users Alice and Bob
- Alice owns 1 Ether, Bob owns no assets
- Alice wants to send her 1 Ether to Bob
- Alice and Bob want to remain private

To do so the following scheme can be applied: 

1. Alice transitions her state from a state `S` where she has `1 Ether` to a state `S'` where she has `0 Ether` transferring the asset into a `Note` using the [P2ID script](https://github.com/0xPolygonMiden/miden-base/blob/main/miden-lib/asm/note_scripts/P2ID.masm).
 
2. Alice transfers this `Note` containing the asset to Bob using arbitrary solutions (On-chain encrypted notes (on the roadmap), Telegram, Signal, etc.)

3. Bob receives the `Note` and transitions his state from `S` where he has `0 Ether` to a state `S'` where he has `1 Ether` consuming the `1 Ether` placed into the `Note` by Alice into his state.

<!-- TODO: Add image -->

#### Account abstraction

Ethereum has set the standard for accounts in the VM enabled blockchain world. There are two types of accounts in Ethereum:

- Externally owned accounts (EOA): are created by generating a public/private ECDSA key pair. Does not hold code. can initiate transactions.
- Contract accounts (Smart contracts): are created by being deployed on the Ethereum blockchain by an EOA. Holds code which can be executed. can't initiate transactions.

We clearly understand here that Ethereum has made the choice to separate user accounts from executable code. What if we could merge both to enable programmable user accounts? Welcoming `Account abstraction`, which can be defined as follows:

"Account abstraction is a method of setting up a blockchain network in which users’ assets are stored exclusively in smart contracts, and not in external accounts (External Owned Accounts, EOAs). When using this approach, a crypto wallet turns into a unique smart contract that can be programmed for various purposes."

[What is account abstraction and why is it important - Medium - Aleksander](https://medium.com/@alex-100/what-is-account-abstraction-and-why-is-it-important-9627a4ced4f3)

Miden supports full account abstraction enabling full programmability of user accounts defined as follows: 

<!-- TODO: Reduce the size of the image -->
![Account definition](../../assets/images/account.png)

The `Miden VM code` field hints that all Miden accounts are `abstracted` which enables arbitrary logic to be executed against them, opening the door to unbounded functionalities. Using this innovations we can imagine improvements in UX like social recovery, abstracted seed phrases, Face-ID signing, and many more.

#### Privacy scales better

<!-- TODO: finish section -->

#### Conclusion

In this section we covered privacy through [peer-to-peer private transfers](#peer-to-peer-private-transfers), improvements in UX through [account abstraction](#account-abstraction) and lastly fast and cheap transactions through [privacy enabling better scaling](#privacy-scales-better).

## The vision

### A powerful payment application on Miden

Leveraging the Miden primitives mentioned above we can imagine a powerful payment application that would combine privacy, fast and cheap payments, a stunning user interface, great user experience while being built on top of a fully permissionless and decentralized infrastructure, Miden.

#### The frontend

##### User interface

Taking inspiration on successful current payments apps like Venmo or Revolut we prioritise simplicity and cleanliness for the interface.

##### User experience

Once again taking inspiration from successful payments applications we want to simplify the onboarding flow enabling users with any technical abilities to use our service. To do so we will go away what has been done in classical blockchain systems and abstract seed phrases for the users through the secure element of their device.

#### The backend

##### Secure enclave and Keystore

##### The relayer

##### What about fees?

#### To note

### Conclusion

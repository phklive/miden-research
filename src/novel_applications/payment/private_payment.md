# Private Payment

![Private payment header image](../../assets/images/private_payment.png)

## Blockchains as Payment Infrastructure

Efficient payments are the economic backbone of human societies. They enable the exchange of value for goods and services, facilitate social functioning (e.g., participating in activities like dining out), and support financial interactions.

Since the inception of Bitcoin, the first blockchain, payments have been the flagship use case. With further developments like Ethereum, alternative L1s (Polygon, Tron, Celo, BNB), and innovations like rollups (Base, Starknet, Scroll), payments have become faster and cheaper.

The lowered transaction fees offered by these alternative chains make them more suitable for routine, person-to-person payments and small-scale commercial transactions. This "real-world" adoption is evident from [data on stablecoin usage across these chains](https://app.artemisanalytics.com/stablecoins?chain=all&stablecoinMetric=STABLECOIN_MC). The term "real world" in this context refers primarily to user-to-user transfers, as opposed to institutional transactions. This interpretation is supported by the notably lower average transaction sizes compared to Ethereum (~$15 vs ~$100k).

While blockchains have vastly improved through the introduction of alternative layer-1 and layer-2 solutions, three critical issues continue to prevent mass adoption:

1. **scalability**: Despite technological advancements, many blockchains remain to slow and expensive to use at scale.

2. **Privacy**: Most blockchain networks still operate on a public ledger system, where all transactions are visible and traceable. The lack of built-in privacy features means that financial transactions lack the confidentiality that users expect from financial systems. This transparency, while beneficial for some use cases, poses significant concerns for personal and business transactions that require discretion.

3. **User Experience (UI/UX)**: Blockchain applications still suffer from poor UI/UX design. The complexity of interacting with blockchain systems remains a significant barrier for non-technical users, often involving confusing wallet interfaces, complicated address systems, sensitive seed phrases and unintuitive transaction processes.

These issues have prevented blockchains from acquiring the role of leading payment infrastructure and forced users to continue relying on classical centralized infrastructures (e.g., Visa, Mastercard, PayPal, Venmo, Revolut, banks, etc.).

This raises the question: *What about current private decentralized systems?*

While decentralized privacy-preserving protocols like [Zcash](https://z.cash) and [Monero](https://getmonero.org) exist, they face significant limitations. These systems lack stable assets, exclusively using their native tokens (ZEC and XMR), which prevents the use of widely adopted stablecoins like USDC or USDT. This absence of stable, pegged currencies hinders real-world transactions and broad adoption. Additionally, their transaction fees (Zcash: $0.023, Monero: $0.061), while seemingly low, are still too high to compete effectively with traditional payment systems or newer blockchain solutions.

Miden solves these issues.

## Miden: an improved payment infrastructure

Given the limitations of existing blockchains and payments infrastructure, both public and privacy-focused, there is a clear need for a solution that combines scalability, privacy, and functionality. This is where Miden enters the picture. By building on the foundational work around privacy of projects like Zcash and Monero and around computation of a project like Ethereum, while also incorporating lessons learned from scalable solutions like rollups, Miden presents a promising approach towards building a more ideal payments infrastructure:

| Aspect | Current Systems | Miden |
|--------|-----------------|-------|
| Network Structure | Centralized | Decentralized (On the roadmap) |
| Asset Support | Lack of stable assets | Arbitrary assets |
| Privacy | Public | Private |
| Cost | Expensive | Cheap |
| Speed | Slow | Fast |
| User Experience | Poor UX | Abstracted UX (Applications) |

The Miden protocol gives us the best of all worlds by addressing each of the key issues identified in current systems.

## Payments using the Miden protocol

Miden enables users to perform [peer-to-peer private transfers](https://polygon.technology/blog/polygon-miden-transaction-model-2) using local execution and local proving. Here is an example of it step by step:

We agree on the following initial state:

- There are 2 users, Alice and Bob
- Alice owns 1 Ether, Bob owns no assets
- Alice wants to send 1 Ether to Bob
- Alice and Bob want to remain private

To do so, the following scheme can be applied:

1. Alice transitions her state from a state `S` where she has `1 Ether` to a state `S'` where she has `0 Ether`, transferring the asset into a `Note` using the [P2ID script](https://github.com/0xPolygonMiden/miden-base/blob/main/miden-lib/asm/note_scripts/P2ID.masm).

2. Alice transfers this `Note` containing the asset to Bob using arbitrary solutions (Signal, E-mail, On-chain encrypted notes (on the roadmap)).

3. Bob receives the `Note` and transitions his state from `S` where he has `0 Ether` to a state `S'` where he has `1 Ether`, consuming the `1 Ether` placed into the `Note` by Alice into his state.

## A powerful payment application on Miden

Leveraging the Miden primitives mentioned above, we can imagine a powerful payment application that would combine privacy, fast and cheap payments, a stunning user interface, great user experience while being built on top of a fully permissionless and decentralized infrastructure, Miden. The following features would be required:

- A modern, simple and clean UI
- Ability to make transfers in a fast, cheap and privacy preserving way
- Ability to create accounts with a simple onboarding flow (abstracted seed phrases/UX)
- Built on top of Miden

Taking inspiration from successful payment applications (e.i. Venmo or Revolut), we would prioritize simplicity and cleanliness for the interface, making the application approachable for users of any technical level. We can imagine a frontend that would aggregate all user assets and transfers, display a portfolio value and enable transfers:

<p align="center">
    <img src="../../assets/images/payment_application.png" width="40%" alt="Imagined payment app">
</p>

We could imagine additional features which could be added to this type of application:

- Simple DeFi page, aggregating best sources of yield / lending / borrowing from different Miden protocols
- Phone-to-phone tap payment
- Creation of digital bank cards and integration with Apple / Google Pay
- Creation of physical bank cards (Gnosis card, Crypto.com card, etc.)

## Conclusion

Miden represents a significant leap forward in blockchain-based payment systems, addressing key limitations of existing solutions. By combining privacy, programmability, and scalability, it opens the door to user-friendly applications that could revolutionize how we transact, as presented above.

## Additional thoughts and questions

#### A better UX on Miden: Account abstraction

*Question: Why would a user have a better UX on Miden than on another blockchain?*

Ethereum has set the standard for accounts in the VM-enabled blockchain world. There are two types of accounts in Ethereum:

- Externally owned accounts (EOA): are created by generating a public/private ECDSA key pair. Does not hold code. Can initiate transactions.
- Contract accounts (Smart contracts): are created by being deployed on the Ethereum blockchain by an EOA. Holds code which can be executed. Can't initiate transactions.

We clearly understand here that Ethereum has made the choice to separate user accounts from executable code. What if we could merge both to enable programmable user accounts? Welcoming `Account abstraction`, which can be defined as follows:

"Account abstraction is a method of setting up a blockchain network in which users' assets are stored exclusively in smart contracts, and not in external accounts (External Owned Accounts, EOAs). When using this approach, a crypto wallet turns into a unique smart contract that can be programmed for various purposes."

[What is account abstraction and why is it important - Medium - Aleksander](https://medium.com/@alex-100/what-is-account-abstraction-and-why-is-it-important-9627a4ced4f3)

Miden supports full account abstraction, enabling full programmability of user [accounts](https://docs.polygon.technology/miden/miden-base/architecture/accounts/).

The `Miden VM code` field hints that all Miden accounts are `abstracted`, which enables arbitrary logic to be executed against them, opening the door to unbounded functionalities. Using these innovations, we can imagine improvements in UX like social recovery, abstracted seed phrases, Face-ID signing, and many more.

#### Making private transfers

*Question: How can the application developer use Miden to make private transfers?*

Using the [Miden SDK](https://www.npmjs.com/package/@demox-labs/miden-sdk?activeTab=readme) or the [Miden client](https://github.com/0xPolygonMiden/miden-client), an application developer can import Miden core components into their application, enabling them to create accounts, execute code against their accounts to transition their state (which we call a [transaction](https://docs.polygon.technology/miden/miden-base/architecture/transactions/overview/)), generate notes, and more.

#### Account storage

*Question: Where and how would I store my account?*

The user account, assets and private key would be stored securely on device. For additional redundancy, we could add social recovery solutions, storage of an encrypted account state in the cloud, and more.

#### Secure enclave and Keystore

*Question: I don't really know what a private key is and it seems too important for me to store it safely, how can I do it?*

To make the user onboarding seamless, we would want to abstract away seed phrases and complex blockchain security measures. We could achieve this using the secure element of modern phones which stores key-pairs in trusted execution environments (TEEs) enabling signatures. They are currently used for face or finger recognition, login, WebAuthn, payment and more:

- [Secure enclave](https://support.apple.com/en-gb/guide/security/sec59b0b31ff/web) for Apple phones
- [Keystore](https://developer.android.com/privacy-and-security/keystore) for Android phones

To leverage these secure elements in Miden, we would need to implement signature verification for their supported signature schemes. A signature scheme supported by both of these secure elements is [ECDSA](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm). [ECDSA signature verification](https://github.com/0xPolygonMiden/miden-vm/blob/4923e3d69622e6b8a5d91fab2949cb83845cf134/stdlib/tests/crypto/ecdsa_secp256k1.rs) support on Miden would enable users to sign Miden transactions using their phones' face / finger recognition, password, etc., making the onboarding flow simpler.

#### The relayer

*Question: Considering that the transactions are made locally by the users, how would notes be handled and delivered to the recipients?*

Relaying could be handled in different ways:

- Off-chain relaying: the application or external actors could provide relaying services for the notes of the users (losing privacy against the relayer)
- On-chain relaying: the users could use on-chain encrypted notes to interact with other users (on the roadmap)

The first method could be assimilated with a `PUSH` scheme where the source would send their notes to the relayer and the relayer would send those notes to the intended target. The second method could be assimilated as a `PULL` scheme where the source would encrypt notes and send them to the Miden rollup. The application would then need to filter existing notes from the rollup notes tree and consume relevant notes for the target user (which could be done on action or time basis).

#### Miden name service

*Question: How would I easily find and interact with my contacts if addresses are a bunch of random characters starting with `0x`?*

To simplify the addition and management of contacts and make the experience more recognizable by web2 users, we could improve the address book of the application by leveraging [Miden name service](../identity/miden_name_service.md). A name service is essential for users to easily find, send and remember contacts (We do not store phone numbers as numbers in our phones, we store them as names, attached is the phone number e.g. Mom -> +123456789).

#### Users need to sign a transaction for each action

*Question: Wouldn't I have a bad experience needing to sign each and every action I make inside the application?*

Most web3 apps forced their users to sign a transaction for each of their actions because each state update of a blockchain requires a valid account signature. We can solve this using these two solutions:

- Batching of user actions: User actions can be batched and do not need to be sent on each action.
- Hybrid web2 / web3 apps: Not everything needs to live on-chain, the application developer can handle some of the actions on a classic web2 backend, while the users keep sovereignty over their assets and data in a web3 way.

#### What about fees?

*Question: Wouldn't I need to pay a lot in fees for each transaction?*

Any digital system incurs costs, be it centralized or decentralized. The cost of the material, running the software, employees, offices, etc. The default behavior for web2 applications is to subsidize these costs for their users, making the use of the service free and finding other ways to make profit. Current blockchains (web3) impose gas fees on their users relative to their computational use of the system. This payment of fees on each action has made the user experience and the cost to use blockchain systems higher than their web2 counterparts, hindering the adoption of blockchain-based applications.

Miden's account abstraction solves this by enabling application developers to subsidize gas costs for their users through a [paymaster](https://www.stackup.sh/blog/what-are-paymasters) scheme. Furthermore, the cost incurred by fees on Miden would be small; thanks to [private scaling](#privacy-scales-better).

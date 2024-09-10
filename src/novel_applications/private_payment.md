# Private Payments

## Introduction

### The Importance of Payments

Efficient payments are a backbone of human societies. They enable exchange of value for goods and services, facilitate social functioning (participating in activities e.g. going to the restaurant), and support economic growth; without them, commerce would grind to a halt, financial stability would be undermined, and access to essential services would be severely limited, potentially leading to economic stagnation and social disruption.

### Blockchains as a Payments Infrastructure

Since the very first blockchain, Bitcoin, payments have been the flagship use case. With further developments like Ethereum and alternative L1's (Polygon, Tron, Near) and innovations like rollups (Base, Starknet, Scroll), payments have been made faster, cheaper, and more powerful (with additional functionalities e.g. through smart contracts).

| Blockchain | Bitcoin | Ethereum | Polygon | Base |
|------------|---------|----------|---------|------|
| Avg. tx fee ($) | 0.68 | 1.85 | 0.003 | 0.00005 |
| Cost | High | High | Low | Virtually free |
| UX | Bad | Bad | Bad | Average |
| Network | Decentralized | Decentralized | Less decentralized | Centralized |
| Privacy | None | None | None | None |

Nonetheless, for years blockchain payments have remained expensive (especially at the base layers e.g. Bitcoin, Ethereum), complex to make due to bad UX, and lack essential functionalities like privacy. This has prevented blockchains from acquiring the role of leading payment infrastructure and forced users to continue relying on classical centralized infrastructures (e.g. Visa, Mastercard, PayPal, Venmo, Revolut, Banks, etc.).

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

I believe that Miden can solve these issues, let me show you how.

## Miden an improved payments infrastructure

Given the limitations of existing blockchains and payments infrastructure, both public and privacy-focused, there is a clear need for a solution that combines privacy, efficiency and functionality. This is where Miden enters the picture.

In the following sections we'll explore how the Miden protocol addresses each of the key issues identified in current systems and how it could be used to build a better future.

### One
### Two
### Three

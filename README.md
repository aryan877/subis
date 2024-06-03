
#  Subis - Decentralized, Gasless Subscription Management

Subis is a decentralized subscription management solution built on zkSync, utilizing Native Account Abstraction supported by zkSync and Chainlink Price Feeds. It aims to provide a permissionless and gasless alternative to centralized financial services, which often charge high fees and may block accounts.

## Project Structure

The project is organized into the following folder structure:

```
subis/
  ├── app/ (separate GitHub repo)
  ├── contracts/ (separate GitHub repo)
  ├── cron/ (separate GitHub repo)
  └── subscription-api-usage-example/ (separate GitHub repo)
```

Each folder represents a separate GitHub repository that should be cloned locally to set up the project.

## Key Features

### Subscription Manager

Subis provides a Subscription Manager contract that allows subscription providers to manage their subscription plans and subscriber accounts. The Subscription Manager contract offers functionality such as creating and managing subscription plans, subscribing users to plans, and automatically charging subscriber accounts based on the subscription terms.

The Subscription Manager contract utilizes Chainlink Price Feeds to fetch the latest ETH/USD price, enabling the conversion between USD and ETH for subscription fees. This ensures that subscribers are charged the correct amount in ETH based on the USD price of the subscription plan.

### Smart Contract Accounts Leveraging the IAccount Interface from zkSync

Subis utilizes concept of Smart Accounts implemeted using the IAccount interface from zkSync, which are smart contract wallets created for each subscriber. These Smart Accounts are deployed using the Native Account Abstraction feature of zkSync, providing advanced functionality and flexibility.

Smart Accounts allow subscribers to set daily spending limits in USD, which are enforced by the contract. The spending limits are converted to ETH using the Chainlink Price Feed, ensuring that subscribers have control over their daily subscription expenses. These wallets can be charged automatically by the subscriber only when their subscription period expires.

### Gasless Subscriptions with zkSync Paymasters

Subis enables gasless subscriptions by leveraging the EIP-712 standard for signing typed data and the EIP-1271 standard for signature verification.

When a user wants to subscribe to a plan or perform any subscription-related action, they sign a structured message using the EIP-712 standard from the frontend. This signed message includes the necessary details such as the subscription plan ID, the user's address, and any additional parameters.

The signed message is then sent to the user's Smart Account contract, which implements the EIP-1271 standard. The Smart Account contract verifies the signature using the `isValidSignature` function defined by EIP-1271. If the signature is valid, the contract proceeds with executing the requested action, such as subscribing to a plan or updating the subscription status.

By utilizing EIP-712 for signing messages on the frontend and EIP-1271 for signature verification within the Smart Account contract, Subis eliminates the need for users to sign and submit transactions themselves. This enables gasless subscriptions, as the gas costs are covered by the Subscription Manager contract or a designated paymaster.

### Charge Wallet Functionality

Subis smart contract wallet deployed for the subscribers allows the subscription provider to directly charge the subscriber's Smart Account based on the subscription terms.

When a subscription payment is due, the Subscription Manager contract calls the `chargeWallet` function on the subscriber's Smart Account contract. This function verifies the subscription details, checks the spending limits, and transfers the required amount of ETH from the subscriber's Smart Account to the Subscription Manager contract.

The Charge Wallet functionality streamlines the subscription payment process and ensures a seamless experience for both the subscription provider and the subscriber. It eliminates the need for manual payment initiation by the subscriber, reducing friction and improving the overall user experience.

### Daily Spend Limits in USD

Subis empowers subscribers to set daily spend limits on their Smart Accounts in USD. By leveraging Chainlink Price Feeds, Subis fetches the real-time ETH/USD price and calculates the equivalent amount of ETH that can be spent daily.

Subscribers can easily manage their subscription expenses by setting and adjusting their daily spend limits through the Subis application. The Smart Account contract enforces these limits, ensuring that the subscriber's spending remains within their specified budget.

### Subscription Plan Prices in USD

Subscription providers can define their subscription plan prices in USD when creating plans using Subis. This brings predictability and stability to the pricing structure, as subscribers know exactly how much they will be charged in USD terms.

Subis utilizes Chainlink Price Feeds to fetch the current ETH/USD price and calculates the equivalent amount of ETH required for each subscription plan. This ensures that the correct amount of ETH is charged based on the USD price at the time of subscription.

### Flexible Subscription Management

Subis offers a range of subscription management features to provide flexibility and convenience for both subscribers and subscription providers. These features include:

- **Subscription Upgrade/Downgrade**: Subscribers can easily upgrade or downgrade their subscription plans based on their changing needs. Subis handles the proration of subscription fees, ensuring that subscribers are charged fairly for the remaining period of their current plan and the new plan they are switching to.

- **Subscription Cancellation**: Subscribers have the option to cancel their subscriptions at any time. Upon cancellation, Subis automatically stops future charges and notifies the subscription provider of the cancellation.

- **Subscription Renewal**: Subis supports automatic renewal of subscriptions based on the specified billing cycle. Subscribers can choose to enable or disable automatic renewal for their subscriptions.

- **Subscription Expiration**: When a subscription expires, Subis automatically marks the subscription as inactive and stops any further charges. Subscribers can choose to renew their subscription or let it expire.

### Subscription Analytics

Subis provides subscription providers with valuable insights and analytics related to their subscription plans and subscribers. The analytics data includes:

- **Subscriber Count**: The total number of active subscribers for each subscription plan.
- **Revenue**: The total revenue generated from each subscription plan and overall ( converted to ETH to USD using Chainlink price feeds ).

These analytics help subscription providers make data-driven decisions, optimize their pricing strategies, and improve their offerings based on subscriber behavior and preferences.

## Cron Job: Run Daily at 12:00 AM UTC to Automatically Charge All Expired Subscriptions

In the future, when Chainlink Automation is supported on zkSync, this process can be further decentralized and automated on-chain, eliminating the need for external cron jobs.

To ensure that subscribed users are charged regularly, subscription providers need to set up a cron job that runs the `chargeExpiredSubscriptions` function on their deployed Subscription Manager contract. This function should be called daily at a specific time (e.g., 12 PM UTC) to process the charges for expired subscriptions.

The cron job takes care of automatically charging subscribers based on their subscription terms and the current time. It retrieves the list of expired subscriptions and initiates the charging process for each subscriber's Smart Account.

## Subscription API Usage Example

Subis provides a flexible infrastructure for subscription providers to integrate subscription management into their own applications. Providers can deploy their own Subscription Manager contracts and utilize the contract addresses to verify user subscriptions and grant access to their services accordingly.

An example of how to use the Subis API to verify user subscriptions can be found in the `subscription-api-usage-example` folder. This example demonstrates how to sign a message using on the frontend and send it to a Next.js API, which verifies the signature and fetches the user's subscribed plan.

## Getting Started

To set up the project locally, follow these steps:

1. Clone the separate GitHub repositories for each folder into the respective directories within the `subis` folder:
   - `app`: `git clone https://github.com/aryan877/subis-app.git`
   - `contracts`: `git clone https://github.com/aryan877/subis-contracts.git`
   - `cron`: `git clone https://github.com/aryan877/subis-cron.git`
   - `subis-subscription-example-nextjs`: `git clone https://github.com/aryan877/subis-subscription-example-nextjs.git`

2. Follow the instructions provided in each repository's README file to set up and run the individual components.

## Deployment

The project is currently deployed on the zkSync Sepolia testnet. Here are the links to the deployed components:
- App: [https://subis-app.vercel.app/](https://subis-app.vercel.app/)

## Contribution

We welcome contributions to the Subis project. If you encounter any issues or have suggestions for improvements, please open an issue or submit a pull request in the respective GitHub repository.

## License

This project is licensed under the MIT License.

---

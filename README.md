
# Subis - Decentralized, Gasless Subscription Management

Subis is a decentralized subscription management solution built on zkSync, utilizing Native Account Abstraction supported by zkSync and Chainlink Price Feeds. It aims to provide a permissionless and gasless alternative to centralized financial services, which often charge high fees and may block accounts. This project is submitted to the Blockmagic Chainlink Hackathon.

## Project Structure

The project is organized into the following folders:

- [app](https://github.com/aryan877/subis-app): The Next.js frontend application that serves as a dashboard for both subscription providers and subscribers. Subscription providers can manage their subscription plans, view analytics, and monitor subscriber activity. Subscribers can explore available subscription plans, subscribe to plans, manage their subscriptions, set daily spending limits, and view their subscription history. The dashboard provides an intuitive and user-friendly interface for seamless subscription management.
- [contracts](https://github.com/aryan877/subis-contracts): The Hardhat project containing the smart contracts for Subis.
- [cron](https://github.com/aryan877/subis-cron): The cron job for automatically charging expired subscriptions.
- [subis-subscription-example-nextjs](https://github.com/aryan877/subis-subscription-example-nextjs): An example usage of the Subis API to verify user subscriptions.

```
subis/
  ├── app/
  │   ├── artifacts-zk/
  │   ├── interaces/
  │   ├── src/
  │   ├── public/
  │   ├── .env
  │   ├── .env.example
  │   ├── package.json
  │   └── ...
  ├── contracts/
  │   ├── artifacts-zk/
  │   ├── contracts/
  │   ├── deploy/
  │   ├── test/
  │   ├── .env
  │   ├── .env.example
  │   ├── hardhat.config.ts
  │   ├── package.json
  │   └── ...
  ├── cron/
  │   ├── artifacts-zk/
  │   ├── .env
  │   ├── .env.example
  │   ├── index.js
  │   ├── cron.log
  │   ├── package.json
  │   └── ...
  └── subis-subscription-example-nextjs/
      ├── artifacts-zk/
      │   ├── interaces/
      ├── src/
      ├── .env
      ├── .env.example
      ├── package.json
      └── ...
```

The `artifacts-zk/` directory is present in each folder and contains the compiled smart contract artifacts that are copied from the `contracts` folder after deployment. These artifacts are used by the frontend application, cron job, and subscription example to interact with the deployed smart contracts.

## Key Features

### Subscription Manager

Subis provides a Subscription Manager contract that allows subscription providers to manage their subscription plans and subscriber accounts. The Subscription Manager contract offers functionality such as creating and managing subscription plans, subscribing users to plans, and automatically charging subscriber accounts based on the subscription terms.

The Subscription Manager contract utilizes Chainlink Price Feeds to fetch the latest ETH/USD price, enabling the conversion between USD and ETH for subscription fees. This ensures that subscribers are charged the correct amount in ETH based on the USD price of the subscription plan.

### Smart Contract Accounts Leveraging the IAccount Interface from zkSync

Subis utilizes the concept of Smart Accounts implemented using the IAccount interface from zkSync, which are smart contract wallets created for each subscriber. These Smart Accounts are deployed using the Native Account Abstraction feature of zkSync, providing advanced functionality and flexibility.

Smart Accounts allow subscribers to set daily spending limits in USD, which are enforced by the contract. The spending limits are converted to ETH using the Chainlink Price Feed, ensuring that subscribers have control over their daily subscription expenses.

### Gasless Subscriptions with zkSync Paymasters and EIP-712 Signing

Subis enables gasless subscriptions by leveraging the EIP-712 standard for signing typed data on the frontend. When a user wants to subscribe to a plan or perform any subscription-related action, they sign a structured message using the EIP-712 standard from the frontend application. This signed message includes the necessary details such as the subscription plan ID, the user's address, and any additional parameters.

The signed message is then sent to the user's Smart Account contract, which verifies the signature and executes the requested action, such as subscribing to a plan or updating the subscription status. By utilizing EIP-712 for signing messages on the frontend, Subis eliminates the need for users to sign and submit transactions themselves. This enables gasless subscriptions, as the gas costs are covered by the Subscription Manager contract or a designated paymaster.

### Charge Wallet Functionality

Subis smart contract wallet deployed for the subscribers allows the subscription provider to directly charge the subscriber's Smart Account based on the subscription terms.

When a subscription payment is due, the Subscription Manager contract calls the `chargeWallet` function on the subscriber's Smart Account contract. This function verifies the subscription details, checks the spending limits, and transfers the required amount of ETH from the subscriber's Smart Account to the Subscription Manager contract.

The Charge Wallet functionality streamlines the subscription payment process and ensures a seamless experience for both the subscription provider and the subscriber. It eliminates the need for manual payment initiation by the subscriber, reducing friction and improving the overall user experience.

### Daily Spend Limits and Subscription Plan Prices in USD

Subis empowers subscribers to set daily spend limits on their Smart Accounts in USD. By leveraging Chainlink Price Feeds, Subis fetches the real-time ETH/USD price and calculates the equivalent amount of ETH that can be spent daily. Subscribers can easily manage their subscription expenses by setting and adjusting their daily spend limits through the Subis application.

Subscription providers can define their subscription plan prices in USD when creating plans using Subis. This brings predictability and stability to the pricing structure, as subscribers know exactly how much they will be charged in USD terms. Subis utilizes Chainlink Price Feeds to fetch the current ETH/USD price and calculates the equivalent amount of ETH required for each subscription plan.

### Flexible Subscription Management

Subis offers a range of subscription management features to provide flexibility and convenience for both subscribers and subscription providers. These features include:

- **Subscription Upgrade/Downgrade**: Subscribers can easily upgrade or downgrade their subscription plans based on their changing needs.
- **Subscription Cancellation**: Subscribers have the option to cancel their subscriptions at any time.
- **Subscription Renewal**: Subis supports automatic renewal of subscriptions based on the specified billing cycle.
- **Subscription Expiration**: When a subscription expires, Subis automatically marks the subscription as inactive and stops any further charges.

### Subscription Analytics

Subis provides subscription providers with valuable insights and analytics related to their subscription plans and subscribers. The analytics data includes:

- **Subscriber Count**: The total number of active subscribers for each subscription plan.
- **Revenue**: The total revenue generated from each subscription plan and overall, converted from ETH to USD using Chainlink Price Feeds.

These analytics help subscription providers make data-driven decisions, optimize their pricing strategies, and improve their offerings based on subscriber behavior and preferences.

## Getting Started

To set up the Subis project locally, follow these steps:

1. Clone the separate GitHub repositories for each folder into the respective directories within the `subis` folder:
   - `app`: `git clone https://github.com/aryan877/subis-app.git`
   - `contracts`: `git clone https://github.com/aryan877/subis-contracts.git`
   - `cron`: `git clone https://github.com/aryan877/subis-cron.git`
   - `subscription-api-usage-example`: `git clone https://github.com/aryan877/subis-subscription-example-nextjs.git`

2. Set up the `contracts` folder:
   - Navigate to the `contracts` folder: `cd contracts`
   - Install the required dependencies: `yarn install`
   - Create a .env file
   - Set up the environment variable `WALLET_PRIVATE_KEY` in the `.env` file with your wallet's private key and `PRICE_FEED_ADDRESS=0xfEefF7c3fB57d18C5C6Cdd71e45D2D0b4F9377bF` (ETH/USD price feed address on zkSync sepolia).
   - Ensure your wallet address (derived from WALLET_PRIVATE_KEY) is funded with test ETH on zkSync Sepolia to deploy the factory contracts.
   - Compile the contracts: `yarn hardhat:compile`
   - Compile the contracts: `yarn hardhat:compile`
   - Deploy the contracts: `yarn deploy`
   - The deployment script will automatically update the `.env` files in the root directory and the `app` directory with the deployed contract addresses.

3. Set up the `app` folder:
   - Navigate to the `app` folder: `cd ../app`
   - The factory contract addresses were be automatically updated in the `.env` file by the deployment script in `/contracts`.
   - Set up the environment variable `NEXT_PUBLIC_PRICE_FEED_ADDRESS=0xfEefF7c3fB57d18C5C6Cdd71e45D2D0b4F9377bF` (ETH/USD price feed address on zkSync Sepolia) in the `.env` file.
   - Install the required dependencies: `yarn install`
   - Start the Next.js development server: `yarn dev`
   - Access the Subis application at `http://localhost:3000`

4. Set up the `cron` folder:
   - Navigate to the `cron` folder: `cd ../cron`
   - Install the required dependencies: `yarn install`
   -  Add the environment variable `SUBSCRIPTION_MANAGER_ADDRESS` once it is deployed through the Next.js frontend in `/app`
   - Add the environment variable `WALLET_PRIVATE_KEY` (the owner wallet private key of the deployed Subscription Manager) to the `.env` file.
   - Start the cron server: `yarn start`
   - Hit the `/start` express endpoint to start the cron job for automatically charging expired subscriptions on 12:00:02 AM UTC daily.

5. Set up the `subscription-api-usage-example` folder:
   - Navigate to the `subscription-api-usage-example` folder: `cd ../subscription-api-usage-example`
   - Install the required dependencies: `yarn install`
   -  Add the environment variable `SUBSCRIPTION_MANAGER_ADDRESS` once it is deployed through Next.js frontend
   - Add the environment variable `AA_FACTORY_ADDRESS` ( deployed in step 2 ) to the `.env` file.
   - Start the Next.js development server: `yarn dev`
   - Access the example API usage at `http://localhost:3000`  which hits the api endpoint at `http://localhost:3000/api/verify-subscription` to verify message signature and retreive the user's subscription plan

## Deployment

The project is currently deployed on the zkSync Sepolia testnet. Here are the links to the deployed components:
- App: [https://subis-app.vercel.app/](https://subis-app.vercel.app/)

## Contribution

We welcome contributions to the Subis project. If you encounter any issues or have suggestions for improvements, please open an issue or submit a pull request in the respective GitHub repository.

## License

This project is licensed under the MIT License.

---

This README provides a comprehensive overview of the Subis project, including its key features, project structure, and setup instructions. It also includes deployment information, contribution guidelines, and the project's license. The GitHub links are provided for each component, along with the folder structure for a clear understanding of the project's organization.

Step 1: Create a Next.js Project

First, you’ll need to install Node.js. Once installed, execute the following command to create a basic Next.js scaffold:

copy the code below into your terminal in the project directory fam.

npx create-next-app@latest

you should see the message below:

Success! Created soltorial at /Users/8bit/Desktop/toot/soltorial

You will be prompted to provide details such as project name, TypeScript, ESLint, Tailwind CSS, and other configurations. For this guide, use the recommended default options, including the App Router.

1.2 Project Structure

After running the setup command, your project directory should look like this:
├── src
│   └── app
│       ├── favicon.ico
│       ├── globals.css
│       ├── layout.tsx
│       └── page.tsx
Step 2: Installing Solana Wallet Adapter

2.1 Install Required Dependencies

To allow your application’s users to connect their preferred Solana wallets, you’ll need to install the following Solana wallet adapter packages:

npm install @solana/web3.js \
    @solana/wallet-adapter-base \
    @solana/wallet-adapter-react \
    @solana/wallet-adapter-react-ui \
    @solana/wallet-adapter-wallets

    These packages provide the essential functions, hooks, and React components needed to integrate Solana wallets into your Next.js app.

Step 3: Setting Up Wallet Adapter in Your Next.js App

3.1 Create a components Folder

To keep your project organized, create a components folder inside the app directory and add a file named AppWalletProvider.tsx. This file will be responsible for setting up the wallet context for your application.

Project Structure Update:

├── src
│   └── app
│       ├── components
│       │   └── AppWalletProvider.tsx
│       ├── favicon.ico
│       ├── globals.css
│       ├── layout.tsx
│       └── page.tsx
3.2 Create AppWalletProvider.tsx

This file exports a context provider that allows all child components to access the Solana wallet adapter functionality. Ensure the file is rendered on the client side by adding the "use client" directive.

components/AppWalletProvider.tsx

"use client";

import React, { useMemo } from "react";
import { ConnectionProvider, WalletProvider } from "@solana/wallet-adapter-react";
import { WalletAdapterNetwork } from "@solana/wallet-adapter-base";
import { WalletModalProvider } from "@solana/wallet-adapter-react-ui";
import { clusterApiUrl } from "@solana/web3.js";
// import { UnsafeBurnerWalletAdapter } from "@solana/wallet-adapter-wallets";

// Import default styles for wallet adapter components
require("@solana/wallet-adapter-react-ui/styles.css");

export default function AppWalletProvider({ children }: { children: React.ReactNode }) {
  const network = WalletAdapterNetwork.Devnet;
  const endpoint = useMemo(() => clusterApiUrl(network), [network]);
  const wallets = useMemo(
    () => [
      // Add legacy wallet adapters here if needed
      // new UnsafeBurnerWalletAdapter(),
    ],
    [network]
  );

  return (
    <ConnectionProvider endpoint={endpoint}>
      <WalletProvider wallets={wallets} autoConnect>
        <WalletModalProvider>{children}</WalletModalProvider>
      </WalletProvider>
    </ConnectionProvider>
  );
}

3.3 Wrap Your Application with AppWalletProvider

To ensure wallet functionality is accessible throughout your application, wrap your Next.js app with AppWalletProvider in the root layout file.

src/app/layout.tsx

Step 4: Add a Connect Wallet Button

4.1 Implement the Connect Wallet Button

The “connect wallet” button allows users to open the wallet selection modal and connect their Solana wallet. For simplicity, we’ll add this button to the home page.

src/app/page.tsx

4.2 Run Your Development Server

After setting up the connect wallet button, start your development server:

Copy
npm run dev
Visit http://localhost:3000 to see your application in action.

Step 5: Using useWallet and useConnection Hooks

5.1 Introduction to Hooks

The useWallet and useConnection hooks allow your application to interact with the user’s Solana wallet and the Solana blockchain. These hooks are accessible to all components that are children of the AppWalletProvider context.

5.2 Example: Airdrop and Balance Check

Here’s an example of how to use these hooks to perform an airdrop and check the wallet’s balance:

src/app/address/page.tsx

Next Steps

You can expand your application by using the useWallet hook to trigger wallet actions like signing transactions or sending requests via the useConnection hook. For additional features, consider using the Unified Wallet Kit for advanced wallet integration.

To scaffold a new Solana dApp with pre-configured settings, run:

npx create-solana-dapp@latest
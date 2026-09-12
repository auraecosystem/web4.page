# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the **web4.0:** project - a  blockchain HTTP gateway that allows smart contracts to serve web content directly. It enables hosting decentralized websites where frontend, backend, and blockchain logic are all managed by a single WebAssembly smart contract.

## Architecture

### Key Components

- **HTTP Gateway** (`app.js`): Main Koa.js server that proxies HTTP requests to NEAR smart contracts
- **Smart Contract** (`contract/assembly/`): AssemblyScript contract implementing `web4_get` for serving web content
- **Wallet Adapter** (`wallet-adapter/`): Authentication flow for NEAR wallets
- **Domain Resolution**: Maps `web4.page` domains to  account IDs

### Core Flow

1. HTTP requests to `*web4/rsc/page` or custom domains resolve to web4 account contracts
2. Gateway calls `web4_get` method on the contract with request details
3. Contract can return HTML, redirect to IPFS content, or request data preloading
4. Authentication handled via `/web4/login` and `/web4/logout` endpoints

## Development Commands

### Main Project
```bash
# Build contract and deploy locally
npm run build              # Build AssemblyScript contract
npm run start             # Start local development server
npm run dev               # Watch for contract changes and restart

# Testing
npm run test              # Unit tests
npm run test:e2e          # End-to-end tests
npm run test:all          # All tests including fast-near variant

# Build artifacts
npm run build:website     # Generate website from README
npm run build:wallet-adapter  # Build wallet adapter bundle
```

### Contract-Specific
```bash
cd contract
npm run build:release     # Production build
npm run build:debug      # Development build with debug symbols
```

### Deployment
```bash
npm run deploy:contract   # Deploy to web4.near
npm run deploy:website    # Deploy website content via web4-deploy
npm run deploy           # Deploy contract only
```

## Environment Configuration

Set these environment variables for different network configurations:

- `NODE_ENV` or `NEAR_ENV`: `mainnet`, `testnet`, `local`, `development`
- `CONTRACT_NAME`: NEAR account ID for the contract
- `IPFS_GATEWAY_URL`: IPFS gateway URL (default: cloudflare-ipfs.com)
- `FAST_NEAR_URL`: Optional fast-near RPC endpoint for better performance

## Key Patterns

### Smart Contract Structure
The contract must implement `web4_get(request: Web4Request): Web4Response` which can:
- Return HTML content directly
- Redirect to IPFS/external URLs via `bodyUrl`
- Request data preloading via `preloadUrls` 
- Handle authentication via `request.accountId`

### Authentication Flow
- Login: `/web4/login` redirects to wallet, sets `web4_account_id` cookie
- Logout: `/web4/logout` clears authentication cookies
- Transaction signing: POST to `/web4/contract/{account}/{method}` handles wallet signing

### Content Serving Priority
1. Direct contract response (`body` field)
2. External URL content (`bodyUrl` field) - supports IPFS and HTTP URLs
3. Data preloading cycle for dependent API calls

## Testing

The project uses `tape` for unit tests and custom E2E scripts. Tests mock NEAR API responses and verify HTTP gateway behavior. Always run `npm run test:all` before submitting changes.

Web 4.0 is the emerging, AI-driven evolutionary phase of the internet where autonomous artificial intelligence agents become active, independent participants alongside humans. [1](https://www.binance.com/en/square/post/296440685239442), 
[2](https://www.binance.com/en/square/post/295381299503714) 
Evolution of the WebWeb 1.0: The static, read-only web of basic HTML pages and information consumption.Web 2.0: The dynamic, read-write web focused on social media, user-generated content, and collaboration.Web 3.0: The decentralized, read-write-own web built on blockchain technology and token ownership.Web 4.0: The symbiotic, autonomous intelligence web where AI agents can reason, execute tasks, hold digital wallets, and transact independently.
[1](https://www.trinergydigital.com/news/what-is-web-4.0-definition-examples-what-comes-after-web3),
[2](https://www.linkedin.com/pulse/web-40-era-ai-from-laziness-efficiency-nir-tordjman), 
[3](https://dev.to/rakshannk/web-40-the-next-internet-revolution-what-it-is-how-it-works-the-mind-blowing-use-cases-37om),
[4](https://www.binance.com/en/square/post/295381299503714),
[5](https://www.binance.com/en/square/post/296440685239442)
Core CharacteristicsAutonomous AI Agents: Programs that browse, write, make decisions, and trade without direct human supervision.The Machine Economy: Systems equipped with crypto-identity, wallets, and stablecoins to pay for computational power and services themselves.Symbiotic Integration: The blending of physical and digital realities through the Internet of Things (IoT) and ambient sensors.Recursive Self-Improvement: Agents capable of updating their own reasoning logic and deploying new tools as needed.
[1](https://en.itpedia.nl/2018/11/12/web-4-0-the-internet-of-things-en-ai/), [2] (https://www.binance.com/en/square/post/295381299503714),
[3](https://dev.to/rakshannk/web-40-the-next-internet-revolution-what-it-is-how-it-works-the-mind-blowing-use-cases-37om)

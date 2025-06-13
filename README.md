# Lotus Finance Docs

Lotus Finance is a decentralized market maker protocol built for high-efficiency DeFi infrastructure on the Sui blockchain. Leveraging the impressive performance, low-cost, and parallel capabilities of Sui alongside the state-of-the-art Deepbook orderbook DEX, a next generation DeFi tech stack is emerging. Yet it recognizes that the “last mile” of DeFi, market making, remains centralized. Our protocol aims to democratize this crucial part of the ecosystem and share its revenue with the community, opening up market making and quant trading opportunities previously reserved for institutional traders.

### Overview

* **Decentralized Market Maker**: Lotus Finance provides a fully on-chain market-making protocol with the flexibility of advanced trading strategies and yield farming incentives.
* **Integrated Infrastructure**: The protocol connects various modules including liquidity farming, yield aggregator, and decentralized exchange features with Deepbook’s matching engine.
* **Yield Farming & Incentives**: It incentivizes liquidity providers using both TVL-based APRs and metric-based rewards to share profits with the community.

### Key Features

1. **Decentralized Market Making on CLOB DEX**\
   Lotus Finance decentralizes the last mile of the DeFi stack by replacing centralized market makers with on-chain, community-driven liquidity strategies. This approach not only secure liquidity from Deepbook but also enables traditional high-frequency trading strategies in a decentralized protocol.
2. **Yield Farming and Incentive Mechanisms**
   * **TVL-Based Incentives**: Rewards liquidity providers proportionally to the total value locked in the vaults.
   * **Volume-Based Incentives**: Offers additional rewards based on trading volumes, driven by innovative ZK trade accounting techniques.
   * **Liquidity-Based Incentives**: Rewards market makers based on their liquidity performance metrics, such as spreads, slippage, and order book depth.
3. **Flexible Liquidity Provisioning**\
   The protocol supports multiple asset types and enables flexible allocation across farms and vaults. This ensures that liquidity provisioning is both capital-efficient and adaptable to rapid market changes.
4. **Advanced Risk Management and Quantitative Strategies**\
   Developed by a team of professional quants and seasoned TradFi/CEX market makers, Lotus Finance incorporates advanced risk management procedures and full-dimensional financial metric calculations, which are critical for robust market making.

### Technology Stack

* **Move Contracts & Deepbook Integration**:\
  The core on-chain logic is implemented in Sui Move, with key modules including:
  * Lotus LP Farm: Manages yield farming and TD (Time Distributor) farm operations. Incentives are topped up into `Farm` and distributed to `Vault`.
  * Lotus DB Vault: Handles deposit, withdrawal, order placement, and incentive reward collection for liquidity. Trades on Deepbook.
* **Oracle Aggregator & Price Feeds**:\
  Real-time asset pricing is provided through the integration with Pyth network, ensuring accurate USD value calculations for deposits, rewards.

### Architecture

#### Modules and Lifecycle

1. **LP Farm Creation**\
   A new LP farm is created where liquidity providers deposit assets (such as SUI) which are then paired with Deep tokens.
   * Farms are created and referenced throughout operations.
   * Farms are configured to accept only specific deposit assets and designated DB Pools available in Deepbook.
2. **Vault Management**
   * Liquidity is pooled into vaults managed through `Lotus DB Vault`, where tokens are deposited and orders are placed via Deepbook’s Move API.
   * Only authorized parties could securly interact with Vaults on deposits, withdrawals, and order executions.
3. **Incentivization Cycle**\
   Incentive mechanisms are triggered through a series of transactions showing operations such as topping up incentive balances, collecting rewards, and redeeming tokens.
4. **Order Placement and Management**\
   Execution of limit and market orders is handled by interactions between the DB Vault module and Deepbook’s order book, ensuring efficient matching and settlement of trades.

### Protocol Benefits & Ecosystem Impact

* **Enhanced Liquidity Security**: By decentralizing market making, Lotus Finance reduces reliance on centralized liquidity providers and mitigates associated risks.
* **Profit Sharing for the Community**: The protocol’s design democratizes market making, enabling community members to earn from both market-making revenues and advanced quant strategies.
* **Scalability and Capital Efficiency**: Lotus Finance’s modular design allows for high capital efficiency and scalability, laying the groundwork for supporting major asset markets, including non-native tokens like BTC and ETH.
* **Innovation in HFT and Quantitative Trading**: The integration of high-frequency and algorithmic trading strategies on-chain represents a pioneering step for decentralized finance on Sui.

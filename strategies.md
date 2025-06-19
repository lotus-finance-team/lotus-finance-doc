# Strategies

## Strategy and LP Risk - Lotus Finance

### Overview

Lotus Finance operates multiple liquidity provision strategies on a Centralized Limit Order Book (CLOB) Decentralized Exchange (DEX), including Fixed Grid Strategy and Pure Market Making (PMM) to optimize trading efficiency and liquidity provider (LP) returns. Each vault can choose different strategy with specific parameters. User's asset stored in the vault is used to trade on CLOB’s order-matching system with advanced market-making techniques traditionally seen in centralized exchanges, aiming to enhance capital efficiency of DEX, reduce slippage, and mitigate risks for LPs while maintaining competitive trading conditions.

### Strategy Description

#### Fixed Grid Strategy

**Mechanism:** The fixed grid strategy involves LPs placing limit orders in predefined price intervals (or "grids") across the trading pair’s price spectrum. Each grid level represents a discrete price point where buy and sell orders are automatically placed via smart contracts and trading engine. On order filling, Lotus Finance will automatically place new orders to maintain the grid, and each pair of buy and sell orders generates strategy yield for LPs when filled.

**Execution on CLOB:** Within the CLOB framework, these grid levels function as limit orders, matched against trader demand in real-time. The fixed nature of the grid simplifies LP participation, as they do not need to actively adjust ranges, and makes consistent yield for LP when paired orders are filled.

#### Pure Market Making (PMM)

**Mechanism:** PMM is most widely adopted and fundamental market making strategy for modern market makers on orderbook. Lotus Finance’s PMM dynamically adjusts liquidity around the current market price using internal and external price feeds (e.g., oracles). This proactive adjustment concentrates liquidity where trading activity is highest, enhancing efficiency beyond traditional constant product AMMs. By capturing mispricing opportunities and short time market inefficiencies, PMM helps LPs generate strategy yield.

**Execution on CLOB:** In the CLOB DEX, PMM operates as an algorithmic market maker, placing and canceling limit orders in response to market conditions. It shifts the liquidity distribution proactively in real-time, ensuring sufficient liquidity depth near the mid-price while minimizing exposure to inactive price ranges.

### Strategy Validation
Lotus Finance employs a comprehensive validation process for its liquidity provision strategies, ensuring they are robust, efficient, and aligned with market conditions. User created vaults are subject to criteria to receive incentives.
* **Strategy Parameters:** The strategy must stay "in range" of the current market price. For the fixed grid strategy, this means that the grid upper and lower bounds must encompass the current price. For PMM, the algorithm must maintain liquidity near the mid-price.
* **Liquidity Concentration:** The strategy must actively participate in trading, and maintain high capital efficiency. For fixed grid strategy this means the upper and lower bounds of the grid must be set to a reasonable range, and for PMM, the algorithm must adjust liquidity dynamically to capture trading opportunities. Vaults created by intentionally deviating from the parameters constrained by our client may still use Lotus Finance, however they may be blacklisted from receiving incentives.
  
### LP Risk Analysis

In the DeFi world, any kind of liquidity provision strategy comes with inherent risks. Liquidity providers on Lotus Finance also face a risk profile due to trading activities and holding positions when executing strategies within a CLOB DEX environment. Below are the primary risks and how they are addressed:

#### Market Risk and Depreciation Below Grid Lower Bound

**Risk Description:** When the token price drops below the lowest grid level, LP positions in that range may hold only one token (e.g., the base token), and the USD value of the position decreases with the token’s market price. This potentially amplifying losses if the price exits the grid entirely.

**Mitigation:** Lotus Finance informs LPs that positions outside the grid become inactive and cease earning fees, exposing them to market volatility. The PMM component partially offsets this by concentrating liquidity near the current price, reducing the likelihood of prolonged inactivity. LPs are encouraged to select grids aligned with long-term price expectations.

**Yield Impact:** Depreciation below the grid’s lower bound effects the vault total value and LP's total return.

#### Impermanent Loss (IL)

**Risk Description:** IL occurs when the relative price of pooled tokens shifts, causing LPs to lose value compared to holding assets outside the pool. The PMM strategy may subject LPs to IL when the market price deviates from the pool’s average price, as the algorithm adjusts liquidity to maintain a tight spread.

**Mitigation:** The PMM algorithm uses oracle-driven price adjustments to minimize IL by keeping liquidity near the market price, while other market participants tend to maintain efficiency of the market, since arbitrages are incentivized to correct the price in the public market. This would mitigate the IL risk for LPs.

**Yield Impact:** IL is included in yield calculations as a reduction in position value, offset by fees earned from active grid levels and PMM-adjusted orders.

#### Liquidity Inactivity Risk

**Risk Description:** In the fixed grid strategy, liquidity in grids far from the current price remains unused, earning no fees.

**Mitigation:** Like concentrated liquidity AMMs, where LPs can adjust ranges, the dynamic fixed grid strategies can also move liquidity into other ranges. However in naive fixed grid vaults (that are available in public beta), users need to close the vault and re-open the vault on new price range.

**Yield Impact:** Inactive liquidity lowers the effective fee yield, as only a portion of the LP’s capital generates returns at any given time.

#### Execution and Front-Running Risk

**Risk Description:** In a CLOB DEX, limit orders from the fixed grid and PMM are visible on-chain, potentially exposing LPs to front-running by high-frequency traders who exploit price movements before orders execute.

**Mitigation:** Strategies in Lotus Finance to some extent are insensitive to front-running, as the PMM strategy aims to capture short-term mispricing opportunities, which are less likely to be exploited by front-runners. The fixed grid strategy is less sensitive to front-running as well, as orders are placed in predefined ranges and levels which are in sensitive to temporal price movements and order executions.

**Yield Impact:** Successful front-running could reduce LP yield capture, though the strategies in Lotus Finance are by design less sensitive to this risk.

#### Smart Contract Risk

**Risk Description:** As a DeFi protocol, Lotus Finance relies on smart contracts for executing strategies on DEX, introducing risks of bugs or exploits that could lead to capital loss.

**Mitigation:** Contracts undergo rigorous audits. LPs are advised to review audit reports and monitor protocol updates.

**Yield Impact:** While not directly affecting yield calculations, a contract failure could result in total loss, a risk LPs must weigh independently.

### Yield Calculation and Risk Integration

The strategy yield for Lotus Finance LPs is calculated as:

**Total Yield = (Final Position Value + Strategy Yield + Cumulative Fees Earned + Incentives Earned) - Initial Investment**

* **Final Position Value:** Reflects market price changes, including depreciation below the grid’s lower bound and IL.
* **Cumulative Fees Earned:** Generated from trades within active grid levels and PMM-adjusted orders, proportional to the LP’s share of liquidity at each price point.
* **Incentives Earned:** Additional rewards from TVL-based APRs and volume-based incentives.
* **Initial Investment:** The capital deposited into the vault, calculated in USD value.

**Strategy Yield:** Generated from trades within active grid levels and PMM orders. Strategy yield APR is calculated based on strategy yield in the past time and linearly extrapolated to the future.

*   **Fixed Grid Strategy:** Yield is generated when paired orders are filled within the grid levels. Closed trade profit for the fixed grid strategy is the realized profit achieved when a filled buy order at one grid level is paired with a filled sell order at the adjacent higher grid level. In technical terms, if $$P_{\text{buy}}$$

    is the execution price of the buy order and $$P_{\text{sell}}$$ is the execution price of the sell order, and $$Q$$ is the order quantity, the closed trade profit $$CTP$$ can be defined as:\
    $$\text{CTP} = (P_{\text{sell}} - P_{\text{buy}}) \times Q$$
* This profit is considered “closed” because the paired orders lock in a profit, independent of subsequent market movements, and is then accumulated into the strategy’s overall yield.

### Protocol Fee

The protocol charges performance fee, a protocol fee charged on the positive yield generated by the LPs, and distributed to the protocol treasury and community fund. The fee is charged when LPs withdraw their capital from the vault. When LPs are incurring losses, the fee is not charged. This fee structure is designed to align the interests of LPs and the protocol, ensuring sustainable growth and community-driven development.

_For Mainnet launch, the fee is set at 5% (both protocol performance fee and strategy fee included, \~50% discount) to encourage participation and gather feedback from the community._

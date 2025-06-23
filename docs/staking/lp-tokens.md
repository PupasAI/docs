# LP Tokens

**Understanding Liquidity Provider tokens and yield generation through price appreciation**

LP tokens represent your stake in the Pupas Protocol pool. Unlike traditional staking that pays rewards in new tokens, Pupas generates yield through LP token price appreciation driven by AI investment returns.

## LP Token Fundamentals

### What Are LP Tokens?

LP (Liquidity Provider) tokens are **receipt tokens** that represent your proportional share of the staking pool:

- **Ownership Proof**: LP tokens prove your stake in the pool
- **Yield Mechanism**: Earn through token price appreciation, not inflation
- **Transferable**: LP tokens can be transferred between wallets
- **Redeemable**: Burn LP tokens to withdraw USDTu/USDT-ERC20 at current price

### Token Standard
- **Blockchain**: Waves native token
- **Standard**: Waves token protocol
- **Decimals**: 8 (matching USDTu/USDT-ERC20 precision)
- **Symbol**: PUPAS-LP

## Price Appreciation Model

### Core Formula
```
LP Token Price = (Total Pool Value + Protocol Account Balance) ÷ Total LP Token Supply
```

## Minting and Burning

### Minting Process (Staking)

When you stake USDTu/USDT-ERC20, new LP tokens are minted:

```javascript
// Calculate LP tokens to mint
const lpTokensToMint = stakeAmount / currentLpPrice;

// Example: Stake 1,000 USDTu at 1.05 LP price
// LP tokens = 1,000 / 1.05 = 952.38 LP tokens
```

#### Minting Example
| Stake Amount | LP Price | LP Tokens Received |
|--------------|----------|-------------------|
| 100 USDTu | 1.00 | 100.00 LP |
| 100 USDTu | 1.10 | 90.91 LP |
| 100 USDTu | 1.25 | 80.00 LP |

### Burning Process (Withdrawal)

When you withdraw, LP tokens are burned for USDTu:

```javascript
// Calculate USDTu to return
const usdtToReturn = lpTokensToBurn * currentLpPrice;

// Example: Burn 952.38 LP tokens at 1.15 LP price
// USDT = 952.38 * 1.15 = 1,095.24 USDTu
```

#### Burning Example
| LP Tokens | LP Price | USDTu Received |
|-----------|----------|----------------|
| 100 LP | 1.00 | 100.00 USDTu |
| 100 LP | 1.10 | 110.00 USDTu |
| 100 LP | 1.25 | 125.00 USDTu |

## Yield Calculation

### APY Through Price Appreciation

Your yield comes from LP token price increases:

```javascript
// Calculate your yield
const initialLpPrice = 1.00;  // Price when you staked
const currentLpPrice = 1.15;  // Current price
const yield = (currentLpPrice / initialLpPrice - 1) * 100;
// Yield = 15%

// Annualized (if held for 6 months)
const daysHeld = 180;
const apy = Math.pow(currentLpPrice / initialLpPrice, 365 / daysHeld) - 1;
// APY ≈ 32.25%
```

### Compound Growth

LP token price appreciation compounds automatically:

```
Month 1: 1.00 → 1.02 (2% gain)
Month 2: 1.02 → 1.04 (2% on 1.02 = 2.04% total)
Month 3: 1.04 → 1.06 (2% on 1.04 = 6.08% total)
```

No need to claim or reinvest - gains compound within the LP token price.

## Price Update Mechanism

### Oracle System

LP token prices update every hour via smart contract oracle. LP token price is calculated as (Total Pool Value + Current Account Balance) ÷ Total LP Token Supply.

### Update Process

1. **Data Collection**: Oracle aggregates pool value and investment returns
2. **Price Calculation**: New LP price computed using formula
3. **Validation**: Price change limits prevent manipulation
4. **On-chain Update**: New price written to smart contract
5. **Event Emission**: Price update event logged for transparency

### Price History Tracking

All price updates are recorded on-chain/blockchain.

## Investment Impact on Price

### How AI Investments Affect LP Price

#### Profitable Investments
```
Before: Pool = 1,000,000 USDTu, LP Supply = 1,000,000, Price = 1.00, Account Balance = 0 USDTu
AI Profit: +50,000 USDTu (5% return)
After: Pool = 1,050,000 USDTu, LP Supply = 1,000,000, Price = 1.05, Account Balance = 50,000 USDTu
```

#### Loss Scenarios
```
Before: Pool = 1,000,000 USDTu, LP Supply = 1,000,000, Price = 1.00
AI Loss: -30,000 USDTu (3% loss)
After: Pool = 970,000 USDTu, LP Supply = 1,000,000, Price = 0.97
```

### Protocol Fee Impact

Protocol fee (0.3%) is deducted at staking time:

```javascript
// Staking with protocol fee
const stakeAmount = 1000;       // USDTu to stake
const protocolFee = stakeAmount * 0.003;  // 3 USDTu fee
const netStake = stakeAmount - protocolFee;  // 997 USDTu

// LP tokens minted based on net stake
const lpTokens = netStake / currentLpPrice;  // 997 LP tokens if price = 1.00
```

## LP Token Utility

### Current Uses
- **Yield Generation**: Hold for price appreciation
- **Withdrawal**: Burn for USDTu at current price
- **Transfer**: Send to other wallets (maintains yield exposure)
- **Portfolio Tracking**: Monitor via block explorers

### Future Utility (Roadmap)
- **Governance Voting**: Vote on protocol proposals (Q4 2024)
- **Fee Discounts**: Reduced protocol fees for large holders
- **Strategy Selection**: Choose preferred AI investment strategies
- **Liquidity Mining**: Additional rewards for providing LP token liquidity

## Risk Factors

### LP Token Risks

#### Smart Contract Risk
- **Code Bugs**: Potential vulnerabilities in staking contract
- **Oracle Manipulation**: Risk of price oracle attacks
- **Admin Keys**: Centralized control during early phases

#### Market Risk
- **AI Strategy Losses**: Investment strategies may lose money
- **Liquidity Risk**: Large withdrawals may face delays
- **Price Volatility**: LP token prices can fluctuate significantly

#### Mitigation Measures
- **Audited Contracts**: Professional security reviews completed
- **Position Limits**: Maximum exposure caps per strategy
- **Diversification**: Risk spread across multiple investments
- **Emergency Procedures**: Circuit breakers and pause mechanisms

{% hint style="warning" %}
**Risk Disclosure**: LP token prices can decrease as well as increase. AI investment strategies don't guarantee profits and may result in losses during adverse market conditions.
{% endhint %}

## Monitoring Your LP Tokens

### Key Metrics to Track

#### Price Performance
- **Current LP Price**: Real-time token value
- **Price History**: 24h, 7d, 30d price charts
- **APY Calculation**: Annualized yield based on price appreciation
- **Benchmark Comparison**: Performance vs USDT savings rates

#### Portfolio Value
```javascript
// Your portfolio calculation
const yourLpTokens = 952.38;
const currentLpPrice = 1.15;
const portfolioValue = yourLpTokens * currentLpPrice;  // 1,095.24 USDTu
const initialStake = 1000;  // Original stake
const totalGain = portfolioValue - initialStake;  // 95.24 USDTu profit
```

### Dashboard Features
- **Real-time Price**: Updates every hour with oracle
- **Performance Charts**: Historical price and yield data
- **Profit Calculator**: Estimate returns based on holding period
- **Withdrawal Simulator**: Preview withdrawal amounts

## Advanced Concepts

### Impermanent Loss Comparison

Unlike traditional LP tokens in AMMs, Pupas LP tokens don't suffer from impermanent loss:

| Traditional AMM LP | Pupas Protocol LP |
|-------------------|-------------------|
| Exposed to IL from price divergence | No impermanent loss risk |
| Earn fees from trades | Earn from AI investment profits |
| Must provide two tokens | Single token (USDTu) staking |
| Price tied to underlying assets | Price reflects pool performance |

### Arbitrage Opportunities

LP token price updates create potential arbitrage:

1. **Oracle Lag**: Brief delays between actual and on-chain prices
2. **Cross-platform**: Price differences between platforms (future)
3. **Withdrawal Timing**: Strategic timing around price updates

{% hint style="info" %}
**Advanced Users**: LP token mechanics enable sophisticated strategies. See our [Technical Guide](../developers/technical-guide.md) for implementation details.
{% endhint %}

## Next Steps

- Learn about [AI Investment Strategies](../tokenomics/ai-strategies.md) that drive LP price appreciation
- Explore [Yield Sources](../tokenomics/yield-sources.md) to understand profit generation
- Check the [FAQ](../support/faq.md) for common LP token questions 
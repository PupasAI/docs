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
- **Decimals**: 6 (matching protocol precision)
- **Symbol**: PUPAS LP

## Price Appreciation Model

### Core Formula
```ride
# LP Token Price = Total Pool Balance ÷ Total LP Token Supply
let currentPrice = tryGetInteger("global_lpPrice")
```

## Minting and Burning

### Minting Process (Staking)

When you stake USDTu/USDT-ERC20, new LP tokens are minted:

```ride
# Calculate LP tokens to mint
let feeAmount = fraction(pmt.amount, MintFee, Scale6)  # 0.3% protocol fee
let cleanAmount = pmt.amount - feeAmount
let lpAmount = fraction(cleanAmount, Scale6, tryGetInteger("global_lpPrice"))

# Mint LP tokens and transfer to user
[
  Reissue(lpId, lpAmount, true),
  ScriptTransfer(i.caller, lpAmount, lpId)
]
```

#### Minting Example
| Stake Amount | LP Price | LP Tokens Received |
|--------------|----------|-------------------|
| 100 USDTu | 1.00 | 99.7 LP (after 0.3% fee) |
| 100 USDTu | 1.10 | 90.64 LP (after 0.3% fee) |
| 100 USDTu | 1.25 | 79.76 LP (after 0.3% fee) |

### Burning Process (Withdrawal)

When you withdraw, LP tokens are burned for USDTu:

```ride
# Calculate USDTu to return
let lpAmount = pmt.amount
let amountToWithdraw = fraction(lpAmount, tryGetInteger("global_lpPrice"), Scale6)

# Burn LP tokens and transfer USDT
[
  Burn(lpId, lpAmount),
  ScriptTransfer(i.caller, amountToWithdraw, fromBase58String(assetIdStr))
]
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

```ride
# Calculate yield (pseudocode for demonstration)
let initialLpPrice = 1000000  # 1.00 with 6 decimals
let currentLpPrice = 1150000  # 1.15 with 6 decimals
let yieldPercent = fraction((currentLpPrice - initialLpPrice), 100, initialLpPrice)
# Yield = 15%
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

LP token prices update hourly via the `updateLpPrice` function:

```ride
@Callable(i)
func updateLpPrice(lpPrice: Int) = {
  let currentOraclePrice = valueOrElse(getInteger("global_lpPrice"), Scale6)
  let oracleChangeDelta = abs(max(
    fraction(currentOraclePrice, Scale6, lpPrice),
    fraction(lpPrice, Scale6, currentOraclePrice)
  ) - Scale6)
  
  # Validate caller and price change limits
  if (i.caller != this) then
    throw("available for self invoke only")
  else if (oracleChangeDelta > OracleVolatilityTolerance) then
    throw("max change for oracle is " + toString(fraction(OracleVolatilityTolerance / 100, Scale6)) + "%")
  else
    [IntegerEntry("global_lpPrice", lpPrice)]
}
```

### Update Process

1. **Data Collection**: Oracle aggregates pool value and investment returns
2. **Price Calculation**: New LP price computed using formula
3. **Validation**: Price change limited to 10% per update
4. **On-chain Update**: New price written to smart contract
5. **Event Logging**: Price update recorded on blockchain

### Price History Tracking

All price updates are recorded on-chain via `IntegerEntry("global_lpPrice", lpPrice)`.

## Investment Impact on Price

### How AI Investments Affect LP Price

#### Profitable Investments
```
Before: Pool = 1,000,000 USDTu, LP Supply = 1,000,000, Price = 1.00
AI Profit: +50,000 USDTu (5% return)
After: Pool = 1,050,000 USDTu, LP Supply = 1,000,000, Price = 1.05
```

#### Loss Scenarios
```
Before: Pool = 1,000,000 USDTu, LP Supply = 1,000,000, Price = 1.00
AI Loss: -30,000 USDTu (3% loss)
After: Pool = 970,000 USDTu, LP Supply = 1,000,000, Price = 0.97
```

### Protocol Fee Impact

Protocol fee (0.3%) is deducted at staking time:

```ride
# Staking with protocol fee
let feeAmount = fraction(pmt.amount, MintFee, Scale6)  # 0.3% of stake
let cleanAmount = pmt.amount - feeAmount
let lpAmount = fraction(cleanAmount, Scale6, tryGetInteger("global_lpPrice"))

# Fee sent to treasury, LP tokens based on net stake
[
  ScriptTransfer(FeeAddress, feeAmount, pmt.assetId),
  Reissue(lpId, lpAmount, true),
  ScriptTransfer(i.caller, lpAmount, lpId)
]
```

## LP Token Utility

### Current Uses
- **Yield Generation**: Hold for price appreciation
- **Withdrawal**: Burn for USDTu at current price
- **Transfer**: Send to other wallets (maintains yield exposure)
- **Portfolio Tracking**: Monitor via Waves Explorer

### Future Utility (Roadmap)
- **Governance Voting**: Vote on protocol proposals
- **Strategy Selection**: Choose preferred AI investment strategies
- **Fee Discounts**: Reduced protocol fees for large holders

## Risk Factors

### LP Token Risks

#### Smart Contract Risk
- **Code Bugs**: Potential vulnerabilities in staking contract
- **Oracle Manipulation**: Risk of price oracle attacks
- **Access Control**: Centralized oracle updates during early phases

#### Market Risk
- **AI Strategy Losses**: Investment strategies may lose money
- **Liquidity Risk**: Large withdrawals may face delays
- **Price Volatility**: LP token prices can fluctuate significantly

#### Mitigation Measures
- **Volatility Limits**: 10% maximum price change per update
- **Access Control**: Only authorized addresses can update prices
- **Error Handling**: Comprehensive validation and error messages
- **Transparency**: All operations recorded on blockchain

{% hint style="warning" %}
**Risk Disclosure**: LP token prices can decrease as well as increase. AI investment strategies don't guarantee profits and may result in losses during adverse market conditions.
{% endhint %}

## Monitoring Your LP Tokens

### Key Metrics to Track

#### Price Performance
- **Current LP Price**: Real-time token value from `global_lpPrice`
- **Price History**: Historical oracle updates
- **APY Calculation**: Annualized yield based on price appreciation
- **Benchmark Comparison**: Performance vs USDT savings rates

#### Portfolio Value
```ride
# Your portfolio calculation (pseudocode)
let yourLpTokens = 997000000  # 997 LP tokens (6 decimals)
let currentLpPrice = tryGetInteger("global_lpPrice")
let portfolioValue = fraction(yourLpTokens, currentLpPrice, Scale6)
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

### Smart Contract Integration

LP token operations are handled by RIDE smart contract functions:

```ride
# Helper functions for safe operations
func tryGetInteger(key: String) = {
  match getInteger(this, key) {
    case a: Int => a
    case _ => 0
  }
}

func tryGetString(key: String) = {
  match getString(this, key) {
    case a: String => a
    case _ => ""
  }
}
```

{% hint style="info" %}
**Advanced Users**: LP token mechanics enable sophisticated strategies. See our [Staking Mechanics](mechanics.md) for technical implementation details.
{% endhint %}

## Next Steps

- Learn about [Yield Sources](../protocol/yield-sources.md) to understand profit generation
- Explore [Protocol Features](../protocol/features.md) to see how AI drives returns
- Check the [FAQ](../support/faq.md) for common LP token questions 
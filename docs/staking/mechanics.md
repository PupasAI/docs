# Staking Mechanics

**Deep dive into Pupas Protocol's AI-powered staking system**

Understanding the technical mechanics behind Pupas Protocol helps you maximize returns and make informed staking decisions. Our system combines traditional LP token mechanics with AI-driven investment strategies.

## Core Staking Process

### Smart Contract Functions

#### Provide Function (Staking)
```ride
@Callable(i)
func provide() = {
  let pmt = i.payments[0]
  
  # Validate supported tokens (USDTu or USDT-ERC20)
  if (pmt.assetId != usdtId && pmt.assetId != usdtuId) then
    throw("please attach USDT: " + usdtIdStr + ", " + usdtuIdStr)
  else
    # Calculate protocol fee (0.3% of stake)
    let feeAmount = fraction(pmt.amount, MintFee, Scale6)
    let cleanAmount = pmt.amount - feeAmount
    
    # Calculate LP tokens to mint based on current price
    let lpAmount = fraction(cleanAmount, Scale6, tryGetInteger("global_lpPrice"))
    
    [
      Reissue(lpId, lpAmount, true),
      ScriptTransfer(i.caller, lpAmount, lpId),
      ScriptTransfer(FeeAddress, feeAmount, pmt.assetId)
    ]
}
```

#### Withdraw Function
```ride
@Callable(i)
func withdraw(assetIdStr: String, lendMarketStr: String) = {
  let pmt = i.payments[0]
  
  # Validate LP token attachment
  if (pmt.assetId != lpId) then
    throw("please attach LP: " + lpIdStr)
  else
    # Validate withdrawal asset (USDTu or USDT-ERC20 only)
    if (assetIdStr != usdtIdStr && assetIdStr != usdtuIdStr) then
      throw("you can only withdraw USDT: " + usdtIdStr + ", " + usdtuIdStr)
    else
      let lpAmount = pmt.amount
      
      # Calculate USDT amount based on current LP price
      let amountToWithdraw = fraction(lpAmount, tryGetInteger("global_lpPrice"), Scale6)
      
      [
        Burn(lpId, lpAmount),
        ScriptTransfer(i.caller, amountToWithdraw, fromBase58String(assetIdStr))
      ]
}
```

## LP Token Price Mechanism

### Price Update Frequency
- **Interval**: Every 60 minutes (3,600 seconds)
- **Oracle Function**: `updateLpPrice()` called by authorized address
- **Validation**: Price change limits prevent manipulation
- **Transparency**: All updates recorded on-chain

#### Oracle Price Update
```ride
@Callable(i)
func updateLpPrice(lpPrice: Int) = {
  let currentOraclePrice = valueOrElse(getInteger("global_lpPrice"), Scale6)
  let oracleChangeDelta = abs(max(
    fraction(currentOraclePrice, Scale6, lpPrice),
    fraction(lpPrice, Scale6, currentOraclePrice)
  ) - Scale6)
  
  # Validate caller authorization
  if (i.caller != this) then
    throw("available for self invoke only")
  else
    # Check volatility tolerance
    if (oracleChangeDelta > OracleVolatilityTolerance) then
      throw("max change for oracle is " + toString(fraction(OracleVolatilityTolerance / 100, Scale6)) + "%")
    else
      [IntegerEntry("global_lpPrice", lpPrice)]
}
```

## Investment Flow

### Fund Allocation Process

1. **Pool Aggregation**: Staked USDTu/USDT-ERC20 collected in main pool
2. **AI Analysis**: Investment agents analyze market conditions
3. **Strategy Selection**: AI choose optimal investment pools
4. **Fund Deployment**: Portion of pool allocated to selected strategies
5. **Return Calculation**: Profits/losses calculated and recorded
6. **Price Update**: New LP token price reflects pool performance and account balance

### Investment Strategies

#### Lending Protocol Integration
```ride
func withdrawFromLend(amount: Int, assetIdStr: String, market: Address) = {
  let inv = invoke(market, "withdraw", [assetIdStr, [amount]], [])
  
  if (inv == inv) then [] 
  else throw("Strict value is not equal to itself.")
}
```

## Risk Management System

### Position Limits
```ride
# Risk management constants
let OracleVolatilityTolerance = 100000  # 10% max price change per update
let MintFee = 3000                      # 0.3% protocol fee
let Scale6 = 1000000                    # 6 decimal precision
```

### Diversification Rules
- **Maximum volatility tolerance**: 10% per price update
- **Protocol fee**: 0.3% of stake amount
- **Automated position management** via AI agents

## Token Economics

### LP Token Supply Management

#### Minting Process
```ride
# LP tokens minted = (stake amount - fee) / current LP price
let lpAmount = fraction(cleanAmount, Scale6, tryGetInteger("global_lpPrice"))
```

#### Burning Process
```ride
# USDT returned = LP tokens × current LP price
let amountToWithdraw = fraction(lpAmount, tryGetInteger("global_lpPrice"), Scale6)
```

### Fee Structure

#### Protocol Fee (0.3% of stake)
```ride
# Applied at staking time, sent to treasury
let feeAmount = fraction(pmt.amount, MintFee, Scale6)  # 0.3% of initial stake
let cleanAmount = pmt.amount - feeAmount

# Send fee to treasury and mint LP tokens based on net stake
[
  ScriptTransfer(FeeAddress, feeAmount, pmt.assetId),
  Reissue(lpId, lpAmount, true),
  ScriptTransfer(i.caller, lpAmount, lpId)
]
```

## Withdrawal Mechanics

### Instant Withdrawal
- **No Lock Period**: Withdraw anytime without penalties
- **Current Price**: Always withdraw at latest LP price
- **Automated Processing**: Smart contract handles transfers

### LP Token Burning
```ride
# Burn LP tokens and transfer corresponding USDT amount
[
  Burn(lpId, lpAmount),
  ScriptTransfer(i.caller, amountToWithdraw, fromBase58String(assetIdStr))
]
```

## Performance Tracking

### Price Calculation
```ride
# Current LP price stored as integer with 6 decimal precision
let currentPrice = tryGetInteger("global_lpPrice")

# Helper function to safely get integer values
func tryGetInteger(key: String) = {
  match getInteger(this, key) {
    case a: Int => a
    case _ => 0
  }
}
```

### Performance Metrics
- **LP Price History**: Tracked via oracle updates
- **Total Value Locked**: Sum of all staked assets
- **Active Strategies**: Monitored by AI agents
- **Fee Collection**: Protocol revenue tracking

## Emergency Procedures

### Access Control
```ride
# Only contract itself can update LP prices
if (i.caller != this) then
  throw("available for self invoke only")
```

### Initialization Security
```ride
@Callable(i)
func init(usdtIdStr: String, usdtuIdStr: String) = {
  if (i.caller != this) then
    throw("available for self invoke only")
  else
    # Initialize protocol with supported tokens
    let newLp = Issue("PUPAS LP", "PUPAS AI TREASURY LP", 0, 6, true)
    let newLpId = toBase58String(calculateAssetId(newLp))
    
    [
      newLp,
      StringEntry("setup_lpId", newLpId),
      StringEntry("setup_usdtId", usdtIdStr),
      StringEntry("setup_usdtuId", usdtuIdStr)
    ]
}
```

{% hint style="info" %}
**Technical Deep Dive**: For additional technical details, see our [Protocol Features](../protocol/features.md) and [LP Tokens](lp-tokens.md) guides.
{% endhint %}

## Waves Blockchain Benefits

### Transaction Efficiency
- **Low Fees**: ~0.005 WAVES per transaction
- **Fast Finality**: ~1 minute confirmation
- **Predictable Costs**: No gas price volatility
- **RIDE Language**: Secure, predictable smart contracts

### Data Storage
```ride
# Protocol configuration stored on-chain
func tryGetString(key: String) = {
  match getString(this, key) {
    case a: String => a
    case _ => ""
  }
}

# Access stored values
let usdtIdStr = tryGetString("setup_usdtId")
let usdtuIdStr = valueOrElse(getString("setup_usdtuId"), "B45iYkZVC9cudR2yxrsJnrM75StiTrwphbfQ7xkyisip")
let lpIdStr = tryGetString("setup_lpId")
```

## Monitoring and Alerts

### Real-time Monitoring
- **Price Deviations**: Oracle volatility tolerance enforced
- **Strategy Performance**: AI agent monitoring
- **Liquidity Levels**: Automated withdrawal processing
- **System Health**: Smart contract state validation

### Error Handling
```ride
# Comprehensive error messages for user guidance
if (pmt.assetId != usdtId && pmt.assetId != usdtuId) then
  throw("please attach USDT: " + usdtIdStr + ", " + usdtuIdStr)

# Volatility protection
if (oracleChangeDelta > OracleVolatilityTolerance) then
  throw("max change for oracle is " + toString(fraction(OracleVolatilityTolerance / 100, Scale6)) + "%")
```

{% hint style="warning" %}
**Important**: LP token prices can go down as well as up. AI strategies don't guarantee profits and may result in losses during adverse market conditions.
{% endhint %}

## Next Steps

- Understand [LP Token mechanics](lp-tokens.md) in detail
- Learn about [Protocol Features](../protocol/features.md) to see AI capabilities
- Explore [Yield Sources](../protocol/yield-sources.md) for profit generation 
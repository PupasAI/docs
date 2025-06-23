# Key Features

**Core functionality of the Pupas Protocol**

## AI Investment System
- **Investment Strategies**: Automated fund allocation across DeFi protocols
- **Risk Management**: Continuous exposure monitoring and safeguards
- **Oracle Integration**: Real-time price data for LP token pricing and yield
- **Performance Optimization**: Dynamic strategy adjustment based on market conditions

## AI Chat System
- **Multi-Agent Interface**: Four specialized assistants for user support
- **Contextual Assistance**: Real-time data processing from multiple sources
- **User Support System**: Automated fund allocation based on AI models
- **Performance Optimization**: Dynamic strategy adjustment based on market conditions

## LP Token Mechanics

### Dynamic Pricing Model
```ride
# LP Token Price stored as integer with 6 decimal precision
let currentPrice = tryGetInteger("global_lpPrice")
```

### Price Updates
- Oracle updates LP prices hourly via `updateLpPrice` function
- Price appreciation provides yield to stakers
- Transparent on-chain calculation process

### Withdrawal Process
```ride
# Withdrawal calculation
let lpAmount = pmt.amount
let amountToWithdraw = fraction(lpAmount, tryGetInteger("global_lpPrice"), Scale6)

# LP tokens are burned, USDTu returned to user
[
  Burn(lpId, lpAmount),
  ScriptTransfer(i.caller, amountToWithdraw, fromBase58String(assetIdStr))
]
```

## Supported Tokens

### Token Types
| Token | Network | Status |
|-------|---------|--------|
| USDT-ERC20 | Ethereum | Bridged to Waves via gateway |
| USDTu | Waves | Native support |

### Integration
- Automatic token type detection via smart contract validation
- Unified staking interface for both tokens
- Seamless conversion and processing

## Oracle System

### Price Updates
- Hourly price updates via `updateLpPrice` smart contract method
- Volatility protection with 10% maximum change per update
- On-chain transparency for all price changes

### Calculation Process
```ride
# Oracle price update with validation
@Callable(i)
func updateLpPrice(lpPrice: Int) = {
  let currentOraclePrice = valueOrElse(getInteger("global_lpPrice"), Scale6)
  let oracleChangeDelta = abs(max(
    fraction(currentOraclePrice, Scale6, lpPrice),
    fraction(lpPrice, Scale6, currentOraclePrice)
  ) - Scale6)
  
  # Validate price change within tolerance
  if (oracleChangeDelta > OracleVolatilityTolerance) then
    throw("max change for oracle is " + toString(fraction(OracleVolatilityTolerance / 100, Scale6)) + "%")
  else
    [IntegerEntry("global_lpPrice", lpPrice)]
}
```

## Chat System

### Four Specialized Agents

#### General Support Agent
- Platform navigation assistance
- Feature explanations
- Basic user guidance

#### Yield Analytics Agent
- APY calculations and insights
- Performance data analysis
- Yield mechanism explanations

#### Technical Support Agent
- Transaction assistance
- Smart contract interaction help
- Technical troubleshooting

#### Security & Risk Agent
- Risk assessment guidance
- Security recommendations
- Best practices education

{% hint style="info" %}
**Learn More**

Visit [pupas.ai](https://pupas.ai) to explore the platform
{% endhint %}
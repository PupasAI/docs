# Staking Mechanics

**Deep dive into Pupas Protocol's AI-powered staking system**

Understanding the technical mechanics behind Pupas Protocol helps you maximize returns and make informed staking decisions. Our system combines traditional LP token mechanics with AI-driven investment strategies.

## Core Staking Process

### Smart Contract Functions

#### Stake Function
```solidity
function stake(uint256 amount, uint8 tokenType) external returns (uint256 lpTokens) {
    // Validate token type
    require(amount > 0, "Amount must be positive");
    
    // Calculate LP tokens to mint
    uint256 lpTokens = amount.mul(PRECISION).div(getCurrentLpPrice());
    
    // Transfer tokens and mint LP tokens
    transferFrom(msg.sender, address(this), amount);
    _mint(msg.sender, lpTokens);
    
    // Update pool state
    totalStaked = totalStaked.add(amount);
    emit Staked(msg.sender, amount, lpTokens);
    
    return lpTokens;
}
```

#### Withdraw Function
```solidity
function withdraw(uint256 lpTokens) external returns (uint256 usdtAmount) {
    // Calculate USDT amount to return
    uint256 usdtAmount = lpTokens.mul(getCurrentLpPrice()).div(PRECISION);
    
    // Burn LP tokens and transfer USDT
    _burn(msg.sender, lpTokens);
    transfer(msg.sender, usdtAmount);
    
    // Update pool state
    totalStaked = totalStaked.sub(usdtAmount);
    emit Withdrawn(msg.sender, lpTokens, usdtAmount);
    
    return usdtAmount;
}
```

## LP Token Price Mechanism

### Price Update Frequency
- **Interval**: Every 60 minutes (3,600 seconds)
- **Data Sources**: Multiple exchange APIs aggregated
- **Validation**: Price change limits prevent manipulation
- **Transparency**: All updates recorded on-chain

## Investment Flow

### Fund Allocation Process

1. **Pool Aggregation**: Staked USDTu/USDT-ERC20 collected in main pool
2. **AI Analysis**: Investment agents analyze market conditions
3. **Strategy Selection**: AI choose optimal investment pools
4. **Fund Deployment**: Portion of pool allocated to selected strategies
5. **Return Calculation**: Profits/losses calculated and recorded
6. **Price Update**: New LP token price reflects pool performance and account balance

### Investment Strategies

#### Lending Protocol Selection
- **Protocol Analysis**: AI evaluates available lending platforms on Waves
- **Risk Assessment**: Evaluate smart contract security and yields
- **Optimal Allocation**: Distribute funds across best protocols

#### Staking Opportunities
- **Validator Selection**: Choose best performing validators
- **Delegation Strategies**: Optimize staking rewards
- **Auto-Compounding**: Reinvest staking rewards automatically

## Risk Management System

### Position Limits
```javascript
const RISK_LIMITS = {
    maxPositionSize: 0.25,      // 25% of pool per strategy
    maxDrawdown: 0.10,          // 10% maximum loss per strategy
    correlationLimit: 0.70,     // Maximum correlation between strategies
    liquidityReserve: 0.15      // 15% kept for withdrawals
};
```

### Stop-Loss Mechanisms
- **Individual Positions**: 5% stop-loss per trade
- **Strategy Level**: 10% drawdown triggers strategy pause
- **Pool Level**: 15% total loss triggers emergency mode

### Diversification Rules
- **Maximum 4 active strategies** at any time
- **No more than 40% in single asset class**
- **Geographic diversification** across exchanges

## Token Economics

### LP Token Supply Management

#### Minting Process
```
New LP Tokens = Stake Amount ÷ Current LP Price
Total LP Supply += New LP Tokens
```

#### Burning Process
```
USDT Return = LP Tokens × Current LP Price
Total LP Supply -= Burned LP Tokens
```

### Fee Structure

#### Protocol Fee (0.3% of stake)
```javascript
// Applied at staking time, sent to treasury
function stake(uint256 amount) external {
    const protocolFee = amount * 0.003;  // 0.3% of initial stake
    const netStake = amount - protocolFee;
    
    // Send fee to treasury
    transfer(treasuryAddress, protocolFee);
    
    // Mint LP tokens based on net stake amount
    const lpTokensToMint = netStake / getCurrentLpPrice();
    _mint(msg.sender, lpTokensToMint);
}
```

## Withdrawal Mechanics

### Instant Withdrawal
- **Liquidity Pool**: 15% of funds kept for instant withdrawals
- **No Lock Period**: Withdraw anytime without penalties
- **Current Price**: Always withdraw at latest LP price

### Large Withdrawal Handling
```javascript
if (withdrawalAmount > liquidityPool * 0.5) {
    // Large withdrawal may require up to 24 hours
    // AI agents liquidate positions as needed
    scheduleDelayedWithdrawal(user, amount);
} else {
    // Instant withdrawal from liquidity pool
    processInstantWithdrawal(user, amount);
}
```

## Performance Tracking

### APY Calculation
```javascript
// Rolling 30-day APY calculation
const periodReturns = [];
for (let i = 0; i < 30; i++) {
    const dailyReturn = (priceToday / priceYesterday) - 1;
    periodReturns.push(dailyReturn);
}

const avgDailyReturn = periodReturns.reduce((a, b) => a + b) / 30;
const annualizedAPY = Math.pow(1 + avgDailyReturn, 365) - 1;
```

### Performance Metrics
- **Sharpe Ratio**: Risk-adjusted returns
- **Maximum Drawdown**: Largest peak-to-trough decline
- **Win Rate**: Percentage of profitable strategies
- **Volatility**: Standard deviation of returns

## Emergency Procedures

### Circuit Breakers
```solidity
modifier emergencyStop() {
    require(!paused, "Contract is paused");
    if (totalLoss > MAX_ACCEPTABLE_LOSS) {
        paused = true;
        emit EmergencyStop(block.timestamp);
    }
    _;
}
```

### Recovery Mechanisms
- **Pause Trading**: Stop new investments during market stress
- **Liquidate Positions**: Convert investments back to USDT
- **Proportional Distribution**: Share remaining funds among LP holders

{% hint style="info" %}
**Technical Deep Dive**: For additional technical details, see our [Protocol Features](../protocol/features.md) and [LP Tokens](lp-tokens.md) guides.
{% endhint %}

## Gas Optimization

### Batch Operations
- **Multiple Stakes**: Combine small stakes to reduce gas costs
- **Withdrawal Queuing**: Process multiple withdrawals together
- **Oracle Updates**: Batch price updates when possible

### Waves Blockchain Benefits
- **Low Fees**: ~0.005 WAVES per transaction
- **Fast Finality**: ~1 minute confirmation
- **Predictable Costs**: No gas price volatility

## Monitoring and Alerts

### Real-time Monitoring
- **Price Deviations**: Alert on unusual LP price movements
- **Strategy Performance**: Monitor individual strategy returns
- **Liquidity Levels**: Track withdrawal capacity
- **System Health**: Overall protocol performance

### User Notifications
- **Price Updates**: Hourly LP price changes
- **Strategy Changes**: When AI switches strategies
- **Risk Alerts**: Unusual market conditions
- **Performance Reports**: Weekly summaries

{% hint style="warning" %}
**Important**: LP token prices can go down as well as up. AI strategies don't guarantee profits and may result in losses during adverse market conditions.
{% endhint %}

## Next Steps

- Understand [LP Token mechanics](lp-tokens.md) in detail
- Learn about [Protocol Features](../protocol/features.md) to see AI capabilities
- Explore [Yield Sources](../protocol/yield-sources.md) for profit generation 
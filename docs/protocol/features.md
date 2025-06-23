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
```
LP Token Price = Total USDT-ERC20/USDTu Balance / Total LP Tokens
```

### Price Updates
- Oracle updates LP prices hourly via smart contract
- Price appreciation provides yield to stakers
- Transparent on-chain calculation process

### Withdrawal Process
```javascript
// Withdrawal calculation
withdrawalAmount = lpTokens * currentLpPrice
// LP tokens are burned, USDTu returned to user
```

## Supported Tokens

### Token Types
| Token | Network | Status |
|-------|---------|--------|
| USDT-ERC20 | Ethereum | Bridged to Waves |
| USDTu | Waves | Native support |

### Integration
- Automatic token type detection
- Unified staking interface for both tokens
- Seamless conversion and processing

## Oracle System

### Price Updates
- Hourly price updates via `updateLpPrice` smart contract method
- Multi-source data aggregation
- On-chain transparency for all price changes

### Calculation Process
```
New LP Price = (Current Pools Balances + Current Account Balances) / Total LP Supply
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
# Frequently Asked Questions

**Quick answers to common questions about Pupas Protocol**

Find answers to frequently asked questions about AI-powered staking, LP tokens, and platform usage.

## Getting Started

<details>
<summary>What is Pupas Protocol?</summary>

Pupas Protocol is an AI-powered staking platform for USDTu and USDT-ERC20 tokens on Waves blockchain. Users stake tokens, receive LP tokens, and earn yield through AI-managed fund allocation across DeFi protocols.
</details>

<details>
<summary>What is the minimum staking amount?</summary>

There is **no minimum staking amount**. You can stake any amount of USDTu or USDT-ERC20 tokens to participate in Pupas Protocol. This makes the protocol accessible to all users regardless of their budget size.
</details>

<details>
<summary>Which tokens can I stake?</summary>

You can stake:
- **USDTu** (Waves native): Contract `34N9YcEETLWn93qYQ64EsP1x89tSruJU44RrEMSXXEPJ`
- **USDT-ERC20** (Ethereum): Bridged to Waves via gateway

Both tokens provide the same staking experience through the unified platform interface.
</details>

<details>
<summary>How do I get started with staking?</summary>

1. **Connect Wallet**: Visit pupas.ai/staking and connect your Waves wallet
2. **Prepare Funds**: Ensure you have USDTu/USDT-ERC20 and WAVES for transaction fees
3. **Stake Tokens**: Enter amount, select token type, and confirm transaction
4. **Receive LP Tokens**: LP tokens representing your stake are minted to your wallet
5. **Monitor Performance**: Track your yield through LP token price changes
</details>

## LP Tokens and Yield

<details>
<summary>How often are LP token prices updated?</summary>

LP token prices update **hourly** via the oracle system using the `updateLpPrice` smart contract method. The price calculation follows:

**LP Price = Total Pool Balance ÷ Total LP Tokens**

Price updates are recorded on-chain and visible through the platform dashboard.
</details>

<details>
<summary>How do I calculate my yield?</summary>

Your yield comes from LP token price appreciation:

```ride
# Example calculation (pseudocode for demonstration)
let initialLpPrice = 1000000  # 1.00 with 6 decimals
let currentLpPrice = 1150000  # 1.15 with 6 decimals
let yieldPercent = fraction((currentLpPrice - initialLpPrice), 100, initialLpPrice)
# Result: 15% gain

# Your portfolio value
let yourLpTokens = 1000000000  # 1000 LP tokens (6 decimals)
let portfolioValue = fraction(yourLpTokens, currentLpPrice, Scale6)
```
</details>

<details>
<summary>Can LP token prices go down?</summary>

**Yes**, LP token prices can decrease if AI strategies result in losses. The protocol doesn't guarantee profits and investments may lose value during adverse market conditions. Only stake what you can afford to lose.
</details>

<details>
<summary>What is the expected APY?</summary>

Based on historical performance:
- **Average APY**: 12-16%
- **Range**: 8-20% depending on market conditions
- **Risk Level**: Medium (volatility around 12.5%)

Performance varies with market conditions and AI strategy effectiveness.
</details>

## AI Investment System

<details>
<summary>How do AI agents work?</summary>

The AI system uses multiple agents for different functions:

1. **Investment Agent**: Manages fund allocation across DeFi protocols
2. **Risk Agent**: Monitors exposure and implements safeguards
3. **Oracle Agent**: Provides price data for LP token pricing
4. **Chat Agents**: Four specialized assistants for user support

All agents work together to optimize returns while managing risk.
</details>

<details>
<summary>What investment strategies are used?</summary>

AI agents focus on DeFi opportunities including:
- Lending protocol optimization
- Staking opportunities
- Yield farming strategies

Specific strategies and allocations are managed automatically by the AI system.
</details>

<details>
<summary>Are the AI strategies safe?</summary>

AI strategies include multiple risk management features:
- **Position Limits**: Maximum 25% of pool per strategy
- **Stop Losses**: 5% stop-loss on individual positions
- **Diversification**: Spread across multiple uncorrelated strategies
- **Real-time Monitoring**: Instant system health checks

However, all investment strategies involve risk and potential losses.
</details>

<details>
<summary>Can I choose which AI strategies to use?</summary>

Currently, the AI system automatically allocates funds across strategies for optimal risk-adjusted returns. **Custom strategy selection** is planned for Q4 2025 as part of the governance token launch.
</details>

## Fees and Costs

<details>
<summary>What fees does Pupas Protocol charge?</summary>

- **Protocol Fee**: 0.3% of stake amount (sent to treasury at staking)
- **Network Fees**: Standard Waves blockchain transaction fees
- **No Withdrawal Fees**: No fee to unstake tokens

The protocol fee is deducted from your stake amount and sent to the treasury when you stake.
</details>

## Withdrawals and Liquidity

<details>
<summary>Can I withdraw my tokens anytime?</summary>

**Yes**, you can withdraw anytime. The withdrawal process burns your LP tokens and returns USDTu/USDT-ERC20 at the current LP token price. Withdrawal timing may vary based on available liquidity and AI position management.
</details>

<details>
<summary>How do withdrawals work?</summary>

1. **Burn LP Tokens**: Your LP tokens are destroyed
2. **Calculate Return**: `USDTu/USDT-ERC20 Return = LP Tokens × Current LP Price`
3. **Transfer Funds**: USDTu/USDT-ERC20 transferred to your wallet
4. **Update Pool**: Total LP supply decreases, pool balance adjusts

You receive the current LP token price at time of withdrawal.
</details>

<details>
<summary>What if I need to withdraw during a loss period?</summary>

You can always withdraw, but you'll receive the current LP token price, which may be lower than your initial stake if AI strategies have lost money. Consider market conditions and your risk tolerance before withdrawing during volatile periods.
</details>

## Technical Questions

<details>
<summary>Which wallets are supported?</summary>

Any Waves-compatible wallet including:
- **Waves.Exchange** (web wallet)
- **Waves Keeper** (browser extension)
- **WX Network** (mobile and web)
- **Ledger** (hardware wallet with Waves app)
- **Other Waves wallets**
</details>

<details>
<summary>Why is my transaction taking so long?</summary>

Waves blockchain typically confirms transactions in ~1 minute. Delays may occur due to:
- **Network Congestion**: High transaction volume
- **Low Fees**: Insufficient WAVES for transaction fees
- **Wallet Issues**: Connection problems with your wallet

Check transaction status on [Waves Explorer](https://wavesexplorer.com) using your transaction hash.
</details>

<details>
<summary>How can I track my performance?</summary>

Monitor your staking performance through:
- **Platform Dashboard**: Real-time LP price and portfolio value
- **Transaction History**: Record of stakes and withdrawals
- **AI Chat System**: Ask agents for performance analysis
- **On-chain Data**: All transactions visible on Waves blockchain
</details>

## Security and Risks

<details>
<summary>Is Pupas Protocol secure?</summary>

Security measures include:
- **Audited Smart Contracts**: Professional security reviews completed
- **Multi-signature Controls**: Critical operations require multiple signatures
- **Timelock Mechanisms**: Administrative functions have delays
- **Emergency Procedures**: Circuit breakers for unusual conditions

However, DeFi protocols always involve smart contract risks.
</details>

<details>
<summary>What are the main risks?</summary>

**Smart Contract Risk**: Potential vulnerabilities in protocol code
**Market Risk**: AI strategies may lose money in adverse conditions
**Liquidity Risk**: Withdrawals may face delays during high demand
**Technology Risk**: System failures or bugs could impact operations

Only stake what you can afford to lose.
</details>

<details>
<summary>How do I protect my funds?</summary>

**Security Best Practices**:
- Never share private keys or seed phrases
- Use hardware wallets for large amounts
- Verify contract addresses before transactions
- Be cautious of phishing attempts
- Keep wallet software updated

The protocol includes security measures but users must also practice good security habits.
</details>

## Platform Features

<details>
<summary>What is the AI chat system?</summary>

The AI chat features four specialized agents:
- **General Support**: Platform navigation and basic questions
- **Yield Analytics**: Performance insights and explanations
- **Technical Support**: Transaction help and troubleshooting
- **Security & Risk**: Risk assessment and security guidance

Available at pupas.ai/assistant for user assistance.
</details>

<details>
<summary>Can I use Pupas Protocol on mobile?</summary>

Yes, the platform is mobile-responsive and works on mobile devices through web browsers. Access the same functionality as desktop users.
</details>

<details>
<summary>Are there any geographic restrictions?</summary>

Pupas Protocol is accessible globally, but users must comply with their local regulations regarding cryptocurrency and DeFi participation. We recommend consulting local legal advice if unsure about regulatory compliance.
</details>

## Development and Future

<details>
<summary>What's planned for future development?</summary>

The protocol continues development with focus on:
- Enhanced AI capabilities
- Improved user interface
- Additional security measures
- Community feedback integration

See the [Development Roadmap](../protocol/roadmap.md) for current development phases.
</details>

<details>
<summary>Will there be a governance token?</summary>

**Yes**, a governance token is planned for **Q4 2025**. Token holders will be able to:
- Vote on protocol parameters and fee structures
- Select AI investment strategies
- Propose new features and improvements
- Participate in revenue sharing (under consideration)
</details>

## Troubleshooting

<details>
<summary>My wallet won't connect to the platform</summary>

**Common Solutions**:
1. **Refresh Page**: Clear browser cache and reload
2. **Check Network**: Ensure wallet is set to Waves mainnet
3. **Update Wallet**: Use latest version of wallet software
4. **Try Different Browser**: Some wallets work better in specific browsers
5. **Disable Ad Blockers**: May interfere with wallet connections

Contact support if issues persist.
</details>

<details>
<summary>I can't see my LP tokens in my wallet</summary>

LP tokens may not appear automatically in all wallets. To add them manually:
1. **Find Contract Address**: Get LP token contract from platform
2. **Add Custom Token**: Use wallet's "Add Token" feature
3. **Enter Details**: Contract address and token information
4. **Refresh Wallet**: LP tokens should now be visible

Your tokens are safe on the blockchain even if not visible in wallet interface.
</details>

<details>
<summary>The platform shows different numbers than my wallet</summary>

**Possible Causes**:
- **Price Updates**: LP prices update hourly, wallet may show outdated value
- **Network Sync**: Blockchain sync delays between platform and wallet
- **Display Precision**: Different decimal precision in calculations

Use the platform dashboard for most accurate, real-time information.
</details>

## Getting Help

<details>
<summary>How can I get additional support?</summary>

**Support Options**:
- **AI Chat**: pupas.ai/assistant - Automated support system
- **Community Channels**: Discord and Telegram for discussions
- **Documentation**: Comprehensive guides and tutorials
- **Direct Contact**: Email support for complex issues

Start with AI chat for immediate assistance.
</details>

<details>
<summary>How do I report a bug or security issue?</summary>

**Bug Reports**: Use community channels or contact support directly

**Security Issues**: 
- Report security concerns through appropriate channels
- Provide detailed information about potential issues
- Follow responsible disclosure practices

Security reports are taken seriously and investigated promptly.
</details>

## Still Have Questions?

{% hint style="info" %}
**Get Help**

Use our AI chat system for immediate assistance with platform questions.

[Ask AI Agents →](https://pupas.ai/assistant)
{% endhint %}

---

*This FAQ covers the most common questions about Pupas Protocol. For additional information, explore our documentation or contact support.*
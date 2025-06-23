# Technical Information

**Protocol addresses and blockchain specifications**

This page contains all essential technical information for interacting with Pupas Protocol on the Waves blockchain.

## Smart Contract Addresses

### Main Protocol Contract
- **Address**: `3P6Ns1K7qVYkXxeQqgQT5B7Ldu6squzroDM`
- **Type**: DApp (Decentralized Application)
- **Network**: Waves Mainnet
- **Language**: RIDE

### Treasury Address
- **Address**: `3P6GcYvq5SvZJVFV9wT9NVkavAL1MJzARmo`
- **Purpose**: Protocol fee collection (0.3% of stake amount)
- **Access**: Controlled by protocol governance

## Supported Token Addresses

### USDTu (Waves Native)
- **Asset ID**: `34N9YcEETLWn93qYQ64EsP1x89tSruJU44RrEMSXXEPJ`
- **Symbol**: USDTu
- **Decimals**: 6
- **Network**: Waves Mainnet
- **Type**: Native Waves token

### USDT-ERC20 (Bridged)
- **Symbol**: USDT-ERC20
- **Network**: Ethereum → Waves (via gateway)
- **Type**: Bridged token
- **Status**: Supported for staking

### LP Token
- **Name**: PUPAS LP
- **Description**: PUPAS AI TREASURY LP
- **Decimals**: 6
- **Type**: Reissuable Waves token
- **Supply**: Dynamic (minted/burned based on staking)

## Contract Functions

### Public Functions

#### provide()
```ride
@Callable(i)
func provide() = { ... }
```
- **Purpose**: Stake USDTu or USDT-ERC20 tokens
- **Input**: Payment attachment (USDTu or USDT-ERC20)
- **Output**: LP tokens minted to caller
- **Fee**: 0.3% protocol fee sent to treasury

#### withdraw(assetIdStr: String, lendMarketStr: String)
```ride
@Callable(i)
func withdraw(assetIdStr: String, lendMarketStr: String) = { ... }
```
- **Purpose**: Burn LP tokens and withdraw USDTu/USDT-ERC20
- **Input**: LP token payment + asset type selection
- **Output**: USDTu or USDT-ERC20 returned to caller
- **Fee**: No withdrawal fee

#### updateLpPrice(lpPrice: Int)
```ride
@Callable(i)
func updateLpPrice(lpPrice: Int) = { ... }
```
- **Purpose**: Update LP token price via oracle
- **Access**: Self-invoke only (contract governance)
- **Validation**: Maximum 10% price change per update
- **Frequency**: Hourly updates

### Administrative Functions

#### init(usdtIdStr: String, usdtuIdStr: String)
```ride
@Callable(i)
func init(usdtIdStr: String, usdtuIdStr: String) = { ... }
```
- **Purpose**: Initialize protocol with supported tokens
- **Access**: Self-invoke only
- **Output**: Creates LP token and sets configuration

## Protocol Constants

### Fee Structure
- **Protocol Fee**: `MintFee = 3000` (0.3% with 6 decimal precision)
- **Scale Factor**: `Scale6 = 1000000` (6 decimal precision)
- **Fee Destination**: Treasury address

### Risk Management
- **Oracle Volatility Tolerance**: `100000` (10% maximum price change)
- **Price Update Frequency**: Hourly
- **Validation**: Comprehensive error checking

## Data Storage Keys

### Configuration Keys
- `setup_usdtId`: USDT-ERC20 asset ID configuration
- `setup_usdtuId`: USDTu asset ID configuration  
- `setup_lpId`: LP token asset ID configuration

### Runtime Data
- `global_lpPrice`: Current LP token price (6 decimal precision)

## Network Information

### Waves Blockchain
- **Network**: Mainnet
- **Block Time**: ~1 minute
- **Transaction Fees**: ~0.005 WAVES
- **Confirmation Time**: 1-2 minutes
- **Explorer**: [wavesexplorer.com](https://wavesexplorer.com)

### Contract Explorer Links
- **Main Contract**: [wavesexplorer.com/addresses/3P6Ns1K7qVYkXxeQqgQT5B7Ldu6squzroDM](https://wavesexplorer.com/addresses/3P6Ns1K7qVYkXxeQqgQT5B7Ldu6squzroDM)
- **Treasury**: [wavesexplorer.com/addresses/3P6GcYvq5SvZJVFV9wT9NVkavAL1MJzARmo](https://wavesexplorer.com/addresses/3P6GcYvq5SvZJVFV9wT9NVkavAL1MJzARmo)
- **USDTu Token**: [wavesexplorer.com/assets/34N9YcEETLWn93qYQ64EsP1x89tSruJU44RrEMSXXEPJ](https://wavesexplorer.com/assets/34N9YcEETLWn93qYQ64EsP1x89tSruJU44RrEMSXXEPJ)

## Integration Guide

### For Developers

#### Contract Interaction
```ride
# Example transaction to stake USDTu
{
  "type": 16,
  "version": 1,
  "dApp": "3P6Ns1K7qVYkXxeQqgQT5B7Ldu6squzroDM",
  "call": {
    "function": "provide",
    "args": []
  },
  "payment": [{
    "amount": 1000000000,  # 1000 USDTu (6 decimals)
    "assetId": "34N9YcEETLWn93qYQ64EsP1x89tSruJU44RrEMSXXEPJ"
  }]
}
```

#### Data Queries
```ride
# Get current LP price
GET /addresses/data/3P6Ns1K7qVYkXxeQqgQT5B7Ldu6squzroDM/global_lpPrice

# Get LP token asset ID
GET /addresses/data/3P6Ns1K7qVYkXxeQqgQT5B7Ldu6squzroDM/setup_lpId
```

### For Users

#### Required Wallet Setup
- **Waves-compatible wallet** (Waves.Exchange, Keeper, WX Network)
- **WAVES tokens** for transaction fees
- **USDTu or USDT-ERC20** for staking

#### Transaction Flow
1. **Connect Wallet**: To pupas.ai platform
2. **Select Token**: USDTu or USDT-ERC20
3. **Enter Amount**: Minimum stake amount
4. **Confirm Transaction**: Pay network fees + protocol fee
5. **Receive LP Tokens**: Automatically minted to your wallet

{% hint style="info" %}
**Developer Resources**: For technical integration support, visit our [GitHub repository](https://github.com/pupas-protocol) or contact technical support via the AI chat system.
{% endhint %}

{% hint style="warning" %}
**Security Notice**: Always verify contract addresses before interacting. Never share private keys or seed phrases with anyone.
{% endhint %} 
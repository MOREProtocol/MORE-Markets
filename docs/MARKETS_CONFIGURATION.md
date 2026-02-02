# Markets Configuration

This document details the configuration parameters for all active markets on MORE Markets.

## Flow EVM Mainnet Markets

### Market Overview

| Asset | Status | Collateral | Borrowable | LTV | Liquidation Threshold | Liquidation Bonus |
|-------|--------|------------|------------|-----|----------------------|-------------------|
| WFLOW | Active | Yes | Yes | 81.50% | 83.00% | 5.00% |
| ankrFLOW | Active | Yes | Yes | 78.50% | 81.00% | 6.00% |
| WETH | Active | Yes | Yes | 80.00% | 83.00% | 5.00% |
| WBTC | Frozen | Yes | No | 73.00% | 78.00% | 5.00% |
| cbBTC | Frozen | Yes | Yes | 73.00% | 78.00% | 7.50% |
| USDF | Active | Yes | Yes | 75.00% | 79.25% | 7.50% |
| stgUSDC | Active | Yes | Yes | 75.00% | 78.00% | 7.50% |
| PYUSD0 | Active | Yes | Yes | 75.00% | 78.00% | 7.50% |
| USDC.e | Frozen | No | No | - | - | - |

---

## Detailed Market Parameters

### WFLOW (Wrapped Flow)

The native Flow token wrapped for EVM compatibility.

| Parameter | Value |
|-----------|-------|
| **Underlying Asset** | `0xd3bF53DAC106A0290B0483EcBC89d40FcC961f3e` |
| **Decimals** | 18 |
| **aToken** | `0x02BF4bd075c1b7C8D85F54777eaAA3638135c059` |
| **Variable Debt Token** | `0x8ba0641DBB9Cd2dBEDFCb8f9F179D88289F6b6C0` |
| **Stable Debt Token** | `0x4D050f27e52b5127BE9dc24d70813960b9488Fe6` |
| **Oracle** | `0xA0f0D87c10A5DedBD79c2EE116bea1339bF5525D` |

**Risk Parameters:**
| Parameter | Value |
|-----------|-------|
| LTV | 81.50% |
| Liquidation Threshold | 83.00% |
| Liquidation Bonus | 5.00% |
| Reserve Factor | 15.00% |
| Supply Cap | Unlimited |
| Borrow Cap | Unlimited |

**Features:**
| Feature | Enabled |
|---------|---------|
| Collateral | Yes |
| Borrowing | Yes |
| Stable Borrow | No |
| Active | Yes |
| Frozen | No |

---

### ankrFLOW (Ankr Staked Flow)

Liquid staking token for Flow via Ankr.

| Parameter | Value |
|-----------|-------|
| **Underlying Asset** | `0x1b97100eA1D7126C4d60027e231EA4CB25314bdb` |
| **Decimals** | 18 |
| **aToken** | `0xD10cd10260e87eFdf36618621458eeAA996B8267` |
| **Variable Debt Token** | `0x8e2052FBaBdA6Cae8AE011Fbe36f4017FF97f202` |
| **Stable Debt Token** | `0x54AbE1Aa16941a3B62Cb75395eDba1AA2Da12c10` |
| **Oracle** | `0xB9c94fc69bf20FAB29c146B1EFA1b2671328d5dC` |

**Risk Parameters:**
| Parameter | Value |
|-----------|-------|
| LTV | 78.50% |
| Liquidation Threshold | 81.00% |
| Liquidation Bonus | 6.00% |
| Reserve Factor | 15.00% |
| Supply Cap | Unlimited |
| Borrow Cap | Unlimited |

**Features:**
| Feature | Enabled |
|---------|---------|
| Collateral | Yes |
| Borrowing | Yes |
| Stable Borrow | No |
| Active | Yes |
| Frozen | No |

---

### WETH (Wrapped Ether)

Bridged Ethereum wrapped token.

| Parameter | Value |
|-----------|-------|
| **Underlying Asset** | `0x2F6F07CDcf3588944Bf4C42aC74ff24bF56e7590` |
| **Decimals** | 18 |
| **aToken** | `0xaD412305cF8aD9545759466F3d889438598F773F` |
| **Variable Debt Token** | `0x152F64483b8253E426ad4b6F600096f73b727D84` |
| **Stable Debt Token** | `0x61B8ec42f939d4e7F94c865D354a8CB3c4cA1123` |
| **Oracle** | `0x0156381a3EEa2552034821FE5CeeFD3c2D8a1D6C` |

**Risk Parameters:**
| Parameter | Value |
|-----------|-------|
| LTV | 80.00% |
| Liquidation Threshold | 83.00% |
| Liquidation Bonus | 5.00% |
| Reserve Factor | 15.00% |
| Supply Cap | Unlimited |
| Borrow Cap | Unlimited |

**Features:**
| Feature | Enabled |
|---------|---------|
| Collateral | Yes |
| Borrowing | Yes |
| Stable Borrow | No |
| Active | Yes |
| Frozen | No |

---

### WBTC (Wrapped Bitcoin)

Bridged Bitcoin wrapped token.

| Parameter | Value |
|-----------|-------|
| **Underlying Asset** | `0x717DAE2BaF7656BE9a9B01deE31d571a9d4c9579` |
| **Decimals** | 8 |
| **aToken** | `0x43658f281Ac6D4eC98b5d1a438b38B9113E6C5CB` |
| **Variable Debt Token** | `0x744c633418f3F6554edEAccaa378342697847B54` |
| **Stable Debt Token** | `0xb4CBa123264d63c407d09D99DA17c93833D5c7d9` |
| **Oracle** | `0xA406284ceEE3A3d5fdbF728Ef6b56A8788a6DAb9` |

**Risk Parameters:**
| Parameter | Value |
|-----------|-------|
| LTV | 73.00% |
| Liquidation Threshold | 78.00% |
| Liquidation Bonus | 5.00% |
| Reserve Factor | 25.00% |
| Supply Cap | Unlimited |
| Borrow Cap | 2 WBTC |

**Features:**
| Feature | Enabled |
|---------|---------|
| Collateral | Yes |
| Borrowing | No |
| Stable Borrow | No |
| Active | Yes |
| Frozen | Yes |

---

### cbBTC (Coinbase Wrapped Bitcoin)

Coinbase wrapped Bitcoin token.

| Parameter | Value |
|-----------|-------|
| **Underlying Asset** | `0xA0197b2044D28b08Be34d98b23c9312158Ea9A18` |
| **Decimals** | 8 |
| **aToken** | `0x72756F76630DfFea4Db019960b00139aa123c2bE` |
| **Variable Debt Token** | `0xB09b46bCf9e8B7dc538Ad214210cBC514F370CDa` |
| **Stable Debt Token** | `0x5e3EaCBC3A69237C1da727A19E54EaB09d31BFd6` |
| **Oracle** | `0x792BE9aF3e07E9C27B7575AF31c678FE4bf2a0AB` |

**Risk Parameters:**
| Parameter | Value |
|-----------|-------|
| LTV | 73.00% |
| Liquidation Threshold | 78.00% |
| Liquidation Bonus | 7.50% |
| Reserve Factor | 20.00% |
| Supply Cap | Unlimited |
| Borrow Cap | Unlimited |

**Features:**
| Feature | Enabled |
|---------|---------|
| Collateral | Yes |
| Borrowing | Yes |
| Stable Borrow | No |
| Active | Yes |
| Frozen | Yes |

---

### USDF (USD Flow)

Native Flow stablecoin.

| Parameter | Value |
|-----------|-------|
| **Underlying Asset** | `0x2aaBea2058b5aC2D339b163C6Ab6f2b6d53aabED` |
| **Decimals** | 6 |
| **aToken** | `0xe43fe00AEA059f3A756CF556655B5A27FAf9bEC5` |
| **Variable Debt Token** | `0x1c80CE0D97B15e4f7a3914CABf60E88F97D6E806` |
| **Stable Debt Token** | `0xe690807E8427328b325a58363f8538E7DdF79160` |
| **Oracle** | `0x69427CDe8C085c3CB44f2666784ECe8721298A7c` |

**Risk Parameters:**
| Parameter | Value |
|-----------|-------|
| LTV | 75.00% |
| Liquidation Threshold | 79.25% |
| Liquidation Bonus | 7.50% |
| Reserve Factor | 20.00% |
| Supply Cap | Unlimited |
| Borrow Cap | Unlimited |

**Features:**
| Feature | Enabled |
|---------|---------|
| Collateral | Yes |
| Borrowing | Yes |
| Stable Borrow | No |
| Active | Yes |
| Frozen | No |

---

### stgUSDC (Stargate USDC)

USDC bridged via Stargate/LayerZero.

| Parameter | Value |
|-----------|-------|
| **Underlying Asset** | `0xF1815bd50389c46847f0Bda824eC8da914045D14` |
| **Decimals** | 6 |
| **aToken** | `0x49c6b2799aF2Db7404b930F24471dD961CFE18b7` |
| **Variable Debt Token** | `0xbD6e2ae2c8A0e3AA8f694C795cb0E7cbB6199d44` |
| **Stable Debt Token** | `0x2be3022E433a2c93Eb1898ceE555D88Ab840fE07` |
| **Oracle** | `0xa03738DB9B78ee011A1607B4726c11862eaB1F40` |

**Risk Parameters:**
| Parameter | Value |
|-----------|-------|
| LTV | 75.00% |
| Liquidation Threshold | 78.00% |
| Liquidation Bonus | 7.50% |
| Reserve Factor | 10.00% |
| Supply Cap | Unlimited |
| Borrow Cap | Unlimited |

**Features:**
| Feature | Enabled |
|---------|---------|
| Collateral | Yes |
| Borrowing | Yes |
| Stable Borrow | No |
| Active | Yes |
| Frozen | No |

---

### PYUSD0 (PayPal USD)

PayPal USD stablecoin.

| Parameter | Value |
|-----------|-------|
| **Underlying Asset** | `0x99aF3EeA856556646C98c8B9b2548Fe815240750` |
| **Decimals** | 6 |
| **aToken** | `0x8Dc4cD78A28D83dc5B564E6b9Fb611DF6Dca3b3C` |
| **Variable Debt Token** | `0x3DE89be09e5E390CDef8a853c78e55d33182D62C` |
| **Stable Debt Token** | `0x37aD85F78ebA83DF9dA4c162b95c668f6189a2A4` |
| **Oracle** | `0x69427CDe8C085c3CB44f2666784ECe8721298A7c` |

**Risk Parameters:**
| Parameter | Value |
|-----------|-------|
| LTV | 75.00% |
| Liquidation Threshold | 78.00% |
| Liquidation Bonus | 7.50% |
| Reserve Factor | 10.00% |
| Supply Cap | Unlimited |
| Borrow Cap | 5,000,000 PYUSD0 |

**Features:**
| Feature | Enabled |
|---------|---------|
| Collateral | Yes |
| Borrowing | Yes |
| Stable Borrow | No |
| Active | Yes |
| Frozen | No |

---

### USDC.e (Bridged USDC)

Legacy bridged USDC (deprecated).

| Parameter | Value |
|-----------|-------|
| **Underlying Asset** | `0x7f27352D5F83Db87a5A3E00f4B07Cc2138D8ee52` |
| **Decimals** | 6 |
| **aToken** | `0x9069AB13a398F1B57670857FcCF95Fc74Fba6c82` |
| **Variable Debt Token** | `0x160994C2625D644AeF4b1A1C81312e78146B97C6` |
| **Stable Debt Token** | `0x14e81eba45dab782ECB024e68d18c071599274B9` |
| **Oracle** | `0xa03738DB9B78ee011A1607B4726c11862eaB1F40` |

**Risk Parameters:**
| Parameter | Value |
|-----------|-------|
| LTV | 0% |
| Liquidation Threshold | 0% |
| Liquidation Bonus | 0% |
| Reserve Factor | 0% |
| Supply Cap | Unlimited |
| Borrow Cap | Unlimited |

**Features:**
| Feature | Enabled |
|---------|---------|
| Collateral | No |
| Borrowing | No |
| Stable Borrow | No |
| Active | Yes |
| Frozen | Yes |

---

## Risk Parameter Definitions

| Parameter | Description |
|-----------|-------------|
| **LTV (Loan-to-Value)** | Maximum borrowing power of a specific collateral. A 75% LTV means users can borrow up to 75% of their collateral value. |
| **Liquidation Threshold** | The threshold at which a position is considered undercollateralized and subject to liquidation. |
| **Liquidation Bonus** | The bonus received by liquidators as an incentive to perform liquidations. A 5% bonus means liquidators receive 5% extra collateral. |
| **Reserve Factor** | The percentage of interest that goes to the protocol treasury. |
| **Supply Cap** | Maximum amount of the asset that can be supplied to the protocol. |
| **Borrow Cap** | Maximum amount of the asset that can be borrowed from the protocol. |

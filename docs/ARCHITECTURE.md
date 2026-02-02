# Architecture

This document provides an overview of the MORE Markets protocol architecture.

## Protocol Overview

MORE Markets is a decentralized liquidity protocol built on the Aave V3 architecture, deployed on Flow EVM. The protocol enables permissionless lending and borrowing of digital assets.

```mermaid
flowchart TB
    subgraph Users
        Supplier[Supplier]
        Borrower[Borrower]
        Liquidator[Liquidator]
    end

    subgraph Core["Core Protocol"]
        Pool[Pool]
        PoolConfigurator[Pool Configurator]
        ACL[ACL Manager]
    end

    subgraph Tokens["Token Layer"]
        aToken[aTokens]
        VDebt[Variable Debt Tokens]
        SDebt[Stable Debt Tokens]
    end

    subgraph Oracle["Price Oracle"]
        AaveOracle[Aave Oracle]
        PriceFeed[Price Feeds]
    end

    subgraph Periphery["Periphery"]
        Gateway[Wrapped Token Gateway]
        DataProvider[Pool Data Provider]
        UIProvider[UI Data Providers]
    end

    Supplier -->|supply| Pool
    Borrower -->|borrow| Pool
    Liquidator -->|liquidate| Pool

    Pool --> aToken
    Pool --> VDebt
    Pool --> SDebt
    Pool --> AaveOracle

    AaveOracle --> PriceFeed

    PoolConfigurator --> Pool
    ACL --> PoolConfigurator

    Gateway --> Pool
    DataProvider --> Pool
    UIProvider --> Pool
```

## Contract Architecture

### Core Contracts

```mermaid
flowchart LR
    subgraph Registry
        PAR[PoolAddressesProviderRegistry]
        PAP[PoolAddressesProvider]
    end

    subgraph Pool
        PoolProxy[Pool Proxy]
        PoolImpl[Pool Implementation]
    end

    subgraph Config
        ConfigProxy[Configurator Proxy]
        ConfigImpl[Configurator Implementation]
    end

    subgraph Access
        ACL[ACL Manager]
    end

    PAR --> PAP
    PAP --> PoolProxy
    PAP --> ConfigProxy
    PAP --> ACL

    PoolProxy --> PoolImpl
    ConfigProxy --> ConfigImpl
```

### Token Architecture

For each supported asset, the protocol deploys three token contracts:

```mermaid
flowchart TB
    subgraph Asset["Underlying Asset (e.g., WFLOW)"]
        Token[ERC20 Token]
    end

    subgraph Protocol["Protocol Tokens"]
        aToken["aToken<br/>(Interest-bearing)"]
        VDebt["Variable Debt Token<br/>(Borrowed amount)"]
        SDebt["Stable Debt Token<br/>(Fixed rate debt)"]
    end

    Token -->|deposit| aToken
    aToken -->|withdraw| Token

    Token -->|borrow| VDebt
    VDebt -->|repay| Token

    Token -->|borrow stable| SDebt
    SDebt -->|repay| Token
```

## Interest Rate Model

The protocol uses a variable interest rate model based on utilization:

```mermaid
flowchart LR
    subgraph Inputs
        U[Utilization Rate]
        Optimal[Optimal Utilization]
    end

    subgraph Strategy["Interest Rate Strategy"]
        Base[Base Rate]
        Slope1[Slope 1<br/>Below Optimal]
        Slope2[Slope 2<br/>Above Optimal]
    end

    subgraph Output
        BorrowRate[Borrow Rate]
        SupplyRate[Supply Rate]
    end

    U --> Strategy
    Optimal --> Strategy
    Strategy --> BorrowRate
    BorrowRate --> SupplyRate
```

**Rate Calculation:**
- When Utilization <= Optimal: `Rate = Base + (Utilization / Optimal) * Slope1`
- When Utilization > Optimal: `Rate = Base + Slope1 + ((Utilization - Optimal) / (1 - Optimal)) * Slope2`

## Liquidation Flow

```mermaid
sequenceDiagram
    participant L as Liquidator
    participant P as Pool
    participant O as Oracle
    participant C as Collateral aToken
    participant D as Debt Token

    L->>P: liquidationCall(collateral, debt, user, amount)
    P->>O: getAssetPrice(collateral)
    P->>O: getAssetPrice(debt)
    P->>P: Calculate health factor

    alt Health Factor < 1
        P->>D: burn(user, debtAmount)
        P->>C: transfer(liquidator, collateralAmount + bonus)
        P-->>L: Liquidation successful
    else Health Factor >= 1
        P-->>L: Revert: Position healthy
    end
```

## Access Control

```mermaid
flowchart TB
    subgraph Roles
        Admin[Pool Admin]
        Emergency[Emergency Admin]
        Risk[Risk Admin]
        Asset[Asset Listing Admin]
    end

    subgraph Permissions
        P1[Configure reserves]
        P2[Pause protocol]
        P3[Update risk params]
        P4[Add new assets]
    end

    Admin --> P1
    Admin --> P3
    Admin --> P4
    Emergency --> P2
    Risk --> P3
    Asset --> P4
```

## Data Flow

### Supply Operation

```mermaid
sequenceDiagram
    participant U as User
    participant G as Gateway
    participant P as Pool
    participant A as aToken
    participant T as Treasury

    U->>G: supply(amount)
    G->>P: supply(asset, amount, user)
    P->>P: Update interest rates
    P->>A: mint(user, amount)
    A-->>U: Receive aTokens
```

### Borrow Operation

```mermaid
sequenceDiagram
    participant U as User
    participant P as Pool
    participant O as Oracle
    participant D as Debt Token
    participant A as Asset

    U->>P: borrow(asset, amount)
    P->>O: getAssetPrice(collateral)
    P->>P: Check borrowing power

    alt Sufficient collateral
        P->>D: mint(user, amount)
        P->>A: transfer(user, amount)
        A-->>U: Receive borrowed asset
    else Insufficient collateral
        P-->>U: Revert
    end
```

## Upgradability

The protocol uses a proxy pattern for upgradability:

```mermaid
flowchart TB
    subgraph Proxies
        PP[Pool Proxy]
        CP[Configurator Proxy]
    end

    subgraph Implementations
        PI1[Pool Impl v1]
        PI2[Pool Impl v2]
        CI1[Config Impl v1]
    end

    subgraph Admin
        PA[Proxy Admin]
    end

    PP -.->|delegatecall| PI1
    PP -.->|upgrade| PI2
    CP -.->|delegatecall| CI1

    PA -->|upgrade| PP
    PA -->|upgrade| CP
```

## External Integrations

```mermaid
flowchart LR
    subgraph Protocol
        Pool[MORE Markets Pool]
    end

    subgraph Oracles
        Pyth[Pyth Network]
        Custom[Custom Feeds]
    end

    subgraph Bridges
        LZ[LayerZero]
        SG[Stargate]
    end

    subgraph DeFi
        Vaults[MORE Vaults]
        DEX[DEX Aggregators]
    end

    Pyth --> Pool
    Custom --> Pool

    LZ --> Pool
    SG --> Pool

    Pool --> Vaults
    Pool --> DEX
```

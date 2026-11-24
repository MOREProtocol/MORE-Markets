# MORE Markets — WFLOW market exploit, 16 Nov 2026: post-mortem

On 16 November 2026 an attacker drained the WFLOW market of MORE Markets on Flow EVM, leaving 18.36M WFLOW of debt with no collateral. That debt has since been cleared: WFLOW deposits are fully backed, and withdrawals have been open since 20 November.

## At a glance

|  |  |
| --- | --- |
| When | 16 November 2026, 17:30 to 17:37 UTC |
| Where | The WFLOW market of MORE Markets on Flow EVM (chain ID 747) |
| How | Flash-loaned e-mode positions taken out of e-mode and liquidated by the attacker itself, 154 times. No keys were compromised and no admin function was used. |
| Bad debt created | 18,357,694 WFLOW, about $297,900 at $0.01623 per FLOW |
| Bad debt today | None: the debt has been cleared |
| WFLOW depositors | Deposits fully backed; no principal lost |
| Not affected | The ankrFLOW token and the other eight markets |
| Funds recovered from the attacker | None so far |
| Status since 20 Nov | All markets frozen (no new deposits or borrows); withdrawals, repayments and liquidations open |

## Timeline

The exploit took seven minutes and the proceeds were off Flow EVM ten minutes later. All times are UTC on 16 November 2026 unless a date is given.

| Time (UTC) | Event |
| --- | --- |
| 17:29:49 | The attacker's wallet deploys the exploit contract |
| 17:30:15 | First of eleven exploit transactions |
| 17:36:21 | The attacker starts swapping the drained WFLOW to PYUSD0 |
| 17:37:07 | Eleventh and last exploit transaction |
| 17:38 to 17:47 | All the PYUSD0 is bridged to the attacker's address on Ethereum |
| 17:41:58 | An ordinary depositor withdraws the last 273,182 WFLOW; the reserve is empty |
| 17:50:23 | On Ethereum, the attacker sends 110.61 ETH to 0xA6A8…F6A |
| 17:55 | MORE posts that it has detected an exploit |
| 17:58:19 | The security council pauses all nine markets |
| 19:57:59 | MORE sends the attacker an on-chain offer: keep 10% and return 90% by 20 Nov, 23:00 UTC |
| 17 Nov, 14:43:35 | 0xA6A8…F6A forwards the 110.61 ETH to 0xCc11…8eA6 |
| 20 Nov, 19:00 | All markets frozen and unpaused in one transaction: withdrawals reopen |
| 20 Nov, 23:00 | The return deadline passes; no funds were returned |

## How the exploit worked

The attacker used the pool's own functions in sequence; no keys were stolen and no code was changed. Two behaviours combined: a borrower could leave e-mode while holding a loan that the standard settings then treat as liquidatable, and a borrower could liquidate its own position. In e-mode, ankrFLOW collateral could back WFLOW loans at 97% loan-to-value (97.5% liquidation threshold, 1% bonus). Outside e-mode, ankrFLOW has a lower threshold and a 6% liquidation bonus.

The exploit contract ran this cycle 14 times in each of its 11 transactions, 154 times in all:

1. Flash-loan WFLOW and ankrFLOW from the MORE Markets pool itself.
2. Deposit the ankrFLOW as collateral under e-mode.
3. Borrow WFLOW at up to 97% of the collateral's value.
4. Leave e-mode. Under ankrFLOW's standard settings the position is now liquidatable.
5. Liquidate the position from the attacker's own contract at the 6% bonus. A loan worth about 97% of the collateral plus a 6% bonus claims more than all of the collateral, so every unit of collateral is paid out before the debt is cleared. The rest of the debt stays on the account.
6. Repay the flash loans and keep the WFLOW left over.

Flash loans supplied all the working capital, so the attacker needed nothing beyond gas. About 2.7% of the WFLOW borrowed in each cycle stayed behind as debt with no collateral.

| Across all 154 cycles | Amount |
| --- | --- |
| WFLOW flash-loaned and borrowed | 671,740,200 WFLOW |
| ankrFLOW deposited as collateral | 565,905,386 ankrFLOW |
| Debt repaid by self-liquidation | 653,384,029 WFLOW |
| Collateral paid out to the attacker's contract | 562,702,148 ankrFLOW |
| Debt left with no collateral | 18,356,171 WFLOW |

These totals come from the pool's events. The debt-token supply rose by 18,357,694 WFLOW over the same blocks; the small gap is most likely interest accrued between the two reads.

## Impact

The loss is 18,357,694 WFLOW of debt with no collateral behind it, read on-chain at block 85,330,589.

| Measure | WFLOW | USD at $0.01623 per FLOW |
| --- | --- | --- |
| Debt left with no collateral (the loss) | 18,357,694 | \~$297,900 |
| WFLOW removed from the reserve | 18,629,353 | \~$302,400 |
| Depositor claims on the WFLOW reserve at the time | 93,548,473 | \~$1,518,300 |

The WFLOW removed is the attacker's 18,356,171 plus 273,182 WFLOW that an ordinary depositor withdrew at 17:41:58 UTC.

The dollar figures use $0.01623 per FLOW, the rate in our incident report. Blockaid's public alert at 17:47 UTC put the loss at about $308,000, and our 17 Nov update said about $348,000. The WFLOW amount is the figure that does not depend on the price used.

**Who was affected.** Several dozen WFLOW depositors could not withdraw from 16 to 20 November. The bad debt has since been cleared and every WFLOW deposit is fully backed, so no depositor lost principal.

**Not affected.** The ankrFLOW token and the other eight markets. The ankrFLOW market held 113,426,864 ankrFLOW against 113,426,754 deposited right after the attack.

## Response and recovery

The markets were paused 21 minutes after the last exploit transaction and reopened on 20 November with every market frozen. The bad debt has been cleared and every WFLOW deposit is backed.

1. **Pause.** The security council paused all nine markets at 17:58:19 UTC on 16 November ([0xcbc3…6341](https://evm.flow.com/tx/0xcbc3166fe5a0a2f3280fe111f59b0544f58999f7de8483ba189b037d03bf6341)). While paused, no deposit, withdrawal, loan, repayment or liquidation is possible.
2. **Offer to the attacker.** At 19:57:59 UTC we sent an on-chain message to the wallet then holding the proceeds, offering 10% if the other 90% was returned by 23:00 UTC on 20 November ([Ethereum tx 0x7278…0636](https://etherscan.io/tx/0x727869a60786ad9a3b58b1e0ec12fd2a312b4c36218f7190b60537d94fa90636)). No funds were returned.
3. **Freeze and reopening.** One transaction at 19:00 UTC on 20 November ([0x6812…5892](https://evm.flow.com/tx/0x6812564bcbfc6e63eda9abdb7d56e53bf0cf9d23956c171efe545295b0f15892)) did four things:
   - froze all nine markets, so no new deposits or borrows are possible;
   - unpaused them, so withdrawals, repayments and liquidations work again;
   - set the WFLOW interest-rate model to the one in force before 16 November;
   - cut the liquidation bonus on WFLOW and ankrFLOW collateral outside e-mode from 5% and 6% to 0.01%. In e-mode it stays at 1%. Loan-to-value ratios and liquidation thresholds are unchanged.
4. **The WFLOW market today.** The bad debt has been cleared. As of 23 November the market holds 14.99M WFLOW and has 72.26M WFLOW lent out, against 87.22M WFLOW of deposits: every deposit is backed.

The attack needed new deposits and borrows, which the freeze blocks. The bonus cut removes the payoff from taking an existing position out of e-mode and liquidating it.

## Where the funds went

The attacker turned the drained WFLOW into PYUSD0, bridged all of it to the same address on Ethereum by 17:47 UTC, and consolidated it into 110.61 ETH.

&#91;embedded content: where the proceeds went · Flow EVM to Ethereum, 16–17 Nov\]

None of the proceeds sit with an issuer that can freeze them: the PYUSD was all sold for ETH by 17:49 UTC, and native ETH has no issuer. We ask exchanges and bridges to flag the addresses listed under Reference.

## What users should do

No action is required to keep your funds safe; you can withdraw or repay at any time at app.more.markets.

- **Lenders:** withdraw at app.more.markets. WFLOW that is not lent out can be withdrawn now; the rest becomes available as borrowers repay or are liquidated.
- **Borrowers:** repay your loan, then withdraw your collateral. New borrowing is not possible while the markets are frozen.
- **If the app is unavailable:** the Pool contract listed under Reference can be called directly from the [Flow EVM explorer](https://evm.flow.com/address/0xbC92aaC2DBBF42215248B5688eB3D3d2b32F2c8d). Check the address character by character first.

**Stay safe.** The only official app is app.more.markets. There is no token, airdrop, claim or migration contract, and nobody from MORE will DM you. Scammers copy the first and last characters of real addresses, so always compare a full address before sending funds.

## Reference

All addresses are on Flow EVM (chain ID 747) unless marked Ethereum.

**Attacker addresses**

| Role | Address |
| --- | --- |
| Attacker wallet (same address on Ethereum) | [0xe1eA6F2365215389f9DCd50a51c974313BF6785C](https://evm.flow.com/address/0xe1eA6F2365215389f9DCd50a51c974313BF6785C) |
| Exploit contract, deployed at 17:29:49 UTC in [0xdb8a…d689](https://evm.flow.com/tx/0xdb8a1c945e1fc2e9d6d9afae64520c0ad99052999ca26441ad3333000465d689) | [0x9a8cC604b3534eAA1A8627c1D6f6084857Bf2765](https://evm.flow.com/address/0x9a8cC604b3534eAA1A8627c1D6f6084857Bf2765) |
| Ethereum wallet that received the 110.61 ETH | [0xA6A8D0583D25ab5d93681994E5667c9021dF6F6A](https://etherscan.io/address/0xA6A8D0583D25ab5d93681994E5667c9021dF6F6A) |
| Ethereum wallet the 110.61 ETH was forwarded to (it also holds ETH from other sources) | [0xCc11D2dE32B29AB73636D874a8AEc0Bb0Fe18eA6](https://etherscan.io/address/0xCc11D2dE32B29AB73636D874a8AEc0Bb0Fe18eA6) |

**The eleven exploit transactions** (16 November 2026)

| # | Time (UTC) | Transaction |
| --- | --- | --- |
| 1 | 17:30:15 | [0xdf9e3ed4858103569f684166d9833e1980b6ba96334c9a8ec172b4e2aa1d15d8](https://evm.flow.com/tx/0xdf9e3ed4858103569f684166d9833e1980b6ba96334c9a8ec172b4e2aa1d15d8) |
| 2 | 17:30:45 | [0xb18d3cf90d8b39a1ff6736b4692a4efd0064dbb23f5e7800e245272aff50ce8c](https://evm.flow.com/tx/0xb18d3cf90d8b39a1ff6736b4692a4efd0064dbb23f5e7800e245272aff50ce8c) |
| 3 | 17:31:14 | [0xcb47d6bc68226be0b1f609095c97e3abc0fe0eace4ebc924d7898a3a7da9d046](https://evm.flow.com/tx/0xcb47d6bc68226be0b1f609095c97e3abc0fe0eace4ebc924d7898a3a7da9d046) |
| 4 | 17:31:42 | [0x6743b6fade27a407a251781910e526d927863165ce1e6a59c8b27d01376c4aae](https://evm.flow.com/tx/0x6743b6fade27a407a251781910e526d927863165ce1e6a59c8b27d01376c4aae) |
| 5 | 17:32:12 | [0xdd6dce5796d9f17bdd81363c7cbcd156be6d624fe080df387e095ae3e26231b6](https://evm.flow.com/tx/0xdd6dce5796d9f17bdd81363c7cbcd156be6d624fe080df387e095ae3e26231b6) |
| 6 | 17:32:40 | [0x9979cedf3a5c55f888a3fb142c82adc55d0d8b3b0670c603ab905132193f52cc](https://evm.flow.com/tx/0x9979cedf3a5c55f888a3fb142c82adc55d0d8b3b0670c603ab905132193f52cc) |
| 7 | 17:33:12 | [0xaf1336e3199aa3c369a59b0b56cb2a5abf4ae9170df838ad77d38c83852e8b2b](https://evm.flow.com/tx/0xaf1336e3199aa3c369a59b0b56cb2a5abf4ae9170df838ad77d38c83852e8b2b) |
| 8 | 17:33:41 | [0xf2fbc2d3900b770c7a0fb862571eff24626cd214d29466a868eaa57aef695cbc](https://evm.flow.com/tx/0xf2fbc2d3900b770c7a0fb862571eff24626cd214d29466a868eaa57aef695cbc) |
| 9 | 17:34:08 | [0x604867f877952e617d77b57b4b24c8a99dfd01b2d5046ff4a97952e74e7ef138](https://evm.flow.com/tx/0x604867f877952e617d77b57b4b24c8a99dfd01b2d5046ff4a97952e74e7ef138) |
| 10 | 17:34:35 | [0x41c8cb0dc975ecc6796d9dc2e459ec13144d8ce260a87f26ff10b9c369c3a716](https://evm.flow.com/tx/0x41c8cb0dc975ecc6796d9dc2e459ec13144d8ce260a87f26ff10b9c369c3a716) |
| 11 | 17:37:07 | [0x59f730f44bbff02b3927c294279e00767dcd24aa7716969253a21244dd5667ad](https://evm.flow.com/tx/0x59f730f44bbff02b3927c294279e00767dcd24aa7716969253a21244dd5667ad) |

**MORE Markets contracts**

| Contract | Address |
| --- | --- |
| Pool | [0xbC92aaC2DBBF42215248B5688eB3D3d2b32F2c8d](https://evm.flow.com/address/0xbC92aaC2DBBF42215248B5688eB3D3d2b32F2c8d) |
| PoolAddressesProvider | [0x1830a96466d1d108935865c75B0a9548681Cfd9A](https://evm.flow.com/address/0x1830a96466d1d108935865c75B0a9548681Cfd9A) |
| mFlowWFLOW (WFLOW deposit token) | [0x02BF4bd075c1b7C8D85F54777eaAA3638135c059](https://evm.flow.com/address/0x02BF4bd075c1b7C8D85F54777eaAA3638135c059) |
| variableDebtWFLOW (WFLOW debt token) | [0x8ba0641DBB9Cd2dBEDFCb8f9F179D88289F6b6C0](https://evm.flow.com/address/0x8ba0641DBB9Cd2dBEDFCb8f9F179D88289F6b6C0) |
| WFLOW | [0xd3bF53DAC106A0290B0483EcBC89d40FcC961f3e](https://evm.flow.com/address/0xd3bF53DAC106A0290B0483EcBC89d40FcC961f3e) |
| ankrFLOW | [0x1b97100eA1D7126C4d60027e231EA4CB25314bdb](https://evm.flow.com/address/0x1b97100eA1D7126C4d60027e231EA4CB25314bdb) |

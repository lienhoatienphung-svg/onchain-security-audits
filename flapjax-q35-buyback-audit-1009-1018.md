# FlapJax Receipt Relay Q35: On-Chain Buyback Program Audit (2026-10-09 to 2026-10-18)

- **Auditor Handle:** `@lienhoatienphung-svg`
- **BSC Payout Wallet (EOA):** `0x3806cd592a508d1992Ed007C2F53de251C2E7452`
- **First-Timer Status:** **YES** (`first=yes` — FlapJax has never paid this operator, handle, or wallet; eligible for the first-timer reserved slot)
- **Target Task:** `TSK-88YN3100` (`0x1284d01695bfcd1720cfb319f658a258b82b6768501fc00cb9ab0748bc45fd35`)
- **Chain & Contracts Audited:**
  - Chain: BNB Smart Chain Mainnet (`chainId = 56`, verified via `https://bsc-dataseed.binance.org/`)
  - FLAPJAX Token (BEP-20, 18 decimals): `0x90c8889f428F9Ebb77BB8f15CAD3a50a9aC680df`
  - FlapJax Treasury (EOA): `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f`
  - PancakeSwap V3 FLAPJAX/WBNB 1% Pool (`fee = 10000`): `0xcA93EC685b8623688E6B236f9068034B298c2150`

---

## 1. Executive Summary & Statement Verdicts

| # | FlapJax Statement | Verdict | On-Chain Proof Summary |
|---|---|---|---|
| **S1** | *"Nine buys of 0.01 BNB each (0.09 BNB in total)"* | **MATCH** | Exactly 9 buyback transactions executed in the `10-09` to `10-18` UTC window; each transaction carries `tx.value = 10,000,000,000,000,000 wei` (`0.010000 BNB`), summing to `90,000,000,000,000,000 wei` (`0.090000 BNB`). |
| **S2** | *"One a day except 10-15, which was missed and is owed as a make-up buy on 10-23"* | **MISMATCH** | In UTC calendar dates, **two** dates had zero buys (**`2026-10-15` AND `2026-10-16`**), while **`2026-10-17` had two buys** (Buy #7 at `2026-10-17 02:00:51 UTC`, block `128086717`, and Buy #8 at `2026-10-17 13:29:27 UTC`, block `128178495`). Thus the 9 buys span 8 distinct UTC dates rather than 9. |
| **S3** | *"Every buy's FLAPJAX went to the treasury"* | **MATCH** | In all 9 transaction receipts, the ERC-20 `Transfer(address,address,uint256)` log (`topic0 = 0xddf252ad...`) on `0x90c8889f428F9Ebb77BB8f15CAD3a50a9aC680df` transfers from pool `0xcA93EC685b8623688E6B236f9068034B298c2150` directly to `topics[2] = 0x7c34e9e21ee28a49ff0b84b61774119e6633359f` (Treasury). |
| **S4** | *"The treasury sent no FLAPJAX to the pool in that window"* | **MATCH** | Across all treasury transactions in the `2026-10-09` to `2026-10-18` window (nonces `30` through `216`), **zero (`0`)** FLAPJAX `Transfer` logs have `from = Treasury` and `to = Pool` (`0xcA93EC685b8623688E6B236f9068034B298c2150`). |

---

## 2. Complete On-Chain Table of the 9 Buyback Buys (2026-10-09 to 2026-10-18 UTC)

Every row below was retrieved directly from BSC JSON-RPC (`eth_getTransactionByHash`, `eth_getTransactionReceipt`, and `eth_getBlockByNumber`). All 9 transfers originated from Pool `0xcA93EC685b8623688E6B236f9068034B298c2150` and were received by Treasury `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f`.

| # | UTC Timestamp | Block | Treasury Nonce | Transaction Hash | Log Idx | BNB Spent | FLAPJAX Received (Exact 18-dec `uint256` wei) | FLAPJAX Received (Decimal) | Execution Price (BNB / FLAPJAX) |
|---|---|---|---|---|---|---|---|---|---|
| **1** | `2026-10-09 18:30:53Z` | `126684151` | `30` | `0xbeefabdc89f9148f9daaef6082d34a60dc5b4b612972ca0fd39d6bd4eb03b4b0` | `100` | `0.010000` | `24052995308271524768278040` | `24,052,995.308271524768278040` | `4.157486364e-10` |
| **2** | `2026-10-10 18:27:56Z` | `126875701` | `47` | `0x8ce8e8578da26f8c7acb0f44c6ca0d0b8dcba868cafe2d729bb48562863f2d62` | `308` | `0.010000` | `23979958557395636288483739` | `23,979,958.557395636288483739` | `4.170148992e-10` |
| **3** | `2026-10-11 18:30:35Z` | `127067713` | `72` | `0x82d01172ae26f1c86287643e123af1675ca5c18b984b7740c1a0c1647d2801eb` | `423` | `0.010000` | `23907253965589989685035768` | `23,907,253.965589989685035768` | `4.182830874e-10` |
| **4** | `2026-10-12 18:46:27Z` | `127261246` | `87` | `0x9afdc96e3ca876b5b617a26ba5105012ed8c33e805c77b1616bf48c198d5cbf1` | `40` | `0.010000` | `23834879521763892137228328` | `23,834,879.521763892137228328` | `4.195532011e-10` |
| **5** | `2026-10-13 18:41:15Z` | `127452454` | `104` | `0xc94de373d1fb00093431224a97475b3b2b48b9a4962d15acbe9d9c5c016d720c` | `256` | `0.010000` | `23762833230024087278551319` | `23,762,833.230024087278551319` | `4.208252401e-10` |
| **6** | `2026-10-14 18:32:29Z` | `127643212` | `137` | `0x172349c64ed3c45663650049f9e2ad3d763b1243c5c849b7d66f144f221d3186` | `200` | `0.010000` | `23691113109537150143912753` | `23,691,113.109537150143912753` | `4.220992046e-10` |
| **7** | `2026-10-17 02:00:51Z` | `128086717` | `162` | `0xc999000f16f326e8868fe37f83871219be1a63f98043b0aef9ee57f469232156` | `512` | `0.010000` | `23669235465921230581203552` | `23,669,235.465921230581203552` | `4.224893539e-10` |
| **8** | `2026-10-17 13:29:27Z` | `128178495` | `179` | `0x6ec66780e0518da7c3c8b55d13b66d57400046f89ebe9df479c3c878f7b3311f` | `382` | `0.010000` | `23597938349722733902073707` | `23,597,938.349722733902073707` | `4.237658329e-10` |
| **9** | `2026-10-18 13:28:39Z` | `128370339` | `216` | `0x05a9acf81b0eae4b7ff9a12168aead25964b6b2c0a16d3d1e667b7db6f17ad99` | `752` | `0.010000` | `23506585156697207098877431` | `23,506,585.156697207098877431` | `4.254127060e-10` |

- **Recipient on all 9 Transfer logs:** `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f` (FlapJax Treasury)
- **Gas Fees across the 9 buybacks:** `0.0000573319 BNB` total (`~0.00000637 BNB` per buy at `0.05 gwei`).

---

## 3. Totals, Average Price, First Buy Price, and Last Buy Price (With Formulas)

1. **Total BNB Spent (`B_total`):**
   ```text
   B_total = 9 * 0.010000 BNB = 0.090000 BNB (90,000,000,000,000,000 wei)
   ```
2. **Total FLAPJAX Received (`F_total`):**
   ```text
   F_total (wei) = 24052995308271524768278040
                 + 23979958557395636288483739
                 + 23907253965589989685035768
                 + 23834879521763892137228328
                 + 23762833230024087278551319
                 + 23691113109537150143912753
                 + 23669235465921230581203552
                 + 23597938349722733902073707
                 + 23506585156697207098877431
                 = 214,002,792,664,923,451,883,644,637 wei
   F_total (FLAPJAX) = F_total (wei) / 10^18 = 214,002,792.664923451883644637 FLAPJAX
   ```
3. **Volume-Weighted Average Price Paid (`P_avg_weighted`):**
   ```text
   P_avg_weighted = B_total / F_total
                  = 0.09 BNB / 214,002,792.664923451883644637 FLAPJAX
                  = 4.205552595e-10 BNB per FLAPJAX (0.000000000420555260 BNB/FLAPJAX)
   ```
   *(Inverse: `2,377,809,918.50 FLAPJAX per 1 BNB`, or `23,778,099.1850 FLAPJAX per 0.01 BNB`)*
4. **Simple Arithmetic Mean of the 9 Buy Prices (`P_avg_simple`):**
   ```text
   P_avg_simple = (1 / 9) * sum(0.01 / F_i)
                = 4.205769068e-10 BNB per FLAPJAX
   ```
5. **First Buy Price (Buy #1 on `2026-10-09`, tx `0xbeefabdc...eb03b4b0`, block `126684151`):**
   ```text
   P_first = 0.01 BNB / 24,052,995.308271524768278040 FLAPJAX
           = 4.157486364e-10 BNB per FLAPJAX (0.000000000415748636 BNB/FLAPJAX)
   ```
6. **Last Buy Price (Buy #9 on `2026-10-18`, tx `0x05a9acf8...6f17ad99`, block `128370339`):**
   ```text
   P_last = 0.01 BNB / 23,506,585.156697207098877431 FLAPJAX
          = 4.254127060e-10 BNB per FLAPJAX (0.000000000425412706 BNB/FLAPJAX)
   ```
   *(Price change from Buy #1 to Buy #9: `+2.3245%` in BNB per FLAPJAX).*

---

## 4. Detailed Statement-by-Statement Verification

1. **Statement 1: *"nine buys of 0.01 BNB each (0.09 BNB in total)"* -> `MATCH`**
   - Confirmed by all 9 transactions in Section 2 (`0xbeefabdc...`, `0x8ce8e857...`, `0x82d01172...`, `0x9afdc96e...`, `0xc94de373...`, `0x172349c6...`, `0xc999000f...`, `0x6ec66780...`, `0x05a9acf8...`). Each carries `value = 0x2386f26fc10000` (`0.01 BNB`), totaling `0.09 BNB`.
2. **Statement 2: *"one a day except 10-15, which was missed and is owed as a make-up buy on 10-23"* -> `MISMATCH`**
   - On-chain block timestamps in UTC show:
     - `2026-10-09`: 1 buy (`0xbeefabdc...` at `18:30:53 UTC`)
     - `2026-10-10`: 1 buy (`0x8ce8e857...` at `18:27:56 UTC`)
     - `2026-10-11`: 1 buy (`0x82d01172...` at `18:30:35 UTC`)
     - `2026-10-12`: 1 buy (`0x9afdc96e...` at `18:46:27 UTC`)
     - `2026-10-13`: 1 buy (`0xc94de373...` at `18:41:15 UTC`)
     - `2026-10-14`: 1 buy (`0x172349c6...` at `18:32:29 UTC`)
     - `2026-10-15`: **0 buys (missed)**
     - `2026-10-16`: **0 buys (also missed in UTC)**
     - `2026-10-17`: **2 buys** (`0xc999000f...` at `02:00:51 UTC`, which was the late make-up for `10-16`, and `0x6ec66780...` at `13:29:27 UTC`)
     - `2026-10-18`: 1 buy (`0x05a9acf8...` at `13:28:39 UTC`)
   - Because `2026-10-16` also had no UTC buy and `2026-10-17` had two UTC buys, the literal statement *"one a day except 10-15"* is a **MISMATCH** against UTC block timestamps.
3. **Statement 3: *"every buy's FLAPJAX went to the treasury"* -> `MATCH`**
   - Verified in all 9 transaction receipts: `topics[2]` of every FLAPJAX `Transfer` log equals `0x0000000000000000000000007c34e9e21ee28a49ff0b84b61774119e6633359f`.
4. **Statement 4: *"and the treasury sent no FLAPJAX to the pool in that window"* -> `MATCH`**
   - Verified across the treasury's outbound transaction history and token transfer index in the `10-09` to `10-18` window: every outbound FLAPJAX transfer from `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f` went to plain EOA bounty/grant recipients; **0 transfers** went to pool `0xcA93EC685b8623688E6B236f9068034B298c2150` (or the BTCB pool `0x740a3b4a91cb8fa47e85d7f9713ca6ec2f8a037e`).

---

## 5. How Any Stranger Can Re-Check Every Figure

Run the following `curl` commands against any public BSC JSON-RPC node (e.g. `https://bsc-dataseed.binance.org/`):

```bash
# Fetch receipt and Transfer log for Buy #1 (10-09, block 126684151):
curl -s -X POST https://bsc-dataseed.binance.org/ \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"eth_getTransactionReceipt","params":["0xbeefabdc89f9148f9daaef6082d34a60dc5b4b612972ca0fd39d6bd4eb03b4b0"]}'
```

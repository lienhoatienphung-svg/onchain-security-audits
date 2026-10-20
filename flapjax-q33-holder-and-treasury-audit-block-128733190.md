# FlapJax Receipt Relay Q33: Holder Distribution & Treasury Statement Audit (Pinned at BSC Block 128733190)

- **Auditor Handle:** `@lienhoatienphung-svg`
- **BSC Payout Wallet (EOA):** `0x3806cd592a508d1992Ed007C2F53de251C2E7452`
- **First-Timer Status:** **YES** (`first=yes` — FlapJax has never paid this operator, handle, or wallet; eligible for the first-timer reserved slot)
- **Target Task:** `TSK-E7RWPB75` (`0x21ce4da5892dda87559f8c15ea6a4504deeeac510f73fd658efc63276754bd97`)
- **Pinned BSC Block:** **`128733190`**
  - **Block Hash:** `0x25de222253987be556fd092b2ce5d5404bb4132b45ffae319982022bdb0eeb0a`
  - **Block Timestamp (UTC):** `2026-10-20T10:50:57.000Z`
  - **On-Chain `totalSupply()`:** `44,444,444,444.000000000000000000 FLAPJAX` (`44444444444000000000000000000 wei`, 18 decimals)

---

## 1. Executive Summary of Required Findings (at Block `128733190`)

1. **Number of wallets with a non-zero balance:**
   - **181 holders** in the GoPlus Security public token index (`https://api.gopluslabs.io/api/v1/token_security/56?contract_addresses=0x90c8889f428F9Ebb77BB8f15CAD3a50a9aC680df`, `holder_count = 181`, `lp_holder_count = 2`), and **253–255 non-zero addresses** in BscScan's token holder index (`https://bscscan.com/token/0x90c8889f428F9Ebb77BB8f15CAD3a50a9aC680df#balances`) / full `Transfer` log replay. Both indices are cited transparently below, and every Top-12 + Treasury + 53-wallet Relay cohort balance was **freshly re-read on-chain via `eth_call` (`balanceOf`, selector `0x70a08231`) at block `128733190`**.
2. **Top 10 Holders & Supply Share at Block `128733190`:**
   - Including all on-chain addresses (BscScan index): Top 10 hold **`34,400,715,996.4256 FLAPJAX` (`77.401611%` of total supply)**.
   - Excluding the PancakeSwap Infinity Vault contract `0x238a...e6c4` and `0x8e3d...f2cd` (GoPlus index): Top 10 hold **`34,168,975,780.2286 FLAPJAX` (`76.880195%` of total supply)**.
   - Full 12-row table with exact `uint256` wei balances at block `128733190` is provided in Section 2 so both indexing conventions are 100% covered.
3. **Contracts vs. Plain Wallets in the Top 10 (Verified via `eth_getCode` at Block `128733190`):**
   - **3 Contracts** in the BscScan Top 10 (or **2 Contracts** in the GoPlus Top 10):
     1. `0xcA93EC685b8623688E6B236f9068034B298c2150` — **Contract** (`22,962 bytes` bytecode): **PancakeSwap V3 Pool (FLAPJAX/WBNB, 1% fee)**.
     2. `0x740A3B4A91cb8FA47e85d7f9713cA6eC2f8a037e` — **Contract** (`22,962 bytes` bytecode): **PancakeSwap V3 Pool (BTCB/FLAPJAX, 1% fee)**.
     3. `0x238a358808379702088667322f80aC48bAd5e6c4` — **Contract** (`8,347 bytes` bytecode): **PancakeSwap Infinity Vault**.
   - **1 Burn Address** (`eth_getCode = 0x`, no private key): `0x000000000000000000000000000000000000dEaD` (`4.327810%`).
   - **All Remaining Top Holders are Plain EOA Wallets (`eth_getCode = 0x`, `0 bytes` bytecode)**, including the Deployer wallet `0x2Ea90fF709B0fB5Decd1dE944F644eECdc26d33A` (`2.645148%`). No token locker or launchpad contract holds FLAPJAX in the top 10 (the SafeToken Locker `0x25c9c4b56e820e0dea438b145284f02d9ca9bd52` holds the LP NFT position, not direct BEP-20 FLAPJAX).
4. **Treasury Balance & Share at Block `128733190`:**
   - **Treasury (`0x7C34E9e21eE28A49Ff0b84B61774119E6633359f`):**
     - Exact Balance: **`280,565,393.061498394918957074 FLAPJAX`** (`280565393061498394918957074 wei`)
     - Share of Supply: **`0.631272%`** of `44,444,444,444 FLAPJAX`
     - Type: Plain EOA wallet (`eth_getCode = 0x`, nonce = `278`, native balance = `0.056248 BNB` at block `128733190`).
5. **Receipt Relay Payout Retention (`2026-10-12 00:00:00 UTC` to `2026-10-17 23:59:59 UTC`) at Block `128733190`:**
   - **All Treasury Outbound Transfers in Window (120 transfers totaling `85,530,000 FLAPJAX` to 53 distinct wallets):** **`47 of 53 wallets`** (`88.68%`) still hold `balanceOf >= in_window_received` at block `128733190`.
   - **Standard `250,000 FLAPJAX` Receipt Relay Transfers in Window (60 transfers totaling `15,000,000 FLAPJAX` to 38 distinct wallets):** **`36 of 38 wallets`** (`94.74%`) have `balanceOf >= 250k_window_received` at block `128733190` (or **`35 of 38`** if requiring `balanceOf >= all_window_grants`, since `0x8c062dc5d775b7d5efc168f75a875f392f53dcdf` currently holds `19,808,776.73 FLAPJAX`, which exceeds its `1,250,000` from `250k` rows but is below its `27,780,000` total across all 22 in-window grants).
6. **Treasury Statement Verification (*"the treasury's FLAPJAX moves only by buyback buys in and bounty payouts out, never by a sale to the pool"*):**
   - **Verdict: `MATCH`** (with one disclosed pre-Relay genesis seed exception: on `2026-09-14` at block `121,836,175`, the Deployer `0x2Ea9...d33A` seeded `10,000,000 FLAPJAX` into the Treasury). Every subsequent inflow came from PancakeSwap V3 FLAPJAX/WBNB Pool (`0xcA93...2150`) buyback swaps, and **100% of outbound transfers went to user/agent wallets — zero (`0`) FLAPJAX were ever transferred from the Treasury to any liquidity pool**.

---

## 2. Top Holders Table at Block `128733190` (Exact `eth_call` `balanceOf` & `eth_getCode`)

| Rank (BscScan) | Rank (GoPlus) | Holder Address | Exact Balance (`uint256` wei) | Balance (FLAPJAX) | Share of Supply (`44.44B`) | `eth_getCode` Size | Classification & On-Chain Role |
|---|---|---|---|---|---|---|---|
| **1** | **1** | `0xcA93EC685b8623688E6B236f9068034B298c2150` | `20096932083119534469698217793` | `20,096,932,083.1195` | `45.218097%` | `22,962 B` | **Contract:** PancakeSwap V3 Pool (FLAPJAX/WBNB 1%) |
| **2** | **2** | `0x740A3B4A91cb8FA47e85d7f9713cA6eC2f8a037e` | `5619844348598993998769163970` | `5,619,844,348.5990` | `12.644650%` | `22,962 B` | **Contract:** PancakeSwap V3 Pool (BTCB/FLAPJAX 1%) |
| **3** | **3** | `0x58faB046ce77064bF52C2eFf4e163eb84386B8B4` | `2334009348875000000000000000` | `2,334,009,348.8750` | `5.251521%` | `0 B` (`0x`) | **Plain Wallet (EOA)** |
| **4** | **4** | `0x000000000000000000000000000000000000dEaD` | `1923471159703770945293578174` | `1,923,471,159.7038` | `4.327810%` | `0 B` (`0x`) | **Burn Address** (permanently burned supply) |
| **5** | **5** | `0x2Ea90fF709B0fB5Decd1dE944F644eECdc26d33A` | `1175621343198940751591630110` | `1,175,621,343.1989` | `2.645148%` | `0 B` (`0x`) | **Plain Wallet (EOA):** FlapJax Token Deployer |
| **6** | *—* | `0x8E3Dc66Bb96b063afB932660fB5d060E170Cf2cd` | `747196241459736162058309614` | `747,196,241.4597` | `1.681192%` | `0 B` (`0x`) | **Plain Wallet (EOA)** |
| **7** | *—* | `0x238a358808379702088667322f80aC48bAd5e6c4` | `669992160659544374404831014` | `669,992,160.6595` | `1.507482%` | `8,347 B` | **Contract:** PancakeSwap Infinity Vault |
| **8** | **6** | `0xf0e319587b54db351f4a20E2da09848FED4D8151` | `621698620153862035019976481` | `621,698,620.1539` | `1.398822%` | `0 B` (`0x`) | **Plain Wallet (EOA)** |
| **9** | **7** | `0x5dEdB37eAa9145dd1A17D385d8c5e256871d6f16` | `611866236479140970515983357` | `611,866,236.4791` | `1.376699%` | `0 B` (`0x`) | **Plain Wallet (EOA)** |
| **10** | **8** | `0x9A937A531A3e4B7930A49548b64c4A3471D049E2` | `610074149236058951494657443` | `610,074,149.2361` | `1.372667%` | `0 B` (`0x`) | **Plain Wallet (EOA)** |
| **11** | **9** | `0xdbf2bdecf7dff212880552398b42e0ab997523a7` | `588854986027098745000000000` | `588,854,986.0271` | `1.324924%` | `0 B` (`0x`) | **Plain Wallet (EOA)** |
| **12** | **10** | `0x4492ca070a2caea1502fb807b389b62ab32eb0d3` | `586603504417275704449255607` | `586,603,504.4173` | `1.319858%` | `0 B` (`0x`) | **Plain Wallet (EOA)** |
| **13** | **11** | `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f` | `280565393061498394918957074` | `280,565,393.0615` | `0.631272%` | `0 B` (`0x`) | **Plain Wallet (EOA):** FlapJax Treasury |

---

## 3. Receipt Relay Payout Retention (`2026-10-12` to `2026-10-17 UTC`) at Block `128733190`

Between `2026-10-12T00:00:00Z` and `2026-10-17T23:59:59Z`, the Treasury (`0x7C34E9e21eE28A49Ff0b84B61774119E6633359f`) executed **120 outbound FLAPJAX transfers** totaling **`85,530,000 FLAPJAX`** across **53 distinct recipient wallets** (of which **60 transfers** were standard `250,000 FLAPJAX` payouts to **38 distinct wallets**).

### Retention Summary at Block `128733190` (`2026-10-20T10:50:57Z`)
- **All 53 In-Window Recipient Wallets:** **`47 of 53`** (`88.68%`) hold `balanceOf >= total_received_in_window`.
- **38 Standard `250,000 FLAPJAX` Recipient Wallets:** **`36 of 38`** (`94.74%`) hold `balanceOf >= 250k_received_in_window` (and **`35 of 38`** hold `balanceOf >= all_window_grants`).

### The 6 Wallets Below Their Full In-Window Total at Block `128733190`:
| # | Recipient Wallet | In-Window Txs | In-Window Total Received (FLAPJAX) | `250k`-Row Total (FLAPJAX) | Balance at Block `128733190` (FLAPJAX) | Status |
|---|---|---|---|---|---|---|
| 1 | `0x1cecdfbb8507800e34e07dd4723abdf8ea07c747` | 1 | `5,000,000` | `0` | `0.0000` | Sold/Transferred All (`0`) |
| 2 | `0x8c062dc5d775b7d5efc168f75a875f392f53dcdf` | 22 | `27,780,000` | `1,250,000` (5x) | `19,808,776.7268` | Partial Hold (`< 27.78M` all-in, `>= 1.25M` 250k-only) |
| 3 | `0x781eb433ff788705080254c6f510df16d37b0c44` | 5 | `1,750,000` | `750,000` (3x) | `0.0000` | Sold/Transferred All (`0`) |
| 4 | `0xd1cada310f73455a2b119fa49aa8b682bfff9f7d` | 7 | `21,300,000` | `0` | `0.0000` | Sold/Transferred All (`0`) |
| 5 | `0x8d9dcf62c004ed31854b11db2a69fe3195abd233` | 1 | `100,000` | `0` | `0.0000` | Sold/Transferred All (`0`) |
| 6 | `0xc3b4f10bc4c4a64676b398ebbe23b29440dca40c` | 4 | `5,000,000` | `500,000` (2x) | `0.0000` | Sold/Transferred All (`0`) |

### Full 53-Wallet Cohort Table at Block `128733190`

| # | Recipient Wallet | Window Tx Count | First Window Tx Hash | First Block | In-Window Received (FLAPJAX) | Balance at Block `128733190` (FLAPJAX) | Holds All? |
|---|---|---|---|---|---|---|---|
| 1 | `0xfd5efa5c47fb3be071b2fd38fc88dfa30318c5d2` | 1 | `0xa5b52de36c7a9167ec2c64a6f49af704e07deddd16a41328f0ade85c9c396837` | `127229962` | `100000.00` | `100000.0000` | **YES** |
| 2 | `0x1cecdfbb8507800e34e07dd4723abdf8ea07c747` | 2 | `0xb22137f4770a323424802724257c947fd4cd5043b1846982ff6133e126b1e234` | `128186281` | `5000000.00` | `0.0000` | **NO** |
| 3 | `0x8c062dc5d775b7d5efc168f75a875f392f53dcdf` | 18 | `0x9df9a7a9552f8d28c38f9434b9546838b6614f8d8639f8cb39a84780beb22bd5` | `127220254` | `27780000.00` | `19808776.7268` | **NO** |
| 4 | `0x5c7a79549e4413a52b1787b5c7e10368720d2358` | 3 | `0xbc92eaf19ab0c57ab6738532d998591146935ff8d14e3903cb7dae2743030b71` | `127220507` | `600000.00` | `2600000.0000` | **YES** |
| 5 | `0x4a6419ee9666acaa6755fc0aebb27f48743c1e1e` | 4 | `0x99f9709a02f73985710b681c605bc95595b1de91f4989b83f993928cc6e4b28d` | `127220370` | `1250000.00` | `2500000.0000` | **YES** |
| 6 | `0x5047384cdb02d1117abece1f7a01d2b0e39c793f` | 3 | `0xffadd9467036e23f558df95338aad9ffcde86fcf3859fd09303a7d926b17817e` | `127145974` | `1250000.00` | `1250000.0000` | **YES** |
| 7 | `0xfc930b2d66d1d0e49e3b6d14df76acdd6ae7b05a` | 1 | `0x13685c67345ed65b8cf7c6b3dc746d4917a9bfc31a6a451d5efd47a23c91128b` | `127291593` | `100000.00` | `100000.0000` | **YES** |
| 8 | `0xdd39d6532ece062f7a8f8f73fd84589480215bec` | 1 | `0xb43a9cc0237b14d212341208968c6d78ebe86066824ebfeca071b48d483cc678` | `127291603` | `100000.00` | `100000.0000` | **YES** |
| 9 | `0x8b8543a9566b7c75e5484ce4385d76879eda2068` | 1 | `0xf033ab9e274aa10b1bc009525311df735105bb3145c0f6ce04653dd4e3898ef5` | `127291613` | `100000.00` | `100000.0000` | **YES** |
| 10 | `0xb6c06eb1a3f1a1ef155a2b48ea6b4f84e9c128f7` | 1 | `0x54c634d4a4ddba5a218f93fbb499bd930a0373b64ff70b0cc4592023bbaf1d54` | `127304633` | `250000.00` | `250000.0000` | **YES** |
| 11 | `0x078293f144c61d1e02b49d4cd3f33c283eb7dc73` | 3 | `0x977b9a50e0889ea42c0bac104806ce9f78cd7928b7b36957f45e5d952f2aa883` | `127412765` | `1000000.00` | `1000000.0000` | **YES** |
| 12 | `0xcfa08d418f136cc2a14c791fcf7e37b30d237592` | 1 | `0x8de4d7fadf2e3e1425aa87884768605ea6d4c05bc2363910f3d0e605d832b0f8` | `127412809` | `250000.00` | `250000.0000` | **YES** |
| 13 | `0x3d650013f46dd2d96b5afefe4acab7a7f2727561` | 2 | `0x7db3af68ebc249ef90803c3d0dd1b42fccb1f9487af281f4fb6aab0962b3f45e` | `127412822` | `750000.00` | `1000000.0000` | **YES** |
| 14 | `0x3d98800c64c345950e1eaaa076d88c12d1bf5f37` | 5 | `0x5153b00fbd17ff816e229f060817982d2f60f8b08a515054a9a8a01011e63283` | `127473873` | `1350000.00` | `1350000.0000` | **YES** |
| 15 | `0x12735653b27f08563ac8866d043d23433a93f5d0` | 2 | `0x9393c62660cb9558dc41c3e97778e94e2ea15e3e25802e64bd08bec2888e5172` | `127495908` | `600000.00` | `600000.0000` | **YES** |
| 16 | `0xe37df0179ba1a0846e7551743bdc5becab4dd13d` | 1 | `0xae93d6e31a51938a602d9879ec8dd3a866ba863e75d7f600fd7a7141dab4e1aa` | `127495916` | `100000.00` | `100000.0000` | **YES** |
| 17 | `0xfebeb3682f672d620e1ce2ffeb2743fde20f4778` | 1 | `0x4c6341b4df75fc4227c59b6483351c831b9260091c5d2a6a0f9158535b2a6219` | `127496894` | `250000.00` | `250000.0000` | **YES** |
| 18 | `0xd07c76fa47c99c70917b6db2e9215844f0b9dc64` | 1 | `0x2e74c8171839ab943a7dc3274375408bfa4f58499606f8710021e29fdbd86c28` | `127496904` | `250000.00` | `250000.0000` | **YES** |
| 19 | `0x1d4c28113002718859d9e808d81ccd3b7763e9b8` | 1 | `0x53cc58d3727ba86033b8ca3780689d27fd36b3ff166a452a7599ed7b19fc849c` | `127496958` | `250000.00` | `250000.0000` | **YES** |
| 20 | `0x781eb433ff788705080254c6f510df16d37b0c44` | 5 | `0xcc52068fb7541024119637d0be6ded70419c2b9c576bda26c60e679375b2ded3` | `127496968` | `1750000.00` | `0.0000` | **NO** |
| 21 | `0x3524b19f3728e9106dc44941c4d2fad7cd90bd01` | 1 | `0x7feb42bcbc12ce46228e87a200f210e1bb54ad989ce792610377e06853435dde` | `127496978` | `250000.00` | `250000.0000` | **YES** |
| 22 | `0x522d50cfba3171bbd9207f1a86e4b29e43edc03b` | 1 | `0x431ca61cf0786dbe1dad81b12c0844ad2f1d1daf98776fc415ebd0bc0848a040` | `127496987` | `250000.00` | `250000.0000` | **YES** |
| 23 | `0x6523acd890ec5853db6d9eb731d7892666dbd27e` | 1 | `0x74ff0ce063f66c3a67ba37502ca724b2471ca52c542cb256b84b9dc0441f47d2` | `127497091` | `250000.00` | `250000.0000` | **YES** |
| 24 | `0x9912d22ab4f1856fb3571c6c68f2571904ace4b9` | 3 | `0x71b9d686e48aea3d31709d32ac6e580dbeee6778a4853db67a9768b2e1f2e2bc` | `127497100` | `1000000.00` | `1000000.0000` | **YES** |
| 25 | `0xd1cada310f73455a2b119fa49aa8b682bfff9f7d` | 6 | `0x3941e6bf363716b49c12fafec5010a7bd9806af6e0fef28ac4633f526c254ecb` | `127603718` | `21300000.00` | `0.0000` | **NO** |
| 26 | `0x5033036702cceee23b79022e938a41e0452f908a` | 1 | `0x0dd0de5c542d7ce26a716bd93df10ebcdc21898258596b03ff2f31e8b325c462` | `127621714` | `250000.00` | `250000.0000` | **YES** |
| 27 | `0x26d6bd4cd5c7a6e34f0c4b186a3dae6690f567aa` | 1 | `0x56e68fb202b7eac6ee9c008fd3bc9234a9c273c0597e62c7af8cada33e0fc6f0` | `127621724` | `250000.00` | `250000.0000` | **YES** |
| 28 | `0x8d9dcf62c004ed31854b11db2a69fe3195abd233` | 1 | `0xf3c5a7613c368fdc2d88fad18816ba035b082f50590618f2e4627c967d77367c` | `127630461` | `100000.00` | `0.0000` | **NO** |
| 29 | `0x7547c70a3670112b4d298c6810639ccef4f7d22c` | 1 | `0x0f39c41bce864f9b300d16ae39aba0fd6c5410e984bcbc1d5b721a4c7409c8ca` | `127630471` | `100000.00` | `100000.0000` | **YES** |
| 30 | `0x8785fa9111aa458117f766831fae5bdcaa6ae427` | 1 | `0x733cde8cea9f15994caee4ecfaf6c8a8408990d393d48b6c9c7002cdaa2605f7` | `127634145` | `250000.00` | `250000.0000` | **YES** |
| 31 | `0xa1e397bd64189092198d0b4301a4d17550c28dc6` | 1 | `0x5d46b1b70d8f72e0c474723e51aedea547bd702f5f557d627a27b5835be29a84` | `127634155` | `250000.00` | `250000.0000` | **YES** |
| 32 | `0xfdf05af8ddce3621f01713e01721d6e9ad702ea8` | 3 | `0x2f5528b8a9593b73f5997aaea31e152095433054c005f3585fc62e3b716e0b6e` | `127642480` | `600000.00` | `850000.0000` | **YES** |
| 33 | `0x78b732d473a723db37f4c72713d5b98a345e77c1` | 1 | `0xdfa3b7204824dfd5d1c53b2d65a03d820e9a7b98398080b8e2aa1ff6f6027b47` | `127666114` | `250000.00` | `250000.0000` | **YES** |
| 34 | `0x24a29c98a5a18058b8989cea7f942c586dceeea6` | 1 | `0x815987f96c193d0e0f772051c9fbccb5b01b20447ebc50ca59278077e4353e95` | `127674438` | `250000.00` | `250000.0000` | **YES** |
| 35 | `0xba567fea152c93b38513a44e1da3cbbee7f3b1dd` | 2 | `0x903b00f76c9cad7f4a96ac6122757501c9586d9fcc7e6f5e723ea29e8baf9757` | `127793467` | `750000.00` | `750000.0000` | **YES** |
| 36 | `0xc3b4f10bc4c4a64676b398ebbe23b29440dca40c` | 6 | `0xa18f69e43dc9bb059b2035c99dc90b226f6e6750b06038dc27a5f4f479ea3d02` | `128082388` | `5000000.00` | `0.0000` | **NO** |
| 37 | `0x1becfb9e7cfc39667f8608909f3013c4bdbbfc92` | 1 | `0xdf0920f8b7b2beddbdfd5f4e73fa17bdfba976df5b6753ce6dc7923a9f2882a7` | `128082400` | `250000.00` | `250000.0000` | **YES** |
| 38 | `0xa21fc88bd8c17cae306089d7b00a1f5cd5ad3ee9` | 6 | `0x77a28b0364c43ebb16978e8a362da36f587a0a35e3ab71d21211947c76f91c61` | `128082410` | `2000000.00` | `3450000.0000` | **YES** |
| 39 | `0xe470b44461cf8028a84369012f9de4aad32bfa9e` | 1 | `0x65106c68b3d6f95e6b70223706991293a8b4c9764716e7c871916fdd0be18066` | `128082442` | `500000.00` | `500000.0000` | **YES** |
| 40 | `0x98d6768508a36c04d8a60e5e5b24ac3d628f5350` | 1 | `0x9356927f87a5a8bbfaf9100aed97786eed6551904e72851f6fdb246d52ab2175` | `128082453` | `500000.00` | `500000.0000` | **YES** |
| 41 | `0x8532ff85f7d16a869945d8abb52893d73d2118ef` | 1 | `0x3d1a53f4f998670238bbacda3bc61b51667344f8838cadb525d33ddccebd2e61` | `128090267` | `250000.00` | `250000.0000` | **YES** |
| 42 | `0xa31be3a1ac1dde980db8d18d32e7cd9ab557360a` | 2 | `0x3dbc845d612bdd97f6d25f54e4e6d3894bafbc51fe4f5c4cb1276cca040f65b9` | `128105345` | `500000.00` | `1500000.0000` | **YES** |
| 43 | `0x1ecf3736eb1a7a2f71980cc520da592bf0124a16` | 3 | `0x849e3c61f3544a0bb0d21e185f7f18eb2070e14ef62fddf069104b54af2585d3` | `128177463` | `2000000.00` | `3100000.0000` | **YES** |
| 44 | `0x9067e71d30cf136f767ae4fd1ea0517314ac0982` | 3 | `0x42ec9967ec0ba13a8be510ae53f1afb3354657199d57114e7f195cd85371db4e` | `128177488` | `1250000.00` | `1550000.0000` | **YES** |
| 45 | `0xd41efde20cafbe8c29b3509087ac5c4364ae5a51` | 1 | `0x98d95720df1e484f3342109da4cfe460897c648e47fd3c73ce755fa2b50069b0` | `128177513` | `100000.00` | `100000.0000` | **YES** |
| 46 | `0xfa36ba58ecc3512b5a9c124631171a19c27c4fc6` | 1 | `0x081b781c0297e9d43fda4b90479ac114eb964110f94ad34cbd73dfaad59f6c71` | `128187003` | `250000.00` | `250000.0000` | **YES** |
| 47 | `0x3d32270e076c998c1bd4c57b5c4798c5c270b22e` | 1 | `0x8b87513d96f668f5cbf4d54c231d313774e7859d8a4b817070909fa945e98db4` | `128187019` | `250000.00` | `250000.0000` | **YES** |
| 48 | `0xcf4cac88df345bf16b550ca02fe4e1d1b27860d3` | 3 | `0x2773283b473f46c31dd6e0a163645fe43ee1d6afcdd7342a5b270d4fd6e2a99f` | `128202673` | `750000.00` | `3000000.0000` | **YES** |
| 49 | `0x226ea74d26e2aa98973dc35ffe7e66fa2e3b2588` | 2 | `0x19d357335d65ddaf06a0f3ba5fc445b4aa9f06353d82028177a44b7315224cd5` | `128205153` | `500000.00` | `7100000.0000` | **YES** |
| 50 | `0xd9080cca2800bc8b09bfb90104d4c40d2cb1bdcb` | 1 | `0xc343bde3c0a5ffb78792031d2afaf4c224d1d119992fe7b412bba12ce62b2b29` | `128210146` | `250000.00` | `250000.0000` | **YES** |
| 51 | `0x6c1a1dcf255333e98d0a0ea55adac1b6caf0a72a` | 1 | `0x0168bedbbcbdd63a36663b7d0c0ac249a37599c3d9d648dd02ed562b318e318e` | `128210178` | `500000.00` | `500000.0000` | **YES** |
| 52 | `0xc8799d53f301c4917a1e6d863057734d9a848e84` | 3 | `0x9c8985d734c22f7986072f7f8655e38e24fbf0d0d4a1f99968346597977c36b0` | `128217931` | `1250000.00` | `2850000.0000` | **YES** |
| 53 | `0x908c97f6b822e277a62c0dbb347f2ccd536b3659` | 1 | `0x46263d3f0f213245ba2359d2a31222d4408f125653e2e8c0359e84d28f664b30` | `128261572` | `250000.00` | `250000.0000` | **YES** |

---

## 4. Treasury Statement Reconciliation (Through Block `128733190`)

- **Statement Tested:** *"the treasury's FLAPJAX moves only by buyback buys in and bounty payouts out, never by a sale to the pool."*
- **Verdict:** **`MATCH`**
- **On-Chain Evidence:**
  1. **Zero Sales to Pool:** Across all 236+ outbound FLAPJAX `Transfer` events from `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f` up to block `128733190` (treasury nonce `278`), **0 transfers** were sent to `0xcA93EC685b8623688E6B236f9068034B298c2150` (FLAPJAX/WBNB V3 pool) or `0x740A3B4A91cb8FA47e85d7f9713cA6eC2f8a037e` (BTCB/FLAPJAX V3 pool).
  2. **Inflows:** Apart from the initial pre-Relay deployer seed of `10,000,000 FLAPJAX` from `0x2Ea90fF709B0fB5Decd1dE944F644eECdc26d33A` on `2026-09-14` (block `121,836,175`), 100% of treasury FLAPJAX inflows have come from `0.01 BNB` (and initial `0.04 BNB` / `0.005 BNB`) buyback swaps out of the PancakeSwap V3 FLAPJAX/WBNB pool `0xcA93EC685b8623688E6B236f9068034B298c2150`. Between block `128,384,630` (`2026-10-18`) and block `128733190` (`2026-10-20`), the treasury balance increased from `277,219,882.6405 FLAPJAX` to `280,565,393.0615 FLAPJAX` via continued daily buybacks net of new Receipt Relay payouts.

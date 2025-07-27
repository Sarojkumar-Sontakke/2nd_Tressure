## 🧠 Scoring Algorithm Explained

This Python script evaluates the financial risk of each wallet by querying their supply and borrow balances from the Compound V2 protocol using GraphQL.

### 🔄 Workflow Breakdown

1. **Input Parsing**
   - Reads wallet IDs from a CSV file (`Wallet_id-Sheet1.csv`).
   - Uses only the first column, ignoring blanks.

2. **API Query via GraphQL**
   - For each wallet, a query is sent to:
     ```
     https://api.thegraph.com/subgraphs/name/graphprotocol/compound-v2
     ```
   - Retrieves data fields:
     - `totalUnderlyingSupplied`
     - `storedBorrowBalance`
   - If no account is found or both values are zero, score defaults to `0`.

3. **Score Calculation**
   - Risk score is based on the ratio of borrowed assets to total activity:
     ```python
     score = int((borrow / (supply + borrow)) × 1000)
     ```
   - Higher scores indicate higher exposure (more borrowing relative to supplied collateral).
   - Capped at a maximum of `1000`.

4. **Output Generation**
   - All wallet scores are saved in `WalletRiskScores.csv` with two columns:
     - `wallet_id`
     - `score`
   - Example:
     ```
     0xabc123..., 540
     0xdef456..., 0
     ```

### 📌 Notes
- Wallet addresses are normalized to lowercase to match schema requirements.
- The scoring method is intentionally simple. Future versions may include features like token diversity, supply volatility, or protocol cross-referencing.

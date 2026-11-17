# Individual Mini Project: Parking Space Reservations
- **Course:** DS 441 A - Blockchain: Theory & Practices
- **Student:** Nguyễn Văn Phi
- **Student ID:** 30211427959

## 1. Prerequisites
- **Ganache** (v2.7.1 or higher) running a local Ethereum blockchain.
- **Remix IDE** (Web browser) for compiling and deploying the Solidity smart contract.
- Modern web browser (Chrome/Edge) with internet access to load `web3.js` via CDN.

## 2. Ganache Network Settings
- **RPC Server:** `http://127.0.0.1:7545`
- **Chain ID / Network ID:** `1337` (or `5777` depending on Ganache workspace settings)
- **Accounts used in demonstration:**
  - `Account 1` (Index 0): Contract Deployer / Facility Manager.
  - `Account 2` (Index 1): Regular User 1 (reserves and releases a parking space).
  - `Account 3` (Index 2): Regular User 2 (tests unauthorized actions).

## 3. Deployment & Run Steps
1. Launch **Ganache** (Quickstart workspace) on port `7545`.
2. Open `contracts/ParkingReservation.sol` in **Remix IDE**.
3. In the **Solidity Compiler** tab, select compiler `0.8.x`, set **EVM Version** to `paris` (to ensure opcode compatibility with local Ganache), and click **Compile**.
4. In the **Deploy & Run Transactions** tab, select **Custom - External Http Provider** (`http://127.0.0.1:7545`), choose `Account 1`, and click **Deploy**.
5. Copy the deployed **Contract Address** and **ABI** into `frontend/index.html`.
6. Open `frontend/index.html` in a web browser (via Live Server or directly) to interact with the DApp.

## 4. Test Results & Explanation
All demonstration screenshots are stored in the `screenshots/` directory:

1. **`01_register_success.jpg` (Valid Registration):**
   - **Action:** `Account 1` (Manager) registers parking space `#101`.
   - **Result:** Transaction succeeds and emits `SpaceRegistered(101)`. Space `#101` appears on the interface with status *"Còn trống"* (Available).
2. **`02_reject_non_manager.jpg` (Unauthorized Registration Rejected):**
   - **Action:** `Account 2` (non-manager) attempts to register parking space `#102`.
   - **Result:** Transaction is reverted by the smart contract's `onlyManager` modifier with the error `"Chi manager moi co quyen thuc hien"`.
3. **`03_reserve_success.jpg` (Valid Reservation):**
   - **Action:** `Account 2` reserves available space `#101`.
   - **Result:** Transaction succeeds and emits `SpaceReserved(101, Account2)`. The UI updates to show space `#101` is reserved by `Account 2`'s address.
4. **`04_reject_duplicate.jpg` (Competing Reservation Rejected):**
   - **Action:** `Account 3` attempts to reserve space `#101` while it is already occupied.
   - **Result:** Transaction is rejected by `require(!spaces[_spaceId].isReserved)` with `"Cho nay da co nguoi dat"`.
5. **`05_reject_unauthorized_release.jpg` (Unauthorized Release Rejected):**
   - **Action:** `Account 3` attempts to release space `#101` reserved by `Account 2`.
   - **Result:** Transaction is rejected by `require(spaces[_spaceId].reservedBy == msg.sender)` with `"Ban khong phai nguoi dat cho nay"`.
6. **`06_release_success.jpg` (Valid Release by Booker):**
   - **Action:** `Account 2` (original reserver) releases space `#101`.
   - **Result:** Transaction succeeds, emits `SpaceReleased(101, Account2)`, and resets space `#101` back to available.

## 5. Citations & References
- **Web3.js (v1.10.0):** Used via jsDelivr CDN for connecting the HTML/JavaScript frontend to the local Ganache RPC node.
- **AI Assistance:** Gemini was used to assist in structuring the HTML/JavaScript Web3 frontend interface and formatting the test documentation.
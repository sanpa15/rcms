

---

### **RCMS to BDS Burial and Deposit Point Moving Process**

#### **1. `bds_temptable.php`**
- **Purpose**: Fetches and processes data from the RCMS server's `daily_trans_bds` table.
- **Process**:
  1. Extracts `pickupdate` and `transid` from the `daily_trans_bds` table on the RCMS server.
  2. Compiles data from the `daily_collection` and `daily_trans` tables based on `pickupdate`.
  3. Joins the data with `shop_details`, `location_master`, and `region_master` tables.
  4. Inserts the processed data into the `daily_trans_bds` table.

---

#### **2. `bds_temptable_static.php`**
- **Purpose**: Handles burial points data from the RCMS server.
- **Process**:
  1. Fetches burial points data from the RCMS server.
  2. Inserts the data into the `daily_trans_burial` table on the RCMS server.

---

#### **3. RCMS Server (Runs every 20 minutes)**

##### **`webservicenew_bds.php`**
- **Purpose**: Updates the `daily_bds_tracker` table with data from `daily_trans_bds`.
- **Process**:
  1. Takes `pickupdate` and `updatetime` from the `daily_trans_bds` table.
  2. Inserts the data into the `daily_bds_tracker` table.

##### **`webservicenew_bds_burial.php`**
- **Purpose**: Displays burial points data.
- **Process**:
  1. Fetches `pickupdate` and `updatetime` from the `daily_trans_bds` table.
  2. Displays burial points data for viewing.

---

#### **4. BDS Server (Runs every 30 minutes)**

##### **`bds_update_data_burial.php`**
- **Purpose**: Fetches and displays data from the RCMS server's `daily_trans_bds` table.
- **Process**:
  1. Extracts `pickupdate`, `count`, and `updatetime` from the `daily_trans_bds` table on the RCMS server.
  2. Displays the data on the BDS server.

##### **`bds_get_data.php`**
- **Purpose**: Transfers data from the RCMS server to the BDS server.
- **Process**:
  1. Uses a cURL call to fetch data from `webservicenew_bds.php` on the RCMS server.
  2. Inserts the fetched data into the `bds_transcount` and `daily_trans` tables on the BDS server.

---

### **Summary of the Flow**
1. Data is extracted and processed from the RCMS server's `daily_trans_bds` table.
2. Burial points data is handled separately and inserted into the `daily_trans_burial` table.
3. The RCMS server updates the `daily_bds_tracker` table every 20 minutes.
4. The BDS server fetches and displays data from the RCMS server every 30 minutes.
5. Data is transferred from the RCMS server to the BDS server using cURL calls and inserted into the respective tables.

---

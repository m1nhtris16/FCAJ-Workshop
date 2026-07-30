---
title: "Setup Amazon DynamoDB Tables"
date: 2026-07-30
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---


In this section, we document how all **Amazon DynamoDB** tables for the CodExecute system were provisioned. Rather than creating each table manually through the Console, all tables were created in a single run using the automated Python setup script [`scripts/create_tables.py`](https://github.com/phuvi301/CodExecute/blob/main/be/scripts/create_tables.py) included in the backend codebase.

The script provisions **8 tables** in total, all with **On-demand (PAY_PER_REQUEST)** billing:

| Table | Partition Key | Sort Key | GSIs |
|---|---|---|---|
| `Users` | `UserID` (S) | — | `Email-index`, `Provider-index` |
| `Submissions` | `SubmissionID` (S) | — | `UserSubmissions-index`, `ProblemSubmissions-index` |
| `Problems` | `ProblemID` (S) | — | `Difficulty-index`, `Category-index` |
| `TestCases` | `ProblemID` (S) | `TestCaseID` (S) | — |
| `Solutions` | `ProblemID` (S) | `SolutionID` (S) | `AuthorSolutions-index` |
| `Notifications` | `UserID` (S) | `CreatedAt` (S) | — |
| `Posts` | `PostID` (S) | — | `Feed-index` |
| `UserFollows` | `FollowerID` (S) | `FollowingID` (S) | `Following-index` |

---

### Step 1: Run the Table Creation Script

Inside the `be/` directory of the CodExecute project, run the setup script with the virtual environment activated:

#### Bash / Linux / macOS:
```bash
cd be

# Activate virtual environment
source venv/bin/activate

# Run the table creation script
python scripts/create_tables.py
```

#### PowerShell (Windows):
```powershell
cd be

# Activate virtual environment
.\venv\Scripts\Activate

# Run the table creation script
python scripts/create_tables.py
```

The script uses **boto3** with credentials from `aws configure` and reads all table names from `app/core/config.py`:

```python
# Khởi tạo DynamoDB Resource (Lấy credentials từ aws configure)
dynamodb = boto3.resource('dynamodb', region_name=settings.AWS_REGION)
```

Each table creation call is idempotent — if a table already exists (`ResourceInUseException`), the script skips it gracefully without failing.

Expected terminal output:

```
--- BẮT ĐẦU TẠO CÁC BẢNG DYNAMODB ---
Đang tạo bảng Users...
Đang tạo bảng Submissions...
✅ Đã tạo bảng Problems thành công.
✅ Đã tạo bảng TestCases thành công.
✅ Đã tạo bảng Solutions thành công.
Đang tạo bảng Notifications...
Đang tạo bảng Posts...
Đang tạo bảng UserFollows...
--- HOÀN TẤT ---
```

---


### Step 2: Verify Tables in DynamoDB Console

After the script completes, open the **Amazon DynamoDB Console** → **Tables** to confirm all tables have been created with status **Active**.

<div align="center">

<img src="/images/5-Workshop/5.5-DynamoDB/tables-list.jpg" alt="All DynamoDB tables in console" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.5.1: All 8 DynamoDB tables listed in the console with Active status</i>
</p>

</div>

Select the `Problems` table and navigate to the **Indexes** tab to verify the GSIs (`Difficulty-index`, `Category-index`) were created correctly.

<div align="center">

<img src="/images/5-Workshop/5.5-DynamoDB/problems-gsi.jpg" alt="Problems table GSI indexes" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.5.2: Problems table GSI indexes confirmed Active in the DynamoDB Console</i>
</p>

</div>

Select the `Submissions` table and verify its GSIs (`UserSubmissions-index`, `ProblemSubmissions-index`).

<div align="center">

<img src="/images/5-Workshop/5.5-DynamoDB/submissions-gsi.jpg" alt="Submissions table GSI indexes" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.5.3: Submissions table GSI indexes confirmed Active in the DynamoDB Console</i>
</p>

</div>

---

### Step 3: Seed Sample Problems into the `Problems` Table

With the tables active, the `Problems` table needs to be populated with problem data before the frontend can display anything. In the **DynamoDB Console**, select the `Problems` table → **Explore table items** → **Create item** → switch to **JSON view** and paste a problem entry:

```json
{
  "ProblemID": {"S": "two-sum"},
  "Title": {"S": "Two Sum"},
  "Difficulty": {"S": "Easy"},
  "Category": {"S": "Array"},
  "Description": {"S": "Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target."},
  "Constraints": {"S": "2 <= nums.length <= 10^4\n-10^9 <= nums[i] <= 10^9\nExactly one valid answer exists."},
  "SampleInput": {"S": "nums = [2,7,11,15], target = 9"},
  "SampleOutput": {"S": "[0,1]"},
  "Explanation": {"S": "Because nums[0] + nums[1] == 9, we return [0, 1]."},
  "Tags": {"L": [{"S": "Array"}, {"S": "Hash Table"}]},
  "AcceptanceRate": {"N": "49.1"},
  "TotalSubmissions": {"N": "0"},
  "AcceptedSubmissions": {"N": "0"}
}
```

<div align="center">

<img src="/images/5-Workshop/5.5-DynamoDB/seed-problem.jpg" alt="Seeding problem item into DynamoDB" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.5.4: Creating a sample problem item in the Problems table via DynamoDB Console JSON editor</i>
</p>

</div>

Click **Create item**. The problem will now be served by the `codeexecute-api` Lambda and rendered in the CodExecute frontend problem list.

<div align="center">

<img src="/images/5-Workshop/5.5-DynamoDB/problem-seeded.jpg" alt="Problem item visible in DynamoDB table" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.5.5: Sample problem item successfully stored and visible in the Problems table</i>
</p>

</div>

---

### Verification

All 8 DynamoDB tables are now active and the `Problems` table contains at least one problem item. The two Lambda functions interact with these tables as follows:

- **`codeexecute-api`**: Reads `Problems` to serve the problem list and detail pages; reads/writes `Submissions` to handle submission status polling.
- **`codeexecute-worker`**: Writes grading results back to `Submissions` after sandbox code execution completes.

All access is performed via **boto3** using the **IAM execution role** attached to each Lambda — no credentials are hardcoded in the application.

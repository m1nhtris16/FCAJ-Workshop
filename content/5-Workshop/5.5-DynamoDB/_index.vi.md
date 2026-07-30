---
title: "Thiết lập Amazon DynamoDB"
date: 2026-07-30
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---


Trong phần này, chúng ta ghi lại quá trình khởi tạo toàn bộ bảng **Amazon DynamoDB** cho hệ thống CodExecute. Thay vì tạo từng bảng thủ công trên Console, toàn bộ bảng được tạo trong một lần chạy duy nhất bằng script Python tự động [`scripts/create_tables.py`](https://github.com/phuvi301/CodExecute/blob/main/be/scripts/create_tables.py) có sẵn trong mã nguồn backend.

Script khởi tạo **8 bảng** với chế độ **On-demand (PAY_PER_REQUEST)**:

| Bảng | Partition Key | Sort Key | GSI |
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

### Bước 1: Chạy Script Tạo Bảng

Từ thư mục `be/` của dự án CodExecute, kích hoạt môi trường ảo và chạy script:

#### Bash / Linux / macOS:
```bash
cd be

# Kích hoạt môi trường ảo
source venv/bin/activate

# Chạy script tạo bảng
python scripts/create_tables.py
```

#### PowerShell (Windows):
```powershell
cd be

# Kích hoạt môi trường ảo
.\venv\Scripts\Activate

# Chạy script tạo bảng
python scripts/create_tables.py
```

Script sử dụng **boto3** với credentials từ `aws configure` và đọc tên bảng từ `app/core/config.py`:

```python
# Khởi tạo DynamoDB Resource (Lấy credentials từ aws configure)
dynamodb = boto3.resource('dynamodb', region_name=settings.AWS_REGION)
```

Mỗi lệnh tạo bảng hoạt động theo kiểu idempotent — nếu bảng đã tồn tại (`ResourceInUseException`), script bỏ qua và tiếp tục mà không báo lỗi.

Kết quả đầu ra mong đợi:

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


### Bước 2: Kiểm Tra Bảng Trên DynamoDB Console

Sau khi script hoàn tất, mở **Amazon DynamoDB Console** → **Tables** để xác nhận tất cả bảng đã được tạo với trạng thái **Active**.

<div align="center">

<img src="/images/5-Workshop/5.5-DynamoDB/tables-list.jpg" alt="Danh sách các bảng DynamoDB trên Console" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.5.1: Toàn bộ 8 bảng DynamoDB hiển thị trong Console với trạng thái Active</i>
</p>

</div>

Chọn bảng `Problems` → tab **Indexes** để xác nhận các GSI (`Difficulty-index`, `Category-index`) đã được tạo đúng.

<div align="center">

<img src="/images/5-Workshop/5.5-DynamoDB/problems-gsi.jpg" alt="GSI của bảng Problems" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.5.2: Xác nhận GSI của bảng Problems đang Active trên DynamoDB Console</i>
</p>

</div>

Chọn bảng `Submissions` và kiểm tra các GSI (`UserSubmissions-index`, `ProblemSubmissions-index`).

<div align="center">

<img src="/images/5-Workshop/5.5-DynamoDB/submissions-gsi.jpg" alt="GSI của bảng Submissions" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.5.3: Xác nhận GSI của bảng Submissions đang Active trên DynamoDB Console</i>
</p>

</div>

---

### Bước 3: Thêm Đề Bài Mẫu Vào Bảng `Problems`

Sau khi các bảng đã Active, bảng `Problems` cần được điền dữ liệu đề bài trước khi frontend có thể hiển thị danh sách bài. Trên **DynamoDB Console**, chọn bảng `Problems` → **Explore table items** → **Create item** → chuyển sang chế độ **JSON view** và dán nội dung đề bài:

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

<img src="/images/5-Workshop/5.5-DynamoDB/seed-problem.jpg" alt="Thêm đề bài mẫu vào DynamoDB" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.5.4: Tạo item đề bài mẫu trong bảng Problems qua JSON editor của DynamoDB Console</i>
</p>

</div>

Nhấn **Create item**. Đề bài sẽ được Lambda `codeexecute-api` phục vụ và hiển thị ngay trong danh sách bài của frontend CodExecute.

<div align="center">

<img src="/images/5-Workshop/5.5-DynamoDB/problem-seeded.jpg" alt="Đề bài mẫu đã được lưu vào DynamoDB" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.5.5: Item đề bài mẫu đã được lưu thành công và hiển thị trong bảng Problems</i>
</p>

</div>

---

### Kết Quả

Toàn bộ 8 bảng DynamoDB đã Active và bảng `Problems` đã có ít nhất một đề bài. Hai Lambda function tương tác với các bảng như sau:

- **`codeexecute-api`**: Đọc `Problems` để phục vụ danh sách và trang chi tiết đề bài; đọc/ghi `Submissions` để xử lý polling trạng thái chấm bài.
- **`codeexecute-worker`**: Ghi kết quả chấm bài trở lại `Submissions` sau khi sandbox thực thi code hoàn tất.

Toàn bộ truy cập thực hiện qua **boto3** sử dụng **IAM execution role** gắn với từng Lambda — không có credentials nào được hardcode trong mã nguồn.

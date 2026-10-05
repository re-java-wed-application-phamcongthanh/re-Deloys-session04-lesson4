# Báo Cáo Bài 4: Quản lý tệp tin bỏ qua (.gitignore) và Sửa lịch sử (Amend)

## 1. Mục tiêu
- Cấu hình tệp tin ẩn .gitignore để tự động bỏ qua các tệp tin nhạy cảm hoặc không cần thiết.
- Sử dụng lệnh git rm --cached để gỡ bỏ tệp tin đã lỡ commit ra khỏi cache theo dõi của Git một cách an toàn mà không làm mất tệp tin vật lý trên đĩa cứng.
- Chỉnh sửa thông điệp và nội dung của commit gần nhất thông qua tùy chọn git commit --amend.

---

## 2. Bối cảnh & Quy trình xử lý

### Bối cảnh
Học viên lỡ tay git add . và commit nhầm tệp tin chứa thông tin bảo mật credentials.txt lên Git history. 

### Quy trình khắc phục chuẩn:

#### Bước 1: Giả lập sự cố commit nhầm tệp tin nhạy cảm
Tạo các tệp tin pp.js và credentials.txt, sau đó commit:
`ash
git add .
git commit -m "Commit nham file credentials.txt"
`

#### Bước 2: Gỡ bỏ tệp tin khỏi cache theo dõi của Git (nhưng giữ lại tệp vật lý)
Sử dụng cờ --cached của lệnh git rm:
`ash
git rm --cached credentials.txt
`
*Giải thích:* Lệnh này đưa credentials.txt ra khỏi vùng Staging Area và ngưng theo dõi phiên bản, nhưng **tệp vật lý credentials.txt vẫn nằm nguyên trên ổ cứng local**.

#### Bước 3: Tạo tệp .gitignore để chặn theo dõi trong tương lai
Tạo tệp .gitignore và thêm tên tệp nhạy cảm vào:
`	ext
credentials.txt
*.log
node_modules/
`
Thêm tệp .gitignore vào Staging Area:
`ash
git add .gitignore
`

#### Bước 4: Chỉnh sửa commit gần nhất bằng tùy chọn --amend
Gộp việc gỡ bỏ credentials.txt và thêm .gitignore vào chính commit vừa tạo trước đó, đồng thời viết lại thông điệp commit cho sạch sẽ:
`ash
git commit --amend -m "Initial commit: Add app.js and .gitignore (removed sensitive credentials)"
`

---

## 3. Kết quả Kiểm tra (Verification)

### Kiểm tra trạng thái tệp tin (git status):
`ash
git status
`
**Output:**
`	ext
On branch main
nothing to commit, working tree clean
`
*(Tệp credentials.txt hoàn toàn bị .gitignore bỏ qua, không còn xuất hiện trong danh sách Untracked hay Staged).*

### Kiểm tra sự tồn tại của tệp vật lý:
`powershell
Test-Path credentials.txt
# Output: True (Tệp tin vật lý vẫn tồn tại cục bộ)
`

### Kiểm tra lịch sử commit gần nhất (git log -n 1):
`ash
git log -n 1
`
**Output:**
`	ext
commit 2cd844f0b12a95c8e31a29f8c61e0e84bc912345
Author: phamcongthanhvn2k6 <phamcongt56@gmail.com>
Date:   Mon Oct 5 14:02:43 2026 +0700

    Initial commit: Add app.js and .gitignore (removed sensitive credentials)
`

---

## 4. Kết luận
- Đã gỡ bỏ thành công tệp thông tin nhạy cảm credentials.txt khỏi sự quản lý của Git bằng git rm --cached.
- Tệp vật lý credentials.txt vẫn được bảo toàn nguyên vẹn tại thư mục cục bộ.
- Tệp .gitignore được cấu hình chuẩn xác để tự động ngăn chặn việc lỡ commit các tệp nhạy cảm trong tương lai.
- Kỹ thuật git commit --amend giúp lịch sử Git sạch đẹp và bảo mật.

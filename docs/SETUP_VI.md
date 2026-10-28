# Hướng dẫn tạo repo GitHub ban đầu (Windows)

Phiên bản này tạo **repo khung, chưa có notebook/code full**. Ba thành viên sẽ thêm ba file `.ipynb` theo phân công trong report bằng các commit riêng.

## 1. Giải nén và kiểm tra bộ khung

1. Tải ZIP starter repo và giải nén vào thư mục dễ tìm, ví dụ `Documents\e-shop-clickstream-forecasting-starter`.
2. Ở thư mục gốc phải thấy `README.md`, `requirements.txt`, `.gitignore`, `CONTRIBUTING.md`, `data`, `docs` và `notebooks`.
3. **Không chép notebook full vào thư mục `notebooks/` ở giai đoạn này.** Thư mục chỉ có `README.md` mô tả kế hoạch chia notebook.

## 2. Tạo repo trống trên GitHub

1. Đăng nhập GitHub → dấu `+` → **New repository**.
2. Đặt tên, ví dụ `e-shop-clickstream-forecasting`.
3. Chọn Public hoặc Private theo yêu cầu của nhóm/giảng viên.
4. Vì bộ khung trên máy đã có `README.md`, hãy tạo repo **trống**: không yêu cầu GitHub tự thêm README, `.gitignore` hoặc License.
5. Bấm **Create repository** và sao chép URL repo.

## 3. Push commit khởi tạo (chỉ tài liệu, cấu hình, report và dữ liệu)

Mở PowerShell ở thư mục gốc repo bằng cách mở thư mục trong File Explorer, bấm thanh địa chỉ, gõ `powershell` và Enter.

Chạy:

```powershell
git init -b main
git add .
git status
git commit -m "Initialize repository structure and project documentation"
git remote add origin https://github.com/<GITHUB_USERNAME>/<REPOSITORY_NAME>.git
git push -u origin main
```

Thay hai phần trong dấu `< >` bằng username và tên repo thật, rồi xóa dấu `< >`. Trước `git commit`, xem `git status` để chắc chắn không có notebook đầy đủ nào nằm trong danh sách staged. Gói starter này không chứa notebook full.

Nếu Git báo chưa có danh tính commit, cấu hình tên/email của chính bạn:

```powershell
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

Sử dụng email bạn muốn gắn với commit; không dùng nguyên các giá trị ví dụ.

## 4. Mời hai thành viên còn lại

Trong repo GitHub, vào **Settings → Collaborators** (hoặc mục quản lý quyền truy cập tương đương), nhập username của từng thành viên và gửi lời mời. Mỗi người chấp nhận lời mời bằng tài khoản của chính mình.

## 5. Để từng thành viên tự upload và lưu lịch sử đóng góp

Mỗi thành viên nên làm việc trên máy của mình bằng tài khoản GitHub riêng, tạo branch riêng, commit file notebook được giao, push branch rồi tạo Pull Request để nhóm review. Cách này ghi lại commit author và lịch sử PR rõ ràng hơn việc một người upload thay tất cả.

Tên file dự kiến:

- Ho Huyen Trang: `notebooks/01_data_quality_and_price2.ipynb`
- Ta Van Sang: `notebooks/02_product_day_panel_eda_features.ipynb`
- Bui Quang Minh: `notebooks/03_modeling_tuning_evaluation.ipynb`

Ví dụ cho thành viên phụ trách notebook thứ nhất:

```powershell
git clone https://github.com/<GITHUB_USERNAME>/<REPOSITORY_NAME>.git
cd <REPOSITORY_NAME>
git checkout -b notebook/01-data-quality-price2
# Tạo/lưu notebook tại notebooks/01_data_quality_and_price2.ipynb
git status
git add notebooks/01_data_quality_and_price2.ipynb
git commit -m "Add data quality and price2 analysis notebook"
git push -u origin notebook/01-data-quality-price2
```

Sau đó mở Pull Request trên GitHub để review và merge vào `main`. Hai thành viên còn lại làm tương tự, đổi tên branch, đường dẫn notebook và commit message cho đúng phần được giao.

## 6. Lưu ý khi tách một notebook thành ba file

Jupyter Notebook không dùng chung biến giữa các file. Vì vậy, trước khi tách, nhóm cần thống nhất dữ liệu trung gian nào được tạo ở notebook trước và được đọc ở notebook sau. Đặt file sinh ra trong `data/processed/`; thư mục này đã được `.gitignore` loại trừ. Ghi rõ tên file, cột và thứ tự chạy trong `notebooks/README.md`. Không để notebook 3 phụ thuộc vào biến chỉ tồn tại trong kernel notebook 1 hoặc 2.

Ba file dự kiến tuân theo phân công ở mục **Team Contributions** của report: Parts 1–3; Parts 4–5; và Parts 6–9. Hãy thống nhất tên/phiên bản notebook cuối cùng với report trước khi nộp.

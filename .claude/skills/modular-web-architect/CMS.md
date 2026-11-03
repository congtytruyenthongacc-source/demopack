# CMS tiếng Việt

Nhánh của [`SKILL.md`](SKILL.md) khi website cần trang quản trị nội dung. Mọi nhãn,
nút, thông báo lỗi và hướng dẫn trong trang này viết bằng tiếng Việt.

## Trình soạn thảo

Đủ dùng cho người viết bài không biết code:

- Tiêu đề các cấp, đoạn văn, in đậm, in nghiêng, danh sách.
- Chèn ảnh — kéo thả được, có xem trước ngay trong trình soạn thảo.
- Nhúng video theo link YouTube hoặc Vimeo.
- Bảng thông số: thêm, xóa hàng và cột ngay trên giao diện.

## Trường dữ liệu

| Trường | Ràng buộc |
|---|---|
| Tiêu đề | Bắt buộc |
| Đường dẫn (slug) | Tự sinh từ tiêu đề, bỏ dấu tiếng Việt, cho sửa tay; trùng slug thì chặn lưu và nói rõ bài nào đang giữ |
| Chuyên mục | Chọn một, bắt buộc |
| Thẻ phân loại | Chọn nhiều, không bắt buộc |
| Ảnh đại diện | Có khung xem trước đúng tỷ lệ sẽ hiển thị thật trên trang |

Slug sinh từ tiêu đề tiếng Việt phải bỏ dấu đúng: "Bao bì giấy Kraft" → `bao-bi-giay-kraft`.
Viết test cho riêng hàm này — dấu tiếng Việt là chỗ sai nhiều nhất.

## Khung SEO on-page

Ba ô nhập, mỗi ô có bộ đếm ký tự chạy trực tiếp:

| Ô | Ngưỡng | Khi vượt |
|---|---|---|
| Meta Title | 60 ký tự | Bộ đếm đổi màu, nói rõ đang vượt bao nhiêu ký tự |
| Meta Description | 160 ký tự | Bộ đếm đổi màu, nói rõ đang vượt bao nhiêu ký tự |
| Từ khóa chính | — | — |

Kèm thanh chấm điểm SEO cập nhật theo từng lần gõ, cộng điểm theo các mục kiểm tra
tường minh — người viết phải biết mất điểm ở đâu để sửa:

- Từ khóa chính có trong tiêu đề.
- Từ khóa chính có trong Meta Description.
- Từ khóa chính có trong đoạn đầu bài.
- Meta Title và Meta Description nằm trong ngưỡng.
- Ảnh đại diện đã có, và có thuộc tính `alt`.

## Trạng thái bài viết

`Bản nháp` → `Chờ duyệt` → `Đã lên lịch` → `Đã xuất bản`

Bài ở trạng thái nào thì trang công khai cũng chỉ hiển thị `Đã xuất bản`. Ba trạng thái
còn lại chỉ người đăng nhập trang quản trị thấy.

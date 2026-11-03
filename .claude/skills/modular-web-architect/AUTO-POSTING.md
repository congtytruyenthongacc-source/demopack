# Cổng đăng bài tự động

Nhánh của [`SKILL.md`](SKILL.md) khi website cần nhận bài từ bên ngoài — kịch bản n8n,
Make, hoặc script Python tự viết.

## Endpoint

Một Cloudflare Worker nhận `POST` kèm JSON:

```
POST /api/posts
Content-Type: application/json
X-Webhook-Secret: <chuỗi bí mật>
```

Secret lưu trong Worker Secrets (`wrangler secret put WEBHOOK_SECRET`), **không** đặt
trong `wrangler.toml` vì file đó nằm trong git.

Worker so sánh header với secret trước khi đọc body. Sai hoặc thiếu thì trả `401` và
dừng — endpoint này có quyền xuất bản lên site thật, nên để mở là để người lạ đăng bài
hộ. So sánh bằng hàm so sánh thời gian cố định, không dùng `===` trên chuỗi.

## Payload

```json
{
  "title": "Tiêu đề bài viết",
  "slug": "duong-dan-bai-viet",
  "content_html": "<p>Nội dung chi tiết...</p>",
  "category": "chuyen-muc",
  "featured_image": "https://...",
  "meta_title": "Tiêu đề SEO",
  "meta_description": "Mô tả SEO",
  "status": "draft" | "publish",
  "schedule_time": "2026-11-03T18:00:00Z"
}
```

Ánh xạ sang trạng thái trong [`CMS.md`](CMS.md):

| `status` | `schedule_time` | Trạng thái lưu |
|---|---|---|
| `draft` | bỏ qua | `Bản nháp` |
| `publish` | không có, hoặc đã qua | `Đã xuất bản` |
| `publish` | ở tương lai | `Đã lên lịch` |

Bài không qua được bộ kiểm duyệt dưới đây luôn vào `Chờ duyệt`, kể cả khi
`status` là `publish`.

`content_html` là HTML từ nguồn ngoài, nên phải lọc trước khi lưu: giữ thẻ định dạng và
thẻ ảnh, bỏ `<script>`, `<iframe>` không thuộc danh sách nhúng cho phép, và mọi thuộc
tính `on*`.

## Bộ kiểm duyệt

Chạy trước khi đưa vào hàng đợi:

| Mục kiểm | Ngưỡng |
|---|---|
| Độ dài nội dung | > 800 từ, đếm trên text đã bóc thẻ HTML |
| `meta_title` | Có, ≤ 60 ký tự |
| `meta_description` | Có, ≤ 160 ký tự |
| `featured_image` | Có, URL tải về được |
| `slug` | Có, chưa bị bài khác chiếm |

Trả `200` kèm danh sách mục không đạt, và lưu bài vào `Chờ duyệt` thay vì chặn hẳn —
bài đã viết rồi, để người duyệt sửa tay nhanh hơn là bắt kịch bản gửi lại.

## Bộ lập lịch

Cron trigger của Worker, chạy mỗi 5 phút: tìm bài `Đã lên lịch` có `schedule_time` đã
qua, chuyển sang `Đã xuất bản`.

Cron chạy theo UTC. `schedule_time` người dùng nhập theo giờ Việt Nam thì phải quy đổi
khi lưu, không phải khi đọc — viết test cho mốc giao ngày, vì UTC+7 làm 18:00 ngày 3
thành 11:00 UTC cùng ngày nhưng 23:00 ngày 3 thành 16:00 UTC ngày 3 và 01:00 ngày 4
thành 18:00 UTC ngày 3.

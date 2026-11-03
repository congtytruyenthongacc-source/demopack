# Đóng gói lên Cloudflare

Nhánh của [`SKILL.md`](SKILL.md) khi đưa site lên thật.

## Cấu trúc thư mục

Phần tĩnh ra `/dist`. Phần Worker để riêng, không lẫn vào output build:

```
/dist              ← Cloudflare Pages trỏ vào đây
/src               ← nguồn, nếu dùng Astro / Next / Vite
/workers           ← Worker nhận webhook và cron
  wrangler.toml
```

`/dist` là output build nên vào `.gitignore` — trừ khi dự án viết HTML tay, lúc đó
`/dist` chính là nguồn và phải commit.

## Hai cách deploy

**Kéo thả** — nhanh, dùng để xem thử. Vào Cloudflare Dashboard → Workers & Pages →
Create → Pages → Upload assets, kéo cả thư mục `/dist` vào. Không có lịch sử phiên bản
theo commit, mỗi lần sửa phải kéo lại tay.

**Nối GitHub** — dùng cho site chạy thật. Dashboard → Pages → Connect to Git, chọn repo
và nhánh, khai lệnh build và thư mục output (`/dist`). Mỗi push vào nhánh đó Cloudflare
tự build lại; push vào nhánh khác thì được một bản xem thử riêng theo URL riêng.

Site chạy thật thì chọn cách nối GitHub. Kéo thả làm mất liên hệ giữa cái đang chạy và
commit nào sinh ra nó, nên khi có sự cố không biết phải quay về đâu.

## Tên miền và SSL

Pages → project → Custom domains → Set up a domain.

Tên miền đã nằm trong Cloudflare thì bản ghi DNS được tạo tự động. Chưa thì Cloudflare
báo cặp nameserver phải đổi ở chỗ đăng ký tên miền — đổi nameserver lan ra toàn cầu mất
vài giờ tới một ngày.

SSL cấp tự động sau khi DNS trỏ đúng, thường trong vài phút. Trong lúc chờ, trang báo
lỗi chứng chỉ — đó là trạng thái bình thường, không phải cấu hình sai.

## Biến môi trường

Pages → Settings → Environment variables cho biến build. Secret của Worker đặt bằng
`wrangler secret put`, không đặt trong `wrangler.toml` (xem [`AUTO-POSTING.md`](AUTO-POSTING.md)).

Khai riêng cho hai môi trường Production và Preview. Để trống Preview thì bản xem thử
sẽ chạy với biến của Production và ghi thẳng vào dữ liệu thật.

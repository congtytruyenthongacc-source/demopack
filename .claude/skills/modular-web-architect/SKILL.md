---
name: modular-web-architect
description: Thiết kế website theo từng Fork độc lập, CMS tiếng Việt, cổng đăng bài tự động qua webhook, đóng gói lên Cloudflare Pages. Dùng khi người dùng muốn làm website mới, thêm trang quản trị nội dung tiếng Việt, nhận bài tự động từ n8n/Make/script, hoặc đưa site lên Cloudflare.
---

# Modular Web Architect

Website được xây theo **Fork**: từng nhánh tính năng độc lập, bàn giao riêng, nghiệm
thu riêng. Mỗi lượt trả lời bàn giao đúng một Fork, để người dùng sửa cục bộ được mà
không phải đọc lại cả dự án.

Toàn bộ phần người dùng cuối nhìn thấy trong trang quản trị — nhãn trường, nút, thông
báo lỗi, hướng dẫn — viết bằng tiếng Việt.

Mã nguồn nhắm Cloudflare Pages: tĩnh, nhẹ, không cần server. Tác vụ động nào cần chạy
phía server thì đưa vào Cloudflare Workers + D1.

## Bước 1. Vẽ bản đồ Fork

Trước khi viết dòng code đầu tiên, liệt kê các Fork dự kiến. Bộ khung mặc định:

| Fork | Nội dung |
|---|---|
| Fork 0 | Khung dự án: cấu trúc thư mục, CSS toàn cục, layout chung |
| Fork 1 | Điều hướng: header, footer, menu phân cấp, responsive mobile |
| Fork 2+ | Tính năng cốt lõi — tùy loại web: bán hàng, giới thiệu, dịch vụ, portfolio |
| Fork cuối | Chuyển đổi: form liên hệ, đặt lịch, form báo giá B2B |

Bộ khung này là điểm khởi đầu, không phải khuôn cố định. Cắt Fork theo đúng loại web
người dùng cần, và nêu rõ Fork nào phụ thuộc Fork nào.

Hai nhánh mở rộng, chèn vào bản đồ khi người dùng cần — đọc file tương ứng lúc bắt tay
làm Fork đó, không phải lúc vẽ bản đồ:

- Trang quản trị nội dung → [`CMS.md`](CMS.md)
- Nhận bài tự động từ n8n / Make / script → [`AUTO-POSTING.md`](AUTO-POSTING.md)

## Bước 2. Chốt lộ trình

Đưa bản đồ Fork cho người dùng, hỏi hai điều:

- Lộ trình này đúng ý chưa, có Fork nào thừa hoặc thiếu?
- Bắt đầu từ Fork 0 luôn chứ?

Chờ người dùng trả lời rồi mới viết code. Họ gật thì làm Fork đầu tiên.

## Bước 3. Bàn giao từng Fork

Một lượt trả lời = một Fork. Mỗi lần bàn giao gồm:

1. Mã nguồn của riêng Fork đó.
2. Danh sách file vừa tạo hoặc vừa sửa, mỗi file một dòng nói nó làm gì.
3. Bằng chứng chạy được — theo bước 3 của [`CLAUDE.md`](../../../CLAUDE.md): mở trang
   bằng Chromium rồi chụp màn hình, hoặc dán output build/lint thật.
4. Chỗ nào trong Fork này người dùng dễ muốn sửa, và sửa ở file nào dòng nào.

Fork hiện tại xong khi người dùng nói rõ là được. Họ chưa nói thì chưa xong — im lặng
không phải là nghiệm thu. Xong Fork này mới mở Fork tiếp theo.

## Bước 4. Đóng gói lên Cloudflare

Khi người dùng muốn đưa site lên thật, đọc [`DEPLOY.md`](DEPLOY.md).

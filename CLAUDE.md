# CLAUDE.md

## Định nghĩa "xong"

Viết xong code mới là hết bước 1. Một mẩu việc chỉ xong khi đã đi hết cả bốn bước
dưới đây, theo thứ tự. Bước 4 là bước cuối cùng của mọi lượt trả lời báo cáo kết quả.

### 1. Làm

Bám đúng scope được giao.

### 2. Review

Chạy `/code-review` trên diff. Sửa hết finding trước khi báo cáo. Còn finding treo
thì việc chưa xong — mô tả finding trong báo cáo không thay thế được việc sửa nó.

### 3. Kiểm lại bằng chứng

Chạy thật thứ vừa làm: test, lint, build, hoặc mở app lên xem. Dán output thật vào
báo cáo. Câu "đã kiểm tra và hoạt động tốt" mà không kèm output là chưa kiểm.

Chỗ nào không chạy được — thiếu quyền, thiếu credential, môi trường không hỗ trợ —
nói thẳng là chưa kiểm được và vì sao.

### 4. Grill ngược lại người dùng

Kết mỗi báo cáo bằng 2–5 câu hỏi. Mỗi câu phải bám vào một thứ cụ thể vừa xảy ra
trong lượt làm việc này:

- Chỗ tôi tự quyết thay vì hỏi — nêu ra để người dùng xác nhận hoặc bác.
- Chỗ tôi đoán ý — nêu giả định đã dùng.
- Chỗ scope bị mở rộng hoặc bị thu hẹp so với yêu cầu ban đầu.
- Rủi ro tôi thấy nhưng chưa xử vì nằm ngoài phạm vi được giao.

Mỗi câu hỏi kèm sẵn phương án tôi khuyên chọn, để người dùng trả lời được bằng một
chữ thay vì phải tự nghĩ lại từ đầu.

Câu hỏi bám vào việc vừa làm, chứ không phải câu xã giao chung chung như "bạn còn
cần gì nữa không" hay "có muốn tôi làm tiếp không".

## Bộ skill trong repo

`.claude/skills/` chứa 24 skill từ [mattpocock/skills](https://github.com/mattpocock/skills).
Các skill hay dùng nhất với quy trình trên:

| Skill | Dùng khi |
|---|---|
| `/grill-me` | Muốn tôi tra hỏi ngược lại về một kế hoạch trước khi bắt tay làm |
| `/tdd` | Xây tính năng hoặc sửa bug theo lối test-first |
| `/diagnosing-bugs` | Có thứ đang hỏng, chạy sai, hoặc chậm |
| `/to-spec` → `/to-tickets` → `/implement` | Việc lớn, cần chẻ nhỏ trước khi làm |
| `/handoff` | Hội thoại dài, cần bàn giao sang session mới |

`/setup-matt-pocock-skills` cần chạy một lần cho repo này. Nó hỏi issue tracker
dùng gì, label triage ra sao, lưu docs ở đâu, rồi ghi kết quả vào `docs/agents/`.
`/to-tickets` và `/triage` đọc từ đó — chưa chạy setup thì chúng dừng lại và bảo
người dùng chạy nó trước.

---
name: prompt-log
description: Append the completed Codex prompt session or available conversation transcript to prompt_log.md. Use when the user asks to log prompts, save the chat, record the full conversation after a prompt finishes, preserve AI interaction evidence, or maintain a prompt log for assignment submission.
---

# Prompt Log Skill

## 1. Mục đích

Ghi lại toàn bộ đoạn chat hoặc phần hội thoại khả dụng vào `prompt_log.md` sau khi một prompt/task đã hoàn thành. Skill này dùng để lưu evidence về cách đã dùng AI trong bài tập.

## 2. Nguyên tắc bắt buộc

1. Ghi log sau khi đã hoàn thành việc chính của prompt.
2. Không thay thế nội dung cũ trong `prompt_log.md`; luôn append thêm entry mới.
3. Nếu không truy cập được toàn bộ lịch sử hội thoại, ghi rõ phạm vi log là `conversation context khả dụng`.
4. Không bịa lại nội dung chat không có trong context.
5. Giữ nguyên nội dung quan trọng của user prompt, assistant action, file đã sửa, command đã chạy, kết quả kiểm tra và blocker nếu có.
6. Không ghi bí mật như token, password, key hoặc credential nếu xuất hiện; thay bằng `[REDACTED]`.

## 3. File output

Ghi tại root project:

`prompt_log.md`

Nếu file chưa tồn tại, tạo file mới.

## 4. Format entry

Mỗi lần ghi log, thêm một entry:

```md
## Prompt Log Entry - <YYYY-MM-DD HH:mm:ss TZ>

### User Prompt
<Nội dung prompt của người dùng trong lượt này>

### Conversation Context
<Tóm tắt hoặc transcript phần hội thoại khả dụng liên quan đến prompt này>

### Actions Taken
- <File đã tạo/cập nhật>
- <Command hoặc validation quan trọng đã chạy>

### Result
<Kết quả cuối cùng, file liên quan, kiểm tra đã pass/fail/block>

### Notes
<Giới hạn, giả định, hoặc phần không log được nếu có>
```

## 5. Khi người dùng yêu cầu "toàn bộ đoạn chat"

1. Ghi transcript đầy đủ nhất có trong context hiện tại.
2. Phân vai rõ `User`, `Assistant`, `Tool`, hoặc `System note` nếu cần.
3. Nếu context đã bị compact hoặc thiếu đoạn cũ, ghi chú: `Một phần hội thoại trước đó không còn trong context, chỉ log phần khả dụng`.
4. Không tự tạo lại nguyên văn những đoạn không còn nhìn thấy.

## 6. Khi dùng cùng skill khác

Nếu prompt chính dùng `$qa-workflow`, `$domain-testing`, `$boundary-value-analysis`, `$test-case-review`, `$test-execution`, hoặc `$bug-report`, hoàn thành skill chính trước. Sau đó mới dùng `$prompt-log` để append entry vào `prompt_log.md`.

# Test Run: FR-26 — Quản lý hồ sơ cá nhân trên Mobile

## Thông tin chung

| Field | Value |
|---|---|
| **Requirement** | FR-26: Quản lý hồ sơ cá nhân trên Mobile |
| **Ngày thực thi** | 28/06/2026 |
| **Môi trường** | App: Mobile (Expo) · OS: Windows 11 / Android · Backend: `http://localhost:3000` |
| **Build / Commit** | `85af3ba` (branch `main`) |
| **Tester** | Kiểm thử thủ công (manual) |

> Cách kiểm thử: Chạy ứng dụng Mobile bằng Expo, đăng nhập tài khoản User `test@eshop.com` / `Test1234!`, mở màn hình **Hồ sơ của bạn**. Với mỗi kịch bản số điện thoại, nhập giá trị vào ô "Số điện thoại" rồi bấm **"Cập nhật"** và quan sát thông báo (Alert "Thành công" hoặc "Lỗi") cùng dữ liệu hồ sơ sau khi lưu. Ô Email hiển thị read-only ("Email (Không đổi)"); màn hình Hồ sơ không có ô nhập `role`. Với kịch bản liên quan tới quyền `role` (DT-008) và việc xác minh ràng buộc ở **phía server**, tester gửi yêu cầu cập nhật trực tiếp tới API qua công cụ kiểm tra mạng/Console để kiểm tra hệ thống có chặn hay không. Dữ liệu hồ sơ được đưa về trạng thái ban đầu giữa các lượt bằng cách chạy lại seed `node database.js`. Tài khoản: User `test@eshop.com` / `Test1234!`.

---

## Kết quả thực thi

### Domain Testing

| Test Case ID | Mô tả | Tester | Result | Related Bug | Note |
|---|---|---|---|---|---|
| TC-FR26-DT-001 | Cập nhật hồ sơ với SĐT hợp lệ 10 số `0912345678` | Manual | **Failed** | BUG-FR26-001 | Số hợp lệ (bắt đầu `0`, 10 số) nhưng app báo lỗi "Số điện thoại không hợp lệ. Vui lòng nhập đúng 9-10 chữ số." và KHÔNG cập nhật. Kỳ vọng: cập nhật thành công. |
| TC-FR26-DT-002 | Cập nhật hồ sơ với SĐT hợp lệ 11 số `09123456789` | Manual | **Failed** | BUG-FR26-001 | Số hợp lệ (bắt đầu `0`, 11 số) nhưng app vẫn báo lỗi và không cập nhật. Kỳ vọng: thành công. |
| TC-FR26-DT-003 | Từ chối SĐT không bắt đầu bằng `0` (`1912345678`) | Manual | **Failed** | BUG-FR26-001 | Số không hợp lệ (không bắt đầu `0`) lại được app chấp nhận và lưu thành công. Kỳ vọng: bị từ chối với lỗi SĐT. |
| TC-FR26-DT-004 | Từ chối SĐT ngắn hơn 10 số (`091234567`) | Manual | **Passed** | | App báo lỗi SĐT không hợp lệ, không cập nhật (kết quả đúng kỳ vọng). |
| TC-FR26-DT-005 | Từ chối SĐT dài hơn 11 số (`091234567890`) | Manual | **Passed** | | App báo lỗi SĐT không hợp lệ, không cập nhật. |
| TC-FR26-DT-006 | Từ chối SĐT chứa ký tự không phải số (`09123abc78`) | Manual | **Passed** | | App báo lỗi SĐT không hợp lệ, không cập nhật. |
| TC-FR26-DT-007 | Email không đổi được qua giao diện mobile | Manual | **Passed** | | Ô Email hiển thị read-only; thao tác cập nhật chỉ gửi Họ Tên/SĐT/Địa chỉ → Email không thay đổi sau khi lưu. |
| TC-FR26-DT-008 | User không tự đổi được `role` | Manual | **Passed** | | Màn hình Hồ sơ không có ô `role`, thao tác cập nhật từ app không gửi `role` → quyền không đổi qua giao diện. |
| TC-FR26-DT-009 | Chưa đăng nhập không cập nhật được hồ sơ | Manual | **Passed** | | Khi chưa đăng nhập, màn hình Hồ sơ hiển thị "Vui lòng đăng nhập" và không cho cập nhật; backend cũng yêu cầu token. |

### Boundary Value Analysis (BVA)

| Test Case ID | Mô tả | Tester | Result | Related Bug | Note |
|---|---|---|---|---|---|
| TC-FR26-BVA-001 | OFF⁻ độ dài: 9 số `091234567` → từ chối | Manual | **Passed** | | App báo lỗi SĐT, không cập nhật (đúng kỳ vọng từ chối). |
| TC-FR26-BVA-002 | ON độ dài tối thiểu: 10 số `0912345678` → chấp nhận | Manual | **Failed** | BUG-FR26-001 | Tại biên hợp lệ 10 số (bắt đầu `0`) app vẫn báo lỗi và không cập nhật. Kỳ vọng: thành công. |
| TC-FR26-BVA-003 | ON độ dài tối đa: 11 số `09123456789` → chấp nhận | Manual | **Failed** | BUG-FR26-001 | Tại biên hợp lệ 11 số (bắt đầu `0`) app vẫn báo lỗi và không cập nhật. Kỳ vọng: thành công. |
| TC-FR26-BVA-004 | OFF⁺ độ dài: 12 số `091234567890` → từ chối | Manual | **Passed** | | App báo lỗi SĐT, không cập nhật. |

---

## Tổng kết

| Trạng thái | Số lượng |
|---|---:|
| Passed | 8 |
| Failed | 5 |
| Blocked | 0 |
| Not Run | 0 |
| **Tổng** | **13** |

> **Ghi chú:** Khi Result = **Failed** hoặc **Blocked** → phải có **Related Bug** (link đến GitHub Issue) hoặc lý do rõ ràng trong cột **Note**.

### Bug phát hiện

| Bug ID | Tóm tắt | Severity / Priority | Found by |
|---|---|---|---|
| BUG-FR26-001 | Validate số điện thoại trên mobile sai ngược: từ chối số hợp lệ (bắt đầu `0`, 10–11 số), chấp nhận số không hợp lệ | High / P1 | TC-FR26-DT-001, TC-FR26-DT-002, TC-FR26-DT-003, TC-FR26-BVA-002, TC-FR26-BVA-003 |

### Ghi chú quan sát

- Quy tắc số điện thoại của app mobile bị đảo ngược so với FR-26: số điện thoại Việt Nam hợp lệ (luôn bắt đầu bằng `0`) đều bị từ chối → người dùng **không thể cập nhật hồ sơ** với số điện thoại đúng. Các test case "từ chối" (DT-004/005/006, BVA-001/004) tình cờ vẫn cho kết quả "bị từ chối" như kỳ vọng, nhưng là do quy tắc sai chứ không phải do kiểm tra độ dài đúng. Thông báo lỗi cũng sai nội dung ("đúng 9-10 chữ số").
- Email được hiển thị read-only và endpoint cập nhật hồ sơ không thay đổi Email → ràng buộc Email read-only được đảm bảo.
- Màn hình Hồ sơ trên mobile không cung cấp ô `role` và thao tác cập nhật từ app không gửi `role` → qua giao diện mobile, người dùng không tự đổi được quyền (đúng kỳ vọng FR-26).

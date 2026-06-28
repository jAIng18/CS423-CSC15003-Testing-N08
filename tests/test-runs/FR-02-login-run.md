# Test Run: FR-02 — Đăng nhập & Khóa tài khoản

## Thông tin chung

| Field | Value |
|---|---|
| **Requirement** | FR-02: Đăng nhập & Khóa tài khoản |
| **Ngày thực thi** | 28/06/2026 |
| **Môi trường** | Browser: Chrome · OS: Windows 11 · Frontend: `http://localhost:5173` (trang Đăng nhập) · Backend: `http://localhost:3000` |
| **Build / Commit** | `969e156` (branch `main`) |
| **Tester** | Kiểm thử thủ công (manual) |

> Cách kiểm thử: thao tác trực tiếp trên form Đăng nhập của trình duyệt. Quan sát thông báo trên giao diện, việc chuyển trang sau khi đăng nhập, và kiểm tra token trong DevTools → Application → Local Storage. Với các kịch bản khóa tài khoản, tài khoản `test@eshop.com` được đưa về trạng thái ban đầu giữa các lượt bằng cách chạy lại seed dữ liệu `node database.js` (theo `setup_guide.md`). Các mốc thời gian 29s/30s/31s được canh bằng đồng hồ.

---

## Kết quả thực thi

### Domain Testing

| Test Case ID | Mô tả | Tester | Result | Related Bug | Note |
|---|---|---|---|---|---|
| TC-FR02-DT-001 | Đăng nhập thành công với tài khoản hợp lệ | Manual | **Passed** | | Đăng nhập được chấp nhận, chuyển về trang chủ, token lưu trong Local Storage. (Quan sát thêm ở tab Network: response chứa cả `password` → BUG-FR02-004) |
| TC-FR02-DT-002 | Từ chối email sai định dạng HTML5 | Manual | **Failed** | BUG-FR02-003 | Nhập `abc`, form KHÔNG cảnh báo định dạng và vẫn submit được (ô nhập là `type="text"`); kết quả hiện thông báo lỗi đăng nhập chung. Kỳ vọng form chặn theo HTML5 không đạt |
| TC-FR02-DT-003 | Từ chối email đúng format nhưng không tồn tại | Manual | **Passed** | | Hiện thông báo chung "Đăng nhập thất bại. Vui lòng kiểm tra lại.", không chuyển trang, không lưu token; không lộ nguyên nhân |
| TC-FR02-DT-004 | Đăng nhập sai dưới ngưỡng khóa không khóa tài khoản | Manual | **Failed** | BUG-FR02-001 | Sau 2 lần nhập sai, đăng nhập bằng mật khẩu đúng vẫn bị từ chối (tài khoản đã bị khóa). Kỳ vọng: chưa khóa trước lần sai thứ 3 |
| TC-FR02-DT-005 | Khóa tài khoản sau 3 lần đăng nhập sai liên tiếp | Manual | **Passed** | BUG-FR02-001 | Sau chuỗi nhập sai, tài khoản bị khóa, không lưu token. Quan sát phụ: tài khoản đã bị khóa ngay từ lần sai thứ 2 (xem BUG-FR02-001) |
| TC-FR02-DT-006 | Từ chối mật khẩu đúng khi tài khoản đang bị khóa | Manual | **Passed** | | Đang khóa, nhập mật khẩu đúng → vẫn bị từ chối, không lưu token. Quan sát phụ: giao diện chỉ hiện thông báo lỗi chung, không báo tài khoản đã bị khóa (xem BUG-FR02-005) |
| TC-FR02-DT-007 | Đăng nhập lại thành công sau khi hết 30 giây tạm khóa | Manual | **Failed** | BUG-FR02-002 | Sau hơn 30 giây (đo tới 31s) vẫn bị khóa; thực tế phải chờ ~180 giây mới đăng nhập lại được |
| TC-FR02-DT-008 | Từ chối đăng nhập khi email để trống | Manual | **Passed** | | Ô Email có `required` → trình duyệt chặn submit ("Please fill out this field"); không đăng nhập, không lưu token |
| TC-FR02-DT-009 | Từ chối đăng nhập khi mật khẩu để trống | Manual | **Passed** | | Ô Mật khẩu có `required` → trình duyệt chặn submit; không đăng nhập, không lưu token. Quan sát phụ: ô mật khẩu là `type="text"` nên mật khẩu hiển thị rõ (xem BUG-FR02-006) |

### Boundary Value Analysis (BVA)

| Test Case ID | Mô tả | Tester | Result | Related Bug | Note |
|---|---|---|---|---|---|
| TC-FR02-BVA-001 | OFF⁻: 2 lần sai liên tiếp → chưa khóa | Manual | **Failed** | BUG-FR02-001 | Sau 2 lần sai, đăng nhập bằng mật khẩu đúng bị từ chối (đã khóa). Kỳ vọng: chưa khóa ở mức 2 |
| TC-FR02-BVA-002 | ON: lần sai thứ 3 → kích hoạt khóa | Manual | **Passed** | BUG-FR02-001 | Tài khoản bị khóa, mật khẩu đúng bị từ chối (khóa kích hoạt sớm hơn ngưỡng — xem BUG-FR02-001) |
| TC-FR02-BVA-003 | OFF⁺: 4 lần sai → vẫn bị từ chối | Manual | **Passed** | | Mọi lượt đều bị từ chối, tài khoản ở trạng thái khóa, không lưu token |
| TC-FR02-BVA-004 | OFF⁻ thời gian: 29s → vẫn bị khóa | Manual | **Passed** | | Tại 29s vẫn bị từ chối, không lưu token (đúng kỳ vọng còn trong thời gian khóa) |
| TC-FR02-BVA-005 | ON thời gian: 30s → cho đăng nhập lại | Manual | **Failed** | BUG-FR02-002 | Tại 30s vẫn bị khóa. Kỳ vọng được đăng nhập lại; thực tế thời gian khóa kéo dài ~180 giây |
| TC-FR02-BVA-006 | OFF⁺ thời gian: 31s → cho đăng nhập lại | Manual | **Failed** | BUG-FR02-002 | Tại 31s vẫn bị khóa. Kỳ vọng được đăng nhập lại; thực tế thời gian khóa kéo dài ~180 giây |

---

## Tổng kết

| Trạng thái | Số lượng |
|---|---:|
| Passed | 9 |
| Failed | 6 |
| Blocked | 0 |
| Not Run | 0 |
| **Tổng** | **15** |

> **Ghi chú:** Khi Result = **Failed** hoặc **Blocked** → phải có **Related Bug** (link đến GitHub Issue) hoặc lý do rõ ràng trong cột **Note**.

### Bug phát hiện

| Bug ID | Tóm tắt | Severity / Priority | Found by |
|---|---|---|---|
| BUG-FR02-001 | Tài khoản bị khóa sau 2 lần sai thay vì 3 | High / P1 | TC-FR02-DT-004, TC-FR02-BVA-001 |
| BUG-FR02-002 | Thời gian tạm khóa ~180 giây thay vì 30 giây | Medium / P2 | TC-FR02-DT-007, TC-FR02-BVA-005, TC-FR02-BVA-006 |
| BUG-FR02-003 | Trường Email dùng `type="text"`, không validate HTML5 email format | Medium / P2 | TC-FR02-DT-002 |
| BUG-FR02-004 | Response đăng nhập để lộ mật khẩu (`password`) | Critical / P1 | TC-FR02-DT-001 (quan sát khi chạy) |
| BUG-FR02-005 | Tài khoản bị khóa nhưng giao diện chỉ báo lỗi chung | Medium / P2 | TC-FR02-DT-006 (quan sát khi chạy) |
| BUG-FR02-006 | Ô Mật khẩu dùng `type="text"`, mật khẩu hiển thị plaintext | Medium / P2 | TC-FR02-DT-009 (quan sát khi chạy) |

### Ghi chú quan sát

- Tất cả trường hợp đăng nhập thất bại (sai mật khẩu, email không tồn tại, hoặc đang bị khóa) đều chỉ hiển thị **cùng một thông báo chung** "Đăng nhập thất bại. Vui lòng kiểm tra lại." trên giao diện — không lộ nguyên nhân cụ thể.
- Thời gian khóa được đo bằng cách thử lại liên tục bằng mật khẩu đúng: tài khoản chỉ đăng nhập lại được sau **~180 giây** kể từ lúc bị khóa.

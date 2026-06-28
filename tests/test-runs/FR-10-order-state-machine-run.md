# Test Run: FR-10 — Trạng thái Đơn hàng (Order State Machine)

## Thông tin chung

| Field | Value |
|---|---|
| **Requirement** | FR-10: Trạng thái Đơn hàng (Order State Machine) |
| **Ngày thực thi** | 28/06/2026 |
| **Môi trường** | Browser: Chrome · OS: Windows 11 · Web (User): `http://localhost:5173` · Admin: `http://localhost:5174` · Backend: `http://localhost:3000` |
| **Build / Commit** | `85af3ba` (branch `main`) |
| **Tester** | Kiểm thử thủ công (manual) |

> Cách kiểm thử: Đơn hàng được tạo bằng cách đăng nhập tài khoản User (`test@eshop.com`) trên web, thêm sản phẩm vào giỏ và Checkout (đơn mới ở trạng thái `pending`). Các thao tác của Admin (xác nhận, giao hàng, hoàn tất, hủy) thực hiện trên trang Admin (`http://localhost:5174`) → tab Đơn hàng. Thao tác hủy của User thực hiện trên web → Hồ sơ → Lịch sử đơn hàng → nút "Hủy đơn". Tài khoản: Admin `admin@eshop.com` / `Admin123!`, User `test@eshop.com` / `Test1234!`. Trạng thái đơn được chuẩn bị cho từng kịch bản bằng cách chạy lại seed dữ liệu `node database.js` rồi đưa đơn về trạng thái cần thiết. Với các chuyển đổi **không hợp lệ** mà giao diện Admin không cung cấp nút tương ứng (bước nhảy, bước lùi, từ trạng thái kết thúc, trạng thái ngoài domain), tester gửi yêu cầu đổi trạng thái trực tiếp qua DevTools → Console/Network để xác minh ràng buộc có được thực thi ở **phía server** hay chỉ ẩn nút trên giao diện. Trạng thái đơn sau mỗi thao tác được kiểm tra lại trên giao diện và qua DevTools → Network.

---

## Kết quả thực thi

### Domain Testing

| Test Case ID | Mô tả | Tester | Result | Related Bug | Note |
|---|---|---|---|---|---|
| TC-FR10-DT-001 | Admin xác nhận đơn `pending` → `confirmed` | Manual | **Passed** | | Trên trang Admin bấm "Xác nhận", đơn chuyển `confirmed`, hiển thị phản hồi thành công. |
| TC-FR10-DT-002 | Admin giao hàng `confirmed` → `shipping` | Manual | **Passed** | | Bấm "Giao hàng", đơn chuyển `shipping`. |
| TC-FR10-DT-003 | Admin hoàn tất `shipping` → `delivered` | Manual | **Passed** | | Bấm "Hoàn tất", đơn chuyển `delivered`. |
| TC-FR10-DT-004 | User hủy đơn `pending` → `canceled` | Manual | **Passed** | | Web → Hồ sơ → "Hủy đơn", đơn chuyển `canceled`. |
| TC-FR10-DT-005 | Admin hủy đơn `confirmed` → `canceled` | Manual | **Passed** | | Trang Admin bấm "Hủy", đơn chuyển `canceled`. |
| TC-FR10-DT-006 | Bước nhảy `pending` → `shipping` (Admin) | Manual | **Passed** | | Trang Admin chỉ hiện nút "Xác nhận"/"Hủy" khi đơn `pending`, không có nút giao hàng. Gửi yêu cầu trực tiếp qua DevTools → server trả lỗi "Invalid state transition from pending to shipping", đơn giữ `pending`. |
| TC-FR10-DT-007 | Bước lùi `confirmed` → `pending` (Admin) | Manual | **Passed** | | Không có thao tác lùi trạng thái trên giao diện; server từ chối ("Invalid state transition from confirmed to pending"), đơn giữ `confirmed`. |
| TC-FR10-DT-008 | Từ final `delivered` → `shipping` | Manual | **Passed** | | Đơn `delivered` không còn nút thao tác; server từ chối, đơn giữ `delivered`. |
| TC-FR10-DT-009 | Từ final `canceled` → `pending` | Manual | **Passed** | | Đơn `canceled` không còn nút thao tác; server từ chối ("Invalid state transition from canceled to pending"), đơn giữ `canceled`. Quan sát phụ: khi thử đích `delivered`, server lại chấp nhận `canceled → delivered` (xem BUG-FR10-003). |
| TC-FR10-DT-010 | User hủy đơn `shipping` | Manual | **Failed** | BUG-FR10-001 | Web vẫn hiện nút "Hủy đơn" khi đơn đang `shipping`; bấm vào hiện "Hủy đơn thành công" và đơn chuyển `canceled`. Kỳ vọng: bị từ chối, giữ `shipping`. |
| TC-FR10-DT-011 | User thực hiện thao tác xác nhận (việc của Admin) | Manual | **Failed** | BUG-FR10-002 | Trang Admin chặn User đăng nhập (kiểm tra role ở giao diện), nhưng endpoint đổi trạng thái phía server không kiểm tra quyền. Gửi yêu cầu xác nhận bằng phiên đăng nhập của User → đơn vẫn chuyển `confirmed`. Kỳ vọng: bị từ chối, giữ `pending`. |
| TC-FR10-DT-012 | Admin → trạng thái đích ngoài domain (`returned`) | Manual | **Passed** | | Giao diện chỉ cho chọn các trạng thái hợp lệ; gửi `returned` qua DevTools → server từ chối, đơn giữ `pending`. |
| TC-FR10-DT-013 | Trạng thái hiện tại ngoài domain (`returned`) → `confirmed` | Manual | **Passed** | | Dựng đơn ở trạng thái `returned` qua dữ liệu; server từ chối ("Invalid state transition from returned to confirmed"), đơn giữ `returned`. |
| TC-FR10-DT-014 | Guest (chưa đăng nhập) hủy đơn `pending` | Manual | **Passed** | | Đăng xuất rồi thực hiện thao tác hủy → server trả 401 Unauthorized, đơn giữ `pending`. |

---

## Tổng kết

| Trạng thái | Số lượng |
|---|---:|
| Passed | 12 |
| Failed | 2 |
| Blocked | 0 |
| Not Run | 0 |
| **Tổng** | **14** |

> **Ghi chú:** Khi Result = **Failed** hoặc **Blocked** → phải có **Related Bug** (link đến GitHub Issue) hoặc lý do rõ ràng trong cột **Note**.

### Bug phát hiện

| Bug ID | Tóm tắt | Severity / Priority | Found by |
|---|---|---|---|
| BUG-FR10-001 | User tự hủy được đơn hàng đang giao (`shipping`) | High / P1 | TC-FR10-DT-010 |
| BUG-FR10-002 | Endpoint đổi trạng thái đơn không kiểm tra quyền Admin/sở hữu | Critical / P1 | TC-FR10-DT-011 |
| BUG-FR10-003 | Cho phép chuyển từ trạng thái kết thúc `canceled` sang `delivered` | High / P2 | TC-FR10-DT-009 (quan sát khi chạy) |

### Ghi chú quan sát

- Trang Admin chỉ hiển thị nút cho các bước chuyển **hợp lệ** theo state machine; các bước không hợp lệ (nhảy/lùi/từ trạng thái kết thúc/ngoài domain) không có nút trên giao diện. Phần lớn ràng buộc cũng được thực thi đúng ở **phía server** (trả "Invalid state transition"), **trừ** trường hợp `canceled → delivered` vẫn được chấp nhận (BUG-FR10-003).
- Endpoint đổi trạng thái đơn (`PUT /api/admin/orders/:id/status`) chỉ yêu cầu đăng nhập, **không** kiểm tra vai trò Admin lẫn quyền sở hữu đơn → bất kỳ người dùng đã đăng nhập nào cũng có thể đổi trạng thái đơn của bất kỳ ai (BUG-FR10-002).
- Trang web hiển thị nút "Hủy đơn" cho mọi đơn chưa `delivered`/`canceled`, bao gồm cả đơn `shipping`, và backend cho phép hủy → User hủy được đơn đang giao (BUG-FR10-001).

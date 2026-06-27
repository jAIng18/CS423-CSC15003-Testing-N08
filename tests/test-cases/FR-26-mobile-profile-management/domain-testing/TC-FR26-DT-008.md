# TC-FR26-DT-008: Không cho phép người dùng tự thay đổi thuộc tính role trong hồ sơ mobile

## Requirement ID
FR-26

## Module / Test type / Technique
Mobile Profile Management / Functional / Domain Testing

## Domain Analysis

### Input Variables & Domain

| Variable | Type | Domain / Constraints |
|---|---|---|
| Trạng thái đăng nhập | System state | Người dùng phải đã đăng nhập để cập nhật hồ sơ |
| Họ Tên | Text | Giá trị danh nghĩa hợp lệ: `Nguyen Van Mobile` |
| Số điện thoại | Text | Giá trị danh nghĩa hợp lệ: `0912345678` |
| Địa chỉ giao hàng mặc định | Text | Giá trị danh nghĩa hợp lệ: `123 Le Loi, Q1, TP.HCM` |
| Thuộc tính `role` | System attribute | Người dùng không thể tự thay đổi thuộc tính `role` |

### Domain Matrix

| TC | Trạng thái / Quyền | Họ Tên | Số điện thoại | Email / role | Expected |
|---|---|---|---|---|---|
| COND-FR26-DT-008 | EC-AUTH-V01, EC-OWNER-V01 | EC-NAME-V01 | EC-PHONE-V01 | EC-ROLE-I01: cố đổi `role` thành `admin` | Giao diện mobile không cho tự thay đổi `role`; sau khi lưu, quyền của user không thay đổi |

## Preconditions
- Ứng dụng Mobile đang hoạt động.
- Người dùng thường `test@eshop.com` đã đăng nhập.
- Người dùng đang ở màn hình Quản lý hồ sơ cá nhân của chính mình.

## Test data

| Field | Value |
|---|---|
| role cố gắng thay đổi | `admin` |
| Họ Tên | `Nguyen Van Mobile` |
| Số điện thoại | `0912345678` |
| Địa chỉ giao hàng mặc định | `123 Le Loi, Q1, TP.HCM` |

## Test steps
1. Mở màn hình Quản lý hồ sơ cá nhân trên Mobile.
2. Quan sát toàn bộ các trường và control có thể chỉnh sửa trên màn hình.
3. Tìm field hoặc control cho phép thay đổi `role`.
4. Nếu không có field `role`, lưu hồ sơ với các giá trị hợp lệ danh nghĩa.
5. Sau khi lưu, kiểm tra người dùng vẫn không có quyền admin trong giao diện mobile.

## Expected result
Giao diện hồ sơ mobile không hiển thị field hoặc control cho phép người dùng tự đổi `role`. Sau khi lưu hồ sơ, người dùng `test@eshop.com` vẫn là người dùng thường và không có quyền admin.

## Status / Related bugs
Not Run / None

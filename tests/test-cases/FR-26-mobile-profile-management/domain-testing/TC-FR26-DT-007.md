# TC-FR26-DT-007: Không cho phép thay đổi Email qua giao diện hồ sơ mobile

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
| Email | UI field | Phải hiển thị read-only, không được thay đổi qua giao diện mobile |

### Domain Matrix

| TC | Trạng thái / Quyền | Họ Tên | Số điện thoại | Email / role | Expected |
|---|---|---|---|---|---|
| COND-FR26-DT-007 | EC-AUTH-V01, EC-OWNER-V01 | EC-NAME-V01 | EC-PHONE-V01 | EC-EMAIL-I01: cố đổi thành `other@eshop.com` | Email không chỉnh sửa được qua giao diện mobile; lưu hồ sơ không làm thay đổi Email |

## Preconditions
- Ứng dụng Mobile đang hoạt động.
- Người dùng `test@eshop.com` đã đăng nhập.
- Người dùng đang ở màn hình Quản lý hồ sơ cá nhân của chính mình.

## Test data

| Field | Value |
|---|---|
| Email hiện tại | `test@eshop.com` |
| Email cố gắng thay đổi | `other@eshop.com` |
| Họ Tên | `Nguyen Van Mobile` |
| Số điện thoại | `0912345678` |
| Địa chỉ giao hàng mặc định | `123 Le Loi, Q1, TP.HCM` |

## Test steps
1. Mở màn hình Quản lý hồ sơ cá nhân trên Mobile.
2. Quan sát trường Email.
3. Thử đặt con trỏ vào trường Email và thay đổi giá trị thành `other@eshop.com`.
4. Nếu giao diện vẫn cho lưu các trường khác, nhập Họ Tên, Số điện thoại và Địa chỉ giao hàng mặc định bằng giá trị hợp lệ rồi lưu.
5. Quan sát Email sau thao tác.

## Expected result
Email được hiển thị dưới dạng read-only hoặc không có cơ chế chỉnh sửa. Người dùng không thể đổi Email thành `other@eshop.com`; sau thao tác, Email vẫn là `test@eshop.com`.

## Status / Related bugs
Not Run / None

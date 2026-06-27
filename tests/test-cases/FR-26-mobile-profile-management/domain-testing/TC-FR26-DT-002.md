# TC-FR26-DT-002: Cập nhật hồ sơ mobile thành công với số điện thoại 11 chữ số

## Requirement ID
FR-26

## Module / Test type / Technique
Mobile Profile Management / Functional / Domain Testing

## Domain Analysis

### Input Variables & Domain

| Variable | Type | Domain / Constraints |
|---|---|---|
| Trạng thái đăng nhập | System state | Người dùng phải đã đăng nhập để cập nhật hồ sơ |
| Chủ sở hữu hồ sơ | System state | Chỉ cập nhật hồ sơ của chính người dùng đang đăng nhập |
| Họ Tên | Text | Có thể cập nhật; format/độ dài chưa được đặc tả |
| Số điện thoại | Text | Hợp lệ khi bắt đầu bằng `0`, từ 10-11 chữ số |
| Địa chỉ giao hàng mặc định | Text | Có thể cập nhật; format/độ dài chưa được đặc tả |
| Email | UI field | Hiển thị read-only, không được phép thay đổi qua giao diện mobile |

### Domain Matrix

| TC | Trạng thái / Quyền | Họ Tên | Số điện thoại | Email / role | Expected |
|---|---|---|---|---|---|
| COND-FR26-DT-002 | EC-AUTH-V01, EC-OWNER-V01 | EC-NAME-V01: `Nguyen Van Mobile` | EC-PHONE-V02: `09123456789` | EC-EMAIL-V01, EC-ROLE-V01 | Cập nhật được chấp nhận với số điện thoại 11 chữ số |

## Preconditions
- Ứng dụng Mobile đang hoạt động.
- Người dùng `test@eshop.com` đã đăng nhập.
- Người dùng đang ở màn hình Quản lý hồ sơ cá nhân của chính mình.

## Test data

| Field | Value |
|---|---|
| Họ Tên | `Nguyen Van Mobile` |
| Số điện thoại | `09123456789` |
| Địa chỉ giao hàng mặc định | `123 Le Loi, Q1, TP.HCM` |
| Email | `test@eshop.com` |

## Test steps
1. Mở màn hình Quản lý hồ sơ cá nhân trên Mobile.
2. Nhập Họ Tên, Số điện thoại và Địa chỉ giao hàng mặc định theo bảng Test data.
3. Lưu cập nhật hồ sơ.
4. Quan sát thông báo phản hồi và dữ liệu hồ sơ sau khi lưu.

## Expected result
Hệ thống chấp nhận số điện thoại `09123456789`, cập nhật hồ sơ thành công và hiển thị rõ thông báo thành công. Dữ liệu hồ sơ sau khi lưu hiển thị đúng các giá trị đã nhập.

## Status / Related bugs
Not Run / None

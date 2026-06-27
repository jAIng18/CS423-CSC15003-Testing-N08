# TC-FR26-DT-003: Từ chối cập nhật hồ sơ khi số điện thoại không bắt đầu bằng 0

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
| Số điện thoại | Text | Invalid nếu không bắt đầu bằng `0` |
| Địa chỉ giao hàng mặc định | Text | Giá trị danh nghĩa hợp lệ: `123 Le Loi, Q1, TP.HCM` |
| Email | UI field | Hiển thị read-only |

### Domain Matrix

| TC | Trạng thái / Quyền | Họ Tên | Số điện thoại | Email / role | Expected |
|---|---|---|---|---|---|
| COND-FR26-DT-003 | EC-AUTH-V01, EC-OWNER-V01 | EC-NAME-V01 | EC-PHONE-I01: `1912345678` | EC-EMAIL-V01, EC-ROLE-V01 | Từ chối cập nhật, hiển thị lỗi số điện thoại không hợp lệ, dữ liệu không thay đổi |

## Preconditions
- Ứng dụng Mobile đang hoạt động.
- Người dùng `test@eshop.com` đã đăng nhập.
- Hồ sơ hiện tại đã có dữ liệu trước khi kiểm thử để đối chiếu sau khi lưu bị từ chối.

## Test data

| Field | Value |
|---|---|
| Họ Tên | `Nguyen Van Mobile` |
| Số điện thoại | `1912345678` |
| Địa chỉ giao hàng mặc định | `123 Le Loi, Q1, TP.HCM` |

## Test steps
1. Mở màn hình Quản lý hồ sơ cá nhân trên Mobile.
2. Nhập Họ Tên và Địa chỉ giao hàng mặc định bằng giá trị hợp lệ danh nghĩa.
3. Nhập Số điện thoại là `1912345678`.
4. Lưu cập nhật hồ sơ.
5. Quan sát thông báo lỗi và kiểm tra dữ liệu hồ sơ sau thao tác.

## Expected result
Hệ thống từ chối cập nhật vì số điện thoại không bắt đầu bằng `0`, hiển thị rõ lỗi về số điện thoại không hợp lệ, và dữ liệu hồ sơ đã lưu trước đó không bị thay đổi.

## Status / Related bugs
Not Run / None

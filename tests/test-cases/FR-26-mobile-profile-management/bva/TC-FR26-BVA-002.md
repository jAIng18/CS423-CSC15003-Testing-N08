# TC-FR26-BVA-002: Cập nhật hồ sơ thành công khi số điện thoại có 10 chữ số

## Requirement ID
FR-26

## Module / Test type / Technique
Mobile Profile Management / Functional / Boundary Value Analysis (BVA)

## Boundary Analysis

### Identified Boundaries

| Variable | Constraint | Boundary Type | BVA Points |
|---|---|---|---|
| Số điện thoại | Bắt đầu bằng `0`, từ 10-11 chữ số | Lower boundary: min = 10 chữ số | 9 chữ số (OFF⁻), **10 chữ số (ON)**, 11 chữ số (OFF⁺) |
| Số điện thoại | Bắt đầu bằng `0`, từ 10-11 chữ số | Upper boundary: max = 11 chữ số | 10 chữ số (OFF⁻), **11 chữ số (ON)**, 12 chữ số (OFF⁺) |

### BVA Test Matrix

| TC | Số điện thoại | Measured value | Boundary Point | Các ràng buộc khác | Expected |
|---|---|---:|---|---|---|
| COND-FR26-BVA-002 | `0912345678` | 10 chữ số | Lower ON / Upper OFF⁻ / BV-PHONE-002 | Đã đăng nhập; đúng chủ sở hữu; Họ Tên và Địa chỉ hợp lệ danh nghĩa; Email read-only; không đổi `role` | Chấp nhận cập nhật, hiển thị thông báo thành công |

> **Ghi chú:** Chuỗi `0912345678` có đúng 10 chữ số, bắt đầu bằng `0` và chỉ gồm chữ số. Đây là ON point của lower boundary.

## Preconditions
- Ứng dụng Mobile đang hoạt động.
- Người dùng `test@eshop.com` đã đăng nhập.
- Người dùng đang ở màn hình Quản lý hồ sơ cá nhân của chính mình.

## Test data

| Field | Value |
|---|---|
| Họ Tên | `Nguyen Van Mobile` |
| Số điện thoại | `0912345678` |
| Độ dài số điện thoại | 10 chữ số |
| Địa chỉ giao hàng mặc định | `123 Le Loi, Q1, TP.HCM` |

## Test steps
1. Mở màn hình Quản lý hồ sơ cá nhân trên Mobile.
2. Nhập Họ Tên, Số điện thoại và Địa chỉ giao hàng mặc định theo bảng Test data.
3. Lưu cập nhật hồ sơ.
4. Quan sát thông báo phản hồi và dữ liệu hồ sơ sau khi lưu.

## Expected result
Hệ thống chấp nhận số điện thoại `0912345678` vì có đúng 10 chữ số, cập nhật hồ sơ thành công và hiển thị rõ thông báo thành công. Dữ liệu hồ sơ sau khi lưu hiển thị đúng giá trị đã nhập.

## Status / Related bugs
Not Run / None

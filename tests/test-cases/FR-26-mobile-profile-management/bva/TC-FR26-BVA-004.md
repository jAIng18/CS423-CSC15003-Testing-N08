# TC-FR26-BVA-004: Từ chối cập nhật hồ sơ khi số điện thoại có 12 chữ số

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
| COND-FR26-BVA-004 | `091234567890` | 12 chữ số | Upper OFF⁺ / BV-PHONE-004 | Đã đăng nhập; đúng chủ sở hữu; Họ Tên và Địa chỉ hợp lệ danh nghĩa; Email read-only; không đổi `role` | Từ chối cập nhật, hiển thị lỗi số điện thoại không hợp lệ, hồ sơ không thay đổi |

> **Ghi chú:** Chuỗi `091234567890` có đúng 12 chữ số, bắt đầu bằng `0` và chỉ gồm chữ số. Test case isolate lỗi độ dài ngay trên maximum boundary.

## Preconditions
- Ứng dụng Mobile đang hoạt động.
- Người dùng `test@eshop.com` đã đăng nhập.
- Người dùng đang ở màn hình Quản lý hồ sơ cá nhân của chính mình.
- Hồ sơ hiện tại đã có dữ liệu trước khi kiểm thử để đối chiếu sau khi lưu bị từ chối.

## Test data

| Field | Value |
|---|---|
| Họ Tên | `Nguyen Van Mobile` |
| Số điện thoại | `091234567890` |
| Độ dài số điện thoại | 12 chữ số |
| Địa chỉ giao hàng mặc định | `123 Le Loi, Q1, TP.HCM` |

## Test steps
1. Mở màn hình Quản lý hồ sơ cá nhân trên Mobile.
2. Nhập Họ Tên và Địa chỉ giao hàng mặc định bằng giá trị hợp lệ danh nghĩa.
3. Nhập Số điện thoại là `091234567890`.
4. Lưu cập nhật hồ sơ.
5. Quan sát thông báo lỗi và kiểm tra dữ liệu hồ sơ sau thao tác.

## Expected result
Hệ thống từ chối cập nhật vì số điện thoại có 12 chữ số, cao hơn maximum boundary 11 chữ số. Giao diện mobile hiển thị rõ lỗi về số điện thoại không hợp lệ và dữ liệu hồ sơ đã lưu trước đó không bị thay đổi.

## Status / Related bugs
Not Run / None

# TC-FR26-DT-009: Chặn cập nhật hồ sơ mobile khi người dùng chưa đăng nhập

## Requirement ID
FR-26

## Module / Test type / Technique
Mobile Profile Management / Functional / Domain Testing

## Domain Analysis

### Input Variables & Domain

| Variable | Type | Domain / Constraints |
|---|---|---|
| Trạng thái đăng nhập | System state | Invalid nếu người dùng chưa đăng nhập nhưng cố cập nhật hồ sơ |
| Chủ sở hữu hồ sơ | System state | Không xác định khi chưa có người dùng đăng nhập |
| Họ Tên | Text | Không áp dụng trong trạng thái chưa đăng nhập |
| Số điện thoại | Text | Không áp dụng trong trạng thái chưa đăng nhập |
| Địa chỉ giao hàng mặc định | Text | Không áp dụng trong trạng thái chưa đăng nhập |

### Domain Matrix

| TC | Trạng thái / Quyền | Họ Tên | Số điện thoại | Email / role | Expected |
|---|---|---|---|---|---|
| COND-FR26-DT-009 | EC-AUTH-I01: chưa đăng nhập | Không áp dụng | Không áp dụng | Không áp dụng | Người dùng không được cập nhật hồ sơ; hệ thống chặn truy cập hoặc yêu cầu đăng nhập |

## Preconditions
- Ứng dụng Mobile đang hoạt động.
- Không có phiên đăng nhập hợp lệ trên ứng dụng Mobile.

## Test data

| Field | Value |
|---|---|
| Trạng thái phiên | Chưa đăng nhập |

## Test steps
1. Mở ứng dụng Mobile khi chưa đăng nhập.
2. Cố truy cập màn hình Quản lý hồ sơ cá nhân.
3. Nếu màn hình hồ sơ vẫn hiển thị, cố nhập dữ liệu hồ sơ và lưu cập nhật.
4. Quan sát phản hồi của hệ thống.

## Expected result
Hệ thống không cho người dùng chưa đăng nhập cập nhật hồ sơ cá nhân. Ứng dụng chặn truy cập hoặc yêu cầu đăng nhập trước khi cập nhật; không có dữ liệu hồ sơ nào được tạo hoặc thay đổi từ phiên chưa đăng nhập.

## Status / Related bugs
Not Run / None

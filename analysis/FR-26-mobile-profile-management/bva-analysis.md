# Phân tích Boundary Value Analysis — FR-26: Quản lý hồ sơ cá nhân trên Mobile

## 1. Tóm tắt requirement

| Thuộc tính | Nội dung |
|---|---|
| Requirement ID | FR-26 |
| Tên chức năng | Quản lý hồ sơ cá nhân trên Mobile |
| Input | Họ Tên, Số điện thoại, Địa chỉ giao hàng mặc định |
| Validation rules | Số điện thoại hợp lệ bắt đầu bằng số `0`, từ 10-11 chữ số |
| Business rules | Email hiển thị read-only; người dùng chỉ cập nhật hồ sơ của chính mình; không thể tự thay đổi thuộc tính `role` |
| Preconditions | Người dùng đã đăng nhập để cập nhật hồ sơ cá nhân |
| Success condition | Cập nhật hồ sơ thành công và giao diện mobile hiển thị rõ thông báo thành công |
| Error conditions | Nhập số điện thoại không hợp lệ thì giao diện mobile hiển thị lỗi rõ ràng; không cập nhật hồ sơ khi dữ liệu không hợp lệ |

## 2. Các biến có biên

| Variable | Type | Constraint | Lower boundary | Upper boundary | Inclusive / Exclusive | Unit | Nominal value | Requirement source | Thông tin còn thiếu |
|---|---|---|---:|---:|---|---|---|---|---|
| Số điện thoại | Text length | Bắt đầu bằng `0`, từ 10-11 chữ số | 10 | 11 | Inclusive cả hai biên | Số chữ số | `0912345678` hoặc `09123456789` | FR-26 | Nội dung chính xác của thông báo lỗi chưa được đặc tả |

Các trường/điều kiện không áp dụng BVA:

| Field / Condition | Lý do không áp dụng BVA |
|---|---|
| Họ Tên | Requirement không nêu độ dài tối thiểu/tối đa, format hoặc bắt buộc/rỗng |
| Địa chỉ giao hàng mặc định | Requirement không nêu độ dài tối thiểu/tối đa, format hoặc bắt buộc/rỗng |
| Email read-only | Đây là trạng thái UI/permission, không phải miền có thứ tự hoặc ngưỡng |
| Thuộc tính `role` | Đây là rule phân quyền, không phải miền có thứ tự hoặc ngưỡng |
| Trạng thái đăng nhập | Đây là điều kiện trạng thái, không có threshold số học trong FR-26 |
| Chủ sở hữu hồ sơ | Đây là điều kiện phân quyền, không có boundary số học trong FR-26 |

## 3. Phương pháp BVA được sử dụng

- Phương pháp: Robust Boundary Value Analysis rút gọn theo giá trị duy nhất.
- Lý do lựa chọn: Requirement có khoảng độ dài hợp lệ `[10, 11]` chữ số cho Số điện thoại. Robust BVA phù hợp vì cần kiểm tra cả giá trị ngay ngoài biên (`9`, `12`) và ngay trên biên (`10`, `11`). Do khoảng hợp lệ chỉ có hai giá trị nguyên, các điểm `min + 1` và `max` trùng nhau ở độ dài `11`, còn `max - 1` và `min` trùng nhau ở độ dài `10`; vì vậy không tạo test case trùng lặp.

## 4. Xác định ON, OFF⁻ và OFF⁺

| Variable | Boundary | OFF⁻ | ON | OFF⁺ | Ghi chú |
|---|---|---:|---:|---:|---|
| Số điện thoại | Lower boundary: độ dài tối thiểu 10 chữ số | 9 | 10 | 11 | Với minimum boundary, `OFF⁻ = min - 1` là invalid; `ON = min` là valid; `OFF⁺ = min + 1` vẫn valid và trùng upper boundary |
| Số điện thoại | Upper boundary: độ dài tối đa 11 chữ số | 10 | 11 | 12 | Với maximum boundary, `OFF⁻ = max - 1` vẫn valid và trùng lower boundary; `ON = max` là valid; `OFF⁺ = max + 1` là invalid |

## 5. Boundary Value Derivation

| Boundary Value ID | Variable | Constraint | Boundary type | Boundary point | Formula | Test value | Độ dài số học | Validity | Expected behavior | Requirement reference |
|---|---|---|---|---|---|---|---:|---|---|---|
| BV-PHONE-001 | Số điện thoại | Từ 10-11 chữ số, bắt đầu bằng `0` | Lower boundary | OFF⁻ | `min - 1 = 10 - 1` | `091234567` | 9 | Invalid | Từ chối cập nhật, hiển thị lỗi số điện thoại không hợp lệ, hồ sơ không thay đổi | FR-26 |
| BV-PHONE-002 | Số điện thoại | Từ 10-11 chữ số, bắt đầu bằng `0` | Lower boundary / Upper boundary | ON lower / OFF⁻ upper | `min = 10`, đồng thời `max - 1 = 11 - 1` | `0912345678` | 10 | Valid | Chấp nhận cập nhật và hiển thị thông báo thành công | FR-26 |
| BV-PHONE-003 | Số điện thoại | Từ 10-11 chữ số, bắt đầu bằng `0` | Lower boundary / Upper boundary | OFF⁺ lower / ON upper | `min + 1 = 10 + 1`, đồng thời `max = 11` | `09123456789` | 11 | Valid | Chấp nhận cập nhật và hiển thị thông báo thành công | FR-26 |
| BV-PHONE-004 | Số điện thoại | Từ 10-11 chữ số, bắt đầu bằng `0` | Upper boundary | OFF⁺ | `max + 1 = 11 + 1` | `091234567890` | 12 | Invalid | Từ chối cập nhật, hiển thị lỗi số điện thoại không hợp lệ, hồ sơ không thay đổi | FR-26 |

## 6. Dependent Boundaries

| ID | Quan hệ | Boundary values | Expected behavior |
|---|---|---|---|
| DB-01 | Độ dài số điện thoại chỉ được đánh giá hợp lệ khi chuỗi bắt đầu bằng `0` và chỉ gồm chữ số | BV-PHONE-001 đến BV-PHONE-004 đều dùng chuỗi bắt đầu bằng `0` và chỉ gồm chữ số | Isolate boundary độ dài; không trộn lỗi prefix hoặc ký tự không phải chữ số vào test BVA |

## 7. BVA Test Matrix

| Test Condition | Variable | Boundary Value ID | Point | Test value | Các ràng buộc khác | Expected validity | Expected behavior | Lý do lựa chọn |
|---|---|---|---|---|---|---|---|---|
| COND-FR26-BVA-001 | Số điện thoại | BV-PHONE-001 | Lower OFF⁻ | `091234567` | Đã đăng nhập; đúng chủ sở hữu; Họ Tên `Nguyen Van Mobile`; Địa chỉ `123 Le Loi, Q1, TP.HCM`; Email read-only; không đổi `role` | Invalid | Từ chối cập nhật, hiển thị lỗi số điện thoại không hợp lệ, hồ sơ không thay đổi | Kiểm tra ngay dưới độ dài tối thiểu |
| COND-FR26-BVA-002 | Số điện thoại | BV-PHONE-002 | Lower ON / Upper OFF⁻ | `0912345678` | Đã đăng nhập; đúng chủ sở hữu; các trường khác valid nominal | Valid | Chấp nhận cập nhật, hiển thị thông báo thành công | Kiểm tra đúng độ dài tối thiểu |
| COND-FR26-BVA-003 | Số điện thoại | BV-PHONE-003 | Lower OFF⁺ / Upper ON | `09123456789` | Đã đăng nhập; đúng chủ sở hữu; các trường khác valid nominal | Valid | Chấp nhận cập nhật, hiển thị thông báo thành công | Kiểm tra đúng độ dài tối đa |
| COND-FR26-BVA-004 | Số điện thoại | BV-PHONE-004 | Upper OFF⁺ | `091234567890` | Đã đăng nhập; đúng chủ sở hữu; các trường khác valid nominal | Invalid | Từ chối cập nhật, hiển thị lỗi số điện thoại không hợp lệ, hồ sơ không thay đổi | Kiểm tra ngay trên độ dài tối đa |

## 8. Quá trình lựa chọn test case

Chỉ biến Số điện thoại có boundary phù hợp với BVA trong FR-26. Các test case được chọn theo Robust BVA nhưng loại bỏ điểm trùng lặp do khoảng `[10, 11]` quá hẹp: độ dài `10` vừa là lower ON vừa là upper OFF⁻; độ dài `11` vừa là lower OFF⁺ vừa là upper ON. Mỗi test case chỉ thay đổi độ dài của số điện thoại, còn prefix `0`, ký tự chữ số, trạng thái đăng nhập, chủ sở hữu hồ sơ, Họ Tên, Địa chỉ, Email read-only và `role` đều giữ hợp lệ danh nghĩa để isolate boundary độ dài.

Không tạo BVA cho Họ Tên, Địa chỉ, Email, `role`, đăng nhập hoặc chủ sở hữu hồ sơ vì FR-26 không đặc tả min/max, độ dài, số lượng, ngày giờ, ngưỡng hoặc miền có thứ tự cho các mục này.

## 9. Ma trận truy vết

| Test Case ID | Boundary Value ID | Boundary | Test Value | Expected Validity | Requirement Reference |
|---|---|---|---|---|---|
| TC-FR26-BVA-001 | BV-PHONE-001 | Lower OFF⁻: 9 chữ số | `091234567` | Invalid | FR-26 |
| TC-FR26-BVA-002 | BV-PHONE-002 | Lower ON / Upper OFF⁻: 10 chữ số | `0912345678` | Valid | FR-26 |
| TC-FR26-BVA-003 | BV-PHONE-003 | Lower OFF⁺ / Upper ON: 11 chữ số | `09123456789` | Valid | FR-26 |
| TC-FR26-BVA-004 | BV-PHONE-004 | Upper OFF⁺: 12 chữ số | `091234567890` | Invalid | FR-26 |

## 10. Tổng kết độ bao phủ

| Metric | Value |
|---|---:|
| Tổng biến có biên | 1 |
| Tổng lower boundaries | 1 |
| Tổng upper boundaries | 1 |
| Tổng dependent boundaries | 1 |
| Phương pháp BVA | Robust BVA rút gọn |
| Tổng boundary values | 4 |
| Tổng test cases | 4 |
| Boundary values đã cover | 4 |
| Boundary values chưa cover | 0 |

| Boundary Value ID | Trạng thái coverage | Ghi chú |
|---|---|---|
| BV-PHONE-001 | Đã cover | TC-FR26-BVA-001 |
| BV-PHONE-002 | Đã cover | TC-FR26-BVA-002 |
| BV-PHONE-003 | Đã cover | TC-FR26-BVA-003 |
| BV-PHONE-004 | Đã cover | TC-FR26-BVA-004 |

## 11. Giả định và thông tin chưa được đặc tả

- Nội dung chính xác của thông báo thành công và lỗi số điện thoại không hợp lệ chưa được đặc tả; test case chỉ yêu cầu thông báo rõ ràng, quan sát được và đúng ý nghĩa.
- Requirement không đặc tả Họ Tên và Địa chỉ giao hàng mặc định có bắt buộc, giới hạn độ dài, format hoặc cho phép rỗng hay không; vì vậy không áp dụng BVA cho hai trường này.
- Requirement không đặc tả boundary nào cho Email read-only, `role`, trạng thái đăng nhập hoặc chủ sở hữu hồ sơ.
- Tài liệu API chỉ được dùng để kiểm tra tính nhất quán rằng hồ sơ cá nhân có các trường tương ứng với Họ Tên, Số điện thoại và Địa chỉ giao hàng mặc định; analysis này không đưa endpoint, method, request body hoặc công cụ kiểm thử vào nội dung test.

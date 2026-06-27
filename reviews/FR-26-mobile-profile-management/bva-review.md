# Review test case - FR-26: Quản lý hồ sơ cá nhân trên Mobile

## 1. Phạm vi review

| Hạng mục | File |
|---|---|
| Requirement chính | `requirements/system-requirements.md` - FR-26 |
| Tài liệu đối chiếu kỹ thuật | `requirements/api-specification.md` - chỉ dùng để kiểm tra tính nhất quán trường hồ sơ, không đưa endpoint/method/request body/công cụ kiểm thử vào test case |
| BVA analysis | `analysis/FR-26-mobile-profile-management/bva-analysis.md` |
| BVA test case | `tests/test-cases/FR-26-mobile-profile-management/bva/TC-FR26-BVA-001.md` đến `TC-FR26-BVA-004.md` |

## 2. Tóm tắt kết quả

| Tiêu chí | Kết quả review |
|---|---|
| FR-26 có boundary phù hợp để áp dụng BVA | Đạt: độ dài Số điện thoại từ 10-11 chữ số |
| Không áp dụng BVA gượng ép cho field không có biên | Đạt |
| Phương pháp BVA được nêu rõ | Đạt: Robust BVA rút gọn theo giá trị duy nhất |
| ON, OFF⁻, OFF⁺ được giải thích đúng | Đạt |
| Test data cụ thể và đúng độ dài | Đạt: `091234567` = 9, `0912345678` = 10, `09123456789` = 11, `091234567890` = 12 chữ số |
| Expected Result quan sát được | Đạt |
| Traceability từ test case về boundary value | Đạt |
| Status ban đầu | Đạt: tất cả là `Not Run / None` |
| Không đưa endpoint/method/request body/công cụ kiểm thử | Đạt |

## 3. Findings cần sửa

Không còn finding cần sửa trước execution.

| Finding ID | Mức độ | File | Mô tả | Trạng thái |
|---|---|---|---|---|
| REV-FR26-BVA-001 | Minor | `analysis/FR-26-mobile-profile-management/bva-analysis.md` | Cần giải thích rõ vì sao Robust BVA chỉ sinh 4 giá trị duy nhất cho khoảng `[10, 11]`. | Đã xử lý trong mục phương pháp BVA và quá trình lựa chọn test case |
| REV-FR26-BVA-002 | Minor | `tests/test-cases/FR-26-mobile-profile-management/bva/*.md` | Cần ghi rõ measured value để kiểm tra độ dài số điện thoại. | Đã xử lý trong Boundary Analysis và Test data từng test case |

## 4. Traceability audit

| Test Case ID | Requirement | Analysis condition | Class/Boundary | Trạng thái | Ghi chú |
|---|---|---|---|---|---|
| TC-FR26-BVA-001 | FR-26 | COND-FR26-BVA-001 | BV-PHONE-001: Lower OFF⁻, 9 chữ số | Hợp lệ | Invalid ngay dưới minimum boundary |
| TC-FR26-BVA-002 | FR-26 | COND-FR26-BVA-002 | BV-PHONE-002: Lower ON / Upper OFF⁻, 10 chữ số | Hợp lệ | Valid tại minimum boundary |
| TC-FR26-BVA-003 | FR-26 | COND-FR26-BVA-003 | BV-PHONE-003: Lower OFF⁺ / Upper ON, 11 chữ số | Hợp lệ | Valid tại maximum boundary |
| TC-FR26-BVA-004 | FR-26 | COND-FR26-BVA-004 | BV-PHONE-004: Upper OFF⁺, 12 chữ số | Hợp lệ | Invalid ngay trên maximum boundary |

## 5. Coverage và duplicate audit

| Coverage item | Test case cover | Trạng thái | Ghi chú |
|---|---|---|---|
| Lower OFF⁻: 9 chữ số | TC-FR26-BVA-001 | Đạt | Dữ liệu bắt đầu bằng `0` và chỉ gồm chữ số để isolate lỗi độ dài |
| Lower ON: 10 chữ số | TC-FR26-BVA-002 | Đạt | Đồng thời là Upper OFF⁻ do khoảng `[10, 11]` |
| Upper ON: 11 chữ số | TC-FR26-BVA-003 | Đạt | Đồng thời là Lower OFF⁺ do khoảng `[10, 11]` |
| Upper OFF⁺: 12 chữ số | TC-FR26-BVA-004 | Đạt | Dữ liệu bắt đầu bằng `0` và chỉ gồm chữ số để isolate lỗi độ dài |
| Họ Tên | Không tạo BVA test case | Không áp dụng | Requirement không đặc tả min/max/độ dài/ngưỡng |
| Địa chỉ giao hàng mặc định | Không tạo BVA test case | Không áp dụng | Requirement không đặc tả min/max/độ dài/ngưỡng |
| Email read-only | Không tạo BVA test case | Không áp dụng | Điều kiện UI/permission, không phải boundary |
| Thuộc tính `role` | Không tạo BVA test case | Không áp dụng | Điều kiện phân quyền, không phải boundary |

Không phát hiện duplicate mục tiêu kiểm thử. Việc không tạo thêm case cho `min + 1` và `max - 1` riêng biệt là hợp lý vì chúng trùng với các điểm 11 và 10 chữ số đã cover.

## 6. Kết luận readiness

Sẵn sàng execution.

FR-26 có boundary phù hợp ở độ dài Số điện thoại nên việc tạo BVA analysis và 4 test case là phù hợp. Các field không có boundary đã được ghi rõ lý do không áp dụng, tránh tạo test case gượng ép.

## 7. Giả định và thông tin cần xác nhận

- Cần xác nhận nội dung chính xác của thông báo thành công và lỗi số điện thoại nếu execution yêu cầu so khớp text tuyệt đối.
- Requirement không đặc tả boundary cho Họ Tên, Địa chỉ, Email read-only, `role`, trạng thái đăng nhập hoặc chủ sở hữu hồ sơ.

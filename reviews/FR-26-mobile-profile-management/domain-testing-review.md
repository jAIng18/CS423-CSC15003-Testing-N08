# Review test case - FR-26: Quản lý hồ sơ cá nhân trên Mobile

## 1. Phạm vi review

| Hạng mục | File |
|---|---|
| Requirement chính | `requirements/system-requirements.md` - FR-26 |
| Tài liệu đối chiếu kỹ thuật | `requirements/api-specification.md` - chỉ dùng để kiểm tra tính nhất quán trường hồ sơ, không đưa endpoint/method/request body/công cụ kiểm thử vào test case |
| Analysis | `analysis/FR-26-mobile-profile-management/domain-testing-analysis.md` |
| Test case | `tests/test-cases/FR-26-mobile-profile-management/domain-testing/TC-FR26-DT-001.md` đến `TC-FR26-DT-009.md` |

## 2. Tóm tắt kết quả

| Tiêu chí | Kết quả review |
|---|---|
| Đúng requirement FR-26 | Đạt |
| Đúng technique Domain Testing | Đạt |
| Có valid và invalid equivalence class | Đạt |
| Có dependent condition | Đạt |
| Test data cụ thể | Đạt |
| Expected Result quan sát được | Đạt |
| Traceability từ test case về condition/class | Đạt |
| Status ban đầu | Đạt: tất cả là `Not Run / None` |
| Không đưa endpoint/method/request body/công cụ kiểm thử | Đạt |
| Không bịa constraint ngoài requirement | Đạt; các phần thiếu được ghi `Chưa được đặc tả` |

## 3. Findings cần sửa

Không còn finding cần sửa trước execution.

| Finding ID | Mức độ | File | Mô tả | Trạng thái |
|---|---|---|---|---|
| REV-FR26-001 | Minor | `analysis/FR-26-mobile-profile-management/domain-testing-analysis.md` | Cần ghi rõ các miền chưa đủ requirement như Họ Tên rỗng, Địa chỉ rỗng và cập nhật hồ sơ người khác để tránh bịa test case. | Đã xử lý trong mục coverage và giả định |
| REV-FR26-002 | Minor | `tests/test-cases/FR-26-mobile-profile-management/domain-testing/TC-FR26-DT-007.md`, `TC-FR26-DT-008.md` | Các case Email/role cần giữ ở mức UI black-box, không dùng API hay request body. | Đã xử lý: test steps chỉ thao tác/quan sát trên Mobile UI |

## 4. Traceability audit

| Test Case ID | Requirement | Analysis condition | Class/Boundary | Trạng thái | Ghi chú |
|---|---|---|---|---|---|
| TC-FR26-DT-001 | FR-26 | COND-FR26-DT-001 | EC-AUTH-V01, EC-OWNER-V01, EC-NAME-V01, EC-PHONE-V01, EC-ADDRESS-V01, EC-EMAIL-V01, EC-ROLE-V01, EC-FEEDBACK-V01, DC-01 đến DC-05 | Hợp lệ | Luồng hợp lệ danh nghĩa |
| TC-FR26-DT-002 | FR-26 | COND-FR26-DT-002 | EC-PHONE-V02, EC-FEEDBACK-V01, DC-03 | Hợp lệ | Bao phủ số điện thoại hợp lệ 11 chữ số |
| TC-FR26-DT-003 | FR-26 | COND-FR26-DT-003 | EC-PHONE-I01, EC-FEEDBACK-I01, DC-03 | Hợp lệ | Invalid isolate: sai tiền tố |
| TC-FR26-DT-004 | FR-26 | COND-FR26-DT-004 | EC-PHONE-I02, EC-FEEDBACK-I01, DC-03 | Hợp lệ | Invalid isolate: thiếu độ dài tối thiểu |
| TC-FR26-DT-005 | FR-26 | COND-FR26-DT-005 | EC-PHONE-I03, EC-FEEDBACK-I01, DC-03 | Hợp lệ | Invalid isolate: vượt độ dài tối đa |
| TC-FR26-DT-006 | FR-26 | COND-FR26-DT-006 | EC-PHONE-I04, EC-FEEDBACK-I01, DC-03 | Hợp lệ | Invalid isolate: có ký tự không phải chữ số |
| TC-FR26-DT-007 | FR-26 | COND-FR26-DT-007 | EC-EMAIL-I01, DC-04 | Hợp lệ | Kiểm tra Email read-only qua UI |
| TC-FR26-DT-008 | FR-26 | COND-FR26-DT-008 | EC-ROLE-I01, DC-05 | Hợp lệ | Kiểm tra không tự đổi `role` qua UI |
| TC-FR26-DT-009 | FR-26 | COND-FR26-DT-009 | EC-AUTH-I01, DC-01 | Hợp lệ | Kiểm tra trạng thái chưa đăng nhập |

## 5. Coverage và duplicate audit

| Coverage item | Test case cover | Trạng thái | Ghi chú |
|---|---|---|---|
| Luồng cập nhật hợp lệ | TC-FR26-DT-001 | Đạt | Bao phủ dữ liệu danh nghĩa và thông báo thành công |
| Số điện thoại hợp lệ 11 chữ số | TC-FR26-DT-002 | Đạt | Không duplicate với TC-FR26-DT-001 vì đại diện cho miền hợp lệ khác |
| Số điện thoại sai tiền tố | TC-FR26-DT-003 | Đạt | Một invalid chính |
| Số điện thoại ít hơn 10 chữ số | TC-FR26-DT-004 | Đạt | Một invalid chính |
| Số điện thoại nhiều hơn 11 chữ số | TC-FR26-DT-005 | Đạt | Một invalid chính |
| Số điện thoại chứa ký tự không phải chữ số | TC-FR26-DT-006 | Đạt | Một invalid chính, suy ra từ "chữ số" |
| Email read-only | TC-FR26-DT-007 | Đạt | Không dùng API-level manipulation |
| Không tự thay đổi `role` | TC-FR26-DT-008 | Đạt | Không dùng API-level manipulation |
| Chưa đăng nhập | TC-FR26-DT-009 | Đạt | Expected Result không ép redirect cụ thể vì chưa được đặc tả |
| Họ Tên invalid | Không cover | Bị chặn do thiếu requirement | Requirement không nêu bắt buộc/format/độ dài |
| Địa chỉ invalid | Không cover | Bị chặn do thiếu requirement | Requirement không nêu bắt buộc/format/độ dài |
| Cập nhật hồ sơ người khác | Không cover | Bị chặn do thiếu requirement | Requirement không mô tả đường đi mobile UI để chọn hồ sơ khác |

Không phát hiện duplicate mục tiêu kiểm thử. Các test case invalid về số điện thoại dùng cùng dữ liệu danh nghĩa cho các trường khác để isolate điều kiện lỗi chính.

## 6. Kết luận readiness

Sẵn sàng execution.

Các test case đã đủ dữ liệu cụ thể, có Expected Result quan sát được, có traceability về Domain Matrix, không ghi Actual Result, và không gán Pass/Fail trước khi chạy.

## 7. Giả định và thông tin cần xác nhận

- Cần xác nhận nội dung chính xác của thông báo thành công và lỗi số điện thoại nếu execution yêu cầu so khớp text tuyệt đối.
- Cần xác nhận hành vi cụ thể khi chưa đăng nhập truy cập hồ sơ mobile: chặn truy cập, yêu cầu đăng nhập, hay redirect.
- Cần bổ sung requirement nếu muốn kiểm thử Họ Tên/Địa chỉ rỗng, giới hạn độ dài, hoặc cập nhật hồ sơ người dùng khác qua đường đi cụ thể trên mobile.

# Phân tích Domain Testing — FR-26: Quản lý hồ sơ cá nhân trên Mobile

## 1. Tóm tắt requirement

| Thuộc tính | Nội dung |
|---|---|
| Requirement ID | FR-26 |
| Tên chức năng | Quản lý hồ sơ cá nhân trên Mobile |
| Actor | Người dùng mobile |
| Preconditions | Người dùng đã đăng nhập để cập nhật hồ sơ cá nhân |
| Input | Họ Tên, Số điện thoại, Địa chỉ giao hàng mặc định; Email hiển thị read-only |
| Output | Thông báo thành công khi cập nhật hồ sơ, hoặc lỗi khi nhập số điện thoại không hợp lệ |
| Business rules | Người dùng chỉ có thể cập nhật hồ sơ của chính mình; không thể tự thay đổi thuộc tính `role`; Email không được phép thay đổi qua giao diện mobile |
| Validation rules | Số điện thoại hợp lệ bắt đầu bằng số `0`, từ 10-11 chữ số |
| Dependency | Trạng thái đăng nhập; hồ sơ thuộc đúng người dùng đang đăng nhập |
| Success condition | Hồ sơ của chính người dùng được cập nhật với dữ liệu hợp lệ và giao diện mobile hiển thị rõ thông báo thành công |
| Error conditions | Chưa đăng nhập; số điện thoại không hợp lệ; cố gắng thay đổi Email hoặc `role`; cố gắng cập nhật hồ sơ không thuộc chính mình |

## 2. Biến đầu vào và ràng buộc

| Variable / Condition | Type | Required | Domain / Constraints | Requirement source |
|---|---|---|---|---|
| Trạng thái đăng nhập | System state | Có | Người dùng phải đã đăng nhập để cập nhật hồ sơ cá nhân | FR-26: "Người dùng đã đăng nhập..." |
| Chủ sở hữu hồ sơ | System state | Có | Người dùng chỉ có thể cập nhật hồ sơ của chính mình | FR-26 |
| Họ Tên | Text | Chưa được đặc tả | Người dùng đã đăng nhập có thể cập nhật; format, độ dài, bắt buộc hay cho phép rỗng chưa được đặc tả | FR-26 |
| Số điện thoại | Text | Chưa được đặc tả | Hợp lệ khi bắt đầu bằng `0` và gồm 10-11 chữ số | FR-26 |
| Địa chỉ giao hàng mặc định | Text | Chưa được đặc tả | Người dùng đã đăng nhập có thể cập nhật; format, độ dài, bắt buộc hay cho phép rỗng chưa được đặc tả | FR-26 |
| Email | UI field / Text | Không được chỉnh sửa | Hiển thị dưới dạng read-only; không được phép thay đổi qua giao diện mobile | FR-26 |
| Thuộc tính `role` | System attribute | Không được chỉnh sửa | Người dùng không thể tự thay đổi thuộc tính `role` | FR-26 |
| Thông báo phản hồi | UI feedback | Có | Hiển thị rõ thông báo thành công khi cập nhật hồ sơ, hoặc lỗi khi số điện thoại không hợp lệ | FR-26 |

## 3. Phân vùng tương đương

| Class ID | Variable / Condition | Mô tả | Validity | Giá trị đại diện | Requirement source | Ghi chú |
|---|---|---|---|---|---|---|
| EC-AUTH-V01 | Trạng thái đăng nhập | Người dùng đã đăng nhập bằng tài khoản hợp lệ | Valid | `test@eshop.com` đã đăng nhập | FR-26 | Điều kiện danh nghĩa cho các case cập nhật |
| EC-AUTH-I01 | Trạng thái đăng nhập | Người dùng chưa đăng nhập nhưng cố truy cập/cập nhật hồ sơ | Invalid | Phiên mobile chưa đăng nhập | FR-26 | Expected cụ thể về redirect/chặn truy cập chưa được đặc tả |
| EC-OWNER-V01 | Chủ sở hữu hồ sơ | Hồ sơ đang cập nhật là hồ sơ của chính người dùng đăng nhập | Valid | Hồ sơ của `test@eshop.com` | FR-26 |  |
| EC-OWNER-I01 | Chủ sở hữu hồ sơ | Người dùng cố cập nhật hồ sơ của người dùng khác | Invalid | User A cố cập nhật hồ sơ User B | FR-26 | Cách thao tác qua mobile UI chưa được đặc tả |
| EC-NAME-V01 | Họ Tên | Họ tên có giá trị văn bản thông thường | Valid | `Nguyen Van Mobile` | FR-26 | Không có rule về độ dài/ký tự |
| EC-NAME-I01 | Họ Tên | Họ tên rỗng hoặc format không hợp lệ | Invalid | Chưa được đặc tả | FR-26 | Bị chặn do thiếu requirement, không sinh test lỗi |
| EC-PHONE-V01 | Số điện thoại | Số điện thoại bắt đầu bằng `0` và có 10 chữ số | Valid | `0912345678` | FR-26 |  |
| EC-PHONE-V02 | Số điện thoại | Số điện thoại bắt đầu bằng `0` và có 11 chữ số | Valid | `09123456789` | FR-26 |  |
| EC-PHONE-I01 | Số điện thoại | Không bắt đầu bằng `0` | Invalid | `1912345678` | FR-26 | Isolate điều kiện tiền tố |
| EC-PHONE-I02 | Số điện thoại | Ít hơn 10 chữ số | Invalid | `091234567` | FR-26 | Isolate điều kiện độ dài dưới |
| EC-PHONE-I03 | Số điện thoại | Nhiều hơn 11 chữ số | Invalid | `091234567890` | FR-26 | Isolate điều kiện độ dài trên |
| EC-PHONE-I04 | Số điện thoại | Có ký tự không phải chữ số | Invalid | `09123abc78` | FR-26 | Suy ra từ yêu cầu "chữ số" |
| EC-ADDRESS-V01 | Địa chỉ giao hàng mặc định | Địa chỉ có giá trị văn bản thông thường | Valid | `123 Le Loi, Q1, TP.HCM` | FR-26 | Không có rule về độ dài/ký tự |
| EC-ADDRESS-I01 | Địa chỉ giao hàng mặc định | Địa chỉ rỗng hoặc format không hợp lệ | Invalid | Chưa được đặc tả | FR-26 | Bị chặn do thiếu requirement, không sinh test lỗi |
| EC-EMAIL-V01 | Email | Email được hiển thị ở trạng thái read-only | Valid | `test@eshop.com` hiển thị read-only | FR-26 |  |
| EC-EMAIL-I01 | Email | Người dùng có thể chỉnh sửa Email trên giao diện mobile | Invalid | Đổi thành `other@eshop.com` | FR-26 | Hành vi không được phép |
| EC-ROLE-V01 | Thuộc tính `role` | Không có cơ chế tự thay đổi `role` trong quản lý hồ sơ mobile | Valid | Không hiển thị field `role` có thể chỉnh sửa | FR-26 |  |
| EC-ROLE-I01 | Thuộc tính `role` | Người dùng tự thay đổi `role` | Invalid | Đổi `role` thành `admin` | FR-26 | Cách nhập `role` qua UI chưa được đặc tả |
| EC-FEEDBACK-V01 | Thông báo phản hồi | Cập nhật hợp lệ hiển thị thông báo thành công rõ ràng | Valid | Thông báo cập nhật thành công | FR-26 | Nội dung chính xác chưa được đặc tả |
| EC-FEEDBACK-I01 | Thông báo phản hồi | Số điện thoại không hợp lệ hiển thị lỗi rõ ràng | Valid | Lỗi về số điện thoại không hợp lệ | FR-26 | Nội dung chính xác chưa được đặc tả |

## 4. Quan hệ phụ thuộc giữa các input và trạng thái hệ thống

| ID | Điều kiện phụ thuộc | Valid condition | Invalid condition | Requirement source |
|---|---|---|---|---|
| DC-01 | Quyền cập nhật phụ thuộc trạng thái đăng nhập | Đã đăng nhập thì được vào chức năng cập nhật hồ sơ | Chưa đăng nhập thì không được cập nhật hồ sơ | FR-26 |
| DC-02 | Quyền cập nhật phụ thuộc chủ sở hữu hồ sơ | Chỉ cập nhật hồ sơ của chính mình | Cập nhật hồ sơ của người dùng khác bị từ chối | FR-26 |
| DC-03 | Kết quả cập nhật phụ thuộc tính hợp lệ của số điện thoại | Số điện thoại bắt đầu bằng `0` và có 10-11 chữ số thì được chấp nhận | Số điện thoại sai tiền tố, sai độ dài hoặc chứa ký tự không phải chữ số thì bị từ chối | FR-26 |
| DC-04 | Email phụ thuộc trạng thái read-only của giao diện | Email chỉ hiển thị read-only và không bị thay đổi sau lưu | Email có thể sửa qua giao diện mobile là không hợp lệ | FR-26 |
| DC-05 | Thuộc tính `role` phụ thuộc quyền tự cập nhật hồ sơ | `role` không thể tự thay đổi trong chức năng hồ sơ | Người dùng tự đổi được `role` là không hợp lệ | FR-26 |

## 5. Domain Matrix

| Test Condition | Trạng thái / Quyền | Họ Tên | Số điện thoại | Email / role | Expected | Covered classes |
|---|---|---|---|---|---|---|
| COND-FR26-DT-001 | Đã đăng nhập, đúng chủ sở hữu | `Nguyen Van Mobile` | `0912345678` | Email read-only, không đổi `role` | Cập nhật được chấp nhận, hồ sơ thay đổi, hiển thị thông báo thành công | EC-AUTH-V01, EC-OWNER-V01, EC-NAME-V01, EC-PHONE-V01, EC-ADDRESS-V01, EC-EMAIL-V01, EC-ROLE-V01, EC-FEEDBACK-V01, DC-01, DC-02, DC-03, DC-04, DC-05 |
| COND-FR26-DT-002 | Đã đăng nhập, đúng chủ sở hữu | `Nguyen Van Mobile` | `09123456789` | Email read-only, không đổi `role` | Cập nhật được chấp nhận với số điện thoại 11 chữ số, hiển thị thông báo thành công | EC-PHONE-V02, EC-FEEDBACK-V01, DC-03 |
| COND-FR26-DT-003 | Đã đăng nhập, đúng chủ sở hữu | `Nguyen Van Mobile` | `1912345678` | Email read-only, không đổi `role` | Cập nhật bị từ chối, hiển thị lỗi số điện thoại không hợp lệ, dữ liệu hồ sơ không thay đổi | EC-PHONE-I01, EC-FEEDBACK-I01, DC-03 |
| COND-FR26-DT-004 | Đã đăng nhập, đúng chủ sở hữu | `Nguyen Van Mobile` | `091234567` | Email read-only, không đổi `role` | Cập nhật bị từ chối, hiển thị lỗi số điện thoại không hợp lệ, dữ liệu hồ sơ không thay đổi | EC-PHONE-I02, EC-FEEDBACK-I01, DC-03 |
| COND-FR26-DT-005 | Đã đăng nhập, đúng chủ sở hữu | `Nguyen Van Mobile` | `091234567890` | Email read-only, không đổi `role` | Cập nhật bị từ chối, hiển thị lỗi số điện thoại không hợp lệ, dữ liệu hồ sơ không thay đổi | EC-PHONE-I03, EC-FEEDBACK-I01, DC-03 |
| COND-FR26-DT-006 | Đã đăng nhập, đúng chủ sở hữu | `Nguyen Van Mobile` | `09123abc78` | Email read-only, không đổi `role` | Cập nhật bị từ chối, hiển thị lỗi số điện thoại không hợp lệ, dữ liệu hồ sơ không thay đổi | EC-PHONE-I04, EC-FEEDBACK-I01, DC-03 |
| COND-FR26-DT-007 | Đã đăng nhập, đúng chủ sở hữu | `Nguyen Van Mobile` | `0912345678` | Cố thay đổi Email từ giao diện | Email không chỉnh sửa được qua giao diện mobile; lưu hồ sơ không làm thay đổi Email | EC-EMAIL-I01, DC-04 |
| COND-FR26-DT-008 | Đã đăng nhập, đúng chủ sở hữu | `Nguyen Van Mobile` | `0912345678` | Cố tự thay đổi `role` | Giao diện mobile không cho tự thay đổi `role`; sau khi lưu, quyền của user không thay đổi | EC-ROLE-I01, DC-05 |
| COND-FR26-DT-009 | Chưa đăng nhập | Không áp dụng | Không áp dụng | Không áp dụng | Người dùng không được cập nhật hồ sơ; hệ thống chặn truy cập hoặc yêu cầu đăng nhập | EC-AUTH-I01, DC-01 |

## 6. Quá trình lựa chọn test case

Các test condition được chọn để bao phủ miền hợp lệ danh nghĩa, hai độ dài hợp lệ của số điện thoại, từng miền không hợp lệ của số điện thoại, và các điều kiện phụ thuộc quan trọng về quyền cập nhật hồ sơ. Không tạo tổ hợp mọi giá trị của Họ Tên và Địa chỉ vì requirement không đặc tả format, độ dài hoặc validation riêng cho hai trường này. Với các test case invalid về số điện thoại, Họ Tên, Địa chỉ, trạng thái đăng nhập, Email và `role` đều giữ ở trạng thái hợp lệ danh nghĩa để isolate lỗi chính.

`EC-NAME-I01`, `EC-ADDRESS-I01` và `EC-OWNER-I01` không được chuyển thành test case executable trong vòng này vì requirement chưa mô tả dữ liệu lỗi cụ thể hoặc cách thao tác qua mobile UI để chọn hồ sơ người dùng khác. Các mục này được ghi là requirement gap thay vì tự bịa hành vi.

## 7. Ma trận truy vết

| Test Case ID | Test Condition | Covered Classes | Requirement Reference | Lý do lựa chọn |
|---|---|---|---|---|
| TC-FR26-DT-001 | COND-FR26-DT-001 | EC-AUTH-V01, EC-OWNER-V01, EC-NAME-V01, EC-PHONE-V01, EC-ADDRESS-V01, EC-EMAIL-V01, EC-ROLE-V01, EC-FEEDBACK-V01, DC-01, DC-02, DC-03, DC-04, DC-05 | FR-26 | Luồng cập nhật hồ sơ hợp lệ danh nghĩa với số điện thoại 10 chữ số |
| TC-FR26-DT-002 | COND-FR26-DT-002 | EC-PHONE-V02, EC-FEEDBACK-V01, DC-03 | FR-26 | Xác nhận miền hợp lệ còn lại của số điện thoại 11 chữ số |
| TC-FR26-DT-003 | COND-FR26-DT-003 | EC-PHONE-I01, EC-FEEDBACK-I01, DC-03 | FR-26 | Kiểm tra rule bắt đầu bằng `0` |
| TC-FR26-DT-004 | COND-FR26-DT-004 | EC-PHONE-I02, EC-FEEDBACK-I01, DC-03 | FR-26 | Kiểm tra rule tối thiểu 10 chữ số |
| TC-FR26-DT-005 | COND-FR26-DT-005 | EC-PHONE-I03, EC-FEEDBACK-I01, DC-03 | FR-26 | Kiểm tra rule tối đa 11 chữ số |
| TC-FR26-DT-006 | COND-FR26-DT-006 | EC-PHONE-I04, EC-FEEDBACK-I01, DC-03 | FR-26 | Kiểm tra rule chỉ gồm chữ số |
| TC-FR26-DT-007 | COND-FR26-DT-007 | EC-EMAIL-I01, DC-04 | FR-26 | Xác nhận Email không được thay đổi qua giao diện mobile |
| TC-FR26-DT-008 | COND-FR26-DT-008 | EC-ROLE-I01, DC-05 | FR-26 | Xác nhận người dùng không thể tự thay đổi `role` |
| TC-FR26-DT-009 | COND-FR26-DT-009 | EC-AUTH-I01, DC-01 | FR-26 | Xác nhận chỉ người dùng đã đăng nhập mới cập nhật được hồ sơ |

## 8. Tổng kết độ bao phủ

| Metric | Value |
|---|---:|
| Tổng input/condition đã phân tích | 8 |
| Tổng valid classes | 9 |
| Tổng invalid classes | 8 |
| Tổng dependent conditions | 5 |
| Tổng test conditions | 9 |
| Tổng test cases | 9 |
| Classes đã cover | 14 |
| Classes chủ động loại trừ | 0 |
| Classes bị chặn do thiếu requirement | 3 |

| Class ID | Trạng thái coverage | Ghi chú |
|---|---|---|
| EC-AUTH-V01 | Đã cover | TC-FR26-DT-001 |
| EC-AUTH-I01 | Đã cover | TC-FR26-DT-009 |
| EC-OWNER-V01 | Đã cover | TC-FR26-DT-001 |
| EC-OWNER-I01 | Bị chặn do thiếu requirement | Chưa đặc tả cách mobile UI cho phép chọn/cập nhật hồ sơ người dùng khác |
| EC-NAME-V01 | Đã cover | TC-FR26-DT-001 |
| EC-NAME-I01 | Bị chặn do thiếu requirement | Chưa đặc tả Họ Tên bắt buộc, format hoặc độ dài |
| EC-PHONE-V01 | Đã cover | TC-FR26-DT-001 |
| EC-PHONE-V02 | Đã cover | TC-FR26-DT-002 |
| EC-PHONE-I01 | Đã cover | TC-FR26-DT-003 |
| EC-PHONE-I02 | Đã cover | TC-FR26-DT-004 |
| EC-PHONE-I03 | Đã cover | TC-FR26-DT-005 |
| EC-PHONE-I04 | Đã cover | TC-FR26-DT-006 |
| EC-ADDRESS-V01 | Đã cover | TC-FR26-DT-001 |
| EC-ADDRESS-I01 | Bị chặn do thiếu requirement | Chưa đặc tả Địa chỉ bắt buộc, format hoặc độ dài |
| EC-EMAIL-V01 | Đã cover | TC-FR26-DT-001 |
| EC-EMAIL-I01 | Đã cover | TC-FR26-DT-007 |
| EC-ROLE-V01 | Đã cover | TC-FR26-DT-001 |
| EC-ROLE-I01 | Đã cover | TC-FR26-DT-008 |
| EC-FEEDBACK-V01 | Đã cover | TC-FR26-DT-001, TC-FR26-DT-002 |
| EC-FEEDBACK-I01 | Đã cover | TC-FR26-DT-003 đến TC-FR26-DT-006 |

## 9. Giả định và thông tin chưa được đặc tả

- Nội dung chính xác của thông báo thành công và thông báo lỗi số điện thoại không hợp lệ chưa được đặc tả; test case chỉ yêu cầu thông báo rõ ràng, quan sát được và đúng ý nghĩa.
- Requirement không đặc tả Họ Tên và Địa chỉ giao hàng mặc định có bắt buộc, giới hạn độ dài, format, hoặc cho phép rỗng hay không.
- Requirement không đặc tả hành vi cụ thể khi người dùng chưa đăng nhập truy cập màn hình hồ sơ: redirect, modal yêu cầu đăng nhập, hay thông báo lỗi đều cần xác nhận khi execution.
- Requirement không đặc tả đường đi mobile UI để người dùng cố cập nhật hồ sơ của người dùng khác; do đó `EC-OWNER-I01` được ghi nhận là gap.
- Tài liệu API chỉ được dùng để kiểm tra tính nhất quán rằng hồ sơ cá nhân có các trường tương ứng với Họ Tên, Số điện thoại và Địa chỉ giao hàng mặc định; analysis này không đưa endpoint, method, request body hoặc công cụ kiểm thử vào nội dung test.

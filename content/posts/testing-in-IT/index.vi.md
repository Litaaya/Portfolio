---
title: "Software, Unit and Integration Testing"
date: 2026-08-02
draft: false
tags: ["software-testing", "unit-testing", "integration-testing"]
description: "Tổng quan về Software Testing, Unit Testing và Integration Testing"
---

> ref: \
    - https://www.guru99.com/software-testing-introduction-importance.html \
    - https://www.guru99.com/unit-testing-guide.html \
    - https://www.guru99.com/integration-testing.html
> 
> Hân hạnh cảm ơn chat GPT đã tài trợ vẽ mấy cái mô hình và ví dụ, vì người viết lười vào draw.io hoặc excalidraw để vẽ =))

---

Software Testing, Unit Testing và Integration Testing là ba khái niệm có quan hệ trực tiếp với nhau:

1. **Software Testing** là khái niệm tổng quát về kiểm thử phần mềm.
2. **Unit Testing** kiểm tra từng đơn vị mã nguồn riêng lẻ.
3. **Integration Testing** kiểm tra sự tương tác khi nhiều đơn vị hoặc module được kết nối với nhau.

Quy trình kiểm thử thường được mô tả theo thứ tự:

```text
Unit Testing
    ↓
Integration Testing
    ↓
System Testing
    ↓
Acceptance Testing
```

---

# Software Testing là gì?

**Software Testing**, hay kiểm thử phần mềm, là quá trình kiểm tra một ứng dụng hoặc hệ thống phần mềm nhằm xác định:

- Kết quả thực tế có khớp với kết quả mong đợi hay không.
- Phần mềm có đáp ứng các yêu cầu đã xác định hay không.
- Phần mềm có lỗi, thiếu sót hoặc hành vi không mong muốn hay không.
- Sản phẩm có đủ chất lượng, độ tin cậy và an toàn để đưa vào sử dụng hay không.

Việc kiểm thử có thể bao gồm thực thi một thành phần hoặc toàn bộ hệ thống để đánh giá một hoặc nhiều thuộc tính cần quan tâm.

Nói đơn giản, testing giúp xác minh rằng phần mềm hoạt động đúng như được thiết kế và giảm rủi ro lỗi xuất hiện khi đưa vào vận hành.

Tuy nhiên, kiểm thử không thể chứng minh tuyệt đối rằng phần mềm hoàn toàn không có lỗi. Nó chủ yếu giúp:

- Phát hiện các lỗi đang tồn tại.
- Giảm xác suất lỗi xuất hiện trong môi trường thực tế.
- Cung cấp thông tin để đánh giá chất lượng và rủi ro của sản phẩm.

---

# Những nguyên nhân thường gây lỗi phần mềm

Lỗi phần mềm có thể xuất hiện từ nhiều nguồn:

- Yêu cầu không rõ ràng hoặc không đầy đủ.
- Hiểu sai yêu cầu.
- Thiết kế hệ thống chưa phù hợp.
- Sai sót khi lập trình.
- Logic xử lý không chính xác.
- Giao tiếp không tốt giữa các thành viên.
- Thay đổi yêu cầu nhưng không cập nhật đầy đủ.
- Tích hợp không đúng giữa các module.
- Môi trường chạy khác môi trường phát triển.
- Dữ liệu đầu vào bất thường.
- Hệ thống bên ngoài hoặc dịch vụ bên thứ ba thay đổi.
- Áp lực tiến độ làm bỏ sót việc kiểm tra.

Vì phần mềm ngày càng phức tạp, việc chỉ dựa vào lập trình viên đọc lại code thường không đủ để phát hiện toàn bộ vấn đề.

---

# Các phương pháp Software Testing cơ bản

## Manual Testing

**Manual Testing** là kiểm thử thủ công. Tester trực tiếp thao tác với ứng dụng, nhập dữ liệu, quan sát kết quả và so sánh với kết quả mong đợi mà không sử dụng script tự động để thực thi test.

Manual testing phù hợp với:

- Exploratory testing.
- Usability testing.
- Ad-hoc testing.
- Các chức năng mới, thường xuyên thay đổi.
- Những trường hợp cần đánh giá bằng cảm nhận và phán đoán của con người.

Hạn chế:

- Mất thời gian khi phải lặp lại nhiều lần.
- Dễ xảy ra sai sót do con người.
- Khó mở rộng cho số lượng test case lớn.
- Không phù hợp với regression testing liên tục.

## Automation Testing

**Automation Testing** sử dụng công cụ và script để:

- Chạy test tự động.
- So sánh kết quả thực tế với kết quả mong đợi.
- Ghi nhận kết quả.
- Báo cáo lỗi.
- Lặp lại test trên nhiều môi trường hoặc bộ dữ liệu.

Automation testing đặc biệt phù hợp với:

- Regression testing.
- Test lặp lại thường xuyên.
- Kiểm thử nhiều bộ dữ liệu.
- Kiểm thử hiệu năng.
- Pipeline CI/CD.
- Những chức năng ổn định và có kết quả rõ ràng.

Tuy nhiên, tự động hóa không thay thế hoàn toàn manual testing. Việc tạo và bảo trì script tự động cũng tốn thời gian, chi phí và kỹ năng kỹ thuật.

---

# Các mức kiểm thử chính

## Unit Testing

Kiểm tra từng hàm, method, class hoặc module nhỏ riêng biệt.

## Integration Testing

Kiểm tra việc trao đổi và tương tác giữa nhiều unit hoặc module.

## System Testing

Kiểm tra toàn bộ hệ thống đã tích hợp hoàn chỉnh theo yêu cầu chức năng và phi chức năng.

## Acceptance Testing

Kiểm tra sản phẩm có đáp ứng nhu cầu kinh doanh và có thể được người dùng hoặc khách hàng chấp nhận hay không.

---

# Unit Testing là gì?

**Unit Testing** là phương pháp kiểm thử trong đó từng đơn vị hoặc thành phần nhỏ của mã nguồn được kiểm tra riêng biệt để xác nhận rằng nó hoạt động chính xác.

Một unit có thể là:

- Một function.
- Một method.
- Một class.
- Một component.
- Một module nhỏ.

Kích thước chính xác của một unit phụ thuộc vào cách phần mềm được thiết kế. Tuy nhiên, nguyên tắc quan trọng nhất là **isolation**, tức tính cô lập.

Khi kiểm thử một unit, các phụ thuộc bên ngoài như database, API, file system, network, email service hoặc message broker nên được thay thế bằng mock, stub hoặc fake để test chỉ tập trung vào logic của unit đang xét.

Ví dụ:

```python
def add(a, b):
    return a + b


def test_add():
    assert add(2, 3) == 5
```

Test trên xác nhận rằng hàm `add()` trả về đúng kết quả khi nhận đầu vào `2` và `3`.

---

# Vì sao cần Unit Testing?

Một quan niệm sai phổ biến là bỏ qua unit test sẽ giúp tiết kiệm thời gian. Trên thực tế, lỗi đơn giản không được phát hiện ở mức unit có thể trở nên khó truy tìm hơn nhiều khi các module đã được tích hợp.

Unit testing mang lại các lợi ích sau:

## Phát hiện lỗi sớm

Lỗi được tìm thấy gần vị trí mà nó được tạo ra, nhờ đó dễ xác định nguyên nhân và sửa nhanh hơn.

## Cải thiện chất lượng code

Code dễ unit test thường có:

- Trách nhiệm rõ ràng.
- Ít phụ thuộc ẩn.
- Module nhỏ hơn.
- Cấu trúc dễ bảo trì hơn.
- Mức độ liên kết giữa các thành phần thấp hơn.

## Ngăn lỗi hồi quy

Khi refactor hoặc bổ sung tính năng, unit test giúp xác nhận những hành vi cũ vẫn hoạt động.

## Tăng tốc độ phát triển

Unit test tự động cung cấp phản hồi nhanh, giảm thời gian kiểm thử thủ công và debugging về sau.

## Tăng sự tự tin khi thay đổi code

Developer có thể chỉnh sửa hoặc refactor code với mức rủi ro thấp hơn vì bộ test đóng vai trò như một safety net.

## Cho phép kiểm thử sớm từng phần

Một module có thể được kiểm tra mà không cần đợi toàn bộ hệ thống hoàn thành.

## Đóng vai trò như tài liệu

Unit test thể hiện:

- Unit nhận đầu vào gì.
- Hoạt động như thế nào.
- Trả về kết quả gì.
- Hành vi mong đợi trong từng trường hợp.

Developer mới có thể đọc unit test để hiểu cách sử dụng một API hoặc component.

---

# Quy trình thực hiện Unit Testing

## Bước 1: Phân tích unit và xác định test case

Xác định hành vi nhỏ nhất cần kiểm tra và liệt kê:

- Happy path: trường hợp hoạt động bình thường.
- Edge case: trường hợp tại ranh giới.
- Error case: trường hợp lỗi.
- Input và output.
- Precondition.
- Postcondition.

Ví dụ, với hàm chia hai số, cần kiểm tra:

- Chia hai số dương.
- Chia số âm.
- Chia cho số thập phân.
- Chia cho 0.
- Đầu vào không hợp lệ.

## Bước 2: Thiết lập môi trường test

Chọn framework kiểm thử phù hợp và chuẩn bị dữ liệu tối thiểu.

Các dependency nên được cô lập bằng:

- Mock.
- Stub.
- Fake.
- Fixture.

Môi trường cần nhẹ để test chạy nhanh và ít bị lỗi không ổn định.

## Bước 3: Viết test theo AAA Pattern

AAA gồm:

### Arrange

Chuẩn bị object, input, test data, dependency và điều kiện ban đầu.

### Act

Gọi function hoặc method cần kiểm tra.

### Assert

So sánh kết quả thực tế với kết quả mong đợi.

```python
# Arrange
cart = Cart(tax_rate=0.1)

# Act
total = cart.total([Item("book", 100)])

# Assert
assert total == 110
```

Nên kiểm tra hành vi đầu ra thay vì phụ thuộc quá sâu vào cách code được triển khai bên trong.

## Bước 4: Chạy test ở local và trong CI

Trước tiên chạy test trên máy phát triển. Sau đó chạy trong môi trường CI để bảo đảm code cũng hoạt động trong một môi trường sạch và nhất quán.

Khi test thất bại, log phải đủ rõ để developer nhanh chóng xác định nguyên nhân.

## Bước 5: Phân tích lỗi, sửa và refactor

Khi test fail, cần xác định:

- Production code sai.
- Test case sai.
- Requirement đã thay đổi.
- Dữ liệu test không đúng.
- Dependency mock chưa phù hợp.

Nên tránh sửa đồng thời cả test và production code mà chưa xác định rõ phía nào sai.

Sau khi test pass, developer có thể refactor mà vẫn có bộ test bảo vệ hành vi hiện tại.

## Bước 6: Chạy lại và bảo trì test

Sau khi sửa:

- Chạy lại test vừa fail.
- Chạy toàn bộ test suite.
- Loại bỏ test bị trùng.
- Sửa flaky test.
- Tối ưu fixture.
- Kiểm tra coverage.
- Phân loại test chậm để chạy với tần suất phù hợp.

Unit test nên nhanh, độc lập và được đặt tên theo hành vi cần kiểm tra. Flaky test phải được coi là một lỗi cần xử lý, không nên đơn giản bỏ qua.

---

# Các kỹ thuật Unit Testing

## Black-box Testing

Kiểm tra dựa trên:

- Input.
- Output.
- Yêu cầu chức năng.
- Hành vi quan sát được.

Người viết test không nhất thiết phải quan tâm code bên trong được triển khai như thế nào.

## White-box Testing

Kiểm tra cấu trúc và logic bên trong của chương trình, ví dụ:

- Branch.
- Condition.
- Loop.
- Execution path.
- Internal logic.

## Gray-box Testing

Kết hợp black-box và white-box. Người kiểm thử có một phần kiến thức về cấu trúc bên trong nhưng vẫn kiểm tra chủ yếu từ góc nhìn hành vi.

---

# Code Coverage trong Unit Testing

Các loại coverage thường gặp:

- **Statement Coverage:** bao nhiêu câu lệnh đã được chạy.
- **Decision Coverage:** bao nhiêu quyết định logic đã được kiểm tra.
- **Branch Coverage:** bao nhiêu nhánh `true` và `false` đã được chạy.
- **Condition Coverage:** bao nhiêu điều kiện con đã nhận cả giá trị đúng và sai.
- **Finite State Machine Coverage:** bao nhiêu trạng thái và chuyển đổi trạng thái đã được kiểm tra.

Tuy nhiên, coverage chỉ là một chỉ báo để tìm phần code chưa được kiểm tra. Coverage cao không tự động có nghĩa là test tốt.

Một test có thể chạy qua một dòng code nhưng không assert kết quả chính xác. Vì vậy, không nên chạy theo mục tiêu 100% coverage bằng mọi giá.

Nên ưu tiên:

- Business logic quan trọng.
- Component tái sử dụng.
- Code có rủi ro cao.
- Logic phức tạp.
- Các khu vực thường xuyên thay đổi.

---

# Mock, Stub và Fake trong Unit Testing

## Vì sao cần Test Double?

Test double thay thế dependency thật để đạt được:

- **Isolation:** chỉ kiểm tra unit cần xét.
- **Determinism:** kết quả ổn định và có thể dự đoán.
- **Speed:** không phải gọi database hoặc network thật.
- **Edge-case simulation:** dễ mô phỏng timeout, exception hoặc dữ liệu bất thường.

## Stub

Stub là một đối tượng thay thế đơn giản, trả về dữ liệu đã được định trước.

Ví dụ:

```python
monkeypatch.setattr(
    "app.get_user_from_db",
    lambda _: {"id": 1, "name": "Alice"}
)
```

Stub chủ yếu cung cấp dữ liệu cho unit. Nó thường không kiểm tra dependency đã được gọi như thế nào.

Dùng stub khi chỉ cần cung cấp dữ liệu hoặc kết quả giả.

## Mock

Mock không chỉ trả dữ liệu mà còn có thể ghi nhận và xác minh interaction.

Có thể kiểm tra:

- Hàm gửi email có được gọi hay không.
- Được gọi bao nhiêu lần.
- Được gọi với tham số gì.
- Các lời gọi diễn ra theo thứ tự nào.

Dùng mock khi cần xác minh sự tương tác giữa unit và dependency.

## Fake

Fake là phiên bản hoạt động đơn giản hơn của dependency thật.

Ví dụ:

- In-memory database.
- Fake repository.
- Local file store.
- Fake message queue.

Fake có hành vi thực nhưng không có đầy đủ độ phức tạp của hệ thống thật.

Nguyên tắc chung:

```text
Cần dữ liệu cố định          → Stub
Cần xác minh interaction    → Mock
Cần implementation đơn giản → Fake
```

## Những vấn đề cần tránh

- Mock mọi dependency làm test quá phụ thuộc vào implementation.
- Kiểm tra mock thay vì hành vi thực sự.
- Setup mock quá dài và khó đọc.
- Test fail chỉ vì code được refactor dù hành vi không thay đổi.
- Dùng mock cho những dependency có thể thay bằng fake đơn giản.

Mock và stub chỉ là công cụ hỗ trợ cho việc cô lập unit; chúng không nên trở thành trọng tâm chính của test.

---

# Các công cụ Unit Testing phổ biến

Một số framework phổ biến:

- **JUnit:** framework unit testing cho Java.
- **NUnit:** framework unit testing mã nguồn mở cho .NET.
- **PHPUnit:** framework unit testing cho PHP.
- **pytest** hoặc **unittest:** dành cho Python.
- **Jest**, **Vitest** hoặc **Mocha:** dành cho JavaScript.
- **xUnit** hoặc **MSTest:** dành cho .NET.
- **Go testing package:** dành cho Go.
- **GoogleTest** hoặc **Catch2:** dành cho C++.

Các framework thường cung cấp:

- Assertion.
- Test runner.
- Setup và teardown.
- Fixture.
- Parameterized testing.
- Mocking support.
- Test report.
- Coverage integration.

---

# TDD và Unit Testing

**Test-Driven Development**, hay TDD, là phương pháp trong đó test được viết trước production code.

Chu trình phổ biến:

```text
Red → Green → Refactor
```

## Red

Viết một test cho hành vi mới. Test sẽ fail vì chức năng chưa được triển khai.

## Green

Viết lượng code tối thiểu để làm test pass.

## Refactor

Cải thiện cấu trúc code mà không làm thay đổi hành vi. Bộ test giúp xác nhận refactor không phá vỡ chức năng.

TDD khuyến khích:

- Unit nhỏ và dễ test.
- Thiết kế đơn giản.
- Phát triển từng bước.
- Tránh xây dựng chức năng chưa cần thiết.
- Có bộ regression test phát triển cùng codebase.

Unit testing không đồng nghĩa với TDD. Có thể viết unit test sau khi viết code. Tuy nhiên, unit-testing framework là nền tảng cần thiết để áp dụng TDD.

---

# Unit Testing trong CI/CD

Khi tích hợp unit test vào CI/CD pipeline, test trở thành một quality gate tự động cho mỗi lần thay đổi code.

Lợi ích:

- Developer nhận phản hồi gần như ngay sau khi commit.
- Bug được phát hiện trước khi merge hoặc release.
- Build chỉ được coi là đạt khi test pass.
- Giảm xung đột khi nhiều developer làm việc cùng lúc.
- Tăng độ tin cậy khi triển khai.
- Hỗ trợ shift-left testing, tức đưa hoạt động kiểm thử về sớm hơn trong vòng đời phát triển.

Pipeline có thể thực hiện:

```text
Checkout code
    ↓
Install dependencies
    ↓
Build
    ↓
Run unit tests
    ↓
Check coverage
    ↓
Run integration tests
    ↓
Package / Deploy
```

---

# Ưu điểm của Unit Testing

- Phát hiện lỗi sớm.
- Giảm chi phí sửa lỗi.
- Cho phép test từng phần độc lập.
- Hỗ trợ refactor.
- Ngăn regression.
- Tạo tài liệu sống cho code.
- Cải thiện thiết kế phần mềm.
- Tăng tốc phản hồi.
- Tăng sự tự tin khi release.
- Có thể tự động hóa và tích hợp vào CI/CD.

---

# Hạn chế của Unit Testing

Unit testing không thể phát hiện mọi lỗi.

Nó không phù hợp để phát hiện đầy đủ:

- Lỗi tương tác giữa các module.
- Lỗi kết nối database thật.
- Lỗi cấu hình môi trường.
- Lỗi giao tiếp với API thật.
- Lỗi end-to-end.
- Lỗi giao diện người dùng.
- Vấn đề hiệu năng toàn hệ thống.
- Lỗi nghiệp vụ xuyên qua nhiều component.

Ngoài ra:

- Không thể kiểm tra mọi execution path trong hệ thống phức tạp.
- Viết và bảo trì test cũng tốn công sức.
- Test thiết kế kém có thể trở nên brittle.
- Việc mock quá nhiều có thể tạo cảm giác an toàn giả.
- Coverage cao không bảo đảm sản phẩm không có lỗi.

Do đó, unit testing phải được kết hợp với integration testing, system testing và các hình thức kiểm thử khác.

---

# Best Practices cho Unit Testing

- Mỗi test phải độc lập.
- Không để test phụ thuộc vào thứ tự chạy.
- Mỗi test chỉ nên kiểm tra một hành vi chính.
- Đặt tên test rõ ràng theo hành vi.
- Chuẩn bị dữ liệu test tối thiểu.
- Tránh gọi network hoặc database thật.
- Test cả happy path, edge case và error case.
- Bug phát hiện ở unit test phải được sửa trước khi sang giai đoạn tiếp theo.
- Khi sửa bug, nên thêm test để ngăn lỗi tái diễn.
- Viết test song song với quá trình viết code.
- Giữ test nhanh.
- Không bỏ qua flaky test.
- Không over-mock.
- Không phụ thuộc quá sâu vào implementation details.
- Không chạy theo 100% coverage nếu test không mang lại giá trị.
- Ưu tiên logic nghiệp vụ quan trọng và khu vực có rủi ro cao.

---

# Integration Testing là gì?

**Integration Testing**, hay kiểm thử tích hợp, là mức kiểm thử trong đó hai hoặc nhiều unit hoặc module đã được unit test được kết hợp và kiểm tra như một nhóm.

Mục đích chính không phải kiểm tra lại logic nội bộ của từng module, mà là kiểm tra:

- Interface giữa các module.
- Dữ liệu truyền từ module này sang module khác.
- Thứ tự gọi.
- Contract giữa các component.
- Giao tiếp với database.
- Giao tiếp với API.
- Xử lý exception xuyên module.
- Sự phối hợp giữa nhiều subsystem.

Ví dụ, một hệ thống thương mại điện tử có:

- Module đăng nhập.
- Module giỏ hàng.
- Module đơn hàng.
- Module thanh toán.
- Module tồn kho.
- Module gửi email.

Mỗi module có thể pass unit test nhưng toàn bộ quy trình vẫn có thể lỗi nếu:

- Module đơn hàng truyền sai số tiền sang module thanh toán.
- Thanh toán thành công nhưng tồn kho không được cập nhật.
- Đơn hàng lưu thành công nhưng email không được gửi.
- Hai module sử dụng định dạng ngày hoặc tiền tệ khác nhau.

Integration testing được thực hiện để phát hiện những lỗi như vậy.

---

# Vì sao cần Integration Testing?

Mặc dù từng module đã được unit test, lỗi vẫn có thể xuất hiện khi chúng kết nối với nhau vì:

- Mỗi module có thể do developer khác nhau xây dựng.
- Các module có thể hiểu khác nhau về interface.
- Format dữ liệu không thống nhất.
- Kiểu dữ liệu không tương thích.
- Thứ tự gọi không đúng.
- API contract thay đổi.
- Exception không được truyền hoặc xử lý đúng.
- Module phụ thuộc vào thời gian, trạng thái hoặc transaction.
- Hệ thống bên ngoài phản hồi khác dự kiến.
- Database schema không khớp với code.
- Các module hoạt động đúng riêng lẻ nhưng sai khi phối hợp.

Integration testing giúp phát hiện các lỗi nằm ở phần kết nối giữa những thành phần đã hoạt động riêng lẻ.

---

# Ví dụ về Integration Testing

Giả sử có ba module:

```text
Login → Account → Transfer
```

Các unit test có thể xác nhận:

- `LoginService` xác minh mật khẩu đúng.
- `AccountService` lấy đúng số dư.
- `TransferService` tính giao dịch đúng.

Integration test cần kiểm tra toàn bộ tương tác:

1. Người dùng đăng nhập.
2. Token được tạo.
3. Token được truyền sang Account Service.
4. Account Service trả về đúng tài khoản.
5. Transfer Service kiểm tra số dư.
6. Tiền được trừ khỏi tài khoản nguồn.
7. Tiền được cộng vào tài khoản đích.
8. Giao dịch được lưu.
9. Trạng thái được trả về cho người dùng.

Lỗi có thể không nằm trong một unit riêng biệt mà nằm ở việc các unit sử dụng dữ liệu của nhau.

---

# Các chiến lược Integration Testing

Bốn phương pháp chính gồm:

1. Big Bang.
2. Top-Down.
3. Bottom-Up.
4. Sandwich hoặc Hybrid.

## Big Bang Integration Testing

Trong Big Bang testing, tất cả hoặc phần lớn module được tích hợp cùng lúc rồi toàn bộ tổ hợp được kiểm tra như một đơn vị.

```text
Module A ┐
Module B ├── Tích hợp đồng thời → Test toàn hệ thống tích hợp
Module C ┤
Module D ┘
```

### Ưu điểm

- Cách tiếp cận đơn giản.
- Không cần lập kế hoạch tích hợp theo nhiều giai đoạn.
- Phù hợp với hệ thống nhỏ.
- Có thể thực hiện khi tất cả module đã hoàn thành.

### Nhược điểm

- Khó xác định module gây ra lỗi.
- Lỗi chỉ được phát hiện khá muộn.
- Phải chờ tất cả module hoàn thành.
- Interface quan trọng có thể không được kiểm tra đầy đủ.
- Việc debugging trở nên phức tạp khi nhiều module cùng tham gia.
- Không phù hợp với hệ thống lớn và có nhiều dependency.

Big Bang phù hợp hơn với các hệ thống nhỏ, ít module và quan hệ giữa các module không quá phức tạp.

## Incremental Integration Testing

Trong incremental testing, các module được tích hợp và kiểm tra từng bước.

```text
A + B → Test
A + B + C → Test
A + B + C + D → Test
```

Ưu điểm:

- Lỗi được phát hiện sớm hơn.
- Dễ xác định vùng gây lỗi.
- Interface được kiểm tra có hệ thống.
- Không cần chờ toàn bộ hệ thống hoàn thành.
- Dễ quản lý hơn Big Bang.

Top-down, bottom-up và sandwich đều là các hình thức incremental integration.

## Top-Down Integration Testing

Trong top-down testing, việc tích hợp bắt đầu từ module cấp cao rồi dần bổ sung các module cấp thấp.

```text
        Module A
        /      \
   Module B   Module C
      /           \
 Module D        Module E
```

Trình tự có thể là:

```text
A
A + B
A + B + C
A + B + C + D
A + B + C + D + E
```

Nếu module cấp thấp chưa hoàn thành, tester sử dụng **stub** để thay thế.

### Stub trong Top-Down

Stub mô phỏng hành vi của module cấp dưới.

Ví dụ, module `OrderService` gọi `PaymentService`, nhưng `PaymentService` chưa hoàn thành. Một stub có thể luôn trả về:

```json
{
  "status": "success"
}
```

### Ưu điểm

- Kiểm tra sớm luồng điều khiển và logic cấp cao.
- Kiểm tra sớm kiến trúc chính.
- Phát hiện sớm lỗi thiết kế ở các module quan trọng.
- Có thể tạo bản prototype hoạt động tương đối sớm.
- Không cần driver.

### Nhược điểm

- Phải xây dựng nhiều stub.
- Module cấp thấp được kiểm tra khá muộn.
- Stub có thể không mô phỏng chính xác hành vi thật.
- Các chức năng xử lý dữ liệu chi tiết có thể chưa được kiểm tra đầy đủ ở giai đoạn đầu.

Top-down phù hợp khi:

- Luồng điều khiển cấp cao quan trọng.
- Module cấp trên hoàn thành trước.
- Cần kiểm tra sớm kiến trúc hoặc workflow chính.

## Bottom-Up Integration Testing

Trong bottom-up testing, việc tích hợp bắt đầu từ các module cấp thấp rồi dần kết nối lên module cấp cao.

```text
Module D + Module E
        ↓
     Module B
        ↓
     Module A
```

Nếu module cấp cao chưa hoàn thành, tester sử dụng **driver** để gọi các module cấp dưới.

### Driver trong Bottom-Up

Driver là chương trình tạm thời đóng vai trò module cấp trên.

Nó có thể:

- Gửi input xuống module.
- Gọi function.
- Nhận output.
- So sánh kết quả.
- Mô phỏng luồng điều khiển từ tầng trên.

### Ưu điểm

- Kiểm tra sớm các utility và service nền tảng.
- Không cần stub.
- Dễ phát hiện lỗi trong module cấp thấp.
- Phù hợp khi module nền tảng hoàn thành trước.
- Có thể kiểm thử chi tiết việc xử lý dữ liệu sớm.

### Nhược điểm

- Phải xây dựng driver.
- Luồng nghiệp vụ cấp cao được kiểm tra muộn.
- Chưa có bản hệ thống hoàn chỉnh ở giai đoạn đầu.
- Các lỗi kiến trúc tổng thể có thể được phát hiện trễ.

Bottom-up phù hợp khi:

- Module tầng dưới đã sẵn sàng.
- Logic xử lý dữ liệu nền tảng rất quan trọng.
- Hệ thống có nhiều service dùng chung.
- Thành phần cấp cao vẫn đang được phát triển.

## Sandwich hoặc Hybrid Integration Testing

Sandwich testing kết hợp top-down và bottom-up.

Việc kiểm thử được thực hiện đồng thời từ:

- Các module cấp cao đi xuống.
- Các module cấp thấp đi lên.

Hai hướng gặp nhau tại lớp trung gian.

```text
Top modules
     ↓
Middle layer
     ↑
Bottom modules
```

Phương pháp này sử dụng cả stub và driver.

### Ưu điểm

- Tận dụng ưu điểm của top-down và bottom-up.
- Có thể kiểm thử song song.
- Phù hợp với hệ thống lớn có nhiều tầng.
- Module cấp cao và cấp thấp đều được kiểm tra sớm.
- Có thể giảm tổng thời gian tích hợp.

### Nhược điểm

- Lập kế hoạch phức tạp.
- Cần nhiều nguồn lực.
- Phải quản lý cả stub và driver.
- Lớp trung gian có thể được kiểm tra muộn.
- Không phù hợp với hệ thống nhỏ, đơn giản.

---

# Quy trình thực hiện Integration Testing

## Bước 1: Chuẩn bị Integration Test Plan

Xác định:

- Module nào sẽ được tích hợp.
- Thứ tự tích hợp.
- Interface cần kiểm tra.
- Phạm vi test.
- Môi trường.
- Dependency.
- Công cụ.
- Entry criteria.
- Exit criteria.

## Bước 2: Xác định các interface

Liệt kê các điểm giao tiếp:

- Function calls.
- API endpoints.
- Database.
- Message queue.
- File exchange.
- Shared memory.
- Event.
- Authentication.
- Third-party service.

## Bước 3: Ưu tiên module quan trọng

Ưu tiên kiểm tra:

- Module có rủi ro cao.
- Module trung tâm.
- Interface được nhiều component sử dụng.
- Payment.
- Authentication.
- Transaction.
- Data synchronization.

## Bước 4: Thiết kế test case

Test case cần kiểm tra:

- Dữ liệu hợp lệ.
- Dữ liệu không hợp lệ.
- Giá trị ranh giới.
- Missing field.
- Timeout.
- Duplicate request.
- Exception.
- Transaction rollback.
- Service unavailable.
- Sai format.
- Sai phiên bản API.
- Mất kết nối.
- Retry behavior.

## Bước 5: Chuẩn bị test data và môi trường

Môi trường integration test thường gần môi trường thật hơn unit test và có thể bao gồm:

- Test database.
- Test API.
- Container.
- Local service.
- Message broker.
- Sandbox của bên thứ ba.
- Test credential.
- Seed data.

## Bước 6: Thực thi test

Chạy test theo chiến lược đã chọn:

- Big Bang.
- Top-Down.
- Bottom-Up.
- Sandwich.

## Bước 7: Ghi nhận và sửa defect

Khi phát hiện lỗi cần xác định:

- Module gửi sai.
- Module nhận hiểu sai.
- Contract không nhất quán.
- Lỗi cấu hình.
- Dữ liệu test sai.
- Hệ thống bên ngoài không ổn định.

## Bước 8: Retest và Regression Test

Sau khi sửa lỗi:

- Chạy lại test case đã fail.
- Chạy regression test cho các integration liên quan.
- Xác nhận thay đổi không làm hỏng interface khác.

Quy trình này giúp bảo đảm các module không chỉ hoạt động riêng lẻ mà còn phối hợp đúng khi kết nối.

---

# Test case quan trọng trong Integration Testing

## Data contract

- Đúng field.
- Đúng kiểu dữ liệu.
- Đúng format.
- Đúng đơn vị.
- Đúng timezone.
- Đúng encoding.
- Đúng version.

## Error handling

- Service trả lỗi.
- Database không khả dụng.
- Network timeout.
- Message không hợp lệ.
- Token hết hạn.
- Request bị từ chối.
- Một bước trong transaction thất bại.

## State và transaction

- Dữ liệu được commit đúng.
- Rollback đúng khi thất bại.
- Không tạo bản ghi trùng.
- Retry không gây double processing.
- Trạng thái giữa nhiều module nhất quán.

## Performance ở interface

- Thời gian phản hồi.
- Số lượng request.
- Batch size.
- Connection pool.
- Queue backlog.

## Security

- Authentication.
- Authorization.
- Token propagation.
- Quyền truy cập database.
- Dữ liệu nhạy cảm.
- Input validation.

---

# Ưu điểm của Integration Testing

- Phát hiện lỗi giao tiếp giữa module.
- Xác minh các component hoạt động cùng nhau.
- Kiểm tra luồng dữ liệu thực tế.
- Kiểm tra contract và interface.
- Tăng độ tin cậy của hệ thống.
- Phát hiện vấn đề unit test không thể phát hiện.
- Xác minh database, API và dịch vụ ngoài.
- Giảm rủi ro trước system testing.
- Phù hợp để kiểm tra workflow xuyên nhiều component.

---

# Hạn chế của Integration Testing

- Chậm hơn unit testing.
- Khó thiết lập môi trường.
- Dependency bên ngoài có thể làm test không ổn định.
- Khó xác định nguyên nhân lỗi hơn unit test.
- Test data phức tạp.
- Có thể cần stub hoặc driver.
- Chi phí bảo trì cao hơn.
- Có thể bị ảnh hưởng bởi network, database hoặc cấu hình.
- Không thay thế system testing hay acceptance testing.

---

# So sánh Unit Testing và Integration Testing

| Tiêu chí          | Unit Testing                  | Integration Testing                       |
|:------------------|:------------------------------|:------------------------------------------|
| Phạm vi           | Một unit riêng lẻ             | Nhiều unit hoặc module kết nối            |
| Mục tiêu          | Kiểm tra logic nội bộ         | Kiểm tra sự tương tác                     |
| Dependency        | Thường được mock hoặc stub    | Thường dùng dependency thật hoặc gần thật |
| Tốc độ            | Rất nhanh                     | Chậm hơn                                  |
| Người thực hiện   | Chủ yếu developer             | Developer hoặc tester                     |
| Lỗi phát hiện     | Logic, condition, calculation | Interface, contract, data flow            |
| Môi trường        | Đơn giản                      | Phức tạp hơn                              |
| Debug             | Dễ                            | Khó hơn                                   |
| Số lượng          | Thường rất nhiều              | Ít hơn unit test                          |
| Độ cô lập         | Cao                           | Thấp hơn                                  |
| Ví dụ             | Test hàm tính tổng            | Test Order Service gọi Payment Service    |
| Vị trí trong SDLC | Trước                         | Sau unit testing                          |

---

# Quan hệ giữa ba nội dung

Software testing là khái niệm tổng thể, còn unit testing và integration testing là hai mức kiểm thử cụ thể.

```text
SOFTWARE TESTING
│
├── Unit Testing
│   ├── Kiểm tra function
│   ├── Kiểm tra method
│   ├── Kiểm tra class
│   └── Cô lập dependency
│
├── Integration Testing
│   ├── Kiểm tra interface
│   ├── Kiểm tra data flow
│   ├── Kiểm tra API hoặc database
│   └── Kiểm tra interaction
│
├── System Testing
│   └── Kiểm tra toàn bộ hệ thống
│
└── Acceptance Testing
    └── Kiểm tra yêu cầu người dùng và doanh nghiệp
```

Hai mức đầu bổ sung cho nhau:

- Unit test pass không có nghĩa là các module tích hợp đúng.
- Integration test pass không có nghĩa là logic bên trong từng unit đã được kiểm tra đầy đủ.
- Unit test giúp tìm lỗi nhanh và chính xác.
- Integration test giúp tìm lỗi tại ranh giới giữa các component.
- Cả hai đều cần thiết trước khi kiểm tra toàn bộ hệ thống.

---

# Ví dụ tổng hợp

Giả sử xây dựng chức năng đặt hàng:

```text
OrderController
      ↓
OrderService
      ↓
InventoryService
      ↓
PaymentService
      ↓
OrderRepository
      ↓
NotificationService
```

## Unit Testing

Test riêng từng thành phần:

- `OrderService` tính tổng tiền đúng.
- `InventoryService` kiểm tra tồn kho đúng.
- `PaymentService` từ chối số tiền không hợp lệ.
- `OrderRepository` chuyển đổi object đúng.
- `NotificationService` tạo nội dung email đúng.

Các dependency có thể được mock.

## Integration Testing

Test sự phối hợp:

- Order Service đọc tồn kho đúng.
- Payment Service nhận đúng số tiền.
- Thanh toán thành công thì đơn hàng được lưu.
- Thanh toán thất bại thì đơn hàng không được xác nhận.
- Database rollback khi cập nhật tồn kho thất bại.
- Email chỉ được gửi sau khi order thành công.
- Retry không tạo hai đơn hàng.
- Authentication token được truyền đúng giữa các service.

## System Testing

Test toàn bộ hệ thống qua giao diện:

1. Người dùng đăng nhập.
2. Chọn sản phẩm.
3. Thêm vào giỏ.
4. Thanh toán.
5. Nhận xác nhận.
6. Kiểm tra lịch sử đơn hàng.

---

# Kết luận

**Software Testing** là hoạt động đánh giá và xác minh phần mềm nhằm phát hiện lỗi, giảm rủi ro và bảo đảm sản phẩm đáp ứng yêu cầu.

**Unit Testing** kiểm tra từng đơn vị mã nguồn trong trạng thái cô lập. Nó giúp phát hiện lỗi sớm, hỗ trợ refactor, chống regression và cung cấp phản hồi nhanh. Unit test nên độc lập, nhanh, tập trung vào hành vi và được tích hợp vào CI/CD.

**Integration Testing** kiểm tra các unit hoặc module sau khi chúng được kết nối. Nó tập trung vào interface, data flow, contract và interaction giữa các thành phần. Các phương pháp chính gồm Big Bang, Top-Down, Bottom-Up và Sandwich.

Một chiến lược kiểm thử tốt không chọn một trong hai mà kết hợp cả hai:

```text
Nhiều Unit Tests
      +
Một lượng Integration Tests hợp lý
      +
Ít System hoặc End-to-End Tests quan trọng
```

Unit test cho biết **từng bộ phận có hoạt động đúng không**, còn integration test cho biết **các bộ phận có làm việc đúng với nhau không**.

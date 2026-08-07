---
title: "CF 103965A - Cân bằng tâm trạng"
description: "Chúng ta được cho một dãy số nguyên. Hãy coi chúng như những “sự thay đổi” hàng ngày đối với một số giá trị đang chạy bắt đầu từ con số 0. Khi chúng ta đi từ trái sang phải, chúng ta duy trì tổng chạy. Mỗi phần tử có thể tăng hoặc giảm tổng số tiền đang chạy này."
date: "2026-07-02T06:34:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103965
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2022-2023, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 103965
solve_time_s: 37
verified: true
draft: false
---

[CF 103965A - Cân bằng tâm trạng](https://codeforces.com/problemset/problem/103965/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên. Hãy coi chúng như những “sự thay đổi” hàng ngày đối với một số giá trị đang chạy bắt đầu từ con số 0. Khi chúng ta đi từ trái sang phải, chúng ta duy trì tổng chạy. Mỗi phần tử có thể tăng hoặc giảm tổng số tiền đang chạy này. 

Nhiệm vụ là xác định giá trị cao nhất mà tổng chạy này từng đạt được trong khi xử lý mảng từ trái sang phải. 

Vì vậy, nếu mảng được hiểu là mức tăng và giảm của một điểm, chúng ta sẽ được hỏi: điểm tối đa đạt được ở bất kỳ ranh giới tiền tố nào là bao nhiêu. 

Đầu ra là một số nguyên duy nhất: tổng tiền tố tối đa trên toàn bộ mảng. 

Các ràng buộc trong bài toán này không đủ lớn để yêu cầu bất cứ điều gì ngoài một lần quét tuyến tính. Điều đó đã loại trừ bất kỳ giải pháp nào cố gắng tính toán lại tổng tiền tố nhiều lần hoặc sử dụng các vòng lặp lồng nhau, vì những giải pháp đó sẽ chuyển thành hành vi bậc hai và không cần thiết. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị đều âm. Trong trường hợp đó, tổng chạy bắt đầu bằng 0 và không bao giờ cải thiện, vì vậy câu trả lời sẽ vẫn là 0. Bất kỳ giải pháp nào giả định ít nhất một lần tăng tiền tố dương sẽ thất bại ở đây. Ví dụ, đầu vào`[-3, -2, -5]`phải sản xuất`0`, không`-3`. 

Một trường hợp cạnh khác là khi tổng tiền tố tối đa xảy ra ở phần tử đầu tiên. Ví dụ`[5, -10, 1]`nên quay lại`5`. Việc triển khai đơn giản chỉ kiểm tra sau khi tích lũy đầy đủ hoặc quên xem xét các tiền tố trung gian có thể bỏ sót điều này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tính tổng của từng tiền tố một cách riêng biệt và lấy mức tối đa. Với mỗi chỉ số i, chúng ta tính tổng từ 0 đến i. Điều này hoạt động chính xác vì nó tuân theo định nghĩa về tổng tiền tố. Tuy nhiên, việc tính lại tổng cho mỗi tiền tố sẽ dẫn đến tổng số thao tác khoảng n + (n-1) + ... + 1, tức là O(n²). Với các ràng buộc thông thường lên tới 10^5, điều này trở nên quá chậm. 

Quan sát quan trọng là tổng tiền tố tăng dần. Thay vì tính toán lại từ đầu, chúng ta có thể duy trì tổng chạy trong khi quét mảng một lần. Mỗi phần tử mới cập nhật tổng hiện tại và chúng tôi ngay lập tức so sánh nó với giá trị tốt nhất được thấy cho đến nay. Điều này làm giảm vấn đề từ việc tính toán lại lặp đi lặp lại thành tích lũy một lượt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại tiền tố Brute Force | O(n²) | O(1) | Quá chậm | 
| Tổng số lần chạy một lượt | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với hai biến:`current_sum = 0`Và`best = 0`. Giá trị tốt nhất ban đầu là 0 vì chúng ta được phép xem xét trạng thái bắt đầu trước khi áp dụng bất kỳ phần tử nào. 
2. Lặp lại mảng từ trái sang phải. Tại mỗi vị trí, thêm phần tử hiện tại vào`current_sum`. Điều này mô phỏng việc áp dụng “sự thay đổi tâm trạng” hoặc “sự gia tăng” tiếp theo. 
3. Sau khi cập nhật`current_sum`, so sánh nó với`best`. Nếu như`current_sum`lớn hơn, thay thế`best`với nó. Bước này đảm bảo chúng ta luôn nhớ giá trị tiền tố cao nhất gặp phải cho đến nay. 
4. Tiếp tục cho đến hết mảng. Giá trị cuối cùng của`best`là câu trả lời. 

### Tại sao nó hoạt động 

Tổng chạy ở vị trí i chính xác là tổng tiền tố của phần tử i đầu tiên. Mỗi tổng tiền tố có thể được đánh giá chính xác một lần trong quá trình quét. Vì mức tối đa của một tập hợp được giữ nguyên trong so sánh tăng dần, nên việc theo dõi mức tối đa được thấy cho đến nay đảm bảo rằng không có tiền tố ứng cử viên nào bị bỏ sót. Không có sự phụ thuộc giữa các tiền tố không liền kề ngoài tổng tích lũy, do đó không cần cấu trúc bổ sung. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

current_sum = 0
best = 0

for x in a:
    current_sum += x
    if current_sum > best:
        best = current_sum

print(best)
```Việc thực hiện là bản dịch trực tiếp của quá trình quét được mô tả ở trên. Quyết định tinh tế duy nhất là khởi tạo`best`về 0 thay vì âm vô cực. Điều này phản ánh rằng tổng hiện có bắt đầu trước khi bất kỳ phần tử nào được lấy, do đó tiền tố trống là hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[1, -2, 3]`| Bước | Yếu tố | Tổng hiện tại | Tốt nhất | 
| --- | --- | --- | --- | 
| 0 | bắt đầu | 0 | 0 | 
| 1 | 1 | 1 | 1 | 
| 2 | -2 | -1 | 1 | 
| 3 | 3 | 2 | 2 | 

Điều này cho thấy mức tối đa có thể xảy ra như thế nào sau một đợt phục hồi tích cực sau đó, không nhất thiết phải ở thời điểm đầu. 

### Ví dụ 2 

đầu vào:`[-1, -2, -3]`| Bước | Yếu tố | Tổng hiện tại | Tốt nhất | 
| --- | --- | --- | --- | 
| 0 | bắt đầu | 0 | 0 | 
| 1 | -1 | -1 | 0 | 
| 2 | -2 | -3 | 0 | 
| 3 | -3 | -6 | 0 | 

Điều này xác nhận rằng thuật toán bảo toàn chính xác số 0 khi tất cả các tiền tố giảm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần với các bản cập nhật liên tục | 
| Không gian | O(1) | Chỉ có hai biến đang chạy được duy trì | 

Quét tuyến tính là tối ưu vì mọi phần tử phải được đọc ít nhất một lần và không cần cấu trúc bổ sung. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    current_sum = 0
    best = 0
    for x in a:
        current_sum += x
        best = max(best, current_sum)
    return str(best)

# provided samples (illustrative)
assert run("3\n1 -2 3\n") == "2"

# all negative
assert run("3\n-1 -2 -3\n") == "0"

# all positive
assert run("4\n1 2 3 4\n") == "10"

# alternating
assert run("5\n1 -1 1 -1 1\n") == "1"

# single element
assert run("1\n5\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều tiêu cực | 0 | xử lý tiền tố trống | 
| tất cả đều tích cực | tổng hợp | tích lũy đầy đủ đúng đắn | 
| xen kẽ | 1 | đúng trung gian tối đa | 
| phần tử đơn | giá trị của chính nó | điều kiện biên | 

## Vỏ cạnh 

Đối với đầu vào bao gồm toàn số âm, tổng hiện có sẽ liên tục giảm. Vì thuật toán luôn so sánh với số 0 được khởi tạo tốt nhất nên thuật toán trả về số 0 một cách chính xác. Ví dụ`[-4, -2, -7]`sản xuất`0`bởi vì không có tiền tố nào vượt quá tiền tố trống. 

Đối với mảng một phần tử, bản cập nhật đầu tiên ngay lập tức đặt cả giá trị hiện tại và giá trị tốt nhất thành giá trị đó nếu nó dương hoặc giữ giá trị tốt nhất ở mức 0 nếu âm. Điều này phù hợp với ý tưởng rằng chúng tôi luôn so sánh với trạng thái bắt đầu trước khi xử lý bất kỳ phần tử nào.

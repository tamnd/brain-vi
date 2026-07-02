---
title: "CF 104221C - \u041a\u0443\u0437\u044f \u0438 \u0434\u0435\u043d\u044c \u0440\u043e\u0436\u0434\u0435\u043d\u0438\u044f"
description: "Kuzya đang chuẩn bị một bữa tiệc sinh nhật, nhưng số lượng khách mà anh có thể mời hoàn toàn phụ thuộc vào cách cắt bánh tại một tiệm bánh không xác định. Có một số tiệm bánh trong thành phố của anh ấy và mỗi tiệm bánh luôn cắt bánh thành một số lát cố định."
date: "2026-07-01T23:45:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104221
codeforces_index: "C"
codeforces_contest_name: "\u0424\u0438\u043d\u0430\u043b \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u043e\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b \u00ab\u041c\u0430\u0448\u0438\u043d\u0430 \u0422\u044c\u044e\u0440\u0438\u043d\u0433\u0430\u00bb \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 104221
solve_time_s: 68
verified: true
draft: false
---

[CF 104221C - \u041a\u0443\u0437\u044f \u0438 \u0434\u0435\u043d\u044c \u0440\u043e\u0436\u0434\u0435\u043d\u0438\u044f](https://codeforces.com/problemset/problem/104221/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Kuzya đang chuẩn bị một bữa tiệc sinh nhật, nhưng số lượng khách mà anh có thể mời hoàn toàn phụ thuộc vào cách cắt bánh tại một tiệm bánh không xác định. Có một số tiệm bánh trong thành phố của anh ấy và mỗi tiệm bánh luôn cắt bánh thành một số lát cố định. Kuzya sẽ không biết trước mẹ mình sẽ chọn tiệm bánh nào. 

Anh ấy muốn chọn một số khách để bất kể dùng tiệm bánh nào, chiếc bánh có thể được chia đều cho mọi người trong bữa tiệc, kể cả chính Kuzya. Mỗi người phải nhận được chính xác số lượng lát như nhau và không có gì có thể không được sử dụng. 

Hãy trình bày lại điều này một cách cụ thể hơn, chúng ta được đưa ra một số con số, mỗi con số mô tả một chiếc bánh có thể được cắt thành bao nhiêu phần bằng nhau. Chúng ta phải chọn kích thước nhóm sao cho với mọi kích thước bánh có thể, chiếc bánh đó có thể được phân chia đều trong nhóm. 

Đầu ra là số lượng khách tối đa có thể có trong ràng buộc này. Vì bản thân Kuzya cũng tham dự nên tổng số người nhiều hơn số lượng chúng tôi xuất ra một đơn vị. 

Hạn chế lên tới 100.000 tiệm bánh có kích thước bánh lên tới 10^9 ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các kích thước nhóm có thể và kiểm tra chúng riêng lẻ một cách ngây thơ. Việc kiểm tra tính chia hết trực tiếp cho mỗi ứng cử viên sẽ dẫn đến khoảng 10^14 thao tác trong trường hợp xấu nhất, vượt xa giới hạn khả thi. 

Một trường hợp thất bại tinh vi phổ biến xuất hiện khi các con số không có cấu trúc chung có ý nghĩa. Ví dụ: nếu tiệm bánh sản xuất 2, 3 và 5 lát bánh, không có cách nào để chọn quy mô nhóm lớn hơn 0 khách, vì không có số nguyên nào lớn hơn 1 chia hết cho tất cả. Một trường hợp phức tạp khác là khi tất cả các giá trị đều giống hệt nhau, trong đó câu trả lời chỉ đơn giản là giá trị đó trừ đi một, vì bản thân toàn bộ kích thước chiếc bánh là hệ số giới hạn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử mọi số người có thể có trong nhóm và xác minh xem con số đó có chia hết cho tất cả các kích cỡ bánh hay không. Cụ thể, chúng tôi sẽ lặp lại các kích thước nhóm ứng viên từ 1 đến giá trị tối thiểu trong mảng và đối với mỗi ứng viên, hãy kiểm tra khả năng chia hết cho tất cả các tiệm bánh. Điều này đúng vì nó trực tiếp thực thi điều kiện mỗi chiếc bánh có thể được chia đều cho số người được chọn. 

Tuy nhiên, cách tiếp cận này quá chậm. Trong trường hợp xấu nhất, chúng ta có thể có giá trị lên tới 10^9 và 100.000 tiệm bánh. Ngay cả khi hạn chế ứng viên ở phần tử tối thiểu, chúng tôi vẫn phải đối mặt với tối đa 10^9 lượt kiểm tra và mỗi lượt kiểm tra sẽ quét 100.000 phần tử, dẫn đến khoảng 10^14 thao tác. 

Quan sát quan trọng là điều kiện “quy mô nhóm phù hợp với tất cả các tiệm bánh” có nghĩa là số lượng người phải chia cho từng a_i. Đó chính xác là định nghĩa về ước chung của toàn bộ mảng. Thay vì kiểm tra tất cả các ứng cử viên, chúng ta chỉ cần số nguyên lớn nhất chia hết cho tất cả các phần tử, đó là ước chung lớn nhất. 

Khi chúng tôi tính toán gcd của tất cả các giá trị, mọi kích thước nhóm hợp lệ đều phải chia gcd này và lựa chọn lớn nhất có thể là chính gcd. Vì Kuzya đã tính cả chính mình vào số lượng nên số lượng khách ít hơn quy mô nhóm một người. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · phút(a_i)) | O(1) | Quá chậm | 
| Tối ưu (GCD) | O(n log A) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta rút gọn bài toán thành việc tìm ước chung lớn nhất của mọi kích cỡ bánh. 

1. Bắt đầu với kích thước bánh của tiệm bánh đầu tiên làm giá trị gcd đề xuất ban đầu. 
2. Lặp lại tất cả các giá trị bánh còn lại, cập nhật gcd với mỗi số mới.

Mỗi bản cập nhật sẽ thay thế gcd hiện tại bằng gcd(gcd, a_i), thu nhỏ hoặc giữ nguyên gcd. 
3. Sau khi xử lý tất cả các giá trị, gcd cuối cùng đại diện cho số lượng lát bánh lớn nhất có thể được phân bổ đều trên tất cả các tiệm bánh. 
4. Chuyển đổi số này thành số lượng khách bằng cách trừ đi một, vì gcd đại diện cho tổng số người (khách cộng với Kuzya). 

Lý do chúng ta trừ một là do cấu trúc: nếu chiếc bánh có thể được chia thành d phần bằng nhau thì có chính xác d người trong bữa tiệc, vậy khách mời là d trừ một. 

### Tại sao nó hoạt động 

Ở mỗi bước, gcd đang chạy là số nguyên lớn nhất chia tất cả các phần tử được xử lý cho đến nay. Khi chúng tôi đưa vào một phần tử mới, việc cập nhật bằng gcd sẽ bảo toàn chính xác tập hợp các ước số chung. Bất biến này đảm bảo rằng sau phần tử cuối cùng, kết quả là số lớn nhất chia cho mọi a_i, do đó không thể tồn tại kích thước nhóm hợp lệ lớn hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import gcd

n = int(input())
a = list(map(int, input().split()))

g = a[0]
for x in a[1:]:
    g = gcd(g, x)

print(g - 1)
```Giải pháp đọc tất cả các kích cỡ bánh nướng và duy trì gcd đang chạy. các`math.gcd`hàm tính toán ước số chung lớn nhất một cách hiệu quả theo thời gian logarit trên mỗi lệnh gọi, điều này rất quan trọng với các giới hạn lớn. 

Phép trừ cuối cùng bằng một phản ánh trực tiếp rằng gcd tính tổng số người tham gia, không chỉ khách. 

Một lỗi triển khai phổ biến là quên khởi tạo gcd với phần tử đầu tiên và thay vào đó bắt đầu từ số 0. Trong khi về mặt toán học thì gcd(0, x) = x, điều này an toàn nhưng ít rõ ràng hơn; khởi tạo với`a[0]`tránh nhầm lẫn và khớp với mô tả bất biến rõ ràng hơn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2
5 10
```Chúng tôi tính toán gcd dần dần. 

| Bước | Giá trị hiện tại | Chạy GCD | 
| --- | --- | --- | 
| 1 | 5 | 5 | 
| 2 | 10 | 5 | 

Gcd cuối cùng là 5, nghĩa là bữa tiệc có thể có tổng cộng 5 người. Kuzya là một trong số đó nên số lượng khách là 4. 

Đầu ra:```
4
```Điều này thể hiện trường hợp tồn tại một ước số chung không tầm thường và vẫn ổn định qua các bản cập nhật. 

### Mẫu 2 

đầu vào:```
5
2 7 8 10 5
```| Bước | Giá trị hiện tại | Chạy GCD | 
| --- | --- | --- | 
| 1 | 2 | 2 | 
| 2 | 7 | 1 | 
| 3 | 8 | 1 | 
| 4 | 10 | 1 | 
| 5 | 5 | 1 | 

Gcd sớm giảm xuống còn 1, nghĩa là không thể thành lập nhóm lớn hơn một người. Vì vậy, không có khách. 

Đầu ra:```
0
```Điều này cho thấy gcd nhanh chóng nắm bắt được sự không tương thích giữa các giá trị như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log A) | Mỗi phép tính gcd là logarit theo kích thước giá trị, được áp dụng trên n phần tử | 
| Không gian | O(1) | Chỉ một giá trị gcd đang chạy được lưu trữ | 

Các ràng buộc cho phép tối đa 100.000 giá trị và mỗi giá trị lên tới 10^9, do đó, các phép toán gcd logarit có thể thoải mái phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io
from math import gcd

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    g = a[0]
    for x in a[1:]:
        g = gcd(g, x)

    return str(g - 1)

def run(inp: str) -> str:
    return solve(inp)

# provided samples
assert run("2\n5 10\n") == "4"
assert run("5\n2 7 8 10 5\n") == "0"

# custom tests
assert run("1\n7\n") == "6", "single bakery"
assert run("3\n6 12 18\n") == "5", "clear gcd structure"
assert run("4\n1 1 1 1\n") == "0", "all ones edge"
assert run("2\n1000000000 500000000\n") == "499999999", "large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 số | giá trị đơn trừ một | xử lý đầu vào tối thiểu | 
| 6 12 18 | 5 | ổn định gcd nhiều bước | 
| tất cả những cái | 0 | trường hợp gcd nhỏ nhất | 
| số lượng lớn | số học đúng | xử lý tràn và cân | 

## Vỏ cạnh 

Khi chỉ có một tiệm bánh, gcd chỉ đơn giản là giá trị đó, do đó câu trả lời trở thành a_i - 1. Thuật toán xử lý điều này một cách tự nhiên vì vòng lặp trên các phần tử còn lại bị bỏ qua, giữ nguyên giá trị ban đầu. 

Đối với đầu vào như 7, gcd đang chạy là 7, do đó đầu ra trở thành 6, nghĩa là sáu khách cộng với Kuzya. 

Khi tất cả các giá trị đều bằng nhau, chẳng hạn như 10 10 10, gcd vẫn bằng 10. Điều này có nghĩa là bữa tiệc có thể có tổng cộng 10 người, tức là 9 khách. Thuật toán bảo toàn điều này vì gcd(a, a) luôn là a. 

Khi mảng chứa 1, gcd ngay lập tức trở thành 1 và không có bản cập nhật nào khác có thể thay đổi nó. Điều này buộc câu trả lời là không có khách nào, vì không thể thành lập nhóm lớn hơn một người.

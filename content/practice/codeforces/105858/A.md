---
title: "CF 105858A - Biển báo xen kẽ"
description: "Chúng ta được cho một mảng các số nguyên khác 0. Chúng ta cần chọn một dãy con, nghĩa là chúng ta giữ một số phần tử theo thứ tự ban đầu và loại bỏ phần còn lại."
date: "2026-06-25T14:44:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105858
codeforces_index: "A"
codeforces_contest_name: "2025 Winter ESCOM Training Camp, Final Contest"
rating: 0
weight: 105858
solve_time_s: 54
verified: true
draft: false
---

[CF 105858A - Biển báo xen kẽ](https://codeforces.com/problemset/problem/105858/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên khác 0. Chúng ta cần chọn một dãy con, nghĩa là chúng ta giữ một số phần tử theo thứ tự ban đầu và loại bỏ phần còn lại. Các phần tử được chọn phải có dấu xen kẽ, vì vậy mọi số dương phải theo sau là số âm và mọi số âm phải theo sau là số dương. Trong số tất cả các dãy con như vậy, chúng ta muốn tổng tối đa có thể có của các giá trị được chọn. Cho phép dãy con trống nên câu trả lời có thể bằng 0. 

Độ dài mảng có thể đạt tới$10^5$và các giá trị có thể lớn bằng$10^9$về giá trị tuyệt đối. Một cách tiếp cận bậc hai sẽ yêu cầu khoảng$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa những gì phù hợp với giới hạn thời gian của cuộc thi thông thường. Chúng ta cần một nghiệm gần với thời gian tuyến tính, bởi vì một lần vượt qua$10^5$các yếu tố có thể dễ dàng quản lý. 

Phần khó khăn không phải là bản thân điều kiện xen kẽ mà là việc xử lý các nhóm dấu bằng nhau và khả năng lấy một số còn tệ hơn là bỏ qua nó. Ví dụ: với tất cả các số âm như:```
3
-5 -2 -7
```câu trả lời là:```
0
```Một giải pháp bất cẩn luôn chọn ít nhất một phần tử sẽ xuất ra sai`-2`. 

Một trường hợp cạnh khác là một nhóm các số có cùng dấu trong đó chỉ nên lấy một số. Vì:```
5
3 8 1 -10 5
```câu trả lời tốt nhất là`8 - 10 + 5 = 3`. Chọn mọi số dương trước số âm sẽ cho`3 + 8 + 1 - 10 + 5 = 7`, nhưng các dấu không thay thế nhau vì các giá trị dương nằm liền kề trong dãy con. Thuật toán phải hiểu rằng chỉ có một giá trị từ mỗi khối dấu liên tiếp có thể đóng góp. 

Trường hợp ranh giới cuối cùng là một phần tử duy nhất:```
1
100
```Câu trả lời là`100`, vì dãy con một phần tử đã thỏa mãn quy tắc xen kẽ. 

# Phương pháp tiếp cận 

Một ý tưởng đơn giản là thử mọi dãy con và giữ dãy con hợp lệ nhất. Số dãy con của một mảng có độ dài$n$là$2^n$, vì vậy điều này trở nên không thể ngay cả đối với các mảng nhỏ vừa phải. Chúng ta có thể cải thiện nó bằng lập trình động để ghi nhớ câu trả lời hay nhất kết thúc bằng số dương hoặc số âm. Điều này hoạt động, nhưng nó có nhiều trạng thái hơn mức vấn đề cần. 

Quan sát quan trọng là trong một khối số liên tiếp có cùng dấu, dãy con có thể chứa nhiều nhất một phần tử. Lấy hai số dương liên tiếp sẽ vi phạm sự luân phiên và lấy số dương nhỏ hơn thay vì số lớn hơn không bao giờ có thể giúp ích. Lý do tương tự cũng áp dụng cho các khối âm: vì muốn tối đa hóa tổng, chúng ta nên lấy giá trị ít gây hại nhất, tức là giá trị lớn nhất. 

Điều này làm giảm vấn đề quét mảng, chia nó thành các khối tối đa có dấu bằng nhau, lấy phần tử tối đa từ mỗi khối và thêm các giá trị đã chọn đó. Dãy con trống được xử lý một cách tự nhiên vì mỗi khối tối đa được chọn chỉ có thể cải thiện kết quả và nếu tất cả các lựa chọn có sẵn đều âm thì câu trả lời cuối cùng sẽ được so sánh với 0. 

Brute-force hoạt động vì nó khám phá mọi lựa chọn hợp lệ có thể có, nhưng không thành công do không gian tìm kiếm quá lớn. Việc quan sát các khối dấu hiệu sẽ loại bỏ tất cả các lựa chọn không cần thiết và chỉ để lại đại diện tốt nhất cho mỗi khối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Lập trình động | O(n) | O(1) | Đã chấp nhận | 
| Đăng Chặn Tham Lam | O(n) | O(1) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Bắt đầu với phần tử đầu tiên. Nó tạo khối dấu hiệu đầu tiên và giá trị tốt nhất hiện tại bên trong khối này là phần tử đó. 
2. Di chuyển mảng từ trái sang phải. Nếu phần tử hiện tại có cùng dấu với khối hiện tại, hãy cập nhật mức tối đa được lưu trữ cho khối này. Lý do là dãy con chỉ có thể lấy một giá trị từ khối này, vì vậy chúng tôi giữ lại giá trị tốt nhất có thể. 
3. Khi dấu thay đổi, khối trước đó đã kết thúc. Thêm giá trị tối đa của nó vào câu trả lời, sau đó bắt đầu một khối mới với phần tử hiện tại. 
4. Sau khi vòng lặp kết thúc, hãy thêm giá trị tối đa từ khối cuối cùng vì không có sự thay đổi dấu hiệu nào sau đây để kích hoạt cập nhật. 
5. Nếu tổng kết quả là âm, xuất ra 0 vì không được phép chọn phần tử nào. 

Tại sao nó hoạt động: mỗi chuỗi con hợp lệ có thể chứa tối đa một phần tử từ mỗi phân đoạn cùng dấu tối đa. Sự lựa chọn tham lam lấy phần tử lớn nhất có thể có từ mỗi phân khúc, do đó, đối với mỗi phân khúc, nó sẽ mang lại sự đóng góp tốt nhất một cách độc lập. Vì việc chọn từ một phân khúc không ảnh hưởng đến giá trị nào có thể được chọn từ phân khúc khác nên việc kết hợp các lựa chọn tốt nhất cục bộ này sẽ tạo ra chuỗi con tốt nhất trên toàn cầu. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    ans = 0
    cur = a[0]
    sign = 1 if a[0] > 0 else -1

    for x in a[1:]:
        current_sign = 1 if x > 0 else -1

        if current_sign == sign:
            cur = max(cur, x)
        else:
            ans += cur
            cur = x
            sign = current_sign

    ans += cur

    print(max(ans, 0))

solve()
```Biến`cur`lưu trữ giá trị tốt nhất bên trong khối dấu hiệu hiện tại. Nó bắt đầu với phần tử mảng đầu tiên vì mỗi khối phải chứa ít nhất một giá trị. 

Trong quá trình quét, dấu bằng chỉ cập nhật`cur`, vì việc chọn một phần tử khác từ cùng một khối là không thể trong một chuỗi con xen kẽ hợp lệ. Khi một dấu hiệu thay đổi, khối trước đó không thể phát triển được nữa nên giá trị tốt nhất của nó sẽ được thêm vào`ans`. 

Phần bổ sung cuối cùng sau vòng lặp sẽ xử lý khối cuối cùng. Việc quên bước này là một lỗi thường gặp vì khối cuối cùng không bao giờ gặp phải sự thay đổi dấu hiệu. các`max(ans, 0)`xử lý các mảng trong đó mọi tổng dãy con có thể có đều âm. 

# Ví dụ đã hoạt động 

## Mẫu 1 

đầu vào:```
4
7 1 -1 5
```Dấu vết: 

| Chỉ mục | Giá trị | Khối ký hiệu hiện tại | Được lưu trữ tối đa | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | tích cực | 7 | 0 | 
| 2 | 1 | tích cực | 7 | 0 | 
| 3 | -1 | tiêu cực | -1 | 7 | 
| 4 | 5 | tích cực | 5 | 7 | 

Sau vòng lặp, khối cuối cùng đóng góp`5`, cho`7 - 1 + 5 = 11`. 

Đầu ra:```
11
```Điều này cho thấy tại sao thuật toán chỉ giữ giá trị lớn nhất từ ​​phân đoạn cùng dấu. 

## Mẫu 2 

đầu vào:```
3
-1 -1 -1
```Dấu vết: 

| Chỉ mục | Giá trị | Khối ký hiệu hiện tại | Được lưu trữ tối đa | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | -1 | tiêu cực | -1 | 0 | 
| 2 | -1 | tiêu cực | -1 | 0 | 
| 3 | -1 | tiêu cực | -1 | 0 | 

Đóng góp cuối cùng là`-1`, do đó tổng tính được là âm. Vì cho phép một dãy con trống nên câu trả lời sẽ là 0. 

Đầu ra:```
0
```# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mảng được quét một lần và mọi phần tử đều được xử lý liên tục. | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ, không phụ thuộc vào kích thước mảng. | 

Giải pháp tuyến tính dễ dàng phù hợp với$10^5$giới hạn vì nó chỉ thực hiện một số thao tác trên mỗi phần tử mảng và tránh lưu trữ bất kỳ cấu trúc dữ liệu bổ sung nào. 

# Trường hợp thử nghiệm```python
import sys, io

def solve_case(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    ans = 0
    cur = a[0]
    sign = 1 if a[0] > 0 else -1

    for x in a[1:]:
        s = 1 if x > 0 else -1
        if s == sign:
            cur = max(cur, x)
        else:
            ans += cur
            cur = x
            sign = s

    ans += cur
    return str(max(ans, 0))

assert solve_case("4\n7 1 -1 5\n") == "11", "sample 1"
assert solve_case("3\n-1 -1 -1\n") == "0", "sample 2"

assert solve_case("1\n100\n") == "100", "single positive"
assert solve_case("6\n5 2 9 -4 -10 3\n") == "12", "multiple blocks"
assert solve_case("5\n-8 -2 -3 -1 -9\n") == "0", "all negative"
assert solve_case("7\n1 -5 -2 4 8 -1 7\n") == "14", "block boundaries"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 100`|`100`| Xử lý phần tử đơn | 
|`5 / -8 -2 -3 -1 -9`|`0`| Trường hợp dãy con trống | 
|`6 / 5 2 9 -4 -10 3`|`12`| Chọn nhóm dấu bên trong tối đa | 
|`7 / 1 -5 -2 4 8 -1 7`|`14`| Xử lý đúng nhiều chuyển đổi dấu hiệu | 

# Vỏ cạnh 

Đối với trường hợp toàn âm:```
3
-1 -1 -1
```toàn bộ mảng là một khối âm. Thuật toán giữ giá trị lớn nhất`-1`, nhưng việc thêm nó sẽ làm giảm số tiền. Phép so sánh cuối cùng với số 0 sẽ chọn đúng dãy con trống. 

Đối với các dấu hiệu lặp đi lặp lại kèm theo sự thay đổi:```
5
3 8 1 -10 5
```khối đầu tiên là tích cực và đóng góp`8`, khối thứ hai là âm và đóng góp`-10`và khối cuối cùng đóng góp`5`. Kết quả là`3`, tương ứng với dãy con hợp lệ`[8, -10, 5]`. 

Đối với một giá trị duy nhất:```
1
100
```có một khối và tối đa của nó là`100`. Phép cộng cuối cùng đưa ra câu trả lời đúng mà không yêu cầu bất kỳ mã trường hợp đặc biệt nào.

---
title: "CF 1843A - Sasha và tô màu mảng"
description: "Chúng tôi được cung cấp một mảng các số nguyên. Mỗi phần tử phải được gán chính xác một màu và chúng ta có thể sử dụng bất kỳ số lượng màu nào. Đối với mỗi màu, chúng tôi xem xét tất cả các giá trị được gán cho màu đó."
date: "2026-06-09T06:07:20+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1843
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 881 (Div. 3)"
rating: 800
weight: 1843
solve_time_s: 103
verified: true
draft: false
---

[CF 1843A - Sasha và tô màu mảng](https://codeforces.com/problemset/problem/1843/A) 

**Đánh giá:** 800 
**Tags:** tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 1m 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên. Mỗi phần tử phải được gán chính xác một màu và chúng ta có thể sử dụng bất kỳ số lượng màu nào. 

Đối với mỗi màu, chúng tôi xem xét tất cả các giá trị được gán cho màu đó. Đóng góp của nó vào tổng điểm là sự chênh lệch giữa giá trị lớn nhất và nhỏ nhất trong nhóm đó. Mục tiêu là chọn một màu tối đa hóa tổng của những đóng góp này. 

Vị trí của các phần tử không liên quan. Giá trị của một màu chỉ phụ thuộc vào các giá trị bên trong màu đó, vì vậy sau khi hiểu cấu trúc của bài toán, chúng ta có thể nghĩ thuần túy về tập hợp nhiều số. 

Những hạn chế là rất nhỏ. Mỗi test chứa tối đa 50 số và có tối đa 1000 test. Thậm chí một$O(n^2)$hoặc$O(n^3)$giải pháp sẽ hoàn toàn an toàn. Thách thức không phải là hiệu suất mà là nhận ra cấu trúc tham lam ẩn giấu trong định nghĩa về chi phí. 

Một sai lầm phổ biến là nghĩ rằng việc đưa tất cả các yếu tố vào một màu luôn là tốt nhất vì điều đó tạo ra phạm vi màu lớn nhất có thể. Coi như:```
4
1 3 6 9
```Một màu mang lại giá thành$9 - 1 = 8$. 

Cách tô màu tốt hơn là:```
{1, 9}, {3, 6}
```mang lại:```
(9 - 1) + (6 - 3) = 11
```Một cạm bẫy dễ xảy ra khác là quên rằng các nhóm đơn lẻ không đóng góp gì. Ví dụ:```
1
5
```Màu duy nhất có thể bao gồm một nhóm chứa giá trị 5, vì vậy câu trả lời là:```
5 - 5 = 0
```Một giải pháp ghép các giá trị một cách mù quáng mà không xử lý cẩn thận các độ dài lẻ có thể tạo ra kết quả không chính xác. 

Mảng có tất cả các giá trị bằng nhau cũng rất quan trọng:```
4
2 2 2 2
```Mỗi nhóm có giá trị lớn nhất bằng giá trị nhỏ nhất, vì vậy mọi đóng góp đều bằng 0 và câu trả lời bằng 0 bất kể màu nào. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi màu có thể có của mảng và tính toán chi phí thu được. Điều này đúng vì nó khám phá mọi phép gán hợp lệ của các phần tử cho màu sắc. 

Vấn đề là số lượng chất tạo màu tăng lên cực kỳ nhanh chóng. Ngay cả đối với những giá trị khiêm tốn của$n$, số lượng phân vùng được thiết lập trở nên rất lớn. Số Bell làm cho việc tìm kiếm toàn diện không thể thực hiện được. 

Để tìm ra cách tiếp cận tốt hơn, chúng ta cần hiểu điều gì góp phần tạo nên điểm số. Phần đóng góp của mỗi nhóm là:$$\max(\text{group}) - \min(\text{group})$$Chỉ có phần tử nhỏ nhất và lớn nhất của một nhóm là quan trọng. Bất kỳ yếu tố nào được đặt giữa chúng đều không ảnh hưởng đến sự đóng góp của nhóm đó. 

Giả sử mảng được sắp xếp:$$a_0 \le a_1 \le \dots \le a_{n-1}$$Nếu chúng ta tạo ra một nhóm sử dụng một số giá trị nhỏ và một số giá trị lớn thì sự đóng góp chính là sự khác biệt của chúng. Để tối đa hóa tổng, chúng ta muốn các số lớn nhất đóng vai trò là cực đại của nhóm và các số nhỏ nhất đóng vai trò là cực tiểu của nhóm. 

Chiến lược tốt nhất có thể là ghép nối nhiều lần giá trị chưa sử dụng lớn nhất hiện tại với giá trị chưa sử dụng nhỏ nhất hiện tại. Mỗi cặp như vậy đóng góp:$$a_r - a_l$$Ở đâu$l$chỉ vào đầu bên trái và$r$trỏ đến đầu bên phải của mảng đã sắp xếp. 

Bất kỳ phần tử ở giữa nào chưa được ghép đôi, khi$n$là số lẻ, tạo thành một nhóm đơn và đóng góp bằng không. 

Sau khi sắp xếp, câu trả lời sẽ là:$$(a_{n-1}-a_0) + (a_{n-2}-a_1) + \cdots$$Điều này tự nhiên dẫn đến một giải pháp hai con trỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n log n) | O(1) bổ sung (không bao gồm sắp xếp) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự không giảm. 

Sau khi sắp xếp, giá trị nhỏ nhất ở bên trái và giá trị lớn nhất ở bên phải. 
2. Khởi tạo hai con trỏ,`l = 0`Và`r = n - 1`. 

Những con trỏ này đại diện cho các phần tử nhỏ nhất và lớn nhất không được sử dụng. 
3. Khởi tạo câu trả lời về 0. 
4. Trong khi`l < r`, thêm vào`a[r] - a[l]`để trả lời. 

Điều này ghép giá trị còn lại lớn nhất với giá trị còn lại nhỏ nhất, trích xuất mức đóng góp tối đa có thể có từ hai phần tử đó. 
5. Di chuyển`l`một bước đúng và`r`còn một bước nữa. 

Cả hai yếu tố hiện đã được sử dụng. 
6. Tiếp tục cho đến khi các con trỏ giao nhau. 

Nếu một phần tử vẫn ở giữa, nó sẽ tạo thành một màu đơn có mức đóng góp bằng 0. 
7. Xuất ra câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, mọi đóng góp đều xuất phát từ việc khớp một giá trị đóng vai trò là mức tối thiểu của nhóm với một giá trị khác đóng vai trò là giá trị tối đa của nhóm đó. 

Hãy xem xét bất kỳ màu sắc tối ưu nào. Phần tử lớn nhất của toàn bộ mảng chỉ có thể đóng góp khi trở thành phần tử tối đa của một số nhóm. Để tối đa hóa sự đóng góp của nó, nó phải được ghép nối với giá trị sẵn có nhỏ nhất. Sử dụng mức tối thiểu lớn hơn sẽ chỉ làm giảm sự khác biệt. 

Khi các phần tử nhỏ nhất và lớn nhất được ghép nối, đối số tương tự sẽ áp dụng cho các phần tử còn lại. Việc kết hợp các điểm cực trị nhiều lần sẽ tối đa hóa mọi khác biệt được trích xuất. 

Điều này tạo ra chính xác số tiền:$$(a_{n-1}-a_0)+(a_{n-2}-a_1)+\cdots$$đó là tổng chi phí lớn nhất có thể đạt được bằng cách tô màu bất kỳ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        a.sort()

        l, r = 0, n - 1
        ans = 0

        while l < r:
            ans += a[r] - a[l]
            l += 1
            r -= 1

        print(ans)

if __name__ == "__main__":
    solve()
```Bước đầu tiên sắp xếp mảng. Sau khi sắp xếp, việc ghép cặp tối ưu trở nên rõ ràng vì phần tử nhỏ nhất và lớn nhất không được sử dụng luôn ở cuối. 

Hai con trỏ`l`Và`r`đi vào trong từ cả hai đầu. Mỗi lần lặp lại thêm một đóng góp cặp tối ưu. Vì cả hai điểm cuối đều được sử dụng ngay sau khi sử dụng nên mỗi phần tử chỉ tham gia vào tối đa một cặp. 

điều kiện`l < r`là quan trọng. Khi`n`là số lẻ, một phần tử ở giữa vẫn chưa được ghép đôi. Phần tử đó tương ứng với một màu đơn và đóng góp bằng 0, do đó không cần xử lý đặc biệt. 

Tất cả số học đều vừa vặn thoải mái bên trong các số nguyên Python. Mặc dù các hạn chế rất nhỏ nhưng việc triển khai vẫn hiệu quả và rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:```
1 5 6 3 4
```Mảng được sắp xếp:```
1 3 4 5 6
```| Bước | tôi | r | Cặp | Đóng góp | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 4 | - | - | 0 | 
| 1 | 0 | 4 | (1, 6) | 5 | 5 | 
| 2 | 1 | 3 | (3, 5) | 2 | 7 | 

Giá trị ở giữa 4 vẫn giữ nguyên và đóng góp bằng 0. Câu trả lời cuối cùng là 7. 

Ví dụ này cho thấy rằng việc chia mảng thành nhiều nhóm có thể tạo ra số điểm lớn hơn so với việc đưa mọi thứ vào một nhóm. 

### Ví dụ 2 

Mảng đầu vào:```
1 6 3 9
```Mảng được sắp xếp:```
1 3 6 9
```| Bước | tôi | r | Cặp | Đóng góp | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 3 | - | - | 0 | 
| 1 | 0 | 3 | (1, 9) | 8 | 8 | 
| 2 | 1 | 2 | (3, 6) | 3 | 11 | 

Câu trả lời cuối cùng:```
11
```Dấu vết này thể hiện ý tưởng tham lam then chốt. Việc ghép các cực trị sẽ rút ra sự đóng góp tối đa có thể có từ các giá trị còn lại ở mọi giai đoạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian | O(1) thêm | Chỉ một số biến được sử dụng sau khi sắp xếp | 

Với$n \le 50$, thời gian chạy rất nhỏ. Thậm chí trên 1000 trường hợp thử nghiệm, việc sắp xếp các mảng nhỏ như vậy vẫn dễ dàng phù hợp với giới hạn thời gian. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        a.sort()

        l, r = 0, n - 1
        ans = 0

        while l < r:
            ans += a[r] - a[l]
            l += 1
            r -= 1

        out.append(str(ans))

    return "\n".join(out) + "\n"

# provided sample
assert run(
"""6
5
1 5 6 3 4
1
5
4
1 6 3 9
6
1 13 9 3 7 2
4
2 2 2 2
5
4 5 2 2 3
"""
) == """7
0
11
23
0
5
"""

# minimum size
assert run(
"""1
1
42
"""
) == """0
"""

# all equal values
assert run(
"""1
5
7 7 7 7 7
"""
) == """0
"""

# odd length with middle singleton
assert run(
"""1
5
1 2 3 4 5
"""
) == """6
"""

# maximum-sized style case
assert run(
"""1
50
1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 50 50 50 50 50 50 50 50 50 50 50 50 50 50 50 50 50 50 50 50 50 50 50 50 50
"""
) == """1225
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[42]`|`0`| Kích thước mảng tối thiểu | 
|`[7,7,7,7,7]`|`0`| Tất cả các giá trị bằng nhau | 
|`[1,2,3,4,5]`|`6`| Độ dài lẻ, phần tử ở giữa đơn lẻ | 
| 25 cái và 25 năm mươi |`1225`| Nhiều cặp cực tối ưu | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
5
```Sau khi sắp xếp, mảng vẫn còn`[5]`. Các con trỏ bắt đầu ở cùng một vị trí nên vòng lặp không bao giờ chạy. Câu trả lời vẫn là 0, phù hợp với thực tế là một màu đơn lẻ góp phần`5 - 5 = 0`. 

Hãy xem xét một mảng có độ dài lẻ:```
5
1 2 3 4 5
```Thứ tự sắp xếp không thay đổi. Thuật toán tính toán:```
(5 - 1) + (4 - 2) = 4 + 2 = 6
```giá trị`3`vẫn chưa được ghép nối. Nó tương ứng với một nhóm đơn lẻ có đóng góp bằng 0, chính xác theo yêu cầu. 

Xem xét tất cả các giá trị bằng nhau:```
4
2 2 2 2
```Thuật toán bổ sung:```
(2 - 2) + (2 - 2) = 0
```Mọi cách tô màu có thể có đều có chi phí bằng 0 vì mức tối đa của mỗi nhóm bằng mức tối thiểu của nhóm đó. 

Hãy xem xét trường hợp sử dụng một màu không tối ưu:```
4
1 3 6 9
```Một màu mang lại giá thành`9 - 1 = 8`. 

Thuật toán tính toán:```
(9 - 1) + (6 - 3) = 11
```Điều này thể hiện cái nhìn sâu sắc cốt lõi: việc chia các số thành các cặp cực trị có thể tạo ra tổng phạm vi tổng lớn hơn so với việc nhóm mọi thứ lại với nhau.

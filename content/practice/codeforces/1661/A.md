---
title: "CF 1661A - Cân bằng mảng"
description: "Tại mỗi vị trí i, chúng ta có một cặp giá trị (a[i], b[i]). Hoạt động duy nhất được phép là hoán đổi hai giá trị trong cùng một cặp. Chúng tôi có thể làm điều này một cách độc lập cho bất kỳ vị trí nào. Sau khi chọn cặp nào để hoán đổi, chúng ta thu được mảng a và b cuối cùng."
date: "2026-06-10T02:55:23+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1661
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 126 (Rated for Div. 2)"
rating: 800
weight: 1661
solve_time_s: 125
verified: true
draft: false
---

[CF 1661A - Cân bằng mảng](https://codeforces.com/problemset/problem/1661/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ở mọi vị trí`i`, ta có một cặp giá trị`(a[i], b[i])`. Hoạt động duy nhất được phép là hoán đổi hai giá trị trong cùng một cặp. Chúng tôi có thể làm điều này một cách độc lập cho bất kỳ vị trí nào. 

Sau khi chọn cặp nào để hoán đổi, chúng ta thu được mảng cuối cùng`a`Và`b`. Chi phí là tổng của sự khác biệt tuyệt đối giữa các phần tử liền kề trong mảng đầu tiên cộng với cùng số lượng trong mảng thứ hai:$$\sum_{i=1}^{n-1} \left(|a_i-a_{i+1}| + |b_i-b_{i+1}|\right)$$Nhiệm vụ là giảm thiểu chi phí này. 

Quan sát quan trọng là sự hoán đổi vị trí`i`chỉ ảnh hưởng đến cặp`(a[i], b[i])`. Nó không bao giờ thay đổi giá trị ở các chỉ số khác. Vì mục tiêu chỉ chứa các cặp chỉ số liền kề nên sự đóng góp của các vị trí`i`Và`i+1`có thể được phân tích độc lập với phần còn lại của mảng. 

Những hạn chế nhỏ đến mức đáng ngạc nhiên. Độ dài của mỗi mảng tối đa là 25, mặc dù có thể có tới 4000 trường hợp thử nghiệm. Việc tìm kiếm mạnh mẽ trên tất cả các cấu hình hoán đổi vẫn không thể thực hiện được vì mỗi vị trí có hai lựa chọn, tạo ra`2^n`cấu hình. Vì`n = 25`, tức là hơn 33 triệu khả năng cho mỗi trường hợp thử nghiệm. 

Một sai lầm phổ biến là nghĩ rằng các quyết định ở các vị trí khác nhau tương tác trên toàn cầu và yêu cầu lập trình năng động. Họ có tương tác, nhưng sự tương tác mang tính địa phương rất cao. Khi chúng ta hiểu cấu trúc của hàm chi phí, mỗi cặp liền kề có thể được tối ưu hóa một cách độc lập. 

Coi như:```
a = [1, 10]
b = [10, 1]
```Nếu không có giao dịch hoán đổi, chi phí là$$|1-10|+|10-1|=18.$$Hoán đổi một vị trí mang lại```
a = [1,1]
b = [10,10]
```với chi phí`0`. 

Một quy tắc tham lam như "trao đổi bất cứ khi nào`a[i] > b[i]`" sẽ thất bại trên nhiều ví dụ vì mục tiêu phụ thuộc vào các vị trí lân cận chứ không phụ thuộc vào từng cặp riêng lẻ. 

Một trường hợp tinh tế khác xảy ra khi các giá trị đã được sắp xếp:```
a = [1,2,3]
b = [6,7,8]
```Bất kỳ sự hoán đổi nào cũng chỉ trộn số nhỏ với số lớn và làm tăng sự khác biệt liền kề. Câu trả lời tối ưu có được bằng cách không làm gì cả. Một thuật toán luôn chuẩn hóa các cặp mà không kiểm tra mức đóng góp thực tế có thể tạo ra chi phí lớn hơn. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là thử mọi cấu hình trao đổi. Mỗi vị trí có thể được giữ nguyên hoặc hoán đổi, tạo ra`2^n`khả năng. Đối với mỗi cấu hình, chúng tôi có thể tính toán chi phí kết quả trong`O(n)`thời gian. 

Phương pháp brute-force là đúng vì nó kiểm tra rõ ràng mọi trạng thái cuối cùng có thể có. Vấn đề là kích thước của không gian tìm kiếm. Vì`n = 25`, chúng ta sẽ cần khoảng$$2^{25} \approx 3.3 \times 10^7$$cấu hình cho một trường hợp thử nghiệm duy nhất, vượt xa giới hạn thời gian cho phép. 

Để tìm điều gì đó tốt hơn, hãy nhìn vào một thuật ngữ của mục tiêu:$$|a_i-a_{i+1}| + |b_i-b_{i+1}|.$$Chỉ có vị trí`i`Và`i+1`xuất hiện trong biểu thức này. Để cặp ở vị trí`i`là`(x,y)`và cặp ở vị trí`i+1`là`(u,v)`. 

Đối với hai vị trí này, chúng ta có thể chọn một trong hai hướng:```
(x,y) or (y,x)
(u,v) or (v,u)
```Giả sử chúng ta luôn sắp xếp lại từng cặp sao cho giá trị nhỏ hơn được đặt trước:$$l_i = \min(a_i,b_i), \qquad r_i = \max(a_i,b_i).$$Bây giờ hãy xem xét sự đóng góp của các vị trí liền kề:$$|l_i-l_{i+1}| + |r_i-r_{i+1}|.$$Một số định hướng khác có thể tốt hơn? 

Đối với hai cặp, khả năng duy nhất là:$$|x-u|+|y-v|$$hoặc$$|x-v|+|y-u|.$$Một bất đẳng thức tiêu chuẩn trên đường biểu thị rằng việc kết hợp các giá trị nhỏ hơn với nhau và các giá trị lớn hơn với nhau không bao giờ tệ hơn việc vượt qua các kết nối:$$|x-u|+|y-v| \le |x-v|+|y-u|$$bất cứ khi nào$x \le y$Và$u \le v$. 

Sau khi sắp xếp từng cặp nội bộ, mọi đóng góp liền kề sẽ được giảm thiểu một cách độc lập. Vì mục tiêu tổng thể chỉ đơn giản là tổng của tất cả các đóng góp liền kề nên việc sắp xếp từng cặp một lần là tối ưu toàn cục. 

Giải pháp trở nên cực kỳ đơn giản: thay thế từng cặp bằng`(min, max)`, sau đó tính chi phí cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mọi vị trí`i`, thay thế cặp`(a[i], b[i])`với`(min(a[i], b[i]), max(a[i], b[i]))`. 

Điều này khắc phục hướng giúp giảm thiểu tất cả các đóng góp liền kề trong tương lai liên quan đến vị trí này. 
2. Khởi tạo câu trả lời cho`0`. 
3. Với mọi cặp chỉ số liền kề`i`Và`i+1`, thêm vào$$|a_i-a_{i+1}| + |b_i-b_{i+1}|$$để trả lời. 

Sau bước 1, các mảng này đã thể hiện hướng tối ưu của mỗi cặp. 
4. Xuất số tiền tích lũy. 

### Tại sao nó hoạt động 

Đối với mọi chỉ số liền kề, hãy$$x=\min(a_i,b_i), \quad y=\max(a_i,b_i),$$Và$$u=\min(a_{i+1},b_{i+1}), \quad v=\max(a_{i+1},b_{i+1}).$$Sự đóng góp của hai vị trí này là$$|x-u|+|y-v|$$hoặc$$|x-v|+|y-u|.$$Từ$x \le y$Và$u \le v$, ghép các giá trị nhỏ hơn với nhau và các giá trị lớn hơn với nhau không bao giờ lớn hơn ghép nối chéo. Do đó, hướng được sắp xếp sẽ giảm thiểu sự đóng góp của cặp liền kề này. 

Mục tiêu tổng thể là tổng của những đóng góp liền kề như vậy. Mỗi thuật ngữ được giảm thiểu bằng cùng một lựa chọn cục bộ, cụ thể là đặt phần tử nhỏ hơn của mỗi cặp vào mảng đầu tiên và phần tử lớn hơn vào mảng thứ hai. Vì mọi số hạng đều được giảm thiểu đồng thời nên toàn bộ tổng được giảm thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        for i in range(n):
            if a[i] > b[i]:
                a[i], b[i] = b[i], a[i]

        ans = 0
        for i in range(n - 1):
            ans += abs(a[i] - a[i + 1])
            ans += abs(b[i] - b[i + 1])

        print(ans)

solve()
```Vòng lặp đầu tiên bình thường hóa mọi cặp sao cho`a[i] <= b[i]`. Đây là bước tham lam quan trọng. Sau sự chuyển đổi đó, mọi đóng góp liền kề đều đã ở dạng tối ưu. 

Vòng lặp thứ hai tính toán mục tiêu chính xác như đã xác định. Vì câu trả lời có thể lớn hơn giới hạn số nguyên 32 bit nên việc sử dụng số nguyên Python sẽ tự động tránh được các vấn đề tràn. 

Một lỗi triển khai phổ biến là tính toán câu trả lời trước khi thực hiện tất cả các phép hoán đổi. Bằng chứng dựa vào việc mỗi cặp được chuẩn hóa trước. Một sai lầm khác là quên rằng phần đóng góp chứa cả hai mảng chứ không chỉ một trong số chúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
3 3 10 10
10 10 3 3
```Sau khi bình thường hóa:```
a = [3, 3, 3, 3]
b = [10, 10, 10, 10]
```| tôi | một [tôi] | b[i] | Đóng góp | 
| --- | --- | --- | --- | 
| 0 | 3 | 10 | 0 | 
| 1 | 3 | 10 | 0 | 
| 2 | 3 | 10 | 0 | 

Tổng số câu trả lời =`0`. 

Ví dụ này cho thấy cách hoán đổi các cặp bên trong có thể tách biệt hoàn toàn các giá trị nhỏ khỏi các giá trị lớn, loại bỏ tất cả các khác biệt liền kề. 

### Ví dụ 2 

đầu vào:```
5
1 2 3 4 5
6 7 8 9 10
```Bình thường hóa không thay đổi gì cả. 

| tôi | Thuật ngữ từ một | Thuật ngữ từ b | Tổng số chạy | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 2 | 
| 1 | 1 | 1 | 4 | 
| 2 | 1 | 1 | 6 | 
| 3 | 1 | 1 | 8 | 

Câu trả lời cuối cùng =`8`. 

Ví dụ này chứng minh rằng chiến lược tối ưu không nhất thiết phải thực hiện hoán đổi. Khi mỗi cặp đã được sắp xếp, mảng vẫn không thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Một lượt để chuẩn hóa các cặp và một lượt để tính câu trả lời | 
| Không gian | O(1) thêm | Chỉ có một vài biến ngoài mảng đầu vào | 

Với`n ≤ 25`, thời gian chạy rất nhỏ. Ngay cả với 4000 trường hợp thử nghiệm, giải pháp chỉ thực hiện vài trăm nghìn thao tác, thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline()

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        for i in range(n):
            if a[i] > b[i]:
                a[i], b[i] = b[i], a[i]

        ans = 0
        for i in range(n - 1):
            ans += abs(a[i] - a[i + 1])
            ans += abs(b[i] - b[i + 1])

        out.append(str(ans))

    return "\n".join(out) + "\n"

# provided samples
assert run(
"""3
4
3 3 10 10
10 10 3 3
5
1 2 3 4 5
6 7 8 9 10
6
72 101 108 108 111 44
10 87 111 114 108 100
"""
) == "0\n8\n218\n"

# minimum size
assert run(
"""1
2
1 10
10 1
"""
) == "0\n"

# all equal values
assert run(
"""1
5
7 7 7 7 7
7 7 7 7 7
"""
) == "0\n"

# already ordered
assert run(
"""1
3
1 2 3
4 5 6
"""
) == "4\n"

# alternating pairs requiring swaps
assert run(
"""1
3
10 1 10
1 10 1
"""
) == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=2`, cặp chéo |`0`| Kích thước hợp lệ nhỏ nhất | 
| Tất cả các giá trị bằng nhau |`0`| Xử lý không khác biệt | 
| Đã đặt hàng mảng |`4`| Hoán đổi không bị ép buộc | 
| Các cặp lớn/nhỏ xen kẽ |`0`| Bình thường hóa tham lam là cần thiết | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
2
1 10
10 1
```Sau khi bình thường hóa:```
a = [1,1]
b = [10,10]
```Đóng góp liền kề duy nhất là$$|1-1|+|10-10|=0.$$Đầu ra của thuật toán`0`, đó là tối ưu. 

Hãy xem xét một đầu vào trong đó mọi giá trị đều giống hệt nhau:```
1
4
5 5 5 5
5 5 5 5
```Bình thường hóa không thay đổi gì cả. Mọi sự khác biệt liền kề đều bằng 0, vì vậy câu trả lời là`0`. Thuật toán tránh đưa ra bất kỳ chi phí nhân tạo nào một cách chính xác. 

Hãy xem xét trường hợp không có trao đổi nào giúp:```
1
3
1 2 3
6 7 8
```Tất cả các cặp đã thỏa mãn`a[i] <= b[i]`. Câu trả lời được tính toán là$$(1+1)+(1+1)=4.$$Bất kỳ sự hoán đổi nào cũng sẽ kết hợp các giá trị nhỏ và lớn và tăng ít nhất một khoản đóng góp liền kề. Thuật toán giữ nguyên các mảng và trả về giá trị tối ưu. 

Hãy xem xét một đầu vào có tính xen kẽ cao:```
1
3
10 1 10
1 10 1
```Bình thường hóa tạo ra```
a = [1,1,1]
b = [10,10,10]
```Mọi đóng góp liền kề đều trở thành số không. Điều này xác nhận tính bất biến khóa: đặt phần tử nhỏ hơn của mỗi cặp vào mảng đầu tiên và phần tử lớn hơn vào mảng thứ hai đồng thời giảm thiểu mọi thuật ngữ liền kề của mục tiêu.

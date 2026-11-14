---
title: "CF 1793A - Một Khuyến Mãi Khác"
description: "Bạn cần ít nhất n kg khoai tây. Cửa hàng mở cửa vào hai ngày khác nhau. Vào ngày đầu tiên, mỗi kg có giá a, và có khuyến mãi: mỗi khi bạn trả m kg, bạn sẽ nhận được thêm một kg miễn phí."
date: "2026-06-09T10:17:56+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1793
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 852 (Div. 2)"
rating: 800
weight: 1793
solve_time_s: 217
verified: true
draft: false
---

[CF 1793A - Một chương trình khuyến mãi khác](https://codeforces.com/problemset/problem/1793/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 3 phút 37 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bạn cần ít nhất`n`kg khoai tây. Cửa hàng mở cửa vào hai ngày khác nhau. 

Ngày đầu tiên giá mỗi kg`a`, và có khuyến mãi: mỗi khi bạn thanh toán`m`kg, bạn nhận được thêm một kg miễn phí. Mua hàng`m`số kg trả tiền mang lại cho bạn`m + 1`tổng số kg. 

Ngày thứ hai giá mỗi kg`b`và không có khuyến mãi. 

Bạn có thể mua bất kỳ số nguyên không âm nào của kilôgam vào một trong hai ngày. Mục tiêu là đạt được ít nhất`n`kg trong khi tiêu số lượng xu nhỏ nhất có thể. 

Các ràng buộc ngay lập tức gợi ý rằng mỗi trường hợp thử nghiệm phải được giải quyết trong thời gian không đổi. Các giá trị có thể đạt tới`10^9`, vì vậy việc lặp lại số lượng mua có thể là không thể. Với tối đa`10^4`trường hợp thử nghiệm, thậm chí một`O(n)`giải pháp cho mỗi trường hợp thử nghiệm sẽ quá chậm. 

Khó khăn chính là quyết định khi nào chương trình khuyến mãi thực sự hữu ích. Một sai lầm phổ biến là cho rằng khuyến mãi ngày đầu tiên phải luôn được sử dụng nhiều nhất có thể. Điều đó là sai khi mua`m`kg vào ngày đầu tiên tốn nhiều tiền hơn là chỉ mua`m + 1`kg vào ngày thứ hai. 

Coi như:```
a = 10, b = 1, m = 2, n = 3
```Một khối khuyến mãi tặng 3 kg với giá 20. Mua hết 3 kg vào ngày thứ hai chỉ có giá 3. Sử dụng khuyến mãi ở đây là một điều tai hại. 

Một lỗi dễ mắc phải khác là xử lý sai khối chưa hoàn chỉnh cuối cùng. 

Coi như:```
a = 5, b = 4, m = 2, n = 4
```Một khối khuyến mãi đầy đủ tặng 3 kg với giá 10. Vẫn cần một kg. Mua kg đó vào ngày thứ hai có giá 4, tổng cộng là 14. Mua hết 4 kg vào ngày thứ hai có giá 16. Phần còn lại phải được xử lý riêng. 

Trường hợp cạnh thứ ba xuất hiện khi`n < m + 1`. 

Coi như:```
a = 5, b = 4, m = 100, n = 2
```Bạn không thể hình thành ngay cả một khối khuyến mãi duy nhất. Câu trả lời đơn giản là`2 * min(a, b)`. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử mọi số kg có thể mua trong ngày đầu tiên, tính xem nhận được bao nhiêu kg sau khi tặng quà, sau đó mua số kg còn lại vào ngày thứ hai. Điều này đúng vì nó xem xét mọi chiến lược khả thi. 

Vấn đề là số lượng khả năng tỷ lệ thuận với`n`, Và`n`Có lẽ`10^9`. Ngay cả một trường hợp thử nghiệm cũng không thể xử lý theo cách này. 

Quan sát quan trọng là chương trình khuyến mãi hoạt động theo các khối có kích thước cố định. Trả tiền cho`m`kg vào ngày đầu tiên mang lại`m + 1`kilôgam. 

Do đó, một khối khuyến mãi có giá$$m \cdot a$$và sản xuất$$m+1$$kilôgam. 

Nếu có được`m+1`kg hoàn toàn vào chi phí ngày thứ hai$$(m+1)\cdot b,$$thì chúng ta nên so sánh hai giá trị này. 

Nếu như$$m a \ge (m+1)b,$$một khối khuyến mãi không bao giờ tốt hơn việc mua cùng số lượng vào ngày thứ hai. Trong tình huống đó chúng ta nên bỏ qua hoàn toàn khuyến mãi và mua từng kg với giá rẻ hơn hàng ngày. 

Nếu như$$m a < (m+1)b,$$mọi khối khuyến mãi đầy đủ đều có lợi. Sau đó, chúng tôi sử dụng càng nhiều khối hoàn chỉnh càng tốt và xử lý riêng các kg còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính giá thành 1 khối khuyến mãi:$$\text{promo} = m \cdot a.$$Một khối khuyến mãi cung cấp chính xác`m + 1`kilôgam. 

1. Tính chi phí để có được cùng số kg đó vào ngày thứ hai:$$\text{normal} = (m+1)\cdot b.$$1. Nếu$$m a \ge (m+1)b,$$bỏ qua khuyến mãi. Mỗi kg đơn giản nên được mua với giá rẻ hơn hàng ngày, mang lại cho$$n \cdot \min(a,b).$$1. Nếu không thì sự thăng tiến sẽ có giá trị. Cho phép$$q = \left\lfloor \frac{n}{m+1} \right\rfloor$$là số khối khuyến mãi hoàn chỉnh. 

1. Các khối này đóng góp$$q \cdot m \cdot a$$tiền xu. 

1. Hãy để$$r = n \bmod (m+1).$$Những kg này không đủ để tạo thành một khối khuyến mãi đầy đủ khác. 

1. Đối với phần còn lại`r`kg, mua từng kg bằng cách sử dụng mức giá rẻ hơn trong hai mức giá hàng ngày:$$r \cdot \min(a,b).$$1. Thêm cả hai đóng góp và in kết quả. 

### Tại sao nó hoạt động 

Bất cứ khi nào$$m a < (m+1)b,$$một khối khuyến mãi hoàn chỉnh sẽ rẻ hơn rất nhiều so với việc đạt được cùng số kg vào ngày thứ hai. Thay thế một khối đầy đủ bằng bất kỳ phương pháp nào khác không thể cải thiện câu trả lời. 

Bất cứ khi nào$$m a \ge (m+1)b,$$một khối khuyến mãi không bao giờ có lợi. Bất kỳ giải pháp nào chứa khối như vậy đều có thể được thay thế bằng cách mua cùng số kg vào ngày thứ hai mà không phải trả thêm chi phí. 

Sau khi lấy hết các khối hoàn chỉnh có lãi, số lượng còn lại ít hơn`m+1`. Việc hình thành một khối khuyến mãi khác nhất thiết sẽ phải mua thêm số kg không cần thiết. Đối với số kg còn lại này, mỗi đơn vị có thể được mua độc lập với giá gốc`min(a,b)`, đó là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        a, b = map(int, input().split())
        n, m = map(int, input().split())

        if m * a >= (m + 1) * b:
            print(n * min(a, b))
        else:
            blocks = n // (m + 1)
            rem = n % (m + 1)

            ans = blocks * m * a
            ans += rem * min(a, b)

            print(ans)

solve()
```Chi nhánh đầu tiên xử lý trường hợp khuyến mại không cạnh tranh được với mua hàng thông thường. Trong tình huống đó, mỗi kg chỉ nên được mua với đơn giá rẻ hơn. 

Nhánh thứ hai sử dụng càng nhiều khối khuyến mãi sinh lời càng tốt. Phép chia số nguyên tính toán có bao nhiêu nhóm hoàn chỉnh của`m + 1`kg phù hợp với số lượng mục tiêu. Phần còn lại được xử lý độc lập. 

Thứ tự nhân chỉ quan trọng đối với khả năng đọc vì số nguyên Python có độ chính xác tùy ý. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, số học 64 bit sẽ được yêu cầu vì các giá trị có thể đạt tới khoảng`10^18`. 

Một lỗi thường gặp là chia cho`m`thay vì`m + 1`. Khuyến mãi tạo ra`m + 1`kilôgam, vì vậy các khối hoàn chỉnh phải được tính bằng kích thước đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = 5, b = 4
n = 3, m = 1
```| Số lượng | Giá trị | 
| --- | --- | 
| Giá khối khuyến mãi | 5 | 
| Số tiền tương tự vào ngày thứ hai | 8 | 
| Khối | 1 | 
| Phần còn lại | 1 | 
| Khối đóng góp | 5 | 
| Đóng góp còn lại | 4 | 
| Trả lời | 9 | 

Một khối khuyến mãi tặng 2 kg với giá 5. Một kg nữa được mua vào ngày thứ hai với giá 4. Tổng chi phí là 9. 

### Ví dụ 2 

đầu vào:```
a = 20, b = 15
n = 10, m = 2
```| Số lượng | Giá trị | 
| --- | --- | 
| Giá khối khuyến mãi | 40 | 
| Số tiền tương tự vào ngày thứ hai | 45 | 
| Khối | 3 | 
| Phần còn lại | 1 | 
| Khối đóng góp | 120 | 
| Đóng góp còn lại | 15 | 
| Trả lời | 135 | 

Ba khối khuyến mãi cung cấp 9 kg. Còn lại một kg và được mua vào ngày thứ hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ một vài phép tính số học | 
| Không gian | O(1) | Không có cấu trúc dữ liệu phụ trợ | 

Ngay cả với`10^4`trường hợp thử nghiệm, tổng công việc là rất nhỏ. Giải pháp dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        a, b = map(int, input().split())
        n, m = map(int, input().split())

        if m * a >= (m + 1) * b:
            out.append(str(n * min(a, b)))
        else:
            blocks = n // (m + 1)
            rem = n % (m + 1)
            ans = blocks * m * a + rem * min(a, b)
            out.append(str(ans))

    return "\n".join(out)

# sample
assert run(
"""5
5 4
3 1
5 4
3 2
3 4
3 5
20 15
10 2
1000000000 900000000
1000000000 8
"""
) == "\n".join([
    "9",
    "10",
    "9",
    "135",
    "888888888900000000"
])

# minimum case
assert run(
"""1
1 1
1 1
"""
) == "1"

# promotion useless
assert run(
"""1
10 1
100 2
"""
) == "100"

# n smaller than one block
assert run(
"""1
5 4
2 100
"""
) == "8"

# exact multiple of block size
assert run(
"""1
3 10
9 2
"""
) == "18"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a=1,b=1,n=1,m=1`|`1`| Đầu vào hợp lệ nhỏ nhất | 
|`a=10,b=1,n=100,m=2`|`100`| Nên bỏ qua khuyến mãi | 
|`a=5,b=4,n=2,m=100`|`8`| Không có khối khuyến mãi hoàn chỉnh nào phù hợp | 
|`a=3,b=10,n=9,m=2`|`18`| Exact multiple of block size |

 ## Vỏ cạnh 

Hãy xem xét:```
a = 10
b = 1
n = 3
m = 2
```Một khối khuyến mãi đầy đủ có giá 20 và mang lại 3 kg. Mua 3 kg vào ngày thứ hai chỉ tốn 3. Vì$$20 \ge 3,$$thuật toán đi vào nhánh đầu tiên và trả về$$3 \cdot 1 = 3.$$Điều này chính xác tránh được khuyến mãi. 

Coi như:```
a = 5
b = 4
n = 2
m = 100
```Không thể hoàn thành khối khuyến mãi nào vì một khối sẽ tạo ra 101 kg. Thuật toán tính toán các khối hoàn chỉnh bằng 0 và mua hai kg còn lại với giá gốc`min(5,4)=4`, cho 8. 

Hãy xem xét:```
a = 3
b = 10
n = 9
m = 2
```Mỗi khối khuyến mãi có giá 6 và mang lại 3 kg. Ba khối hoàn chỉnh cung cấp chính xác số lượng cần thiết. Phần còn lại bằng 0 nên câu trả lời đơn giản là$$3 \cdot 6 = 18.$$Điều này xác minh rằng khả năng chia hết chính xác được xử lý mà không cần mua số kg không cần thiết.

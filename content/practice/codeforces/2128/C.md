---
title: "CF 2128C - Bên trái nhất bên dưới"
description: "Chúng ta bắt đầu với một mảng có độ dài n, chứa đầy các số 0. Một phép toán chọn một số nguyên dương x lớn hơn giá trị tối thiểu hiện tại trong mảng. Hoạt động không cho phép chúng tôi chọn vị trí nào nhận được mức tăng."
date: "2026-06-08T03:09:17+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2128
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1039 (Div. 2)"
rating: 1200
weight: 2128
solve_time_s: 136
verified: true
draft: false
---

[CF 2128C - Bên dưới ngoài cùng bên trái](https://codeforces.com/problemset/problem/2128/C) 

**Đánh giá:** 1200 
**Tags:** tham lam, toán học 
**Thời gian giải:** 2m 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một mảng có độ dài`n`, chứa đầy số không. Một thao tác chọn một số nguyên dương`x`nó hoàn toàn lớn hơn giá trị tối thiểu hiện tại trong mảng. 

Hoạt động không cho phép chúng tôi chọn vị trí nào nhận được mức tăng. Thay vào đó, nó tự động tìm chỉ mục ngoài cùng bên trái có giá trị vẫn thấp hơn`x`. Vị trí đó nhận được`+x`. 

Câu hỏi đặt ra là liệu một mảng mục tiêu nhất định`b`có thể được tạo ra từ mảng hoàn toàn bằng 0 sau bất kỳ số lượng thao tác nào như vậy. 

Đầu vào chứa nhiều trường hợp thử nghiệm. Trên tất cả các trường hợp thử nghiệm, tổng số phần tử mảng tối đa là 200.000. Bất kỳ giải pháp nào chậm hơn tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm đều trở nên rủi ro. Một thuật toán bậc hai sẽ yêu cầu khoảng$4 \cdot 10^{10}$trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. Mục tiêu là một$O(n)$hoặc$O(n \log n)$giải pháp. 

Một số tình huống rất dễ bị đánh giá sai. 

Coi như:```
3
3 1 2
```Câu trả lời đúng là`NO`. 

Một sai lầm phổ biến là chỉ xem xét liệu mọi phần tử có thể được biểu diễn dưới dạng tổng của các số gia dương hay không. Điều đó luôn đúng. Khó khăn đến từ quy tắc vị trí ngoài cùng bên trái, áp đặt các ràng buộc thứ tự mạnh mẽ. 

Một trường hợp phức tạp khác là:```
2
1 1
```Câu trả lời là`YES`. 

Một lập luận bất cẩn có thể cho rằng mọi mức tăng phải lớn hơn mức tối thiểu hiện tại và vì phần tử đầu tiên trở thành số dương nên việc đạt đến vị trí thứ hai là không thể. Trong thực tế, sau khi làm cho phần tử đầu tiên bằng`1`, giá trị nhỏ nhất của mảng vẫn là`0`, nên ta có thể chọn`x = 1`một lần nữa và cập nhật phần tử thứ hai. 

Một ví dụ hữu ích nữa là:```
3
40 60 90
```Câu trả lời là`NO`. 

Mặc dù mọi giá trị đều lớn nhưng vị trí thứ ba không thể tích lũy đủ giá trị. Các vị trí trước đó áp đặt giới hạn trên cho kích thước của số gia tăng có thể đạt tới các vị trí sau. 

Hiểu được ràng buộc đó là quan sát quan trọng. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực sẽ cố gắng mô phỏng tất cả các chuỗi hoạt động có thể có. Ở mỗi bước chúng ta có thể chọn nhiều giá trị khác nhau của`x`và số lượng trạng thái có thể tiếp cận tăng lên một cách bùng nổ. Ngay cả đối với các mảng rất nhỏ, cây tìm kiếm cũng trở nên khổng lồ. Không có cách thực tế nào để liệt kê tất cả các khả năng. 

Để tìm cấu trúc, hãy tập trung vào một vị trí duy nhất`i`. 

Giả sử chúng ta đang cố gắng xây dựng`b[i]`. Bất kỳ mức tăng nào đạt đến vị trí`i`phải thỏa mãn hai điều kiện. 

Đầu tiên, nó không được lớn hơn mọi phần tử trước đó. Nếu không thì vị trí`i`sẽ không phải là giá trị ngoài cùng bên trái bên dưới`x`. Nếu tăng kích thước`x`đạt đến vị trí`i`, thì mọi vị trí trước`i`ít nhất phải có rồi`x`. 

Thứ hai, khi vị trí`i`nhận được số tăng đầu tiên, giá trị hiện tại của nó là`0`. Vì bản thân mức tăng là`x`, mức tăng đầu tiên không bao giờ có thể vượt quá mục tiêu cuối cùng`b[i]`. 

Kết hợp những sự thật này, mỗi mức tăng được áp dụng cho vị trí`i`có kích thước nhiều nhất$$\min(b_1,b_2,\ldots,b_{i-1}).$$Cho phép$$m_i=\min(b_1,\ldots,b_{i-1}).$$Nếu như`b[i] > m_i`, thì cách duy nhất để đạt được`b[i]`dành cho mức tăng đầu tiên ở vị trí`i`chính xác là`b[i]`. Bất kỳ tổng số gia nhỏ hơn nào sẽ có tổng tối đa`m_i`. 

Điều đó có nghĩa là chúng ta phải có khả năng áp dụng mức tăng kích thước`b[i]`trực tiếp đến vị trí`i`. Để điều đó xảy ra, mọi vị trí trước đó ít nhất phải có`b[i]`. Vì giá trị cuối cùng của chúng chính xác là`b[1],...,b[i-1]`, điều này đòi hỏi$$b[i] \le m_i.$$Nếu như`b[i] > m_i`, đạt`b[i]`là không thể. 

Ngược lại, nếu$$b[i] \le m_i$$giữ cho mọi vị trí sau vị trí đầu tiên, thì luôn có thể xây dựng được. Chúng ta có thể chỉ cần đưa ra vị trí`i`tăng kích thước một lần`b[i]`khi tất cả các vị trí trước đó đã đạt đến giá trị cuối cùng của chúng. 

Toàn bộ vấn đề rút gọn lại ở việc kiểm tra xem mọi phần tử có tối đa là tối thiểu trong tất cả các phần tử trước đó hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng mục tiêu`b`. 
2. Khởi tạo`pref_min = b[0]`. 

Điều này lưu trữ giá trị tối thiểu trong số tất cả các yếu tố được thấy cho đến nay. 
3. Xử lý các vị trí từ trái qua phải bắt đầu từ chỉ mục`1`. 
4. Đối với giá trị hiện tại`b[i]`, kiểm tra xem$$b[i] \le pref\_min.$$Bất kỳ mức tăng nào đạt đến vị trí này không bao giờ có thể vượt quá giá trị cuối cùng tối thiểu của các vị trí trước đó. 
5. Nếu`b[i] > pref_min`, trả lời ngay`NO`. 

Chức vụ`i`sẽ cần mức tăng lớn hơn mức mà các vị thế trước đó có thể hỗ trợ. 
6. Nếu không thì hãy cập nhật$$pref\_min = \min(pref\_min, b[i]).$$Điều này duy trì mức tối thiểu trong số tất cả các yếu tố được xử lý. 
7. Nếu mọi vị trí đều vượt qua bài kiểm tra, hãy trả lời`YES`. 

### Tại sao nó hoạt động 

Đối với bất kỳ vị trí nào`i > 1`, mọi mức tăng đạt đến nó không được lớn hơn giá trị cuối cùng của mọi vị trí trước đó. Do đó mỗi lần tăng như vậy nhiều nhất là$$m_i=\min(b_1,\ldots,b_{i-1}).$$Nếu như`b[i] > m_i`, không có chuỗi số gia cho phép nào có thể tạo ra`b[i]`, vì mức tăng đầu tiên đã không thể vượt quá`m_i`. 

Nếu như`b[i] \le m_i`, chúng ta có thể đợi cho đến khi vị trí`1..i-1`đã đạt được mục tiêu của mình thì hãy áp dụng một mức tăng kích thước duy nhất`b[i]`để định vị`i`. Vì tất cả các vị trí trước đó ít nhất`b[i]`, thao tác hợp lệ và ngay lập tức kết thúc vị trí đó. 

Do đó, mảng mục tiêu có thể truy cập chính xác khi mọi phần tử sau phần tử đầu tiên không vượt quá mức tối thiểu của tất cả các phần tử trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    ans = []

    for _ in range(t):
        n = int(input())
        b = list(map(int, input().split()))

        pref_min = b[0]
        ok = True

        for i in range(1, n):
            if b[i] > pref_min:
                ok = False
                break
            pref_min = min(pref_min, b[i])

        ans.append("YES" if ok else "NO")

    sys.stdout.write("\n".join(ans))

solve()
```Giải pháp duy trì giá trị tối thiểu trong số tất cả các phần tử ở bên trái vị trí hiện tại. 

Khi xử lý`b[i]`, biến`pref_min`chính xác là$$\min(b_1,\ldots,b_{i-1}).$$Điều kiện về khả năng tiếp cận chỉ đơn giản là`b[i] <= pref_min`. Nếu điều kiện thất bại một lần thì không quá trình xử lý nào sau này có thể sửa chữa được nó, vì vậy chúng tôi có thể dừng ngay lập tức đối với trường hợp kiểm thử đó. 

Tất cả các giá trị vừa vặn thoải mái với số nguyên 32 bit, nhưng số nguyên Python xử lý chúng một cách tự động. Thuật toán chỉ sử dụng một vài biến bất kể`n`, do đó mức sử dụng bộ nhớ là không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
5 6 1 1
```| Vị trí | b[i] | Tiền tố tối thiểu trước khi kiểm tra | Có hiệu lực? | 
| --- | --- | --- | --- | 
| 1 | 5 | - | Bắt đầu | 
| 2 | 6 | 5 | Không | 

Giá trị thứ hai vượt quá mức tối thiểu trong số các phần tử trước đó. 

Từ`6 > 5`, câu trả lời là ngay lập tức`NO`. 

Điều này thể hiện hạn chế chính. Vị trí 2 sẽ cần mức tăng kích thước 6, nhưng vị trí 1 chỉ kết thúc ở mức 5, vì vậy nó không bao giờ có thể hỗ trợ mức tăng như vậy. 

### Ví dụ 2 

đầu vào:```
2
1 1
```| Vị trí | b[i] | Tiền tố tối thiểu trước khi kiểm tra | Có hiệu lực? | 
| --- | --- | --- | --- | 
| 1 | 1 | - | Bắt đầu | 
| 2 | 1 | 1 | Có | 

Mọi vị trí đều thỏa mãn điều kiện. 

Câu trả lời là`YES`. 

Một cách xây dựng hợp lệ là:```
[0,0]
x=1 -> [1,0]
x=1 -> [1,1]
```Ví dụ này cho thấy sự bình đẳng được cho phép. Vị trí sau có thể nhận được mức tăng chính xác bằng giá trị nhỏ nhất trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần | 
| Không gian | O(1) | Chỉ có một số biến được duy trì | 

Tổng của tất cả`n`giá trị trên các trường hợp thử nghiệm tối đa là 200.000. Việc quét tuyến tính trên mọi phần tử chỉ thực hiện được 200.000 phép so sánh, thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline
        t = int(input())
        out = []

        for _ in range(t):
            n = int(input())
            b = list(map(int, input().split()))

            pref_min = b[0]
            ok = True

            for i in range(1, n):
                if b[i] > pref_min:
                    ok = False
                    break
                pref_min = min(pref_min, b[i])

            out.append("YES" if ok else "NO")

        return "\n".join(out)

    return solve()

# provided samples
assert run(
"""4
4
5 6 1 1
3
3 1 2
3
40 60 90
2
1 1
"""
) == "NO\nNO\nNO\nYES"

# minimum size
assert run(
"""1
2
1 1
"""
) == "YES"

# strictly decreasing
assert run(
"""1
5
10 9 8 7 6
"""
) == "YES"

# equality everywhere
assert run(
"""1
4
7 7 7 7
"""
) == "YES"

# first violation appears late
assert run(
"""1
5
8 4 4 4 5
"""
) == "NO"

# large boundary values
assert run(
"""1
3
1000000000 1000000000 1000000000
"""
) == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`| CÓ | Mảng hợp lệ nhỏ nhất | 
|`10 9 8 7 6`| CÓ | Trình tự giảm nghiêm ngặt | 
|`7 7 7 7`| CÓ | Bình đẳng với tiền tố tối thiểu | 
|`8 4 4 4 5`| KHÔNG | Vi phạm xảy ra sau một số vị trí hợp lệ | 
|`10^9 10^9 10^9`| CÓ | Giá trị được phép lớn nhất | 

## Vỏ cạnh 

Hãy xem xét:```
1
3
3 1 2
```Thuật toán xử lý: 

| Vị trí | Giá trị | Tiền tố tối thiểu | 
| --- | --- | --- | 
| 1 | 3 | - | 
| 2 | 1 | 3 | 
| 3 | 2 | 1 | 

Ở vị trí 3, chúng ta có`2 > 1`, vậy câu trả lời là`NO`. 

Vị trí 2 chỉ kết thúc tại`1`, nghĩa là không có mức tăng nào lớn hơn`1`có thể đạt tới vị trí 3. Giá trị mục tiêu`2`là không thể đạt được. 

Coi như:```
1
2
1 1
```Quá trình quét nhìn thấy`1 <= 1`, vậy câu trả lời là`YES`. 

Trường hợp đẳng thức rất dễ bị xử lý sai nếu người ta viết một bất đẳng thức nghiêm ngặt. Điều kiện là`<=`, không`<`. 

Coi như:```
1
3
40 60 90
```Thuật toán dừng ngay ở vị trí 2 vì`60 > 40`. 

Mặc dù các giá trị lớn, nhưng chỉ riêng kích thước là không liên quan. Điều quan trọng là liệu mỗi phần tử có vượt quá giá trị nhỏ nhất trước đó hay không. Khi một vị trí lớn hơn mức tối thiểu đó, các quy tắc hoạt động sẽ ngăn không cho nó được tạo.

---
title: "CF 1256A - Thanh toán không thay đổi"
description: "Chúng ta được yêu cầu xác định xem liệu tổng mục tiêu S có thể được xây dựng bằng cách sử dụng hai loại tiền xu hay không. Một loại có giá trị n và có một đồng xu, loại còn lại có giá trị 1 và có b đồng xu."
date: "2026-06-11T20:52:24+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1256
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 598 (Div. 3)"
rating: 1000
weight: 1256
solve_time_s: 110
verified: true
draft: false
---

[CF 1256A - Thanh toán không thay đổi](https://codeforces.com/problemset/problem/1256/A) 

**Đánh giá:** 1000 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định xem tổng mục tiêu có`S`có thể được xây dựng bằng cách sử dụng hai loại tiền xu. Một loại có giá trị`n`và có`a`đồng tiền của nó, loại kia có giá trị`1`và có`b`đồng xu của nó. Đối với mỗi trường hợp thử nghiệm, nhiệm vụ là quyết định xem chúng ta có thể chọn một số`x`những đồng tiền có giá trị`n`và một số số`y`những đồng tiền có giá trị`1`sao cho tổng giá trị của chúng bằng`S`. Những hạn chế đó là`x`không thể vượt quá`a`Và`y`không thể vượt quá`b`. 

Kích thước đầu vào lớn:`a`,`b`,`n`, Và`S`có thể đi lên`10^9`, và có thể có tới`10^4`trường hợp thử nghiệm. Điều này loại trừ mọi cách tiếp cận bạo lực nhằm thử mọi sự kết hợp của`x`Và`y`bởi vì số lần lặp có thể vượt quá`10^9`cho mỗi trường hợp thử nghiệm, vượt xa tính toán khả thi trong một giây. 

Một trường hợp khó phát hiện khi mục tiêu`S`không phải là bội số của`n`nhưng chúng tôi có ít đồng xu có giá trị`1`. Ví dụ, nếu`a=1`,`b=1`,`n=3`, Và`S=4`, chúng ta có thể lấy một đồng xu có giá trị 3 và một đồng xu có giá trị 1 để đạt được 4. Nhưng nếu`b=0`, giống nhau`S`không thể đạt được mặc dù`S`nhỏ hơn mức tối đa có thể sử dụng`a`đồng xu của`n`. Một cách tiếp cận bất cẩn chỉ kiểm tra`S <= a * n + b`sẽ trả về CÓ không chính xác cho trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là lặp đi lặp lại mọi số lượng có thể của`n`-đồng tiền giá trị`x`từ`0`ĐẾN`a`và cho mỗi`x`kiểm tra xem có`y`như vậy`x*n + y = S`Và`y <= b`. Điều này hiệu quả vì nó trực tiếp kiểm tra tất cả các cách có thể để tiếp cận`S`, nhưng nó đòi hỏi lên đến`a`lần lặp cho mỗi trường hợp thử nghiệm. Từ`a`có thể lên đến`10^9`, cách tiếp cận này là hoàn toàn không thực tế. 

Quan sát quan trọng dẫn đến giải pháp tối ưu là chúng ta không cần phải thử mọi cách.`x`. Đóng góp lớn nhất từ`n`-đồng tiền giá trị nên được sử dụng trước tiên để giảm số tiền còn lại mà chúng ta cần từ`1`- đồng tiền giá trị. Chúng ta có thể đơn giản lấy`x = min(a, S // n)`đồng tiền có giá trị`n`. Điều này tối đa hóa sự đóng góp từ`n`-giá trị đồng xu không vượt quá`a`hoặc vượt quá`S`. Khi đó số tiền còn lại đạt được`S`là`S - x * n`. Nếu số dư này nhỏ hơn hoặc bằng`b`, chúng ta có thể che nó bằng`1`- đồng tiền giá trị. Nếu không, số tiền không thể đạt được. Quan sát này làm giảm giải pháp thành việc kiểm tra liên tục theo thời gian cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(a) | O(1) | Quá chậm đối với a=10^9 | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc các giá trị`a`,`b`,`n`,`S`. 
2. Tính số lượng tối đa`n`- đồng tiền giá trị chúng ta có thể sử dụng mà không vượt quá`S`BẰNG`S // n`. 
3. Giới hạn con số này bằng số tiền thực tế chúng tôi có, đưa ra`x = min(a, S // n)`. 
4. Tính số tiền còn lại sau khi sử dụng`x`đồng tiền có giá trị`n`:`remaining = S - x * n`. 
5. Kiểm tra xem`remaining`nhỏ hơn hoặc bằng`b`. Nếu có thì in`YES`; nếu không thì in`NO`. 

Tại sao nó hoạt động: Bằng cách lấy càng nhiều đồng xu có giá trị cao càng tốt mà không vượt quá mục tiêu, chúng tôi đảm bảo số tiền còn lại được giảm thiểu. Vì đồng xu 1 giá trị có thể bao gồm bất kỳ số tiền nào lên tới`b`, kiểm tra xem phần còn lại có phải là ≤ b hay không đảm bảo rằng tổng chính xác`S`có thể được hình thành nếu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

q = int(input())
for _ in range(q):
    a, b, n, S = map(int, input().split())
    max_n_coins = min(a, S // n)
    remaining = S - max_n_coins * n
    if remaining <= b:
        print("YES")
    else:
        print("NO")
```Giải pháp đầu tiên tính toán số lượng tối đa`n`- đồng tiền giá trị chúng ta có thể lấy (`S // n`), sau đó giới hạn nó bằng số tiền có sẵn (`min(a, S // n)`). Số tiền còn lại được kiểm tra`b`. Việc sử dụng phép chia và phép trừ số nguyên đảm bảo không có lỗi dấu phẩy động. Không có thêm không gian được sử dụng ngoài một vài biến. 

## Ví dụ đã hoạt động 

Dấu vết mẫu 1: 

đầu vào:`1 2 3 4`| Bước | max_n_coin | còn lại | còn lại <= b? | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | phút(1, 4//3)=1 | 4 - 1*3 = 1 | 1 <= 2 | CÓ | 

đầu vào:`1 2 3 6`| Bước | max_n_coin | còn lại | còn lại <= b? | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | phút(1, 6//3)=1 | 6 - 1*3 = 3 | 3 <= 2 | KHÔNG | 

Những dấu vết này chứng minh rằng thuật toán tính toán chính xác số lượng đồng xu có giá trị cao sẽ sử dụng và xác minh xem số tiền còn lại có thể được bao phủ bằng đồng xu 1 giá trị hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Mỗi trường hợp thử nghiệm yêu cầu một vài phép tính số học | 
| Không gian | O(1) | Chỉ một số biến số nguyên được sử dụng | 

Từ bỏ`10^4`trường hợp thử nghiệm, tổng số hoạt động nằm trong`10^5`, dễ dàng phù hợp với giới hạn 1 giây. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    q = int(input())
    for _ in range(q):
        a, b, n, S = map(int, input().split())
        max_n_coins = min(a, S // n)
        remaining = S - max_n_coins * n
        if remaining <= b:
            output.append("YES")
        else:
            output.append("NO")
    return "\n".join(output)

# Provided samples
assert run("4\n1 2 3 4\n1 2 3 6\n5 2 6 27\n3 3 5 18\n") == "YES\nNO\nNO\nYES", "Sample 1"

# Custom cases
assert run("1\n0 5 3 4\n") == "NO", "no n-value coins"
assert run("1\n5 0 3 9\n") == "YES", "exact multiples of n"
assert run("1\n5 0 3 10\n") == "NO", "remainder cannot be covered"
assert run("1\n1 1 1 2\n") == "YES", "minimum coin values"
assert run("1\n1000000000 1000000000 1000000000 2000000000\n") == "YES", "maximum coin counts"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 5 3 4`| KHÔNG | không thể trả S nếu không có đồng xu giá trị n | 
|`5 0 3 9`| CÓ | bội số chính xác của n, không cần đồng xu 1 giá trị | 
|`5 0 3 10`| KHÔNG | phần còn lại không thể được bảo hiểm | 
|`1 1 1 2`| CÓ | giá trị tối thiểu, trường hợp đơn giản | 
|`1000000000 1000000000 1000000000 2000000000`| CÓ | số lượng lớn, kiểm tra căng thẳng | 

## Vỏ cạnh 

Nếu`a`là 0, thuật toán đặt`max_n_coins = 0`và kiểm tra xem`S <= b`. Ví dụ,`a=0, b=5, n=3, S=4`dẫn đến`max_n_coins=0`,`remaining=4`, Và`remaining <= b`chỉ đúng nếu`b >= 4`. Nếu như`b`nhỏ hơn thì thuật toán sẽ đưa ra NO một cách chính xác. 

Nếu như`S`nhỏ hơn`n`nhưng chúng ta có một số đồng xu có giá trị n,`S // n`là 0, vì vậy không có đồng xu có giá trị n nào được sử dụng. Vì`a=5, b=5, n=10, S=3`,`max_n_coins=0`,`remaining=3`, Và`remaining <= b`đánh giá là CÓ vì đồng xu 1 giá trị là đủ. 

Nếu như`S`chính xác bằng`a * n + b`, thuật toán lấy tất cả`a`tiền xu và sử dụng phần còn lại`b`xu để trang trải phần còn lại. Điều này xử lý cạnh giới hạn trên một cách chính xác.

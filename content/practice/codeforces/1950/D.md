---
title: "CF 1950D - Tích của số thập phân nhị phân"
description: "Chúng tôi được cung cấp tới 50.000 truy vấn độc lập. Đối với mỗi truy vấn, một số n được cung cấp, trong đó 1 ≤ n ≤ 100000. Một số được gọi là số thập phân nhị phân nếu mọi chữ số trong biểu diễn thập phân thông thường của nó là 0 hoặc 1. Các ví dụ bao gồm 1, 10, 11, 101 và 1001."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "implementation", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1950
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 937 (Div. 4)"
rating: 1100
weight: 1950
solve_time_s: 77
verified: true
draft: false
---

[CF 1950D - Sản phẩm của số thập phân nhị phân](https://codeforces.com/problemset/problem/1950/D) 

**Đánh giá:** 1100 
**Tags:** vũ lực, dp, thực hiện, lý thuyết số 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp tới 50.000 truy vấn độc lập. Đối với mỗi truy vấn, một số`n`được cung cấp ở đâu`1 ≤ n ≤ 100000`. 

Một số được gọi là số thập phân nhị phân nếu mỗi chữ số trong biểu diễn thập phân thông thường của nó là`0`hoặc`1`. Ví dụ bao gồm`1`,`10`,`11`,`101`, Và`1001`. 

Nhiệm vụ là xác định xem liệu`n`có thể được viết dưới dạng tích của một hoặc nhiều số thập phân nhị phân. Sự lặp lại được cho phép, vì vậy một cái gì đó như`121 = 11 × 11`là hợp lệ. 

Quan sát quan trọng nhất từ ​​các ràng buộc là`n`không bao giờ vượt quá`100000`. Mặc dù có nhiều trường hợp thử nghiệm nhưng phạm vi giá trị có thể có là rất nhỏ. Thay vì giải quyết từng truy vấn từ đầu, chúng ta có thể xử lý trước tất cả các số lên đến`100000`một lần và trả lời mọi trường hợp thử nghiệm trong thời gian không đổi. 

Một vài trường hợp đáng chú ý. 

Coi như:```
1
1
```Câu trả lời đúng là`YES`. số`1`bản thân nó là một số thập phân nhị phân. Một giải pháp chỉ cố gắng phân tích các số thành số thập phân nhị phân nhỏ hơn có thể vô tình từ chối nó. 

Coi như:```
1
100000
```Câu trả lời đúng là`YES`, bởi vì mọi chữ số đều là`0`hoặc`1`, do đó bản thân số đó đã là một tích hợp lệ gồm một thừa số. 

Coi như:```
1
99
```Câu trả lời đúng là`NO`. Mặc dù`99 = 9 × 11`, hệ số`9`không phải là số thập phân nhị phân. Việc triển khai bất cẩn chỉ kiểm tra xem một số ước số có phải là số thập phân nhị phân hay không và bỏ qua tính hợp lệ của yếu tố còn lại một cách đệ quy sẽ tạo ra câu trả lời sai. 

Một trường hợp tế nhị khác là:```
1
121
```Câu trả lời là`YES`, mặc dù`121`bản thân nó không phải là số thập phân nhị phân. Nó bằng`11 × 11`, vì vậy chúng ta phải xét tích của số thập phân nhị phân, không chỉ các số có chữ số riêng là`0`Và`1`. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là phân tích đệ quy mọi số. Đối với một nhất định`n`, liệt kê tất cả các số thập phân nhị phân không vượt quá`n`, hãy thử chia cho từng cái và kiểm tra thương số một cách đệ quy. 

Điều này đúng vì mọi biểu diễn hợp lệ có thể được xem là liên tục loại bỏ một thừa số thập phân nhị phân tại một thời điểm. Vấn đề là sự lặp lại. Với tới 50.000 ca kiểm thử, nhiều con số được kiểm tra đi kiểm tra lại. Khám phá đệ quy cũng xem lại các trạng thái tương tự thông qua các thứ tự phân tích nhân tử khác nhau. 

Quan sát quan trọng xuất phát từ giới hạn rất nhỏ,`n ≤ 100000`. 

Có bao nhiêu số thập phân nhị phân`100000`? Một số có sáu chữ số có các chữ số bị giới hạn`{0,1}`có nhiều nhất`2^6 = 64`khả năng. Sau khi loại trừ các giá trị không hợp lệ như dạng số 0 đứng đầu và số vượt quá`100000`, chỉ còn lại vài chục chiếc. 

Điều này biến vấn đề thành một câu hỏi về khả năng tiếp cận. 

Bắt đầu từ`1`, điều này chắc chắn có thể đại diện được. Nếu một số`x`có thể biểu diễn được thì`x × b`cũng có thể biểu diễn cho mọi số thập phân nhị phân`b`, miễn là sản phẩm không vượt quá`100000`. 

Chúng ta có thể xây dựng mọi số có thể biểu diễn lên tới`100000`sử dụng một quá trình năng động đơn giản. Khi quá trình tiền xử lý kết thúc, việc trả lời một truy vấn chỉ là kiểm tra xem`n`đã đạt được. 

Đây thực chất là một biểu đồ trong đó mỗi số trỏ đến`number × binary_decimal`. Vì chỉ có`100000`trạng thái và chỉ vài chục số nhân thập phân nhị phân, toàn bộ tập hợp có thể truy cập có thể được tính một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ trong độ sâu nhân tố hóa | O(độ sâu) | Quá chậm | 
| Tối ưu | Tiền xử lý O(MAX × B), O(1) mỗi truy vấn | O(TỐI ĐA) | Đã chấp nhận | 

Đây`MAX = 100000`Và`B`là số thập phân nhị phân lên đến`100000`, chỉ có vài chục. 

## Hướng dẫn thuật toán 

1. Tạo mỗi số thập phân nhị phân không vượt quá`100000`. 

Một số là số thập phân nhị phân nếu mỗi chữ số thập phân là một trong hai`0`hoặc`1`. Vì giới hạn này nhỏ nên chúng ta có thể đơn giản kiểm tra mọi số từ`1`ĐẾN`100000`. 
2. Tạo một mảng boolean`reachable`kích thước`100001`.`reachable[x]`sẽ có nghĩa là`x`có thể được biểu diễn dưới dạng tích của số thập phân nhị phân. 
3. Đánh dấu`reachable[1] = True`. 

số`1`bản thân nó là số thập phân nhị phân, vì vậy nó là trạng thái bắt đầu hợp lệ. 
4. Lặp lại tất cả các số từ`1`ĐẾN`100000`. 

Bất cứ khi nào`reachable[x]`là đúng, xử lý`x`như một sản phẩm hợp lệ được biết đến. 
5. Với mọi số thập phân nhị phân`b`, tính toán`x × b`. 

Nếu sản phẩm không vượt quá`100000`, đánh dấu nó có thể truy cập được. 

Điều này phản ánh định nghĩa của vấn đề. Nhân một sản phẩm hợp lệ với một số thập phân nhị phân khác sẽ tạo ra một sản phẩm hợp lệ khác. 
6. Sau khi tiền xử lý, trả lời từng trường hợp kiểm thử bằng cách in`YES`nếu như`reachable[n]`là đúng và`NO`nếu không thì. 

### Tại sao nó hoạt động 

Chứng minh được rút ra trực tiếp từ việc đóng dưới phép nhân. 

Ban đầu,`reachable[1]`là đúng. Mỗi khi thuật toán đánh dấu một số mới, nó sẽ thực hiện bằng cách nhân một số đã biểu thị được với một số thập phân nhị phân. Vì vậy, mọi số được đánh dấu đều có thể biểu diễn được một cách thực sự. 

Đối với hướng khác, giả sử một số`n`có thể được viết dưới dạng tích của số thập phân nhị phân:```
n = b1 × b2 × ... × bk
```Bắt đầu từ`1`, quá trình tiền xử lý có thể nhân với`b1`, sau đó bởi`b2`, vân vân. Mỗi sản phẩm trung gian được đánh dấu là có thể tiếp cận được, do đó cuối cùng`n`cũng được đánh dấu. 

Tập hợp được tạo ra bởi thuật toán chính xác là tập hợp các số được biểu diễn dưới dạng tích của số thập phân nhị phân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXN = 100000

def is_binary_decimal(x):
    while x:
        d = x % 10
        if d != 0 and d != 1:
            return False
        x //= 10
    return True

binary_decimals = []
for x in range(1, MAXN + 1):
    if is_binary_decimal(x):
        binary_decimals.append(x)

reachable = [False] * (MAXN + 1)
reachable[1] = True

for x in range(1, MAXN + 1):
    if not reachable[x]:
        continue

    for b in binary_decimals:
        y = x * b
        if y > MAXN:
            break
        reachable[y] = True

t = int(input())
ans = []

for _ in range(t):
    n = int(input())
    ans.append("YES" if reachable[n] else "NO")

sys.stdout.write("\n".join(ans))
```Phần đầu tiên tạo ra tất cả các số thập phân nhị phân lên đến`100000`. Vì giới hạn rất nhỏ nên việc kiểm tra từng số từng chữ số là hoàn toàn hợp lý. 

các`reachable`mảng lưu trữ kết quả tiền xử lý. Chúng tôi bắt đầu từ`1`và nhân liên tục với mỗi số thập phân nhị phân. Vì các số được xử lý theo thứ tự tăng dần nên mọi giá trị có thể tiếp cận cuối cùng sẽ truyền khả năng tiếp cận của nó tới các sản phẩm lớn hơn. 

Một chi tiết triển khai hữu ích là`break`bên trong vòng lặp nhân. Danh sách số thập phân nhị phân được sắp xếp. Một lần`x * b`vượt quá`100000`, mọi phép nhân sau này cũng sẽ vượt quá giới hạn, do đó việc tiếp tục sẽ lãng phí công sức. 

Không cần xử lý đặc biệt đối với`1`. Vì nó vừa là trạng thái bắt đầu vừa là số thập phân nhị phân nên quá trình tiền xử lý sẽ xử lý nó một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
121
```Dấu vết tiền xử lý có liên quan: 

| Hiện tại x | Số thập phân nhị phân được sử dụng | Sản phẩm y | có thể truy cập[y] | 
| --- | --- | --- | --- | 
| 1 | 11 | 11 | Đúng | 
| 11 | 11 | 121 | Đúng | 

Sau khi tiền xử lý: 

| Truy vấn n | có thể truy cập[n] | Đầu ra | 
| --- | --- | --- | 
| 121 | Đúng | CÓ | 

Điều này chứng tỏ rằng một số không nhất thiết phải là số thập phân nhị phân. Chỉ cần nhân các hệ số nhị phân-thập phân là đủ. 

### Ví dụ 2 

đầu vào:```
99
```Các giá trị có thể tiếp cận có liên quan gần 99: 

| Số | Có thể truy cập được? | 
| --- | --- | 
| 11 | Đúng | 
| 22 | Đúng | 
| 55 | Đúng | 
| 99 | Sai | 

Giai đoạn truy vấn: 

| Truy vấn n | có thể truy cập[n] | Đầu ra | 
| --- | --- | --- | 
| 99 | Sai | KHÔNG | 

Điều này chứng tỏ rằng việc có ước số thập phân nhị phân là không đủ. Mọi thừa số trong phân tích tích cuối cùng phải ở dạng nhị phân-thập phân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(MAX × B + t) | Quá trình tiền xử lý truy cập mọi trạng thái có thể truy cập và thử tất cả các số nhân thập phân nhị phân | 
| Không gian | O(TỐI ĐA) | Mảng khả năng tiếp cận lưu trữ một boolean cho mỗi số | 

Với`MAX = 100000`và chỉ vài chục số thập phân nhị phân, quá trình tiền xử lý dễ dàng đủ nhanh. Mỗi truy vấn được trả lời trong thời gian không đổi, lý tưởng cho tối đa 50.000 trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MAXN = 100000

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def is_binary_decimal(x):
        while x:
            d = x % 10
            if d != 0 and d != 1:
                return False
            x //= 10
        return True

    binary_decimals = [
        x for x in range(1, MAXN + 1)
        if is_binary_decimal(x)
    ]

    reachable = [False] * (MAXN + 1)
    reachable[1] = True

    for x in range(1, MAXN + 1):
        if not reachable[x]:
            continue
        for b in binary_decimals:
            y = x * b
            if y > MAXN:
                break
            reachable[y] = True

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        out.append("YES" if reachable[n] else "NO")

    return "\n".join(out)

# provided sample
assert run(
"""11
121
1
14641
12221
10110
100000
99
112
2024
12421
1001
"""
) == """YES
YES
YES
YES
YES
YES
NO
NO
NO
NO
YES"""

# minimum value
assert run(
"""1
1
"""
) == "YES"

# binary decimal itself
assert run(
"""1
10101
"""
) == "YES"

# non-representable small number
assert run(
"""1
2
"""
) == "NO"

# boundary value
assert run(
"""1
100000
"""
) == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`YES`| Trường hợp cơ sở và trạng thái bắt đầu | 
|`10101`|`YES`| Một số đã là số thập phân nhị phân | 
|`2`|`NO`| Giá trị nhỏ không thể biểu thị | 
|`100000`|`YES`| Giới hạn trên của phạm vi cho phép | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1
1
```Quá trình tiền xử lý bắt đầu bằng`reachable[1] = True`. Từ`1`đã có thể biểu diễn được, truy vấn sẽ trả về ngay lập tức`YES`. Điều này xử lý chính xác đầu vào nhỏ nhất có thể. 

Coi như:```
1
100000
```Bản thân số đó là số thập phân nhị phân vì các chữ số của nó chỉ`1`Và`0`. Trong quá trình tiền xử lý, nhân`1`qua`100000`dấu hiệu`reachable[100000] = True`. Câu trả lời là`YES`. 

Coi như:```
1
121
```Thuật toán đánh dấu đầu tiên`11`có thể truy cập từ`1`, sau đó đánh dấu`121 = 11 × 11`. Số này không phải là số thập phân nhị phân mà thuộc dạng đóng của phép nhân thập phân nhị phân nên kết quả là`YES`. 

Coi như:```
1
99
```Không có chuỗi phép nhân thập phân nhị phân nào tạo ra`99`. Vì nó không bao giờ được đánh dấu trong bảng khả năng tiếp cận nên truy vấn trả về`NO`. Điều này tránh được lỗi phổ biến khi chấp nhận các số chỉ vì chúng có một ước số thập phân nhị phân.

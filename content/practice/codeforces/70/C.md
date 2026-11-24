---
title: "CF 70C - Vé may mắn"
description: "Mỗi vé được mô tả bằng hai số nguyên dương. Đầu tiên là số sê-ri a, số thứ hai là số vé bên trong sê-ri b đó."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 70
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 64"
rating: 2200
weight: 70
solve_time_s: 131
verified: true
draft: false
---

[CF 70C - Vé may mắn](https://codeforces.com/problemset/problem/70/C) 

**Đánh giá:** 2200 
**Thẻ:** tìm kiếm nhị phân, cấu trúc dữ liệu, sắp xếp, hai con trỏ 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi vé được mô tả bằng hai số nguyên dương. Đầu tiên là số seri`a`, số thứ hai là số vé bên trong dãy đó`b`. Một tấm vé được gọi là may mắn khi$a \cdot b = \operatorname{rev}(a) \cdot \operatorname{rev}(b)$chức năng`rev(x)`đảo ngược các chữ số thập phân và loại bỏ các số 0 đứng đầu sau khi đảo ngược. Ví dụ,`rev(1200) = 21`. 

Chúng tôi được phép chọn số lượng bộ sách sẽ in và số lượng vé trong mỗi bộ sách. Nếu chúng ta chọn`x`loạt và`y`vé cho mỗi loạt, sau đó tất cả các cặp`(a, b)`với`1 ≤ a ≤ x`Và`1 ≤ b ≤ y`hiện hữu. Trong số những cặp đó ít nhất`w`phải là may mắn. Chúng tôi muốn giảm thiểu tổng số vé được in, tức là`x * y`. 

Các ràng buộc ngay lập tức loại trừ áp lực vũ phu đối với tất cả các cặp. Cả hai`maxx`Và`maxy`có thể đạt được`10^5`, do đó toàn bộ lưới chứa tối đa`10^10`cặp. Ngay cả việc kiểm tra một điều kiện số học duy nhất cho mỗi cặp cũng không thể thực hiện được trong vòng hai giây. 

Số lượng vé may mắn mục tiêu có thể lên tới`10^7`, điều đó có nghĩa là chúng ta cũng không thể dựa vào cấu trúc thưa thớt. Lời giải phải khai thác cấu trúc trong chính phương trình đó. 

Phần khó khăn là hiểu loại số nào thỏa mãn điều kiện. Việc thực hiện bất cẩn có thể cho rằng việc đảo ngược hoạt động theo cấp số nhân, nhưng điều đó là sai. Ví dụ:```
a = 12, b = 21
12 * 21 = 252
rev(12) * rev(21) = 21 * 12 = 252
```Cặp này thật may mắn, mặc dù cả hai số đều không phải là số palindrome. 

Một sai lầm dễ mắc phải khác là quên rằng việc đảo ngược sẽ loại bỏ các số 0 đứng đầu. Coi như:```
a = 10, b = 1
10 * 1 = 10
rev(10) * rev(1) = 1 * 1 = 1
```Tấm vé không may mắn. điều trị`rev(10)`BẰNG`01 = 10`sẽ âm thầm đưa ra những câu trả lời sai. 

Vấn đề tế nhị thứ ba xuất hiện khi không có câu trả lời hợp lệ. Giả định:```
1 1 2
```Chỉ có một vé thôi`(1,1)`, và thật là may mắn. Vì chúng ta cần hai tấm vé may mắn nên kết quả đúng là`-1`. Giải pháp chỉ tìm kiếm một hình chữ nhật đủ lớn mà không kiểm tra tính khả thi sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi cặp`(a, b)`bên trong hình chữ nhật đã chọn, tính cả hai tích và đếm xem có bao nhiêu tích thỏa mãn điều kiện. Sau đó thử tất cả các giá trị có thể có của`x`Và`y`và chọn diện tích nhỏ nhất đạt ít nhất`w`vé may mắn. 

Điều này hoạt động hợp lý vì định nghĩa của một tấm vé may mắn hoàn toàn mang tính cục bộ đối với một cặp. Vấn đề là quy mô. Có tới`10^10`cặp, và thậm chí việc liệt kê tất cả các hình chữ nhật đã tiêu tốn một hệ số khác`10^10`. Tổng số hoạt động bùng nổ vượt xa bất cứ điều gì thực tế. 

Để cải thiện điều này, chúng ta cần hiểu chính phương trình. 

Bắt đầu từ$a \cdot b = \operatorname{rev}(a) \cdot \operatorname{rev}(b)$Sắp xếp lại mang lại$\frac{a}{\operatorname{rev}(a)} = \frac{\operatorname{rev}(b)}{b}$Điều này có nghĩa là tỷ lệ liên quan đến`a`phải khớp chính xác với tỷ lệ nghịch đảo liên quan đến`b`. 

Xác định một biểu diễn kinh điển: 

f(x)=\frac{x}{\operatorname{rev}(x)}} 

Hai số tạo thành một cặp may mắn khi phân số rút gọn của chúng bằng nhau. 

Điều đó thay đổi hoàn toàn vấn đề. Thay vì kiểm tra từng cặp một cách độc lập, chúng ta có thể nhóm các số theo phân số rút gọn`x / rev(x)`. 

Giả sử một phân số cụ thể xuất hiện`cx`lần giữa các số`1..x`, và phân số tương tự xuất hiện`cy`lần giữa các số`1..y`. Khi đó mọi sự kết hợp giữa các nhóm đó sẽ tạo thành một tấm vé may mắn. Tổng số tiền đóng góp là$cx \cdot cy$Bây giờ nhiệm vụ trở thành đếm tần số của các phân số rút gọn. 

Số số nguyên chỉ là`10^5`, vì vậy chúng tôi có thể xử lý trước mọi giá trị một lần. Đối với mỗi số, hãy tính giá trị đảo ngược của nó, giảm phân số bằng gcd và lưu trữ cặp chuẩn hóa`(p, q)`. 

Sau khi tiền xử lý, chúng ta có thể quét qua các kích thước hình chữ nhật có thể một cách hiệu quả. Nhận xét quan trọng là khi chúng ta tăng từng cạnh một thì chỉ đóng góp của một số mới duy nhất là thay đổi. Điều này cho phép xử lý kiểu hai con trỏ thay vì tính toán lại mọi thứ từ đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(maxx · maxy) trên mỗi hình chữ nhật | O(1) | Quá chậm | 
| Tối ưu | Tiền xử lý O(maxx + maxy) + quét O(maxx + maxy) | O(maxx + maxy) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mọi số nguyên từ`1`ĐẾN`max(maxx, maxy)`, tính giá trị đảo ngược của nó. 
2. Giảm phân số`x / rev(x)`bằng cách chia cả hai vế cho gcd của chúng. Lưu trữ cặp chuẩn hóa`(p, q)`. 

Bước giảm thiểu là rất quan trọng. Các số khác nhau có thể tạo ra tỷ lệ tương đương. Ví dụ:```
12 / 21 = 4 / 7
24 / 42 = 4 / 7
```Cả hai phải thuộc cùng một nhóm. 

1. Xây dựng hai mảng:`typeX[i]`đối với các số có thể sử dụng làm chỉ số chuỗi,`typeY[i]`cho các số có thể sử dụng làm chỉ số vé. 

Mỗi mục lưu trữ phần chuẩn hóa của số đó. 

1. Sử dụng quy trình kiểu cửa sổ trượt`x`. 

Duy trì bảng tần số`cnt`cho tất cả các phân số chuẩn hóa hiện có trong`1..y`. 

Đồng thời duy trì:$good = \sum_t cntX_t \cdot cntY_t$Ở đâu`cntX_t`Và`cntY_t`là số đếm của một loại phân số`t`ở cả hai bên. 

1. Bắt đầu với`y = 0`. Đối với mỗi`x`từ`1`ĐẾN`maxx`, thêm loại phân số mới từ`x`. 
2. Trong khi`good < w`Và`y < maxy`, mở rộng`y`bởi một. 

Khi thêm một số mới vào`y`bên cạnh, giả sử loại của nó là`t`. Mỗi số hiện có trên`x`bên cùng loại tạo thành một cặp may mắn mới, vì vậy:```
good += cntX[t]
cntY[t] += 1
```1. Một lần`good ≥ w`, hình chữ nhật`(x, y)`là khả thi. 

So sánh khu vực của nó với câu trả lời tốt nhất được thấy cho đến nay. 

1. Tiếp tục cho đến hết`x`được xử lý. 
2. Nếu không tìm thấy hình chữ nhật khả thi, hãy in`-1`. 

### Tại sao nó hoạt động 

Điều bất biến là tất cả các cặp may mắn đều được tính chính xác bằng cách khớp các loại phân số chuẩn hóa. 

Đối với bất kỳ cặp nào`(a,b)`:$a \cdot b = \operatorname{rev}(a) \cdot \operatorname{rev}(b)$giữ khi và chỉ khi$\frac{a}{\operatorname{rev}(a)} = \frac{\operatorname{rev}(b)}{b}$Sau khi giảm phân số, đẳng thức trở thành đẳng thức của các cặp chuẩn hóa. Mỗi vé may mắn thuộc về chính xác một loại phân số và mọi sự kết hợp trong cùng một loại đều là may mắn. 

Cửa sổ trượt duy trì số lượng chính xác về số lượng mỗi lớp xuất hiện ở cả hai bên. Vì mỗi bản cập nhật chỉ thay đổi một tần số,`good`luôn bằng số vé may mắn thực sự trong hình chữ nhật hiện tại. 

Bởi vì`y`chỉ di chuyển về phía trước, tổng số chuyển động của con trỏ là tuyến tính. 

## Giải pháp Python```python
import sys
from math import gcd
from collections import defaultdict

input = sys.stdin.readline

def rev(x):
    return int(str(x)[::-1])

def solve():
    maxx, maxy, w = map(int, input().split())

    limit = max(maxx, maxy)

    typ = [None] * (limit + 1)

    for i in range(1, limit + 1):
        r = rev(i)
        g = gcd(i, r)
        typ[i] = (i // g, r // g)

    cnt_x = defaultdict(int)
    cnt_y = defaultdict(int)

    good = 0
    y = 0

    best_area = None
    best_ans = None

    for x in range(1, maxx + 1):
        t = typ[x]

        good += cnt_y[t]
        cnt_x[t] += 1

        while y < maxy and good < w:
            y += 1
            ty = typ[y]

            good += cnt_x[ty]
            cnt_y[ty] += 1

        if good >= w:
            area = x * y

            if best_area is None or area < best_area:
                best_area = area
                best_ans = (x, y)

    if best_ans is None:
        print(-1)
    else:
        print(best_ans[0], best_ans[1])

solve()
```Phần đầu tiên tính toán loại phân số chuẩn cho mọi số. Việc giảm gcd là nguyên nhân làm cho các tỷ lệ tương đương thu gọn lại thành cùng một khóa. Nếu không chuẩn hóa, các giá trị như`(12,21)`Và`(24,42)`không chính xác sẽ xuất hiện khác nhau. 

Các từ điển`cnt_x`Và`cnt_y`lưu trữ số lần mỗi loại phân số xuất hiện trong tiền tố hiện tại. Biến`good`luôn lưu trữ số lượng cặp may mắn bên trong hình chữ nhật hiện tại. 

Khi chúng ta thêm một giá trị mới vào`x`bên cạnh đó, số lượng vé may mắn mới bằng số loại vé phù hợp đã có trên`y`bên. Đó là lý do tại sao thứ tự cập nhật là:```
good += cnt_y[t]
cnt_x[t] += 1
```Đảo ngược thứ tự sẽ đếm phần tử mới so với chính nó. 

Logic tương tự được áp dụng khi mở rộng`y`. 

Cấu trúc hai con trỏ hiệu quả vì`y`không bao giờ giảm. Trong toàn bộ thuật toán, nó tăng nhiều nhất`maxy`lần. 

Giải pháp sử dụng bộ dữ liệu làm khóa băm. Python xử lý việc này một cách hiệu quả và có thể quản lý được tổng số phân số rút gọn riêng biệt`10^5`những con số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2 1
```Các loại phân số chuẩn hóa: 

| Số | vòng quay (số) | Loại giảm | 
| --- | --- | --- | 
| 1 | 1 | (1,1) | 
| 2 | 2 | (1,1) | 

Dấu vết: 

| x | y | tốt | Hành động | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | thêm x=1 | 
| 1 | 1 | 1 | mở rộng y | 
| 1 | 1 | 1 | khả thi | 

Thuật toán dừng lại với diện tích`1`. Chiếc vé duy nhất là`(1,1)`, thật may mắn vì cả hai bên đều bằng nhau`1`. 

### Ví dụ 2 

đầu vào:```
3 5 6
```Tất cả các số từ`1`ĐẾN`9`là palindromes, vì vậy mọi loại đều`(1,1)`. 

Dấu vết: 

| x | y | tốt | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | mở rộng y | 
| 1 | 2 | 2 | mở rộng y | 
| 1 | 3 | 3 | mở rộng y | 
| 1 | 4 | 4 | mở rộng y | 
| 1 | 5 | 5 | mở rộng y | 
| 2 | 5 | 10 | thêm x=2 | 
| 2 | 5 | 10 | khả thi | 

Hình chữ nhật khả thi đầu tiên là`(2,5)`với diện tích`10`. 

Dấu vết này cho thấy làm thế nào`good`tăng dần. Việc thêm một số mới sẽ đóng góp chính xác vào số loại phân số phù hợp ở phía đối diện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(maxx + maxy) | Mỗi số được xử lý một lần và`y`con trỏ chỉ di chuyển về phía trước | 
| Không gian | O(max(maxx, maxy)) | Mảng và bảng băm lưu trữ một mục nhập cho mỗi số/loại | 

Với giới hạn lên tới`10^5`, tiền xử lý tuyến tính và quét tuyến tính vừa vặn thoải mái trong vòng hai giây. Việc sử dụng bộ nhớ cũng vẫn ở mức nhỏ vì chỉ lưu trữ các số nhận dạng phân số và tần số đã giảm. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import gcd
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def rev(x):
        return int(str(x)[::-1])

    maxx, maxy, w = map(int, input().split())

    limit = max(maxx, maxy)

    typ = [None] * (limit + 1)

    for i in range(1, limit + 1):
        r = rev(i)
        g = gcd(i, r)
        typ[i] = (i // g, r // g)

    cnt_x = defaultdict(int)
    cnt_y = defaultdict(int)

    good = 0
    y = 0

    best_area = None
    best_ans = None

    for x in range(1, maxx + 1):
        t = typ[x]

        good += cnt_y[t]
        cnt_x[t] += 1

        while y < maxy and good < w:
            y += 1
            ty = typ[y]

            good += cnt_x[ty]
            cnt_y[ty] += 1

        if good >= w:
            area = x * y

            if best_area is None or area < best_area:
                best_area = area
                best_ans = (x, y)

    if best_ans is None:
        return "-1"

    return f"{best_ans[0]} {best_ans[1]}"

# provided sample
assert run("2 2 1\n") == "1 1", "sample 1"

# minimum size
assert run("1 1 1\n") == "1 1", "single lucky ticket"

# impossible case
assert run("1 1 2\n") == "-1", "cannot reach enough lucky tickets"

# all small palindromes
assert run("3 3 9\n") == "3 3", "all pairs are lucky"

# boundary style test
out = run("100000 100000 1\n")
assert out != "-1", "should always find one lucky ticket"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`1 1`| Đầu vào hợp lệ nhỏ nhất | 
|`1 1 2`|`-1`| Phát hiện bất khả thi chính xác | 
|`3 3 9`|`3 3`| Cặp nào cũng may mắn với số có một chữ số | 
|`100000 100000 1`| bất kỳ cặp hợp lệ nào | Xử lý ranh giới lớn | 

## Vỏ cạnh 

Hãy xem xét cấu hình không thể:```
1 1 2
```Giai đoạn tiền xử lý tạo ra một loại:```
1 -> (1,1)
```Cuộc truy quét tiến hành: 

| x | y | tốt | 
| --- | --- | --- | 
| 1 | 1 | 1 | 

Thuật toán đạt đến hình chữ nhật tối đa có thể và vẫn chỉ có một vé may mắn. Từ`good < w`, không có câu trả lời nào được ghi lại và đầu ra cuối cùng là`-1`. 

Bây giờ hãy xem xét hành vi số 0 đứng đầu:```
10 1 1
```số`10`trở thành:```
rev(10) = 1
```Loại rút gọn của nó là`(10,1)`, trong khi loại`1`là`(1,1)`. 

Thuật toán không bao giờ được tính`(10,1)`thật may mắn vì các loại khác nhau. Điều này xử lý chính xác sự biến mất của các số 0 đứng đầu sau khi đảo ngược. 

Cuối cùng, hãy xem xét các tỷ lệ tương đương:```
24 42 1
```Chúng tôi nhận được: 

| Số | vòng quay | Loại giảm | 
| --- | --- | --- | 
| 12 | 21 | (4,7) | 
| 24 | 42 | (4,7) | 

Cả hai số đều thuộc cùng một lớp sau khi giảm gcd. Bất kỳ giải pháp nào so sánh trực tiếp các phân số không được rút gọn sẽ bỏ lỡ đẳng thức này. Bước chuẩn hóa đảm bảo chúng được khớp chính xác.

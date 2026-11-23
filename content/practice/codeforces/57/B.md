---
title: "CF 57B - Kiến trúc sao Hỏa"
description: "Mỗi công trình mô tả một cầu thang được đặt trên một dòng ô một chiều. Một cầu thang bắt đầu ở ô l, kết thúc ở ô r và tăng thêm độ cao theo cấp số cộng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 57
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 53"
rating: 1600
weight: 57
solve_time_s: 97
verified: true
draft: false
---

[CF 57B - Kiến trúc sao Hỏa](https://codeforces.com/problemset/problem/57/B) 

**Đánh giá:** 1600 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi công trình mô tả một cầu thang được đặt trên một dòng ô một chiều. Một cầu thang bắt đầu tại phòng giam`l`, kết thúc tại ô`r`và cộng chiều cao trong cấp số cộng. Nếu chiều cao ban đầu là`x`, thì số đá được thêm vào là:`x`Tại`l`

`x + 1`Tại`l + 1`

`x + 2`Tại`l + 2`và cứ thế cho đến khi`x + (r - l)`Tại`r`. 

Một số cầu thang có thể chồng lên nhau và sự đóng góp của chúng sẽ tích lũy. 

Chúng tôi không được yêu cầu xây dựng lại toàn bộ con đường. Thay vào đó chỉ`k`tế bào cụ thể quan trọng. Chúng ta cần tổng số viên đá có trong các ô được truy vấn đó sau khi áp dụng tất cả các bậc thang. 

Đầu vào mang lại: 

- số lượng tế bào, 
- danh sách các công trình cầu thang, 
- tập hợp các ô thú vị có số lượng đá cuối cùng phải được tổng hợp. 

Các giá trị lớn nhất nằm xung quanh`10^5`, điều này ngay lập tức loại trừ mọi thứ bậc hai. Một mô phỏng trực tiếp cập nhật mọi ô bị ảnh hưởng cho mỗi cầu thang có thể hoạt động xung quanh`10^10`hoạt động trong trường hợp xấu nhất, vượt xa giới hạn 2 giây. 

Chi tiết thú vị đó là`k ≤ 100`. Chỉ một số lượng nhỏ các ô được truy vấn. Điều đó thay đổi hướng hoàn toàn. Thay vì cập nhật từng ô mà mỗi cầu thang chạm vào, chúng ta chỉ có thể xử lý các vị trí được truy vấn. 

Một số trường hợp khó xử lý. 

Giả sử một cầu thang bắt đầu trước một ô được truy vấn và kết thúc sau ô đó.```
5 1 1
1 5 3
4
```Ô được truy vấn là`4`. Cầu thang góp phần:`3 4 5 6 7`Tế bào`4`nhận được`6`, không`3`và không`7`. Việc triển khai bất cẩn chỉ lưu trữ giá trị bắt đầu sẽ không tính đến phần bù từ lần bắt đầu cầu thang. 

Một trường hợp tinh vi khác xuất hiện khi các ô được truy vấn nằm ngoài khoảng cầu thang.```
5 1 2
2 4 1
1 5
```Cầu thang chỉ ảnh hưởng đến các ô`2,3,4`. Câu trả lời đúng là`0`. Các lỗi ngẫu nhiên trong kiểm tra theo khoảng thời gian thường bao gồm không chính xác các ô ranh giới. 

Cầu thang chồng chéo cũng có vấn đề.```
5 2 1
1 3 1
2 5 2
3
```Cầu thang đầu tiên góp phần`3`đến tế bào`3`. 

Cầu thang thứ hai góp phần`3`đến tế bào`3`. 

Giá trị cuối cùng là`6`. 

Việc ghi đè ngây thơ thay vì tích lũy sẽ cho kết quả sai. 

Cuối cùng, câu trả lời có thể vượt quá phạm vi số nguyên 32 bit. Mặc dù mỗi cầu thang đều bắt đầu bằng nhiều nhất`1000`, có thể có`10^5`cầu thang. Việc triển khai phải sử dụng số học 64-bit. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là xây dựng toàn bộ con đường một cách rõ ràng. 

Đối với mỗi cầu thang`(l, r, x)`, chúng tôi lặp qua tất cả các ô bị ảnh hưởng và thêm giá trị cấp số cộng phù hợp:```
cell l     += x
cell l + 1 += x + 1
cell l + 2 += x + 2
...
```Sau khi xử lý tất cả các bậc thang, chúng tôi tính tổng các ô được truy vấn. 

Điều này hoạt động hợp lý vì mỗi đóng góp của cầu thang được áp dụng chính xác một lần cho mọi vị trí được che chắn. Vấn đề là chi phí. Một cầu thang đơn có thể bao phủ`10^5`tế bào và có thể có`10^5`cầu thang. Điều đó dẫn đến khoảng`10^10`cập nhật trong trường hợp xấu nhất. 

Quan sát quan trọng là chúng ta không bao giờ cần giá trị của tất cả các ô. Chỉ nhiều nhất`100`vị trí quan trọng. 

Thay vì lặp qua tất cả các ô bên trong một cầu thang, chúng ta có thể lặp qua tất cả các vị trí được truy vấn và kiểm tra xem cầu thang có ảnh hưởng đến chúng hay không. 

Giả sử chúng ta xử lý một ô được truy vấn`p`. 

một cầu thang`(l, r, x)`góp phần vào`p`chỉ khi:```
l ≤ p ≤ r
```Nếu cầu thang bao phủ`p`, thì giá trị tăng thêm là:```
x + (p - l)
```bởi vì mỗi bước sang phải sẽ tăng chiều cao thêm một. 

Bây giờ tổng công việc trở thành:```
m × k
```Từ`m ≤ 100000`Và`k ≤ 100`, mức tối đa chỉ là`10^7`kiểm tra, điều này hoàn toàn được chấp nhận trong Python. 

Lực lượng vũ phu hoạt động vì nó mô phỏng theo nghĩa đen mọi viên đá được thêm vào. Giải pháp tối ưu hóa hoạt động vì bộ truy vấn rất nhỏ nên chỉ kiểm tra các vị trí có liên quan sẽ rẻ hơn nhiều so với việc xây dựng toàn bộ con đường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Tối ưu | O(mk) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các vị trí được truy vấn và lưu trữ chúng vào danh sách. 

Vì chỉ những vị trí này là quan trọng nên không cần theo dõi các ô khác. 
2. Tạo một mảng`ans`chiều dài`k`.`ans[i]`sẽ lưu trữ tổng số đá tích lũy tại`i-th`vị trí được truy vấn. 
3. Xử lý từng bậc thang`(l, r, x)`từng cái một. 

Mỗi bậc thang đều độc lập nên chúng ta có thể thêm ngay phần đóng góp của nó vào tất cả các ô được truy vấn có liên quan. 
4. Đối với mọi vị trí được truy vấn`p`: 

Kiểm tra xem`l ≤ p ≤ r`. 

Nếu sai, cầu thang này không đóng góp gì cho`p`. 
5. Nếu cầu thang che chắn`p`, thêm vào:```
x + (p - l)
```vào mục trả lời tương ứng. 

Thuật ngữ`(p - l)`biểu thị số bước ở bên phải ô được truy vấn tính từ đầu cầu thang. 
6. Sau khi tất cả các bậc thang được xử lý, tính tổng tất cả các mục của`ans`. 

Điều này cung cấp tổng số đá trên tất cả các ô được truy vấn. 

### Tại sao nó hoạt động 

Đối với mọi cầu thang và mọi vị trí được truy vấn, thuật toán sẽ xem xét chính xác một trong hai khả năng. 

Nếu ô được truy vấn nằm ngoài khoảng cầu thang thì cầu thang không đóng góp gì. 

Nếu ô được truy vấn nằm trong khoảng, thì bậc thang sẽ đóng góp chính xác giá trị cấp số cộng tương ứng với khoảng cách của nó đến ô bắt đầu. 

Vì mỗi đóng góp của cầu thang được thêm độc lập và tích lũy là tuyến tính nên giá trị được lưu trữ cuối cùng cho mỗi ô được truy vấn bằng tổng số đá thực sự trong ô đó. Tổng hợp các giá trị đó sẽ tạo ra câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())

    roads = [tuple(map(int, input().split())) for _ in range(m)]
    queries = list(map(int, input().split()))

    ans = [0] * k

    for l, r, x in roads:
        for i in range(k):
            p = queries[i]

            if l <= p <= r:
                ans[i] += x + (p - l)

    print(sum(ans))

solve()
```Giải pháp chỉ giữ lại các vị trí được truy vấn thay vì xây dựng một mảng kích thước`n`. 

Vòng lặp bên ngoài lặp qua cầu thang. Vòng lặp bên trong kiểm tra tất cả các ô được truy vấn. Từ`k`nhiều nhất là`100`, điều này vẫn hiệu quả ngay cả khi`m`đạt tới`100000`. 

Biểu thức:```
x + (p - l)
```là cốt lõi của giải pháp. Nếu cầu thang bắt đầu lúc`l`với chiều cao`x`, sau đó di chuyển một ô sang bên phải sẽ tăng số đá được thêm vào lên một. Một vị trí được truy vấn`p`chính xác là`p - l`bước đi từ đầu. 

Kiểm tra định kỳ:```
if l <= p <= r:
```là rất quan trọng. Việc quên một trong hai ranh giới sẽ tạo ra những đóng góp không chính xác ở các cạnh. 

Câu trả lời cuối cùng là tổng của tất cả các giá trị ô được truy vấn chứ không phải bản thân các giá trị riêng lẻ. Vấn đề yêu cầu tổng cộng một. 

Số nguyên Python tự động xử lý các giá trị lớn nên việc tràn dữ liệu không phải là vấn đề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2 1
1 5 1
2 4 1
3
```Ô được truy vấn là`3`. 

| Cầu thang | Bao gồm 3? | Đóng góp cho 3 | Tổng số chạy | 
| --- | --- | --- | --- | 
| (1, 5, 1) | Có | 1 + (3 - 1) = 3 | 3 | 
| (2, 4, 1) | Có | 1 + (3 - 2) = 2 | 5 | 

Câu trả lời cuối cùng:```
5
```Dấu vết này cho thấy các cầu thang chồng lên nhau tích tụ một cách độc lập như thế nào. 

### Ví dụ 2 

đầu vào:```
6 3 2
1 3 2
2 5 1
4 6 3
2 5
```Các ô được truy vấn là`2`Và`5`. 

| Cầu thang | Ô truy vấn | Bìa? | Đóng góp | Giá trị đang chạy | 
| --- | --- | --- | --- | --- | 
| (1, 3, 2) | 2 | Có | 3 | 3 | 
| (1, 3, 2) | 5 | Không | 0 | 0 | 
| (2, 5, 1) | 2 | Có | 1 | 4 | 
| (2, 5, 1) | 5 | Có | 4 | 4 | 
| (4, 6, 3) | 2 | Không | 0 | 4 | 
| (4, 6, 3) | 5 | Có | 4 | 8 | 

Các giá trị được truy vấn cuối cùng là:```
cell 2 = 4
cell 5 = 8
```Tổng số câu trả lời:```
12
```Ví dụ này thể hiện cả các ô không được che phủ và sự tăng trưởng cấp số cộng trong các khoảng thời gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(mk) | Mỗi cầu thang kiểm tra tất cả các vị trí được truy vấn | 
| Không gian | O(k) | Chỉ tổng số ô được truy vấn mới được lưu trữ | 

Với`m ≤ 100000`Và`k ≤ 100`, tổng số lần lặp nhiều nhất là`10^7`. Điều đó thoải mái phù hợp với giới hạn thời gian trong Python. Việc sử dụng bộ nhớ rất nhỏ vì thuật toán chỉ lưu trữ các ô được truy vấn thay vì toàn bộ con đường. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n, m, k = map(int, input().split())

        roads = [tuple(map(int, input().split())) for _ in range(m)]
        queries = list(map(int, input().split()))

        ans = [0] * k

        for l, r, x in roads:
            for i in range(k):
                p = queries[i]

                if l <= p <= r:
                    ans[i] += x + (p - l)

        return str(sum(ans))

    return solve()

# provided sample
assert run(
"""5 2 1
1 5 1
2 4 1
3
"""
) == "5", "sample 1"

# minimum-size input
assert run(
"""1 1 1
1 1 7
1
"""
) == "7", "minimum case"

# queried cells outside all ranges
assert run(
"""5 1 2
2 4 3
1 5
"""
) == "0", "outside intervals"

# overlapping staircases
assert run(
"""5 2 1
1 3 1
2 5 2
3
"""
) == "6", "overlap accumulation"

# boundary check
assert run(
"""5 1 2
2 4 1
2 4
"""
) == "4", "inclusive boundaries"

# large arithmetic progression
assert run(
"""10 1 1
1 10 5
10
"""
) == "14", "progression growth"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cầu thang đơn và cầu thang đơn | 7 | Hạn chế tối thiểu | 
| Truy vấn ngoài khoảng thời gian | 0 | Loại trừ khoảng thời gian chính xác | 
| Hai cầu thang chồng lên nhau | 6 | Tích lũy hợp lý | 
| Truy vấn chính xác tại các ranh giới khoảng | 4 | Điểm cuối bao gồm | 
| Truy vấn ở cuối cầu thang | 14 | Bù lũy tiến số học đúng | 

## Vỏ cạnh 

Hãy xem xét một ô được truy vấn bên trong một cầu thang nhưng cách xa điểm bắt đầu của nó.```
5 1 1
1 5 3
4
```Cầu thang nói thêm:```
3 4 5 6 7
```Khi xử lý truy vấn`4`, thuật toán tính toán:```
3 + (4 - 1) = 6
```phù hợp với sự đóng góp thực sự. Điều này xác nhận công thức bù đắp là chính xác. 

Bây giờ hãy xem xét các ô được truy vấn hoàn toàn nằm ngoài tất cả các cầu thang.```
5 1 2
2 4 1
1 5
```Đối với truy vấn`1`, điều kiện`2 ≤ 1 ≤ 4`thất bại. 

Đối với truy vấn`5`, điều kiện`2 ≤ 5 ≤ 4`cũng thất bại. 

Không có đóng góp nào được thêm vào, vì vậy cả hai ô được truy vấn vẫn bằng 0 và câu trả lời cuối cùng là`0`. 

Cầu thang chồng chéo là một nguồn phổ biến khác của lỗi.```
5 2 1
1 3 1
2 5 2
3
```Cầu thang đầu tiên góp phần:```
1 + (3 - 1) = 3
```Thứ hai đóng góp:```
2 + (3 - 2) = 3
```Thuật toán thêm cả hai giá trị vào cùng một bộ tích lũy:```
3 + 3 = 6
```phù hợp với kết quả mong đợi. 

Cuối cùng, hãy xem xét ranh giới khoảng.```
5 1 2
2 4 1
2 4
```Tại truy vấn`2`, phần đóng góp là:```
1 + (2 - 2) = 1
```Tại truy vấn`4`, phần đóng góp là:```
1 + (4 - 2) = 3
```Câu trả lời cuối cùng là`4`. 

Điều này xác nhận việc kiểm tra định kỳ đã được thực hiện ở cả hai đầu.

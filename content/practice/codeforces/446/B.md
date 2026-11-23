---
title: "CF 446B - DZY Thích Sửa Đổi"
description: "Chúng ta có một ma trận số nguyên n × m. Chúng ta phải thực hiện chính xác k thao tác. Một thao tác chọn toàn bộ một hàng hoặc toàn bộ một cột. Tổng hiện tại của hàng hoặc cột đó được thêm vào câu trả lời của chúng ta, sau đó mọi phần tử bên trong nó sẽ giảm đi p."
date: "2026-06-07T16:05:23+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 446
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round #FF (Div. 1)"
rating: 2000
weight: 446
solve_time_s: 143
verified: true
draft: false
---

[CF 446B - DZY thích sửa đổi](https://codeforces.com/problemset/problem/446/B) 

**Xếp hạng:** 2000 
**Tags:** bạo lực, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một`n × m`ma trận các số nguyên. Chúng ta phải thực hiện chính xác`k`hoạt động. 

Một thao tác chọn toàn bộ một hàng hoặc toàn bộ một cột. Tổng hiện tại của hàng hoặc cột đó được thêm vào câu trả lời của chúng tôi, sau đó mọi phần tử bên trong nó sẽ giảm đi`p`. 

Cùng một hàng hoặc cột có thể được chọn nhiều lần. Vì mỗi lựa chọn sẽ giảm tất cả các ô của nó nên giá trị thu được từ việc chọn lại nó sẽ trở nên nhỏ hơn. 

Nhiệm vụ là tối đa hóa tổng số niềm vui được tích lũy trong một khoảng thời gian chính xác.`k`hoạt động. 

Kích thước ma trận nhiều nhất là`1000`, đó là vừa phải. Thử thách thực sự đó là`k`có thể lớn như`10^6`. Bất kỳ mô phỏng nào cập nhật rõ ràng các hàng và cột sau mỗi thao tác đều không thể thực hiện được. Thậm chí một`O(k·n)`hoặc`O(k·m)`thuật toán sẽ quá chậm trong trường hợp xấu nhất. 

Câu trả lời có thể trở nên rất lớn. Tổng số hàng có thể vào khoảng`10^6`, và chúng tôi thực hiện lên đến`10^6`hoạt động, do đó số nguyên 64-bit là bắt buộc. 

Phần tinh tế nhất là các thao tác hàng và thao tác cột tương tác với nhau. Việc chọn một hàng sẽ thay đổi tổng của mỗi cột và việc chọn một cột sẽ thay đổi mỗi tổng của hàng. Chiến lược tham lam luôn chiếm hàng hoặc cột lớn nhất hiện tại có thể thất bại vì các lựa chọn trong tương lai bị ảnh hưởng. 

Coi như:```
2 2 2 100
1000 1
1 1
```Hoạt động đầu tiên rõ ràng là hàng đầu tiên. Sau đó, mọi phần tử trong hàng đó giảm đi`100`, làm thay đổi tổng cả hai cột. Một chiến lược liên tục tính toán lại lựa chọn lớn nhất toàn cầu không tiết lộ một cấu trúc tối ưu dễ dàng. 

Một sai lầm dễ mắc phải khác là tính toán trình tự tốt nhất của các thao tác hàng và trình tự thao tác cột tốt nhất một cách độc lập, sau đó chỉ cần cộng chúng lại. 

Ví dụ:```
1 1 2 1
10
```Nếu chúng ta lấy một hàng và một cột thì tổng đơn giản là:```
10 + 10 = 20
```Điều này là không thể. Sau khi lấy hàng, ô đơn sẽ trở thành`9`, do đó cột chỉ mang lại`9`. 

Câu trả lời đúng là:```
19
```Sự tương tác giữa các hàng và cột phải được tính toán rõ ràng. 

## Phương pháp tiếp cận 

Một quan điểm vũ phu là đơn giản. Ở mỗi bước, hãy tính tổng hàng và tổng cột hiện tại, chọn phép toán tốt nhất, cập nhật ma trận và tiếp tục. Điều này đúng vì nó tuân theo quy trình một cách chính xác. 

Vấn đề là quy mô. Ma trận chứa tới`10^6`tế bào và chúng tôi có thể thực hiện lên đến`10^6`hoạt động. Việc tính lại số tiền sau mỗi bước sẽ yêu cầu hàng nghìn tỷ thao tác. 

Quan sát quan trọng là các hàng tương tác với các hàng theo cách rất đơn giản. 

Giả sử một hàng hiện có tổng`S`. Chọn nó mang lại lợi nhuận`S`, thì mỗi cái của nó`m`tế bào giảm đi`p`. Tổng hàng của nó trở thành:```
S - m·p
```Không có gì khác ảnh hưởng đến tổng hàng này khi chúng ta chỉ xem xét các phép toán hàng. 

Điều đó có nghĩa là chuỗi lợi nhuận có được từ việc lựa chọn hàng hoàn toàn độc lập với các cột. Mỗi khi một hàng được chọn, giá trị tương lai của nó sẽ giảm chính xác`m·p`. 

Ý tưởng tương tự cũng áp dụng cho các cột. Sau khi chọn một cột, giá trị tương lai của nó giảm đi một cách chính xác`n·p`. 

Điều này biến vấn đề thành hai tiến trình xếp hàng ưu tiên độc lập. 

Cho phép:`row[i]`= lợi nhuận tối đa có thể đạt được từ chính xác`i`hoạt động hàng.`col[i]`= lợi nhuận tối đa có thể đạt được từ chính xác`i`các thao tác cột. 

Chúng ta có thể tính toán cả hai mảng một cách tham lam với vùng heap tối đa. 

Đối với hàng: 

- Ban đầu chèn tất cả các tổng hàng vào một đống tối đa. 
- Trích xuất nhiều lần tổng hàng lớn nhất hiện tại`x`. 
- Thêm vào`x`đến lợi nhuận tích lũy. 
- Lắp lại`x - m·p`. 

Sau đó`k`lặp đi lặp lại, chúng tôi biết lợi nhuận tốt nhất có thể đạt được từ bất kỳ số lượng hoạt động hàng nào. 

Các cột được tính toán tương tự. 

Bây giờ giả sử chúng ta thực hiện chính xác`i`hoạt động hàng và`k-i`các thao tác cột. 

Bỏ qua sự tương tác, lợi nhuận sẽ là:```
row[i] + col[k-i]
```Tuy nhiên, mỗi thao tác hàng sẽ giảm mỗi ô trong hàng đó đi`p`. Mỗi thao tác với cột sẽ giảm mỗi ô trong cột đó đi`p`. 

Một ô nằm ở giao điểm của thao tác hàng đã chọn và thao tác cột đã chọn sẽ được tính hai lần. 

Nếu chúng ta biểu diễn`i`hoạt động hàng và`k-i`hoạt động cột, có:```
i · (k-i)
```cặp hàng-cột. 

Mỗi cặp giảm một ô giao nhau bằng`p`, và sự mất mát này được tính vào việc tính toán mức độ hài lòng. Trên toàn bộ ma trận, tổng số vượt mức là:```
i · (k-i) · p
```Do đó giá trị thực là:```
row[i] + col[k-i] - i·(k-i)·p
```Chúng tôi thử tất cả`i`từ`0`ĐẾN`k`và lấy tối đa. 

### So sánh độ phức tạp 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k·n·m) hoặc tệ hơn | O(n·m) | Quá chậm | 
| Tối ưu | O((n+m+k) log(n+m)) | O(n+m+k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ma trận và tính tổng hàng và tổng cột. 
2. Xây dựng vùng heap tối đa chứa tất cả các tổng hàng. 
3. Hãy để`row_profit[0] = 0`. 
4. Lặp lại`k`lần: 

1. Trích xuất tổng hàng lớn nhất`x`. 
2. Thêm`x`vào tổng số đang chạy. 
3. Lưu trữ tổng số tích lũy trong`row_profit`. 
4. Lắp lại`x - m·p`. 

Điều này tham lam tạo ra lợi nhuận tối đa có thể đạt được sau mỗi số thao tác hàng có thể. 
5. Tạo một vùng heap tối đa khác chứa tất cả các tổng cột. 
6. Hãy để`col_profit[0] = 0`. 
7. Lặp lại`k`lần: 

1. Trích tổng cột lớn nhất`x`. 
2. Thêm`x`vào tổng số đang chạy. 
3. Lưu trữ tổng số tích lũy trong`col_profit`. 
4. Lắp lại`x - n·p`. 
8. Đối với mọi`i`từ`0`ĐẾN`k`: 

1. Giả sử chính xác`i`hoạt động hàng. 
2. Sau đó có`k-i`các thao tác cột. 
3. Tính toán```
row_profit[i]
+ col_profit[k-i]
- i·(k-i)·p
```4. Cập nhật câu trả lời. 
9. Xuất ra giá trị lớn nhất tìm được. 

### Tại sao nó hoạt động 

Quá trình xử lý heap cho các hàng là tối ưu vì mỗi hàng hoạt động độc lập đối với các hoạt động của hàng trong tương lai. Việc chọn một hàng chỉ thay đổi giá trị tương lai của hàng đó một lượng cố định`m·p`. Tình huống này hoàn toàn giống như việc liên tục lấy phần tử lớn nhất từ ​​một bộ sưu tập và chèn lại nó sau khi trừ đi một hằng số. Sự lựa chọn tham lam luôn là tối ưu. 

Đối số tương tự áp dụng cho các cột. 

Một lần`row_profit[i]`Và`col_profit[j]`đều được biết đến, mọi giải pháp với`i`hoạt động hàng và`j`hoạt động cột đóng góp lợi nhuận độc lập của họ cộng với một điều khoản tương tác. Mỗi cặp hàng-cột gây ra thêm chính xác một phép trừ`p`ở ngã tư của họ. có`i·j`những cặp như vậy, do đó việc điều chỉnh chính xác là`i·j·p`. Vì vậy, mọi chiến lược khả thi với`i+j=k`có giá trị```
row_profit[i] + col_profit[j] - i·j·p.
```Lấy mức tối đa trên tất cả các phần tách có thể mang lại câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m, k, p = map(int, input().split())

    row_sums = [0] * n
    col_sums = [0] * m

    for i in range(n):
        row = list(map(int, input().split()))
        row_sums[i] = sum(row)
        for j, x in enumerate(row):
            col_sums[j] += x

    row_heap = [-x for x in row_sums]
    heapq.heapify(row_heap)

    row_profit = [0] * (k + 1)
    cur = 0

    for i in range(1, k + 1):
        x = -heapq.heappop(row_heap)
        cur += x
        row_profit[i] = cur
        heapq.heappush(row_heap, -(x - m * p))

    col_heap = [-x for x in col_sums]
    heapq.heapify(col_heap)

    col_profit = [0] * (k + 1)
    cur = 0

    for i in range(1, k + 1):
        x = -heapq.heappop(col_heap)
        cur += x
        col_profit[i] = cur
        heapq.heappush(col_heap, -(x - n * p))

    ans = -10**30

    for i in range(k + 1):
        value = (
            row_profit[i]
            + col_profit[k - i]
            - i * (k - i) * p
        )
        ans = max(ans, value)

    print(ans)

solve()
```Phần đầu tiên tính tổng hàng và tổng cột trong một lần quét ma trận. 

Hai vòng lặp heap gần như giống hệt nhau. Mỗi vòng lặp tạo ra lợi nhuận tích lũy tốt nhất sau mỗi số hoạt động có thể có của loại đó. Vùng heap tối đa được triển khai bằng vùng heap tối thiểu của Python bằng cách lưu trữ các giá trị phủ định. 

Bước lắp lại là chi tiết quan trọng. Sau khi chọn một hàng có tổng hiện tại`x`, mỗi cái của nó`m`mục giảm dần`p`, do đó tổng hàng trong tương lai của nó trở thành`x - m·p`. Cột sử dụng`x - n·p`. 

Vòng lặp cuối cùng thử mọi cách phân chia có thể của`k`hoạt động. Từ`row_profit[i]`đã đại diện cho tổng số tối ưu từ chính xác`i`hoạt động hàng và`col_profit[k-i]`biểu thị tổng tối ưu từ các thao tác cột còn lại, chỉ phải áp dụng hiệu chỉnh tương tác. 

Số nguyên Python có độ chính xác tùy ý, do đó không có rủi ro tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 2 2 2
1 3
2 4
```Tổng hàng:```
[4, 6]
```Tổng cột:```
[3, 7]
```#### Quá trình heap hàng 

| Bước | Đã trích xuất | Tích lũy | 
| --- | --- | --- | 
| 0 | - | 0 | 
| 1 | 6 | 6 | 
| 2 | 4 | 10 | 

Vì thế:```
row_profit = [0, 6, 10]
```#### Quá trình tạo đống cột 

| Bước | Đã trích xuất | Tích lũy | 
| --- | --- | --- | 
| 0 | - | 0 | 
| 1 | 7 | 7 | 
| 2 | 3 | 10 | 

Vì thế:```
col_profit = [0, 7, 10]
```#### Đánh giá cuối cùng 

| Hàng | Cột | Giá trị | 
| --- | --- | --- | 
| 0 | 2 | 10 | 
| 1 | 1 | 6 + 7 - 2 = 11 | 
| 2 | 0 | 10 | 

Trả lời:```
11
```Dấu vết này cho thấy mục đích của thuật ngữ hiệu chỉnh. Không trừ`2`, chiến lược hỗn hợp sẽ được định giá quá cao. 

### Mẫu 2 

Hãy xem xét:```
2 2 5 2
1 3
2 4
```Lợi nhuận hàng: 

| Hoạt động | Lợi nhuận | 
| --- | --- | 
| 0 | 0 | 
| 1 | 6 | 
| 2 | 10 | 
| 3 | 12 | 
| 4 | 12 | 
| 5 | 10 | 

Cột lợi nhuận: 

| Hoạt động | Lợi nhuận | 
| --- | --- | 
| 0 | 0 | 
| 1 | 7 | 
| 2 | 10 | 
| 3 | 11 | 
| 4 | 10 | 
| 5 | 7 | 

Đánh giá từng lần chia: 

| Hàng Ops | Đại tá | Giá trị | 
| --- | --- | --- | 
| 0 | 5 | 7 | 
| 1 | 4 | 8 | 
| 2 | 3 | 10 | 
| 3 | 2 | 10 | 
| 4 | 1 | 11 | 
| 5 | 0 | 10 | 

Trả lời:```
11
```Ví dụ này cho thấy câu trả lời tốt nhất có thể đến từ sự kết hợp không rõ ràng giữa các thao tác hàng và cột thay vì tập trung hoàn toàn vào một loại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm + k log n + k log m) | Tính tổng, sau đó thực hiện k thao tác heap cho hàng và cột | 
| Không gian | O(n + m + k) | Heaps cộng với mảng tiền tố lợi nhuận | 

Từ`n,m ≤ 1000`, đọc chi phí ma trận nhiều nhất`10^6`hoạt động. Công việc chủ yếu là xử lý đống. Với`k ≤ 10^6`, mỗi thao tác heap chỉ tốn chi phí`log 1000`, dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m, k, p = map(int, input().split())

    row_sums = [0] * n
    col_sums = [0] * m

    for i in range(n):
        row = list(map(int, input().split()))
        row_sums[i] = sum(row)
        for j, x in enumerate(row):
            col_sums[j] += x

    row_heap = [-x for x in row_sums]
    heapq.heapify(row_heap)

    rp = [0] * (k + 1)
    cur = 0
    for i in range(1, k + 1):
        x = -heapq.heappop(row_heap)
        cur += x
        rp[i] = cur
        heapq.heappush(row_heap, -(x - m * p))

    col_heap = [-x for x in col_sums]
    heapq.heapify(col_heap)

    cp = [0] * (k + 1)
    cur = 0
    for i in range(1, k + 1):
        x = -heapq.heappop(col_heap)
        cur += x
        cp[i] = cur
        heapq.heappush(col_heap, -(x - n * p))

    ans = max(
        rp[i] + cp[k - i] - i * (k - i) * p
        for i in range(k + 1)
    )

    return str(ans)

# sample 1
assert run(
"""2 2 2 2
1 3
2 4
"""
) == "11"

# minimum size
assert run(
"""1 1 1 1
5
"""
) == "5"

# repeated selection of same row
assert run(
"""1 1 2 1
10
"""
) == "19"

# all equal values
assert run(
"""2 2 1 3
5 5
5 5
"""
) == "10"

# boundary-style small case
assert run(
"""2 1 2 1
5
1
"""
) == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1×1`, một thao tác |`5`| Ví dụ tối thiểu | 
|`1×1`, hai thao tác |`19`| Liên tục chọn cùng một cấu trúc | 
| Tất cả các giá trị bằng nhau |`10`| Xử lý đối xứng | 
|`2×1`ma trận |`10`| Small dimensions and split logic |

 ## Vỏ cạnh 

### Repeatedly selecting the same row

 đầu vào:```
1 1 2 1
10
```Hàng duy nhất có tổng`10`. 

Sau một lần lựa chọn, tổng tương lai của nó sẽ trở thành:```
9
```Đống hàng tạo ra:```
row_profit = [0, 10, 19]
```Đống cột tạo ra trình tự tương tự. 

Đánh giá tất cả các phần chia: 

| Hàng | Cột | Giá trị | 
| --- | --- | --- | 
| 0 | 2 | 19 | 
| 1 | 1 | 19 | 
| 2 | 0 | 19 | 

Đầu ra:```
19
```Heap mô hình hóa chính xác việc sử dụng lặp lại cùng một hàng hoặc cột. 

### Các thao tác hỗn hợp hàng và cột 

đầu vào:```
1 1 2 1
10
```Một tính toán ngây thơ có thể thêm:```
10 + 10 = 20
```cho phép toán một hàng và phép toán một cột. 

Thuật toán áp dụng:```
10 + 10 - 1·1·1 = 19
```chính xác chiếm ô được chia sẻ. 

### Số lượng lớn các hoạt động 

đầu vào:```
1 1 5 2
3
```Trình tự các tổng hàng có thể đạt được là:```
3, 1, -1, -3, -5
```Lợi nhuận âm cuối cùng sẽ xuất hiện. Thuật toán vẫn hoạt động vì vùng heap luôn chọn giá trị khả dụng tốt nhất, ngay cả khi tất cả các lựa chọn còn lại đều âm. Vì chính xác`k`cần phải thực hiện các thao tác thì không thể tránh khỏi những lựa chọn tiêu cực. 

Mảng tiền tố xử lý tình huống này một cách tự nhiên và mức tối đa hóa cuối cùng vẫn chính xác.

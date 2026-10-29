---
title: "CF 106215D - Máy đo khoảng cách"
description: "Chúng ta được cung cấp một mảng trong đó mỗi vị trí đại diện cho một từ và điểm được chỉ định của nó. Đối với mọi cặp chỉ số (i, j) có i < j, bài toán xác định $$text{dist}(i,j)=ai+aj$$ Một cặp được coi là đẹp khi khoảng cách khớp chính xác với chênh lệch giữa…"
date: "2026-06-25T06:50:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106215
codeforces_index: "D"
codeforces_contest_name: "2025-2026 Whitney Young Practice Contest 1"
rating: 0
weight: 106215
solve_time_s: 43
verified: true
draft: false
---

[CF 106215D - Chỉ báo khoảng cách](https://codeforces.com/problemset/problem/106215/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng`a`trong đó mỗi vị trí đại diện cho một từ và điểm được chỉ định của nó. 

Với mỗi cặp chỉ số`(i, j)`với`i < j`, bài toán xác định$$\text{dist}(i,j)=a_i+a_j$$Một cặp được coi là đẹp khi khoảng cách khớp chính xác với sự khác biệt giữa các vị trí của chúng:$$j-i=a_i+a_j$$Đối với mỗi trường hợp thử nghiệm, chúng ta phải đếm xem có bao nhiêu cặp thỏa mãn đẳng thức này. 

Độ dài mảng có thể lớn bằng`2 · 10^5`, và tổng của tất cả`n`trên các trường hợp thử nghiệm cũng nhiều nhất`2 · 10^5`. Bất kỳ giải pháp nào kiểm tra từng cặp sẽ yêu cầu khoảng$$\frac{n(n-1)}2$$so sánh. Vì`n = 2 · 10^5`, đó là về`2 · 10^{10}`hoạt động hoàn toàn không thể thực hiện được. 

Tổng kích thước đầu vào gợi ý rõ ràng rằng chúng ta cần thứ gì đó gần với thời gian tuyến tính cho mỗi trường hợp thử nghiệm hoặc tệ nhất là`O(n log n)`. 

Một quan sát tinh tế đến từ giới hạn trên`a_i`. Vì mọi điểm đều dương nên nếu`a_i >= n`, thì không có cặp nào liên quan`i`có thể đẹp mãi vì$$a_i+a_j \ge a_i+1 > n-1$$trong khi giá trị lớn nhất có thể có của`j-i`chỉ là`n-1`. 

### Trường hợp Edge 1: Không thể có cặp 

đầu vào:```
1
3
2025 2025 2025
```Mỗi điểm số đều lớn hơn nhiều so với bất kỳ sự khác biệt chỉ số nào có thể có. Câu trả lời là:```
0
```Việc triển khai ngây thơ không khai thác thực tế này vẫn đúng nhưng lãng phí rất nhiều thời gian để kiểm tra các cặp không thể. 

### Trường hợp Edge 2: Nhiều cặp chỉ số chia sẻ hợp lệ 

đầu vào:```
1
5
1 1 1 1 1
```Các cặp hợp lệ là`(1,3)`,`(2,4)`, Và`(3,5)`vì mỗi người đều có khoảng cách`2`. 

Trả lời:```
3
```Một chiến lược đếm bất cẩn đánh dấu một chỉ mục là đã được sử dụng sẽ bị tính thiếu. 

### Trường hợp Edge 3: Giá trị lớn gần ranh giới mảng 

đầu vào:```
1
4
3 1 1 1
```Đối với chỉ mục`1`, mọi khoảng cách có thể ít nhất là`4`, nhưng chênh lệch chỉ số tối đa chỉ là`3`. 

Trả lời:```
0
```Điều này minh họa tại sao nhiều vị trí có thể bị loại bỏ ngay lập tức. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là kiểm tra từng cặp`(i, j)`, tính toán`a_i + a_j`, so sánh nó với`j - i`và đếm các trận đấu. 

Phương pháp này rõ ràng là đúng vì nó kiểm tra điều kiện chính xác từ định nghĩa. Vấn đề là sự phức tạp của nó. Vì`n = 2 · 10^5`, số lượng cặp khoảng hai mươi tỷ, vượt xa giới hạn. 

Để tìm thứ gì đó nhanh hơn, hãy viết lại điều kiện:$$j-i=a_i+a_j$$Di chuyển các thuật ngữ liên quan đến cùng một chỉ mục sang cùng một phía:$$j-a_j=i+a_i$$Bây giờ cặp đẹp đúng giá trị$$i+a_i$$cho điểm cuối bên trái bằng$$j-a_j$$cho điểm cuối phù hợp. 

Điều này biến bài toán từ việc kiểm tra các cặp thành việc đếm các giá trị bằng nhau. 

Giả sử chúng ta xử lý các chỉ số từ trái sang phải. Khi chúng tôi đến vị trí`j`, chúng ta cần biết có bao nhiêu vị trí trước đó`i`thỏa mãn$$i+a_i=j-a_j$$Nếu chúng ta duy trì bản đồ tần số của tất cả các giá trị đã thấy trước đó`i+a_i`, thì số lượng đối tác hợp lệ cho`j`đơn giản là tần số của`j-a_j`. 

Mỗi chỉ số đóng góp chính xác một lần, đưa ra giải pháp thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Bản đồ băm tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo bản đồ tần số trống. 
2. Xử lý mảng từ trái sang phải bằng cách lập chỉ mục dựa trên 1. 
3. Đối với vị trí hiện tại`j`, tính:$$key = j - a_j$$Mọi chỉ số trước đó`i`thỏa mãn`i + a_i = key`tạo thành một cặp đẹp với`j`. 

1. Thêm tần số của`key`để trả lời. 

Điều này tính tất cả các đối tác hợp lệ trước đó và không có đối tác nào không hợp lệ vì phương trình được chuyển đổi hoàn toàn tương đương với điều kiện ban đầu. 

1. Tính toán$$store = j + a_j$$và chèn nó vào bản đồ tần số. 

Các vị trí trong tương lai có thể sử dụng chỉ mục này làm điểm cuối bên trái của chúng. 

1. Tiếp tục cho đến khi tất cả các vị trí được xử lý. 

### Tại sao nó hoạt động 

Bản đồ tần số luôn chứa các giá trị`i + a_i`đối với các chỉ số nhỏ hơn hoàn toàn so với vị trí hiện tại. 

Khi xử lý chỉ số`j`, cặp nào đẹp cũng phải ưng ý$$i+a_i=j-a_j$$và mọi chỉ mục trên bản đồ đều đã có`i < j`. Như vậy tần số của`j-a_j`chính xác là số chỉ số hợp lệ trước đó. Mỗi cặp đẹp được tính một lần khi điểm cuối bên phải của nó được xử lý và không có cặp không hợp lệ nào có thể thỏa mãn đẳng thức được chuyển đổi. Điều này thiết lập tính đúng đắn. 

## Giải pháp Python```python
import sys
from collections import defaultdict

input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        freq = defaultdict(int)
        ans = 0

        for i in range(1, n + 1):
            x = a[i - 1]

            ans += freq[i - x]
            freq[i + x] += 1

        print(ans)

solve()
```Lời giải tuân theo phép biến đổi đại số trực tiếp. 

Biến`freq`lưu trữ số lượng giá trị`i + a_i`từ các chỉ số được xử lý trước đó. Đối với mỗi vị trí, trước tiên chúng tôi truy vấn`freq[i - a_i]`bởi vì chỉ những chỉ số trước đó mới có thể tạo thành các cặp hợp lệ. Sau khi đếm đóng góp, chúng tôi chèn`i + a_i`vào bản đồ. 

Thứ tự là quan trọng. Nếu quá trình chèn xảy ra trước, chỉ mục hiện tại có thể khớp chính nó không chính xác khi`a_i = 0`. Bài toán đảm bảo các giá trị dương, nhưng việc duy trì đúng thứ tự sẽ giữ cho logic chính xác về mặt toán học. 

Việc sử dụng chỉ mục dựa trên 1 trong vòng lặp khớp chính xác với phương trình ban đầu và tránh các điều chỉnh bổ sung trong quá trình lấy đạo hàm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
5
1 1 1 1 1
```| tôi | một [tôi] | tôi - một[i] | Tìm thấy trận đấu | i + a[i] Đã lưu trữ | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 2 | 0 | 
| 2 | 1 | 1 | 0 | 3 | 0 | 
| 3 | 1 | 2 | 1 | 4 | 1 | 
| 4 | 1 | 3 | 1 | 5 | 2 | 
| 5 | 1 | 4 | 1 | 6 | 3 | 

Câu trả lời cuối cùng:```
3
```Ba cặp được tính là`(1,3)`,`(2,4)`, Và`(3,5)`. 

### Ví dụ 2 

đầu vào:```
1
4
3 1 1 1
```| tôi | một [tôi] | tôi - một[i] | Tìm thấy trận đấu | i + a[i] Đã lưu trữ | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | -2 | 0 | 4 | 0 | 
| 2 | 1 | 1 | 0 | 3 | 0 | 
| 3 | 1 | 2 | 0 | 4 | 0 | 
| 4 | 1 | 3 | 1 | 5 | 1 | 

Câu trả lời cuối cùng:```
1
```Cặp đẹp duy nhất là`(2,4)`bởi vì$$4-2=2$$Và$$1+1=2.$$Ví dụ này chứng minh rằng đẳng thức được chuyển đổi sẽ tự động phát hiện cặp này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được xử lý một lần | 
| Không gian | O(n) | Bản đồ tần số có thể lưu trữ tối đa n khóa riêng biệt | 

Vì tổng của tất cả độ dài mảng trong các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`, tổng thời gian chạy là tuyến tính ở kích thước đầu vào. Điều này dễ dàng phù hợp với giới hạn của cuộc thi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline()

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        freq = defaultdict(int)
        ans = 0

        for i in range(1, n + 1):
            ans += freq[i - a[i - 1]]
            freq[i + a[i - 1]] += 1

        out.append(str(ans))

    return "\n".join(out)

# custom cases

assert run("1\n1\n1\n") == "0", "minimum size"

assert run("1\n5\n1 1 1 1 1\n") == "3", "multiple matches"

assert run("1\n3\n2025 2025 2025\n") == "0", "all impossible"

assert run("1\n4\n3 1 1 1\n") == "1", "boundary example"

assert run("1\n6\n1 2 3 1 2 3\n") == "1", "general case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 1`|`0`| Kích thước mảng tối thiểu | 
|`1 / 5 / 1 1 1 1 1`|`3`| Nhiều cặp hợp lệ | 
|`1 / 3 / 2025 2025 2025`|`0`| Giá trị rất lớn | 
|`1 / 4 / 3 1 1 1`|`1`| Sự khác biệt ranh giới | 
|`1 / 6 / 1 2 3 1 2 3`|`1`| Trường hợp hỗn hợp chung | 

## Vỏ cạnh 

Hãy xem xét:```
1
3
2025 2025 2025
```Quá trình xử lý tạo ra khóa`-2024`,`-2023`, Và`-2022`. Không có giá trị nào trong số này xuất hiện dưới dạng giá trị được lưu trữ`i + a_i`, vì vậy mọi tra cứu đều trả về 0. Các đầu ra thuật toán:```
0
```điều này đúng vì không có sự khác biệt chỉ số nào có thể lên tới hàng nghìn. 

Coi như:```
1
5
1 1 1 1 1
```Các giá trị được lưu trữ trở thành`2, 3, 4, 5, 6`. Tại các vị trí`3`,`4`, Và`5`, các giá trị được truy vấn là`2`,`3`, Và`4`, mỗi cái đã có mặt đúng một lần. Câu trả lời trở thành`3`, đếm chính xác tất cả các cặp hợp lệ mà không cần tính hai lần. 

Coi như:```
1
4
3 1 1 1
```Bản đồ phát triển như sau:```
after i=1: {4:1}
after i=2: {4:1,3:1}
after i=3: {4:2,3:1}
```Tại`i=4`, giá trị truy vấn là`3`, tồn tại một lần. Câu trả lời tăng thêm một và kết thúc tại:```
1
```phù hợp với cặp đẹp duy nhất`(2,4)`.

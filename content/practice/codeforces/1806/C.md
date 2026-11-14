---
title: "CF 1806C - Trình tự chính"
description: "Chúng ta được cho một mảng p có độ dài 2n. Chúng tôi muốn sửa đổi các giá trị của nó và thu được một mảng q khác có cùng độ dài. Mảng mục tiêu không phải là tùy ý."
date: "2026-06-09T09:08:28+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1806
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 858 (Div. 2)"
rating: 1600
weight: 1806
solve_time_s: 125
verified: true
draft: false
---

[CF 1806C - Trình tự chính](https://codeforces.com/problemset/problem/1806/C) 

**Đánh giá:** 1600 
**Tags:** vũ lực, thuật toán xây dựng, toán học 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng`p`chiều dài`2n`. Chúng tôi muốn sửa đổi giá trị của nó và thu được một mảng khác`q`có cùng độ dài. 

Mảng mục tiêu không phải là tùy ý. Nó phải thỏa mãn một điều kiện rất hạn chế: với mọi lựa chọn chính xác`n`vị trí thì tích các giá trị được chọn phải bằng tổng các giá trị ở các vị trí còn lại`n`các vị trí. 

Trong số tất cả các mảng thỏa mãn điều kiện đó, chúng ta cần một mảng có khoảng cách tối thiểu từ Manhattan đến`p`:$$\sum |p_i-q_i|$$Đầu ra chỉ là khoảng cách tối thiểu đó. 

Các ràng buộc là gợi ý đầu tiên rằng đây không phải là một vấn đề tìm kiếm mang tính xây dựng. Tổng giá trị của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2 \cdot 10^5`, do đó, giải pháp được chấp nhận phải gần tuyến tính cho mỗi trường hợp thử nghiệm. Mọi nỗ lực liệt kê các tập hợp con đều không thể thực hiện được vì ngay cả một phép thử đơn lẻ với`n = 20`đã có hơn$10^{11}$tập hợp con có kích thước`n`. 

Thách thức thực sự là hiểu được một mảng tốt có thể trông như thế nào. Khi cấu trúc đó được biết đến, việc tối ưu hóa sẽ trở nên đơn giản. 

Một lỗi phổ biến là cho rằng có nhiều mảng mục tiêu hợp lệ. Trong thực tế, điều kiện này mạnh đến mức gần như tất cả các mảng đều bị loại trừ. 

Ví dụ, khi`n = 3`, mảng```
1 1 1 1 1 1
```là không tốt. Bộ ba nào được chọn đều có tích`1`, trong khi bộ ba còn lại có tổng`3`. 

Một cái bẫy dễ dàng khác xuất hiện cho cả`n`. Coi như`n = 4`Và```
-1 -1 -1 -1 -1 -1 -1 4
```Mảng này thực sự là tốt. Một giải pháp chỉ kiểm tra ứng viên hoàn toàn bằng 0 sẽ bỏ lỡ điều tối ưu. 

Vụ án`n = 1`cũng đặc biệt. Điều kiện trở thành$$q_1=q_2$$nên cặp số bằng nhau đều tốt. điều trị`n = 1`với logic chung sẽ đưa ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô tả đặc điểm của mảng ứng cử viên và xác minh điều kiện bằng cách kiểm tra mọi tập hợp con có kích thước`n`. 

Đối với một mảng có độ dài cố định`2n`, xác minh đã có chi phí$$\binom{2n}{n}$$hoạt động. Điều này phát triển theo cấp số nhân và trở nên vô vọng gần như ngay lập tức. 

Quan sát quan trọng là tình trạng này mạnh hơn nhiều so với lần đầu xuất hiện. Thay vì tìm kiếm giữa tất cả các mảng, chúng ta hoàn toàn có thể phân loại từng mảng tốt. 

Hãy để chúng tôi kiểm tra những gì các điều kiện lực lượng. 

Vì`n = 1`, mọi mảng tốt đều có dạng```
[x, x]
```bởi vì mỗi phần tử phải bằng phần tử kia. 

Đối với lớn hơn`n`, một đối số so sánh tiêu chuẩn giữa hai tập hợp con khác nhau ở một vị trí cho thấy hầu hết tất cả các giá trị đều phải giống hệt nhau. Đẩy ý tưởng này đến kết luận mang lại sự phân loại đầy đủ được sử dụng trong giải pháp chính thức: 

Đối với số lẻ`n ≥ 3`, mảng tốt duy nhất là```
0 0 0 ... 0
```Thậm chí`n ≥ 4`, có chính xác hai mảng tốt có thể hoán vị:```
0 0 0 ... 0
```Và```
-1 -1 ... -1 n
```ở đâu có`2n-1`bản sao của`-1`và một bản sao của`n`. 

Vì`n = 2`, có thêm một khả năng:```
2 2 2 2
```Cùng với```
0 0 0 0
```Và```
-1 -1 -1 2
```đây đều là những mảng tốt. 

Một khi không gian tìm kiếm thu hẹp lại chỉ còn một số ít ứng viên thì việc tối ưu hóa sẽ trở nên dễ dàng. Chúng ta chỉ cần tính khoảng cách từ`p`cho mỗi họ hợp lệ và lấy giá trị tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Trường hợp`n = 1`1. Gọi hai số là`a`Và`b`. 
2. Mỗi mảng tốt là`[x, x]`. 
3. Giá trị tối thiểu của`|a-x| + |b-x|`thu được khi`x`nằm giữa`a`Và`b`. 
4. Câu trả lời đơn giản là`|a-b|`. 

### Trường hợp`n ≥ 2`1. Tính khoảng cách tới mảng toàn 0:$$\sum |p_i|$$Ứng viên này luôn tồn tại. 

1. Nếu`n = 2`, đồng thời tính khoảng cách đến mảng toàn hai:$$\sum |p_i-2|$$1. Nếu`n`là số chẵn, hãy tính khoảng cách đến họ chứa một giá trị`n`Và`2n-1`giá trị bằng`-1`. 
2. Đối với gia đình đó, hãy bắt đầu với$$\text{base}=\sum |p_i+1|$$đó là chi phí để biến mọi vị trí thành`-1`. 

1. Chọn một vị trí để trở thành`n`thay vì. Thay thế vị trí`i`thay đổi chi phí bằng cách$$|p_i-n|-|p_i+1|$$1. Chi phí tốt nhất cho gia đình này là$$\text{base} + \min_i \left(|p_i-n|-|p_i+1|\right)$$1. Lấy mức tối thiểu trong số tất cả các ứng viên hợp lệ. 

### Tại sao nó hoạt động 

Định lý phân loại quy giản phạm vi các mảng tốt thành một số họ không đổi. Mỗi mảng tốt đều thuộc về một trong những họ đó và mỗi họ mà chúng tôi đánh giá đều bao gồm toàn bộ các mảng tốt. Bước tối ưu hóa tính toán khoảng cách tối thiểu chính xác đến từng họ. Vì mọi mảng tốt có thể đều được bao phủ và không có ứng cử viên nào bị bỏ qua, nên khoảng cách tối thiểu trong số các khoảng cách đó chính xác là câu trả lời được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))

        if n == 1:
            ans.append(str(abs(p[0] - p[1])))
            continue

        res = sum(abs(x) for x in p)

        if n == 2:
            res = min(res, sum(abs(x - 2) for x in p))

        if n % 2 == 0:
            base = sum(abs(x + 1) for x in p)
            best_delta = min(abs(x - n) - abs(x + 1) for x in p)
            res = min(res, base + best_delta)

        ans.append(str(res))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Nhánh đầu tiên xử lý các vấn đề đặc biệt`n = 1`trường hợp. Giá trị bằng nhau tối ưu không cần phải tính toán rõ ràng vì khoảng cách tối thiểu giữa hai số và mục tiêu chung chính xác là chênh lệch tuyệt đối của chúng. 

Đối với lớn hơn`n`, câu trả lời được khởi tạo với chi phí biến đổi mọi thứ thành 0. 

các`n = 2`nhánh đánh giá mảng tốt cả hai bổ sung. 

Thậm chí-`n`chi nhánh đánh giá gia đình bằng một`n`và tất cả các giá trị còn lại bằng`-1`. Việc thực hiện tránh thử từng vị trí riêng biệt. Biến`base`đảm nhận mọi vị trí đều trở thành`-1`. Khi đó chúng ta chỉ cần vị trí rẻ nhất để nâng cấp từ`-1`ĐẾN`n`, đó chính xác là biểu thức delta tối thiểu. 

Tất cả số học đều phù hợp thoải mái với số nguyên Python. Khoảng cách có thể đạt khoảng$2 \cdot 10^5 \cdot 10^9$, vượt xa giới hạn 32 bit, nhưng Python tự động xử lý việc này. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 2```
n = 2
p = [1, 2, 2, 1]
```| Ứng viên | Chi phí | 
| --- | --- | 
| tất cả số không | 6 | 
| tất cả hai | 2 | 
| ba -1 và một 2 | 8 | 

Tối thiểu là`2`. 

Ví dụ này cho thấy tại sao đặc biệt`n = 2`ứng viên không thể bỏ qua. Mảng tốt tối ưu là`[2,2,2,2]`. 

### Đầu vào mẫu 4```
n = 4
p = [-3,-2,-1,0,1,2,3,4]
```| Số lượng | Giá trị | 
| --- | --- | 
| chi phí cho tất cả các số không | 16 | 
| cơ sở = Σ | pi+1 | 
| đồng bằng tốt nhất | -7 | 
| chi phí gia đình đặc biệt | 13 | 

Câu trả lời là```
min(16, 13) = 13
```Ví dụ này chứng tỏ sự đồng đều`n`gia đình. Mảng hoàn toàn bằng 0 là không tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi bài kiểm tra sẽ quét mảng một số lần không đổi | 
| Không gian | O(1) | Chỉ một số tiền đang chạy được lưu trữ | 

Tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2 · 10^5`, do đó, thời gian chạy tổng thể là tuyến tính theo kích thước đầu vào và dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))

        if n == 1:
            out.append(str(abs(p[0] - p[1])))
            continue

        res = sum(abs(x) for x in p)

        if n == 2:
            res = min(res, sum(abs(x - 2) for x in p))

        if n % 2 == 0:
            base = sum(abs(x + 1) for x in p)
            best_delta = min(abs(x - n) - abs(x + 1) for x in p)
            res = min(res, base + best_delta)

        out.append(str(res))

    return "\n".join(out)

# provided samples
assert run(
"""4
1
6 9
2
1 2 2 1
2
-2 -2 2 2
4
-3 -2 -1 0 1 2 3 4
"""
) == """3
2
5
13"""

# minimum size
assert run(
"""1
1
5 5
"""
) == "0"

# n=2, all-twos candidate
assert run(
"""1
2
2 2 2 2
"""
) == "0"

# odd n, only zeros matter
assert run(
"""1
3
1 1 1 1 1 1
"""
) == "6"

# even n, special family already achieved
assert run(
"""1
4
-1 -1 -1 -1 -1 -1 -1 4
"""
) == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1`,`[5,5]`|`0`| Vỏ kích thước tối thiểu | 
|`n=2`, tất cả hai |`0`| Gia đình hai người đặc biệt | 
|`n=3`, tất cả những cái |`6`| Số lẻ`n`chỉ sử dụng ứng cử viên hoàn toàn bằng 0 | 
|`n=4`, bảy`-1`và một`4`|`0`| Thậm chí-`n`gia đình đặc biệt | 

## Vỏ cạnh 

### Trường hợp 1:`n = 1`đầu vào:```
1
1
6 9
```Các mảng hợp lệ duy nhất là`[x,x]`. 

Thuật toán ngay lập tức trả về:$$|6-9| = 3$$Không có nhánh nào khác được thực thi. 

### Trường hợp 2: Chẵn`n`nơi gia đình đặc biệt là tối ưu 

đầu vào:```
1
4
-1 -1 -1 -1 -1 -1 -1 4
```Chi phí của mảng bằng 0 là:$$7 \cdot 1 + 4 = 11$$Dành cho gia đình đặc biệt,```
base = 5
best_delta = -5
```đưa ra tổng chi phí`0`. 

Thuật toán nhận dạng chính xác rằng đầu vào đã là một mảng tốt. 

### Trường hợp 3: Lẻ`n`đầu vào:```
1
3
-2 -2 2 2 0 0
```Đối với số lẻ`n ≥ 3`, mảng tốt duy nhất là tất cả số không. 

Thuật toán tính toán:$$2+2+2+2+0+0=8$$và trả về`8`. 

Thậm chí không-`n`ứng cử viên được xem xét, đó chính xác là những gì phân loại yêu cầu.

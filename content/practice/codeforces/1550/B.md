---
title: "CF 1550B - Xóa chi phí tối đa"
description: "Chúng ta được cung cấp một chuỗi nhị phân và liên tục loại bỏ các chuỗi con chỉ bao gồm một loại ký tự. Mỗi lần xóa một chuỗi con có độ dài l sẽ cho một điểm l + b. Chuỗi co lại sau mỗi thao tác vì các phần còn lại được nối với nhau."
date: "2026-06-10T13:27:42+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1550
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 111 (Rated for Div. 2)"
rating: 1000
weight: 1550
solve_time_s: 219
verified: true
draft: false
---

[CF 1550B - Xóa chi phí tối đa](https://codeforces.com/problemset/problem/1550/B) 

**Đánh giá:** 1000 
**Tags:** tham lam, toán học 
**Thời gian giải:** 3 phút 39 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân và liên tục loại bỏ các chuỗi con chỉ bao gồm một loại ký tự. Mỗi lần xóa một chuỗi con có độ dài`l`cho`a * l + b`điểm. Chuỗi co lại sau mỗi thao tác vì các phần còn lại được nối với nhau. 

Mục đích không phải là tìm một chuỗi bị xóa mà là để tính tổng số điểm tối đa có thể có sau khi toàn bộ chuỗi bị xóa. 

Điều đầu tiên cần chú ý là mỗi ký tự sẽ bị xóa đúng một lần. Cho dù chúng ta chọn phép toán như thế nào thì tổng của tất cả các độ dài đã xóa luôn là`n`. Vì vậy, sự đóng góp từ`a`đã được sửa:`a * n`Phần duy nhất bị ảnh hưởng bởi các lựa chọn của chúng tôi là số lượng thao tác xóa chúng tôi thực hiện. Nếu chúng ta biểu diễn`k`xóa, tổng số điểm sẽ trở thành:`a * n + b * k`Quan sát này thay đổi hoàn toàn vấn đề. Chúng tôi không còn quan tâm đến độ dài chính xác của chuỗi con bị xóa. Chúng ta chỉ quan tâm tới việc cực đại hóa hay cực tiểu hóa số phép toán, tùy thuộc vào dấu của`b`. 

Những hạn chế là rất nhỏ. Độ dài chuỗi tối đa là 100 và có tối đa 2000 trường hợp thử nghiệm. Ngay cả những giải pháp khá kém hiệu quả cũng sẽ phù hợp một cách thoải mái. Tuy nhiên, giải pháp dự định là một quan sát tham lam đơn giản chạy theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

Một số trường hợp đặc biệt rất dễ xử lý sai nếu chúng ta chỉ tập trung vào việc xóa thay vì công thức tính điểm. 

Coi như:```
n = 5
a = 3
b = 4
s = 11111
```Một giải pháp bất cẩn có thể xóa toàn bộ chuỗi trong một thao tác và ghi điểm`15 + 4 = 19`. Câu trả lời đúng là`5 * (3 + 4) = 35`, bởi vì khi`b > 0`, mỗi thao tác bổ sung sẽ cho thêm điểm, vì vậy xóa từng ký tự một là tối ưu. 

Coi như:```
n = 6
a = 1
b = -5
s = 000000
```Xóa một ký tự cùng một lúc sẽ thực hiện sáu thao tác và phát sinh phần thưởng âm sáu lần. Chiến lược tối ưu là xóa toàn bộ chuỗi cùng một lúc, đưa ra:```
1 * 6 + (-5) = 1
```Một trường hợp tinh tế khác là một chuỗi xen kẽ:```
n = 5
a = 0
b = -1
s = 01010
```Thoạt nhìn có vẻ như chúng tôi buộc phải thực hiện năm lần xóa vì có năm lần chạy. Trên thực tế, sau khi xóa tất cả các lần xuất hiện của một loại ký tự, các ký tự còn lại sẽ hợp nhất thành một khối. Câu trả lời tối ưu đến từ công thức đếm số lượt chạy được thảo luận sau và bằng`-3`, không`-5`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các thao tác xóa hợp lệ có thể có và khám phá đệ quy các chuỗi kết quả. Điều này đúng vì mọi trình tự hoạt động hợp pháp đều được kiểm tra. Thật không may, số lượng trình tự xóa có thể tăng theo cấp số nhân. Ngay cả đối với các chuỗi có độ dài 100, không gian tìm kiếm vẫn rất lớn và hoàn toàn không khả thi. 

Quan sát quan trọng là tổng đóng góp từ độ dài là cố định. Mỗi nhân vật đóng góp chính xác`a`một lần, bất kể lệnh xóa. Toàn bộ vấn đề tối ưu hóa giảm xuống việc chọn số lượng hoạt động. 

Điểm có thể được viết lại như sau:```
a * n + b * k
```Ở đâu`k`là số lần xóa. 

Nếu như`b >= 0`, mỗi lần xóa bổ sung sẽ làm tăng điểm. Chiến lược tốt nhất là tối đa hóa`k`. Vì bản thân mỗi ký tự đều là một chuỗi con đơn sắc hợp lệ nên chúng ta có thể xóa từng ký tự một và đạt được:```
k = n
```Trường hợp thú vị là`b < 0`. Bây giờ mọi thao tác đều tốn điểm, vì vậy chúng tôi muốn có càng ít thao tác xóa càng tốt. 

Cho chuỗi gồm có`runs`khối tối đa của các ký tự bằng nhau. 

Đối với chuỗi nhị phân, trước tiên chúng ta luôn có thể xóa tất cả các dòng của một loại ký tự. Sau đó, tất cả các lần chạy còn lại sẽ hợp nhất với nhau thành một lần chạy duy nhất và có thể bị xóa trong một thao tác cuối cùng. 

Nếu có`runs`các khối thì chỉ xuất hiện tối đa một ký tự`runs / 2`khối. Chúng tôi chọn nhân vật có ít khối hơn. 

Số lượng hoạt động tối thiểu có thể trở thành:```
floor(runs / 2) + 1
```Một công thức tương đương nổi tiếng là:```
max(count_0_runs, count_1_runs)
```Từ:```
max(count_0_runs, count_1_runs)
= floor(runs / 2) + 1
```cho bất kỳ chuỗi nhị phân nào. 

Khi chúng ta biết số lượng thao tác tối ưu, việc tính toán sẽ có câu trả lời ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`a`,`b`, và chuỗi nhị phân. 
2. Nếu`b >= 0`, trở lại:```
n * (a + b)
```Điều này tương ứng với việc xóa từng ký tự riêng biệt, đưa ra chính xác`n`hoạt động. 
3. Nếu`b < 0`, đếm số lần chạy trong chuỗi. 

Một cuộc chạy bắt đầu ở vị trí`0`và bất cứ khi nào`s[i] != s[i-1]`. 
4. Tính toán:```
operations = runs // 2 + 1
```Đây là số lần xóa tối thiểu có thể đạt được đối với một chuỗi nhị phân. 
5. Trở về:```
a * n + b * operations
```Sau khi đếm số lượt chạy, câu trả lời sẽ được đưa ra trực tiếp từ công thức tính điểm. 

### Tại sao nó hoạt động 

Tổng chiều dài bị xóa trong toàn bộ quá trình luôn là`n`, vì vậy mọi chiến lược đều kiếm được chính xác`a * n`điểm từ thời hạn dài. Phần thay đổi duy nhất là`b`nhân với số lần thực hiện. 

Khi`b >= 0`, tăng số lượng thao tác luôn cải thiện điểm số. Xóa từng ký tự một sẽ đạt được giá trị tối đa có thể`k = n`. 

Khi`b < 0`, ít thao tác hơn thì tốt hơn. Trong một chuỗi nhị phân, việc xóa tất cả các dòng có ký tự ít thường xuyên hơn trước tiên sẽ khiến các dòng còn lại hợp nhất thành một khối. Nếu có`runs`tổng số khối thì ký tự chiếm ít thường xuyên hơn`floor(runs / 2)`chạy. Những lần chạy đó sẽ bị xóa riêng lẻ và phần còn lại đã hợp nhất sẽ bị xóa một lần nữa, mang lại`floor(runs / 2) + 1`hoạt động. Không có chiến lược nào có thể làm tốt hơn, vì vậy đây là số lần xóa tối thiểu có thể. 

Vì thuật toán luôn sử dụng giá trị tối ưu của`k`, nó luôn tạo ra số điểm tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n, a, b = map(int, input().split())
    s = input().strip()

    if b >= 0:
        print(n * (a + b))
    else:
        runs = 1
        for i in range(1, n):
            if s[i] != s[i - 1]:
                runs += 1

        operations = runs // 2 + 1
        print(a * n + b * operations)
```Nhánh đầu tiên xử lý trường hợp dễ dàng trong đó mọi thao tác xóa thêm đều có lợi. Vì luôn cho phép xóa từng ký tự riêng lẻ nên số thao tác tối ưu là chính xác`n`. 

Nhánh thứ hai đếm lượt chạy. Một ranh giới chạy xảy ra bất cứ khi nào các ký tự liền kề khác nhau. Bắt đầu từ một lần chạy và đếm các lần chuyển đổi sẽ tránh được những sai sót nhỏ. 

Sau khi có được số lần chạy ta áp dụng công thức`runs // 2 + 1`để có thể xóa tối thiểu. Điểm cuối cùng sau đó được tính trực tiếp từ`a * n + b * operations`. 

Không cần xử lý đặc biệt đối với các chuỗi có độ dài bằng một. Bộ đếm chạy chính xác vẫn bằng 1, tạo ra một thao tác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
a = -2
b = 5
s = 11001
```Từ`b > 0`, chúng ta sử dụng ngay trường hợp đầu tiên. 

| Biến | Giá trị | 
| --- | --- | 
| n | 5 | 
| một | -2 | 
| b | 5 | 
| Chiến lược | tối đa hóa hoạt động | 
| Hoạt động | 5 | 
| Trả lời | 5 × (-2 + 5) = 15 | 

Ví dụ này chứng tỏ rằng khi`b`là dương, cấu trúc chuỗi không quan trọng chút nào. Chỉ có chiều dài quan trọng. 

### Ví dụ 2 

đầu vào:```
n = 6
a = 1
b = -4
s = 100111
```Đếm số lần chạy. 

| Vị trí | Nhân vật | Đếm lần chạy | 
| --- | --- | --- | 
| 0 | 1 | 1 | 
| 1 | 0 | 2 | 
| 2 | 0 | 2 | 
| 3 | 1 | 3 | 
| 4 | 1 | 3 | 
| 5 | 1 | 3 | 

Bây giờ tính toán câu trả lời. 

| Biến | Giá trị | 
| --- | --- | 
| chạy | 3 | 
| hoạt động | 3 // 2 + 1 = 2 | 
| một × n | 6 | 
| b × hoạt động | -8 | 
| Trả lời | -2 | 

Dấu vết này cho thấy ý tưởng tham lam then chốt. Mặc dù chuỗi chứa ba lần chạy nhưng chỉ cần xóa hai lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần quét chuỗi để đếm số lần chạy | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Với`n ≤ 100`và nhiều nhất`2000`các trường hợp thử nghiệm, giải pháp chỉ thực hiện tổng cộng vài trăm nghìn ký tự. Nó thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n, a, b = map(int, input().split())
        s = input().strip()

        if b >= 0:
            ans.append(str(n * (a + b)))
        else:
            runs = 1
            for i in range(1, n):
                if s[i] != s[i - 1]:
                    runs += 1
            ans.append(str(a * n + b * (runs // 2 + 1)))

    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue()

# provided samples
assert run(
"""3
3 2 0
000
5 -2 5
11001
6 1 -4
100111
"""
) == "6\n15\n-2"

# minimum size
assert run(
"""1
1 3 -2
0
"""
) == "1"

# all equal, negative b
assert run(
"""1
6 1 -5
000000
"""
) == "1"

# alternating pattern
assert run(
"""1
5 0 -1
01010
"""
) == "-3"

# maximum operations beneficial
assert run(
"""1
4 -1 10
1010
"""
) == "36"

# length 100, all same
assert run(
"""1
100 1 -100
""" + "0" * 100 + "\n"
) == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, s=0, a=3, b=-2`|`1`| Chuỗi kích thước tối thiểu | 
|`000000, b<0`|`1`| Chạy một lần, xóa một lần | 
|`01010, b=-1`|`-3`| Chạy xen kẽ và chạy công thức | 
|`1010, b>0`|`36`| Tiền thưởng tích cực, tối đa hóa hoạt động | 
| Độ dài 100 tất cả số không |`0`| Chiều dài lớn nhất trong một lần chạy | 

## Vỏ cạnh 

### Chuỗi ký tự đơn 

đầu vào:```
1
1 3 -2
0
```Chuỗi có một lần chạy.```
runs = 1
operations = 1 // 2 + 1 = 1
answer = 3 * 1 + (-2) * 1 = 1
```Thuật toán xử lý chính xác đầu vào nhỏ nhất có thể mà không yêu cầu bất kỳ trường hợp đặc biệt nào. 

### Tất cả các ký tự đều bằng nhau 

đầu vào:```
1
6 1 -5
000000
```Có chính xác một lần chạy.```
runs = 1
operations = 1
answer = 6 - 5 = 1
```Một chiến lược đơn giản là xóa từng ký tự riêng lẻ sẽ thực hiện sáu thao tác và mất thêm điểm. Công thức xác định chính xác rằng một lần xóa là tối ưu. 

### Chuỗi xen kẽ 

đầu vào:```
1
5 0 -1
01010
```Đếm lần chạy mang lại:```
runs = 5
operations = 5 // 2 + 1 = 3
answer = -3
```Mặc dù ban đầu có năm lần chạy, nhưng việc xóa tất cả các lần chạy thuộc một loại ký tự trước tiên sẽ cho phép các lần chạy còn lại hợp nhất. Đây chính xác là tình huống mà nhiều giải pháp không chính xác đánh giá quá cao số lượng thao tác tối thiểu. 

### Phần thưởng tích cực sau một lần chạy 

đầu vào:```
1
5 2 3
11111
```Từ`b > 0`, thuật toán bỏ qua cấu trúc chạy và tính toán:```
answer = 5 * (2 + 3) = 25
```Xóa một ký tự cùng một lúc sẽ mang lại năm thao tác, tốt hơn là xóa toàn bộ chuỗi cùng một lúc. Nhánh tiền thưởng tích cực sẽ tự động nắm bắt hành vi này.

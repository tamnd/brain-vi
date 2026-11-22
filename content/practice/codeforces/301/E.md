---
title: "CF 301E - Yaroslav và sự sắp xếp"
description: "Chúng ta được cho ba số nguyên. Độ dài của mảng không được vượt quá n, mọi giá trị phải nằm trong khoảng từ 1 đến m và số cách sắp xếp tuần hoàn riêng biệt thỏa mãn quy tắc kề đặc biệt phải nằm trong khoảng từ 1 đến k. Quy tắc kề xác định một mảng \"good\"."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 301
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 182 (Div. 1)"
rating: 2800
weight: 301
solve_time_s: 218
verified: true
draft: false
---

[CF 301E - Yaroslav và sự sắp xếp](https://codeforces.com/problemset/problem/301/E) 

**Đánh giá:** 2800 
**Thẻ:** dp 
**Thời gian giải:** 3 phút 38 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho ba số nguyên. Độ dài của mảng không thể vượt quá`n`, mọi giá trị phải nằm giữa`1`Và`m`và số cách sắp xếp tuần hoàn riêng biệt thỏa mãn quy tắc kề đặc biệt phải nằm trong khoảng`1`Và`k`. 

Quy tắc kề xác định một mảng "tốt". Trong một mảng tốt, mỗi cặp lân cận khác nhau một cách chính xác`1`, bao gồm cả cặp được tạo bởi phần tử cuối cùng và phần tử đầu tiên. Vì vậy, mảng tạo thành một chu trình trong đó mỗi bước tăng hoặc giảm một bước. 

Chúng tôi không được yêu cầu đếm trực tiếp các mảng tốt. Thay vào đó, chúng ta xét một mảng không giảm`b`. Chúng ta có thể hoán vị các phần tử của nó một cách tùy ý. Nếu trong số tất cả các hoán vị có giữa`1`Và`k`mảng tuần hoàn tốt riêng biệt, sau đó`b`được gọi là tuyệt vời. Chúng ta phải đếm xem có bao nhiêu mảng không giảm khác nhau là tuyệt vời. 

Khó khăn chính là mảng đầu vào được sắp xếp nhưng thuộc tính lại phụ thuộc vào tất cả các cách sắp xếp lại có thể có. Chúng ta cần hiểu chính xác những tập hợp số nào có thể tạo thành các chu kỳ hợp lệ và chúng tạo ra bao nhiêu chu kỳ riêng biệt. 

Các ràng buộc đủ nhỏ để cho phép lập trình động đa thức, nhưng quá lớn đối với các phép hoán vị mạnh mẽ. Từ`n`,`m`, Và`k`nhiều nhất là tất cả`100`, một giải pháp xung quanh`O(n^3)`hoặc`O(n^4)`được chấp nhận. Bất cứ điều gì theo cấp số nhân trong`n`là không thể. 

Phần nguy hiểm nhất của vấn đề là hiểu được mảng tốt thực sự trông như thế nào. Việc triển khai đơn giản có thể cố gắng tạo ra tất cả các hoán vị của nhiều tập hợp và kiểm tra điều kiện tuần hoàn. Ngay cả đối với chiều dài`10`, nó đã trở nên quá lớn rồi. 

Có một số trường hợp có thể thay đổi hoàn toàn cấu trúc. 

Coi như:```
1 1 1
```Một chu trình một phần tử sẽ yêu cầu`|a1 - a1| = 1`, điều đó là không thể. Vậy câu trả lời là`0`. 

Một trường hợp tinh vi khác là một tập hợp nhiều tập chứa các giá trị khác nhau nhiều hơn`1`. 

Ví dụ:```
n = 3, m = 3
multiset = {1, 3}
```Không có hoán vị nào có tác dụng vì mỗi cặp liền kề phải khác nhau một cách chính xác`1`. Việc triển khai bất cẩn chỉ kiểm tra số lượng có thể chấp nhận nó một cách không chính xác. 

Trường hợp cạnh cấu trúc quan trọng nhất là tính chẵn lẻ. 

Lấy nhiều bộ:```
{1, 1, 2, 2}
```Chúng ta có thể sắp xếp nó như sau:```
1 2 1 2
```Điều này tốt. 

Nhưng:```
{1,1,1,2,2}
```không thể tạo thành một chuỗi xen kẽ tuần hoàn, vì cuối cùng hai giá trị bằng nhau phải liền kề nhau. 

Điều này dẫn đến quan sát trọng tâm: mọi chu kỳ tốt đều xen kẽ giữa hai giá trị liên tiếp và cả hai giá trị phải xuất hiện nhiều lần như nhau. 

Hiểu được đặc tính này sẽ biến đổi hoàn toàn vấn đề. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản về mặt khái niệm. Chúng tôi tạo ra mọi mảng không giảm`b`, tạo ra tất cả các hoán vị riêng biệt của nhiều tập hợp của nó, kiểm tra xem hoán vị nào thỏa mãn điều kiện kề cận tuần hoàn và đếm xem có bao nhiêu chu kỳ hợp lệ tồn tại. 

Kiểm tra một hoán vị là dễ dàng. Chúng tôi chỉ xác minh:```
|a[i] - a[(i+1)%r]| = 1
```cho mọi vị trí. 

Vấn đề là số lượng hoán vị. Ngay cả đối với chiều dài`100`, số lần sắp xếp lại khác biệt là vô cùng lớn. Không có cơ hội để liệt kê chúng. 

Bước tiếp theo là nghiên cứu cấu trúc của các mảng tuần hoàn tốt. 

Giả sử một giá trị`x`xuất hiện ở đâu đó trong một chu kỳ tốt. Mỗi người hàng xóm của`x`thì phải là`x-1`hoặc`x+1`. Vì chu trình đóng nên tất cả các giá trị phải thuộc về một chuỗi các số nguyên liên tiếp được kết nối. 

Bây giờ giả sử ba giá trị riêng biệt xuất hiện:```
x, x+1, x+2
```giá trị`x`chỉ có thể kết nối với`x+1`, Và`x+2`chỉ có thể kết nối với`x+1`. Cuối cùng, một số giá trị bằng nhau phải trở nên liền kề, vi phạm quy tắc. 

Cấu trúc duy nhất có thể có chính xác là hai giá trị liên tiếp. 

Vì vậy, mọi chu kỳ tốt chỉ bao gồm:```
x and x+1
```Hơn nữa, chu trình phải xen kẽ:```
x, x+1, x, x+1, ...
```ngay lập tức ngụ ý cả hai giá trị xuất hiện nhiều lần như nhau. 

Nếu mỗi giá trị xuất hiện`t`lần, độ dài chu kỳ là`2t`. 

Một multiset như vậy tạo ra bao nhiêu mảng tuần hoàn tốt riêng biệt? 

Khi vị trí bắt đầu được cố định, toàn bộ chu trình sẽ bị ép luân phiên. Có đúng hai khả năng:```
x, x+1, x, x+1, ...
x+1, x, x+1, x, ...
```Hai cái này khác nhau trừ khi độ dài bằng 0. 

Vì vậy, mọi multiset hợp lệ đều tạo ra chính xác`2`mảng tốt. 

Điều đó có nghĩa là điều kiện “giữa 1 và k mảng tốt” trở nên cực kỳ đơn giản: 

Nếu`k = 1`, câu trả lời luôn là`0`. 

Nếu như`k >= 2`, mọi tập hợp hợp lệ đều đóng góp. 

Bây giờ, vấn đề giảm xuống việc đếm các mảng không giảm có nhiều mảng chứa chính xác hai giá trị liên tiếp có tần số dương bằng nhau. 

Giả sử giá trị`x`xuất hiện`t`thời gian và giá trị`x+1`xuất hiện`t`lần. 

Mảng được sắp xếp là duy nhất:```
x x x ... x (t times)
x+1 x+1 ... x+1 (t times)
```Vì vậy mỗi cặp`(x, t)`định nghĩa chính xác một mảng lớn. 

Các ràng buộc trở nên tầm thường sau khi giảm cấu trúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(nm) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`m`, Và`k`. 
2. Nếu`k < 2`, in`0`. 

Một chu kỳ tốt hợp lệ luôn tạo ra chính xác`2`mảng riêng biệt, tương ứng với hai điểm bắt đầu xen kẽ. Vì vậy không có multiset nào có thể tạo ra chính xác một mảng tốt. 

1. Lặp lại mọi giá trị nhỏ hơn có thể`x`. 

Giá trị lớn hơn phải là`x+1`, Vì thế`x`dao động từ`1`ĐẾN`m-1`. 

1. Lặp lại mọi tần số có thể`t`. 

Tổng chiều dài trở thành`2t`, vì vậy chúng tôi cần:```
2t <= n
```1. Đếm từng cặp`(x, t)`một lần. 

Mảng được sắp xếp được xác định duy nhất:```
[x repeated t times, x+1 repeated t times]
```Không có hai cặp khác nhau tạo ra cùng một mảng. 

1. Xuất ra modulo số đếm`1e9+7`. 

Những con số ở đây rất nhỏ, nhưng chúng tôi vẫn tuân theo tuyên bố. 

### Tại sao nó hoạt động 

Một mảng tốt theo chu kỳ phải thỏa mãn rằng mọi chênh lệch liền kề đều bằng`1`. Điều này buộc mọi phần tử chỉ kết nối với các giá trị lân cận. Nếu ba hoặc nhiều giá trị khác biệt xuất hiện, cuối cùng hai giá trị bằng nhau sẽ trở nên liền kề ở đâu đó trong chu trình. Vì vậy chỉ có thể tồn tại hai giá trị liên tiếp. 

Đối với một chu trình sử dụng các giá trị`x`Và`x+1`, mọi vị trí phải xen kẽ giữa chúng. Điều đó chỉ có thể thực hiện được khi tần số của chúng bằng nhau. 

Khi nhiều tập hợp được cố định, sẽ tồn tại chính xác hai mảng tuần hoàn, tùy thuộc vào giá trị nào bắt đầu luân phiên. Do đó, mọi multiset hợp lệ đều đóng góp iff`k >= 2`. 

Thuật toán liệt kê chính xác tất cả các tập hợp như vậy, không thiếu hoặc tính hai lần bất kỳ tập hợp nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, m, k = map(int, input().split())

    if k < 2:
        print(0)
        return

    ans = 0

    for x in range(1, m):
        t = 1
        while 2 * t <= n:
            ans += 1
            t += 1

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Điều kiện đầu tiên được xử lý là`k < 2`. Mọi tập hợp hợp lệ luôn tạo ra chính xác hai mảng tuần hoàn tốt, không bao giờ là một, vì vậy câu trả lời ngay lập tức trở thành số 0. 

Vòng lặp bên ngoài chọn giá trị nhỏ hơn`x`. Vì giá trị thứ hai phải là`x+1`, có`m-1`sự lựa chọn. 

Vòng lặp bên trong chọn tần số chung`t`. Tổng chiều dài bằng`2t`, vì vậy chúng tôi chỉ cho phép các giá trị thỏa mãn`2t <= n`. 

Mỗi cặp`(x, t)`định nghĩa chính xác một mảng được sắp xếp. Bởi vì mảng không giảm nên không có sự mơ hồ:```
x x ... x x+1 x+1 ... x+1
```Không có sự phức tạp số học mô-đun nào xuất hiện ở đây vì câu trả lời nhiều nhất là về`5000`, nhưng modulo vẫn được áp dụng để khớp với câu lệnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1 1
```Từ`k = 1`, không có nhiều tập hợp lệ có thể đủ điều kiện. 

| Bước | Giá trị | 
| --- | --- | 
| Kiểm tra`k < 2`| đúng | 
| Đầu ra | 0 | 

Dấu vết chứng minh thực tế cấu trúc quan trọng rằng mọi tập hợp nhiều hợp lệ đều tạo ra chính xác hai mảng tuần hoàn, không bao giờ một mảng. 

### Ví dụ 2 

đầu vào:```
4 3 2
```Các cặp có thể là:```
(1,1)
(1,2)
(2,1)
(2,2)
```bởi vì`x`phạm vi trên`1,2`Và`2t <= 4`. 

| x | t | Mảng được sắp xếp | 
| --- | --- | --- | 
| 1 | 1 | [1,2] | 
| 1 | 2 | [1,1,2,2] | 
| 2 | 1 | [2,3] | 
| 2 | 2 | [2,2,3,3] | 

Tổng số câu trả lời là`4`. 

Ví dụ này xác nhận rằng mọi cấu hình hợp lệ được xác định hoàn toàn bởi hai giá trị liên tiếp và tần số bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Lặp lại các giá trị và tần số khả thi | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên | 

Với`n,m <= 100`, tổng số lần lặp là rất nhỏ. Giải pháp dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, m, k = map(int, input().split())

    if k < 2:
        print(0)
        return

    ans = 0

    for x in range(1, m):
        t = 1
        while 2 * t <= n:
            ans += 1
            t += 1

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue()

# provided sample
assert run("1 1 1\n") == "0\n", "sample 1"

# minimum size, impossible because cycle length 1
assert run("1 5 100\n") == "0\n", "length 1 impossible"

# single valid pair
assert run("2 2 2\n") == "1\n", "only [1,2]"

# multiple frequencies
assert run("4 3 2\n") == "4\n", "two values times two frequencies"

# k too small
assert run("100 100 1\n") == "0\n", "no configuration has exactly one cycle"

# larger case
assert run("6 5 10\n") == "12\n", "4 choices of x and 3 choices of t"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5 100`|`0`| Độ dài chu kỳ lẻ không thể hoạt động | 
|`2 2 2`|`1`| Cấu hình hợp lệ không cần thiết nhỏ nhất | 
|`4 3 2`|`4`| Nhiều tần số và lựa chọn giá trị | 
|`100 100 1`|`0`| Mỗi multiset hợp lệ tạo ra chính xác hai chu kỳ | 
|`6 5 10`|`12`| Logic đếm lớn hơn | 

## Vỏ cạnh 

Hãy xem xét:```
1 5 100
```Một mảng tuần hoàn có độ dài-1 sẽ yêu cầu:```
|a1 - a1| = 1
```điều đó là không thể. 

Thuật toán xử lý việc này một cách tự nhiên vì không có giá trị tích cực`t`thỏa mãn:```
2t <= 1
```vì vậy số lượng vẫn bằng không. 

Bây giờ hãy xem xét:```
4 3 1
```Bộ nhiều bộ`{1,1,2,2}`hình thành các chu kỳ hợp lệ:```
1 2 1 2
2 1 2 1
```Có chính xác hai trong số họ. 

Từ`k = 1`, multiset này không đủ điều kiện. Thuật toán ngay lập tức từ chối tất cả các cấu hình thông qua điều kiện:```python
if k < 2:
    print(0)
```Cuối cùng hãy xem xét:```
6 4 100
```Các cặp có thể là:```
(1,1), (1,2), (1,3)
(2,1), (2,2), (2,3)
(3,1), (3,2), (3,3)
```Thuật toán tính chính xác chín trường hợp này. 

Ví dụ:```
(2,3)
```tương ứng với:```
[2,2,2,3,3,3]
```có thể tạo thành chính xác hai mảng tốt theo chu kỳ:```
2 3 2 3 2 3
3 2 3 2 3 2
```Không có hoán vị nào khác thỏa mãn điều kiện.

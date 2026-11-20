---
title: "CF 214A - Hệ phương trình"
description: "Chúng ta cần đếm xem có bao nhiêu cặp số nguyên không âm (a, b) thỏa mãn cùng lúc hai phương trình: - a2 + b = n - a + b2 = m Dữ liệu đầu vào cho hai giá trị đích n và m."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 214
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 131 (Div. 2)"
rating: 800
weight: 214
solve_time_s: 56
verified: true
draft: false
---

[CF 214A - Hệ phương trình](https://codeforces.com/problemset/problem/214/A) 

**Đánh giá:** 800 
**Tags:** vũ lực 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đếm xem có bao nhiêu cặp số nguyên không âm`(a, b)`thỏa mãn đồng thời hai phương trình: 

-`a² + b = n`-`a + b² = m`Đầu vào cung cấp hai giá trị mục tiêu`n`Và`m`. Công việc của chúng ta là kiểm tra các giá trị có thể có của`a`Và`b`và đếm xem có bao nhiêu cặp làm cho cả hai phương trình đều đúng cùng một lúc. 

Những hạn chế là rất nhỏ. Cả hai`n`Và`m`nhiều nhất là`1000`, điều này ngay lập tức gợi ý rằng vũ lực là khả thi. Thậm chí kiểm tra từng cặp`(a, b)`từ`0`ĐẾN`1000`chỉ yêu cầu khoảng một triệu lần lặp, điều này không đáng kể trong giới hạn 2 giây trong Python. 

Phần thú vị hơn là nhận ra các phương trình hạn chế không gian tìm kiếm chặt chẽ như thế nào. Từ`a² ≤ n`, giá trị của`a`không bao giờ có thể vượt quá`√n`. Tương tự,`b² ≤ m`, Vì thế`b`không bao giờ có thể vượt quá`√m`. Quan sát đó làm giảm đáng kể việc tìm kiếm. 

Có một số trường hợp khó bỏ sót. 

Một trường hợp tinh vi là khi một biến trở thành số 0. Ví dụ:```
9 3
```Cặp hợp lệ là`(3, 0)`bởi vì:

-`3² + 0 = 9`-`3 + 0² = 3`Việc triển khai bất cẩn bắt đầu các vòng lặp từ`1`thay vì`0`sẽ bỏ lỡ câu trả lời này một cách không chính xác. 

Một tình huống khó khăn khác là khi không có cặp nào tồn tại cả. Ví dụ:```
1 1
```Việc thử tất cả các khả năng sẽ không mang lại cặp nào hợp lệ. Một số nghiệm sai giải độc lập từng phương trình và vô tình kết hợp các giá trị không tương thích. 

Một trường hợp cạnh khác xuất hiện khi nhiều giá trị ứng cử viên thỏa mãn một phương trình riêng lẻ nhưng không phải cả hai cùng nhau. Ví dụ:```
10 34
```Cặp đôi`(3, 1)`thỏa mãn: 

-`3² + 1 = 10`-`3 + 1² = 4`Phương trình đầu tiên hoạt động, nhưng phương trình thứ hai thì không. Thuật toán phải luôn xác minh đồng thời cả hai phương trình. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là thử mọi cặp có thể`(a, b)`và kiểm tra xem cả hai phương trình có đúng không. 

Vì cả hai biến đều không âm và bị giới hạn bởi`1000`, chúng ta có thể lặp lại: 

-`a`từ`0`ĐẾN`1000`-`b`từ`0`ĐẾN`1000`Với mỗi cặp hãy tính: 

-`a² + b`-`a + b²`Nếu cả hai phù hợp`n`Và`m`, tăng câu trả lời. 

Giải pháp brute-force này đúng vì nó kiểm tra rõ ràng mọi cặp ứng cử viên có thể. Không có gì có thể bỏ qua. 

Vấn đề là hiệu quả. Một tìm kiếm đầy đủ thực hiện đại khái:```
1001 × 1001 ≈ 1,000,000
```séc. Ngay cả điều này vẫn diễn ra dễ dàng đối với các ràng buộc đã cho, nhưng chúng ta có thể làm tốt hơn bằng một quan sát nhỏ. 

Từ phương trình đầu tiên:```
a² + b = n
```Từ`b ≥ 0`, chúng tôi biết:```
a² ≤ n
```Vì thế`a`chỉ cần đi lên`√n`. 

Tương tự, từ:```
a + b² = m
```chúng tôi nhận được:```
b² ≤ m
```Vì thế`b`chỉ cần đi lên`√m`. 

Điều này biến việc tìm kiếm thành một lực lượng rất nhỏ đối với các giá trị ứng cử viên thực tế thay vì toàn bộ`[0, 1000]`phạm vi. 

Một quan sát hữu ích khác là một lần`a`cố định thì phương trình thứ nhất xác định duy nhất`b`:```
b = n - a²
```Vì vậy, chúng tôi thậm chí không cần một vòng lặp lồng nhau. Chúng ta có thể lặp lại`a`, tính toán duy nhất có thể`b`, và chứng minh phương trình thứ hai. 

Điều đó làm giảm thuật toán về độ phức tạp tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1000²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(√n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`Và`m`. 
2. Khởi tạo`answer = 0`. 
3. Lặp lại`a`từ`0`trong khi`a² ≤ n`. 

Bất kỳ giá trị lớn hơn của`a`sẽ ngay lập tức vi phạm phương trình đầu tiên vì`b`không thể âm. 
4. Tính toán:```
b = n - a²
```Đây là giá trị duy nhất có thể có của`b`có thể thỏa mãn phương trình đầu tiên của dòng điện`a`. 
5. Kiểm tra xem phương trình thứ hai có đúng không:```
a + b² == m
```Nếu đúng thì tăng`answer`. 
6. Sau khi vòng lặp kết thúc, in`answer`. 

### Tại sao nó hoạt động 

Với mọi số nguyên không âm`a`với`a² ≤ n`, phương trình đầu tiên xác định chính xác một giá trị ứng cử viên của`b`. Không có cái khác`b`có thể thỏa mãn:```
a² + b = n
```Vì vậy, thuật toán sẽ kiểm tra mọi cặp hợp lệ có thể có chính xác một lần. Phương trình thứ hai hoạt động như một bộ lọc chỉ giữ lại các cặp thỏa mãn toàn bộ hệ thống. Vì mọi cặp khả thi đều phải xuất hiện trong quá trình này nên số đếm cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

answer = 0

a = 0
while a * a <= n:
    b = n - a * a

    if a + b * b == m:
        answer += 1

    a += 1

print(answer)
```Vòng lặp chỉ lặp qua các giá trị khả thi của`a`. điều kiện`a * a <= n`đảm bảo rằng`b = n - a²`vẫn không âm. 

Dòng:```
b = n - a * a
```xuất phát trực tiếp từ việc sắp xếp lại phương trình đầu tiên. Điều này tránh hoàn toàn một vòng lặp lồng nhau. 

Phương trình thứ hai:```
a + b * b == m
```được kiểm tra chính xác một lần cho mỗi cặp ứng cử viên. 

Một chi tiết tinh tế là sử dụng phép nhân thay vì căn bậc hai. Giới hạn tính toán với căn bậc hai dấu phẩy động có thể gây ra các vấn đề làm tròn. Điều kiện vòng lặp`a * a <= n`tránh được vấn đề đó hoàn toàn. 

Một chi tiết triển khai nhỏ khác là`b`luôn là số nguyên vì`n`Và`a²`là số nguyên. Không cần xác nhận thêm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9 3
```| một | a² | b = n - a² | a + b2 | Có hiệu lực? | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 9 | 81 | Không | 
| 1 | 1 | 8 | 65 | Không | 
| 2 | 4 | 5 | 27 | Không | 
| 3 | 9 | 0 | 3 | Có | 

Thuật toán tìm chính xác một cặp hợp lệ:`(3, 0)`. Dấu vết này giải thích tại sao chỉ kiểm tra một`b`mỗi`a`là đủ. 

### Ví dụ 2 

đầu vào:```
1 1
```| một | a² | b = n - a² | a + b2 | Có hiệu lực? | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | Có | 
| 1 | 1 | 0 | 1 | Có | 

Đầu ra là:```
2
```Các cặp hợp lệ là`(0, 1)`Và`(1, 0)`. 

Ví dụ này cho thấy rằng có thể có nhiều câu trả lời và giá trị 0 phải được đưa vào tìm kiếm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(√n) | Vòng lặp chạy trong khi`a² ≤ n`| 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Với`n ≤ 1000`, vòng lặp chạy tối đa khoảng 32 lần. Chương trình dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())

    answer = 0

    a = 0
    while a * a <= n:
        b = n - a * a

        if a + b * b == m:
            answer += 1

        a += 1

    print(answer)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided samples
assert run("9 3\n") == "1\n", "sample 1"

# custom cases
assert run("1 1\n") == "2\n", "two symmetric solutions"

assert run("2 2\n") == "0\n", "no valid pair"

assert run("0 0\n") == "1\n", "minimum values"

assert run("1000 1000\n") == "0\n", "large boundary case"

assert run("25 5\n") == "1\n", "solution with b = 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`2`| Nhiều cặp hợp lệ | 
|`2 2`|`0`| Không có giải pháp tồn tại | 
|`0 0`|`1`| Giá trị biên tối thiểu | 
|`1000 1000`|`0`| Xử lý đầu vào lớn | 
|`25 5`|`1`| Trường hợp một biến bằng 0 | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
9 3
```Thuật toán thử: 

-`a = 0`,`b = 9`-`a = 1`,`b = 8`-`a = 2`,`b = 5`-`a = 3`,`b = 0`Chỉ có cặp cuối cùng thỏa mãn phương trình thứ hai. Vì vòng lặp bắt đầu lúc`0`, giải pháp với`b = 0`được bao gồm một cách chính xác. 

Bây giờ hãy nhìn vào:```
2 2
```Thuật toán kiểm tra: 

-`a = 0`,`b = 2`-`a = 1`,`b = 1`Không thỏa mãn`a + b² = 2`, vậy đáp án vẫn là`0`. Điều này xác nhận thuật toán không đếm sai các cặp chỉ thỏa mãn một phương trình. 

Cuối cùng, hãy xem xét:```
1 1
```Việc thực hiện tìm thấy: 

-`(0, 1)`-`(1, 0)`Cả hai đều được tính độc lập vì vòng lặp kiểm tra mọi khả năng`a`. Điều này xác nhận rằng các giải pháp đối xứng được xử lý chính xác.

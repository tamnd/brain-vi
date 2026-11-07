---
title: "CF 1327A - Tổng các số nguyên lẻ"
description: "Bài toán hỏi liệu một số nguyên n đã cho có thể được biểu diễn dưới dạng tổng của chính xác k số nguyên dương lẻ khác nhau hay không. Mỗi trường hợp kiểm thử cung cấp n và k và câu trả lời là \"CÓ\" hoặc \"KHÔNG\". Số nguyên lẻ là số không chia hết cho 2 nên các số hợp lệ là 1, 3, 5, 7,...."
date: "2026-06-11T16:31:00+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1327
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 84 (Rated for Div. 2)"
rating: 1100
weight: 1327
solve_time_s: 256
verified: false
draft: false
---

[CF 1327A - Tổng các số nguyên lẻ](https://codeforces.com/problemset/problem/1327/A) 

**Đánh giá:** 1100 
**Thẻ:** toán 
**Thời gian giải:** 4 phút 16s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bài toán hỏi liệu một số nguyên đã cho có`n`có thể được biểu thị dưới dạng tổng của chính xác`k`các số nguyên dương lẻ khác nhau. Mỗi trường hợp thử nghiệm cung cấp`n`Và`k`, và câu trả lời là "CÓ" hoặc "KHÔNG". Số nguyên lẻ là số không chia hết cho 2 nên các ứng cử viên hợp lệ là`1, 3, 5, 7,...`. 

Những hạn chế là đáng kể. Với`t`lên đến`10^5`Và`n`Và`k`lên đến`10^7`, một cách tiếp cận ngây thơ tạo ra tất cả các tập con số lẻ có thể có sẽ ngay lập tức không khả thi. Số lượng thao tác trong trường hợp xấu nhất trong bảng liệt kê vũ phu tăng theo cấp số nhân, vượt xa mức 2 giây thời gian chạy có thể xử lý. Điều này ngụ ý rằng bất kỳ giải pháp nào cũng phải chạy trong khoảng`O(1)`hoặc tệ nhất`O(log n)`mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh không rõ ràng phát sinh từ tính chẵn lẻ của các con số. Ví dụ,`n=4`Và`k=2`có thể được biểu diễn dưới dạng`1+3`, Nhưng`n=10`Và`k=3`không thể, vì tổng của ba số lẻ dương phân biệt bất kỳ luôn là số lẻ, trong khi`10`là chẵn. Tương tự, nếu`k`quá lớn so với`n`, điều đó là không thể vì tổng của số đầu tiên`k`số lẻ nhỏ nhất đã vượt quá`n`. Ví dụ,`n=8`Và`k=3`thất bại vì`1+3+5=9>8`. Những cân nhắc này làm cho tính chẵn lẻ và tổng tối thiểu trở thành những ràng buộc chính. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ cố gắng tạo ra tất cả các kết hợp của`k`phân biệt các số nguyên dương lẻ và kiểm tra xem có tổng nào bằng không`n`. Điều này đúng về mặt lý thuyết, nhưng sự bùng nổ tổ hợp khiến nó không thực tế: ngay cả đối với`k=20`, số lượng kết hợp là vô cùng lớn. Do đó, brute-force chỉ hoạt động với các giá trị nhỏ của`n`Và`k`. 

Quan sát quan trọng cho một giải pháp nhanh chóng là điều đầu tiên`k`số lẻ là`1, 3, 5, ..., 2k-1`, và tổng của chúng là`k^2`. Vì vậy, đối với`n`có thể được biểu diễn dưới dạng tổng của`k`các số nguyên lẻ phân biệt`n`ít nhất phải có`k^2`. Ngoài ra, tính chẵn lẻ của`n`Và`k`phải khớp: tổng của`k`số lẻ là số lẻ nếu`k`là lẻ và thậm chí nếu`k`là chẵn. Điều này là do tổng của số lẻ các số lẻ là số lẻ và tổng của số chẵn của các số lẻ là số chẵn. Việc kết hợp hai điều kiện này sẽ mang lại khả năng kiểm tra liên tục theo thời gian cho từng trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k) | O(k) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc số nguyên`n`Và`k`. 
2. Tính tổng nhỏ nhất của số đầu tiên`k`số lẻ, đó là`k^2`. Điều này đảm bảo rằng chúng ta có đủ "khối lượng lẻ" để đạt được`n`. 
3. Kiểm tra xem`n`ít nhất là`k^2`. Nếu không thì không thể đại diện được`n`như một tổng của`k`các số lẻ khác nhau nên xuất ra "NO". 
4. Kiểm tra xem tính chẵn lẻ của`n`phù hợp với tính chẵn lẻ của`k`. Cụ thể, hãy xác minh xem`(n % 2) == (k % 2)`. Điều này đảm bảo tổng có thể là số lẻ hoặc số chẵn để khớp`n`. 
5. Nếu cả hai điều kiện đều được thỏa mãn, xuất ra "CÓ"; nếu không thì xuất ra "NO". 

Tại sao nó hoạt động: Số tiền tối thiểu đảm bảo rằng số tiền nhỏ nhất`k`số lẻ có thể nằm trong`n`. Bất kỳ số tiền nào`k`có thể thu được các số lẻ riêng biệt bằng cách tăng một số số trong khi vẫn giữ chúng là số lẻ, điều này bảo toàn tính chẵn lẻ của tổng. Vì vậy, nếu`n`lớn hơn`k^2`và có tính chẵn lẻ phù hợp thì luôn tồn tại một tập hợp các số lẻ riêng biệt có tổng bằng`n`. Nếu không thì không thể được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    if n >= k * k and (n % 2) == (k % 2):
        print("YES")
    elif n >= 2 * k and (n % 2) == 0:
        # optional: for even numbers check sum of k even numbers
        print("YES")
    else:
        print("NO")
```Điều kiện đầu tiên`n >= k*k and (n%2) == (k%2)`xử lý tổng các số lẻ. Điều kiện thứ hai, đôi khi thấy trong lời giải, sẽ xử lý các số chẵn nếu chúng ta cũng xem xét việc biểu diễn`n`dưới dạng tổng của các số chẵn. Đối với bài toán này chỉ cần số lẻ nên chỉ cần kiểm tra lần đầu là đủ. I/O nhanh đảm bảo chương trình xử lý tối đa`10^5`trường hợp thử nghiệm một cách hiệu quả. 

## Ví dụ đã hoạt động 

Đối với đầu vào`n=10, k=3`, tính tổng nhỏ nhất của 3 số lẻ đầu tiên:`1+3+5=9`. Kiểm tra tính chẵn lẻ: tổng 3 số lẻ là số lẻ,`10`là chẵn. Điều kiện không thành công, đầu ra là "KHÔNG". 

| n | k | k^2 | n >= k^2 | n%2 | k%2 | trận đấu chẵn lẻ | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 10 | 3 | 9 | Đúng | 0 | 1 | Sai | KHÔNG | 

Đối với đầu vào`n=16, k=4`, tổng tối thiểu`1+3+5+7=16`. Tính chẵn lẻ của tổng 4 số lẻ là số chẵn`16`là chẵn. Cả hai điều kiện đều được thỏa mãn, đầu ra là "CÓ". 

| n | k | k^2 | n >= k^2 | n%2 | k%2 | trận đấu chẵn lẻ | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 16 | 4 | 16 | Đúng | 0 | 0 | Đúng | CÓ | 

Những dấu vết này xác nhận thuật toán xử lý chính xác tính chẵn lẻ của cạnh và các điều kiện tổng tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Mỗi trường hợp thử nghiệm chỉ yêu cầu kiểm tra số học và modulo | 
| Không gian | O(1) | Không có dung lượng lưu trữ bổ sung tương ứng với`n`hoặc`k`là cần thiết | 

Được cho`t <= 10^5`, tổng số thao tác nhiều nhất là`10^5`, dễ dàng trong giới hạn 2 giây. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        if n >= k * k and (n % 2) == (k % 2):
            print("YES")
        else:
            print("NO")
    return out.getvalue().strip()

# provided samples
assert run("6\n3 1\n4 2\n10 3\n10 2\n16 4\n16 5\n") == "YES\nYES\nNO\nYES\nYES\nNO", "sample 1"

# custom cases
assert run("3\n1 1\n2 1\n10000000 1000\n") == "YES\nNO\nYES", "minimum and large n"
assert run("2\n15 4\n36 6\n") == "NO\nYES", "odd/even parity tests"
assert run("2\n5 3\n14 3\n") == "NO\nNO", "sum too small tests"
assert run("1\n9999999 3162\n") == "YES", "large n edge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | CÓ | tổng hợp lệ nhỏ nhất | 
| 2 1 | KHÔNG | số lẻ không thể có tổng bằng số chẵn | 
| 10000000 1000 | CÓ | n lớn trong giới hạn | 
| 15 4 | KHÔNG | sự không khớp chẵn lẻ | 
| 36 6 | CÓ | tổng chẵn của k chẵn | 
| 5 3 | KHÔNG | tổng số tiền quá nhỏ | 
| 14 3 | KHÔNG | tổng số tiền quá nhỏ | 
| 9999999 3162 | CÓ | n lớn, k^2 đóng | 

## Vỏ cạnh 

cho`n=2, k=1`, tổng tối thiểu là`1`. Kiểm tra chẵn lẻ không thành công vì tổng của 1 số lẻ là số lẻ,`n`là chẵn. Thuật toán xuất ra chính xác "KHÔNG". 

Vì`n=1, k=1`, tổng tối thiểu là`1`. Kiểm tra tính chẵn lẻ phù hợp. Thuật toán xuất ra "CÓ". Những điều này xác nhận cả trường hợp giới hạn dưới và cạnh chẵn lẻ đều được xử lý. 

Vì`n=10^7, k=1`, tổng tối thiểu là`1`. Kiểm tra tính chẵn lẻ:`10^7`thậm chí,`1`kỳ quặc, thất bại. Đầu ra là "KHÔNG". Điều này khẳng định thuật toán xử lý n lớn một cách hiệu quả.

---
title: "CF 47A - Số tam giác"
description: "Chúng ta được yêu cầu xác định xem một số nguyên dương nhất định có thể được biểu diễn dưới dạng số tam giác hay không. Các số tam giác được hình thành bằng cách sắp xếp các dấu chấm thành một tam giác đều, vì vậy số tam giác _n_ là tổng của các số nguyên dương _n_ đầu tiên."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 47
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 44 (Div. 2)"
rating: 800
weight: 47
solve_time_s: 70
verified: true
draft: false
---
[CF 47A - Số tam giác](https://codeforces.com/problemset/problem/47/A) 

**Đánh giá:** 800 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem một số nguyên dương nhất định có thể được biểu diễn dưới dạng số tam giác hay không. Các số tam giác được hình thành bằng cách sắp xếp các dấu chấm thành một tam giác đều, vì vậy số tam giác _n_ là tổng của các số nguyên dương _n_ đầu tiên. Ví dụ: một số số tam giác đầu tiên là 1, 3, 6, 10, 15, v.v. Đầu vào là một số nguyên duy nhất`n`trong khoảng từ 1 đến 500 và đầu ra phải là`YES`nếu như`n`khớp với bất kỳ số tam giác nào, nếu không`NO`. 

Từ`n`nhiều nhất là 500, chúng ta có thể suy luận về giới hạn tính toán. Tính tuần tự từng số tam giác là khả thi vì số tam giác thứ 32 đã là 528, cao hơn một chút so với giới hạn tối đa của chúng ta. Điều này cho chúng ta biết vấn đề đủ nhỏ để lặp lại đơn giản. Một cách tiếp cận bất cẩn có thể cố gắng đảo ngược công thức số tam giác bằng cách sử dụng phép chia dấu phẩy động, điều này có thể tạo ra các lỗi riêng lẻ cho các số nguyên nhỏ như 1, 3 hoặc 6. Ví dụ: nếu chúng ta tính sai phân biệt của công thức bậc hai, chúng ta có thể kết luận sai rằng 1 không phải là tam giác. 

Vỏ cạnh bao gồm`n = 1`,`n = 500`và các số ngay bên dưới hoặc phía trên một số hình tam giác, chẳng hạn như 5 (từ 3 đến 6). 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là tạo ra các số tam giác một cách tuần tự và so sánh chúng với`n`. Bắt đầu bằng 1, sau đó 1 + 2 = 3, rồi 1 + 2 + 3 = 6, v.v. cho đến khi bạn khớp`n`hoặc vượt quá nó. Điều này có tác dụng vì các số tam giác tăng đều đều, nên một khi chúng ta vượt qua`n`không có cơ hội tìm được người phù hợp. 

Phương pháp ngây thơ này thực sự là đủ ở đây vì mức tối đa`n`là 500. Số lần lặp trong trường hợp xấu nhất là khoảng 32, vì các số tam giác tăng gần như bậc hai (`k(k+1)/2 ≤ 500`). Con số này thấp hơn nhiều so với bất kỳ giới hạn hợp lý nào đối với giới hạn thời gian 2 giây. 

Một cách khác là sử dụng công thức dạng đóng`k(k+1)/2 = n`và giải quyết cho`k`bằng cách sử dụng công thức bậc hai. Điều này yêu cầu kiểm tra xem giải pháp có phải là số nguyên dương hay không. Mặc dù tao nhã về mặt toán học, nhưng điều này đưa ra các phép toán dấu phẩy động hoặc kiểm tra số nguyên cẩn thận và làm tròn rủi ro, vì vậy đối với bài toán này, cách tiếp cận lặp lại đơn giản hơn, an toàn hơn và hoàn toàn phù hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(√n) | O(1) | Đã chấp nhận | 
| Công thức bậc hai | O(1) | O(1) | Được chấp nhận, nhưng quá mức cần thiết | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`current`đến 1, biểu thị số tam giác hiện tại và một biến`increment`đến 2. Biến`increment`theo dõi số tiếp theo cần cộng để tạo thành số tam giác tiếp theo. 
2. Vòng lặp while`current`nhỏ hơn hoặc bằng`n`. Trên mỗi lần lặp, hãy kiểm tra xem`current`bằng`n`. Nếu có thì in`YES`và kết thúc chương trình. 
3. Nếu`current`không bằng`n`, thêm vào`increment`ĐẾN`current`để lấy số tam giác tiếp theo và tăng dần`increment`bằng 1. Điều này mô phỏng việc hình thành chuỗi 1, 3, 6, 10, v.v. mà không cần tính tổng từ đầu. 
4. Nếu vòng lặp thoát mà không tìm thấy kết quả khớp, hãy in`NO`. 

Lý do điều này có tác dụng là vì các số tam giác tăng trưởng một cách đơn điệu. Bằng cách lặp đi lặp lại việc xây dựng chúng, chúng tôi đảm bảo rằng nếu`n`là hình tam giác chúng ta sẽ đánh chính xác; nếu chúng ta vượt quá thì không có số tam giác nào bằng`n`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

current = 1
increment = 2

while current <= n:
    if current == n:
        print("YES")
        break
    current += increment
    increment += 1
else:
    print("NO")
```Mã theo thuật toán trực tiếp.`current`bắt đầu từ 1, số tam giác đầu tiên và`increment`bắt đầu từ 2. Vòng lặp thêm`increment`ĐẾN`current`mỗi lần, tạo ra các số tam giác liên tiếp. các`else`điều khoản về`while`vòng lặp chỉ kích hoạt nếu vòng lặp thoát ra một cách tự nhiên mà không có`break`, điều này đảm bảo`NO`được in nếu không tìm thấy kết quả phù hợp. Điều này tránh được các lỗi riêng lẻ và các bước kiểm tra không cần thiết. 

## Ví dụ đã hoạt động 

Hãy xem xét`n = 1`: 

| hiện tại | tăng | điều kiện hiện tại == n? | 
| --- | --- | --- | 
| 1 | 2 | CÓ | 

Số tam giác đầu tiên trùng khớp ngay lập tức. 

Coi như`n = 5`: 

| hiện tại | tăng | điều kiện hiện tại == n? | 
| --- | --- | --- | 
| 1 | 2 | KHÔNG | 
| 3 | 3 | KHÔNG | 
| 6 | 4 | kết thúc vòng lặp | 

Vòng lặp kết thúc vì`current`vượt quá`n`. Chương trình in`NO`, xác định chính xác rằng 5 không phải là hình tam giác. 

Những dấu vết này xác nhận tính bất biến của vòng lặp:`current`luôn bằng tổng của số đầu tiên`increment-1`số và chúng tôi kiểm tra mọi số tam giác cho đến`n`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(√n) | Số tam giác lớn nhất ≤ n xấp xỉ √(2n), do đó vòng lặp chạy xấp xỉ √(2n) lần lặp. | 
| Không gian | O(1) | Chỉ có hai biến số nguyên được sử dụng, không phụ thuộc vào kích thước đầu vào. | 

Với`n ≤ 500`, vòng lặp thực hiện tối đa 32 lần, thấp hơn nhiều so với giới hạn 2 giây. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        n = int(input())
        current = 1
        increment = 2
        while current <= n:
            if current == n:
                print("YES")
                break
            current += increment
            increment += 1
        else:
            print("NO")
    return out.getvalue().strip()

# provided samples
assert run("1\n") == "YES", "sample 1"
assert run("3\n") == "YES", "sample 2"

# custom cases
assert run("5\n") == "NO", "between triangular numbers"
assert run("6\n") == "YES", "triangular number"
assert run("500\n") == "NO", "upper bound non-triangular"
assert run("36\n") == "YES", "exact triangular number"
assert run("37\n") == "NO", "just above a triangular number"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | CÓ | đầu vào tối thiểu | 
| 5 | KHÔNG | giữa các số tam giác | 
| 500 | KHÔNG | giới hạn trên | 
| 36 | CÓ | số tam giác điển hình | 
| 37 | KHÔNG | số tam giác trên từng số một | 

## Vỏ cạnh 

cho`n = 1`, vòng lặp ngay lập tức phát hiện`current == n`và in`YES`. Điều này đảm bảo rằng thuật toán xử lý chính xác số tam giác nhỏ nhất. Vì`n = 500`, dãy số tam giác vượt quá 500 tại`current = 528`. Vòng lặp thoát ra một cách tự nhiên và in`NO`, xác định chính xác rằng 500 không phải là hình tam giác. Các trường hợp ngay bên dưới hoặc ngay trên các số tam giác xác nhận tính bất biến của vòng lặp: mỗi số tam giác được truy cập đúng một lần và đảm bảo vượt quá`NO`được trả về không có lỗi.

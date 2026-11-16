---
title: "CF 1902A - Mất cân bằng nhị phân"
description: "Chúng ta được cấp một chuỗi nhị phân chỉ gồm các ký tự '0' và '1'. Chúng ta có thể thực hiện một thao tác chèn cụ thể giữa hai ký tự liên tiếp bất kỳ. Nếu hai ký tự giống nhau, chúng ta chèn '1'. Nếu chúng khác nhau, chúng ta chèn '0'."
date: "2026-06-08T21:07:01+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1902
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 159 (Rated for Div. 2)"
rating: 800
weight: 1902
solve_time_s: 105
verified: true
draft: false
---

[CF 1902A - Mất cân bằng nhị phân](https://codeforces.com/problemset/problem/1902/A) 

**Đánh giá:** 800 
**Tas:** thuật toán xây dựng 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi nhị phân chỉ gồm các ký tự '0' và '1'. Chúng ta có thể thực hiện một thao tác chèn cụ thể giữa hai ký tự liên tiếp bất kỳ. Nếu hai ký tự giống nhau, chúng ta chèn '1'. Nếu chúng khác nhau, chúng ta chèn '0'. Nhiệm vụ là xác định xem liệu có thể sử dụng bất kỳ số lượng thao tác nào để làm cho số lượng số 0 lớn hơn số lượng số một hay không. 

Đầu vào có nhiều trường hợp kiểm thử, mỗi trường hợp có một chuỗi có độ dài tối đa là 100. Vì độ dài nhỏ nên chúng ta có thể suy luận cẩn thận về từng chuỗi mà không phải lo lắng về vấn đề hiệu suất đối với đầu vào lớn. Một quan sát quan trọng là chuỗi chỉ phát triển bằng cách chèn vào, nhưng loại ký tự được chèn hoàn toàn được xác định bởi mẫu kề. Nếu một chuỗi không chứa các ký tự liền kề khác nhau thì không thể chèn số 0 vào. Ngược lại, nếu một chuỗi chứa ít nhất một số '01' hoặc '10', chúng ta có thể tạo ra nhiều số 0 hơn. 

Một trường hợp đặc biệt quan trọng là khi chuỗi hoàn toàn là '1'. Cho dù chúng tôi thực hiện bao nhiêu lần chèn, mỗi lần chèn giữa các ký tự bằng nhau sẽ tạo ra một số '1' khác, vì vậy số 0 không thể nhiều hơn số 1. Ví dụ: với đầu vào "11", câu trả lời là "KHÔNG". Một trường hợp cạnh khác là một chuỗi có một số '0', chẳng hạn như "0". Ở đây chuỗi đã đáp ứng yêu cầu và không cần thực hiện thao tác nào. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng tất cả các lần chèn có thể cho đến khi số 0 vượt quá số một hoặc chúng tôi sử dụng hết mọi khả năng. Mỗi lần chèn sẽ tăng độ dài chuỗi lên một và có các vị trí O(n) cần xem xét ở mỗi bước. Với n=100, cách tiếp cận này có thể bùng nổ về mặt tổ hợp, tạo ra hàng triệu chuỗi tiềm năng và khiến nó không khả thi. Về nguyên tắc, nó hoạt động vì nó trực tiếp mô hình hóa vấn đề, nhưng rõ ràng là quá chậm. 

Thông tin chi tiết quan trọng là hoạt động mang tính quyết định dựa trên các ký tự liền kề. Nếu chuỗi đã có nhiều số 0 hơn số 1 thì câu trả lời là "CÓ". Nếu nó có nhiều hơn hoặc bằng nhau, chúng ta chỉ cần kiểm tra xem có ít nhất một cặp liền kề khác nhau hay không. Một cặp khác nhau sẽ tạo ra số 0 khi chúng ta chèn vào giữa chúng và việc chèn nó sẽ làm tăng số 0 mà không làm tăng số một. Vì chúng ta có thể lặp lại thao tác ở nhiều cặp khác nhau nên chúng ta luôn có thể đạt được cấu hình trong đó số 0 nhiều hơn số một nếu chuỗi không phải là tất cả một. 

Quan sát này làm giảm vấn đề thành một kiểm tra đơn giản: nếu chuỗi chứa ít nhất một số '0' và nó không hoàn toàn là '1' thì câu trả lời là "CÓ". Nếu chuỗi hoàn toàn là '1' thì câu trả lời là "KHÔNG". 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n^2) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, đếm số số 0 và số 1 trong chuỗi. Nếu số 0 đã lớn hơn số 1, ngay lập tức xuất ra "CÓ". 
2. Nếu chuỗi chỉ chứa số 1, hãy xuất "NO" vì không có thao tác nào có thể tạo ra số 0. 
3. Nếu có ít nhất một số '0' và số số 0 không lớn hơn số 1, hãy kiểm tra xem có vị trí nào mà các ký tự liền kề khác nhau không. Nếu một cặp như vậy tồn tại, số 0 có thể được chèn vào để cuối cùng làm cho số 0 vượt quá số 1. Đầu ra "CÓ". 
4. Ngược lại, xuất ra "NO". 

Tại sao nó hoạt động: các quy tắc chèn xác định đảm bảo rằng các ký tự liền kề khác nhau là nguồn duy nhất của các số 0 bổ sung. Bất kỳ chuỗi thao tác nào cũng không thể giảm các số 0 và không thể chèn các số 0 vào giữa các số bằng nhau. Do đó, hoặc chuỗi có khả năng tạo ra đủ số 0 thông qua các lần chèn này, hoặc là không thể. Việc kiểm tra các cặp liền kề khác nhau nắm bắt chính xác điều kiện cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()
    
    zeros = s.count('0')
    ones = s.count('1')
    
    if zeros > ones:
        print("YES")
        continue
    
    if zeros == 0:
        print("NO")
        continue
    
    found_diff_pair = False
    for i in range(n - 1):
        if s[i] != s[i + 1]:
            found_diff_pair = True
            break
    
    if found_diff_pair:
        print("YES")
    else:
        print("NO")
```Giải pháp đầu tiên là đếm số không và số một. Nếu số 0 đã chiếm ưu thế thì không cần thực hiện thao tác nào. Nếu chuỗi toàn là số 1 thì không thể chèn số 0 vào. Mặt khác, vòng lặp sẽ kiểm tra ít nhất một cặp liền kề khác nhau, đảm bảo khả năng chèn số 0. Chúng ta dừng lại ở lần xuất hiện đầu tiên để tránh tính toán không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1: "00" 

| Bước | số không | những cái | liền kề có khác nhau không? | Đầu ra | 
| --- | --- | --- | --- | --- | 
| ban đầu | 2 | 0 | Không | CÓ | 

Số không đã vượt quá số một. 

### Ví dụ 2: "11" 

| Bước | số không | những cái | liền kề có khác nhau không? | Đầu ra | 
| --- | --- | --- | --- | --- | 
| ban đầu | 0 | 2 | Không | KHÔNG | 

Tất cả những cái một, không thể chèn số 0. 

### Ví dụ 3: "10" 

| Bước | số không | những cái | liền kề có khác nhau không? | Đầu ra | 
| --- | --- | --- | --- | --- | 
| ban đầu | 1 | 1 | Có | CÓ | 

Cặp khác nhau ở vị trí 1-2 cho phép chèn số không. 

Những ví dụ này chứng minh rằng thuật toán xử lý chính xác câu hỏi có tầm thường, không tầm thường và trường hợp việc chèn tạo ra ưu thế bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Việc đếm các số 0 và số 1 cũng như quét các cặp liền kề khác nhau cần có thời gian tuyến tính tính theo độ dài chuỗi | 
| Không gian | O(1) | Chỉ sử dụng bộ đếm và một số biến boolean, không có cấu trúc dữ liệu bổ sung | 

Cho n ≤ 100 và t ≤ 100, tổng công việc tối đa là 10.000 thao tác, nằm trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# provided samples
assert run("3\n2\n00\n2\n11\n2\n10\n") == "YES\nNO\nYES", "sample 1"

# custom cases
assert run("2\n1\n0\n1\n1\n") == "YES\nNO", "single character"
assert run("1\n3\n010\n") == "YES", "alternating pattern"
assert run("1\n4\n1111\n") == "NO", "all ones"
assert run("1\n5\n00100\n") == "YES", "already zeros dominate"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 ký tự "0"/"1" | CÓ / KHÔNG | trường hợp cạnh đầu vào tối thiểu | 
| "010" | CÓ | các cặp liền kề khác nhau tạo ra số không | 
| "1111" | KHÔNG | chuỗi có toàn số 1 không thể tạo ra số 0 | 
| "00100" | CÓ | chuỗi đã thỏa mãn điều kiện | 

## Vỏ cạnh 

Trường hợp cạnh đầu vào tối thiểu có một số '0' duy nhất xác nhận rằng thuật toán không yêu cầu bất kỳ thao tác nào để trả về CÓ. Một số '1' trả về chính xác KHÔNG. Một chuỗi có tất cả các ký tự giống hệt nhau, như "1111" hoặc "0000", cho thấy yếu tố quyết định duy nhất là số lượng ban đầu và sự hiện diện của các cặp liền kề khác nhau. Các mẫu xen kẽ như "1010" chứng minh rằng việc chèn số 0 luôn có thể làm thay đổi số dư và thuật toán phát hiện điều này một cách chính xác thông qua quá trình quét các cặp khác nhau.

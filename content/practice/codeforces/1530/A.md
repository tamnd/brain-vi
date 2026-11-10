---
title: "CF 1530A - Thập phân nhị phân"
description: "Bài toán yêu cầu chúng ta chia một số nguyên dương $n$ thành một tổng các số có chữ số thập phân chỉ là 0 hoặc 1. Những số này được gọi là số thập phân nhị phân, như 1, 10, 11, 101, 1000. Mục đích là xác định số nhỏ nhất trong số các số thập phân nhị phân có tổng bằng $n$."
date: "2026-06-10T16:53:26+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1530
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 733 (Div. 1 + Div. 2, based on VK Cup 2021 - Elimination (Engine))"
rating: 800
weight: 1530
solve_time_s: 124
verified: true
draft: false
---

[CF 1530A - Số thập phân nhị phân](https://codeforces.com/problemset/problem/1530/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta phá một số nguyên dương$n$thành một tổng các số có chữ số thập phân chỉ là 0 hoặc 1. Những số này được gọi là số thập phân nhị phân, như 1, 10, 11, 101, 1000. Mục tiêu là xác định số nhỏ nhất của các số thập phân nhị phân có tổng bằng$n$. Mỗi trường hợp thử nghiệm cung cấp một số nguyên$n$và chúng ta cần xuất ra một số duy nhất cho mỗi trường hợp thử nghiệm biểu thị số lượng tối thiểu này. 

Những ràng buộc cho phép$n$lên đến$10^9$và lên tới 1000 trường hợp thử nghiệm. Điều này có nghĩa là bất kỳ giải pháp nào cũng phải chạy trong thời gian gần như tuyến tính cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất. Ví dụ: một thuật toán lặp qua tất cả các số nhỏ hơn$n$sẽ quá chậm, vì$n$có thể là gần một tỷ. 

Các trường hợp cạnh phát sinh khi$n$rất nhỏ, như 1 hoặc 5, hoặc khi$n$bản thân nó là một số thập phân nhị phân. Một cách tiếp cận tham lam ngây thơ chỉ trừ số thập phân nhị phân lớn nhất nhỏ hơn$n$nhiều lần có thể hiệu quả nhưng có thể phức tạp hơn mức cần thiết. Những cách tiếp cận bất cẩn cố gắng xây dựng trực tiếp số thập phân nhị phân mà không xem xét từng chữ số riêng biệt sẽ thất bại đối với các số như 121 hoặc 1102, tạo ra quá nhiều phần. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả các số thập phân nhị phân lên tới$n$và thử mọi sự kết hợp có tổng$n$. Điều này rõ ràng là không thể thực hiện được: vì$n$xung quanh$10^9$, có hàng nghìn số thập phân nhị phân và việc kiểm tra tất cả các kết hợp sẽ cần thời gian theo cấp số nhân. 

Cái nhìn sâu sắc quan trọng là số lượng thập phân nhị phân tối thiểu cần thiết để tính tổng$n$được xác định bởi chữ số thập phân lớn nhất trong$n$. Mỗi chữ số$d_i$TRONG$n$đóng góp$d_i$"những cái" ở vị trí thập phân đó. Ví dụ: số 121 có các chữ số 1, 2, 1. Chữ số lớn nhất là 2, nghĩa là chúng ta cần ít nhất hai số thập phân nhị phân để tính tổng bằng 121. Chúng ta có thể xây dựng các số này bằng cách phân tách từng chữ số theo chiều dọc: lấy mỗi chữ số và gán giá trị của nó cho nhiều số thập phân nhị phân bằng số lớn nhất của chữ số đó. Điều này làm giảm vấn đề xuống việc quét các chữ số đơn giản. 

Cách tiếp cận này khá tham lam nhưng tối ưu vì mỗi số thập phân nhị phân chỉ có thể đóng góp 0 hoặc 1 cho mỗi chữ số thập phân. Do đó, không có phần đóng góp của chữ số nào có thể được kết hợp thành ít hơn chữ số tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k) | O(k) | Quá chậm | 
| Quét chữ số tham lam | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, hãy đọc số nguyên$n$. 
2. Chuyển đổi$n$thành một chuỗi để chúng ta có thể lặp lại các chữ số thập phân của nó. 
3. Khởi tạo một biến`max_digit`về không. Điều này sẽ theo dõi chữ số lớn nhất trong$n$. 
4. Lặp lại từng ký tự`c`trong biểu diễn chuỗi của$n$. Chuyển thành`c`đến một số nguyên`digit`. 
5. Nếu`digit`lớn hơn`max_digit`, cập nhật`max_digit`ĐẾN`digit`. 
6. Sau khi quét tất cả các chữ số,`max_digit`đại diện cho số thập phân nhị phân tối thiểu được yêu cầu. 
7. Đầu ra`max_digit`. 

Tại sao nó hoạt động: mỗi số thập phân nhị phân chỉ có thể đóng góp 1 hoặc 0 ở mỗi vị trí thập phân. Để đạt được một chữ số$d_i$ở vị trí$i$, chúng ta cần ít nhất$d_i$những cái ở vị trí đó trên tất cả các số thập phân nhị phân. Do đó, chữ số tối đa ở bất kỳ vị trí nào trong số sẽ chỉ ra số lần triệu hồi tối thiểu. Bất biến này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = input().strip()
    max_digit = max(int(c) for c in n)
    print(max_digit)
```Giải pháp đọc số lượng ca kiểm thử, sau đó lặp qua từng ca kiểm thử. Việc chuyển đổi số thành chuỗi sẽ tránh lặp lại các phép chia và modulo, đơn giản hóa việc trích xuất chữ số. sử dụng`max(int(c) for c in n)`tính toán ngắn gọn chữ số lớn nhất trên một dòng. Không có vấn đề riêng biệt nào vì các chữ số luôn nằm trong khoảng từ 0 đến 9 và đảm bảo đầu vào$n \ge 1$. 

## Ví dụ đã hoạt động 

### Mẫu 1:`121`| Bước | n | Chữ số | max_digit | 
| --- | --- | --- | --- | 
| 1 | 121 | 1 | 1 | 
| 2 | 121 | 2 | 2 | 
| 3 | 121 | 1 | 2 | 

Chữ số lớn nhất là 2 nên đáp án là 2. Điều này tương ứng với phép phân tách 110 + 11 hoặc 111 + 10. 

### Mẫu 2:`5`| Bước | n | Chữ số | max_digit | 
| --- | --- | --- | --- | 
| 1 | 5 | 5 | 5 | 

Chữ số tối đa là 5, vì vậy cần có 5 số thập phân nhị phân, ví dụ: 1+1+1+1+1. 

Những dấu vết này cho thấy rằng việc quét các chữ số và lấy số tối đa trực tiếp mang lại số lượng triệu hồi tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) cho mỗi trường hợp thử nghiệm | Quét từng chữ số thập phân của n, là O(log10 n) | 
| Không gian | O(1) | Chỉ có cửa hàng`max_digit`và biến vòng lặp | 

Với$n \le 10^9$Và$t \le 1000$, thuật toán thực hiện tối đa$1000 * 9 = 9000$hoạt động, trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    
    t = int(input())
    for _ in range(t):
        n = input().strip()
        max_digit = max(int(c) for c in n)
        print(max_digit)
        
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("3\n121\n5\n1000000000\n") == "2\n5\n1", "sample 1"

# Custom cases
assert run("1\n1\n") == "1", "minimum input"
assert run("1\n999\n") == "9", "maximum single-digit"
assert run("1\n101010\n") == "1", "alternating digits"
assert run("1\n111111111\n") == "1", "all ones"
assert run("1\n1203\n") == "3", "mixed digits, max in middle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | Đầu vào tối thiểu | 
| 999 | 9 | Trình kích hoạt một chữ số tối đa chính xác max | 
| 101010 | 1 | Các số 0 và số xen kẽ | 
| 111111111 | 1 | Tất cả các chữ số là một | 
| 1203 | 3 | Chữ số tối đa ở giữa | 

## Vỏ cạnh 

cho`n = 1`, thuật toán quét một chữ số 1, đặt`max_digit`thành 1 và đầu ra 1. Đối với`n = 1000000000`, các chữ số chủ yếu là số 0 có số 1 đứng đầu, vì vậy chữ số tối đa là 1, mang lại số thập phân nhị phân đơn chính xác. Vì`n = 1203`, các chữ số là 1, 2, 0, 3; bộ quét`max_digit`đến 3, đảm bảo số lệnh triệu tập tối thiểu bằng chữ số thập phân lớn nhất. Những trường hợp này xác nhận thuật toán xử lý chính xác các vị trí chữ số tối thiểu, tối đa và không rõ ràng.

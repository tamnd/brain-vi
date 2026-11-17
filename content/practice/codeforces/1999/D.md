---
title: "CF 1999D - Kỳ thi Slavic"
description: "Tác vụ yêu cầu tổng các chữ số của một số có hai chữ số cho nhiều trường hợp thử nghiệm. Mỗi số đầu vào $n$ được đảm bảo nằm trong khoảng từ 10 đến 99, vì vậy chữ số đầu tiên luôn khác 0."
date: "2026-06-08T14:22:08+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1999
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 964 (Div. 4)"
rating: 1100
weight: 1999
solve_time_s: 164
verified: false
draft: false
---

[CF 1999D - Kỳ thi dành cho người Slav](https://codeforces.com/problemset/problem/1999/D) 

**Đánh giá:** 1100 
**Tags:** tham lam, thực hiện, chuỗi 
**Thời gian giải:** 2m 44s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Tác vụ yêu cầu tổng các chữ số của một số có hai chữ số cho nhiều trường hợp thử nghiệm. Mỗi số đầu vào$n$được đảm bảo bao gồm từ 10 đến 99, vì vậy chữ số đầu tiên luôn khác 0. Đầu vào bắt đầu bằng$t$, số lượng ca kiểm thử, theo sau là$t$mỗi dòng chứa một số nguyên có hai chữ số. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất biểu thị tổng của các chữ số hàng chục và hàng đơn vị. 

Với những hạn chế, số lượng ca kiểm thử$t$nhỏ, lên tới 90, và mỗi$n$được cố định về kích thước. Không cần các thuật toán hoặc cấu trúc dữ liệu phức tạp vì ngay cả cách tiếp cận đơn giản tính tổng cho từng số riêng lẻ cũng đủ nhanh. Các trường hợp đặc biệt rất đơn giản: số có hai chữ số nhỏ nhất là 10, có tổng chữ số là 1 và số lớn nhất là 99, có tổng chữ số là 18. Cách tiếp cận bất cẩn có thể cố gắng chuyển đổi số thành chuỗi một cách không cần thiết hoặc xử lý sai số 10, nhưng cách tiếp cận số học đơn giản bằng cách sử dụng phép chia và modulo sẽ tránh được những cạm bẫy này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là chuyển đổi từng số thành một chuỗi, chia nó thành các ký tự, chuyển đổi các ký tự đó thành số nguyên và tính tổng chúng. Điều này hoạt động chính xác vì mỗi số được đảm bảo có chính xác hai chữ số. Nhược điểm là nó liên quan đến các thao tác chuỗi, không cần thiết đối với các số có kích thước này. 

Cách tiếp cận tối ưu hoàn toàn là số học. Đối với số có hai chữ số$n$, chữ số hàng chục có thể được tính là$n // 10$và chữ số hàng đơn vị là$n % 10$. Tính tổng hai giá trị này sẽ cho ra tổng các chữ số một cách trực tiếp, tránh mọi thao tác chuỗi và hoạt động trong thời gian không đổi cho mỗi trường hợp thử nghiệm. Cho rằng$t$tối đa là 90, phương pháp này sẽ hoàn thành trong chưa đầy một giây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chuyển đổi sang chuỗi và chữ số tổng | O(t) | O(1) | Đã chấp nhận | 
| Trích xuất chữ số số học | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$t$từ đầu vào, đại diện cho số lượng trường hợp thử nghiệm. 
2. Lặp lại từng test case từ 1 đến$t$. 
3. Đối với trường hợp thử nghiệm hiện tại, hãy đọc số nguyên$n$. 
4. Tính chữ số hàng chục là$n // 10$. 
5. Tính chữ số hàng đơn vị là$n % 10$. 
6. Tính tổng các chữ số hàng chục và hàng đơn vị để có kết quả. 
7. In tổng. 

Điều này có hiệu quả vì mọi số đều được đảm bảo có hai chữ số, do đó các phép chia số nguyên và modulo sẽ tách biệt chính xác hàng chục và đơn vị. Không có trường hợp số nào có ít hơn hai chữ số hoặc nhiều hơn hai chữ số nên các thao tác này đều an toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        print(n // 10 + n % 10)

if __name__ == "__main__":
    solve()
```Giải pháp đọc số lượng trường hợp thử nghiệm, sau đó lặp qua từng số, tính tổng các chữ số bằng phép chia số nguyên và modulo rồi in kết quả. sử dụng`sys.stdin.readline`đảm bảo xử lý đầu vào nhanh ngay cả khi số lượng trường hợp kiểm thử gần giới hạn trên. Không có lỗi riêng lẻ vì phép chia số nguyên và modulo cho số có hai chữ số rất đơn giản. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu đầu tiên 77, chữ số hàng chục là 7 và chữ số hàng đơn vị là 7. Tổng các giá trị này cho kết quả 14. Đối với đầu vào 10, chữ số hàng chục là 1 và chữ số hàng đơn vị là 0, cho tổng bằng 1. 

| n | hàng chục (n // 10) | đơn vị (n % 10) | tổng hợp | 
| --- | --- | --- | --- | 
| 77 | 7 | 7 | 14 | 
| 21 | 2 | 1 | 3 | 
| 40 | 4 | 0 | 4 | 
| 10 | 1 | 0 | 1 | 
| 99 | 9 | 9 | 18 | 

Dấu vết này xác nhận rằng thuật toán tính toán chính xác tổng cho cả đầu vào có hai chữ số tối thiểu và tối đa, đồng thời xử lý các số có chữ số hàng đơn vị bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm bao gồm một số phép tính số học không đổi. | 
| Không gian | O(1) | Không cần cấu trúc dữ liệu bổ sung ngoài việc lưu trữ số nguyên đầu vào. | 

Với$t \le 90$, giải pháp này chạy trong thời gian không đáng kể và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided sample
assert run("8\n77\n21\n40\n34\n19\n84\n10\n99\n") == "14\n3\n4\n7\n10\n12\n1\n18", "sample 1"

# Custom cases
assert run("3\n10\n99\n55\n") == "1\n18\n10", "edge digit sums"
assert run("2\n11\n20\n") == "2\n2", "leading or trailing zero"
assert run("1\n88\n") == "16", "equal digits"
assert run("1\n15\n") == "6", "arbitrary mid-range"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10, 99, 55 | 1, 18, 10 | Tổng chữ số tối thiểu và tối đa, và các chữ số hàng chục và hàng đơn vị bằng nhau | 
| 11, 20 | 2, 2 | Xử lý đúng khi chữ số hàng đơn vị bằng 0 hoặc cả hai chữ số đều nhỏ | 
| 88 | 16 | Cả hai chữ số đều bằng nhau và có tổng > 15 | 
| 15 | 6 | Tổng số có hai chữ số | 

## Vỏ cạnh 

Số nhỏ nhất có hai chữ số 10 tạo ra tổng bằng 1. Số 99 lớn nhất tạo ra tổng bằng 18. Các số như 20 hoặc 30 kiểm tra xem thuật toán có xử lý chính xác các số 0 hay không; phép chia số nguyên và modulo trích xuất chính xác hàng chục và đơn vị trong những trường hợp này. Các số có chữ số giống nhau, chẳng hạn như 55 hoặc 88, cũng hoạt động chính xác vì tổng đơn giản gấp đôi chữ số lặp lại. Điều này xác nhận thuật toán xử lý tất cả các số có hai chữ số trong phạm vi cho phép.

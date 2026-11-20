---
title: "CF 2185A - Root hoàn hảo"
description: "Chúng ta được yêu cầu tạo ra một chuỗi các \"gốc hoàn hảo\" riêng biệt cho mỗi trường hợp thử nghiệm. Một nghiệm hoàn hảo đơn giản là một số nguyên $x$ sao cho $x^2$ cũng là một số nguyên, điều này gần như đúng với mọi số nguyên dương."
date: "2026-06-07T21:28:33+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 2185
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1074 (Div. 4)"
rating: 800
weight: 2185
solve_time_s: 108
verified: false
draft: false
---

[CF 2185A - Root hoàn hảo](https://codeforces.com/problemset/problem/2185/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tạo ra một chuỗi các "gốc hoàn hảo" riêng biệt cho từng trường hợp thử nghiệm. Một gốc hoàn hảo chỉ đơn giản là một số nguyên$x$như vậy$x^2$cũng là một số nguyên, điều này đúng với mọi số nguyên dương. Nói cách khác, mọi số nguyên dương đều đủ điều kiện là một nghiệm hoàn hảo, vì bình phương của nó cũng là một số nguyên. 

Đầu vào bao gồm một số trường hợp thử nghiệm$t$, mỗi cái xác định một số nguyên$n$. Đối với mỗi trường hợp thử nghiệm, chúng ta phải xuất ra$n$các số nguyên dương phân biệt, nhiều nhất mỗi số nguyên dương$10^9$. Không có ràng buộc nào về giá trị của các nghiệm hoàn hảo ngoài việc khác biệt cho mỗi trường hợp thử nghiệm và trong phạm vi được chỉ định. 

Cái nhìn sâu sắc quan trọng là vấn đề liên quan nhiều đến việc tạo ra bất kỳ tập hợp số nguyên dương riêng biệt nào hơn là thực hiện bất kỳ phép tính toán học phức tạp nào. Cho rằng$n$nhỏ (lên tới 20) và giới hạn giá trị cao ($10^9$), chúng ta không cần phải lo lắng về hiệu suất hoặc việc tạo số ở quy mô lớn. Các trường hợp cạnh có thể liên quan đến$n = 1$hoặc$n = 20$, nhưng bất kỳ chuỗi đơn giản nào gồm các số nguyên liên tiếp từ 1 đến$n$là đủ và an toàn. Ví dụ: việc triển khai bất cẩn có thể vô tình lặp lại các số trong một trường hợp thử nghiệm hoặc vượt quá giới hạn cho phép.$10^9$giới hạn. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là chọn ngẫu nhiên các số và kiểm tra xem chúng có khác biệt hay không. Điều này sẽ làm việc cho nhỏ$n$, nhưng chi phí kiểm tra tính duy nhất là không cần thiết. Hơn nữa, tính ngẫu nhiên có thể gây ra nguy cơ xung đột, cần phải thử lại. 

Cách tiếp cận tối ưu mang tính quyết định: tạo ra kết quả đầu tiên$n$số nguyên dương trực tiếp. Điều này đảm bảo rằng tất cả các số đều khác nhau, theo định nghĩa, mỗi số đều là một nghiệm hoàn hảo và không vượt quá$10^9$. Kể từ khi tối đa$n$là 20, giải pháp này rất nhanh và đơn giản. Không cần thủ thuật tối ưu hóa hoặc cấu trúc dữ liệu nâng cao. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tạo ngẫu nhiên với kiểm tra tính duy nhất | O(n²) trong trường hợp xấu nhất | O(n) | Sự phức tạp không cần thiết | 
| Số nguyên tuần tự xác định | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$t$về số lượng ca kiểm thử. Điều này thiết lập một vòng lặp để xử lý từng trường hợp thử nghiệm một cách độc lập. 
2. Với mỗi test, hãy đọc số nguyên$n$trong đó chỉ định có bao nhiêu gốc hoàn hảo để xuất ra. 
3. Tạo danh sách các số nguyên từ 1 đến$n$. Mỗi số nguyên vốn là một căn bậc hoàn hảo vì bình phương của nó là một số nguyên hợp lệ. 
4. Xuất danh sách các số nguyên dưới dạng các giá trị được phân tách bằng dấu cách trên một dòng. 
5. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: Thuật toán bảo toàn tính bất biến rằng tất cả các số trong mỗi danh sách đầu ra là khác biệt và dương, và mỗi số$x$thỏa mãn$x^2$là một số nguyên. Bởi vì$n \leq 20$, trong khoảng từ 1 đến$n$cũng nằm trong giới hạn của$10^9$, đảm bảo tính đúng đắn 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    # generate perfect roots 1, 2, ..., n
    result = [str(i) for i in range(1, n + 1)]
    print(" ".join(result))
```Giải pháp đọc số lượng trường hợp kiểm thử và xử lý từng trường hợp một cách tuần tự. Đối với mỗi$n$, nó xây dựng một danh sách các số nguyên từ 1 đến$n$. Chuyển đổi số nguyên thành chuỗi cho phép sử dụng`" ".join()`để tạo ra đầu ra được phân tách bằng dấu cách cần thiết. Cách tiếp cận này tránh được từng lỗi một và đảm bảo tất cả các số đều khác biệt. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
3
1
2
5
```**Theo dõi bước cho từng trường hợp thử nghiệm:** 

| Trường hợp thử nghiệm | n | Danh sách đã tạo | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 1 | [1] | 1 | 
| 2 | 2 | [1, 2] | 1 2 | 
| 3 | 5 | [1, 2, 3, 4, 5] | 1 2 3 4 5 | 

Dấu vết này chứng tỏ rằng thuật toán tạo ra một cách chính xác các nghiệm hoàn hảo riêng biệt cho bất kỳ$n \le 20$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n) | Mỗi trường hợp thử nghiệm tạo ra$n$số nguyên một cách tuần tự. Tối đa t = 20, n = 20, các phép toán tầm thường. | 
| Không gian | O(n) | Lưu trữ danh sách các số nguyên cho từng trường hợp thử nghiệm. | 

Với những hạn chế, giải pháp này chạy thoải mái trong vòng 1 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    t = int(input())
    for _ in range(t):
        n = int(input())
        result = [str(i) for i in range(1, n + 1)]
        print(" ".join(result))

    return output.getvalue().strip()

# provided samples
assert run("3\n1\n2\n5\n") == "1\n1 2\n1 2 3 4 5", "sample 1"

# custom cases
assert run("1\n1\n") == "1", "minimum size input"
assert run("1\n20\n") == "1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20", "maximum n"
assert run("2\n3\n4\n") == "1 2 3\n1 2 3 4", "multiple test cases"
assert run("1\n5\n") == "1 2 3 4 5", "general case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | đầu vào kích thước tối thiểu | 
| 20 | 1 2 ... 20 | tối đa n | 
| 3 4 | 1 2 3 / 1 2 3 4 | nhiều trường hợp thử nghiệm | 
| 5 | 1 2 3 4 5 | trường hợp chung | 

## Vỏ cạnh 

Khi nào$n = 1$, thuật toán tạo ra`[1]`đó là hợp lệ. Vì$n = 20$, đầu ra`[1, 2, ..., 20]`vẫn còn trong$10^9$giới hạn, xác nhận không xảy ra tràn. Nếu các số nguyên liên tiếp được thay thế bằng các giá trị không liên tiếp nhưng vẫn khác biệt thì lời giải vẫn hợp lệ, nhưng lựa chọn tuần tự đảm bảo tính đơn giản và chính xác mà không có nguy cơ trùng lặp.

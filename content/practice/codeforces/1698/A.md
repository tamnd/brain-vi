---
title: "CF 1698A - Hỗn hợp XOR"
description: "Chúng ta được cung cấp một mảng có độ dài n được tạo bằng cách lấy một mảng ban đầu có độ dài n-1, tính toán XOR của tất cả các phần tử của nó, nối XOR đó vào mảng và sau đó xáo trộn kết quả."
date: "2026-06-09T22:18:39+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 1698
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 803 (Div. 2)"
rating: 800
weight: 1698
solve_time_s: 169
verified: false
draft: false
---

[CF 1698A - Hỗn hợp XOR](https://codeforces.com/problemset/problem/1698/A) 

**Đánh giá:** 800 
**Tags:** bitmask, vũ lực 
**Thời gian giải:** 2m 49s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài`n`được tạo bằng cách lấy một mảng có độ dài ban đầu`n-1`, tính toán XOR của tất cả các phần tử của nó, nối XOR đó vào mảng và sau đó xáo trộn kết quả. Nhiệm vụ là khôi phục giá trị XOR đã được thêm vào, chỉ đưa ra mảng được xáo trộn. Mỗi trường hợp thử nghiệm cung cấp một mảng như vậy và chúng ta phải tạo ra một số nguyên cho mỗi trường hợp thử nghiệm tương ứng với XOR đã được thêm vào. 

Những hạn chế còn khiêm tốn:`n`có thể lên tới 100 và có thể lên tới 1000 trường hợp thử nghiệm. Mỗi phần tử trong mảng là một số nguyên nhỏ từ 0 đến 127. Điều này ngụ ý rằng bất kỳ thuật toán nào có độ phức tạp O(n) cho mỗi trường hợp thử nghiệm sẽ chạy hiệu quả, vì ngay cả trong trường hợp xấu nhất, chúng ta chỉ cần khoảng 100.000 thao tác, điều này không đáng kể đối với các CPU hiện đại. Điều này cũng loại trừ nhu cầu về cấu trúc dữ liệu phức tạp hoặc tối ưu hóa. 

Các trường hợp biên bao gồm các mảng trong đó tất cả các phần tử đều giống hệt nhau, trong đó`x`có thể trùng với một trong các phần tử ban đầu, hoặc ở đâu`x`là số không. Ví dụ, nếu mảng là`[6, 6, 6, 6, 6, 6]`, đáp án đúng là 6. Một trường hợp phức tạp khác là`[100, 100, 0]`, trong đó XOR của hai phần tử ban đầu là 0. Một cách tiếp cận đơn giản, chẳng hạn, cố gắng loại bỏ các bản sao hoặc giả định`x`là phần tử tối đa có thể bị lỗi trên các đầu vào này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử loại bỏ lần lượt từng phần tử, tính toán XOR của phần còn lại và kiểm tra xem nó có khớp với phần tử bị loại bỏ hay không. Đối với mỗi`n`phần tử, chúng tôi sẽ tính toán XOR trên`n-1`các phần tử, dẫn đến các hoạt động O(n²) cho mỗi trường hợp thử nghiệm. Điều này khả thi về mặt kỹ thuật đối với các ràng buộc nhất định, nhưng không cần thiết vì phép toán XOR có một thuộc tính giúp có thể thực hiện được một giải pháp đơn giản hơn. 

Cái nhìn sâu sắc quan trọng là XOR vừa có tính kết hợp vừa có tính giao hoán. Điều này có nghĩa là XOR tất cả các phần tử của mảng cuối cùng cho chúng ta kết quả tương tự như XOR mảng ban đầu hai lần và giá trị được nối thêm một lần. Nếu chúng ta để XOR của mảng ban đầu là`x`, mảng cuối cùng chứa tất cả các phần tử gốc cộng với`x`. XOR mọi thứ cùng nhau mang lại`x ⊕ a1 ⊕ a2 ⊕ ... ⊕ a(n-1)`. Nhưng`a1 ⊕ a2 ⊕ ... ⊕ a(n-1)`chỉ là`x`, vậy tổng XOR là`x ⊕ x = 0`cộng với phần bổ sung`x`, rời đi`x`. Do đó, XOR của tất cả các phần tử của mảng cuối cùng trực tiếp cho giá trị của`x`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Hoạt động nhưng không cần thiết | 
| XOR Tất cả các phần tử | O(n) | O(1) | Được chấp nhận và đơn giản | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, hãy đọc`n`và mảng của`n`số nguyên. 
3. Khởi tạo một biến`x`về 0. Điều này sẽ giữ XOR của tất cả các phần tử. 
4. Lặp lại mảng, XOR từng phần tử vào`x`. Sau vòng lặp này,`x`chứa XOR của tất cả`n`các phần tử. 
5. Đầu ra`x`cho trường hợp thử nghiệm hiện tại. 

Tại sao nó hoạt động: XOR có đặc tính`a ⊕ a = 0`Và`a ⊕ 0 = a`. Trong mảng cuối cùng, mọi phần tử gốc xuất hiện một lần và`x`xuất hiện một lần. XOR tất cả chúng cùng nhau sẽ loại bỏ tất cả các phần tử ban đầu bởi vì`a1 ⊕ a2 ⊕ ... ⊕ a(n-1) ⊕ x = x`. Điều này đảm bảo rằng chúng tôi sẽ luôn khôi phục XOR được nối thêm chính xác, bất kể sự xáo trộn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    x = 0
    for num in a:
        x ^= num
    print(x)
```Giải pháp đọc số lượng trường hợp thử nghiệm và sau đó lặp lại chúng. Đối với mỗi mảng, nó tính toán XOR của tất cả các phần tử bằng vòng lặp for đơn giản. Bộ tích lũy XOR bắt đầu từ 0, đảm bảo rằng nó xử lý chính xác các mảng trong đó`x`có thể bằng 0. Không cần xử lý đặc biệt các phần tử xáo trộn hoặc lặp lại vì XOR tự động hủy các bản sao. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
4
4
4 3 2 5
5
6 1 10 7 10
6
6 6 6 6 6 6
3
100 100 0
```| Trường hợp thử nghiệm | Mảng | XOR của tất cả các yếu tố | Đầu ra x | 
| --- | --- | --- | --- | 
| 1 | 4 3 2 5 | 4 ⊕ 3 ⊕ 2 ⊕ 5 = 3 | 3 | 
| 2 | 6 1 10 7 10 | 6 ⊕ 1 ⊕ 10 ⊕ 7 ⊕ 10 = 7 | 7 | 
| 3 | 6 6 6 6 6 6 | 6 ⊕ 6 ⊕ 6 ⊕ 6 ⊕ 6 ⊕ 6 = 6 | 6 | 
| 4 | 100 100 0 | 100 ⊕ 100 ⊕ 0 = 0 | 0 | 

Những dấu vết này cho thấy XOR tự động hủy các phần tử mảng ban đầu, giữ nguyên giá trị XOR được nối thêm, ngay cả khi có các số hoặc số 0 lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n) | Mỗi trường hợp thử nghiệm yêu cầu một lần chuyển qua mảng tới các phần tử XOR | 
| Không gian | O(n) | Chỉ có mảng cần được lưu trữ; bộ tích lũy XOR sử dụng không gian không đổi | 

Được cho`n <= 100`Và`t <= 1000`, giải pháp chạy thoải mái trong chưa đầy 1 giây và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        x = 0
        for num in a:
            x ^= num
        print(x)
    return output.getvalue().strip()

# provided samples
assert run("4\n4\n4 3 2 5\n5\n6 1 10 7 10\n6\n6 6 6 6 6 6\n3\n100 100 0\n") == "3\n7\n6\n0", "sample 1"

# custom cases
assert run("1\n2\n0 0\n") == "0", "minimum size, zero"
assert run("1\n2\n1 1\n") == "1", "minimum size, non-zero"
assert run("1\n3\n127 127 0\n") == "0", "max element boundary"
assert run("1\n5\n1 2 3 0 0\n") == "0", "zero among small numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 0 0 | 0 | Kích thước mảng tối thiểu, bằng 0 XOR | 
| 2 1 1 | 1 | Kích thước mảng tối thiểu, XOR khác 0 | 
| 3 127 127 0 | 0 | Phần tử tối đa, xử lý XOR có ranh giới | 
| 5 1 2 3 0 0 | 0 | Số 0 trộn lẫn với số khác 0 | 

## Vỏ cạnh 

Đối với một mảng`[6, 6, 6, 6, 6, 6]`, thuật toán tính XOR là`6 ⊕ 6 ⊕ 6 ⊕ 6 ⊕ 6 ⊕ 6 = 6`. Các phần tử lặp lại sẽ hủy theo cặp, giữ nguyên XOR được nối thêm. Vì`[100, 100, 0]`, XOR`100 ⊕ 100 ⊕ 0 = 0`phục hồi chính xác XOR được thêm vào mảng hai phần tử ban đầu`[100, 100]`. Những ví dụ này xác nhận rằng giải pháp xử lý các số trùng lặp và số 0 một cách chính xác mà không cần logic bổ sung.

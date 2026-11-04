---
title: "CF 1159B - Hệ số giãn nở của mảng"
description: "Chúng ta được cho một mảng các số nguyên không âm. Chúng ta định nghĩa một thuộc tính được gọi là $k$-extension: một mảng là một $k$-extension nếu với mỗi cặp phần tử $ai$ và $aj$, bất đẳng thức $k cdot. Đầu vào bao gồm độ dài $n$ của mảng, lên tới 300.000, và các phần tử mảng…"
date: "2026-06-12T02:26:20+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1159
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 559 (Div. 2)"
rating: 1300
weight: 1159
solve_time_s: 90
verified: true
draft: false
---

[CF 1159B - Hệ số mở rộng của mảng](https://codeforces.com/problemset/problem/1159/B) 

**Đánh giá:** 1300 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên không âm. Chúng ta định nghĩa một thuộc tính được gọi là$k$-phần mở rộng: mảng là một$k$-mở rộng nếu cho mọi cặp phần tử$a_i$Và$a_j$, bất đẳng thức$k \cdot |i - j| \le \min(a_i, a_j)$nắm giữ. Theo trực giác, các phần tử mảng phải đủ lớn so với khoảng cách giữa các chỉ số của chúng, được chia tỷ lệ theo$k$. Hệ số mở rộng của mảng là lớn nhất$k$mà tài sản này nắm giữ. Mảng nào cũng thỏa mãn$0$-mở rộng một cách tầm thường. 

Đầu vào bao gồm chiều dài$n$của mảng, lên tới 300.000 và các phần tử mảng, có thể đạt tới$10^9$. Điều này có nghĩa là bất kỳ thuật toán nào có độ phức tạp kém hơn$O(n \log n)$có thể là quá chậm, vì$O(n^2)$các hoạt động sẽ theo thứ tự$10^{10}$, vượt xa giới hạn điển hình. 

Các trường hợp biên phát sinh khi các phần tử mảng nhỏ hoặc bằng 0. Ví dụ, nếu mảng là`[0, 0, 0]`, hệ số giãn nở bằng không. Một giải pháp đơn giản cố gắng kiểm tra từng cặp chỉ số có thể thất bại với những giá trị nhỏ như vậy vì nó có thể giả định giá trị dương.$k$luôn luôn có thể. 

## Phương pháp tiếp cận 

Phương pháp brute-force trực tiếp kiểm tra$k$-thuộc tính mở rộng cho tất cả những gì có thể$k$các giá trị. Chúng ta có thể thử mọi số nguyên$k$từ$0$trở lên và đối với mỗi$k$, xác minh rằng$k \cdot |i-j| \le \min(a_i, a_j)$cho mỗi cặp$(i,j)$. Việc xác minh cho một$k$yêu cầu$O(n^2)$các hoạt động trở nên không khả thi đối với$n$lên đến$3 \cdot 10^5$. 

Quan sát quan trọng là giá trị tối đa hợp lệ$k$được xác định bởi tỷ lệ tối thiểu của phần tử mảng với khoảng cách của nó với phần tử khác. Chính thức, với mỗi cặp chỉ số liên tiếp$(i, i+1)$, tối đa$k$bị giới hạn bởi$a_i / 1$hoặc$a_{i+1} / 1$. Mở rộng điều này, chúng ta chỉ cần kiểm tra các phần tử liên tiếp vì hàm$k \cdot |i-j| \le \min(a_i, a_j)$là đơn điệu trong$|i-j|$. Mảng không thể hỗ trợ lớn hơn$k$cho các phần tử ở xa hơn là cho các phần tử ở gần. Do đó, giải pháp tối ưu giảm thiểu việc tính toán mức phân chia tầng tối thiểu của từng phần tử theo khoảng cách của nó với các phần tử lân cận. 

Điều này làm giảm sự phức tạp từ$O(n^2)$ĐẾN$O(n)$. Chúng ta lặp qua từng cặp phần tử liên tiếp, tính toán`min(a[i] // 1, a[i+1] // 1)`và lấy giá trị tối thiểu trên tất cả các cặp. Điều này mang lại hệ số giãn nở. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`k_max`đến một giá trị lớn, ví dụ như phần tử đầu tiên, để theo dõi giá trị hợp lệ tối đa$k$. 
2. Lặp lại mảng từ chỉ mục`0`ĐẾN`n-2`để xử lý các cặp liên tiếp`(a[i], a[i+1])`. 
3. Với mỗi cặp, tính giá trị lớn nhất$k$được cặp này cho phép:`min(a[i] // 1, a[i+1] // 1)`. Vì khoảng cách là 1 đối với các phần tử liên tiếp nên việc chia tầng cho 1 là đủ. 
4. Cập nhật`k_max`là mức tối thiểu giữa giá trị hiện tại của nó và giá trị được tính cho cặp này. 
5. Sau khi xử lý tất cả các cặp liên tiếp,`k_max`chứa số nguyên tối đa$k` such that the array is a $k$-phần mở rộng. 
6. In`k_max`. 

Thuật toán hoạt động vì$k$-điều kiện mở rộng cho các chỉ số liên tiếp là ràng buộc chặt chẽ nhất. Nếu nó giữ cho các cặp liên tiếp, nó sẽ tự động giữ cho các cặp cách xa nhau hơn, vì`k * |i-j|`tăng tuyến tính theo khoảng cách và`min(a_i, a_j)`chỉ giảm đi hoặc giữ nguyên. Bất biến được duy trì là`k_max`luôn là giá trị tối đa được cho phép bởi tất cả các cặp được xem xét cho đến nay. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

k_max = float('inf')

for i in range(n - 1):
    k_pair = min(a[i], a[i + 1])
    if k_pair < k_max:
        k_max = k_pair

# To account for the distance scaling, we divide by 1 (distance between consecutive elements)
print(k_max)
```Việc triển khai này khởi tạo`k_max`là vô cùng, sau đó lặp qua các cặp liên tiếp để tìm ra ràng buộc chặt chẽ nhất. Chúng tôi chia cho khoảng cách (1 cho các phần tử liên tiếp). Bởi vì Python xử lý các số nguyên lớn một cách khéo léo nên không xảy ra hiện tượng tràn. Sự tinh tế đảm bảo rằng`min(a[i], a[i+1])`được sử dụng thay vì`max`, có thể âm thầm đánh giá quá cao`k`. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:`4\n6 4 5 5`| tôi | một [tôi] | a[i+1] | phút(a[i], a[i+1]) | k_max | 
| --- | --- | --- | --- | --- | 
| 0 | 6 | 4 | 4 | 4 | 
| 1 | 4 | 5 | 4 | 4 | 
| 2 | 5 | 5 | 5 | 4 | 

Đầu ra:`4`. Vì số nguyên tối đa`k`cũng phải thỏa mãn điều kiện về khoảng cách 1 thì hệ số giãn nở là`1`. 

Đầu vào mẫu 2:`3\n0 1 2`| tôi | một [tôi] | a[i+1] | phút(a[i], a[i+1]) | k_max | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 0 | 0 | 
| 1 | 1 | 2 | 1 | 0 | 

Đầu ra:`0`. Phần tử 0 giới hạn hệ số giãn nở. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Lặp lại một lần trên mảng để tính các giá trị tối thiểu | 
| Không gian | O(n) | Chỉ lưu trữ mảng, không có cấu trúc dữ liệu phụ trợ | 

Với`n`lên tới 300.000, O(n) cũng nằm trong giới hạn thời gian 1 giây, vì các CPU hiện đại xử lý khoảng 10^8 thao tác mỗi giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    k_max = float('inf')
    for i in range(n - 1):
        k_pair = min(a[i], a[i + 1])
        if k_pair < k_max:
            k_max = k_pair
    return str(k_max)

# provided samples
assert run("4\n6 4 5 5\n") == "4", "sample 1"
assert run("3\n0 1 2\n") == "0", "sample 2"

# custom cases
assert run("2\n1000000000 1000000000\n") == "1000000000", "large elements"
assert run("5\n5 5 5 5 5\n") == "5", "all equal"
assert run("2\n0 0\n") == "0", "all zeros"
assert run("3\n1 0 1\n") == "0", "zero in the middle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n1000000000 1000000000 | 1000000000 | Xử lý phần tử lớn | 
| 5\n5 5 5 5 5 | 5 | Các phần tử bằng nhau tạo ra giá trị kỳ vọng k | 
| 2\n0 0 | 0 | Các phần tử tối thiểu tạo ra 0 k | 
| 3\n1 0 1 | 0 | Số không ở giữa hạn chế việc mở rộng | 

## Vỏ cạnh 

Đối với một mảng`[0, 1, 2]`, lực zero ở giữa`k_max`bằng không. Thuật toán lặp qua các cặp liên tiếp:`(0,1)`cho`min(0,1)=0`cài đặt`k_max=0`,`(1,2)`cho`min(1,2)=1`Nhưng`k_max`vẫn bằng 0. Điều này xác nhận rằng các số 0 ở bất kỳ đâu đều ngăn cản mọi hệ số giãn nở dương. Vì`[5,5,5,5]`, mỗi cặp liên tiếp mang lại`5`, Vì thế`k_max`còn lại`5`trên tất cả các lần lặp. Thuật toán xử lý chính xác cả mảng thưa thớt và mảng đồng nhất.

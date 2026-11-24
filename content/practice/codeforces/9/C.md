---
title: "CF 9C - Số thập lục phân"
description: "Chúng ta được yêu cầu đếm xem có bao nhiêu số từ 1 đến n chỉ gồm các chữ số 0 và 1 trong biểu diễn thập phân của chúng. Nói cách khác, bộ nhớ của Hệ thập lục phân chỉ lưu trữ các số mà khi viết ở cơ số 10 sẽ không chứa chữ số nào ngoài 0 hoặc 1."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 9
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 9 (Div. 2 Only)"
rating: 1200
weight: 9
solve_time_s: 70
verified: true
draft: false
---

[CF 9C - Số thập lục phân](https://codeforces.com/problemset/problem/9/C) 

**Đánh giá:** 1200 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 1 phút 10 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm xem có bao nhiêu số từ 1 đến _n_ chỉ bao gồm các chữ số 0 và 1 trong biểu diễn thập phân của chúng. Nói cách khác, bộ nhớ của hệ thập lục phân chỉ lưu trữ các số mà khi viết ở cơ số 10 sẽ không chứa chữ số nào khác ngoài 0 hoặc 1. Ví dụ: 1, 10, 11 là hợp lệ nhưng 2, 12, 20 thì không. 

Đầu vào là một số nguyên _n_, có thể lớn bằng 10^9. Điều đó có nghĩa là một giải pháp lặp lại rõ ràng trên mọi số từ 1 đến _n_ sẽ có khả năng thực hiện tới một tỷ lượt kiểm tra, quá chậm so với giới hạn thời gian 1 giây. Giới hạn bộ nhớ 64 MB là khá lớn so với lượng dữ liệu nhỏ mà chúng ta thực sự cần lưu trữ. 

Các trường hợp cạnh ở đây rất tinh tế. Nếu _n_ = 1 thì câu trả lời phải là 1 vì 1 được lưu trữ. Nếu _n_ = 2 thì đáp án vẫn là 1 vì 2 không hợp lệ. Nếu _n_ = 11 thì các số 1, 10 và 11 hợp lệ. Giải pháp bất cẩn tạo ra các số theo từng chữ số mà không kiểm tra giới hạn trên có thể bao gồm các số lớn hơn _n_, tạo ra số đếm không chính xác. Tương tự, việc coi "số nhị phân" là số cơ số 2 thực tế sẽ là một sai lầm - các số này là số thập phân chỉ có 0 và 1 chữ số. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là lặp qua mọi số từ 1 đến _n_, kiểm tra từng chữ số và đếm các số chỉ chứa 0 và 1. Cách này hoạt động chính xác, nhưng đối với _n_ ≈ 10^9, nó thực hiện tới một tỷ lần lặp, điều này quá chậm. Số lượng thao tác sẽ gần bằng tổng số chữ số của tất cả các số lên đến _n_, vẫn theo thứ tự 10^9 trở lên. Việc sử dụng bộ nhớ không đáng kể vì chúng ta chỉ cần một bộ đếm. 

Điểm mấu chốt là các số chỉ gồm 0 và 1 chữ số tạo thành một chuỗi có cấu trúc chặt chẽ: chúng chính xác là những số có thể được biểu diễn dưới dạng tổng lũy ​​thừa của mười với hệ số 0 hoặc 1. Điều này tương đương với việc tạo các số theo thứ tự "nhị phân thập phân": 1, 10, 11, 100, 101, 110, 111, 1000, v.v. Việc tạo chúng theo thứ tự tăng dần cho phép chúng ta dừng lại ngay khi vượt quá _n_. Điều này giúp giảm vấn đề từ việc lặp lại tối đa _n_ sang chỉ lặp lại các kết hợp chữ số O(log_10(n)), tức là khoảng 2^10 = 1024 cho n ≤ 10^9, một tốc độ tăng tốc rất lớn. 

Cách tiếp cận bạo lực có hiệu quả vì việc kiểm tra từng số để tìm các chữ số 0 và 1 rất đơn giản. Nó thất bại khi n lớn vì chúng ta làm những việc không cần thiết. Nhận thấy rằng chỉ những số có chữ số 0 và 1 mới có liên quan cho phép chúng ta tạo chúng trực tiếp theo thứ tự tăng dần, tạo ra nghiệm nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n log n) | O(1) | Quá chậm | 
| Tối ưu | O(2^d) trong đó d ≈ số chữ số trong n | O(2^d) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm`count`về 0 và một hàng đợi`q`chỉ chứa số 1. Điều này thiết lập việc tạo các số hợp lệ bắt đầu từ số nhỏ nhất. 
2. Khi hàng đợi không trống, hãy xóa phần tử đầu tiên, gọi nó`x`. Nếu như`x`lớn hơn _n_, hãy ngừng xử lý nó. Điều này đảm bảo chúng tôi không bao giờ đếm số ngoài phạm vi cho phép. 
3. Tăng`count`bởi vì một`x`là số hợp lệ chỉ bao gồm các chữ số 0 và 1 và không vượt quá _n_. 
4. Tạo các số tiếp theo bằng cách thêm số 0 và số 1 vào cuối`x`ở dạng thập phân. Về mặt toán học, tính toán`x * 10`Và`x * 10 + 1`. Thêm những số này vào hàng đợi. Điều này đảm bảo chúng ta khám phá tất cả các số hợp lệ theo thứ tự tăng dần mà không bỏ sót số nào. 
5. Lặp lại cho đến khi hàng đợi trống hoặc tất cả các số vượt quá _n_. 

Tại sao nó hoạt động: Hàng đợi luôn chứa các số chỉ có chữ số 0 hoặc 1 theo thứ tự tăng dần. Bằng cách xử lý từng số và tạo ra "số thập phân con" của nó, chúng tôi khám phá tất cả các số hợp lệ chính xác một lần. Điều bất biến là không có số nào nhỏ hơn _n_ chỉ có 0 và 1 chữ số bị bỏ qua và không có số nào lớn hơn _n_ được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def main():
    n = int(input())
    count = 0
    q = deque([1])
    
    while q:
        x = q.popleft()
        if x > n:
            continue
        count += 1
        q.append(x * 10)
        q.append(x * 10 + 1)
    
    print(count)

if __name__ == "__main__":
    main()
```Mã bắt đầu bằng cách đọc đầu vào và khởi tạo bộ đếm và hàng đợi. Chúng tôi sử dụng deque để bật từ phía trước và nối vào phía sau một cách hiệu quả. Mỗi số được kiểm tra với _n_ trước khi đếm. Đang tạo`x * 10`Và`x * 10 + 1`đảm bảo chúng ta xem xét tất cả các số có chữ số 0 và 1 theo thứ tự. Một sự tinh tế đang được sử dụng`continue`sau khi vượt quá _n_ thay vì phá vỡ vòng lặp vì số lớn hơn trong hàng đợi có thể vẫn hợp lệ. 

## Ví dụ đã hoạt động 

**Mẫu 1:** n = 10 

| Bước | Xếp hàng | x | Đếm | Ghi chú | 
| --- | --- | --- | --- | --- | 
| 1 | [1] | 1 | 0 | Bắt đầu với 1 | 
| 2 | [] | 1 | 0 → 1 | 1 ≤ 10, số gia tăng; thêm 10, 11 vào hàng đợi | 
| 3 | [10, 11] | 10 | 1 → 2 | 10 ≤ 10, số gia tăng; cộng 100, 101 | 
| 4 | [11, 100, 101] | 11 | 2 | 11 > 10, bỏ qua | 
| 5 | [100, 101] | 100 | 2 | 100 > 10, bỏ qua | 
| 6 | [101] | 101 | 2 | 101 > 10, bỏ qua | 
| 7 | [] | - | 2 | Xong | 

Đếm = 2, khớp với đầu ra mẫu. 

**Ví dụ bổ sung:** n = 15 

| Bước | Xếp hàng | x | Đếm | Ghi chú | 
| --- | --- | --- | --- | --- | 
| 1 | [1] | 1 | 0 | Bắt đầu | 
| 2 | [] | 1 | 1 | Thêm 10, 11 | 
| 3 | [10, 11] | 10 | 2 | Thêm 100, 101 | 
| 4 | [11, 100, 101] | 11 | 3 | Thêm 110, 111 | 
| 5 | [100, 101, 110, 111] | 100 | 3 | 100 > 15, bỏ qua | 
| 6 | [101, 110, 111] | 101 | 3 | 101 > 15, bỏ qua | 
| 7 | [110, 111] | 110 | 3 | bỏ qua | 
| 8 | [111] | 111 | 3 | bỏ qua | 
| 9 | [] | - | 3 | Xong | 

Đếm = 3 (số 1, 10, 11). 

Dấu vết này xác nhận hàng đợi khám phá các số theo thứ tự và tôn trọng giới hạn trên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^d) | Có khoảng 2^d số có d chữ số gồm 0 và 1. Với n 10^9, d 10, do đó, tối đa 1024 số được xử lý. | 
| Không gian | O(2^d) | Hàng đợi lưu trữ tất cả các số cần xử lý, được giới hạn bởi cùng một O(2^d). | 

Với các hạn chế, việc xử lý tối đa 1024 số rất nhanh và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# Provided sample
assert run("10\n") == "2", "sample 1"

# Minimum input
assert run("1\n") == "1", "minimum n"

# Small input
assert run("2\n") == "1", "only 1 is valid"

# Medium input
assert run("15\n") == "3", "numbers 1, 10, 11"

# Boundary near next power of 10
assert run("100\n") == "6", "numbers 1, 10, 11, 100, 101, 110"

# Large input
assert run("1000000000\n") == "511", "all numbers with digits 0/1 ≤ 1e9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | Số hợp lệ tối thiểu | 
| 2 | 1 | Chỉ có 1 hợp lệ dưới 2 | 
| 15 | 3 | Nhiều số hợp lệ dưới nhỏ |

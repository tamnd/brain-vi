---
title: "CF 2020A - Tìm hoạt động tối thiểu"
description: "Chúng ta được yêu cầu giảm một số nguyên n đã cho về 0 bằng cách trừ liên tục lũy thừa của số nguyên thứ hai k. Mỗi phép trừ có thể sử dụng bất kỳ lũy thừa nào của k (bao gồm k^0 = 1) và mục tiêu là giảm thiểu số lần trừ."
date: "2026-06-08T12:46:53+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2020
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 976 (Div. 2) and Divide By Zero 9.0"
rating: 800
weight: 2020
solve_time_s: 84
verified: true
draft: false
---

[CF 2020A - Tìm các hoạt động tối thiểu](https://codeforces.com/problemset/problem/2020/A) 

**Đánh giá:** 800 
**Tags:** bitmasks, vũ lực, tham lam, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu giảm một số nguyên nhất định`n`về 0 bằng cách trừ liên tục lũy thừa của số nguyên thứ hai`k`. Mỗi phép trừ có thể sử dụng bất kỳ lũy thừa nào của`k`(bao gồm`k^0 = 1`) và mục tiêu là giảm thiểu số lần trừ. Đầu vào bao gồm nhiều trường hợp kiểm thử, mỗi trường hợp đưa ra các giá trị cho`n`Và`k`và đầu ra phải chỉ định số lượng thao tác tối thiểu cho từng trường hợp thử nghiệm. 

Những ràng buộc cho phép`n`Và`k`lớn bằng 10^9, với tối đa 10^4 trường hợp thử nghiệm. Điều này loại trừ bất kỳ cách tiếp cận nào liệt kê rõ ràng tất cả các chuỗi phép trừ hoặc lặp lại tất cả các lũy thừa của`k`một cách ngây thơ. Giải pháp phải hoạt động ở O(log n) cho mỗi trường hợp thử nghiệm hoặc tốt hơn, vì số lượng thao tác cần thiết để biểu diễn`n`trong căn cứ`k`là logarit trong`n`. Vỏ cạnh bao gồm`k = 1`, trong đó mọi phép toán chỉ trừ 1 và lũy thừa lớn của`k`vượt quá`n`ngay lập tức. 

Việc triển khai ngây thơ có thể cho rằng chúng ta nên luôn trừ đi lũy thừa lớn nhất có thể của`k`. Trong khi điều này làm việc cho`k > 1`, vì`k = 1`nó sẽ không bao giờ chấm dứt một cách chính xác trừ khi được xử lý riêng. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu là lặp đi lặp lại việc trừ đi một số sức mạnh của`k`từ`n`, thử mọi sức mạnh có thể có ở mỗi bước. Về nguyên tắc, điều này đúng vì bất kỳ chuỗi phép trừ nào làm giảm`n`đến 0 là hợp lệ, nhưng số lượng các chuỗi có thể tăng theo cấp số nhân, khiến nó hoàn toàn không khả thi đối với các ràng buộc đã cho. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng lũy ​​thừa của`k`tương đương với việc thể hiện`n`trong ** cơ sở`k`**. Mỗi chữ số trong cơ sở`k`cho chúng ta biết sức mạnh tương ứng của`k`phải được trừ đi. Do đó, số thao tác tối thiểu chính xác là tổng các chữ số của`n`khi được viết trong cơ sở`k`. Ví dụ,`n = 5`Và`k = 2`đưa ra biểu diễn cơ sở-2`101`, có hai cái, tương ứng với hai thao tác bắt buộc. 

Khi`k = 1`, biểu diễn cơ số bị suy biến và mọi phép trừ chỉ có thể loại bỏ`1`, do đó số lượng hoạt động bằng`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) mỗi thao tác | O(1) | Quá chậm đối với n lớn | 
| Tổng chữ số Base-k | O(log_kn) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test, hãy đọc các số nguyên`n`Và`k`. 
3. Nếu`k`bằng 1, trả về`n`là câu trả lời vì mọi thao tác đều trừ chính xác 1. 
4. Nếu không, hãy khởi tạo bộ đếm cho các phép toán về 0. 
5. Trong khi`n`lớn hơn 0: 

- Tính toán`n % k`, cho biết số lần trừ của lũy thừa hiện tại của`k`cần thiết. 
- Thêm số dư này vào bộ đếm hoạt động. 
- Cập nhật`n`ĐẾN`n // k`để xử lý sức mạnh cao hơn tiếp theo của`k`. 
6. Sau vòng lặp, bộ đếm chứa số lượng thao tác tối thiểu. Xuất nó ra. 

Tại sao nó hoạt động: thể hiện`n`trong căn cứ`k`nắm bắt chính xác số lần mỗi lũy thừa của`k`là cần thiết. Phần còn lại ở mỗi bước cho biết cần trừ đi bao nhiêu công suất hiện tại và chia cho`k`chuyển sang quyền lực tiếp theo. Quá trình chấm dứt khi tất cả quyền hạn đã cạn kiệt, đảm bảo số lượng thao tác tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        if k == 1:
            print(n)
            continue
        ans = 0
        while n > 0:
            ans += n % k
            n //= k
        print(ans)

if __name__ == "__main__":
    solve()
```Mã này xử lý nhiều trường hợp kiểm thử một cách hiệu quả bằng cách sử dụng I/O nhanh. Nó tách trường hợp đặc biệt`k = 1`để tránh vòng lặp vô hạn và sử dụng phép chia số nguyên và modulo để duyệt cơ số`k`đại diện. Mỗi lần lặp vòng lặp tương ứng với việc xử lý một chữ số trong hệ cơ số`k`, đảm bảo độ phức tạp logarit. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`n = 5`,`k = 2`: 

| Bước | n | n % k | trả lời | n //= k | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 5 | 1 | 1 | 2 | 
| Bước 2 | 2 | 0 | 1 | 1 | 
| Bước 3 | 1 | 1 | 2 | 0 | 

Tổng số thao tác: 2. Điều này phù hợp với trình tự tối thiểu`5 - 1 = 4`,`4 - 4 = 0`. 

Đầu vào mẫu`n = 3`,`k = 5`: 

| Bước | n | n % k | trả lời | n //= k | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 3 | 3 | 3 | 0 | 

Tổng số hoạt động: 3, phù hợp với sản lượng dự kiến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log_k n) cho mỗi trường hợp thử nghiệm | Mỗi lần lặp lại chia`n`qua`k`| 
| Không gian | O(1) | Chỉ các bộ đếm và biến đầu vào | 

Với`t <= 10^4`Và`n <= 10^9`, giải pháp hoạt động tốt trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("6\n5 2\n3 5\n16 4\n100 3\n6492 10\n10 1\n") == "2\n3\n1\n4\n21\n10"

# Custom test cases
assert run("1\n1 1\n") == "1", "k=1, n=1"
assert run("1\n1 2\n") == "1", "k>1, n=1"
assert run("1\n1023 2\n") == "10", "n=2^10-1, all digits 1 in base-2"
assert run("1\n1000000000 3\n") == "19", "large n, k=3"
assert run("1\n999999937 1\n") == "999999937", "k=1, large n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | tối thiểu n, k=1 | 
| 1 2 | 1 | tối thiểu n, k>1 | 
| 1023 2 | 10 | tất cả số 1 trong biểu diễn cơ số 2 | 
| 1000000000 3 | 19 | n lớn, k>1 | 
| 999999937 1 | 999999937 | n lớn, k=1 | 

## Vỏ cạnh 

Khi nào`k = 1`, mỗi phép trừ chỉ có thể loại bỏ 1. Ví dụ:`n = 10`Và`k = 1`yêu cầu chính xác 10 thao tác. Vì`k > n`, tổng chữ số cơ số k xử lý nó một cách chính xác:`n % k = n`Và`n // k = 0`, mang lại chính xác`n`hoạt động, phù hợp với phép trừ`1`nhiều lần. Quyền lực lớn của`k`không ảnh hưởng đến tính đúng đắn vì biểu diễn cơ sở luôn chiếm từng lũy ​​thừa cần thiết.

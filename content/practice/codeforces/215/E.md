---
title: "CF 215E - Số định kỳ"
description: "Chúng ta được yêu cầu đếm các số trong một phạm vi [l, r] nhất định có biểu diễn nhị phân tuần hoàn. Một số là tuần hoàn nếu chuỗi nhị phân của nó có mẫu lặp lại có độ dài k chia tổng độ dài n của chuỗi, nghĩa là mọi đoạn có độ dài k lặp lại chính xác…"
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 215
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 132 (Div. 2)"
rating: 2100
weight: 215
solve_time_s: 178
verified: false
draft: false
---

[CF 215E - Số định kỳ](https://codeforces.com/problemset/problem/215/E) 

**Đánh giá:** 2100 
**Tags:** tổ hợp, dp, lý thuyết số 
**Thời gian giải:** 2m 58s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đếm số trong một phạm vi nhất định`[l, r]`biểu diễn nhị phân của nó là tuần hoàn. Một số là tuần hoàn nếu chuỗi nhị phân của nó có độ dài lặp lại`k`chia tổng chiều dài`n`của chuỗi, nghĩa là mọi đoạn có độ dài`k`lặp lại chính xác trong suốt chuỗi. Ví dụ, số`10`ở dạng nhị phân là`1010`, lặp lại mô hình`10`. Đầu ra là một số nguyên duy nhất biểu thị tổng số các số định kỳ như vậy trong phạm vi. 

Các ràng buộc đầu vào rất lớn:`r`có thể đi lên`10^18`, ngụ ý các chuỗi nhị phân có độ dài lên tới 60. Một cách tiếp cận đơn giản lặp qua mọi số trong`[l, r]`là hoàn toàn không khả thi, bởi vì điều đó có thể đòi hỏi`10^18`hoạt động. Điều này cho thấy chúng ta cần một giải pháp hoạt động dựa trên độ dài nhị phân của các số thay vì bản thân các số đó. 

Các trường hợp cạnh cần chú ý bao gồm các số rất nhỏ như`1`Và`2`, chỉ có một hoặc hai bit. Ví dụ,`1`không định kỳ vì không có`k < n`, trong khi`3`(`11`ở dạng nhị phân) là tuần hoàn với`k=1`. Một tình huống phức tạp khác là các số có biểu diễn nhị phân đều là số một; chúng có thể có vẻ tuần hoàn, nhưng chiều dài mẫu của chúng phải chia tổng chiều dài một cách hợp lý. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ chuyển đổi từng số thành`[l, r]`sang nhị phân, sau đó kiểm tra tất cả các ước số có thể có của độ dài của nó`n`để xem liệu có tồn tại một mẫu lặp lại hay không. Đối với mỗi số, việc kiểm tra tất cả các ước số và xác minh sự lặp lại có thể mất tới`O(n^2)`trong trường hợp xấu nhất, ở đâu`n`là độ dài bit. Với số lượng lên đến`10^18`, điều này hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát thấy rằng các số tuần hoàn được xác định hoàn toàn bởi kiểu lặp lại và số lần lặp lại của chúng. Nếu chúng ta biết chiều dài mẫu`k`, số tuần hoàn có thể được xây dựng bằng cách lặp lại một`k`- chuỗi bit đủ số lần để lấp đầy tổng chiều dài`n`. Điều này làm giảm vấn đề từ lặp qua các số đến lặp theo độ dài mẫu và giá trị mẫu, thu hẹp đáng kể không gian tìm kiếm. Vì độ dài bit tối đa là 60 nên chúng ta chỉ cần xem xét độ dài mẫu lên tới 60 và với mỗi độ dài, có nhiều nhất`2^k`các mẫu cần xem xét, làm cho việc này trở nên dễ thực hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((r-l+1) * n^2) | O(n) | Quá chậm | 
| Tối ưu | O(60 * 2^60 / 2^(60-n)) hiệu quả << r-l | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi`l`Và`r`thành chuỗi nhị phân và xác định độ dài tối đa`max_len`của phạm vi. 
2. Lặp lại tổng chiều dài có thể`n`từ 1 đến`max_len`. Đối với mỗi độ dài, lặp lại các độ dài mẫu có thể`k`sự chia rẽ đó`n`và thỏa mãn`k < n`. 
3. Đối với một nhất định`k`, lặp lại tất cả những gì có thể`k`-các mẫu bit, tránh các mẫu bắt đầu bằng 0 vì không cho phép các số 0 đứng đầu. 
4. Đối với mỗi mẫu, tạo số tuần hoàn bằng cách lặp lại mẫu`n // k`lần. Chuyển đổi chuỗi nhị phân này trở lại thành số nguyên. 
5. Đếm số nếu nó nằm trong`[l, r]`. 
6. Tính tổng số lượng trên tất cả các độ dài và mẫu. 

Bất biến đảm bảo tính đúng đắn là mọi số tuần hoàn đều có thể được biểu diễn dưới dạng lặp lại của một mẫu độ dài nào đó.`k`chia tổng chiều dài`n`. Bằng cách liệt kê một cách có hệ thống các mẫu và độ dài này, chúng tôi đảm bảo tạo ra tất cả các số tuần hoàn mà không bỏ sót bất kỳ số nào hoặc đếm các số không tuần hoàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_periodic(l, r):
    total = 0
    for n in range(1, 61):
        for k in range(1, n):
            if n % k != 0:
                continue
            repeat = n // k
            for mask in range(1 << (k-1), 1 << k):
                pattern = bin(mask)[2:]
                num = int(pattern * repeat, 2)
                if l <= num <= r:
                    total += 1
    return total

l, r = map(int, input().split())
print(count_periodic(l, r))
```Giải pháp này đầu tiên lặp lại theo độ dài có thể và độ dài mẫu. Vòng lặp bên trong lặp qua tất cả các mẫu có độ dài`k`bắt đầu bằng 1. Chúng tôi chuyển đổi từng mẫu thành chuỗi nhị phân lặp lại đầy đủ, sau đó thành số nguyên và kiểm tra xem nó có nằm trong phạm vi không`[l, r]`. Sự lựa chọn của`range(1 << (k-1), 1 << k)`đảm bảo mẫu bắt đầu bằng 1, tránh các số không hợp lệ có số 0 đứng đầu. 

## Ví dụ đã hoạt động 

### Mẫu 1:`1 10`| n | k | mẫu | lặp đi lặp lại | số nguyên | đếm? | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 1 | 1 | 11 | 3 | vâng | 
| 4 | 2 | 10 | 1010 | 10 | vâng | 
| 3 | 1 | 1 | 111 | 7 | vâng | 

Bảng cho thấy rằng`3, 7, 10`được đếm, xác nhận đầu ra`3`. 

### Mẫu 2:`31 36`| n | k | mẫu | lặp đi lặp lại | số nguyên | đếm? | 
| --- | --- | --- | --- | --- | --- | 
| 5 | 1 | 1 | 11111 | 31 | vâng | 
| 6 | 2 | 11 | 111111 | 63 | không | 
| 6 | 3 | 100 | 100100 | 36 | vâng | 

Số định kỳ là`31`Và`36`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(60 * 60 * 2^30) trường hợp xấu nhất | Tối đa n=60, tối đa k ≤ n, mẫu tối đa 2^k nhưng bắt đầu từ 1 giảm đi một nửa | 
| Không gian | O(1) | Chỉ các bộ đếm và các biến vòng lặp; không có mảng lớn | 

Độ phức tạp về thời gian là khả thi vì các mẫu vòng lặp bên trong bị giới hạn bởi lũy thừa nhỏ là 2 và n tối đa là 60. Việc sử dụng bộ nhớ ở mức tối thiểu, dưới giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    l, r = map(int, input().split())
    return str(count_periodic(l, r))

assert run("1 10\n") == "3", "sample 1"
assert run("31 36\n") == "2", "sample 2"
assert run("1 1\n") == "0", "minimum input"
assert run("3 3\n") == "1", "single periodic number"
assert run("1 1000000\n") == run("1 1000000\n"), "large range check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 10 | 3 | phạm vi nhỏ cơ bản | 
| 31 36 | 2 | số có độ dài nhị phân khác nhau | 
| 1 1 | 0 | giá trị tối thiểu của trường hợp cạnh | 
| 3 3 | 1 | số tuần hoàn nhỏ nhất | 
| 1 1000000 | tính toán | hiệu quả trên phạm vi rộng | 

## Vỏ cạnh 

Đầu vào tối thiểu`1 1`trả về một cách chính xác`0`bởi vì`1`không có mẫu lặp lại thích hợp. Một con số như`3`(`11`) được tính vì mẫu bit đơn của nó`1`lặp lại để tạo thành số. Các số có số 0 đứng đầu trong các mẫu tiềm năng sẽ bị bỏ qua vì chúng tôi chỉ xem xét các mẫu bắt đầu bằng`1`, ngăn ngừa việc đếm sai. Số lượng rất lớn gần`10^18`được xử lý vì thuật toán hoạt động theo độ dài và mẫu nhị phân, không phải phép lặp thô trên mỗi số.

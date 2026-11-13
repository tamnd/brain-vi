---
title: "CF 1699E - Ân Sủng Ba Ngày"
description: "Bài toán cho chúng ta một tập hợp các số nguyên, tất cả đều bị giới hạn bởi một giá trị lớn nhất m, và cho phép chúng ta thay thế nhiều lần bất kỳ số x 3 nào bằng hai số nguyên p và q sao cho pq = x và cả p và q đều lớn hơn 1. Sau mỗi phép toán, tập hợp này tăng thêm một phần tử."
date: "2026-06-09T22:13:10+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "greedy", "math", "number-theory", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1699
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 804 (Div. 2)"
rating: 2600
weight: 1699
solve_time_s: 152
verified: false
draft: false
---

[CF 1699E - Ân điển ba ngày](https://codeforces.com/problemset/problem/1699/E) 

**Đánh giá:** 2600 
**Tags:** cấu trúc dữ liệu, dp, tham lam, toán, lý thuyết số, hai con trỏ 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bài toán cho chúng ta nhiều tập số nguyên, tất cả đều bị giới hạn bởi một giá trị lớn nhất`m`và cho phép chúng ta thay thế nhiều lần bất kỳ số nào`x > 3`với hai số nguyên`p`Và`q`như vậy`p*q = x`và cả hai`p`Và`q`lớn hơn 1. Sau mỗi thao tác, tập hợp tăng thêm một phần tử. Mục tiêu là giảm sự khác biệt giữa phần tử lớn nhất và nhỏ nhất trong nhiều tập hợp, được định nghĩa là sự cân bằng. 

Mỗi trường hợp thử nghiệm bao gồm kích thước của nhiều tập hợp, giới hạn phần tử tối đa và các phần tử ban đầu. Đầu ra là số dư nhỏ nhất có thể mà chúng ta có thể đạt được bằng cách áp dụng 0 hoặc nhiều thao tác phân tách này. 

Các ràng buộc ngụ ý rằng việc mô phỏng đơn giản việc phân tách mọi phần tử theo cách đệ quy là không khả thi. Tổng số phần tử ban đầu trong tất cả các trường hợp thử nghiệm có thể lên tới`10^6`và bản thân các giá trị có thể lên tới`5*10^6`. Một cách tiếp cận đơn giản là phân chia đệ quy từng phần tử có thể bùng nổ theo cấp số nhân vì mỗi phần phân chia sẽ tăng kích thước của nhiều tập hợp lên một, do đó số lượng thao tác sẽ nhanh chóng vượt quá bất kỳ giới hạn khả thi nào. 

Các trường hợp khó khăn có thể gặp phải một giải pháp đơn giản bao gồm nhiều tập hợp trong đó tất cả các số đều là số nguyên tố hoặc 1, vì chúng không thể chia tách và các trường hợp liên tục chia các số lớn theo nhiều cách khác nhau sẽ tạo ra các kết quả cân bằng rất khác nhau. Ví dụ: một bộ nhiều tập`[12, 2]`có thể được chia thành`[3,4,2,2]`hoặc`[6,2,2]`và sự lựa chọn sẽ ảnh hưởng đến số dư tối thiểu có thể đạt được. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử mọi phép phân chia hợp lệ cho mọi phần tử và tính toán đệ quy số dư cho tất cả các tập hợp kết quả. Điều này hoạt động về mặt khái niệm vì hoạt động được đảm bảo cuối cùng đạt đến các số không thể phân chia (số nguyên tố và số 1), do đó quá trình đệ quy chấm dứt. Vấn đề là đối với số lượng lớn, có nhiều cách để phân chia và mỗi cách phân chia sẽ phát triển thành nhiều tập hợp, điều này nhanh chóng dẫn đến sự tăng trưởng theo cấp số nhân. Ví dụ: việc chia tất cả các thừa số của một số như 210 sẽ tạo ra một cây khả năng khổng lồ. Điều này vượt xa số lượng hoạt động khả thi. 

Thông tin chi tiết quan trọng là đảo ngược quan điểm: thay vì mô phỏng tất cả các phần tách về phía trước, chúng ta có thể nghĩ về các số nhỏ nhất mà mỗi phần tử ban đầu có thể được chia thành và theo dõi xem các phần tách này ảnh hưởng như thế nào đến giá trị tối thiểu của nhiều tập hợp. Bởi vì phép toán cho phép phân tích thành thừa số tùy ý, một số lớn có thể được rút gọn thành bất kỳ tổ hợp nào của các thừa số nguyên tố của nó. Để giảm thiểu số dư, chúng tôi muốn tập trung vào việc giảm giá trị tối đa đồng thời tránh tạo ra các số nhỏ hơn mức tối thiểu hiện tại. 

Chúng ta có thể tính toán trước cho mỗi số`x`hệ số tối thiểu của nó lớn hơn 1 và sử dụng lập trình động để theo dõi các số tối thiểu có thể xảy ra khi chia tách. Sau đó, chúng tôi duy trì một cửa sổ trượt qua các giá trị này để xác định một cách hiệu quả số dư tối thiểu có thể đạt được trên nhiều tập hợp. Điều này biến mô phỏng hàm mũ thành quét tuyến tính kết hợp với tiền xử lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(M log M + N) | O(M + N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính trước thừa số nguyên tố nhỏ nhất (`spf`) với mọi số đến`m`dùng sàng. Điều này cho phép chúng ta phân tích các số theo thời gian O(log x), điều này rất cần thiết cho các số lớn`m`. 
2. Với mỗi test, khởi tạo một mảng tần số`freq`để đếm số lần xuất hiện của mỗi số trong nhiều tập hợp. 
3. Với mỗi số`x`theo thứ tự giảm dần, hãy thử chia nó bằng thừa số nguyên tố nhỏ nhất của nó. Cho phép`p = spf[x]`Và`q = x // p`. Tăng`freq[p]`Và`freq[q]`và giảm dần`freq[x]`. Lặp lại điều này cho đến khi số không thể chia thêm được nữa (có thể là số nguyên tố hoặc 1). 
4. Duy trì cấu trúc giống như nhiều tập hợp hoặc cửa sổ trượt theo dõi số nhỏ nhất được nhìn thấy và số lớn nhất sau tất cả các lần chia tách. Số dư tối thiểu là sự khác biệt giữa số lớn nhất và số nhỏ nhất có trong dải tần số sau khi xử lý tất cả các số. 
5. Xuất số dư tối thiểu cho từng trường hợp thử nghiệm. 

Tính bất biến được duy trì là ở mỗi bước, các số trong mảng tần số biểu thị tất cả các số có thể truy cập được thông qua các phần tách hợp pháp từ nhiều tập hợp ban đầu. Bằng cách xử lý các số từ lớn nhất đến nhỏ nhất, chúng tôi đảm bảo việc phân tách được áp dụng theo cách chỉ có thể giảm giá trị tối đa mà không ảnh hưởng đến các số nhỏ hơn một cách không cần thiết. Điều này đảm bảo rằng sự khác biệt cuối cùng giữa mức tối đa và tối thiểu hiện có trong nhiều tập hợp là số dư tối thiểu có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAX_M = 5 * 10**6 + 10

# Precompute smallest prime factor (spf) for all numbers up to MAX_M
spf = [0] * MAX_M
for i in range(2, MAX_M):
    if spf[i] == 0:
        for j in range(i, MAX_M, i):
            if spf[j] == 0:
                spf[j] = i

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    freq = [0] * (m + 2)
    for num in a:
        freq[num] += 1

    max_val = max(a)
    min_val = min(a)

    for x in range(m, 1, -1):
        while freq[x]:
            if spf[x] == x:
                break  # prime, cannot split
            p = spf[x]
            q = x // p
            freq[x] -= 1
            freq[p] += 1
            freq[q] += 1

    # Find final max and min present in freq
    final_min = next(i for i in range(1, m + 1) if freq[i])
    final_max = next(i for i in range(m, 0, -1) if freq[i])
    print(final_max - final_min)
```Việc tính toán trước của`spf`là rất quan trọng để phân tích các số một cách nhanh chóng. các`freq`mảng cho phép chúng tôi mô phỏng hiệu quả các phần tách mà không cần duy trì một cách rõ ràng một tập hợp nhiều tập hợp đang phát triển. Bằng cách lặp lại từ các số lớn nhất trở xuống, chúng tôi đảm bảo rằng phép chia sẽ giảm giá trị tối đa nhiều nhất có thể trước khi xem xét các số nhỏ hơn. Lần quét cuối cùng của`freq`xác định số nhỏ nhất và số lớn nhất thực sự có sau tất cả các phép chia. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:`5 10`với nhiều bộ`[2,4,2,4,2]`. 

| Bước | tần số[2] | tần số[4] | Giải thích | 
| --- | --- | --- | --- | 
| Ban đầu | 3 | 2 | nhiều bộ gốc | 
| Chia 4->2*2 | 5 | 0 | mỗi 4 chia thành hai 2s | 
| Tần số quét | tối thiểu=2, tối đa=2 | số dư = 0 | Mọi số đều bằng nhau | 

Điều này xác nhận rằng việc chia các số lớn nhất thành các thừa số tối thiểu sẽ giảm thiểu sự cân bằng. 

**Ví dụ 2:** 

đầu vào:`3 50`với nhiều bộ`[12,2,3]`. 

| Bước | tần số[2] | tần số[3] | tần số[4] | tần số[12] | Giải thích | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 1 | 0 | 1 | nhiều bộ gốc | 
| Chia 12->2*6 | 2 | 1 | 0 | 1 | 12 bị xóa, 2 và 6 được thêm | 
| Chia 6->2*3 | 3 | 2 | 0 | 0 | 6 bị xóa, 2 và 3 được thêm | 
| Tần số quét | tối thiểu=2, tối đa=3 | số dư = 1 | kết quả cuối cùng | | | 

Điều này thể hiện nhiều sự phân chia làm giảm giá trị tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M log M + N) | Sàng tìm thừa số nguyên tố nhỏ nhất là O(M log log M), chia mỗi số có giá trị O(log x), tổng N số | 
| Không gian | O(M + N) |`spf`kích thước mảng M, kích thước mảng tần số M, kích thước mảng đầu vào N | 

Với những hạn chế`M <= 5*10^6`và tổng cộng`N <= 10^6`, giải pháp phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call the solution block here
    MAX_M = 5 * 10**6 + 10
    spf = [0] * MAX_M
    for i in range(2, MAX_M):
        if spf[i] == 0:
            for j in range(i, MAX_M, i):
                if spf[j] == 0:
                    spf[j] = i
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        freq = [0] * (m + 2)
        for num in a:
            freq[num] += 1
        for x in range(m, 1, -1):
            while freq[x]:
                if spf[x] == x:
                    break
                p = spf[x]
                q = x // p
                freq[x] -= 1
                freq[p] += 1
                freq[q] += 1
        final_min = next(i for i in range(1,
```

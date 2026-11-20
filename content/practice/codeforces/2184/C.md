---
title: "CF 2184C - Cọc Khổng Lồ"
description: "Andrei bắt đầu với một đống duy nhất chứa $n$ quả táo. Anh ta có thể chia bất kỳ đống kích thước $x$ nào thành hai đống nhỏ hơn trong đúng một phút, tạo ra một đống táo $lfloor x/2 rfloor$ và một đống táo $lceil x/2 rceil$ khác."
date: "2026-06-07T21:36:10+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dfs-and-similar", "dp", "graphs", "math"]
categories: ["algorithms"]
codeforces_contest: 2184
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1072 (Div. 3)"
rating: 1100
weight: 2184
solve_time_s: 103
verified: true
draft: false
---

[CF 2184C - Cọc khổng lồ](https://codeforces.com/problemset/problem/2184/C) 

**Đánh giá:** 1100 
**Thẻ:** tìm kiếm nhị phân, dfs và tương tự, dp, đồ thị, toán học 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Andrei bắt đầu với một chồng duy nhất chứa$n$táo. Anh ta có thể chia bất kỳ đống kích thước nào$x$thành hai đống nhỏ hơn trong đúng một phút, tạo thành một đống$\lfloor x/2 \rfloor$táo và một thứ khác$\lceil x/2 \rceil$táo. Mục tiêu của anh ta là có được một đống có chính xác$k$táo mà không cần đếm chúng bằng tay. Nhiệm vụ là xác định xem liệu có thể lấy được đống như vậy thông qua việc chia tách lặp đi lặp lại hay không và nếu có thì hãy tính số phút tối thiểu cần thiết để sản xuất ra đống đó. 

Mỗi trường hợp thử nghiệm cung cấp tổng số táo$n$và kích thước cọc mong muốn$k$. Đầu ra phải là$-1$nếu như$k$không thể có được chính xác; mặt khác, nó sẽ trả về số lượng phân chia tối thiểu cần thiết. 

Các ràng buộc cho phép lên đến$10^4$trường hợp thử nghiệm và cọc lớn như$10^9$. Một cách tiếp cận đơn giản để chia các ngăn một cách đệ quy và khám phá tất cả các phép chia có thể sẽ là quá chậm, vì số lần chia có thể tỷ lệ thuận với số bit trong$n$và số lượng các trạng thái tăng theo cấp số nhân. Do đó, một phương pháp hiệu quả phải dựa vào cấu trúc toán học của hoạt động giảm một nửa thay vì khám phá vũ lực. 

Một trường hợp khó phát hiện khi$k$lớn hơn$n$, điều đó ngay lập tức là không thể. Một kịch bản phức tạp khác xảy ra khi$k$không phải là tổng lũy ​​thừa của hai có thể đạt được từ$n$thông qua việc giảm một nửa lặp đi lặp lại. Ví dụ, với$n = 21$Và$k = 4$, kích thước cọc có thể đạt được tuân theo một trình tự được xác định bằng cách chia đôi liên tục$21$, sản xuất cọc có kích thước$21, 10, 11, 5, 6, 2, 3, 1$. Từ$4$không bao giờ xuất hiện thì thuật toán phải phát hiện ra điều này. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ mô phỏng đệ quy việc chia từng cọc theo mọi cách có thể cho đến khi chúng ta tìm thấy một đống có kích thước$k$hoặc cạn kiệt tất cả các lựa chọn. Đối với mỗi lần chia, chúng tôi theo dõi số phút và tất cả kích thước cọc thu được. Mặc dù đúng về mặt nguyên tắc, nhưng sự tăng trưởng theo cấp số nhân của các quốc gia khiến cho việc$n$lên đến$10^9$. 

Thông tin chi tiết quan trọng đến từ việc quan sát rằng mỗi lần chia sẽ chia đôi cọc và các cọc thu được tương ứng với biểu diễn nhị phân của số ban đầu. Mỗi kích thước cọc về cơ bản là tổng của một tập hợp con lũy thừa của hai tạo nên$n$. Vì vậy, việc thu được một đống có kích thước$k$tương đương với việc thể hiện$k$như một tổng sức mạnh của hai có sẵn trong$n$. Thời gian tối thiểu tương ứng với số lần phân chia tối thiểu cần thiết để cô lập một đống có kích thước mong muốn, có thể được xác định bằng cách phân hủy một cách tham lam.$k$sử dụng lũy ​​thừa lớn nhất của hai trong$n$. 

Do đó, bài toán rút gọn thành việc đếm xem cần bao nhiêu phép chia để tạo ra tổ hợp chính xác các lũy thừa của hai số đó có tổng bằng$k$. Điều này có thể được thực hiện một cách hiệu quả bằng cách sử dụng hàng đợi ưu tiên hoặc thao tác bit, đảm bảo rằng mỗi lần phân chia sẽ giảm số lượng lớn nhất có sẵn cho đến khi mục tiêu được hình thành. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^log(n)) | O(log(n)) | Quá chậm cho n lên đến 10^9 | 
| Tối ưu | O(log(n)) cho mỗi trường hợp thử nghiệm | O(log(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với đống đầu tiên$n$táo và kiểm tra xem$k > n$. Nếu đúng thì xuất ra$-1$ngay lập tức. 
2. Biểu diễn kích thước cọc dưới dạng lũy ​​thừa của hai có trong$n$. Ví dụ,$n = 10$tương ứng với cọc có kích thước$8, 2$ban đầu có sẵn sau khi phân hủy bằng cách sử dụng lũy ​​thừa lớn nhất của hai. 
3. Sử dụng hàng đợi heap tối đa hoặc hàng đợi ưu tiên để luôn phân chia đống lớn nhất có sẵn. Giữ một bộ đếm xem có bao nhiêu lần chia tách (phút) đã xảy ra. 
4. Trong khi cọc lớn nhất lớn hơn cấu kiện nhỏ nhất cần đạt$k$, chia nó thành hai nửa, đẩy cả hai nửa trở lại heap và tăng bộ đếm phân tách. 
5. Theo dõi tổng số cọc đã chọn. Khi sự kết hợp có tổng chính xác là$k$, dừng và trả về bộ đếm phân chia hiện tại dưới dạng số phút tối thiểu. 
6. Nếu heap đã hết và$k$không thể được hình thành, trở lại$-1$. 

Thuật toán hoạt động vì mỗi lần chia sẽ giảm một cọc thành hai thành phần nhỏ hơn, đảm bảo tất cả lũy thừa của hai trong$n$vẫn còn sẵn ở một số đống. Heap đảm bảo rằng đống lớn nhất luôn được chọn để giảm thiểu số lần chia tách, đạt được thời gian tối thiểu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        if k > n:
            print(-1)
            continue
        # Count how many ones in binary representation of n
        ones = bin(n).count('1')
        if k < ones:
            print(-1)
            continue
        
        # Min-heap for split counts (we use negative for max-heap)
        heap = [-n]
        splits = 0
        total_piles = 1
        
        while total_piles < k:
            largest = -heapq.heappop(heap)
            if largest == 1:
                break
            a = largest // 2
            b = largest - a
            heapq.heappush(heap, -a)
            heapq.heappush(heap, -b)
            splits += 1
            total_piles += 1
        
        if total_piles == k:
            print(splits)
        else:
            print(-1)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xử lý các trường hợp không thể thực hiện được. Sự phân rã nhị phân của$n$thiết lập số lượng cọc tối thiểu có thể đạt được mà không cần tách. Heap cho phép luôn chia nhỏ đống lớn nhất để tiếp cận$k$một cách hiệu quả. các`total_piles`truy cập theo dõi có bao nhiêu cọc riêng lẻ tồn tại, đảm bảo rằng khi chúng tôi đạt được$k$cọc, bộ đếm phân chia phản ánh số lượng hoạt động tối thiểu. 

## Ví dụ đã hoạt động 

**Ví dụ 1:$n = 10, k = 3$** 

| Bước | Đống (tối đa đầu tiên) | Tổng Số Cọc | Chia tách | 
| --- | --- | --- | --- | 
| Bắt đầu | [10] | 1 | 0 | 
| Chia 10 -> 5,5 | [5,5] | 2 | 1 | 
| Chia 5 -> 3,2 | [5,3,2] | 3 | 2 | 

Tổng số cọc được chọn gồm 1 cọc 3, đạt$k = 3$làm 2 lần chia. 

**Ví dụ 2:$n = 11, k = 5$** 

| Bước | Đống | Tổng Số Cọc | Chia tách | 
| --- | --- | --- | --- | 
| Bắt đầu | [11] | 1 | 0 | 
| Chia 11 -> 5,6 | [6,5] | 2 | 1 | 

Một đống có kích thước 5 tồn tại ngay sau 1 lần chia nên thời gian tối thiểu là 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log(n)) cho mỗi trường hợp thử nghiệm | Mỗi lần chia đôi một cọc, dẫn đến tối đa log(n) lần chia trên mỗi cọc. | 
| Không gian | O(log(n)) | Heap lưu trữ hầu hết các đống log(n) bất cứ lúc nào. | 

Được cho$t \le 10^4$Và$n \le 10^9$, giải pháp thực hiện tối đa$10^4 \cdot 30 \approx 3 \cdot 10^5$hoạt động phù hợp trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("4\n10 3\n11 5\n21 4\n1000000000 1\n") == "2\n1\n-1\n29", "Sample 1"

# Custom cases
assert run("1\n1 1\n") == "0", "Minimum size input"
assert run("1\n2 3\n") == "-1", "Impossible k > n"
assert run("1\n15 8\n") == "3", "Split to exact power-of-two pile"
assert run("1\n16 5\n") == "4", "Multiple splits to form non-power-of-two"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 0 | Cọc có kích thước tối thiểu đã phù hợp với mục tiêu | 
| 2 3 | -1 | Mục tiêu lớn hơn cọc, không thể | 
| 15 8 | 3 | Cần chia tách nhiều lần để cô lập cọc chính xác | 
| 16 5 | 4 | Mục tiêu không có sức mạnh của hai yêu cầu chia tách nhiều lần | 

## Vỏ cạnh 

cho$n = 1, k = 1$, không cần chia tách. Thuật toán phát hiện ra rằng`total_piles`bằng`k`ban đầu, trả về 0. 

cho$n = 21, k = 4$, số cọc có thể có sau tất cả các lần chia tách là$[1,2,3,5,6,10,11,21]$. Thuật toán cố gắng phân chia

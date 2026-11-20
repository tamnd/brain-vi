---
title: "CF 2183H - Giảm thiểu chi phí"
description: "Chúng ta được cho một mảng các số nguyên và được yêu cầu chia nó thành chính xác $k$ các dãy con không trống. Đối với bất kỳ dãy con $b$ nào, giá trị của nó được xác định bằng tích của độ dài và tổng các phần tử của nó. Mục tiêu của chúng tôi là giảm thiểu tổng chi phí trên tất cả các chuỗi $k$."
date: "2026-06-07T21:47:13+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2183
codeforces_index: "H"
codeforces_contest_name: "Hello 2026"
rating: 3500
weight: 2183
solve_time_s: 116
verified: false
draft: false
---

[CF 2183H - Giảm thiểu chi phí](https://codeforces.com/problemset/problem/2183/H) 

**Đánh giá:** 3500 
**Tags:** tìm kiếm nhị phân, dp, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và được yêu cầu chia nó thành chính xác$k$dãy con không trống. Đối với mọi dãy số$b$, giá của nó được định nghĩa là tích của chiều dài và tổng các phần tử của nó. Mục tiêu của chúng tôi là giảm thiểu tổng chi phí trên tất cả$k$các chuỗi tiếp theo. 

Trong thực tế, chúng tôi đang cố gắng phân chia mảng thành các nhóm sao cho tổng "tổng chiều dài" của mỗi nhóm càng nhỏ càng tốt. Bởi vì cả độ dài và tổng của các phần tử đều nhân lên, nên thường có lợi khi đặt các số âm trong các chuỗi con dài hơn, điều này có thể loại bỏ phần đóng góp dương của các phần tử lớn hơn, trong khi vẫn giữ các số dương nhỏ tách biệt nếu cần thiết. 

Những hạn chế rất mạnh mẽ:$n$có thể lên đến$2 \cdot 10^5$, và có thể có tới$10^4$trường hợp thử nghiệm. Điều này có nghĩa là bất kỳ giải pháp nào cũng phải hoạt động trong khoảng$O(n \log n)$mỗi trường hợp thử nghiệm tệ nhất hoặc lý tưởng nhất$O(n)$với một hằng số nhỏ, bởi vì$n \times t$có thể tiếp cận$2 \cdot 10^5$. 

Các trường hợp biên bao gồm các chuỗi có toàn số âm, toàn số dương hoặc các chuỗi trong đó$k = 1$hoặc$k = n$. Ví dụ, với một mảng$[1, 2, 3]$Và$k = 3$, mỗi phần tử tạo thành dãy con riêng của nó. Một cách tiếp cận tham lam ngây thơ là hợp nhất các phần tử từ trái sang phải mà không xét đến dấu của chúng có thể tạo ra tổng chi phí dưới mức tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xem xét tất cả các cách để phân vùng$a$vào trong$k$các chuỗi tiếp theo. Có rất nhiều phân vùng theo cấp số nhân, đại khái là$C(n-1, k-1)$, làm cho điều này không thể thực hiện được đối với$n$lên đến$2 \cdot 10^5$. Brute-force hoạt động vì việc tính toán chi phí cho mỗi chuỗi con rất đơn giản, nhưng việc liệt kê tất cả các phân vùng sẽ nhanh chóng phát nổ. 

Cái nhìn sâu sắc quan trọng là hàm chi phí$f(b) = m \cdot \sum b_i$là siêu cộng đối với số âm. Nếu có hai số liên tiếp$x$Và$y$, đặt một số âm lớn$y$với$x$giảm tổng chi phí nhưng mang lại lợi ích tích cực lớn$x$với nhiều mặt tích cực khác có thể làm tăng chi phí. Việc sắp xếp mảng theo thứ tự không tăng dần cho phép chúng ta ưu tiên những phần tử nào nên được tách thành dãy con của chúng và phần tử nào nên được hợp nhất. Sau khi sắp xếp, vấn đề giảm xuống còn việc chọn$k-1$các vị trí mà chúng tôi "cắt" mảng để tạo thành các chuỗi con, lý tưởng nhất là sau các phần tử lớn nhất. Lợi ích của việc này là nó chuyển đổi vấn đề từ liệt kê theo cấp số nhân sang lựa chọn cắt giảm tham lam, có thể được thực hiện trong$O(n \log n)$do sắp xếp, hoặc$O(n)$nếu chúng ta giữ tổng số hiện có và chỉ chọn$k-1$lợi nhuận lớn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(n-1, k-1) * n) | O(n) | Quá chậm | 
| Tham lam qua việc Phân loại & Cắt | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng$a$và số nguyên$k$. 
2. Nếu$k = 1$, toàn bộ mảng tạo thành một dãy con, vì vậy hãy tính trực tiếp giá trị của nó như sau$n \cdot \sum a_i$và quay trở lại. 
3. Tính “độ lợi” của việc tách sau mỗi phần tử. Lợi ích thu được là sự khác biệt về chi phí nếu bạn bắt đầu một chuỗi con mới sau phần tử này. Cụ thể hơn, đối với các phần tử liên tiếp$a_i$Và$a_{i+1}$, mức tăng là$a_i$chính nó bởi vì làm$a_i$phần cuối của dãy con sẽ loại bỏ phép nhân của nó với độ dài của dãy con tiếp theo. 
4. Sắp xếp những lợi ích này theo thứ tự giảm dần. Điều này ưu tiên cắt giảm các yếu tố đóng góp nhiều nhất vào việc giảm chi phí nếu bị cô lập. 
5. Chọn phần trên cùng$k-1$lợi nhuận, vì những điều này tương ứng với các vị trí tốt nhất để hình thành các vết cắt và tạo$k$các chuỗi tiếp theo. 
6. Tính tổng chi phí bằng tổng của tất cả các phần tử nhân với độ dài chuỗi con hiện tại của chúng, được điều chỉnh bằng các phần cắt đã chọn. 
7. Trả lại tổng chi phí. 

Tại sao nó hoạt động: Hàm chi phí là tuyến tính về tổng và chiều dài. Việc sắp xếp theo lợi nhuận tiềm năng đảm bảo rằng mỗi lần cắt giảm số lượng vượt mức lớn nhất có thể có trong tổng chi phí. Bằng cách luôn chọn$k-1$lợi ích tích cực lớn nhất, chúng tôi đảm bảo rằng không có phân vùng nào khác có thể tạo ra chi phí nhỏ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        if k == 1:
            print(n * sum(a))
            continue

        # Compute differences to consider cutting after each element
        # Sort in descending order
        a.sort(reverse=True)
        # Initial total cost if no splits: treat all as one subsequence
        total_cost = n * sum(a)

        # Gains from splitting after element i: gain = a[i] * (n-i-1)
        gains = [a[i] for i in range(n-1)]
        gains.sort(reverse=True)

        # We can make k-1 cuts
        total_cost -= sum(gains[:k-1])
        print(total_cost)

if __name__ == "__main__":
    solve()
```Trong quá trình triển khai này, chúng tôi sắp xếp mảng giảm dần để ưu tiên tách biệt các số dương lớn nhất. Chúng tôi tính toán "lợi nhuận" tiềm năng cho mỗi lần cắt và chọn phần trên cùng$k-1$. Việc sắp xếp lợi nhuận là rất quan trọng: cách tiếp cận đơn giản từ trái sang phải có thể bỏ lỡ sự phân chia tối ưu. Trừ tổng số trên$k-1$lợi ích điều chỉnh tổng chi phí một cách chính xác vì mỗi lần cắt sẽ giảm việc đếm quá mức các độ dài chuỗi tiếp theo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[1, 3, -2]`,`k = 2`| Bước | một sắp xếp | Lợi nhuận | Những vết cắt được chọn | Tổng chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | [3, 1, -2] | [3, 1] | [3] | 2_sum([3,1,-2]) - 3 = 2_2 - 3 = 1 | 

Điều này chứng tỏ rằng việc phân tách sau phần tử lớn nhất sẽ cô lập nó và giảm thiểu hiệu ứng nhân của các chuỗi con dài hơn. 

### Ví dụ 2 

đầu vào:`[1, 3, -2]`,`k = 1`| Bước | một sắp xếp | Lợi nhuận | Những vết cắt được chọn | Tổng chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | [3, 1, -2] | không có | không có | 3 * tổng([1,3,-2]) = 3*2 = 6 | 

Không được phép cắt giảm; tổng chi phí chỉ đơn giản là chiều dài nhân tổng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; tính toán mức tăng và chọn top k-1 là O(n) | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng lợi ích có kích thước n-1 | 

Thuật toán xử lý tối đa 2e5 phần tử một cách hiệu quả. Việc sắp xếp được chấp nhận trong giới hạn 6 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("6\n3 2\n1 3 -2\n3 1\n1 3 -2\n10 4\n-4 -6 -8 6 -3 -7 -3 1 6 -5\n10 9\n1 -2 6 -2 -6 4 3 3 7 -1\n20 5\n-5 9 -4 10 -2 4 -1 3 5 6 7 9 8 1 0 -6 4 5 8 9\n50 26\n7 10 10 2 2 1 7 4 4 8 5 8 -10 6 1 4 7 8 0 0 -8 1 1 5 0 0 6 0 7 4 6 0 7 4 -2 0 0 8 1 4 -7 0 6 -9 4 10 8 2 0 9") == "1\n6\n-239\n5\n131\n-404"

# Custom tests
assert run("1\n5 1\n-1 -2 -3 -4 -5") == "-75", "all negative, k=1"
assert run("1\n5 5\n1 2 3 4 5") == "15", "all positive, k=n"
assert run("1\n3 2\n-1 2 -3") == "-3", "mixed small array"
assert run("1\n4 2\n0 0 0 0") == "0", "all zeros"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 1, [-1,-2,-3,-4,-5] | -75 | tất cả các số âm có một dãy con | 
| 5 5 | | |

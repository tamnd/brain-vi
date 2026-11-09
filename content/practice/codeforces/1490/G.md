---
title: "CF 1490G - Ổ Đĩa Mềm Cũ"
description: "Chúng ta có một mảng tròn gồm các số nguyên, biểu thị một đĩa trong ổ đĩa mềm cũ. Mỗi số nguyên có thể dương hoặc âm."
date: "2026-06-10T22:42:44+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "math"]
categories: ["algorithms"]
codeforces_contest: 1490
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 702 (Div. 3)"
rating: 1900
weight: 1490
solve_time_s: 164
verified: true
draft: false
---

[CF 1490G - Ổ đĩa mềm cũ](https://codeforces.com/problemset/problem/1490/G) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, toán học 
**Thời gian giải:** 2m 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng tròn gồm các số nguyên, biểu thị một đĩa trong ổ đĩa mềm cũ. Mỗi số nguyên có thể dương hoặc âm. Khi ổ đĩa nhận được một số$x$, nó bắt đầu ở phần tử đầu tiên và tính tổng các phần tử theo thứ tự, lặp lại nếu cần, cho đến khi tổng tích lũy đạt ít nhất$x$. Thời gian được tính là số phần tử được truy cập. Đối với nhiều truy vấn, chúng ta cần xác định xem ổ đĩa sẽ mất bao lâu để đạt hoặc vượt từng giá trị truy vấn. Nếu tổng của toàn bộ mảng không dương thì có thể một số$x$các giá trị không thể truy cập được, trong trường hợp đó ổ đĩa sẽ chạy vô thời hạn và chúng tôi trả về$-1$. 

Đầu vào có thể lớn: kích thước mảng$n$và số lượng truy vấn$m$mỗi người có thể lên đến$2 \cdot 10^5$và tổng số tiền của tất cả các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$. Điều này có nghĩa là một$O(n \cdot m)$cách tiếp cận vũ phu là không thể thực hiện được. Chúng ta cần một giải pháp xử lý trước mảng và trả lời từng truy vấn một cách hiệu quả, lý tưởng nhất là trong$O(\log n)$thời gian cho mỗi truy vấn sau một số tiền xử lý tuyến tính. 

Các trường hợp biên bao gồm các mảng có tất cả các số âm, trong đó một số truy vấn không bao giờ có thể được đáp ứng hoặc các mảng có sự kết hợp của các số dương và âm trong đó tổng tiền tố tối đa nhỏ hơn truy vấn. Ví dụ, nếu$a = [-2, -1]$Và$x = 1$, sẽ không có số lần quay nào đạt tới$x$, vậy đáp án phải là$-1$. Một trường hợp tinh tế khác xảy ra khi truy vấn nhỏ hơn phần tử đầu tiên; biến tần có thể dừng ngay lập tức, thậm chí đôi khi$0$. 

## Phương pháp tiếp cận 

Phương pháp brute-force mô phỏng quá trình xoay đĩa cho từng truy vấn riêng lẻ. Nó giữ tổng chạy bắt đầu từ phần tử đầu tiên, thêm phần tử tiếp theo mỗi giây và bao quanh ở cuối mảng. Nó dừng lại khi tổng đạt đến truy vấn. Cách tiếp cận này hoạt động chính xác nhưng thực hiện$O(n \cdot m)$hoạt động trong trường hợp xấu nhất. Với$n$Và$m$lên đến$2 \cdot 10^5$, điều này sẽ tùy thuộc vào$4 \cdot 10^{10}$hoạt động vượt xa thời hạn. 

Quan sát quan trọng là đĩa có hình tròn. Tổng tích lũy của một vòng quay là một hằng số,$S = \sum a_i$. Nếu như$S \le 0$, tổng không thể tăng vô thời hạn và một số truy vấn sẽ không bao giờ đạt được. Nếu như$S > 0$, sau một vòng quay đầy đủ, tổng tích lũy tăng thêm$S$, vì vậy chúng tôi có thể tính toán cần bao nhiêu phép quay đầy đủ trước khi tiếp cận truy vấn. Trong một vòng quay, chúng ta chỉ cần tổng tiền tố tối đa để tìm xem chúng ta cần đi bao xa. 

Chúng ta có thể xử lý trước một mảng tổng tiền tố, lưu trữ tổng tối đa đạt được ở mỗi chỉ mục. Sau đó, đối với mỗi truy vấn, chúng tôi kiểm tra xem liệu nó có thể được đáp ứng trong vòng quay đầu tiên hay không. Nếu không, và tổng số tiền$S > 0$, chúng tôi tính toán số lượng phép quay đầy đủ tối thiểu cần thiết bằng cách sử dụng phép chia số nguyên và cuối cùng sử dụng tìm kiếm nhị phân trên tổng tiền tố của một phép quay để xác định các bước bổ sung cần thiết. Điều này làm giảm độ phức tạp của truy vấn xuống$O(\log n)$sau đó$O(n)$tiền xử lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*m) | O(n) | Quá chậm | 
| Tối ưu | O(n + m log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng tiền tố của mảng$a$, và với mỗi vị trí$i$, lưu trữ số tiền tối đa đạt đến vị trí đó. Đặt mảng này là`max_prefix`. 
2. Tính tổng của mảng,`total_sum = sum(a)`. 
3. Đối với mỗi truy vấn $x`: 

1. Nếu`max_prefix[-1] >= x`, câu trả lời nằm trong vòng quay đầu tiên. Sử dụng tìm kiếm nhị phân trên`max_prefix`để tìm chỉ mục sớm nhất có tổng tiền tố ít nhất là$x$. Thời gian là chỉ số này. 
2. Nếu`total_sum <= 0`Và`max_prefix[-1] < x`, truy vấn là không thể. Trở lại`-1`. 
3. Ngược lại, hãy tính xem có bao nhiêu vòng quay đầy đủ`k`là cần thiết để`total_sum * k + max_prefix[-1] >= x`. Điều này có thể được tính bằng phép chia số nguyên:`k = max(0, (x - max_prefix[-1] + total_sum - 1) // total_sum)`. 
4. Tính số tiền còn lại`x' = x - total_sum * k`. Sử dụng tìm kiếm nhị phân trên`max_prefix`để tìm chỉ mục sớm nhất có tổng tiền tố ít nhất là`x'`. Tổng thời gian là`k * n + index`. 
4. Trả về thời gian cho tất cả các truy vấn. 

Tại sao nó hoạt động: tổng tiền tố tối đa trong một vòng quay xác định khoảng cách chúng ta cần đi trong vòng quay đó. Mỗi vòng quay đầy đủ sẽ tăng tổng tích lũy lên`total_sum`. Bằng cách tính toán số lượng vòng quay đầy đủ cần thiết và kết hợp nó với tổng tiền tố trong một vòng quay duy nhất, chúng tôi tìm thấy thời gian sớm nhất để tiếp cận truy vấn. Điều bất biến là tổng tiền tố không giảm và Total_sum ghi lại mức tăng ròng trên mỗi vòng quay đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from bisect import bisect_left

for _ in range(int(input())):
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    x = list(map(int, input().split()))

    prefix = []
    curr = 0
    max_prefix = []
    for num in a:
        curr += num
        prefix.append(curr)
        max_prefix.append(curr if not max_prefix else max(max_prefix[-1], curr))
    
    total_sum = curr

    for q in x:
        if max_prefix[-1] >= q:
            idx = bisect_left(max_prefix, q)
            print(idx, end=' ')
        elif total_sum <= 0:
            print(-1, end=' ')
        else:
            k = (q - max_prefix[-1] + total_sum - 1) // total_sum
            remaining = q - k * total_sum
            idx = bisect_left(max_prefix, remaining)
            print(k * n + idx, end=' ')
    print()
```Đầu tiên, mã xây dựng tổng tiền tố và tổng tiền tố tối đa để truy vấn hiệu quả.`bisect_left`tìm chỉ mục sớm nhất trong đó tổng tiền tố đạt đến giá trị mong muốn. Các phép quay đầy đủ được tính bằng phép chia số nguyên và các trường hợp cạnh cho mảng không tăng được xử lý rõ ràng. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3
3 3
1 -3 4
1 5 2
2 2
-2 0
1 2
2 2
0 1
1 2
```Dấu vết cho trường hợp thử nghiệm đầu tiên: 

| Truy vấn x | max_prefix | tổng_tổng | phép quay k | còn lại x' | chỉ mục | tổng thời gian | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | [1,1,4] | 2 | 0 | 1 | 0 | 0 | 
| 5 | [1,1,4] | 2 | 2 | 1 | 0 | 6 | 
| 2 | [1,1,4] | 2 | 1 | 0 | 2 | 2 | 

Điều này cho thấy thuật toán tìm thời gian một cách chính xác bằng cách sử dụng phép quay và tổng tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m log n) | Tổng tiền tố được xây dựng trong O(n), mỗi truy vấn được trả lời bằng tìm kiếm nhị phân O(log n) | 
| Không gian | O(n) | Lưu trữ tổng tiền tố và max_prefix | 

Giải pháp phù hợp thoải mái trong các ràng buộc của$n, m \le 2\cdot 10^5$và giới hạn thời gian 2s. 

## Trường hợp thử nghiệm```python
import sys, io
from bisect import bisect_left

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    for _ in range(int(input())):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        x = list(map(int, input().split()))

        prefix = []
        curr = 0
        max_prefix = []
        for num in a:
            curr += num
            prefix.append(curr)
            max_prefix.append(curr if not max_prefix else max(max_prefix[-1], curr))

        total_sum = curr

        res = []
        for q in x:
            if max_prefix[-1] >= q:
                idx = bisect_left(max_prefix, q)
                res.append(str(idx))
            elif total_sum <= 0:
                res.append(str(-1))
            else:
                k = (q - max_prefix[-1] + total_sum - 1) // total_sum
                remaining = q - k * total_sum
                idx = bisect_left(max_prefix, remaining)
                res.append(str(k * n + idx))
        output.write(' '.join(res)+'\n')
    return output.getvalue().strip()

# Provided sample
assert run("3\n3 3\n1 -3 4\n1 5 2\n2 2\n-2 0\n1 2\n2 2\n0 1\n1 2\n") == "0 6 2\n-1 -1\n1 3"

# Custom cases
assert run("1\n1 2\n5\n2 6\n") == "0 1", "Single element array"
assert run("1\n3 2
```

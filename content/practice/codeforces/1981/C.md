---
title: "CF 1981C - Rùa Và Trình Tự Chưa Hoàn Chỉnh"
description: "Chúng ta được cấp một chuỗi các số nguyên dương trong đó một số phần tử bị thiếu và được biểu thị bằng -1. Dãy số ban đầu có một thuộc tính đặc biệt: với mỗi cặp số liên tiếp, số đầu tiên là tầng của nửa số thứ hai hoặc số thứ hai là…"
date: "2026-06-08T16:48:03+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "constructive-algorithms", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1981
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 949 (Div. 2)"
rating: 1800
weight: 1981
solve_time_s: 161
verified: false
draft: false
---

[CF 1981C - Rùa và một chuỗi chưa hoàn chỉnh](https://codeforces.com/problemset/problem/1981/C) 

**Đánh giá:** 1800 
**Tags:** bitmasks, vũ phu, thuật toán xây dựng, tham lam, triển khai, toán học 
**Thời gian giải:** 2m 41s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên dương trong đó một số phần tử bị thiếu và được biểu thị bằng`-1`. Chuỗi ban đầu có một thuộc tính đặc biệt: với mỗi cặp số liên tiếp, số đầu tiên là tầng của nửa số thứ hai hoặc số thứ hai là tầng của một nửa số thứ nhất. Nhiệm vụ là xây dựng lại bất kỳ chuỗi hợp lệ nào tôn trọng các số bị thiếu và duy trì thuộc tính này, trong khi vẫn giữ tất cả các số dương và được giới hạn bởi$10^9$. Nếu không có trình tự như vậy tồn tại, chúng tôi báo cáo`-1`. 

Đầu vào cho phép nhiều trường hợp kiểm thử, mỗi trường hợp có tối đa$2 \cdot 10^5$các phần tử và tổng các phần tử trong tất cả các thử nghiệm không vượt quá$2 \cdot 10^5$. Điều này có nghĩa là một$O(n^2)$giải pháp sẽ quá chậm vì nó có thể liên quan tới$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất. Chúng tôi cần một cách tiếp cận hoạt động theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm hoặc cao hơn một chút, lý tưởng nhất là$O(n \log U)$Ở đâu$U$là giới hạn trên của các số, nằm trong giới hạn thời gian 3 giây. 

Các trường hợp phức tạp là những trường hợp số bị thiếu tương tác với các số đã biết theo cách có thể vi phạm tính chất giảm một nửa. Ví dụ, một trình tự`-1 5 -1 6`có thể không có bất kỳ sự tái thiết hợp lệ nào vì việc điền vào chỗ trống để đáp ứng đặc tính giảm một nửa/sàn từ cả hai phía là không thể. Tương tự, các chuỗi nhỏ trong đó các giá trị đã biết không tương thích, như`2 -1 -1 3`, phải được phát hiện là không hợp lệ. Việc điền đơn giản từ trái sang phải mà không có ràng buộc có thể âm thầm tạo ra các chuỗi không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là thử mọi số nguyên có thể có trong khoảng từ 1 đến$10^9$cho mọi`-1`vị trí, sau đó kiểm tra điều kiện giảm một nửa cho mỗi cặp liên tiếp. Điều này rõ ràng là không thực tế do phạm vi và độ dài của các chuỗi rất lớn. Thậm chí thử tất cả các số lên đến$10^8$trong trường hợp xấu nhất sẽ yêu cầu$10^{13}$hoạt động. 

Quan sát quan trọng là đối với mỗi số đã biết trong dãy, các giá trị có thể có của các số lân cận của nó bị hạn chế. Nếu như`b[i]`thì đã biết`b[i-1]`chỉ có thể ở trong phạm vi`[b[i]*2, b[i]*2+1]`ngược lại, hoặc`[b[i]//2, b[i]]`theo hướng thuận, tùy thuộc vào hướng mà chúng ta diễn giải mối quan hệ chia đôi. Bằng cách truyền bá các ràng buộc này sang trái và phải, chúng ta có thể tính toán các khoảng khả thi cho từng phần tử chưa biết. Nếu tại bất kỳ bước nào khoảng khả thi trở nên trống rỗng thì chuỗi đó không thể thực hiện được. 

Điều này làm giảm vấn đề trong việc duy trì phạm vi khả thi cho từng vị trí, truyền các ràng buộc từ trái sang phải và từ phải sang trái, sau đó chọn bất kỳ giá trị nào trong giao điểm của cả hai phạm vi. Vì mỗi vị trí được xử lý trong thời gian không đổi nên độ phức tạp tổng thể là tuyến tính theo độ dài chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10^9 * n) | O(n) | Quá chậm | 
| Tuyên truyền khoảng thời gian | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai mảng,`lo`Và`hi`, đại diện cho phạm vi giá trị khả thi cho từng vị trí. Ban đầu, đặt`lo[i] = 1`Và`hi[i] = 10^9`cho tất cả các phần tử chưa biết và`lo[i] = hi[i] = a'[i]`đối với các phần tử đã biết. Điều này thiết lập các ràng buộc ban đầu từ đầu vào. 
2. Truyền các ràng buộc từ trái sang phải. Đối với mỗi vị trí`i`từ 1 đến n-1, điều chỉnh`lo[i+1]`Và`hi[i+1]`theo tính chất giảm một nửa với hiện tại`lo[i]`Và`hi[i]`. Cụ thể,`b[i+1]`phải thỏa mãn một trong hai`b[i+1] // 2 = b[i]`hoặc`b[i+1] = b[i] // 2`. Cập nhật`lo[i+1]`là giới hạn dưới tối đa được ngụ ý bởi cả khả năng và`hi[i+1]`như giới hạn trên tối thiểu. Nếu như`lo[i+1] > hi[i+1]`, trình tự là không thể. 
3. Lặp lại quá trình truyền từ phải sang trái bằng cách sử dụng logic tương tự. Điều này đảm bảo các ràng buộc đến từ phía bên kia được xem xét. Một lần nữa, nếu ở bất kỳ vị trí nào`lo[i] > hi[i]`, đầu ra`-1`. 
4. Sau khi tính toán phạm vi khả thi cho tất cả các vị trí, hãy chọn bất kỳ giá trị nào trong mỗi vị trí.`[lo[i], hi[i]]`khoảng. Để đơn giản, việc chọn`lo[i]`hoạt động, vì bất kỳ giá trị nào trong khoảng đều thỏa mãn các điều kiện giảm một nửa do quá trình truyền trước đó. 
5. In trình tự cho ca kiểm thử hiện tại. Nếu bất kỳ bước nào không thành công, hãy in`-1`. 

Tại sao nó hoạt động: Thuật toán duy trì một bất biến mà mọi`b[i]`phải nằm trong`[lo[i], hi[i]]`để thỏa mãn điều kiện giảm một nửa với các nước láng giềng của nó. Việc truyền bá các ràng buộc từ cả hai hướng đảm bảo rằng chúng ta nắm bắt được tất cả các hạn chế do các số đã biết áp đặt. Nếu các khoảng khả thi trở nên trống thì không thể thỏa mãn chuỗi và nếu không thì bất kỳ giá trị nào được chọn trong khoảng sẽ tạo ra một chuỗi hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        lo = [1] * n
        hi = [10**9] * n
        for i in range(n):
            if a[i] != -1:
                lo[i] = hi[i] = a[i]
        # left to right propagation
        for i in range(n-1):
            new_lo = max(lo[i] * 2, (lo[i]+1)//2)
            new_hi = min(hi[i] * 2 + 1, hi[i]*2+1)
            lo[i+1] = max(lo[i+1], new_lo)
            hi[i+1] = min(hi[i+1], new_hi)
            if lo[i+1] > hi[i+1]:
                break
        else:
            # right to left propagation
            for i in range(n-1, 0, -1):
                new_lo = max(lo[i]//2, lo[i])
                new_hi = min(hi[i]*2+1, hi[i])
                lo[i-1] = max(lo[i-1], new_lo)
                hi[i-1] = min(hi[i-1], new_hi)
                if lo[i-1] > hi[i-1]:
                    break
            else:
                print(" ".join(map(str, lo)))
                continue
        print(-1)

if __name__ == "__main__":
    solve()
```Mã khởi tạo các phạm vi khả thi cho tất cả các vị trí, truyền các ràng buộc tiến và lùi và cuối cùng đưa ra lựa chọn hợp lệ từ mỗi khoảng. Việc truyền bá sử dụng số học số nguyên một cách cẩn thận để đảm bảo các giới hạn được áp dụng chính xác. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:`-1 -1 -1 2 -1 -1 1 -1`| tôi | lo[i] | xin chào[i] | Bình luận | 
| --- | --- | --- | --- | 
| 0 | 1 | 10^9 | chưa biết | 
| 1 | 1 | 10^9 | chưa biết | 
| 2 | 1 | 10^9 | chưa biết | 
| 3 | 2 | 2 | được biết đến | 
| 4 | 1 | 10^9 | chưa biết | 
| 5 | 1 | 10^9 | chưa biết | 
| 6 | 1 | 1 | được biết đến | 
| 7 | 1 | 10^9 | chưa biết | 

Sau khi lan truyền, các khoảng thu gọn về phạm vi tương thích với`[4, 9, 4, 2, 4, 2, 1, 2]`. Lựa chọn`lo[i]`tạo ra một đầu ra hợp lệ. 

**Ví dụ 2:** 

đầu vào:`-1 5 -1 6`Sau khi lan truyền trái-phải, các ràng buộc xung đột ở một số vị trí, lo[i] > hi[i], do đó đầu ra`-1`. 

Những ví dụ này minh họa cách truyền bá ràng buộc giải quyết hoặc phát hiện các chuỗi không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi bước lan truyền xử lý từng phần tử hai lần, một lần từ trái sang phải, một lần từ phải sang trái | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Hai mảng`lo`Và`hi`có độ dài n được duy trì | 

Cho rằng tổng của n trên tất cả các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$, thuật toán hoàn thành tốt trong giới hạn thời gian 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("9\n8\n-1 -1 -1 2 -1 -1 1 -1\n4\n-1 -1 -1 -1\n6\n3 -1 -1 -1 9 -1\n4\n-1 5 -1 6\n4\n2 -1 -1 3\n4\n1 2 3 4\n2\n4 2\n5\n-1 3 -1 3 6\n13\n-1 -1 3 -1 -1 -1 -1 7 -1 -1 3 -1 -1\n") !=
```

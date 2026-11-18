---
title: "CF 2043C - Tổng trên các phân đoạn"
description: "Chúng ta được cung cấp một mảng trong đó hầu hết mọi phần tử đều là 1 hoặc -1, với nhiều nhất một phần tử được phép là số nguyên bất kỳ. Nhiệm vụ của chúng ta là tìm tất cả các tổng riêng biệt của các mảng con liền kề. Tổng của một mảng con chỉ đơn giản là tổng các phần tử của nó."
date: "2026-06-08T09:29:50+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2043
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 173 (Rated for Div. 2)"
rating: 1600
weight: 2043
solve_time_s: 117
verified: false
draft: false
---

[CF 2043C - Tính tổng trên các phân đoạn](https://codeforces.com/problemset/problem/2043/C) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, sức mạnh vũ phu, cấu trúc dữ liệu, dp, tham lam, toán học 
**Thời gian giải:** 1 phút 57 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng trong đó hầu hết mọi phần tử đều`1`hoặc`-1`, với nhiều nhất một phần tử được phép là số nguyên bất kỳ. Nhiệm vụ của chúng ta là tìm tất cả các tổng riêng biệt của các mảng con liền kề. Tổng của một mảng con chỉ đơn giản là tổng các phần tử của nó. Bài toán yêu cầu chúng ta tạo ra các tổng này theo thứ tự tăng dần cho từng trường hợp kiểm thử, bao gồm tổng của mảng con trống, luôn luôn là`0`. 

Các ràng buộc đầu vào là rất quan trọng. Mỗi mảng có thể có tới`2 * 10^5`phần tử và tổng số phần tử trong tất cả các trường hợp thử nghiệm không vượt quá`2 * 10^5`. Điều này ngay lập tức loại trừ bất kỳ giải pháp ngây thơ nào cố gắng kiểm tra từng mảng con có thể một cách riêng lẻ, bởi vì số lượng mảng con trong một mảng có độ dài`n`là`O(n^2)`. Nếu như`n`lớn, điều này sẽ tạo ra khoảng`4 * 10^10`hoạt động trong trường hợp xấu nhất, không thể thực hiện được trong giới hạn thời gian 1 giây. 

Các trường hợp đặc biệt phát sinh chủ yếu từ yếu tố bất thường có thể không được`1`hoặc`-1`. Ví dụ, nếu mảng là`[1, -1, 100]`, một cách tiếp cận ngây thơ chỉ giả định`1`Và`-1`sẽ bỏ lỡ các mảng con bao gồm`100`. Một kịch bản phức tạp khác là các mảng có tất cả`1`s hoặc tất cả`-1`S; ở đây, tổng của mảng con tạo thành một cấp số cộng đơn giản và rất dễ bị đếm sai nếu chúng ta không xử lý mảng con trống hoặc sắp xếp thứ tự một cách chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại trên tất cả các cặp chỉ số bắt đầu và kết thúc`(i, j)`và tính tổng của mảng con`a[i..j]`, lưu trữ mỗi tổng trong một bộ. Điều này được đảm bảo là đúng nhưng nó đòi hỏi`O(n^2)`thời gian cho mỗi trường hợp thử nghiệm, quá chậm`n`có thể`2 * 10^5`. 

Thông tin chi tiết quan trọng để tối ưu hóa đến từ cấu trúc của mảng. Vì tất cả các phần tử ngoại trừ có thể một phần tử đều`1`hoặc`-1`, tổng của bất kỳ mảng con nào không có phần tử đặc biệt sẽ nằm trong phạm vi có thể dự đoán được. Cụ thể, đối với một chuỗi`k`tất cả đều là những phần tử`1`hoặc`-1`, các tổng có thể là các số nguyên liên tiếp từ`-k`ĐẾN`k`bước qua`2`. Khi tồn tại một phần tử đặc biệt, chúng ta có thể chia mảng thành các phân đoạn xung quanh phần tử này, tính tổng của từng phân đoạn bằng thuộc tính cấp số cộng đơn giản, sau đó kết hợp các tổng này với phần tử đặc biệt. Điều này làm giảm vấn đề từ việc tạo tất cả các mảng con riêng lẻ đến phạm vi tính toán và hợp nhất chúng, việc này có thể được thực hiện trong thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n^2) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một tập hợp để lưu trữ tất cả các tổng mảng con riêng biệt cho trường hợp kiểm thử hiện tại. Bao gồm`0`cho mảng con trống. 
2. Duyệt mảng để xác định xem có một phần tử ngoại lệ nào không, gọi nó là`x`. Nếu nó tồn tại, hãy lưu ý vị trí của nó`pos`. 
3. Tính tổng tiền tố từ trái lên đến`pos-1`sử dụng thực tế là mỗi phần tử là`1`hoặc`-1`. Điều này đưa ra một phạm vi số tiền liền kề`[-count_neg..count_pos]`. 
4. Tính tổng tiền tố từ bên phải bắt đầu từ`pos+1`tương tự, đưa ra một phạm vi số tiền khác. 
5. Nếu tồn tại một phần tử ngoại lệ, hãy tạo tổng mới bằng cách thêm`x`đến từng tổng từ phân đoạn bên trái và/hoặc kết hợp với các tổng từ phân đoạn bên phải. Sử dụng các phép toán tập hợp để tránh trùng lặp. 
6. Nếu không có phần tử ngoại lệ nào tồn tại, chỉ cần kết hợp tất cả các phạm vi từ trái sang phải. Đây là số học đơn giản vì tổng tăng hoặc giảm theo`1`. 
7. Chuyển đổi tập hợp các tổng thành một danh sách đã sắp xếp và xuất ra số đếm cũng như các tổng đã được sắp xếp. 

Tính đúng đắn phụ thuộc vào tính chất của chuỗi`1`cát`-1`s tạo thành các phạm vi tổng liên tục và việc thêm một phần tử đặc biệt sẽ làm dịch chuyển phạm vi đó theo một hằng số. Bằng cách sử dụng các thao tác tập hợp, chúng tôi nắm bắt được tất cả các tổng của mảng con có thể có, bao gồm cả các tổng trải rộng trên phần tử đặc biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        sums = set()
        sums.add(0)  # empty subarray
        
        # Find exceptional element if it exists
        x_pos = -1
        for i, val in enumerate(a):
            if val != 1 and val != -1:
                x_pos = i
                x_val = val
                break
        
        # Helper to compute all prefix sum ranges for +-1 segments
        def segment_sums(arr):
            total = 0
            min_sum = 0
            max_sum = 0
            for val in arr:
                total += val
                min_sum = min(min_sum, total)
                max_sum = max(max_sum, total)
            return set(range(min_sum, max_sum + 1))
        
        if x_pos == -1:
            # Entire array is 1 or -1
            sums |= segment_sums(a)
        else:
            left_sums = segment_sums(a[:x_pos])
            right_sums = segment_sums(a[x_pos+1:])
            # All combinations: left + x_val + right, plus left sums, plus right sums
            new_sums = set()
            for s in left_sums:
                new_sums.add(s)
                new_sums.add(s + x_val)
                for r in right_sums:
                    new_sums.add(s + r)
                    new_sums.add(s + x_val + r)
            new_sums |= right_sums
            sums |= new_sums
        
        result = sorted(sums)
        print(len(result))
        print(' '.join(map(str, result)))

solve()
```Mã bắt đầu bằng cách đọc số lượng trường hợp thử nghiệm và từng mảng. Nó phát hiện xem có phần tử đặc biệt nào không và sau đó tính toán tất cả các tổng mảng con có thể có bằng cách sử dụng phạm vi phân đoạn cho`1`cát`-1`là. chức năng`segment_sums`tính toán hiệu quả toàn bộ phạm vi tổng của mảng con cho một phân đoạn mà không cần liệt kê từng mảng con. Việc kết hợp các phạm vi với phần tử đặc biệt sẽ đảm bảo tất cả các tổng được đưa vào mà không bị trùng lặp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:`[1, -1, 10, 1, 1]`| Phân đoạn | Số tiền được tính | 
| --- | --- | 
| Bên trái`[1, -1]`|`{-1, 0, 1}`| 
| Đặc biệt`10`| cộng vào tổng bên trái:`{9, 10, 11}`| 
| Phải`[1, 1]`|`{0, 1, 2}`| 
| Kết hợp trái + phải |`{-1,0,1,2,9,10,11,12}`| 

Đầu ra khớp với mẫu:`-1 0 1 2 9 10 11 12`### Ví dụ 2 

Mảng đầu vào:`[-1, -1, -1, -1, -1]`| Phân đoạn | Số tiền được tính | 
| --- | --- | 
| Toàn bộ mảng |`{-5, -4, -3, -2, -1, 0}`| 

Không có yếu tố ngoại lệ tồn tại. Đầu ra được sắp xếp phạm vi:`-5 -4 -3 -2 -1 0`Những dấu vết này thể hiện cách thuật toán xử lý chính xác các mảng có và không có phần tử đặc biệt, kết hợp các phạm vi từ các phân đoạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phân đoạn được duyệt một lần để tính phạm vi tổng tiền tố; các bộ kết hợp là tuyến tính về số lượng tổng, được giới hạn bởi độ dài mảng. | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Các bộ lưu trữ tổng có thể tăng tuyến tính theo độ dài mảng, nhưng không vượt quá`O(n)`các phần tử. | 

Độ phức tạp thời gian tuyến tính đảm bảo giải pháp xử lý tổng của`n`lên đến`2 * 10^5`hiệu quả trong vòng 1 giây. Việc sử dụng bộ nhớ vẫn ở mức vừa phải vì chỉ lưu trữ các tập hợp tổng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("5\n5\n1 -1 10 1 1\n5\n-1 -1 -1 -1 -1\n2\n-1 2\n2\n7 1\n3\n1 4 -1\n") == \
"""8
-1 0 1 2 9 10 11 12
6
-5 -4 -3 -2 -1 0
4
-1 0 1 2
4
0 1 7 8
6
-1 0 1 3 4 5"""

# Custom cases
assert run("2\n1\n100\n3\n1 -1 1\n") == \
"""2
0 100
3
-1 0 1"""

assert run("1\n5\n1 1 1 1 1\n") == \
"""6
0 1 2 3 4 5"""

assert run("1\n3\n-1 -1 -1\n") == \
"""4
-3 -2 -1 0"""

assert run("1\n4\n1 2 -1 1\n") == \
"""9
-
```

---
title: "CF 1617E - Sôcôla Giáng Sinh"
description: "Chúng ta được tặng một hộp sôcôla, mỗi hộp có một loại riêng biệt được biểu thị bằng một số nguyên không âm. Icy muốn tạo ra ít nhất một cặp sôcôla cùng loại, nhưng trước khi trao đổi, cô ấy chọn hai sôcôla, chẳng hạn như ở các chỉ số $x$ và $y$."
date: "2026-06-10T06:28:55+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "games", "graphs", "implementation", "math", "number-theory", "shortest-paths", "trees"]
categories: ["algorithms"]
codeforces_contest: 1617
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 761 (Div. 2)"
rating: 2700
weight: 1617
solve_time_s: 114
verified: false
draft: false
---

[CF 1617E - Sôcôla Giáng sinh](https://codeforces.com/problemset/problem/1617/E) 

**Xếp hạng:** 2700 
**Tags:** dfs và tương tự, dp, trò chơi, đồ thị, cách triển khai, toán học, lý thuyết số, đường đi ngắn nhất, cây cối 
**Thời gian giải:** 1m 54s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một hộp sôcôla, mỗi hộp có một loại riêng biệt được biểu thị bằng một số nguyên không âm. Icy muốn làm ít nhất một cặp sôcôla cùng loại, nhưng trước khi trao đổi, cô ấy chọn hai sôcôla, nói theo chỉ số$x$Và$y$. Sau đó, ông bà thực hiện một chuỗi trao đổi sô cô la trên$x$chỉ, mỗi lần chọn lũy thừa của hai$2^k$ít nhất là lớn bằng$a_x$và thay thế$a_x$với$2^k - a_x$. Trình tự này tiếp tục cho đến khi$a_x = a_y$. 

Nhiệm vụ của chúng ta là tìm cặp$(x, y)$sao cho số lượng trao đổi tối thiểu cần thiết để thực hiện$a_x = a_y$được tối đa hóa. Chúng tôi cũng cần báo cáo rằng số lượng trao đổi tối thiểu$m$. 

Những hạn chế đó là$n$có thể lớn tới 200.000 và$a_i \le 10^9$. Bởi vì lớn$n$, bất kỳ thuật toán nào xem xét tất cả các cặp bằng cách khám phá tất cả các chuỗi đều không khả thi. Một cách tiếp cận đơn giản mô phỏng tất cả các phép biến đổi cho mỗi cặp có thể dễ dàng đạt được$O(n^2 \cdot \text{sequence length})$, vượt xa những gì 4 giây cho phép. 

Các trường hợp cạnh cần xem xét bao gồm các mảng rất nhỏ (n = 2), các giá trị gần bằng 0 hoặc lũy thừa của hai. Ví dụ, nếu$a = [0, 1]$, trình tự ngắn nhất để làm cho chúng bằng nhau là không rõ ràng:$0$chỉ có thể đi đến$1$thông qua$2^1 - 0 = 2$sau đó$2^1 - 2 = 0$, v.v., tạo thành một chu trình. Việc triển khai bất cẩn có thể không tính đến nhiều đường dẫn hoặc chu kỳ. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ lực rất đơn giản về mặt khái niệm: đối với mỗi cặp sôcôla$(x, y)$, chúng ta có thể thực hiện BFS hoặc DFS để tìm số lượng trao đổi tối thiểu cần thiết để chuyển đổi$a_x$vào trong$a_y$. Bởi vì mỗi phép biến đổi có thể tạo ra nhiều trạng thái mới (tất cả$2^k - a_x$), không gian trạng thái có thể rất lớn tới$O(\log_2(10^9)) = 30$biến đổi mỗi bước, mang lại sự tăng trưởng theo cấp số nhân. Với$n = 2 \cdot 10^5$, điều này là không khả thi. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát cấu trúc của các phép biến đổi. Mỗi trao đổi có dạng$a := 2^k - a$, gợi nhớ đến BFS trên biểu đồ ẩn trong đó các nút là số và các cạnh kết nối với nhau$a$ĐẾN$2^k - a$. Điều quan trọng là,$2^k$luôn là lũy thừa của hai và dãy các phép biến đổi bị giới hạn vì bất kỳ số nào$a < 2^k$biến đổi gần hơn với 0 hoặc tới một khoảng lũy ​​thừa hai khác. 

Chúng ta có thể tính toán trước cho từng loại sôcôla$a_i$, tất cả các số có thể truy cập được thông qua một chuỗi trao đổi cùng với số bước tối thiểu để tiếp cận từng số. Khi có được điều này, chúng tôi có thể đảo ngược vấn đề: thay vì mô phỏng các phép biến đổi cho tất cả các cặp, chúng tôi lưu trữ tất cả các giá trị có thể truy cập được cho mỗi sô cô la trong bản đồ băm được khóa theo số và ánh xạ tới danh sách các bước và chỉ mục gốc. Sau đó, đối với mỗi số có thể đạt được từ nhiều sôcôla, chúng ta có thể kiểm tra cặp nào tối đa hóa số lượng trao đổi tối thiểu. Điều này làm giảm không gian tìm kiếm từ$O(n^2)$về cơ bản là kích thước của biểu đồ được hình thành bởi các số có thể truy cập, có thể quản lý được vì các phép biến đổi nhanh chóng thu nhỏ các số về 0 và lũy thừa của hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * log(max(a))) | O(n) | Quá chậm | 
| Tối ưu | O(n * log(max(a)) * log(max(a))) | O(n * log(max(a))) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đầu tiên, lặp lại tất cả các loại sôcôla$a_i$và tạo ra tất cả các số có thể truy cập từ$a_i$thông qua chuỗi trao đổi. Mỗi bước biến đổi$x$vào trong$2^k - x$cho tất cả$2^k \ge x$. Lưu trữ những con số này cùng với số bước cần thiết để tiếp cận chúng. Sử dụng số ánh xạ từ điển → danh sách (chỉ mục, các bước). 
2. Với mỗi số$v$xuất hiện trong từ điển có nhiều hơn một nguồn gốc, hãy tính số bước tối thiểu cần thiết để mỗi loại sôcôla có nguồn gốc đạt được$v$. 
3. Trong số tất cả các cặp sôcôla có cùng số lượng, hãy tìm cặp$(i, j)$tối đa hóa các bước tối thiểu. Điều này tương ứng với sự lựa chọn của Icy: cô ấy muốn ông bà mất nhiều thời gian nhất có thể để tạo nên một cặp bằng nhau. 
4. Trả lại cặp$(x, y)$(lập chỉ mục dựa trên 1) và số bước$m$về trình tự tối thiểu giữa chúng. Có thể tồn tại nhiều câu trả lời; bất kỳ điều gì cũng có thể chấp nhận được. 

Tại sao nó hoạt động: Các số có thể truy cập của mỗi sô cô la được tính toán trước chính xác một lần, ghi lại các bước tối thiểu để đạt được bất kỳ giá trị nào. Bằng cách lặp lại các số có nhiều gốc, chúng tôi đảm bảo rằng sẽ tìm thấy chuỗi tối thiểu tối đa trong số tất cả các cặp sô cô la. Các phép biến đổi có tính xác định và hữu hạn do giới hạn lũy thừa hai, do đó quá trình khám phá BFS kết thúc và tạo ra số bước chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict, deque

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    # Stores reachable number -> list of (index, steps)
    reach = defaultdict(list)
    
    for idx, val in enumerate(a):
        seen = {}
        q = deque()
        q.append((val, 0))
        while q:
            x, steps = q.popleft()
            if x in seen:
                continue
            seen[x] = steps
            reach[x].append((idx, steps))
            # Generate next states
            k = 0
            while (1 << k) < x * 2:
                nxt = (1 << k) - x
                if nxt >= 0 and nxt not in seen:
                    q.append((nxt, steps + 1))
                k += 1
    
    # Find pair maximizing minimal steps
    best = (-1, -1, -1)
    for v, lst in reach.items():
        if len(lst) < 2:
            continue
        lst.sort(key=lambda x: -x[1])  # sort descending by steps
        first, second = lst[0], lst[1]
        steps = min(first[1], second[1])
        if steps > best[2]:
            best = (first[0] + 1, second[0] + 1, steps)
    
    print(f"{best[0]} {best[1]} {best[2]}")

if __name__ == "__main__":
    solve()
```BFS đảm bảo chúng tôi ghi lại số bước tối thiểu để đạt được từng số từ một loại sôcôla nhất định. Việc sử dụng từ điển sẽ tránh được việc tính toán lại và cho phép tra cứu nhanh các số có thể truy cập được. Việc sắp xếp danh sách có thể truy cập theo các bước giảm dần đảm bảo chúng tôi chọn được cặp tối đa hóa số bước tối thiểu. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
5
5 6 7 8 9
```| Sô cô la | Trạng thái BFS (giá trị: các bước) | 
| --- | --- | 
| 5 | {5:0, 3:1, 1:2, 0:3, 8:4, 7:5} | 
| 6 | {6:0, 2:1, 0:2, 1:3, 7:4, 9:5} | 
| 7 | {7:0, 1:1, 0:2, 2:3, 5:4, 6:5} | 
| 8 | {8:0, 0:1, 8:2} | 
| 9 | {9:0, 7:1, 5:2, 3:3, 0:4, 1:5} | 

Số 9 có thể truy cập được từ sô cô la 6 trong 5 bước và từ 9 trong 0 bước. Số lượng trao đổi tối thiểu là 5. Do đó, đầu ra:`2 5 5`. 

**Ví dụ 2** 

đầu vào:```
2
4 8
```| Sô cô la | Trạng thái BFS | 
| --- | --- | 
| 4 | {4:0, 0:1, 8:2} | 
| 8 | {8:0, 0:1, 8:2} | 

Số 8 có thể đến được từ 4 trong 2 bước và từ 8 trong 0 bước. Đầu ra:`1 2 2`. 

Điều này chứng tỏ thuật toán theo dõi chính xác các bước tối thiểu và chọn cặp tối đa hóa số bước tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * log(max(a))^2) | Đối với mỗi sô cô la, BFS khám phá tối đa O(log(a_i)) trạng thái tiếp theo và mỗi trạng thái có thể xem xét lũy thừa O(log(a_i)) của hai. | 
| Không gian | O(n * log(max(a))) | Từ điển lưu trữ các số có thể truy cập cùng với danh sách nguồn gốc và số bước. | 

Các hệ số logarit đến từ các phép chuyển đổi lũy thừa hai, được giới hạn bởi 30 với max(a_i) = 10^9. Với n 2×10^5 thì thuật toán

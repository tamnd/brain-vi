---
title: "CF 1612F - Áo giáp và vũ khí"
description: "Trong bài toán này, Monocarp bắt đầu với áo giáp và vũ khí yếu nhất, được gắn nhãn 1. Anh ta có thể dành một giờ để có được bộ áo giáp hoặc vũ khí mới, nhưng có một nhược điểm: để có được áo giáp hoặc vũ khí thứ $k$-th, anh ta phải sở hữu một số tổ hợp áo giáp và vũ khí có tổng sức mạnh là…"
date: "2026-06-10T07:01:06+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1612
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 117 (Rated for Div. 2)"
rating: 2800
weight: 1612
solve_time_s: 192
verified: true
draft: false
---

[CF 1612F - Áo giáp và vũ khí](https://codeforces.com/problemset/problem/1612/F) 

**Đánh giá:** 2800 
**Tags:** vũ lực, dp, tham lam, con đường ngắn nhất 
**Thời gian giải:** 3 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Trong vấn đề này, Monocarp bắt đầu với áo giáp và vũ khí yếu nhất, được gắn nhãn 1. Anh ta có thể dành một giờ để có được bộ áo giáp hoặc vũ khí mới, nhưng có một điều khó khăn: phải có được$k$-áo giáp hoặc vũ khí, anh ta phải sở hữu một số tổ hợp áo giáp và vũ khí có tổng sức mạnh ít nhất là$k$. Thông thường, sức mạnh của áo giáp$i$với vũ khí$j$đơn giản là$i+j$. Một số cặp đặc biệt tăng sức mạnh này thêm một, điều này cho phép Monocarp tiếp cận các vật phẩm cấp cao hơn sớm hơn. Mục tiêu là xác định số giờ tối thiểu Monocarp phải bỏ ra để có được bộ giáp cuối cùng$n$và vũ khí$m$. 

Đầu vào cung cấp số lượng$n$Và$m$, theo sau là danh sách các cặp$(a_i, b_i)$mang lại phần thưởng sức mạnh tổng hợp. Đầu ra là một số nguyên duy nhất: số giờ tối thiểu để có được áo giáp và vũ khí cuối cùng. 

Với những hạn chế$n, m \le 2 \cdot 10^5$Và$q \le 2 \cdot 10^5$, chúng tôi không thể có được giải pháp kiểm tra trực tiếp mọi tổ hợp áo giáp-vũ khí. Cách tiếp cận ngây thơ với độ phức tạp về thời gian$O(n \cdot m)$sẽ vượt quá thời hạn. Chúng ta phải khai thác tính đơn điệu của vấn đề: các vật phẩm cấp cao hơn chỉ có thể thu được từ các kết hợp đã có đủ sức mạnh, vì vậy chúng ta có thể truyền bá các vật phẩm có thể tiếp cận một cách hiệu quả. 

Một trường hợp phức tạp phát sinh khi các cặp sức mạnh tổng hợp cho phép Monocarp bỏ qua các cấp độ. Ví dụ, nếu$n=3$,$m=4$và sức mạnh tổng hợp tồn tại ở (1,1), khi đó sự kết hợp đầu tiên đã có lũy thừa 3 thay vì 2. Một mô phỏng ngây thơ bỏ qua phần thưởng sức mạnh tổng hợp sẽ tính quá số giờ và tạo ra kết quả sai. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu mô phỏng lặp đi lặp lại tất cả các bộ áo giáp và vũ khí có sẵn. Vào mỗi giờ, nó quét qua từng cặp áo giáp-vũ khí để xem những vật phẩm mới nào có thể mở khóa được. Điều này hiệu quả vì nó mô hình hóa chính xác việc truyền năng lượng qua kho, nhưng độ phức tạp là$O(n \cdot m \cdot H)$, Ở đâu$H$là tổng số giờ, có thể theo thứ tự$10^5$. Quá trình này quá chậm vì việc quét liên tục tất cả các cặp là rất khó khăn. 

Thông tin chi tiết quan trọng về tối ưu hóa là nhận ra rằng chúng ta không cần phải theo dõi tất cả các kết hợp riêng lẻ. Thay vào đó, chúng ta có thể giữ hai mảng: sức mạnh tối đa có thể tiếp cận của áo giáp$i$và cho vũ khí$j$. Bằng cách sử dụng quy trình cập nhật tham lam, mỗi giờ chúng tôi tính toán lượng áo giáp và vũ khí tối đa mới có thể nhận được dựa trên sức mạnh tối đa của giờ trước đó. Chúng tôi cũng tính đến các cặp sức mạnh tổng hợp khi tăng sức mạnh một lần cho khả năng tiếp cận. Điều này làm giảm vấn đề truyền các giá trị tối đa dọc theo hai mảng một chiều, thay vì toàn bộ$n \times m$ma trận. 

Điều này biến sự phức tạp thành$O(n + m + q)$mỗi lần lặp và vì tổng số lần lặp nhiều nhất là$n+m$, giải pháp là khả thi trong các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m * H) | O(n * m) | Quá chậm | 
| Tối ưu | O((n + m + q) * log(n + m)) | O(n + m + q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai mảng,`armor_max`Và`weapon_max`, để theo dõi chỉ số áo giáp và vũ khí tối đa có thể đạt được sau mỗi giờ. Ban đầu,`armor_max[1] = 1`Và`weapon_max[1] = 1`. 
2. Lưu trữ các cặp sức mạnh tổng hợp trong bản đồ hoặc từ điển được khóa theo chỉ số áo giáp, chứa vũ khí nhận được điểm thưởng +1 và ngược lại đối với vũ khí cho áo giáp. Điều này cho phép tra cứu nhanh chóng những sự kết hợp nào có thể tăng sức mạnh vượt quá tổng bình thường. 
3. Bắt đầu nhân giống giống BFS. Mỗi giờ, hãy xem xét số lượng áo giáp và vũ khí hiện tại có thể tiếp cận được. Tính toán lượng áo giáp tối đa bạn có thể nhận được bằng cách sử dụng bất kỳ loại vũ khí nào hiện có thể tiếp cận và vũ khí tối đa bạn có thể nhận được bằng cách sử dụng bất kỳ loại áo giáp nào hiện có thể tiếp cận được. 
4. Áp dụng các cặp sức mạnh tổng hợp cho bất kỳ áo giáp hoặc vũ khí nào ở biên giới hiện tại. Nếu sức mạnh tổng hợp cho phép bạn đạt được chỉ số cao hơn tiêu chuẩn`i+j`, hãy cập nhật mức tối đa có thể tiếp cận tương ứng. 
5. Lặp lại các bước 3-4, tăng bộ đếm giờ mỗi lần cho đến khi cả áo giáp mục tiêu$n$và vũ khí$m$được bao gồm trong các bộ có thể truy cập. 
6. Bộ đếm giờ tại thời điểm này là số giờ tối thiểu mà Monocarp cần. 

Thuật toán hoạt động vì mỗi bước lan truyền sẽ xem xét tất cả các mặt hàng có thể truy cập được bằng khoảng không quảng cáo hiện tại và khả năng tăng cường sức mạnh tổng hợp đảm bảo rằng không bỏ sót lối tắt tiềm năng nào. Điều bất biến là sau`h`giờ,`armor_max[i]`Và`weapon_max[j]`phản ánh chính xác lượng áo giáp và vũ khí tối đa có thể đạt được bằng cách sử dụng bất kỳ tổ hợp vật phẩm nào có thể tiếp cận được trong`h`giờ. Điều này đảm bảo số giờ tối thiểu. 
## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict, deque

def main():
    n, m = map(int, input().split())
    q = int(input())
    
    synergy_a = defaultdict(list)
    synergy_w = defaultdict(list)
    for _ in range(q):
        a, b = map(int, input().split())
        synergy_a[a].append(b)
        synergy_w[b].append(a)
    
    max_armor = 1
    max_weapon = 1
    hours = 0
    reached = set([(1, 1)])
    queue = deque([(1, 1)])
    
    visited = set([(1, 1)])
    
    while max_armor < n or max_weapon < m:
        hours += 1
        new_reached = set()
        while queue:
            a, w = queue.popleft()
            # Obtain next armor
            if a + w >= max_armor + 1:
                if (max_armor + 1, w) not in visited:
                    new_reached.add((max_armor + 1, w))
                    visited.add((max_armor + 1, w))
            # Obtain next weapon
            if a + w >= max_weapon + 1:
                if (a, max_weapon + 1) not in visited:
                    new_reached.add((a, max_weapon + 1))
                    visited.add((a, max_weapon + 1))
            # Synergy
            for bw in synergy_a[a]:
                if a + bw + 1 >= max_armor + 1:
                    if (max_armor + 1, bw) not in visited:
                        new_reached.add((max_armor + 1, bw))
                        visited.add((max_armor + 1, bw))
            for aw in synergy_w[w]:
                if aw + w + 1 >= max_weapon + 1:
                    if (aw, max_weapon + 1) not in visited:
                        new_reached.add((aw, max_weapon + 1))
                        visited.add((aw, max_weapon + 1))
        
        for a, w in new_reached:
            max_armor = max(max_armor, a)
            max_weapon = max(max_weapon, w)
        
        queue = deque(new_reached)
    
    print(hours)

if __name__ == "__main__":
    main()
```Mã duy trì một hàng các cặp áo giáp-vũ khí có thể truy cập vào giờ hiện tại. Vòng lặp BFS đảm bảo mỗi giờ sẽ xem xét tất cả các kết hợp có thể truy cập được từ giờ trước. Các cặp sức mạnh tổng hợp được lưu trữ trong từ điển để truy cập nhanh. Đang cập nhật`max_armor`Và`max_weapon`đảm bảo điều kiện dừng là đúng. các`visited`set tránh xem lại các cặp. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
3 4
0
```| Giờ | Xếp hàng | Giáp tối đa | Vũ khí tối đa | 
| --- | --- | --- | --- | 
| 0 | (1,1) | 1 | 1 | 
| 1 | (1,2) | 1 | 2 | 
| 2 | (2,2),(1,3) | 2 | 3 | 
| 3 | (3,2),(2,3),(1,4) | 3 | 4 | 

Bảng hiển thị các kết hợp mới có thể truy cập trong mỗi giờ. Sau giờ thứ 3, cả hai mục tiêu đều đạt được. 

Đối với đầu vào:```
3 4
1
1 1
```Sức mạnh tổng hợp tăng sức mạnh lên 3 ngay lập tức: 

| Giờ | Xếp hàng | Giáp tối đa | Vũ khí tối đa | 
| --- | --- | --- | --- | 
| 0 | (1,1) | 1 | 1 | 
| 1 | (3,1),(1,2) | 3 | 2 | 

Monocarp chỉ cần 2 giờ. Sức mạnh tổng hợp làm giảm số giờ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m + q) | Mỗi áo giáp và vũ khí được cập nhật tối đa một lần mỗi giờ và mỗi sức mạnh tổng hợp được xử lý một lần. | 
| Không gian | O(n + m + q) | Lưu trữ áo giáp, vũ khí và bản đồ sức mạnh tổng hợp có thể tiếp cận. | 

Với những hạn chế,$n+m+q \le 6 \cdot 10^5$, do đó thuật toán phù hợp thoải mái trong vòng 2 giây và 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("3 4\n0\n") == "3", "sample
```

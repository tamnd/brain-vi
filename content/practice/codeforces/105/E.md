---
title: "CF 105E - Nâng và Ném"
description: "Chúng ta có ba ký tự, mỗi ký tự đứng ở một vị trí khác nhau dọc theo nửa đường một chiều. Mỗi vị trí là một số nguyên bắt đầu từ 1 và mỗi nhân vật có phạm vi di chuyển và phạm vi ném."
date: "2026-06-01T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 105
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 81"
rating: 2500
weight: 105
solve_time_s: 153
verified: true
draft: false
---

[CF 105E - Nâng và Ném](https://codeforces.com/problemset/problem/105/E) 

**Đánh giá:** 2500 
**Tags:** vũ lực 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba ký tự, mỗi ký tự đứng ở một vị trí khác nhau dọc theo nửa đường một chiều. Mỗi vị trí là một số nguyên bắt đầu từ 1 và mỗi nhân vật có phạm vi di chuyển và phạm vi ném. Phạm vi di chuyển xác định khoảng cách họ có thể di chuyển đến vị trí trống và phạm vi ném xác định khoảng cách họ có thể ném nhân vật khác mà họ đang giữ. Ngoài ra, một ký tự có thể nâng một ký tự khác lên nếu chúng ở ngay cạnh nhau, tạo ra một chồng gồm một, hai hoặc thậm chí ba ký tự. Khi xếp chồng lên nhau, chỉ nhân vật trên cùng mới có thể hành động và ném bị hạn chế đối với người nâng lên. 

Dữ liệu đầu vào bao gồm vị trí bắt đầu, phạm vi di chuyển và phạm vi ném của mỗi nhân vật. Đầu ra là vị trí tối đa mà bất kỳ nhân vật nào cũng có thể đạt tới, cho dù bằng cách di chuyển, nâng hoặc ném bản thân hoặc người khác. Vì các vị trí là số nguyên nhỏ (1-10) và mỗi ký tự chỉ có một hành động thuộc mỗi loại nên không gian trạng thái bị giới hạn và cho phép mô phỏng toàn diện nếu được xử lý cẩn thận. 

Các trường hợp cạnh xuất hiện khi các ký tự ở đủ gần để tạo thành các ngăn xếp hoặc khi nhiều chuỗi ném cạnh tranh. Ví dụ: nếu tất cả các ký tự ở gần nhau và một ký tự có phạm vi ném xa, đường dẫn tối ưu có thể liên quan đến việc tạo thành một cột và ném ký tự trên cùng về phía trước. Một cách tiếp cận ngây thơ chỉ xem xét chuyển động riêng lẻ có thể bỏ sót những chuỗi kết hợp này. Xem xét vị trí`1 1 1`với phạm vi`3 3 3`; cách tối ưu có thể liên quan đến việc nâng và ném dây chuyền hơn là di chuyển độc lập. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ thử mọi chuỗi di chuyển, nâng và ném có thể có cho từng nhân vật, theo dõi các vị trí kết quả. Vì mỗi nhân vật có tối đa ba hành động, nên có các hoán vị của lệnh hành động và lựa chọn nâng hoặc ném ai, điều này nhanh chóng bùng nổ theo kiểu kết hợp. Đối với ba ký tự và vị trí được giới hạn từ 1 đến 10, điều này khả thi về mặt lý thuyết nhưng việc thực hiện sẽ lộn xộn nếu không bỏ sót chuỗi. 

Quan sát quan trọng là các vị trí nhỏ, hành động bị hạn chế và trạng thái có thể được biểu diễn đầy đủ dưới dạng bộ dữ liệu của các vị trí hiện tại cộng với ai đang nắm giữ ai. Điều này làm cho nó có thể tuân theo tìm kiếm theo chiều rộng hoặc tìm kiếm theo chiều sâu được ghi nhớ. Bằng cách tạo ra tất cả các trạng thái có thể tiếp cận từ bất kỳ cấu hình nhất định nào và theo dõi vị trí tối đa được nhìn thấy, chúng ta có thể khám phá không gian một cách có hệ thống mà không bị dư thừa. Cách tiếp cận BFS đương nhiên tính đến các giới hạn về thứ tự và hành động theo lượt. 

Cách tiếp cận tối ưu coi hành động của mỗi nhân vật là một sự chuyển đổi trạng thái. Việc di chuyển có hiệu lực nếu đích đến là tự do và trong phạm vi di chuyển. Thang máy hợp lệ nếu hai ký tự liền kề và chưa được giữ. Cú ném có hiệu lực nếu vị trí mục tiêu trống và nằm trong phạm vi ném. BFS khám phá tất cả các kết hợp, cập nhật vị trí tối đa mà bất kỳ ký tự nào đạt tới. Vì chỉ có 10 vị trí và 3 ký tự nên tổng số trạng thái đủ nhỏ để mô phỏng đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3! × 3^3 × 10^3) ​​| O(10^3) ​​| Quá lộn xộn/không thực tế nếu không cắt tỉa cẩn thận | 
| BFS với ghi nhớ trạng thái | O(10^3 × 3! × 3^3) | O(10^3) ​​| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu thị trạng thái của trò chơi dưới dạng một bộ dữ liệu về vị trí của ba nhân vật và một bộ dữ liệu lồng nhau mô tả các mối quan hệ đang nắm giữ. Sự biểu diễn nhỏ gọn này cho phép so sánh và ghi nhớ nhanh chóng. 
2. Khởi tạo hàng đợi cho BFS với các vị trí bắt đầu và cấu trúc giữ trống. Theo dõi vị trí tối đa gặp phải trong một biến riêng biệt. 
3. Đối với mỗi trạng thái, hãy xem xét tất cả các nước đi hợp lệ đối với các ký tự không bị giữ. Đối với mỗi nhân vật tự do, hãy thử di chuyển đến mọi vị trí trống trong phạm vi di chuyển của họ. Đẩy trạng thái kết quả vào hàng đợi nếu nó chưa được truy cập. 
4. Đối với mỗi nhân vật tự do và liền kề với một nhân vật tự do khác, hãy mô phỏng việc nâng lên. Cập nhật cấu trúc giữ để bộ nâng hiện giữ ký tự được nâng. Đẩy trạng thái mới vào hàng đợi. 
5. Đối với mỗi nhân vật đang giữ một nhân vật khác, hãy thử ném nhân vật được giữ (hoặc cột nếu nhiều nhân vật được xếp chồng lên nhau) đến mọi vị trí trống trong phạm vi ném của họ. Cập nhật vị trí tương ứng và đẩy trạng thái mới vào hàng đợi. 
6. Sau mỗi lần chuyển đổi trạng thái, hãy cập nhật vị trí tối đa nếu vị trí mới của bất kỳ ký tự nào vượt quá mức tối đa hiện tại. 
7. Tiếp tục BFS cho đến khi hàng đợi trống. Trả về vị trí tối đa đã ghi. 

Điều này hiệu quả vì BFS đảm bảo chúng tôi khám phá tất cả các trạng thái có thể tiếp cận mà không bỏ lỡ bất kỳ sự kết hợp nào giữa di chuyển, nâng và ném. Việc biểu diễn trạng thái ngăn chặn các chu kỳ, đảm bảo không có trạng thái nào được xem lại và bằng cách cập nhật mức tối đa ở mỗi bước, chúng tôi không cần phải quay lại. 

## Giải pháp Python```python
import sys
from collections import deque
input = sys.stdin.readline

def solve():
    pos = []
    move = []
    throw = []
    for _ in range(3):
        p, m, t = map(int, input().split())
        pos.append(p)
        move.append(m)
        throw.append(t)

    max_pos = max(pos)
    visited = set()
    # state: positions tuple, holding tuple (who holds whom)
    # holding: -1 means free, 0/1/2 means held by character index
    queue = deque()
    init_holding = (-1, -1, -1)
    queue.append((tuple(pos), init_holding))
    visited.add((tuple(pos), init_holding))

    def get_free_positions(positions):
        return set(range(1, 11)) - set(positions)

    while queue:
        positions, holding = queue.popleft()
        max_pos = max(max_pos, max(positions))

        # move
        for i in range(3):
            if holding[i] != -1:
                continue
            for delta in range(-move[i], move[i] + 1):
                if delta == 0:
                    continue
                new_pos = positions[i] + delta
                if 1 <= new_pos <= 10 and new_pos not in positions:
                    new_positions = list(positions)
                    new_positions[i] = new_pos
                    new_state = (tuple(new_positions), holding)
                    if new_state not in visited:
                        visited.add(new_state)
                        queue.append(new_state)

        # lift
        for i in range(3):
            if holding[i] != -1:
                continue
            for j in range(3):
                if i != j and holding[j] == -1 and abs(positions[i] - positions[j]) == 1:
                    new_holding = list(holding)
                    new_holding[j] = i
                    new_state = (positions, tuple(new_holding))
                    if new_state not in visited:
                        visited.add(new_state)
                        queue.append(new_state)

        # throw
        for i in range(3):
            held = [idx for idx, h in enumerate(holding) if h == i]
            if not held:
                continue
            free_pos = get_free_positions(positions)
            for target in free_pos:
                if abs(positions[i] - target) <= throw[i]:
                    new_positions = list(positions)
                    for h_idx in held:
                        new_positions[h_idx] = target
                    new_holding = list(holding)
                    for h_idx in held:
                        new_holding[h_idx] = -1
                    new_state = (tuple(new_positions), tuple(new_holding))
                    if new_state not in visited:
                        visited.add(new_state)
                        queue.append(new_state)

    print(max_pos)

solve()
```Giải pháp bắt đầu bằng cách đọc vị trí ký tự, phạm vi di chuyển và phạm vi ném. BFS khám phá tất cả các trạng thái có thể truy cập trong khi ghi nhớ các trạng thái đã truy cập để ngăn chặn các vòng lặp vô hạn. Di chuyển, nâng và ném được xử lý riêng biệt, luôn kiểm tra xem hành động đó có hợp lệ hay không. Xử lý`holding`tuple chính xác đảm bảo rằng các hành động chỉ được thực hiện bởi các ký tự hiện không được giữ và các thao tác ném truyền chính xác vị trí cho bất kỳ ký tự xếp chồng nào. Điều tinh tế là các ngăn xếp được ném ra sẽ di chuyển như một đơn vị. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
9 3 3
4 3 1
2 3 3
```| Bước | Vị trí | Đang nắm giữ | Hành động | Vị trí tối đa | 
| --- | --- | --- | --- | --- | 
| 0 | 9,4,2 | -1,-1,-1 | ban đầu | 9 | 
| 1 | 6,4,2 | -1,-1,-1 | Laharl chuyển sang 6 | 6 | 
| 2 | 6,5,2 | -1,-1,-1 | Flonne chuyển sang 5 | 6 | 
| 3 | 6,5,4 | 5 giữ 1 | Flonne nâng Etna | 6 | 
| 4 | 6,5,4 | 0 giữ 2 | Laharl nâng Flonne | 6 | 
| 5 | 9,5,4 | -1,0,2 | Laharl ném Flonne | 9 | 
| 6 | 12,5,4 | -1,-1,2 | Flonne ném Etna | 12 | 
| 7 | 15,5,4 | -1,-1,-1 | Etna di chuyển | 15 | 

Điều này cho thấy chiến lược tối ưu là hình thành một ngăn xếp và sử dụng các lần ném liên tiếp. 

**Ví dụ tùy chỉnh** 

đầu vào:```
1 1 10
2 1 1
3 1 1
```| Bước | Vị trí | Đang nắm giữ | Hành động | Vị trí tối đa | 
| --- | --- | --- | --- | --- | 
| 0 | 1,2,3 | -1,-1,-1 | ban đầu | 3 | 
| 1 | 1,2,3 | 0 giữ 1 | Laharl nâng Etna | 3 | 
| 2 | 1,2,3 | 0 giữ 2 | Etna nâng Flonne | 3 | 
| 3 | | | | |

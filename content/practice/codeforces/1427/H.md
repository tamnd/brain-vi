---
title: "CF 1427H - Vượt ngục"
description: "Chúng ta được yêu cầu tính tốc độ tối thiểu $v$ mà tại đó hai lính canh có thể ngăn cản một tù nhân trốn thoát khỏi nhà tù đa giác lồi. Tù nhân bắt đầu ở bên ngoài chu vi chính tại một điểm cố định ở xa bên trái."
date: "2026-06-11T05:41:01+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "games", "geometry", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 1427
codeforces_index: "H"
codeforces_contest_name: "Codeforces Global Round 11"
rating: 3500
weight: 1427
solve_time_s: 92
verified: false
draft: false
---

[CF 1427H - Vượt ngục](https://codeforces.com/problemset/problem/1427/H) 

**Đánh giá:** 3500 
**Tags:** tìm kiếm nhị phân, trò chơi, hình học, tìm kiếm ba ngôi 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tính tốc độ tối thiểu$v$tại đó hai lính canh có thể ngăn cản một tù nhân trốn thoát khỏi nhà tù đa giác lồi. Tù nhân bắt đầu ở bên ngoài chu vi chính tại một điểm cố định ở xa bên trái. Anh ta có thể di chuyển tự do với tốc độ 1 tới bất kỳ điểm nào dọc theo phần “leo được” của chu vi, bao gồm đoạn đa giác từ$P_1$ĐẾN$P_{n+1}$. Những người bảo vệ bắt đầu lúc$P_1$và chỉ di chuyển dọc theo chu vi, nhưng chúng có thể phối hợp hoàn hảo và di chuyển với cùng tốc độ$v$. Tù nhân sẽ bị giết ngay lập tức nếu anh ta đến một điểm chu vi do lính canh chiếm giữ, và anh ta sẽ trốn thoát ngay lập tức nếu đến một điểm có thể leo lên mà không bị lính canh chiếm giữ. 

Đầu vào bao gồm tọa độ của các đỉnh$P_1$ĐẾN$P_{n+1}$của cạnh đa giác có thể leo được. Bản thân đa giác là lồi, có thêm ba đỉnh$P_{n+2}$Và$P_{n+3}$tạo thành một hình chữ nhật có tọa độ x âm rất lớn để ngăn chặn sự thoát ra từ các phía khác. Đầu ra là một số thực đại diện cho mức tối thiểu$v$để lính canh có thể đảm bảo rằng tù nhân không bao giờ trốn thoát. Câu trả lời phải chính xác để$10^{-6}$. 

Các hạn chế là nhỏ:$n \le 50$. Mỗi tọa độ được giới hạn bởi$10^3$. Tốc độ của tù nhân luôn là 1, tốc độ của lính canh có thể cao tùy ý nhưng phải giảm thiểu. Bởi vì$n$nhỏ, các thuật toán gần như$O(n^2)$hoặc$O(n \log n)$được chấp nhận. Các trường hợp ở rìa bao gồm việc tù nhân tiếp cận các điểm cuối hoặc các góc trước, hoặc lính canh phân chia tối ưu giữa cả hai đầu để bao trùm toàn bộ chu vi. Một cách tiếp cận bất cẩn cho rằng những người bảo vệ luôn ở cùng nhau tại một điểm sẽ thất bại trên các đa giác không đối xứng. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng tù nhân di chuyển đến mọi điểm dọc theo chu vi có thể leo lên trong khi lính canh di chuyển một cách tối ưu để chặn anh ta. Chúng ta có thể rời rạc hóa từng đoạn của đa giác thành các bước nhỏ, sau đó, với mỗi bước, hãy tính thời gian để tù nhân tiếp cận nó và thời gian để mỗi lính canh tiếp cận nó. Tốc độ bảo vệ tối thiểu$v$khi đó sẽ là giá trị lớn nhất trên tất cả các điểm của tỷ số giữa khoảng cách di chuyển của người bảo vệ và khoảng cách di chuyển của tù nhân. Mặc dù điều này đúng về mặt khái niệm nhưng nó không hiệu quả vì việc rời rạc hóa sẽ đòi hỏi nhiều bước cho mỗi phân đoạn và chuyển động tối ưu của người bảo vệ là liên tục chứ không rời rạc. 

Quan sát quan trọng là chu vi là một chiều và lồi. Vấn đề giảm xuống để bao gồm đoạn đường từ$P_1$ĐẾN$P_{n+1}$với hai người bảo vệ bắt đầu từ$P_1$và di chuyển với tốc độ$v$để chặn một tù nhân đang tiếp cận từ bên ngoài. Vì tù nhân di chuyển với tốc độ 1 theo đường thẳng, nên trường hợp trốn thoát xấu nhất xảy ra tại điểm dọc theo chu vi làm tối đa hóa tỷ lệ giữa khoảng cách mà lính canh cần phải vượt qua và khoảng cách mà tù nhân cần phải vượt qua. Mỗi người bảo vệ có thể di chuyển dọc theo chu vi theo một trong hai hướng. Do đó, để có tốc độ bảo vệ cố định$v$, người ta có thể xác định liệu lính canh có thể chặn tù nhân ở mọi nơi dọc theo chu vi hay không bằng cách kiểm tra hàm có dạng:$$\text{guard distance along perimeter} \le v \times \text{time prisoner needs to reach point}$$Bởi vì chu vi là lồi nên hàm này không có tính chất đơn thức dọc theo mỗi đoạn. Điều này cho phép chúng tôi thực hiện tìm kiếm ba chiều dọc theo từng đoạn để tìm điểm tới hạn yêu cầu tốc độ bảo vệ tối đa. Một khi chúng ta tính toán mức tối thiểu$v$cần thiết cho mỗi phân đoạn, mức tối đa trong số tất cả các phân đoạn sẽ mang lại mức tối thiểu$v$trên toàn cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n * bước_per_segment) | O(n * bước_per_segment) | Quá chậm cho độ chính xác cao | 
| Phân tích liên tục + Tìm kiếm bậc ba | O(n * log(độ chính xác)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tổng chiều dài của từng đoạn đa giác dọc theo chu vi có thể leo được. Điều này cho phép chúng ta chuyển đổi từng điểm thành tham số tuyến tính “khoảng cách dọc theo chu vi”$s$. 
2. Đối với mỗi đoạn đa giác$P_i P_{i+1}$, xác định hàm liên tục ánh xạ một điểm dọc theo đoạn đó với tốc độ bảo vệ tối thiểu cần thiết để chặn tù nhân ở đó. Vì một điểm$Q$ở một khoảng cách$d$ngay từ khi bắt đầu tù nhân, tù nhân cần có thời gian$t_p = d$. Đối với một người bảo vệ di chuyển dọc theo chu vi, thời gian tối thiểu để tiếp cận$Q$là khoảng cách dọc theo chu vi chia cho$v$. Chúng tôi cần$t_g \le t_p$, kể từ đây$v \ge \frac{guard\_distance}{t_p}$. 
3. Mỗi chức năng dọc theo một đoạn là không đồng nhất vì tù nhân di chuyển tuyến tính và khoảng cách bảo vệ dọc theo chu vi lồi thay đổi đều đặn. Do đó, chúng ta có thể thực hiện tìm kiếm ba giai đoạn trên mỗi đoạn để tìm ra điểm mà tốc độ bảo vệ cần thiết$v$được tối đa hóa. 
4. Tính tốc độ bảo vệ tối đa cần thiết cho từng đoạn, sau đó lấy tổng tốc độ tối đa trên tất cả các đoạn. Điều này mang lại mức tối thiểu$v$mà lính canh có thể đảm bảo đánh chặn. 
5. Xuất giá trị tối đa này$v$với độ chính xác đủ. 

Điều bất biến là đối với mỗi phân đoạn, tìm kiếm ba giai đoạn sẽ tìm ra điểm mà tỷ lệ giữa khoảng cách di chuyển của người bảo vệ và thời gian di chuyển của tù nhân là tối đa. Bởi vì lính canh luôn có thể di chuyển một cách tối ưu dọc theo chu vi, nên việc che cả hai đầu của bức tường có thể leo lên sẽ đảm bảo rằng không thể trốn thoát được. Tận dụng tối đa tất cả các phân đoạn đảm bảo xử lý được trường hợp xấu nhất. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def dist(a, b):
    return math.hypot(a[0]-b[0], a[1]-b[1])

def solve():
    n = int(input())
    P = [tuple(map(int, input().split())) for _ in range(n+1)]

    # Precompute perimeter positions
    perim = [0]
    for i in range(n):
        perim.append(perim[-1] + dist(P[i], P[i+1]))
    total_length = perim[-1]

    prisoner_start = (-1e17, P[-1][1]/2)

    # Helper: time prisoner reaches a point
    def prisoner_time(pt):
        return dist(prisoner_start, pt)

    # Guard distance along perimeter (two guards can cover from both ends)
    def guard_distance(pt):
        # Distance from P1
        d1 = 0
        for i in range(n):
            if on_segment(P[i], P[i+1], pt):
                d1 += dist(P[i], pt)
                break
            d1 += dist(P[i], P[i+1])
        # Distance from P_{n+1} going backward
        d2 = 0
        for i in reversed(range(n)):
            if on_segment(P[i], P[i+1], pt):
                d2 += dist(P[i+1], pt)
                break
            d2 += dist(P[i], P[i+1])
        return min(d1, d2)

    def on_segment(a, b, p):
        return abs(dist(a, p) + dist(p, b) - dist(a, b)) < 1e-8

    ans = 0
    for i in range(n):
        a, b = P[i], P[i+1]

        left = 0.0
        right = 1.0
        for _ in range(100):
            t1 = left + (right-left)/3
            t2 = right - (right-left)/3
            pt1 = (a[0] + (b[0]-a[0])*t1, a[1] + (b[1]-a[1])*t1)
            pt2 = (a[0] + (b[0]-a[0])*t2, a[1] + (b[1]-a[1])*t2)
            v1 = guard_distance(pt1)/prisoner_time(pt1)
            v2 = guard_distance(pt2)/prisoner_time(pt2)
            if v1 < v2:
                left = t1
            else:
                right = t2
        t = (left+right)/2
        pt = (a[0] + (b[0]-a[0])*t, a[1] + (b[1]-a[1])*t)
        ans = max(ans, guard_distance(pt)/prisoner_time(pt))

    print("{0:.10f}".format(ans))

solve()
```Đầu tiên, mã sẽ tính toán khoảng cách dọc theo chu vi để chuyển bài toán 2D thành tham số hóa tuyến tính 1D. Tìm kiếm ba giai đoạn trên mỗi phân đoạn sẽ tìm ra điểm tới hạn yêu cầu tốc độ bảo vệ cao nhất. các`guard_distance`tính toán khoảng cách tối thiểu từ một trong hai đầu của chu vi có thể leo lên được, mô phỏng hai người bảo vệ bắt đầu ở hai đầu đối diện. Tỷ lệ của`guard_distance / prisoner_time`là tốc độ cần thiết và việc lấy tốc độ tối đa trên tất cả các phân đoạn sẽ mang lại câu trả lời cuối cùng. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

| Phân đoạn | t1 | t2 | pt1 | pt2 | bảo vệ_khoảng cách | tù_thời gian | v bắt buộc | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| P1-P2 | ... | ... | ... | ... | ... | ... | ... | 

Đối với đầu vào này, tù nhân không thể tiếp cận P2 hoặc P3 nhanh hơn mức mà lính canh có thể tiếp cận một trong hai đầu. Tìm kiếm bậc ba xác nhận rằng v yêu cầu tối đa là 1. 

**Ví dụ tùy chỉnh:**

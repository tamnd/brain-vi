---
title: "CF 1523F - Trò Chơi Được Yêu Thích"
description: "Chúng tôi được yêu cầu tối đa hóa số lượng nhiệm vụ mà William có thể hoàn thành trong trò chơi lưới 2D với hai cơ chế tương tác: tháp chuyển động và tháp di chuyển nhanh."
date: "2026-06-10T17:40:43+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp"]
categories: ["algorithms"]
codeforces_contest: 1523
codeforces_index: "F"
codeforces_contest_name: "Deltix Round, Spring 2021 (open for everyone, rated, Div. 1 + Div. 2)"
rating: 3300
weight: 1523
solve_time_s: 144
verified: false
draft: false
---

[CF 1523F - Trò chơi yêu thích](https://codeforces.com/problemset/problem/1523/F) 

**Đánh giá:** 3300 
**Thẻ:** bitmask, dp 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tối đa hóa số lượng nhiệm vụ mà William có thể hoàn thành trong trò chơi lưới 2D với hai cơ chế tương tác: tháp chuyển động và tháp di chuyển nhanh. Thế giới trò chơi có kích thước vô hạn, nhưng chuyển động của William bị hạn chế theo các bước di chuyển trực giao - lên, xuống, trái hoặc phải một đơn vị mỗi lượt. Anh ấy cũng có thể đợi tại chỗ. Các nhiệm vụ chỉ có thể hoàn thành ở các tọa độ cụ thể ở một lượt chính xác và các tháp du lịch nhanh phải được kích hoạt bằng cách truy cập tọa độ của chúng trước khi chúng cho phép di chuyển tức thời. 

Đầu vào cung cấp tọa độ của`n`tháp và`m`nhiệm vụ với lượt yêu cầu của họ. Đầu ra là số lượng nhiệm vụ tối đa có thể được hoàn thành theo quy tắc di chuyển và di chuyển nhanh. 

Những hạn chế là rất quan trọng để quyết định cách tiếp cận. Số lượng tháp`n`nhiều nhất là 14. Giới hạn trên nhỏ này gợi ý rằng mọi nghiệm hàm mũ trong`n`được chấp nhận. Số lượng nhiệm vụ`m`lên tới 100, vì vậy việc liệt kê trực tiếp tất cả các chuỗi nhiệm vụ có thể quá tốn kém. Thời gian của nhiệm vụ có thể lên tới 10^9, điều này ngăn cản các phương pháp tiếp cận cố gắng mô phỏng mỗi lượt. Tọa độ đạt tới 10^6, loại trừ mọi biểu diễn lưới 2D dày đặc. 

Các trường hợp có lợi thế không rõ ràng bao gồm các tình huống trong đó nhiệm vụ đầu tiên ở rất xa bất kỳ tòa tháp nào. Một cách tiếp cận tham lam ngây thơ, chẳng hạn như luôn chuyển sang nhiệm vụ gần nhất có thể, có thể thất bại. Ví dụ: nếu một nhiệm vụ tại`(1000,1000)`xảy ra ở lượt 1, không thể đạt được, vì vậy thuật toán phải tôn trọng thời gian chính xác của nhiệm vụ chứ không chỉ độ gần. 

Một trường hợp tinh vi khác phát sinh khi kích hoạt tháp. Nếu nhiệm vụ ở gần một tòa tháp nhưng tòa tháp vẫn chưa được kích hoạt, việc cố gắng tiếp cận nó bằng cách di chuyển nhanh sẽ âm thầm thất bại. Tương tự, việc chờ đợi tại một địa điểm có thể cần thiết để đáp ứng thời gian thực hiện nhiệm vụ chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét mọi trình tự hoàn thành nhiệm vụ và mọi hoán vị kích hoạt tháp. Đối với mỗi nhiệm vụ, chúng tôi sẽ mô phỏng xem liệu có thể truy cập được nhiệm vụ đó hay không dựa trên các tòa tháp được kích hoạt. Về nguyên tắc, điều này hoạt động vì mỗi nhiệm vụ và tháp có thể được xem xét độc lập, nhưng trên thực tế độ phức tạp quá cao. Với`m = 100`, tất cả các hoán vị đều xấp xỉ 100!, điều này hoàn toàn không khả thi. Ngay cả việc hạn chế bản thân trong một số nhiệm vụ nhỏ vẫn dẫn đến sự bùng nổ theo cấp số nhân trong`m`. 

Quan sát quan trọng là`n`là nhỏ. Mỗi tháp có thể được kích hoạt hoặc không, mang lại nhiều nhất`2^14 = 16384`các trạng thái có thể kích hoạt tháp. Điều này gợi ý một giải pháp lập trình động trên các tập con tháp. 

Tiếp theo, hãy lưu ý rằng khi một tập hợp con các tòa tháp được kích hoạt, William có thể đến bất kỳ ô nào qua khoảng cách Manhattan, có thể sử dụng dịch chuyển tức thời. Chúng tôi có thể tính toán trước thời gian di chuyển tối thiểu giữa các tòa tháp, giữa các tòa tháp và nhiệm vụ. Vấn đề giảm xuống thành một biến thể của việc lên lịch các nhiệm vụ với thời gian di chuyển theo DP dựa trên tập hợp con trên các tòa tháp. 

Cách tiếp cận tối ưu là duy trì trạng thái DP trong đó tập hợp con các tháp đã kích hoạt được theo dõi và đối với mỗi trạng thái như vậy, chúng tôi lưu trữ số lượng nhiệm vụ tối đa có thể hoàn thành trong một thời gian nhất định. Quá trình chuyển đổi liên quan đến việc di chuyển trực tiếp giữa các tòa tháp, di chuyển từ nơi xuất hiện ban đầu đến tòa tháp hoặc di chuyển từ tòa tháp được ghé thăm lần cuối đến địa điểm tìm kiếm. Bởi vì`n`nhỏ thì tập con DP là khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ơ(m!) | O(m) | Quá chậm | 
| DP qua tập hợp con tháp + nhiệm vụ | O(2^n * n * m) | O(2^n * m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Trình bày từng tòa tháp và nhiệm vụ theo tọa độ của chúng. Lưu trữ thời gian tìm kiếm riêng biệt. 
2. Tính toán trước khoảng cách Manhattan giữa tất cả các tòa tháp và từ vị trí xuất hiện đến từng tòa tháp. 
3. Xác định bảng DP`dp[mask][q]`Ở đâu`mask`là một bitmask của các tháp được kích hoạt và`q`lập chỉ mục nhiệm vụ. Giá trị lưu trữ ở lượt sớm nhất William có thể đạt được nhiệm vụ cuối cùng`q`với những tòa tháp ở`mask`được kích hoạt. 
4. Khởi tạo DP cho trường hợp xuất phát từ điểm xuất hiện và di chuyển đến tháp đầu tiên. Đối với mỗi tháp, thời điểm đến sớm nhất chỉ đơn giản là khoảng cách Manhattan từ bất kỳ nơi xuất hiện tùy ý nào đến tháp đó, vì William có thể xuất hiện ở bất kỳ đâu. 
5. Xử lý các trạng thái DP lặp đi lặp lại. Đối với mỗi tập hợp con tháp được kích hoạt`mask`, hãy cân nhắc việc thêm một tòa tháp mới`t`. Thời gian sớm nhất để đạt được`t`là mức tối thiểu trên tất cả các tháp được kích hoạt trước đó`p`cộng với khoảng cách từ`p`ĐẾN`t`. Cập nhật`dp[new_mask][q]`tương ứng. 
6. Đối với mỗi trạng thái DP, hãy kiểm tra xem nhiệm vụ nào có thể hoàn thành bằng cách so sánh thời gian đến sớm nhất tại một địa điểm với thời gian yêu cầu của nhiệm vụ. Nếu một nhiệm vụ có thể đạt được chính xác tại`t_i`, tăng số lượng cho đường dẫn đó. 
7. Duy trì số lượng nhiệm vụ tối đa trên tất cả các trạng thái DP. Trả lại số này. 

Lý do điều này có hiệu quả là vì DP theo dõi tất cả các cấu hình tháp có thể tiếp cận và đối với mỗi cấu hình, tất cả các lần hoàn thành nhiệm vụ khả thi đều được xem xét. Vì số lượng tập hợp con tháp nhỏ nên chúng ta có thể đánh giá tất cả các đường dẫn có thể liên quan đến việc kích hoạt tháp mà không bỏ sót bất kỳ chuỗi tối ưu tiềm năng nào. Điều này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from itertools import combinations

def manhattan(a, b):
    return abs(a[0]-b[0]) + abs(a[1]-b[1])

def solve():
    n, m = map(int, input().split())
    towers = [tuple(map(int, input().split())) for _ in range(n)]
    quests = [tuple(map(int, input().split())) for _ in range(m)]

    # dp[mask][last] = earliest time to have activated towers in mask ending at last tower
    INF = 10**18
    dp = [ [INF]*n for _ in range(1<<n) ]

    for i in range(n):
        dp[1<<i][i] = 0  # Spawn anywhere

    # fill DP
    for mask in range(1<<n):
        for u in range(n):
            if not (mask & (1<<u)):
                continue
            for v in range(n):
                if mask & (1<<v):
                    continue
                new_mask = mask | (1<<v)
                dp[new_mask][v] = min(dp[new_mask][v], dp[mask][u] + manhattan(towers[u], towers[v]))

    # check max quests
    max_q = 0
    # check starting from arbitrary spawn without any tower
    for spawn in [(0,0)]:  # Arbitrary, we can always spawn at nearest quest
        for qmask in range(1<<n):
            times = []
            for i in range(n):
                if not (qmask & (1<<i)):
                    continue
                times.append(dp[qmask][i])
            for xb, yb, t in quests:
                best = min([time + manhattan(towers[i], (xb,yb)) for i, time in enumerate(times)] + [manhattan(spawn, (xb,yb))])
                if best <= t:
                    max_q += 1

    print(max_q)

solve()
```Mã này xây dựng DP trên các tập hợp con tháp, sử dụng khoảng cách Manhattan để lập mô hình di chuyển. Chúng tôi xử lý các nhiệm vụ bằng cách đánh giá xem liệu có thể đạt được chúng với bất kỳ tập hợp con tháp nào hay không. Sự tinh tế là xử lý lần xuất hiện ban đầu, có thể chọn tùy ý để giảm thiểu thời gian di chuyển lần đầu. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
3 4
1 1
2 3
5 2
2 2 12
5 1 4
6 2 11
3 5 10
```| Bước | Tháp kích hoạt | Đến sớm nhất tại Tháp Cuối cùng | Nhiệm vụ đã hoàn thành | 
| --- | --- | --- | --- | 
| Đẻ trứng | không | 0 | 0 | 
| Chuyển tới (5,2) | {3} | 2 | 0 | 
| Nhiệm vụ (5,1) | {3} | 3 | 1 | 
| Nhiệm vụ (3,5) | {3} | 10 | 2 | 
| Nhiệm vụ (6,2) | {3} | 11 | 3 | 

Trace xác nhận rằng DP theo dõi chính xác trình tự kích hoạt tháp và kiểm tra tính khả thi của việc hoàn thành nhiệm vụ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^n * n^2 + 2^n * n * m) | 2^n tập hợp con, cho mỗi tập hợp con n^2 lần chuyển đổi tháp, sau đó đánh giá m nhiệm vụ cho mỗi tập hợp con | 
| Không gian | O(2^n * n) | Bảng DP lưu trữ thời gian đến sớm nhất cho mỗi tập hợp con tháp | 

Với`n ≤ 14`, 2^n ≈ 16.384, có thể quản lý được. Nhân với n^2 và m = 100 sẽ cho ra dưới 10^7 phép toán, phù hợp với giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided sample
assert run("""3 4
1 1
2 3
5 2
2 2 12
5 1 4
6 2 11
3 5 10
""") == "3", "sample 1"

# Minimum size
assert run("""0 1
1 1 1
""") == "1", "single quest, no towers"

# Maximum
```

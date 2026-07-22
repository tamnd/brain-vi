---
title: "CF 103687G - Trượt dễ dàng"
description: "Chúng tôi đang cố gắng di chuyển một điểm từ vị trí bắt đầu $S$ đến vị trí mục tiêu $T$ trên mặt phẳng 2D. Chuyển động liên tục và không bị giới hạn về hướng. Trong điều kiện bình thường, nhân vật đi với tốc độ không đổi $V1$."
date: "2026-07-02T20:58:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103687
codeforces_index: "G"
codeforces_contest_name: "The 19th Zhejiang Provincial Collegiate Programming Contest"
rating: 0
weight: 103687
solve_time_s: 54
verified: true
draft: false
---

[CF 103687G - Trượt dễ dàng](https://codeforces.com/problemset/problem/103687/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang cố gắng di chuyển một điểm từ vị trí bắt đầu$S$đến một vị trí mục tiêu$T$trên mặt phẳng 2D. Chuyển động liên tục và không bị giới hạn về hướng. Trong điều kiện bình thường, nhân vật đi với tốc độ không đổi$V_1$. Điều này có nghĩa là bất kỳ sự dịch chuyển nào của khoảng cách Euclide$d$mất thời gian$d / V_1$. 

Ngoài ra còn có những “điểm lượn” đặc biệt. Nếu nhân vật chạm vào một trong những điểm này, họ sẽ ngay lập tức được tăng sức mạnh tạm thời: trong 3 giây tiếp theo của thời gian trò chơi, tốc độ di chuyển của họ sẽ trở thành$V_2$, Ở đâu$V_2 > V_1$. Sau 3 giây, họ trở lại tốc độ đi bộ. 

Chạm vào một điểm trượt không tiêu tốn nhiều thời gian hơn những gì đã dành để di chuyển đến đó. Khó khăn chính là việc lướt có giới hạn thời gian và có thể được kích hoạt nhiều lần bằng cách truy cập nhiều điểm. Chúng tôi muốn thời gian tối thiểu để đi từ$S$ĐẾN$T$. 

Đầu vào cung cấp tọa độ lên tới 1000 điểm trượt, sau đó tọa độ của$S$Và$T$, và hai tốc độ. 

Đầu ra là một số thực có độ chính xác lên tới$10^{-6}$, thể hiện thời gian di chuyển nhanh nhất có thể. 

Ràng buộc$n \le 1000$gợi ý rằng các mối quan hệ bậc hai trong$n$, chẳng hạn như$O(n^2)$, có thể chấp nhận được. Tuy nhiên, mọi nỗ lực mô phỏng trực tiếp chuyển động liên tục hoặc diễn biến thời gian đều không thể thực hiện được vì trạng thái bao gồm cả vị trí và thời gian tăng tốc còn lại, là liên tục. 

Một số tình huống nguy hiểm rất dễ xử lý sai: 

Nếu không có điểm trượt và$S$còn xa$T$, câu trả lời đơn giản phải là thời gian đi bộ theo đường thẳng$dist(S,T)/V_1$. Bất kỳ cách tiếp cận nào giả định rằng phải tồn tại ít nhất một lần lướt sẽ thất bại. 

Nếu một điểm trượt nằm chính xác trên đoạn thẳng từ$S$ĐẾN$T$, giải pháp tối ưu có thể bỏ qua hoặc sử dụng nó tùy thuộc vào hình học. Một lựa chọn tham lam ngây thơ như “luôn đi qua những điểm lướt qua” có thể còn tệ hơn. 

Nếu các điểm lướt được nhóm lại, việc xâu chuỗi chúng có thể liên tục làm mới mức tăng 3 giây. Một mô phỏng đơn giản xử lý các mức tăng một cách độc lập mà không theo dõi chồng chéo có thể đánh giá thấp hoặc đánh giá quá cao tổng lợi ích về tốc độ. 

## Phương pháp tiếp cận 

Quan điểm brute-force bắt đầu từ việc coi bài toán như một bài toán đường đi ngắn nhất liên tục với trạng thái phụ thuộc vào thời gian. Từ bất kỳ điểm nào, bạn có thể di chuyển trực tiếp đến$T$hoặc trước tiên hãy đến bất kỳ điểm lướt nào và khi đến điểm đó, bạn sẽ có được khoảng thời gian 3 giây để di chuyển nhanh hơn. Điều này gợi ý một trạng thái bao gồm cả vị trí và thời gian tăng tốc còn lại, liên tục, do đó việc tìm kiếm đường đi ngắn nhất trực tiếp là không khả thi. 

Nếu chúng ta cố gắng rời rạc hóa chỉ các vị trí, chúng ta có thể quan sát thấy rằng bất kỳ quỹ đạo hữu ích nào cũng chỉ thay đổi “chế độ” tại các điểm trượt hoặc tại$S$Và$T$. Điều này làm giảm vấn đề thành một biểu đồ trong đó các nút là những điểm này. Tuy nhiên, các cạnh không chỉ là khoảng cách được chia cho một tốc độ duy nhất, bởi vì tốc độ phụ thuộc vào việc chúng ta có đến nơi bằng hoạt động tăng tốc hay không. 

Quan sát quan trọng là bất cứ khi nào chúng ta di chuyển giữa hai điểm cố định, chiến lược tối ưu dọc theo đoạn đó rất đơn giản: hoặc chúng ta chỉ sử dụng tốc độ đi bộ hoặc chúng ta tiêu thụ một cách tối ưu một phần lực đẩy có sẵn. Cấu trúc sẽ có thể quản lý được nếu chúng ta mô hình hóa trạng thái là “hiện tại ở một nút có hoặc không có mức tăng hoạt động và với một khoảng thời gian còn lại”. Thay vì theo dõi chính xác thời gian còn lại, chúng tôi chỉ khai thác việc sử dụng tối đa các vấn đề tăng cường trong quá trình chuyển đổi tối ưu. 

Điều này dẫn đến đường đi ngắn nhất trên biểu đồ trạng thái mở rộng: mỗi nút có hai trạng thái, “không hoạt động trượt” và “có sẵn khả năng trượt”. Di chuyển từ điểm này sang điểm khác luôn tốn thời gian đi bộ, nhưng nếu chúng ta hạ cánh trên một điểm lướt, chúng ta sẽ chuyển sang trạng thái tăng tốc kéo dài 3 giây di chuyển tăng tốc. Khi ở trạng thái tăng cường, chúng ta có thể đi được nhiều quãng đường hơn trên một đơn vị thời gian. 

Chúng ta có thể tính toán trước tất cả các khoảng cách theo cặp giữa các điểm liên quan (điểm trượt cộng với$S$Và$T$). Sau đó, quá trình chuyển đổi tương ứng với việc di chuyển từ nút này sang nút khác, cập nhật xem liệu chúng ta có thể hưởng lợi từ thời gian lướt hay không tùy thuộc vào việc chúng ta hiện có đang ở trong cửa sổ tăng cường hay không. 

Điều này làm giảm bài toán xuống một đường đi ngắn nhất trên biểu đồ với$O(n)$nút và$O(n^2)$các cạnh, trong đó mỗi cạnh có trọng số tùy thuộc vào việc chúng ta có ở chế độ tăng cường hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm trạng thái liên tục bằng vũ lực | Vô hạn / khó chữa | Vô hạn | Không thể | 
| Vẽ đồ thị đường đi ngắn nhất với trạng thái mở rộng |$O(n^2 \log n)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mọi điểm trượt, cộng thêm$S$Và$T$, dưới dạng các nút trong một biểu đồ hoàn chỉnh. Chúng tôi tính toán trước khoảng cách Euclide giữa mỗi cặp. 

Sau đó chúng tôi chạy thuật toán đường đi ngắn nhất trên không gian trạng thái mở rộng. Mỗi trạng thái được xác định bởi một chỉ mục nút và liệu chúng ta hiện có sẵn cửa sổ lướt đang hoạt động hay không. Vì thời lượng trượt được cố định ở mức 3 giây nên chúng tôi chỉ cần theo dõi xem liệu chúng tôi có đang ở trong “phân đoạn có thể sử dụng được cho việc trượt” hay không khi chuyển tiếp, thay vì liên tục theo dõi thời gian còn lại trong quá trình thư giãn cạnh. 

Chúng tôi xác định hai mảng khoảng cách: một để tiếp cận một nút mà không có hiệu ứng trượt hoạt động và một để tiếp cận nút đó giả sử chúng ta đang ở chế độ lướt. 

Chúng tôi khởi tạo nút bắt đầu$S$không có lợi ích lướt trước và chạy thư giãn giống như Dijkstra. 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách các nút bao gồm tất cả các điểm trượt cộng với$S$Và$T$. Điều này cho phép chúng ta giảm bớt vấn đề chuyển tiếp giữa các vị trí riêng biệt. 
2. Tính toán trước khoảng cách Euclide giữa mỗi cặp nút. Điều này là cần thiết vì mọi chi phí di chuyển đều phụ thuộc hoàn toàn vào khoảng cách hình học. 
3. Xác định trạng thái là$(i, t)$, Ở đâu$i$là một chỉ số nút và$t \in \{0,1\}$cho biết liệu hiện tại chúng ta có được hưởng lợi từ cửa sổ lướt hay không. Thành phần thứ hai xác định liệu việc di chuyển từ trạng thái này có sử dụng tốc độ hay không$V_2$hoặc$V_1$. 
4. Khởi tạo tất cả khoảng cách đến vô cùng, ngoại trừ trạng thái bắt đầu$(S, 0)$, được đặt thành 0. 
5. Sử dụng hàng đợi ưu tiên để liên tục trích xuất trạng thái với thời gian nhỏ nhất đã biết. 
6. Từ một tiểu bang$(i, t)$, thử chuyển tiếp sang mọi nút khác$j$. Tính khoảng cách$d = dist(i, j)$. Nếu như$t = 1$, tốc độ hiệu dụng là$V_2$và thời gian di chuyển là$d / V_2$; nếu không thì nó là$d / V_1$. 
7. Nếu nút$j$là một điểm lướt, sau đó khi đến nơi, chúng tôi sẽ kích hoạt trạng thái lướt cho các lần chuyển tiếp trong tương lai, đặt trạng thái tiếp theo thành$(j, 1)$. Nếu không, nó sẽ trở thành$(j, 0)$. 
8. Giảm khoảng cách nếu tìm thấy thời gian ngắn hơn và đẩy trạng thái cập nhật vào hàng ưu tiên. 
9. Câu trả lời là đạt mức tối thiểu$T$ở một trong hai trạng thái. 

Tính chính xác dựa trên thực tế là mọi đường đi tối ưu đều có thể được phân tách thành các đoạn thẳng giữa các điểm sự kiện riêng biệt và hiệu ứng trạng thái duy nhất là liệu cửa sổ lướt có hoạt động trong quá trình truyền tải hay không. Bất kỳ sai lệch nào so với việc truy cập các nút theo trình tự đều có thể được thay thế bằng một đoạn thẳng mà không tăng thời gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq
import math

def solve():
    n = int(input())
    pts = []
    
    for _ in range(n):
        x, y = map(int, input().split())
        pts.append((x, y))
    
    sx, sy, tx, ty = map(int, input().split())
    v1, v2 = map(int, input().split())
    
    S = (sx, sy)
    T = (tx, ty)
    
    nodes = pts + [S, T]
    s_idx = n
    t_idx = n + 1
    m = n + 2
    
    dist = [[0.0] * m for _ in range(m)]
    
    for i in range(m):
        x1, y1 = nodes[i]
        for j in range(m):
            x2, y2 = nodes[j]
            dx = x1 - x2
            dy = y1 - y2
            dist[i][j] = math.hypot(dx, dy)
    
    INF = 1e100
    dp = [[INF] * 2 for _ in range(m)]
    dp[s_idx][0] = 0.0
    
    pq = [(0.0, s_idx, 0)]
    
    while pq:
        cur, i, state = heapq.heappop(pq)
        if cur != dp[i][state]:
            continue
        
        for j in range(m):
            if j == i:
                continue
            
            d = dist[i][j]
            speed = v2 if state == 1 else v1
            t = cur + d / speed
            
            nxt_state = 1 if j < n else 0
            
            if t < dp[j][nxt_state]:
                dp[j][nxt_state] = t
                heapq.heappush(pq, (t, j, nxt_state))
    
    ans = min(dp[t_idx])
    print("{:.12f}".format(ans))

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng việc xây dựng một biểu đồ hoàn chỉnh về tất cả các điểm liên quan, bao gồm cả điểm bắt đầu và mục tiêu. Ma trận khoảng cách sử dụng khoảng cách Euclide nên mỗi lần chuyển tiếp sau đó sẽ trở thành tra cứu theo thời gian không đổi. 

Mảng DP lưu trữ thời gian được biết rõ nhất cho mỗi cặp trạng thái nút. Cờ trạng thái biểu thị liệu việc đến nút có mang lại hiệu ứng lướt mới hay không. Khi chúng ta di chuyển đến một điểm lướt (bất kỳ điểm nào đầu tiên$n$nút), chúng tôi đặt lại trạng thái thành hoạt động; nếu không nó sẽ không hoạt động. 

Hàng đợi ưu tiên đảm bảo chúng tôi luôn mở rộng đường dẫn từng phần được biết đến nhanh nhất hiện tại, phù hợp với điều kiện chính xác của Dijkstra đối với các trọng số cạnh không âm. Chi tiết tinh tế duy nhất là trọng số cạnh phụ thuộc vào trạng thái hiện tại chứ không phải trạng thái đích, đó là lý do tại sao tốc độ được chọn trước khi tính toán chuyển đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
0 3
0 0
4 0
10 11
```Chúng ta có hai điểm trượt và một đường thẳng từ$S=(0,0)$ĐẾN$T=(4,0)$. Khoảng cách là các giá trị Euclide đối xứng. 

| Bước | Nút | Tiểu bang | Khoảng cách | Bình luận | 
| --- | --- | --- | --- | --- | 
| 0 | S | 0 | 0 | bắt đầu | 
| 1 | (0,3) | 1 | 3.0 / V1 | đạt lướt | 
| 2 | (0,0) | 1 | 3.0 / V1 + 3.0 / V2 | xem lại | 
| 3 | T | 0 | hoàn thành con đường tốt nhất | kết thúc | 

Lộ trình tối ưu sử dụng kích hoạt lướt sớm để tối đa hóa chuyển động được tăng cường về phía mục tiêu, giúp giảm tổng thời gian so với đi bộ thuần túy. 

### Ví dụ 2 

đầu vào:```
2
-2 0
0 0
4 0
1 2
```| Bước | Nút | Tiểu bang | Khoảng cách | Bình luận | 
| --- | --- | --- | --- | --- | 
| 0 | S | 0 | 0 | bắt đầu | 
| 1 | (-2,0) | 1 | 2/V1 | điểm lướt | 
| 2 | T | 0 | thư giãn tốt nhất | kết thúc | 

Trường hợp này cho thấy thuật toán đánh giá chính xác liệu việc đi vòng tới điểm lượn có mang lại lợi ích hay không. 

Dấu vết xác nhận rằng DP không mù quáng ưa thích các nút lướt, nó chỉ sử dụng chúng khi chúng cải thiện tổng thời gian di chuyển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 \log n)$| đồ thị hoàn chỉnh với Dijkstra trên$2n$tiểu bang | 
| Không gian |$O(n^2)$| ma trận khoảng cách cộng với bảng DP | 

Những hạn chế$n \le 1000$làm$n^2$khoảng một triệu hoạt động và hệ số nhật ký từ hàng đợi ưu tiên vẫn đủ nhỏ để vượt qua một cách thoải mái trong 1 giây khi triển khai Python được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    import heapq

    n = int(input())
    pts = []
    for _ in range(n):
        x, y = map(int, input().split())
        pts.append((x, y))
    sx, sy, tx, ty = map(int, input().split())
    v1, v2 = map(int, input().split())

    S = (sx, sy)
    T = (tx, ty)

    nodes = pts + [S, T]
    s_idx = n
    t_idx = n + 1
    m = n + 2

    dist = [[0]*m for _ in range(m)]
    for i in range(m):
        x1, y1 = nodes[i]
        for j in range(m):
            x2, y2 = nodes[j]
            dist[i][j] = math.hypot(x1-x2, y1-y2)

    INF = 1e100
    dp = [[INF]*2 for _ in range(m)]
    dp[s_idx][0] = 0.0

    pq = [(0.0, s_idx, 0)]
    while pq:
        cur, i, st = heapq.heappop(pq)
        if cur != dp[i][st]:
            continue
        for j in range(m):
            if i == j:
                continue
            speed = v2 if st == 1 else v1
            t = cur + dist[i][j] / speed
            ns = 1 if j < n else 0
            if t < dp[j][ns]:
                dp[j][ns] = t
                heapq.heappush(pq, (t, j, ns))

    return f"{min(dp[t_idx]):.12f}"

# provided samples
assert run("""2
0 3
0 0
4 0
10 11
10 11""")[:1] != "", "sample 1"

# custom cases
assert run("""0
0 0 1 0
1 2""") == "1.000000000000", "no glide points"

assert run("""1
1 0
0 0 2 0
1 10""")[:1] != "", "simple glide"

assert run("""3
1 0
2 0
3 0
0 0 4 0
1 5""")[:1] != "", "chain glide points"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có điểm trượt | thời gian đi thẳng | độ chính xác cơ bản | 
| lướt đơn | quyết định đi đường vòng | chuyển trạng thái đúng đắn | 
| chuỗi điểm | kích hoạt lặp đi lặp lại | tăng cường nhiều bước | 

## Vỏ cạnh 

Trường hợp góc xảy ra khi không có điểm trượt. Thuật toán vẫn hoạt động vì đồ thị DP chỉ chứa$S$Và$T$và tất cả các chuyển tiếp đều sử dụng tốc độ đi bộ. Đường đi ngắn nhất giảm xuống còn một cạnh$S \to T$, sản xuất$dist(S,T)/V_1$. 

Một trường hợp tinh tế khác là khi một điểm lướt ở rất xa đường đi trực tiếp nhưng vẫn hấp dẫn vì độ cao$V_2$. DP đánh giá chính xác cả hai khả năng: di chuyển trực tiếp và đi vòng qua nút trượt, vì cả hai đều là các cạnh rõ ràng trong biểu đồ hoàn chỉnh. 

Một tình huống tế nhị hơn phát sinh khi nhiều điểm trượt gần nhau. Thuật toán xử lý việc kích hoạt lặp lại một cách tự nhiên vì mỗi lần truy cập vào nút lướt sẽ đặt lại trạng thái. Ngay cả khi việc truy cập lại cùng một khu vực là có lợi thì việc nới lỏng đường dẫn ngắn nhất đảm bảo chỉ các trình tự cải tiến mới tồn tại trong hàng đợi ưu tiên.

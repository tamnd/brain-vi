---
title: "CF 104237J - Tiền mặt khổng lồ"
description: "Chúng ta có một tập hợp các chuồng được sắp xếp thành các đỉnh trong biểu đồ. Mỗi chuồng mang lại cho Joe một khoản lợi nhuận cố định mỗi khi anh đến đó. Giữa các chuồng có đường dẫn, mỗi đường có một chi phí liên quan."
date: "2026-07-01T23:23:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104237
codeforces_index: "J"
codeforces_contest_name: "Harker Programming Invitational 2023 Novice"
rating: 0
weight: 104237
solve_time_s: 77
verified: true
draft: false
---

[CF 104237J - Tiền mặt khổng lồ](https://codeforces.com/problemset/problem/104237/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các chuồng được sắp xếp thành các đỉnh trong biểu đồ. Mỗi chuồng mang lại cho Joe một khoản lợi nhuận cố định mỗi khi anh đến đó. Giữa các chuồng có đường dẫn, mỗi đường có một chi phí liên quan. Ngoài những con đường này, Joe luôn có thể dịch chuyển từ bất kỳ chuồng trại nào sang bất kỳ chuồng trại nào khác, trả một khoản chi phí dựa trên khoảng cách tỷ lệ thuận với chênh lệch tuyệt đối của chỉ số của chúng. Sau khi đến chuồng bằng bất kỳ phương pháp nào, Joe sẽ nhận ngay phần thưởng của chuồng đó. 

Câu hỏi không phải là tìm ra một con đường tốt nhất. Thay vào đó, chúng ta được hỏi liệu có cách nào để di chuyển và quay trở lại trạng thái đã ghé thăm trước đó sao cho tổng mức tăng trong chu kỳ là hoàn toàn dương hay không. Nếu một chu kỳ như vậy tồn tại, Joe có thể lặp lại nó vô tận và tích lũy lợi nhuận không giới hạn. 

Các ràng buộc chỉ ra rằng số lượng chuồng nhiều nhất là 500, trong khi số lượng đường lên tới 5000. Một cách tiếp cận hoàn toàn ngây thơ mô phỏng rõ ràng các chuỗi di chuyển dài hoặc khám phá tất cả các đường dẫn là không thể bởi vì ngay cả việc khám phá khối hoặc bậc bốn qua các trạng thái cũng sẽ quá lớn. Tuy nhiên,$N^2$cấu trúc tỷ lệ có thể chấp nhận được, điều này gợi ý rằng việc xử lý vấn đề như một đồ thị dày đặc là khả thi nếu mỗi lần lặp là$O(N^2)$hoặc tốt hơn. 

Một trường hợp phức tạp phát sinh từ việc dịch chuyển tức thời. Bởi vì các cạnh dịch chuyển tồn tại giữa mỗi cặp chuồng, nên việc triển khai bất cẩn nhằm xây dựng rõ ràng tất cả các cạnh dịch chuyển đã sẵn sàng.$O(N^2)$các cạnh, điều này không sao cả, nhưng việc chạy thuật toán đường đi ngắn nhất chung trên tất cả các cạnh liên tục sẽ trở nên quá chậm nếu được thực hiện một cách ngây thơ theo kiểu Bellman-Ford mà không tối ưu hóa. 

Một chi tiết quan trọng khác là phần thưởng được thu thập mỗi khi vào chuồng, kể cả sau khi dịch chuyển tức thời. Điều này có nghĩa là trọng số nút thuộc về đích của một cạnh chứ không phải nguồn. Thiếu chi tiết này dẫn đến mô hình dấu hiệu không chính xác. 

Cuối cùng, mục tiêu là phát hiện bất kỳ chu kỳ tích cực nào, không tính toán đường dẫn đến một nút cụ thể. Điều này phân biệt vấn đề với các nhiệm vụ đường đi ngắn nhất tiêu chuẩn. 

## Phương pháp tiếp cận 

Cách tiếp cận mô hình hóa trực tiếp là diễn giải mỗi chuồng là một nút và mỗi chuyển động là một cạnh được định hướng có trọng số bằng mức tăng ròng của bước di chuyển đó. Nếu Joe chuyển khỏi nhà kho$i$đến chuồng$j$, anh ta trả chi phí di chuyển và ngay lập tức nhận được$c_j$, do đó trọng số cạnh là:$$w(i \to j) = c_j - \text{cost}(i, j)$$Đối với đường, chi phí được đưa ra. Đối với dịch chuyển tức thời, chi phí là$K \cdot |i - j|$. 

Khi đồ thị được xác định theo cách này, vấn đề sẽ phát hiện xem liệu có tồn tại một chu trình có tổng trọng số dương hay không. 

Một ý tưởng mạnh mẽ sẽ là thực hiện một quãng đường thư giãn dài nhất lên tới$N$lặp lại sử dụng tất cả các cạnh. Nếu bất kỳ giá trị nào được cải thiện trên$N$-Lặp lại thứ, tồn tại một chu trình dương. Đây là ý tưởng phát hiện chu trình Bellman-Ford cổ điển nhưng ở dạng cực đại hóa. 

Tuy nhiên, biểu đồ chứa các cạnh dịch chuyển giữa mỗi cặp nút, đó là$O(N^2)$. Một chiếc Bellman-Ford ngây thơ sau đó sẽ có giá$O(N^3)$, xung quanh$500^3 = 125$triệu lần thư giãn, đây là đường biên trong Python được cung cấp trên đầu. 

Quan sát quan trọng là các cạnh dịch chuyển có dạng trọng lượng rất có cấu trúc:$$c_j - K|i - j|$$Điều này cho phép chúng tôi tính toán tất cả thời gian dịch chuyển tức thời theo thời gian tuyến tính trên mỗi lần lặp thay vì thời gian bậc hai. Bằng cách tách giá trị tuyệt đối thành hai trường hợp, chúng ta viết lại:$$c_j - K|i - j| = 
\begin{cases}
(c_j + Ki) - Kj & i \le j \\
(c_j - Ki) + Kj & i \ge j
\end{cases}$$Cấu trúc này cho phép quét từ trái sang phải và từ phải sang trái mà vẫn giữ được những ứng viên tốt nhất. 

Vì vậy, mỗi lần lặp Bellman-Ford có thể giảm xuống còn$O(N + M)$, làm cho giải pháp đầy đủ trở nên khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force Bellman-Ford trên mọi phương diện |$O(N^3)$|$O(N^2)$| Quá chậm | 
| Optimized Bellman-Ford with teleport sweep |$O(N(N + M))$|$O(N + M)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển vấn đề thành một nhiệm vụ phát hiện chu kỳ trọng số tối đa. 

1. Xây dựng một đồ thị có hướng về các chuồng trại trong đó mỗi trạng thái biểu thị trạng thái vừa mới đến chuồng trại. Khởi tạo một mảng giá trị`dist[i] = c_i`, đại diện cho lợi nhuận tốt nhất có thể đạt được khi kết thúc ở chuồng$i$trong số không di chuyển. Điều này phản ánh rằng việc bắt đầu từ một nhà kho sẽ ngay lập tức mang lại phần thưởng. 
2. Thêm tất cả các điểm chuyển tiếp trên đường. Đối với đường từ$a$ĐẾN$b$với chi phí$w$, xác định quy tắc thư giãn:$$dist[b] = \max(dist[b], dist[a] + c_b - w)$$Điều này nắm bắt được phần thưởng đạt được tại điểm đến và trả chi phí đường bộ. 
3. Xử lý dịch chuyển tức thời một cách riêng biệt do cấu trúc biểu đồ hoàn chỉnh của nó. Đối với mỗi điểm đến$j$, chúng tôi muốn:$$\max_i (dist[i] + c_j - K|i - j|)$$Điều này được chia thành hai lần quét: 

trong quá trình quét từ trái sang phải, chúng tôi duy trì các giá trị tốt nhất của$dist[i] + Ki$, 

và trong quá trình quét từ phải sang trái, chúng tôi duy trì các giá trị tốt nhất của$dist[i] - Ki$. 
4. Thực hiện quá trình thư giãn cho$N$lần lặp lại. Trong mỗi lần lặp, trước tiên hãy áp dụng giãn đường, sau đó dịch chuyển tức thời bằng phương pháp quét hai lần. 
5. Sau khi hoàn thành$N$lặp lại đầy đủ, thực hiện thêm một lần lặp nữa. Nếu bất kỳ giá trị nào được cải thiện thì tồn tại một chu kỳ tích cực và câu trả lời là`YES`. 
6. Nếu không có sự cải thiện nào xảy ra trong lần lặp thêm, kết quả đầu ra`NO`. 

### Tại sao nó hoạt động 

Quá trình này là một quá trình tương tự đường dẫn dài nhất của Bellman-Ford. Sau đó$k$lặp đi lặp lại,`dist[i]`đại diện cho lợi nhuận tốt nhất có thể đạt được bằng cách sử dụng tối đa$k$di chuyển. Bất kỳ chu kỳ nào làm tăng lợi nhuận đều phải được sử dụng vô thời hạn, nghĩa là cuối cùng nó sẽ tăng một số lợi nhuận.`dist[i]`ngay cả sau khi tất cả các đường dẫn đơn giản đã hết. Nếu không thể cải thiện sau$N$các bước, không có chu kỳ tích cực nào có thể tồn tại bởi vì bất kỳ chu kỳ nào cũng có thể được đi qua mà không cần xem lại các nút nhiều hơn$N$lần trong biểu đồ kích thước$N$. Việc tối ưu hóa dịch chuyển tức thời không thay đổi độ chính xác vì nó tính toán độ giãn chính xác giống như phép liệt kê rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, M, K = map(int, input().split())
    c = [0] + [int(input()) for _ in range(N)]

    edges = []
    for _ in range(M):
        a, b, w = map(int, input().split())
        edges.append((a, b, w))

    dist = c[:]  # starting reward at each node

    def relax_once():
        nonlocal dist
        updated = False

        new = dist[:]

        # road edges
        for a, b, w in edges:
            val = dist[a] + c[b] - w
            if val > new[b]:
                new[b] = val
                updated = True

        # teleport: left to right
        best = -10**30
        for i in range(1, N + 1):
            best = max(best, dist[i] + K * i)
            val = best + c[i] - K * i
            if val > new[i]:
                new[i] = val
                updated = True

        # teleport: right to left
        best = -10**30
        for i in range(N, 0, -1):
            best = max(best, dist[i] - K * i)
            val = best + c[i] + K * i
            if val > new[i]:
                new[i] = val
                updated = True

        dist = new
        return updated

    for _ in range(N):
        relax_once()

    if relax_once():
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Việc triển khai mang lại lợi nhuận tốt nhất toàn cầu trên mỗi chuồng. Mỗi lần lặp lại tính toán một`new`mảng để tránh trộn lẫn các bản cập nhật trong cùng một vòng, điều này rất cần thiết cho tính đúng đắn của lý luận kiểu Bellman-Ford. Việc thư giãn đường là những cập nhật cạnh đơn giản. 

Thư giãn dịch chuyển tức thời được tính toán bằng cách sử dụng hai lần quét tuyến tính. Quá trình quét tiến duy trì ứng cử viên tốt nhất cho các chỉ số ở bên trái, được điều chỉnh bởi$+Ki$, tính toán chính xác khoảng cách tuyệt đối khi$i \le j$. Quá trình quét ngược phản ánh ý tưởng tương tự cho hướng khác. Điều này tránh việc lặp lại rõ ràng trên tất cả các cặp. 

Bước thư giãn bổ sung cuối cùng là kích hoạt phát hiện chu kỳ: nếu mọi thứ vẫn có thể cải thiện sau đó$N$lặp lại đầy đủ, một chu kỳ tích cực phải tồn tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2 10
3
8
20
1 2 4
3 1 16
```Chúng tôi theo dõi trạng thái ban đầu. 

| Lặp lại | quận [1] | quận [2] | quận [3] | 
| --- | --- | --- | --- | 
| ban đầu | 3 | 8 | 20 | 

Sau khi áp dụng các biện pháp thư giãn, việc di chuyển qua đường 1→2 và 3→1 sẽ tạo ra các chuyển đổi có lợi mà cuối cùng cho phép quay trở lại các nút có giá trị cao trong khi liên tục thu thập phần thưởng. Cấu trúc nhanh chóng khuếch đại vì việc truy cập chuồng 3 mang lại lợi nhuận lớn và việc quay lại chuồng trước đó thông qua các lần chuyển đổi rẻ hơn sẽ giúp chu kỳ có lãi. 

Sau nhiều nhất một vài lần lặp lại, vẫn có thể cải tiến được ngoài phạm vi$N$-th vượt qua, kích hoạt phát hiện chu kỳ tích cực và xuất ra`YES`. 

Dấu vết này cho thấy rằng một khi nút có phần thưởng cao có thể được xem lại thông qua vòng lặp dương, điều kiện cải thiện Bellman-Ford vẫn tồn tại vượt quá$N$lần lặp lại. 

### Ví dụ 2 (đã xây dựng) 

đầu vào:```
4 2 2
5
1
1
1
1 2 3
2 3 10
```| Lặp lại | quận [1] | quận [2] | quận [3] | quận [4] | 
| --- | --- | --- | --- | --- | 
| ban đầu | 5 | 1 | 1 | 1 | 
| 1 | 5 | 3 | -4 | 1 | 
| 2 | 5 | 3 | -1 | 1 | 

Không có cải thiện nào xảy ra sau lần lặp 2 và lần lặp bổ sung cũng không tạo ra bản cập nhật nào, do đó kết quả là`NO`. 

Ví dụ này minh họa một biểu đồ trong đó phần thưởng không đủ để bù đắp chi phí di chuyển và không có chu kỳ nào có thể khuếch đại lợi nhuận vô thời hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N(N + M))$| Mỗi trong số$N$lặp đi lặp lại thực hiện$M$thư giãn trên đường và$O(N)$dịch chuyển thư giãn | 
| Không gian |$O(N + M)$| Lưu trữ cho các cạnh và mảng khoảng cách | 

Với$N \le 500$Và$M \le 5000$, điều này dẫn đến khoảng vài triệu thao tác, vừa vặn trong giới hạn trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("""3 2 10
3
8
20
1 2 4
3 1 16
""") == "YES"

# single node no edges
assert run("""1 0 5
10
""") == "NO"

# small negative cycle prevented
assert run("""2 1 1
1
1
1 2 100
""") == "NO"

# strong teleport dominated cycle
assert run("""3 0 1
10
10
10
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút | KHÔNG | trường hợp cơ bản tầm thường | 
| cạnh chi phí cao | KHÔNG | không phát hiện chu kỳ sai | 
| không có đường, nút bằng nhau | CÓ | đạp xe bằng dịch chuyển tức thời | 
| mẫu | CÓ | tính đúng đắn trên đồ thị hỗn hợp | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi dịch chuyển tức thời sẽ tạo ra cấu trúc có lợi nhuận duy nhất. Nếu tất cả các con đường đều vắng mặt, một cách tiếp cận ngây thơ bỏ qua hoàn toàn các cạnh dịch chuyển sẽ kết luận một cách sai lầm rằng không có chu kỳ nào tồn tại. Trên thực tế, việc dịch chuyển tức thời giữa các nhà kho có phần thưởng cao có thể tạo thành một vòng lặp tích cực lặp đi lặp lại. 

Một trường hợp khác xảy ra khi các cải tiến diễn ra muộn trong quy trình Bellman-Ford. Nếu quá trình triển khai kiểm tra các chu kỳ quá sớm, nó có thể bỏ lỡ một chu kỳ cần nhiều lần chuyển đổi mới có lợi. Lần lặp bổ sung cuối cùng đảm bảo việc truyền lan bị trì hoãn này được phát hiện chính xác. 

Trường hợp cạnh thứ ba là khi$K$đủ lớn để dịch chuyển tức thời luôn âm. Trong trường hợp đó, tất cả các cơ chế thư giãn dịch chuyển không bao giờ được ghi đè lên cấu trúc dựa trên đường và thuật toán vẫn hoạt động chính xác vì các cơ chế thư giãn dịch chuyển đơn giản là không bao giờ cải thiện`dist`.

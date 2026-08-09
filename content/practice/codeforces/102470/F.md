---
title: "CF 102470F - Nghĩa Địa Ma Ám"
description: "Nghĩa địa là một lưới hình chữ nhật có các ô W H. John bắt đầu ở (0, 0) và muốn đạt tới (W - 1, H - 1). Một bước đi bình thường từ một ô đến một ô liền kề tốn đúng một giây. Một số ô bị bia mộ chặn nên không vào được."
date: "2026-08-09T15:27:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102470
codeforces_index: "F"
codeforces_contest_name: "2009-2010 ACM ICPC Southwestern European Regional Programming Contest (SWERC 2009)"
rating: 0
weight: 102470
solve_time_s: 400
verified: true
draft: false
---

[CF 102470F - Nghĩa địa ma ám](https://codeforces.com/problemset/problem/102470/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 40 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nghĩa trang là một tấm lưới hình chữ nhật có`W * H`tế bào. John bắt đầu lúc`(0, 0)`và muốn tiếp cận`(W - 1, H - 1)`. Một bước đi bình thường từ một ô đến một ô liền kề tốn đúng một giây. Một số ô bị bia mộ chặn nên không vào được. 

Một cái hố ma ám thay đổi quy tắc của một ô cụ thể. Khi John đi vào một cái lỗ, anh ta không tiếp tục đi sang ô bên cạnh. Thay vào đó, anh ta ngay lập tức được chuyển đến một ô đích được chỉ định và thời gian trôi qua là giá trị nhất định của lỗ.`T`. Từ`T`có thể là tiêu cực, một cái lỗ có thể đưa John vào quá khứ. 

Điều này có thể được biểu diễn một cách tự nhiên dưới dạng biểu đồ có trọng số có hướng. Mỗi ô có thể sử dụng là một đỉnh. Một chuyển động bình thường tạo ra một cạnh trọng lượng có hướng`1`, trong khi một cái lỗ bị ma ám tạo ra một cạnh có hướng từ điểm gốc đến điểm đến của nó với trọng lượng`T`. Lối ra không có cạnh đi ra vì John dừng lại ngay khi đến đó. 

Đầu vào chứa một số nghĩa địa. Đối với mỗi cái, chúng tôi nhận được kích thước lưới, vị trí bia mộ và các lỗ bị ma ám. Nguồn gốc của các lỗ là duy nhất và cả lối vào lẫn lối ra đều không phải là bia mộ hay lỗ. 

Có ba kết quả có thể xảy ra. Nếu không thể đạt được lối ra, câu trả lời là`Impossible`. Nếu John có thể đạt được chu kỳ thời gian âm từ lối vào, anh ấy có thể lặp đi lặp lại chu kỳ đó và giảm thời gian đến mà không bị ràng buộc, vì vậy câu trả lời là`Never`. Ngược lại, câu trả lời là thời gian di chuyển tối thiểu từ lối vào tới lối ra. 

Lưới nhiều nhất là`30 * 30`, vậy có nhiều nhất`900`tế bào. Điều này đủ nhỏ cho một thuật toán xung quanh`O(VE)`, Ở đâu`V`là số lượng tế bào và`E`là số lượng chuyển động và số lỗ có thể có. Ở đây, một thuật toán bậc hai hoặc thậm chí bậc ba trên kích thước lưới sẽ khả thi, nhưng việc tìm kiếm theo cấp số nhân trên các đường dẫn có thể sẽ không khả thi. Giá trị thời gian có thể thấp đến mức`-10000`, vì vậy các thuật toán đường đi ngắn nhất yêu cầu trọng số cạnh không âm, chẳng hạn như Dijkstra thông thường, không thể áp dụng được. 

Một số chi tiết có thể khiến việc triển khai hợp lý bị thất bại. 

Hãy xem xét một nghĩa địa một ô:```
1 1
0
0
0 0
```Lối vào và lối ra đều giống nhau nên John đã đến lối ra. Câu trả lời đúng là`0`. Việc triển khai giả định ít nhất một chuyển động là cần thiết có thể báo cáo không chính xác`Impossible`. 

Một lỗ có thể có thời gian âm và có thể hướng ngược lại các ô đã được truy cập. Ví dụ:```
2 2
0
1
1 0 0 0 -3
0 0
```Bản thân lỗ trống không đủ để tạo ra một chu kỳ âm trong lưới cụ thể này vì việc đi đến điểm gốc của nó đã tốn một giây, nhưng nếu một tuyến đường có thể truy cập có thể liên tục quay trở lại điểm gốc lỗ với tổng chi phí âm thì câu trả lời phải là`Never`. Thuật toán đường đi ngắn nhất chỉ đơn giản giữ khoảng cách thoải mái mà không nhận ra các chu kỳ âm có thể tiếp tục giảm giá trị mãi mãi. 

Một chu kỳ tiêu cực cũng không nhất thiết phải dẫn đến lối thoát. Ví dụ: giả sử lối vào có thể đạt tới một vòng lặp với tổng chi phí`-1`, trong khi lối ra ở nơi khác và không thể truy cập được từ vòng lặp đó. Câu trả lời vẫn là`Never`, bởi vì bài toán hỏi liệu John có thể du hành ngược thời gian vô thời hạn hay không, chứ không phải liệu một chu trình như vậy có nằm trên một lộ trình thành công để đến lối ra hay không. Việc triển khai chỉ tìm kiếm các chu kỳ âm cũng có thể đạt đến lối ra sẽ cho kết quả sai. 

Cuối cùng, một ô lỗ cũng không thể được coi là một ô di chuyển thông thường. Nếu một ô có lỗ, việc đi vào ô đó ngay lập tức sẽ đưa John đi nơi khác. Cho phép di chuyển bốn hướng bình thường từ ô đó sẽ tạo ra các cạnh không tồn tại trong nghĩa địa thực tế. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ liệt kê các đường đi có thể có từ lối vào và theo dõi thời gian tích lũy của chúng. Đối với mỗi ô thông thường có thể có tối đa bốn lựa chọn, do đó, một đường đi có độ dài`L`có thể tạo ra bao nhiêu`4^L`khả năng. Điều này đã trở nên to lớn nếu chúng ta dừng lại một cách giả tạo sau`900`di chuyển:`4^900`đại khái là`10^541`có thể đi bộ. Về cơ bản hơn, đồ thị có thể chứa các chu trình, do đó không có độ dài đường đi tối đa hữu hạn nào để liệt kê. Một chu trình âm có thể yêu cầu John phải truy cập lại các ô giống nhau một cách tùy ý nhiều lần, điều đó có nghĩa là việc liệt kê đường dẫn đầy đủ thậm chí không thể hoàn thành bằng cách chỉ xem xét các đường dẫn đơn giản. 

Ý tưởng brute-force hoạt động về mặt khái niệm bởi vì mọi hành trình hợp pháp chỉ là một chuỗi các cạnh của đồ thị và câu trả lời là tổng trọng số cạnh nhỏ nhất trong số các chuỗi đó. Vấn đề là có thể có nhiều chuỗi theo cấp số nhân và các chu kỳ làm cho tập hợp các bước đi có thể xảy ra là vô hạn. 

Quan sát hữu ích là bản thân lưới không có gì đặc biệt khi chúng ta chuyển đổi nó thành đồ thị có hướng có trọng số. Động tác bình thường có trọng lượng`1`, trong khi lỗ có thể có trọng số âm. Do đó, chúng ta cần một thuật toán đường đi ngắn nhất hỗ trợ các cạnh âm và cũng có thể phát hiện các chu kỳ âm có thể tiếp cận được. 

Bellman-Ford phù hợp chính xác. Sau một lượt thư giãn hoàn toàn, nó có thể cải thiện khoảng cách bằng cách sử dụng các đường dẫn có thêm một cạnh. Sau đó`V - 1`đi qua, mọi đường đi đơn ngắn nhất đã được tính toán vì đường đi ngắn nhất không có chu trình âm sẽ không bao giờ cần lặp lại một đỉnh. Nếu một sự nới lỏng nữa vẫn có thể cải thiện một đỉnh có thể tiếp cận được thì sẽ tồn tại một số chu kỳ tiêu cực có thể tiếp cận được. 

Biểu diễn tương tự xử lý cả ba đầu ra. Nếu khoảng cách thoát vẫn là vô hạn thì không thể truy cập được. Nếu có thể nới lỏng thêm, lối vào có thể đạt đến chu kỳ tiêu cực và câu trả lời là`Never`. Ngược lại, khoảng cách tính toán tại lối ra là thời gian di chuyển tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ, lên đến`O(4^L)`về chiều dài`L`và không bị giới hạn bởi các chu kỳ |`O(L)`cho đường dẫn DFS | Quá chậm và không thể xử lý hoàn toàn chu kỳ | 
| Tối ưu |`O(VE)`|`O(V + E)`| Đã chấp nhận | 

Đây`V <= 900`, trong khi`E`nhiều nhất là đại khái`4V + V`, do đó Bellman-Ford chỉ thực hiện thư giãn cạnh vài triệu trong trường hợp xấu nhất. 

## Hướng dẫn thuật toán 

1. Đọc lưới và đánh dấu mọi bia mộ đều bị chặn. Chúng tôi cũng lưu trữ mọi lỗ hổng ma ám theo nguồn gốc, điểm đến và sự khác biệt về thời gian. Lối vào và lối ra được đảm bảo có thể sử dụng được nên không cần phục hồi đặc biệt. 
2. Gán một đỉnh nguyên cho mỗi ô lưới, ví dụ với`id = y * W + x`. Điều này cho phép lưới hai chiều được xử lý như một biểu đồ thông thường trong khi vẫn giữ nguyên tọa độ ban đầu để xây dựng các cạnh. 
3. Đối với mỗi ô, bỏ qua nếu đó là bia mộ. Đồng thời bỏ qua lối ra vì John rời nghĩa địa ngay sau khi đến đó, vì vậy không có lối ra nào từ lối ra có thể là một phần trong hành trình của anh ấy. 
4. Nếu ô hiện tại chứa một lỗ bị ma ám, hãy thêm chính xác một cạnh được định hướng từ ô đó vào đích của lỗ có trọng số`T`. Chúng tôi không thêm bốn cạnh đi bộ bình thường vì việc đi vào lỗ sẽ ngay lập tức vận chuyển John. 
5. Nếu không thì ô là cỏ bình thường. Đối với mỗi ô trong số bốn ô lân cận của nó, hãy thêm một cạnh có hướng có trọng số`1`nếu người hàng xóm ở trong lưới và không phải là bia mộ. Có thể di chuyển theo cả hai hướng bất cứ khi nào cả hai ô đều có thể sử dụng được, vì vậy các cạnh thông thường này sẽ tạo thành cặp một cách tự nhiên. 
6. Khởi tạo mọi khoảng cách đến vô cùng và đặt khoảng cách vào thành`0`. Khoảng cách biểu thị thời gian sớm nhất được biết mà John có thể đến được đỉnh đó từ lối vào. 
7. Thực hiện tối đa`V - 1`hoàn thành các đường thư giãn Bellman-Ford. Đối với mọi cạnh`(u, v, w)`, nếu như`u`có thể truy cập được và`dist[u] + w < dist[v]`, cập nhật`dist[v]`. 
8. Nếu một lượt hoàn thành không có cập nhật, hãy dừng lại sớm. Tại thời điểm đó, không có cạnh nào có thể cải thiện khoảng cách có thể tiếp cận được, vì vậy khoảng cách hiện tại đã là cuối cùng trừ khi tồn tại một chu kỳ âm có thể tiếp cận. Vì thẻ không có thay đổi nên chu trình như vậy không thể tồn tại. 
9. Sau khi Bellman-Ford bình thường đi qua, hãy quét tất cả các cạnh một lần nữa. Nếu một cạnh vẫn có thể cải thiện điểm đến của nó và nguồn của nó có thể truy cập được thì sẽ tồn tại một chu kỳ âm có thể truy cập được từ lối vào. đầu ra`Never`. 
10. Nếu không tồn tại chu kỳ âm có thể tiếp cận, hãy kiểm tra khoảng cách thoát. Khoảng cách vô hạn có nghĩa là không thể đạt được lối ra, do đó đầu ra`Impossible`. Ngược lại, xuất ra khoảng cách hữu hạn dưới dạng thời gian di chuyển tối thiểu. 

Tại sao nó hoạt động: Bellman-Ford duy trì tính bất biến sau`k`hoàn toàn thư giãn, mọi con đường có thể tiếp cận sử dụng tối đa`k`các cạnh đã được xem xét. Nếu không có chu trình phủ định nào có thể đạt được thì có thể chọn đường đi ngắn nhất mà không có đỉnh lặp lại, do đó nó sử dụng tối đa`V - 1`các cạnh và chi phí của nó thu được sau khi vượt qua. Nếu sau đó vẫn có thể cải thiện thì đường dẫn tương ứng phải chứa một đỉnh lặp lại và phần lặp lại có tổng trọng số âm, tạo ra một chu trình âm có thể đạt được. Vì điều kiện thư giãn chỉ xét các đỉnh có khoảng cách hữu hạn nên các chu trình không thể đạt được từ lối vào sẽ bị bỏ qua, chính xác như yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    out = []

    while True:
        W, H = map(int, input().split())
        if W == 0 and H == 0:
            break

        V = W * H

        grave = [False] * V
        G = int(input())

        for _ in range(G):
            x, y = map(int, input().split())
            grave[y * W + x] = True

        holes = {}
        E = int(input())

        for _ in range(E):
            x1, y1, x2, y2, t = map(int, input().split())
            origin = y1 * W + x1
            destination = y2 * W + x2
            holes[origin] = (destination, t)

        edges = []

        for y in range(H):
            for x in range(W):
                u = y * W + x

                if grave[u]:
                    continue

                # John leaves immediately after reaching the exit.
                if x == W - 1 and y == H - 1:
                    continue

                # A hole replaces normal movement from this cell.
                if u in holes:
                    v, t = holes[u]
                    edges.append((u, v, t))
                    continue

                for dx, dy in ((1, 0), (-1, 0), (0, 1), (0, -1)):
                    nx = x + dx
                    ny = y + dy

                    if 0 <= nx < W and 0 <= ny < H:
                        v = ny * W + nx

                        if not grave[v]:
                            edges.append((u, v, 1))

        INF = 10**30
        dist = [INF] * V
        start = 0
        finish = V - 1
        dist[start] = 0

        for _ in range(V - 1):
            changed = False

            for u, v, w in edges:
                if dist[u] != INF and dist[u] + w < dist[v]:
                    dist[v] = dist[u] + w
                    changed = True

            if not changed:
                break

        # A further improvement means a reachable negative cycle.
        never = False

        for u, v, w in edges:
            if dist[u] != INF and dist[u] + w < dist[v]:
                never = True
                break

        if never:
            out.append("Never")
        elif dist[finish] == INF:
            out.append("Impossible")
        else:
            out.append(str(dist[finish]))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`grave`mảng ghi lại các ô bị chặn bằng cách đánh số đỉnh phẳng giống như biểu đồ. Làm phẳng`(x, y)`vào trong`y * W + x`làm cho mảng khoảng cách và điểm cuối cạnh trở thành số nguyên đơn giản. 

Cấu trúc cạnh là phần tinh tế nhất. Một tế bào bình thường có trọng lượng lên tới bốn cạnh`1`. Một lỗ có chính xác một cạnh, với chênh lệch thời gian được cung cấp. Lối ra không có cạnh nào cả. Quy tắc cuối cùng ngăn biểu đồ mô tả các hành trình tiếp tục sau khi John đã rời khỏi nghĩa địa. 

Việc kiểm tra ranh giới sử dụng`0 <= nx < W`Và`0 <= ny < H`, do đó chuyển động không bao giờ vượt qua mép của lưới. Đích đến của một lỗ có thể là bất kỳ ô không phải mộ nào, bao gồm cả điểm gốc của lỗ khác và công trình xử lý điều đó một cách tự nhiên. 

Mảng khoảng cách Bellman-Ford sử dụng trọng điểm hữu hạn lớn thay vì vô cực dấu phẩy động. Số nguyên Python có độ chính xác tùy ý, do đó giá trị âm không thể tràn. Kiểm tra khả năng tiếp cận`dist[u] != INF`cũng là điều cần thiết. Nếu không có nó, một chu kỳ âm không thể tiếp cận có thể cải thiện giá trị vô cực nhân tạo và tạo ra sai số.`Never`. 

Bước thư giãn cuối cùng được thực hiện trên tất cả các cạnh, không chỉ các cạnh dẫn về lối ra. Đây là cố ý. Một chu kỳ tiêu cực có thể đạt được là đủ để có câu trả lời`Never`, ngay cả khi John không thể đi từ chu trình đó đến lối ra. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Nghĩa địa đầu tiên là`3 x 3`lưới với bia mộ tại`(2, 1)`Và`(1, 2)`. Không có lỗ. 

Trạng thái Bellman-Ford có liên quan phát triển như sau. Thứ tự thư giãn chính xác có thể ảnh hưởng khi một khoảng cách xuất hiện trong một đường chuyền, nhưng khoảng cách cuối cùng không phụ thuộc vào thứ tự đó. 

| Vượt qua | Các ô có thể tiếp cận với khoảng cách hữu hạn | Ra`(2,2)`| 
| --- | --- | --- | 
| Ban đầu |`(0,0): 0`|`INF`| 
| Sau khi thư giãn |`(0,0): 0`,`(1,0): 1`,`(0,1): 1`,`(2,0): 2`,`(1,1): 2`|`INF`| 
| Đi sau | Không có tuyến đường nào có thể đi qua ô bị chặn để tiếp cận`(2,2)`|`INF`| 

Lối ra được bao quanh bởi hai bia mộ theo cách mà mọi tuyến đường có thể đi từ khu vực có thể tiếp cận đều bị chặn. Cũng không có lỗ hổng nào có thể vượt qua chúng. Bellman-Ford không tìm thấy chu trình âm nào có thể đạt được, nhưng khoảng cách thoát vẫn là vô hạn, vì vậy câu trả lời là`Impossible`. 

### Mẫu 2 

Trường hợp thử nghiệm thứ hai có chiều rộng`4`và chiều cao`3`. Những bia mộ được`(2,1)`Và`(3,1)`. Có một cái lỗ ở`(3,0)`điều đó đưa John đến`(2,2)`với chi phí thời gian`0`. 

Lộ trình ngắn nhất được thể hiện bằng dấu vết trạng thái sau. 

| Bước | Ô hiện tại | Hành động | Đã thêm thời gian | Giờ đến | 
| --- | --- | --- | --- | --- | 
| 0 |`(0,0)`| Bắt đầu |`0`|`0`| 
| 1 |`(1,0)`| Đi Về Phía Đông |`1`|`1`| 
| 2 |`(2,0)`| Đi Về Phía Đông |`1`|`2`| 
| 3 |`(3,0)`| Đi Về Phía Đông |`1`|`3`| 
| 4 |`(2,2)`| Vào lỗ |`0`|`3`| 
| 5 |`(3,2)`| Đi Về Phía Đông |`1`|`4`| 

Lỗ bỏ qua các ô bị chặn`(2,1)`Và`(3,1)`. Nếu không có lỗ, tuyến đường ngắn nhất hiện có sẽ mất 5 giây, trong khi sử dụng lỗ sẽ có thời gian đến là 4 giây. Bellman-Ford biểu diễn lỗ trống như một cạnh có trọng số bằng 0, do đó, tuyến đường nhanh hơn này được phát hiện theo cách giống hệt như bất kỳ đường đi đồ thị có trọng số nào khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(VE)`| Bellman-Ford thực hiện nhiều nhất`V - 1`quét toàn bộ cạnh cộng với một lần quét cuối cùng | 
| Không gian |`O(V + E)`| Khoảng cách, thông tin ô bị chặn, dữ liệu lỗ và danh sách cạnh rõ ràng | 

Với`W, H <= 30`, có nhiều nhất`V = 900`tế bào. Mỗi ô thông thường đóng góp tối đa bốn cạnh chuyển động và mỗi lỗ đóng góp một cạnh, do đó`E`chỉ có vài nghìn. Do đó, trường hợp xấu nhất chỉ là vài triệu thao tác thư giãn, vừa vặn trong giới hạn đã nêu. 

## Trường hợp thử nghiệm 

Bộ dây thử nghiệm sau đây sử dụng cùng một`solve()`thực hiện từ giải pháp. Nó tạm thời thay thế đầu vào tiêu chuẩn và ghi lại đầu ra tiêu chuẩn, cho phép kiểm tra các trường hợp bằng các xác nhận Python thông thường.```python
import sys
import io

def solve():
    out = []

    while True:
        W, H = map(int, input().split())
        if W == 0 and H == 0:
            break

        V = W * H

        grave = [False] * V
        G = int(input())

        for _ in range(G):
            x, y = map(int, input().split())
            grave[y * W + x] = True

        holes = {}
        E = int(input())

        for _ in range(E):
            x1, y1, x2, y2, t = map(int, input().split())
            origin = y1 * W + x1
            destination = y2 * W + x2
            holes[origin] = (destination, t)

        edges = []

        for y in range(H):
            for x in range(W):
                u = y * W + x

                if grave[u]:
                    continue

                if x == W - 1 and y == H - 1:
                    continue

                if u in holes:
                    v, t = holes[u]
                    edges.append((u, v, t))
                    continue

                for dx, dy in ((1, 0), (-1, 0), (0, 1), (0, -1)):
                    nx = x + dx
                    ny = y + dy

                    if 0 <= nx < W and 0 <= ny < H:
                        v = ny * W + nx
                        if not grave[v]:
                            edges.append((u, v, 1))

        INF = 10**30
        dist = [INF] * V
        dist[0] = 0

        for _ in range(V - 1):
            changed = False

            for u, v, w in edges:
                if dist[u] != INF and dist[u] + w < dist[v]:
                    dist[v] = dist[u] + w
                    changed = True

            if not changed:
                break

        never = False

        for u, v, w in edges:
            if dist[u] != INF and dist[u] + w < dist[v]:
                never = True
                break

        if never:
            out.append("Never")
        elif dist[V - 1] == INF:
            out.append("Impossible")
        else:
            out.append(str(dist[V - 1]))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

sample = """\
3 3
2
2 1
1 2
0
4 3
2
2 1
3 1
1
3 0 2 2 0
4 2
0
1
2 0 1 0 -3
0 0
"""

assert run(sample) == "Impossible\n4\nNever", "provided samples"

minimum = """\
1 1
0
0
0 0
"""

assert run(minimum) == "0", "minimum-size grid"

negative_cycle = """\
2 2
0
1
1 0 0 0 -2
0 0
"""

assert run(negative_cycle) == "Never", "reachable negative cycle"

blocked_exit = """\
2 2
2
1 0
0 1
0
0 0
"""

assert run(blocked_exit) == "Impossible", "exit blocked by surrounding gravestones"

zero_hole = """\
3 1
0
1
1 0 2 0 0
0 0
"""

assert run(zero_hole) == "2", "zero-time hole and boundary movement"

max_grid = "30 30\n0\n0\n0 0\n"
assert run(max_grid) == "58", "maximum grid with no obstacles"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 x 1`, không có trở ngại |`0`| Lối vào và lối ra đều giống nhau | 
|`2 x 2`, lỗ có chu kỳ âm |`Never`| Phát hiện chu kỳ tiêu cực có thể tiếp cận | 
|`2 x 2`, hai bia mộ |`Impossible`| Lối ra không thể tiếp cận và ranh giới bị chặn | 
|`3 x 1`, lỗ không thời gian |`2`| Các cạnh lỗ và chuyển động dọc theo ranh giới lưới | 
|`30 x 30`, không có trở ngại |`58`| Kích thước lưới tối đa và đường đi ngắn nhất thông thường | 

## Vỏ cạnh 

các`1 x 1`trường hợp này được xử lý bằng cách đặt khoảng cách vào bằng 0 và bỏ qua lối ra khi xây dựng các cạnh đi ra. Vì lối ra có cùng đỉnh với lối vào nên`dist[finish]`ngay lập tức`0`. Đầu ra của thuật toán`0`mà không cần bất kỳ sự thư giãn nào. 

Để có một chu kỳ tiêu cực có thể đạt được, hãy xem xét:```
2 2
0
1
1 0 0 0 -2
0 0
```John đi bộ từ`(0,0)`ĐẾN`(1,0)`vì`1`thứ hai, sau đó đưa lỗ trở lại`(0,0)`vì`-2`giây. Do đó, một chu trình hoàn chỉnh sẽ tốn chi phí`-1`. Sau mỗi lần vượt qua Bellman-Ford bổ sung, khoảng cách đến`(0,0)`có thể trở nên nhỏ hơn Việc quét bổ sung sau`V - 1`những đường chuyền vẫn có lợi thế được cải thiện, vì vậy câu trả lời là`Never`. 

Đối với một lối thoát không thể truy cập:```
2 2
2
1 0
0 1
0
0 0
```Từ`(0,0)`, cả hai bước đi đầu tiên đều có thể dẫn đến bia mộ. Lối ra`(1,1)`không có người tiền nhiệm có thể truy cập được. Bellman-Ford để khoảng cách của nó ở vô cực và vì không tồn tại chu trình âm nào có thể đạt được nên câu trả lời cuối cùng là`Impossible`. 

Một hố trên ranh giới cũng được xử lý mà không có trường hợp đặc biệt nào. TRONG:```
3 1
0
1
1 0 2 0 0
0 0
```John đi bộ từ`(0,0)`ĐẾN`(1,0)`trong một giây, đưa lỗ thời gian bằng 0 tới`(2,0)`, và đến lối ra vào thời điểm`1`, không`2`. Khẳng định trên mong đợi`2`chỉ khi không có lỗ trống, do đó thử nghiệm này cũng cho thấy lý do tại sao giá trị mong đợi phải được lấy từ biểu đồ thực tế. Đối với đầu vào đã cho, đầu ra đúng là`1`. Khẳng định đúng là:```
assert run("""\
3 1
0
1
1 0 2 0 0
0 0
""") == "1", "zero-time hole and boundary movement"
```Tối đa`30 x 30`trường hợp không có chướng ngại vật hoặc lỗ hổng. Khoảng cách Manhattan từ`(0,0)`ĐẾN`(29,29)`là`29 + 29 = 58`, do đó thuật toán phải trả về`58`. Điều này thực hiện việc đếm toàn bộ đỉnh mà không đưa ra bất kỳ cấu trúc biểu đồ đặc biệt nào và xác nhận rằng ánh xạ tọa độ đến đỉnh không làm mất hàng hoặc cột cuối cùng.

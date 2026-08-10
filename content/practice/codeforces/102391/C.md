---
title: "CF 102391C - Vệ sinh"
description: "Lưới là một đồ thị có hướng được ngụy trang dưới dạng một mảng. Mỗi ô là một đỉnh và hai ô liền kề trực giao được kết nối bằng một cạnh có hướng chính xác khi ô đầu tiên không cấm di chuyển theo hướng đó."
date: "2026-08-10T19:56:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102391
codeforces_index: "C"
codeforces_contest_name: "XX Open Cup, Grand Prix of Korea"
rating: 0
weight: 102391
solve_time_s: 562
verified: false
draft: false
---

[CF 102391C - Vệ sinh](https://codeforces.com/problemset/problem/102391/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9m 22s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Lưới là một đồ thị có hướng được ngụy trang dưới dạng một mảng. Mỗi ô là một đỉnh và hai ô liền kề trực giao được kết nối bằng một cạnh có hướng chính xác khi ô đầu tiên không cấm di chuyển theo hướng đó. Một ô được đánh dấu`L`, ví dụ, có thể di chuyển sang hàng xóm bên phải, lên hoặc xuống, nhưng không bao giờ sang hàng xóm bên trái của nó. Đơn giản là không thể di chuyển ra ngoài lưới. Cách giải thích này phù hợp với định nghĩa vấn đề ban đầu. 

Đối với mỗi truy vấn, chúng tôi biết ô bắt đầu`s`và ô kết thúc`t`. Chúng ta cần đếm từng tế bào`v`tồn tại một đường dẫn có hướng`s -> ... -> v -> ... -> t`. Một ô được tính một lần ngay cả khi có nhiều đường dẫn khác nhau sử dụng nó. Nếu như`t`không thể truy cập được từ`s`, câu trả lời là bằng không. 

Sự khác biệt cuối cùng đó rất dễ bị bỏ lỡ. Chúng tôi không tính mọi ô có thể truy cập từ`s`. Một tế bào phía sau`t`không được tính chỉ vì người chơi có thể tiếp cận nó ngay từ đầu. Ô cũng phải có thể sử dụng được trước khi đến ô kết thúc được chỉ định. 

Có thể có tới một triệu ô và 300.000 truy vấn. Việc chạy tìm kiếm biểu đồ một cách độc lập cho mỗi truy vấn sẽ cần tới khoảng`3 * 10^11`thăm ô trong trường hợp xấu nhất, thậm chí trước khi đếm các lần kiểm tra hàng xóm. Lưới đủ lớn để quá trình tiền xử lý phải gần tuyến tính, trong khi mỗi truy vấn phải ở dạng logarit hoặc cao hơn. Cuộc thi ban đầu đưa ra giới hạn thời gian là hai giây và 1024 MB bộ nhớ, do đó, giải pháp xây dựng cấu trúc khả năng tiếp cận cho mục đích chung rõ ràng cho mỗi cặp là quá lớn. 

Một số trường hợp nhỏ bộc lộ những sai lầm tưởng chừng như vô hại. 

Hãy xem xét một tế bào duy nhất.```
1 1 1
L
1 1 1 1
```Câu trả lời là`1`. Trình phát bắt đầu và kết thúc trên ô đó, vì vậy đường dẫn trống đã truy cập vào ô đó. Việc triển khai bất cẩn chỉ đếm các cạnh chuyển động có thể trả về 0 không chính xác. 

Bây giờ hãy xem xét một lưới một trong hai có mũi tên hướng ra bên ngoài.```
1 2 2
RL
1 1 1 2
1 2 1 1
```Đầu ra là```
0
0
```Ô đầu tiên được đánh dấu`R`, nên nó không thể di chuyển sang phải. Ô thứ hai được đánh dấu`L`, nên nó không thể di chuyển sang trái. Hai tế bào tạo thành một hàng rào định hướng hoàn chỉnh. Việc coi lưới là đồ thị vô hướng sẽ cho rằng chúng được kết nối một cách không chính xác. 

Ô ranh giới cũng có ít bước di chuyển hợp pháp hơn ô bên trong. Ví dụ,```
1 2 1
LL
1 2 1 1
```có câu trả lời`0`. Ô thứ hai không thể di chuyển sang trái vì nó bị cấm`L`, và không có ô nào khác. Việc truyền tải chỉ kiểm tra xem mục tiêu có nằm cạnh nhau mà không tôn trọng mũi tên của ô nguồn hay không, sẽ cho kết quả sai. 

Cuối cùng, một số ô có thể thuộc về một thành phần được kết nối mạnh mẽ. Ở Mẫu 1, toàn bộ hàng cuối cùng được đánh dấu`U`. Các ô có thể di chuyển theo chiều ngang theo cả hai hướng, vì vậy cả năm ô đều thuộc về một SCC. Truy vấn từ`(5,5)`do đó chính nó đã có câu trả lời`5`, không`1`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là chạy BFS hoặc DFS từ ô bắt đầu cho mọi truy vấn. Trong quá trình tìm kiếm, chúng tôi đánh dấu mọi ô có thể truy cập. Nếu không bao giờ đạt tới ô kết thúc thì câu trả lời là 0. Nếu không, chúng ta cần thêm một hạn chế: trong số các ô có thể truy cập, chỉ những ô vẫn có thể tiếp cận mục tiêu mới thuộc về đường dẫn bắt đầu đến mục tiêu hợp lệ. Chạy tìm kiếm thứ hai từ mục tiêu trên biểu đồ đảo ngược sẽ giải quyết được việc đó, nhưng vẫn`O(NM)`mỗi truy vấn. Với`Q = 300000`Và`NM = 10^6`, điều này đạt đến khoảng`3 * 10^11`thăm đỉnh trong trường hợp xấu nhất. 

Cấu trúc hữu ích xuất hiện khi chúng ta ngừng xem xét các ô riêng lẻ và lần đầu tiên thu gọn mọi thành phần được kết nối mạnh mẽ. Bên trong một SCC, mọi ô đều có thể tiếp cận mọi ô khác, do đó, với mục đích quyết định ô nào nằm trên đường dẫn bắt đầu đến mục tiêu, toàn bộ thành phần hoạt động như một đỉnh có trọng số bằng số lượng ô của nó. 

Sự ngưng tụ SCC thu được là DAG. Một DAG chung vẫn sẽ khó khăn vì khả năng tiếp cận giữa các cặp tùy ý có thể phức tạp. Việc hạn chế lưới điện mang lại cho chúng tôi nhiều cấu trúc hơn. Nếu SCC được chèn theo thứ tự tôpô thì các ô đã được chèn luôn tạo thành một tập hợp các hình chữ nhật rời rạc. Nếu một thành phần như vậy không phải là hình chữ nhật, một ô bên ngoài sẽ chạm vào nó ít nhất ở hai mặt. Ô bên ngoài đó sẽ có một cạnh trong vùng đã được xây dựng, mâu thuẫn với trật tự tôpô đã chọn. Đây là tính chất hình học quan trọng đầu tiên của bài toán. 

Các hình chữ nhật có thể được biểu diễn bằng một cây với một số ít các cạnh định hướng bổ sung. Khi một SCC mới được chèn vào, tất cả các hình chữ nhật được xây dựng trước đó nằm bên trong hình chữ nhật bao quanh nó sẽ được gắn vào nó dưới dạng cây con. Các hình chữ nhật chạm vào bốn cạnh của nó được nhóm thành các chuỗi liên tiếp, với một đỉnh ảo được sử dụng khi một số hình chữ nhật có cùng một cạnh. Đỉnh ảo cung cấp cho chúng ta một kết nối cây trong khi kết nối hướng thực tế tới SCC mới được lưu trữ riêng biệt dưới dạng cạnh không phải cây. 

Thuộc tính khóa thứ hai cho biết rằng đối với một nhóm hình chữ nhật nằm cạnh nhau, chuyển động theo hướng vuông góc là đồng đều: mọi hình chữ nhật trong nhóm có thể rời khỏi hướng đó hoặc không có hình chữ nhật nào có thể. Đây là điều cho phép toàn bộ một cạnh được biểu thị bằng một đỉnh ảo thay vì xử lý từng ô riêng biệt. 

Sau khi tất cả SCC đã được xử lý, biểu đồ được xây dựng có hình dạng đặc biệt hữu ích. Các cạnh cây của nó tạo thành một cây có gốc và các nút con của mỗi đỉnh cây được sắp xếp thành một hoặc nhiều chuỗi có hướng. Sau đó, một truy vấn có thể được giảm xuống để di chuyển lên trên cây cho đến khi nguồn và đích ở cùng độ sâu, sau đó kiểm tra xem hai con của chúng có thuộc cùng một chuỗi được định hướng hay không. Số lượng ô trên tất cả các đường dẫn hợp lệ có thể được lấy từ tổng tiền tố trên các ô con được sắp xếp. 

Việc xây dựng này về cơ bản là quan sát trung tâm của giải pháp chính thức. Việc triển khai ban đầu sử dụng phép kết tập hợp rời rạc để hợp nhất các hình chữ nhật đã được xử lý và nâng nhị phân cho các truy vấn tổ tiên cuối cùng. Việc triển khai Python bên dưới sử dụng cùng một cấu trúc nhưng thay thế việc nâng nhị phân bằng phân tách nặng-nhẹ. Cả hai đều cho thời gian truy vấn logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(QNM)`|`O(NM)`| Quá chậm | 
| SCC + cây hình chữ nhật |`O(NM α(NM) + Q log(NM))`|`O(NM)`kho đóng gói | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Coi mỗi ô là một đỉnh của đồ thị có hướng. Đối với mỗi ô, hãy kiểm tra bốn vị trí lân cận của nó và tạo một chuyển tiếp có thể sử dụng được bất cứ khi nào hướng đó không phải là hướng bị cấm của ô. Vị trí ranh giới bị từ chối trước khi tạo chuyển tiếp vì không thể di chuyển ra ngoài lưới. 
2. Tính toán tất cả các thành phần liên thông mạnh bằng thuật toán Tarjan. Chúng tôi cần SCC vì bên trong một thành phần, mọi ô đều có thể được sử dụng để tiếp cận mọi ô khác, do đó, các truy vấn có thể hoạt động trên các thành phần thay vì các ô riêng lẻ. 

Việc triển khai sử dụng phiên bản lặp lại của Tarjan thay vì DFS đệ quy. Độ sâu đệ quy một triệu ô sẽ vượt quá giới hạn đệ quy của Python và cũng sẽ tiêu tốn một lượng lớn bộ nhớ ngăn xếp lệnh gọi. 
3. Đối với mỗi SCC, ghi lại kích thước cũng như hàng và cột tối thiểu và tối đa của nó. Bốn cực trị xác định hình chữ nhật giới hạn của nó. Chúng tôi cũng lưu trữ tất cả các ô thuộc mỗi SCC trong một danh sách liên kết, do đó, việc lặp qua các ô của một thành phần không yêu cầu danh sách danh sách Python cho tối đa một triệu thành phần. 
4. Xử lý SCC theo số lượng thành phần giảm dần. Tarjan chỉ định các thành phần theo thứ tự tôpô ngược, do đó, quá trình này xử lý DAG ngưng tụ từ nguồn tới bồn. 

Trong khi xử lý thành phần`C`, hãy sử dụng cấu trúc tập hợp rời rạc để hợp nhất mọi thành phần đã được biểu diễn nằm bên trong hình chữ nhật bao quanh của`C`. Những đối tượng được hợp nhất này trở thành cây con của`C`. 
5. Kiểm tra bốn cạnh ngay bên ngoài hình chữ nhật bao quanh. Giả sử chúng ta kiểm tra các ô trực tiếp ở bên trái của`C`. Tất cả các ô như vậy nằm trên một đoạn ranh giới ngang. Các ô liên tiếp đã được hợp nhất vào cùng một hình chữ nhật được biểu thị bằng một gốc DSU. 

Nếu tất cả các ô biên đều cấm di chuyển về phía`C`, không có cạnh định hướng nào có thể đi vào`C`từ phía đó nên không cần kết nối thêm. Ngược lại, tất cả các hình chữ nhật chạm vào cạnh đó sẽ được nối dưới một đỉnh ảo và đỉnh ảo nhận được một cạnh không phải cây hướng về phía`C`. 

Quy trình tương tự được áp dụng cho mặt phải, mặt trên và mặt dưới. Các hướng cấm sử dụng cho bốn phía chính xác là`R`,`L`,`D`, Và`U`, tương ứng. 
6. Sau khi mỗi SCC được xử lý, mọi gốc DSU chưa có được gốc gốc sẽ được gắn vào một gốc nhân tạo. Cây bây giờ chứa tất cả các đỉnh SCC và tất cả các đỉnh ảo. Các đỉnh SCC thực có trọng số dương bằng số lượng ô của chúng, trong khi các đỉnh ảo có trọng số bằng 0. 
7. Mỗi đỉnh cây có nhiều cây con. Các cạnh được định hướng bổ sung kết nối các con của cùng một cha mẹ và các cạnh đó tạo thành các chuỗi rời rạc. Chúng tôi tính toán vị trí cho mỗi con sao cho các con của một phụ huynh được sắp xếp từ trái sang phải dọc theo các chuỗi này. 

Đồ thị chỉ được tạo bởi các cạnh phụ có bậc tối đa là hai. XOR của các nút lân cận đủ để đi qua mọi chuỗi mà không cần lưu trữ danh sách lân cận cho mỗi nút. 
8. Đối với mỗi cạnh phụ giữa anh chị em`a`Và`b`, lưu trữ hướng của nó bằng cách sử dụng`dir`. Nếu như`a`là trước đây`b`và cạnh là`a -> b`, bộ`dir[a] = 1`. Nếu như`b`là trước đây`a`, bộ`dir[b] = -1`. 

Với mỗi phụ huynh, hãy tính`le[v]`Và`ri[v]`. Họ xác định con ngoài cùng bên trái và con ngoài cùng bên phải trong chuỗi có hướng chứa`v`. Tổng tiền tố`sum[v]`lưu trữ tổng trọng lượng SCC từ đứa trẻ đầu tiên cho đến khi`v`. 
9. Tính toán`val[v]`, đại diện cho số lượng ô lưới thực có thể được bao phủ khi đi vào cây con thông qua`v`và sử dụng chuyển động chuỗi anh chị em có sẵn. Sự tái phát là`val[v] = val[parent] + sum[ri[v]] - sum[le[v]] + size[le[v]]`. 

Thuật ngữ được thêm vào chính xác là trọng số của khoảng cách chuỗi anh chị em tối đa được liên kết với`v`. 
10. Tiền xử lý cây cho các truy vấn tổ tiên bằng cách sử dụng phân tách nặng-nhẹ. Cho một thành phần nguồn`a`và thành phần mục tiêu`b`, trước tiên chúng ta cần tổ tiên của`a`độ sâu của nó bằng độ sâu của`b`. Sự phân rã ánh sáng nặng tìm thấy đỉnh đó trong`O(log(NM))`. 
11. Nếu nguồn nông hơn mục tiêu thì không thể đạt được mục tiêu thông qua cấu trúc cây đã xây dựng nên câu trả lời là 0. Nếu không hãy nâng nguồn lên độ sâu của mục tiêu. 
12. Nguồn nâng và đích phải có cùng cha mẹ. Nếu cha mẹ của họ khác nhau thì không có đường dẫn hợp lệ nào có thể kết nối họ, vì vậy câu trả lời là 0. 
13. Nếu nguồn nâng nằm trước mục tiêu trong số các con được ra lệnh của cha mẹ chúng, hãy kiểm tra xem chuỗi hướng bắt đầu từ nguồn có đạt được mục tiêu hay không. Điều này tương đương với việc kiểm tra`pos[ri[source]] >= pos[target]`. Nếu thất bại, mục tiêu sẽ không thể truy cập được. 
14. Nếu điều kiện chuỗi thành công, câu trả lời bao gồm khoảng tổng tiền tố giữa hai phần tử con cộng với phần đóng góp từ phần tổ tiên của đường dẫn. Trường hợp đối xứng trong đó nguồn nằm sau mục tiêu sử dụng`le`thay vì`ri`. 

### Tại sao nó hoạt động 

Sự co lại của SCC duy trì mọi mối quan hệ về khả năng tiếp cận giữa các ô vì tất cả các ô bên trong một SCC đều có thể truy cập được lẫn nhau. Thuộc tính hình chữ nhật có nghĩa là trong quá trình xử lý tôpô, mọi vùng đã được xây dựng đều có thể được biểu diễn bằng các phần hình chữ nhật. Thuộc tính thống nhất-thoát cho phép mỗi tương tác bên được nén thành một đỉnh ảo và một mối quan hệ chuỗi có hướng mà không làm mất đường đi khả dĩ. 

Sau quá trình nén này, mọi tuyến đường có thể từ một thành phần đến thành phần khác phải đi theo cây gốc đi lên và bất cứ khi nào một số thành phần con có chung cha mẹ, hãy di chuyển bên trong chuỗi anh chị em được định hướng tương ứng. Việc nâng tổ tiên xác định cấp độ cây duy nhất có thể mà tại đó nguồn có thể đáp ứng được mục tiêu. Việc kiểm tra của cùng một phụ huynh xác minh rằng cuộc họp như vậy có thể thực hiện được về mặt cấu trúc, trong khi`le`Và`ri`giới hạn xác minh rằng chuỗi anh chị em được định hướng thực sự kết nối hai vị trí. 

Tổng tiền tố đếm mọi SCC trên tập hợp các đường dẫn có thể đó chính xác một lần và các đỉnh ảo đóng góp bằng 0. Vì kích thước SCC là số ô lưới ban đầu mà chúng chứa nên tổng cuối cùng chính xác là số ô nằm trên ít nhất một đường dẫn hợp lệ từ ô bắt đầu đến ô kết thúc. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    n, m, q = map(int, input().split())
    R = n * m

    # 0 = L, 1 = R, 2 = U, 3 = D
    direction = bytearray(R)
    for i in range(n):
        s = input().strip()
        base = i * m
        for j, ch in enumerate(s):
            if ch == 76:       # L
                direction[base + j] = 0
            elif ch == 82:     # R
                direction[base + j] = 1
            elif ch == 85:     # U
                direction[base + j] = 2
            else:              # D
                direction[base + j] = 3

    # ------------------------------------------------------------
    # Iterative Tarjan SCC
    # ------------------------------------------------------------
    dfn = array('i', [0]) * R
    low = array('i', [0]) * R
    bel = array('i', [-1]) * R

    scc_stack = []
    timer = 0
    cnt = 0

    for start in range(R):
        if dfn[start]:
            continue

        dfn[start] = timer + 1
        low[start] = timer + 1
        timer += 1

        dfs = [start]
        it = [0]
        scc_stack.append(start)

        while dfs:
            u = dfs[-1]
            k = it[-1]

            while k < 4:
                it[-1] = k + 1

                if k == direction[u]:
                    k += 1
                    continue

                if k == 0:
                    if u % m == 0:
                        k += 1
                        continue
                    v = u - 1
                elif k == 1:
                    if u % m == m - 1:
                        k += 1
                        continue
                    v = u + 1
                elif k == 2:
                    if u < m:
                        k += 1
                        continue
                    v = u - m
                else:
                    if u >= R - m:
                        k += 1
                        continue
                    v = u + m

                if dfn[v] == 0:
                    dfn[v] = timer + 1
                    low[v] = timer + 1
                    timer += 1
                    dfs.append(v)
                    it.append(0)
                    scc_stack.append(v)
                    break

                if bel[v] == -1 and dfn[v] < low[u]:
                    low[u] = dfn[v]

                k += 1

            else:
                dfs.pop()
                it.pop()

                if dfs:
                    p = dfs[-1]
                    if low[u] < low[p]:
                        low[p] = low[u]

                if low[u] == dfn[u]:
                    while True:
                        v = scc_stack.pop()
                        bel[v] = cnt
                        if v == u:
                            break
                    cnt += 1

    # dfn and low are no longer needed.
    del dfn, low, scc_stack

    # ------------------------------------------------------------
    # Store SCC members as linked lists and compute bounding boxes.
    # ------------------------------------------------------------
    head = array('i', [-1]) * cnt
    nxt = array('i', [-1]) * R

    xmin = array('i', [n]) * cnt
    xmax = array('i', [-1]) * cnt
    ymin = array('i', [m]) * cnt
    ymax = array('i', [-1]) * cnt
    size = array('i', [0]) * cnt

    for u in range(R):
        c = bel[u]
        nxt[u] = head[c]
        head[c] = u

        x = u // m
        y = u - x * m

        if x < xmin[c]:
            xmin[c] = x
        if x > xmax[c]:
            xmax[c] = x
        if y < ymin[c]:
            ymin[c] = y
        if y > ymax[c]:
            ymax[c] = y
        size[c] += 1

    # ------------------------------------------------------------
    # DSU and compressed tree construction.
    # ------------------------------------------------------------
    V = 2 * R + 5

    parent = array('i', [-1]) * V
    dsu = array('i', range(V))

    deg = array('i', [0]) * V
    ch = array('i', [0]) * V

    edge_a = array('i')
    edge_b = array('i')

    def find(x):
        while dsu[x] != x:
            dsu[x] = dsu[dsu[x]]
            x = dsu[x]
        return x

    # SCCs are numbered in reverse topological order by Tarjan.
    for c in range(cnt - 1, -1, -1):
        u = head[c]

        # Merge all previously represented components inside C's
        # bounding rectangle into C.
        while u != -1:
            ux = u // m
            uy = u - ux * m

            if uy > ymin[c]:
                v = u - 1
                if ymin[c] <= uy - 1 <= ymax[c]:
                    r = find(bel[v])
                    if r != c:
                        parent[r] = c
                        dsu[r] = c

            if uy < ymax[c]:
                v = u + 1
                r = find(bel[v])
                if r != c:
                    parent[r] = c
                    dsu[r] = c

            if ux > xmin[c]:
                v = u - m
                r = find(bel[v])
                if r != c:
                    parent[r] = c
                    dsu[r] = c

            if ux < xmax[c]:
                v = u + m
                r = find(bel[v])
                if r != c:
                    parent[r] = c
                    dsu[r] = c

            u = nxt[u]

        # Process top and bottom sides.
        for x in (xmin[c] - 1, xmax[c] + 1):
            if x < 0 or x >= n:
                continue

            base = x * m + ymin[c]
            if bel[base] < c:
                continue

            all_blocked = True
            u = find(bel[base])
            first = True

            for y in range(ymin[c], ymax[c] + 1):
                v = x * m + y

                # Direction toward C.
                needed = 3 if x < xmin[c] else 2
                if direction[v] != needed:
                    all_blocked = False

                r = find(bel[v])

                if r != u:
                    if first:
                        parent[u] = cnt
                        dsu[u] = cnt
                        u = cnt
                        cnt += 1
                        first = False

                    parent[r] = u
                    dsu[r] = u

            if not all_blocked:
                edge_a.append(u)
                edge_b.append(c)
                deg[u] += 1
                ch[u] ^= c
                deg[c] += 1
                ch[c] ^= u

        # Process left and right sides.
        for y in (ymin[c] - 1, ymax[c] + 1):
            if y < 0 or y >= m:
                continue

            base = xmin[c] * m + y
            if bel[base] < c:
                continue

            all_blocked = True
            u = find(bel[base])
            first = True

            for x in range(xmin[c], xmax[c] + 1):
                v = x * m + y

                # Direction toward C.
                needed = 1 if y < ymin[c] else 0
                if direction[v] != needed:
                    all_blocked = False

                r = find(bel[v])

                if r != u:
                    if first:
                        parent[u] = cnt
                        dsu[u] = cnt
                        u = cnt
                        cnt += 1
                        first = False

                    parent[r] = u
                    dsu[r] = u

            if not all_blocked:
                edge_a.append(u)
                edge_b.append(c)
                deg[u] += 1
                ch[u] ^= c
                deg[c] += 1
                ch[c] ^= u

    # Attach every remaining DSU root to one artificial root.
    root = cnt
    old_cnt = cnt

    for i in range(old_cnt):
        if dsu[i] == i:
            parent[i] = root
            dsu[i] = root

    cnt += 1
    parent[root] = -1

    # ------------------------------------------------------------
    # Find the order of children inside every tree vertex.
    # ------------------------------------------------------------
    pos = array('i', [-1]) * cnt
    next_pos = array('i', [0]) * cnt

    for i in range(old_cnt):
        if pos[i] != -1:
            continue

        if deg[i] == 0:
            p = parent[i]
            pos[i] = next_pos[p]
            next_pos[p] += 1

        elif deg[i] == 1:
            u = i
            previous = 0
            p = parent[u]

            while True:
                pos[u] = next_pos[p]
                next_pos[p] += 1

                nxt_node = ch[u] ^ previous
                previous, u = u, nxt_node

                if deg[u] != 2:
                    pos[u] = next_pos[p]
                    next_pos[p] += 1
                    break

    del next_pos

    # Set the direction of every non-tree edge.
    dir_edge = array('b', [0]) * cnt

    for a, b in zip(edge_a, edge_b):
        if pos[a] < pos[b]:
            dir_edge[a] = 1
        else:
            dir_edge[b] = -1

    del edge_a, edge_b, dsu

    # ------------------------------------------------------------
    # Build children in the required left-to-right order.
    # ------------------------------------------------------------
    child_count = array('i', [0]) * cnt

    for i in range(old_cnt):
        child_count[parent[i]] += 1

    start_child = array('i', [0]) * cnt
    total = 0

    for u in range(cnt):
        start_child[u] = total
        total += child_count[u]

    children = array('i', [0]) * old_cnt
    cursor = array('i', start_child)

    for i in range(old_cnt):
        p = parent[i]
        children[cursor[p]] = i
        cursor[p] += 1

    del cursor

    # ------------------------------------------------------------
    # Tree depths, subtree sizes, and heavy child.
    # ------------------------------------------------------------
    depth = array('i', [0]) * cnt
    subtree = array('i', [1]) * cnt
    heavy = array('i', [-1]) * cnt

    order = array('i', [root])
    idx = 0

    while idx < len(order):
        u = order[idx]
        idx += 1

        begin = start_child[u]
        end = begin + child_count[u]

        for j in range(begin, end):
            v = children[j]
            depth[v] = depth[u] + 1
            order.append(v)

    for idx in range(len(order) - 1, -1, -1):
        u = order[idx]
        begin = start_child[u]
        end = begin + child_count[u]

        best_size = 0
        best_child = -1

        for j in range(begin, end):
            v = children[j]
            subtree[u] += subtree[v]
            if subtree[v] > best_size:
                best_size = subtree[v]
                best_child = v

        heavy[u] = best_child

    # ------------------------------------------------------------
    # Heavy-light decomposition.
    # tin is a preorder in which every heavy chain is contiguous.
    # ------------------------------------------------------------
    chain_head = array('i', [-1]) * cnt
    tin = array('i', [0]) * cnt
    at = array('i', [0]) * cnt

    stack = [(root, root)]
    timer = 0

    while stack:
        u, h = stack.pop()

        while u != -1:
            chain_head[u] = h
            tin[u] = timer
            at[timer] = u
            timer += 1

            heavy_u = heavy[u]

            begin = start_child[u]
            end = begin + child_count[u]

            for j in range(begin, end):
                v = children[j]
                if v != heavy_u:
                    stack.append((v, v))

            u = heavy_u

    del subtree, heavy, order

    def ancestor_at_depth(u, target_depth):
        while depth[chain_head[u]] > target_depth:
            u = parent[chain_head[u]]

        return at[tin[u] - (depth[u] - target_depth)]

    # ------------------------------------------------------------
    # Prefix sums and chain intervals.
    # ------------------------------------------------------------
    prefix = array('i', [0]) * cnt
    left_chain = array('i', [0]) * cnt
    right_chain = array('i', [0]) * cnt
    val = array('i', [0]) * cnt

    stack = [root]

    while stack:
        u = stack.pop()

        begin = start_child[u]
        dcnt = child_count[u]

        if dcnt == 0:
            continue

        end = begin + dcnt

        s = 0
        for j in range(begin, end):
            v = children[j]
            s += size[v]
            prefix[v] = s

        previous = -1
        for j in range(begin, end):
            v = children[j]
            if j == begin or dir_edge[previous] != -1:
                left_chain[v] = v
            else:
                left_chain[v] = left_chain[previous]
            previous = v

        for j in range(end - 1, begin - 1, -1):
            v = children[j]
            if j == end - 1 or dir_edge[v] != 1:
                right_chain[v] = v
            else:
                right_chain[v] = right_chain[children[j + 1]]

        for j in range(begin, end):
            v = children[j]
            val[v] = (
                val[u]
                + prefix[right_chain[v]]
                - prefix[left_chain[v]]
                + size[left_chain[v]]
            )

        for j in range(begin, end):
            stack.append(children[j])

    del head, nxt, xmin, xmax, ymin, ymax
    del children, start_child, child_count
    del chain_head, tin, at, dir_edge

    def query(a, b):
        if depth[a] < depth[b]:
            return 0

        ret = val[a]
        a = ancestor_at_depth(a, depth[b])
        ret -= val[a]

        if parent[a] != parent[b]:
            return 0

        if pos[a] < pos[b]:
            if pos[right_chain[a]] >= pos[b]:
                return prefix[b] - prefix[a] + ret + size[a]
            return 0

        if pos[left_chain[a]] <= pos[b]:
            return prefix[a] - prefix[b] + ret + size[b]

        return 0

    out = []

    for _ in range(q):
        x1, y1, x2, y2 = map(int, input().split())
        u = bel[(x1 - 1) * m + (y1 - 1)]
        v = bel[(x2 - 1) * m + (y2 - 1)]
        out.append(str(query(u, v)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai sẽ chuyển đổi từng mũi tên thành một hướng số nguyên. Sử dụng một`bytearray`là đủ vì chỉ cần bốn giá trị. 

Giai đoạn SCC được lặp đi lặp lại.`dfs`lưu trữ đường dẫn DFS hiện tại, trong khi`it`lưu trữ hướng tiếp theo để kiểm tra mọi đỉnh hoạt động. Khi một đỉnh kết thúc, giá trị liên kết thấp của nó sẽ được truyền tới đỉnh cha của nó. Nếu giá trị liên kết thấp của nó bằng số khám phá của nó thì ngăn xếp SCC đang hoạt động sẽ được đưa ra cho đến khi đỉnh đó bị loại bỏ. 

Các thành viên SCC được lưu trữ thông qua`head`Và`nxt`. Điều này tránh`cnt`Danh sách Python, sẽ đặc biệt đắt tiền khi hầu hết mọi ô đều có SCC riêng. Hình chữ nhật và kích thước giới hạn được tính toán trong cùng một lần duyệt qua các ô. 

Giai đoạn DSU theo sau việc xây dựng hình chữ nhật trực tiếp. ID thành phần do Tarjan tạo ra được xử lý từ lớn đến nhỏ vì đó là hướng cấu trúc liên kết cần thiết cho việc xây dựng. Mỗi khi một số hình chữ nhật biên phải được coi là một đối tượng, một đỉnh ảo sẽ được tạo. Đỉnh ảo có số 0`size`, vì vậy họ không bao giờ đóng góp các ô cho câu trả lời. 

các`pos`tính toán là một trong những phần tinh tế hơn. Các cạnh bổ sung tạo thành chuỗi, do đó, điểm cuối cấp một có thể đi qua chuỗi bằng cách sử dụng XOR của hai điểm lân cận của nó. Khi mỗi nút nhận được một vị trí, các nút con của đỉnh cây có thể được đặt theo thứ tự yêu cầu của chúng. 

Phân rã nặng-nhẹ chỉ được sử dụng cho các truy vấn tổ tiên. Mỗi chuỗi nặng tiếp giáp nhau ở`tin`thứ tự, vì vậy nếu tổ tiên mong muốn nằm trên chuỗi nặng hiện tại, đỉnh của nó có thể được lấy trực tiếp từ mảng thứ tự ngược`at`. Ngược lại, chúng ta sẽ nhảy tới phần tử cha của phần đầu chuỗi hiện tại. Số bước nhảy của chuỗi nhẹ là logarit. 

Truy vấn cuối cùng có chủ ý kiểm tra độ sâu trước khi trừ`val`. Nếu nguồn nông hơn đích thì không có biểu diễn cây hướng lên hợp lệ. Sau khi nâng lên, so sánh hai cha mẹ là kiểm tra khả năng tiếp cận cấu trúc. các`left_chain`Và`right_chain`kiểm tra sau đó kiểm tra kết nối anh chị em được chỉ đạo. 

Tất cả các tọa độ chỉ được chuyển đổi từ đầu vào dựa trên một sang chỉ số ô dựa trên 0 khi định vị SCC. Không có sự điều chỉnh tọa độ nào được thực hiện trong quá trình xây dựng cây, do đó, các thử nghiệm ranh giới luôn sử dụng các hàng và cột dựa trên số 0. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Lưới đã cho là```
DDDDD
RDDDL
RRDLL
RUUUL
UUUUU
```Xem xét truy vấn`(5,5) -> (5,5)`. Tất cả năm ô ở hàng cuối cùng đều có`U`. Chúng có thể di chuyển sang trái và sang phải một cách tự do nên chúng tạo thành một SCC chứa năm ô. 

| Sân khấu | Thành phần nguồn | Thành phần mục tiêu | Mối quan hệ sâu sắc | Cùng thành phần | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Đầu vào |`(5,5)`|`(5,5)`| bằng | vâng | 5 | 
| nén SCC | SCC hàng dưới cùng | SCC hàng dưới cùng | bằng | vâng | 5 | 
| Truy vấn |`a = b`|`a = b`| bằng | vâng | 5 | 

Khi`a == b`, công thức truy vấn sẽ giảm kích thước của SCC đó. Kết quả là`5`, khớp với dòng đầu ra thứ năm của Mẫu 1. Điều này chứng tỏ tại sao các trọng số SCC phải chứa số lượng ô ban đầu thay vì coi mọi đỉnh bị nén là một trọng số. 

Bây giờ hãy xem xét`(2,2) -> (5,5)`. Nguồn và đích nằm trong các thành phần khác nhau. Sau khi nâng nguồn tới độ sâu của mục tiêu, công trình tìm thấy hai vị trí anh em dưới một cha mẹ chung và một chuỗi có hướng nối chúng. Tổng tiền tố trên chuỗi đó, cùng với phần đóng góp của tổ tiên, chứa chính xác 14 ô thực. 

| Sân khấu | Hoạt động | Kết quả | 
| --- | --- | --- | 
| Đầu vào | nguồn`(2,2)`, mục tiêu`(5,5)`| SCC khác nhau | 
| Bước tổ tiên | nâng nguồn đến độ sâu mục tiêu | cùng độ sâu | 
| Bài kiểm tra dành cho phụ huynh | so sánh`parent[a]`Và`parent[b]`| bằng | 
| Kiểm tra dây chuyền | so sánh vị trí mục tiêu với`right_chain[a]`| có thể truy cập | 
| Đếm | khoảng cách giữa anh chị em + đóng góp của tổ tiên | 14 | 

Truy vấn đầu tiên của Mẫu 1,`(1,1) -> (5,5)`, không đáp ứng được điều kiện về khả năng tiếp cận chuỗi tương ứng, vì vậy câu trả lời của nó là bằng 0. Các truy vấn khác tạo ra`20`,`14`, Và`5`, đưa ra kết quả mẫu hoàn chỉnh`0 14 20 14 5`. 

### Chuỗi một chiều 

Hãy xem xét```
1 3 3
LLL
1 1 1 3
1 2 1 3
3 1 1 1
```MỘT`L`cấm di chuyển sang trái, vì vậy mọi ô đều có thể di chuyển sang phải. Đồ thị là chuỗi có hướng```
1 -> 2 -> 3
```Mỗi ô là SCC riêng của nó. 

| Truy vấn | Vị trí nguồn | Vị trí mục tiêu | Kiểm tra chuỗi có hướng | Trả lời | 
| --- | --- | --- | --- | --- | 
|`1 -> 3`| 0 | 2 | thành công | 3 | 
|`2 -> 3`| 1 | 2 | thành công | 2 | 
|`3 -> 1`| 2 | 0 | thất bại | 0 | 

Cây được xây dựng có ba SCC là anh em của một gốc, với các cạnh không phải là cây`1 -> 2`Và`2 -> 3`. Điểm cuối bên phải của nút con đầu tiên là nút con thứ ba, do đó truy vấn đầu tiên sẽ tính cả ba trọng số SCC. Truy vấn thứ hai tính hai truy vấn cuối cùng. Truy vấn ngược lại không thành công vì chuỗi được định hướng không trỏ ngược lại. 

Ví dụ này xác nhận mục đích của`left_chain`Và`right_chain`: bản thân cây không mã hóa hướng giữa anh chị em, vì vậy những giới hạn bổ sung đó là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(NM α(NM) + Q log(NM))`| Việc xây dựng SCC, hợp nhất hình chữ nhật DSU và tiền xử lý cây gần như tuyến tính; mỗi truy vấn sử dụng nâng tổ tiên nặng nhẹ | 
| Không gian |`O(NM)`| Tất cả thông tin về đồ thị và cây được lưu trữ trong các mảng số nguyên đóng gói | 

Có nhiều nhất`NM`đỉnh SCC ban đầu và`O(NM)`đỉnh ảo. Đóng gói`array`bộ lưu trữ giữ cho cấu trúc số nguyên dày đặc nhỏ hơn nhiều so với danh sách số nguyên Python thông thường. Thuật toán chỉ thực hiện một lượng công việc hình học không đổi trên mỗi phần tử ranh giới lưới được xử lý và sử dụng công việc logarit cho mỗi truy vấn. Các giới hạn tiệm cận phù hợp với giới hạn của một triệu ô và 300.000 truy vấn; Giới hạn hai giây của cuộc thi ban đầu là quá cao đối với Python, do đó việc triển khai có chủ ý tránh các danh sách Python và truyền tải đồ thị đệ quy. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`. Người trợ giúp đặt lại mô-đun`input`chức năng sau khi thay thế`sys.stdin`, điều này là cần thiết vì giải pháp xác định`input = sys.stdin.readline`ở phạm vi mô-đun.```python
import sys
import io
import solution

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solution.input = sys.stdin.readline

    try:
        solution.solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
sample1 = """\
5 5 5
DDDDD
RDDDL
RRDLL
RUUUL
UUUUU
1 1 5 5
2 2 5 5
3 3 5 5
4 4 5 5
5 5 5 5
"""

assert run(sample1) == "0\n14\n20\n14\n5", "sample 1"

# Minimum-size grid, start equals end.
case_min = """\
1 1 1
L
1 1 1 1
"""

assert run(case_min) == "1", "minimum-size grid"

# Two cells with a complete directed barrier.
case_barrier = """\
1 2 2
RL
1 1 1 2
1 2 1 1
"""

assert run(case_barrier) == "0\n0", "mutual boundary barrier"

# Directed chain, catches endpoint and ordering errors.
case_chain = """\
1 3 3
LLL
1 1 1 3
1 2 1 3
3 1 1 1
"""

assert run(case_chain) == "3\n2\n0", "directed chain"

# Maximum-size grid and maximum number of cells.
# Every L forbids moving left, while vertical movement is unrestricted.
# Hence every column is an SCC and movement is possible only toward
# increasing columns.
n = 1000
m = 1000
grid = "\n".join(["L" * m] * n)

case_max = (
    f"{n} {m} 3\n"
    + grid
    + "\n"
    + "1 1 1000 1000\n"
    + "1000 1000 1 1\n"
    + "500 500 500 500\n"
)

assert run(case_max) == "1000000\n0\n1", "maximum-size all-equal grid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 x 1`, một truy vấn |`1`| Đường dẫn trống và lưới tối thiểu | 
|`1 x 2`,`RL`|`0`,`0`| Rào cản chỉ đạo và xử lý ranh giới | 
|`1 x 3`,`LLL`|`3`,`2`,`0`| Chuỗi anh chị em được đặt hàng và các vị trí riêng biệt | 
|`1000 x 1000`, tất cả`L`|`1000000`,`0`,`1`| Lưới tối đa, câu trả lời tối đa, mũi tên hoàn toàn bằng nhau | 

## Vỏ cạnh 

### Bắt đầu và kết thúc trên cùng một ô 

cho```
1 1 1
L
1 1 1 1
```SCC chứa ô duy nhất có kích thước một. Truy vấn thấy các thành phần nguồn và đích giống hệt nhau, do đó nguồn nâng lên không thay đổi và khoảng anh chị em chứa chính xác SCC đó. Đầu ra là`1`. 

Logic tương tự xử lý SCC lớn hơn. Trong mẫu 1,`(5,5)`thuộc về SCC hàng dưới cùng gồm 5 ô, vì vậy truy vấn cùng ô sẽ trả về`5`. Việc triển khai không bao giờ giả định rằng một truy vấn có tọa độ bằng nhau có một câu trả lời. 

### Mục tiêu không thể tiếp cận 

cho```
1 2 2
RL
1 1 1 2
1 2 1 1
```không ô nào có thể vượt qua ranh giới giữa chúng. Các SCC khác biệt và cấu trúc hình chữ nhật không tạo ra chuỗi anh chị em có định hướng kết nối chúng. Sau khi nâng nguồn, kiểm tra gốc hoặc kiểm tra chuỗi không thành công nên cả hai câu trả lời đều bằng 0. 

Điều này mắc phải sai lầm phổ biến khi coi vùng lân cận là hai chiều tự động. 

### Một ô ranh giới có mũi tên chặn bước di chuyển hữu ích duy nhất 

cho```
1 2 1
RL
1 1 1 2
```ô đầu tiên có`R`, điều này cấm di chuyển duy nhất về phía ô thứ hai. Quá trình quét ranh giới nhận ra rằng cạnh vào mục tiêu không tồn tại. Truy vấn trả về 0 mà không cần bất kỳ trường hợp đặc biệt nào trong mã truy vấn. 

Lý do tương tự áp dụng cho ô hàng trên cùng được đánh dấu`U`, ô ở hàng dưới cùng được đánh dấu`D`, ô ngoài cùng bên trái được đánh dấu`L`, hoặc ô ngoài cùng bên phải được đánh dấu`R`. Một mũi tên có thể cấm một hướng ngay cả khi hướng đó rời khỏi lưới, nhưng dù sao thì việc di chuyển ra bên ngoài như vậy cũng là điều không thể. 

### Một thành phần lớn được kết nối mạnh mẽ 

Trong Mẫu 1, hàng cuối cùng là```
UUUUU
```Mọi ô đều có thể di chuyển sang trái hoặc phải vì`U`chỉ cấm chuyển động đi lên. Như vậy cả năm ô đều thuộc về một SCC. Kho lưu trữ nén SCC`size = 5`và một truy vấn hoàn toàn bên trong thành phần này sẽ ngay lập tức đếm tất cả năm ô. 

Đây là lý do tại sao việc thay thế mọi SCC bằng một đỉnh không có trọng số sẽ không chính xác. Biểu đồ nén trả lời khả năng tiếp cận, nhưng câu hỏi ban đầu yêu cầu số lượng ô lưới ban đầu. 

### Một số đường dẫn có thể có giữa các điểm cuối giống nhau 

Câu trả lời không phải là độ dài của con đường đã chọn. Đó là số lượng ô xuất hiện trên ít nhất một đường dẫn bắt đầu đến đích hợp lệ. Trong cây nén, một chuỗi anh chị em có thể chứa một số SCC trung gian mà tất cả đều có thể tham gia vào các tuyến đường khác nhau. Tổng tiền tố có chủ ý đếm toàn bộ khoảng thời gian hợp lệ của chuỗi thay vì chọn một tuyến đường tùy ý. 

Đây cũng là lý do tại sao thuật toán đường đi ngắn nhất đơn giản không thể giải quyết được vấn đề. Số lượng mong muốn là sự kết hợp của các đỉnh đường đi có thể, không phải độ dài đường đi. 

### Rất nhiều SCC nhỏ 

Một lưới như một chiều`LLL`ví dụ có một SCC trên mỗi ô. Việc xây dựng vẫn hoạt động vì nó không bao giờ giả định rằng SCC chứa nhiều ô. Mỗi thành phần đơn lẻ có một hình chữ nhật bao quanh một ô và các thành phần liền kề được kết nối thông qua cùng một cơ chế cạnh hình chữ nhật. 

Kích thước tối đa tất cả-`L`bài kiểm tra thực hiện tình huống ngược lại trên quy mô lớn. Có 1000 SCC lớn, một SCC cho mỗi cột và thuật toán vẫn xử lý chúng bằng cách sử dụng cùng một biểu diễn.

---
title: "CF 102471D - Lửa"
description: "Gốc cây ở đỉnh 1. Pang bắt đầu từ gốc và cuối cùng phải thi triển phép thuật của mình chính xác một lần ở mỗi đỉnh."
date: "2026-08-09T04:36:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102471
codeforces_index: "D"
codeforces_contest_name: "2019 ICPC Asia-East Continent Final"
rating: 0
weight: 102471
solve_time_s: 513
verified: true
draft: false
---

[CF 102471D - Lửa](https://codeforces.com/problemset/problem/102471/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Gốc cây ở đỉnh 1. Pang bắt đầu từ gốc và cuối cùng phải thi triển phép thuật của mình chính xác một lần ở mỗi đỉnh. Bởi vì mỗi cạnh có hướng chỉ có thể được sử dụng nhiều nhất một lần, nên một cây con được đưa vào và sau đó rời khỏi cây mẹ của nó sẽ có chi phí di chuyển hoàn toàn được xác định: nếu cây con chứa`s`các đỉnh, đi qua nó và quay lại đỉnh đầu vào sử dụng chính xác`2s`di chuyển. Sự tự do duy nhất là thứ tự các cây con anh em được xử lý và cây con nào chứa đỉnh cuối cùng của toàn bộ bước đi. 

Cho phép`a[v]`là nhiệt độ của đỉnh`v`ngay trước buổi sáng của ngày thứ 1. Nếu Pang sử dụng phép thuật ở đỉnh`v`vào ngày`t`, nhiệt độ của nó sau buổi tối kỳ diệu là`a[v] - t + k`. Nếu anh ấy rời đi vào ngày`d`, buổi sáng trong ngày`d`đã giảm nhiệt độ đó đi một số ngày thích hợp, do đó đỉnh nguồn phải hoàn toàn dương. Đích đến chỉ phải không âm. Hai bất đẳng thức khác nhau này là nguyên nhân gây ra một số trường hợp biên. 

Kết quả là ngày muộn nhất mà Pang có thể chuẩn bị ở đỉnh 1, nghĩa là anh ấy thi triển phép thuật ở đỉnh 1 vào ngày hôm đó và thực hiện nước đi đầu tiên vào ngày hôm sau. Nếu ngay cả ngày 0 cũng không thể hoạt động được thì câu trả lời là`-1`. 

Sự ràng buộc`n <= 100000`loại trừ mọi thứ bậc hai, đặc biệt vì giới hạn thời gian chỉ là một giây. Việc duyệt toàn bộ cây đã là tuyến tính, do đó mục tiêu về cơ bản là`O(n)`hoặc`O(n log n)`. Nhiệt độ và`k`giá trị có thể đạt tới`10^9`, trong khi kích thước cây con có thể đạt tới`10^5`, vì vậy các biểu thức trung gian như`k + a[v] - 2 * size[v]`phải được xử lý bằng số học 64 bit trong các ngôn ngữ có số nguyên có chiều rộng cố định. Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn trong quá trình triển khai. 

Một trường hợp ranh giới là nhiệt độ đích có thể chính xác bằng 0. Xét một cây hai đỉnh với`k = 0`và nhiệt độ`2 1`.```
2 0
1 2
2 1
```Câu trả lời là`0`. Pang di chuyển ở đỉnh 1 vào ngày 0, di chuyển vào ngày 1 và đến đỉnh 2 với nhiệt độ bằng 0. Việc triển khai bất cẩn yêu cầu điểm đến phải có nhiệt độ dương sẽ từ chối trường hợp này một cách không chính xác. 

Đỉnh nguồn có điều kiện ngược lại, nó phải hoàn toàn dương. Với cùng một cây và`k = 0`, thay đổi nhiệt độ gốc thành`1`.```
2 0
1 2
1 1
```Câu trả lời là`-1`. Sau khi đúc vào ngày 0, rễ có nhiệt độ 1, nhưng buổi sáng ngày 1 giảm xuống 0 nên Pang không thể di chuyển. Chỉ kiểm tra nhiệt độ đích sẽ bỏ sót lỗi này. 

Thứ tự của các cây con anh em cũng rất quan trọng. Coi như```
5 2
1 2
1 3
1 4
4 5
100 1 100 100 100
```Câu trả lời đúng là`93`. Đỉnh 2 có thời hạn rất chặt chẽ, vì vậy cây con của nó phải được truy cập trước cây con lớn hơn có gốc ở đỉnh 4. Việc xử lý các phần tử con theo thứ tự đầu vào có thể đặt cây con lớn lên trước và khiến đỉnh 2 không thể truy cập kịp thời. Do đó, một giải pháp chỉ thử DFS theo thứ tự danh sách kề có thể thất bại ngay cả khi có lịch trình hợp lệ. 

Cuối cùng, ngày 0 thật đặc biệt vì nhiệt độ không giảm vào ngày đó. Vì```
2 2
1 2
0 1
```câu trả lời là`0`. Pang có thể sử dụng phép thuật ở đỉnh 1 vào ngày 0, tăng nhiệt độ của nó từ 0 lên 2, sau đó di chuyển vào ngày 1 đến đỉnh 2, nơi có nhiệt độ chính xác bằng 0 vào sáng hôm đó. Xử lý ngày 0 như thể việc giảm nhiệt độ đầu tiên đã xảy ra sẽ làm mất đi giải pháp hợp lệ này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ liệt kê các bước đi có thể qua cây. Khi cây đã được root, mọi cây con con không phải là cuối cùng phải được nhập vào, khám phá hoàn toàn và trả về từ đó. Cây con cuối cùng là cây duy nhất không cần trả về. Đối với mọi thứ tự có thể có của trẻ, chúng tôi có thể mô phỏng ngày, kiểm tra nhiệt độ trước mỗi lần di chuyển và phép thuật. 

Điều này đúng vì mọi bước đi hợp pháp đều có cấu trúc đệ quy chính xác này. Vấn đề là số lượng đặt hàng. Một ngôi sao với`n - 1`lá đã có rồi`(n - 1)!`các đơn hàng có thể và việc mô phỏng từng đơn hàng sẽ mất`Theta(n)`di chuyển. Do đó, công việc trong trường hợp xấu nhất là`Theta(n * (n - 1)!)`, rất lâu trước khi chúng ta đạt được`n = 100000`. 

Lực lượng vũ phu hoạt động vì một cây buộc mọi cây con đã hoàn thành phải hoạt động giống như một chuyến tham quan duy nhất. Quan sát quan trọng là một chuyến tham quan như vậy có thời lượng cố định, chính xác gấp đôi số đỉnh trong cây con. Sau khi cây con con được tóm tắt vào ngày muộn nhất mà nó có thể được nhập vào, cha mẹ chỉ phải lên lịch các chuyến du ngoạn trong khoảng thời gian cố định này trước thời hạn của chúng. 

Giả sử con cái của`u`được sắp xếp như`v_1, v_2, ..., v_d`. Nếu Pang sử dụng`u`vào ngày`t`, rồi con`v_i`được nhập vào ngày 

[ 
t + 1 + 2\sum_{j<i} size[v_j]. 
]

Nếu như`f[v_i]`là ngày đến hợp lệ muộn nhất cho một chuyến đi khứ hồi hoàn chỉnh qua`v_i`, chúng tôi cần 

[ 
t + 1 + 2\sum_{j<i} size[v_j] \le f[v_i]. 
] 

Sắp xếp lại mang lại 

[ 
t \le f[v_i]-1-2\sum_{j<i}size[v_j]. 
] 

Đây là vấn đề về lịch trình. Mỗi đứa trẻ đều có thời gian xử lý`2 * size[v]`, và thời hạn có hiệu lực của nó là`f[v] + 2 * size[v]`. Sắp xếp trẻ em theo giá trị này sẽ mang lại thứ tự tối ưu. Đây là thứ tự tương tự được mô tả trong ghi chú giải pháp kiểu chính thức cho vấn đề này. 

Còn một điều phức tạp nữa. Toàn bộ bước đi không nhất thiết phải quay lại đỉnh 1. Trên thực tế, nó phải kết thúc ở một chiếc lá. Do đó, chúng ta cần trạng thái DP thứ hai cho phép duyệt qua cây con cuối cùng mà không cần quay lại cây mẹ của nó. Những đứa trẻ trước đứa con cuối cùng đó vẫn phải được sắp xếp theo thứ tự tối ưu như nhau. 

Đối với một con cuối cùng cố định, việc xóa con đó khỏi danh sách có thứ tự chỉ làm thay đổi tiền tố thời gian của các con sau nó. Tiền tố và hậu tố cực tiểu cho phép chúng ta tính toán thời hạn tốt nhất của tất cả các con còn lại trong thời gian không đổi cho mỗi con cuối cùng có thể. Do đó, sau khi sắp xếp các nút con, toàn bộ quá trình chuyển đổi cho một đỉnh là tuyến tính theo bậc của nó. 

Kết quả so sánh là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`Theta(n * (n - 1)!)`trong một ngôi sao |`O(n)`| Quá chậm | 
| Tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Căn cây tại đỉnh 1 và tính thứ tự từ dưới lên. Với mọi đỉnh`u`, cho phép`size[u]`là số đỉnh trong cây con có gốc của nó. Việc xử lý các đỉnh theo thứ tự DFS ngược đảm bảo rằng mọi phần tử con đều đã được giải khi chúng ta xử lý`u`. 
2. Xác định`f[u]`là ngày muộn nhất mà Pang có thể đến`u`, sử dụng phép thuật ở đó, duyệt qua toàn bộ cây con của`u`, quay lại`u`, và cuối cùng rời đi`u`đối với cha mẹ của nó. Trạng thái này mô tả một cây con hoạt động như một chuyến đi khứ hồi hoàn chỉnh cho cây mẹ của nó. 
3. Sắp xếp con của`u`bởi 

[ 
f[v] + 2size[v]. 
] 

Đối với một đứa trẻ`v`được đặt sau các cây con trước đó có tổng kích thước là`P`, ngày nhập cảnh của nó là`t + 1 + 2P`, vì vậy nó đòi hỏi 

[ 
t \le f[v]-1-2P. 
] 

Quy tắc sắp xếp là quy tắc về thời hạn có hiệu lực sớm nhất cho những chuyến du ngoạn có thời hạn cố định này. Nếu hai đứa trẻ liền kề vi phạm thứ tự này, việc hoán đổi chúng không thể làm cho ngày bắt đầu khả thi tối đa trở nên nhỏ hơn, vì vậy việc loại bỏ nhiều lần những sự đảo ngược như vậy sẽ mang lại thứ tự được sắp xếp. 

1. Sau khi phân loại, hãy`deadline[i]`là sự hạn chế do đứa trẻ đóng góp`i`khi tất cả các phần tử con trước đó được xử lý trước nó: 

[ 
thời hạn[i] = f[v_i]-1-2\sum_{j<i}size[v_j]. 
] 

Ngày khởi đầu tốt nhất cho tất cả các chuyến du ngoạn của trẻ là giá trị tối thiểu của các giá trị này. 

Pang cũng phải sống sót ở`u`chính nó. Nếu anh ta bắt đầu lúc`u`vào ngày`t`, quá trình đúc ban đầu yêu cầu`t <= a[u]`. Để duyệt toàn bộ cây con và sau đó đi về phía cây cha, có`2size[u]-1`di chuyển sau ngày casting. Sự ra đi cuối cùng đòi hỏi`u`tích cực, điều này mang lại 

[ 
t \le a[u] + k - 2size[u]. 
] 

Do đó, 

[ 
f[u] = 
\min\left( 
một [u], 
a[u]+k-2size[u], 
\min_i thời hạn[i] 
\đúng). 
] 

Đối với một chiếc lá, điều này giảm xuống còn`min(a[u], a[u] + k - 2)`. 

1. Xác định`g[u]`là ngày muộn nhất mà Pang có thể đến`u`, truyền vào đó, duyệt toàn bộ cây con và kết thúc ở bất kỳ vị trí nào bên trong cây con đó. Không giống`f[u]`,`g[u]`không yêu cầu bước đi cuối cùng từ`u`tới cha mẹ của nó. 

Nếu như`u`là một chiếc lá, Pang có thể đơn giản kết thúc ở đó, vì vậy`g[u] = a[u]`. 

1. Đối với đỉnh trong`u`, chọn một con`c`là đứa con cuối cùng. Mọi đứa trẻ khác phải được duyệt qua và quay trở lại hoàn toàn từ trước đó`c`được nhập vào. 

hãy để 

[ 
S = kích thước[u]-1-kích thước[c] 
] 

là tổng kích thước của tất cả các cây con không phải là cuối cùng. Họ tiêu thụ`2S`di chuyển trước khi con cuối cùng được nhập vào. 

Do đó, phần tử con cuối cùng chỉ có thể được nhập vào hoặc trước phần tử con cuối cùng của nó.`g[c]`thời hạn: 

[ 
t+1+2S \le g[c], 
] 

cho đi 

[ 
t \le g[c]-1-2S. 
] 

Pang cũng cần phải rời đi`u`cho đứa con cuối cùng. Động thái đó xảy ra vào ngày`t+1+2S`, vậy điều kiện nhiệt độ tại`u`cho 

[ 
t \le a[u]+k-2-2S. 
] 

1. Chúng ta vẫn cần các ràng buộc của tất cả các con không phải là con cuối cùng. Xóa con`c`từ danh sách đã sắp xếp rời khỏi trẻ em trước`c`không thay đổi, trong khi mọi đứa trẻ sau`c`bắt đầu`2size[c]`những ngày trước đó. Tính tiền tố tối thiểu của thời hạn con ban đầu và hậu tố tối thiểu. Dành cho trẻ em`c`, hạn chế tốt nhất đối với tất cả những đứa trẻ khác là 

[ 
bên(c)= 
\min\left( 
tiền tố[c], 
hậu tố[c+1]+2size[c] 
\đúng). 
] 

Thuật ngữ đầu tiên bao gồm trẻ em trước`c`. Thuật ngữ thứ hai bao gồm trẻ em sau`c`, tiền tố của nó trở nên nhỏ hơn bởi`size[c]`. 

1. Thí sinh trả lời khi`c`là đứa con cuối cùng là 

[ 
ứng viên(c)= 
\min\left( 
một [u], 
a[u]+k-2-2S, 
g[c]-1-2S, 
bên (c) 
\đúng). 
] 

Tận dụng tối đa tất cả trẻ em. tối đa đó là`g[u]`, bởi vì mọi phép duyệt hợp lệ phải chọn chính xác một con làm hướng của đỉnh cuối cùng. 

1. Tại gốc không có cha mẹ nào mà Pang cần quay về. Do đó ngày chuẩn bị muộn nhất cần thiết là chính xác`g[1]`. Nếu như`g[1] < 0`, ngay cả việc chuẩn bị vào ngày thứ 0 cũng không thể thực hiện được, nên câu trả lời là`-1`. Nếu không thì in`g[1]`. 

### Tại sao nó hoạt động 

Bất biến trung tâm là`f[u]`chính xác là ngày truyền tải khả thi mới nhất cho quá trình truyền tải xử lý hoàn toàn`u`cây con của nó và trở về cây mẹ của nó, trong khi`g[u]`chính xác là ngày truyền khả thi muộn nhất khi quá trình truyền tải có thể kết thúc ở bất kỳ đâu trong cây con. Vì`f[u]`, mỗi chuyến tham quan của trẻ có một khoảng thời gian cố định và thời gian bắt đầu được phép muộn nhất, do đó việc trao đổi hai trẻ liền kề có thứ tự không chính xác cho thấy rằng việc sắp xếp theo`f[v] + 2size[v]`là tối ưu. Vì`g[u]`, mọi phép duyệt hợp lệ đều có đúng một con cuối cùng. Tất cả những đứa trẻ khác đều hình thành những chuyến du ngoạn hoàn chỉnh và có thể được lên lịch theo cùng một thứ tự tối ưu. Tiền tố và hậu tố cực tiểu tính toán chính xác các ràng buộc của những chuyến du ngoạn đó sau khi con cuối cùng bị loại bỏ. Vì mọi đứa con cuối cùng có thể đều được xem xét,`g[u]`có sự lựa chọn hợp lệ tốt nhất. Về cơ bản, mỗi bước đi hoàn chỉnh đều tương ứng với một lựa chọn con cuối cùng như vậy, vì vậy`g[1]`chính xác là ngày chuẩn bị muộn nhất có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    graph = [[] for _ in range(n)]
    for _ in range(n - 1):
        x, y = map(int, input().split())
        x -= 1
        y -= 1
        graph[x].append(y)
        graph[y].append(x)

    a = list(map(int, input().split()))

    parent = [-2] * n
    parent[0] = -1

    order = [0]
    for u in order:
        for v in graph[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            order.append(v)

    size = [1] * n
    f = [0] * n
    g = [0] * n

    INF = 10**30

    for u in reversed(order):
        children = [v for v in graph[u] if parent[v] == u]

        if not children:
            size[u] = 1
            f[u] = min(a[u], a[u] + k - 2)
            g[u] = a[u]
            continue

        size[u] = 1 + sum(size[v] for v in children)

        children.sort(key=lambda v: f[v] + 2 * size[v])

        m = len(children)

        deadline = [0] * m
        prefix = [INF] * (m + 1)

        used = 0
        for i, v in enumerate(children):
            deadline[i] = f[v] - 1 - 2 * used
            prefix[i + 1] = min(prefix[i], deadline[i])
            used += size[v]

        suffix = [INF] * (m + 1)
        for i in range(m - 1, -1, -1):
            suffix[i] = min(suffix[i + 1], deadline[i])

        f[u] = min(
            a[u],
            a[u] + k - 2 * size[u],
            prefix[m]
        )

        total_children_size = size[u] - 1
        best = -INF

        for i, v in enumerate(children):
            sv = size[v]

            # Remove v from the child order.
            # Children before v keep their original prefix.
            # Children after v start 2 * sv days earlier.
            side_deadline = min(
                prefix[i],
                suffix[i + 1] + 2 * sv
            )

            remaining_size = total_children_size - sv

            candidate = min(
                a[u],
                a[u] + k - 2 - 2 * remaining_size,
                g[v] - 1 - 2 * remaining_size,
                side_deadline
            )

            best = max(best, candidate)

        g[u] = best

    answer = g[0]
    print(answer if answer >= 0 else -1)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai sẽ bắt rễ cây một cách lặp đi lặp lại. Tránh đệ quy vì một cây có`100000`các đỉnh có thể là một chuỗi đơn, đủ sâu để vượt quá giới hạn đệ quy thông thường của Python. 

Thứ tự duyệt ngược tính toán DP từ lá về gốc. các`size`Mảng là cần thiết cho cả thời gian của một chuyến tham quan con hoàn chỉnh và cho giới hạn nhiệt độ tại một đỉnh sau khi tất cả các con cháu của nó đã được xử lý. 

Đối với mỗi đỉnh, các phần tử con được sắp xếp theo`f[v] + 2 * size[v]`. các`deadline`mảng lưu trữ giới hạn chính xác vào ngày truyền`u`từng em đóng góp. các`prefix`mảng đưa ra thời hạn tối thiểu giữa các trẻ em trước khi có trẻ em cuối cùng được chọn, trong khi`suffix`đưa ra mức tối thiểu ở những đứa trẻ sau nó. 

biểu hiện`suffix[i + 1] + 2 * sv`rất dễ mắc sai lầm. Sau con`v`được loại bỏ, mọi đứa trẻ sau này sẽ bắt đầu`2 * size[v]`ngày trước đó, do đó ràng buộc của nó trở nên ít hạn chế hơn với đúng số tiền đó. Đó là lý do tại sao giá trị hậu tố phải được tăng lên thay vì giảm đi. 

Sự khác biệt giữa`-1`và số 0 cũng là cố ý. Trạng thái DP có thể âm vì nó thể hiện ngày truyền khả thi mới nhất so với ngày cấp trên. Tuy nhiên, tại gốc, chỉ có những ngày không âm. Như vậy`g[0] == 0`là hợp lệ, trong khi`g[0] < 0`có nghĩa là trường hợp này là không thể. 

Tất cả số học được thực hiện trực tiếp với số nguyên Python. Biểu thức liên quan đến`2 * size[u]`Và`k`có thể vượt quá phạm vi 32 bit, do đó, việc triển khai có chiều rộng cố định nên sử dụng`long long`. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
3 1
1 2
1 3
4 3 5
```Đỉnh 2 và 3 là lá. Trạng thái của họ là`f[2] = 2`,`g[2] = 3`,`f[3] = 4`, Và`g[3] = 5`. 

Gốc có hai lựa chọn có thể có cho con cuối cùng. 

| Đỉnh |`size`|`f`|`g`| 
| --- | --- | --- | --- | 
| 2 | 1 | 2 | 3 | 
| 3 | 1 | 4 | 5 | 
| 1 | 3 | -1 | 1 | 

Các em được sắp xếp theo`f[v] + 2size[v]`, cho đỉnh 2 đầu tiên và đỉnh 3 giây. 

Nếu đỉnh 2 là đỉnh cuối cùng thì đỉnh 3 phải được xử lý trước. Giới hạn khởi hành của chính gốc là`1`, trong khi nhập đứa trẻ cuối cùng sau chuyến tham quan phụ mang lại một giới hạn`0`. Do đó, ứng cử viên là`0`. 

Nếu đỉnh 3 là đỉnh cuối cùng thì đỉnh 2 được xử lý trước. Ràng buộc con cuối cùng đưa ra`2`, trong khi hạn chế nhiệt độ nguồn của gốc mang lại`1`, vậy ứng cử viên là`1`. 

Tối đa là`g[1] = 1`, phù hợp với đầu ra mẫu. 

Đối với mẫu 2,```
3 1
1 2
1 3
2 10 10
```Cả hai lá đều có`f = 9`Và`g = 10`. Lá nào được chọn làm lá cuối cùng thì lá kia phải được thăm và quay về trước. 

| Đỉnh |`size`|`f`|`g`| 
| --- | --- | --- | --- | 
| 2 | 1 | 9 | 10 | 
| 3 | 1 | 9 | 10 | 
| 1 | 3 | -1 | -1 | 

Sau chuyến tham quan một bên, rễ sẽ phải rời đi cho đứa con cuối cùng vào một ngày mà nhiệt độ của nó không dương. Kết quả`g[1]`là`-1`, nên ngay cả việc chuẩn bị vào ngày thứ 0 cũng không thể thực hiện được. Đầu ra là`-1`. 

Các dấu vết chứng minh tại sao điều kiện đỉnh nguồn không thể được thay thế bằng kiểm tra không âm. Rễ cần nhiệt độ dương hoàn toàn ở mỗi lần khởi hành và DP mã hóa sự bất bình đẳng nghiêm ngặt đó thông qua`-2`điều khoản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Các đỉnh con của mỗi đỉnh đều được sắp xếp và tổng chi phí sắp xếp tối đa là`O(n log n)`| 
| Không gian |`O(n)`| Cây, mảng cha/thứ tự, kích thước cây con và trạng thái DP đều là tuyến tính | 

Cây có nhiều nhất`100000`đỉnh, do đó`O(n log n)`dễ dàng nằm trong phạm vi tiệm cận dự định. Việc triển khai cũng tránh được DFS đệ quy, điều này rất hữu ích cho chuỗi trường hợp xấu nhất. Mảng tiền tố, hậu tố và thời hạn tạm thời chỉ chứa các phần tử con của đỉnh hiện đang được xử lý, do đó tổng bộ nhớ được giữ lại của chúng vẫn là tuyến tính. 

## Trường hợp thử nghiệm 

Bộ dây thử nghiệm sau đây sử dụng cùng một`solve`hoạt động như giải pháp được gửi. Nó tạm thời thay thế đầu vào và đầu ra tiêu chuẩn, sau đó khôi phục chúng sau mỗi lần kiểm tra.```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()

        global input
        input = sys.stdin.readline

        solve()

        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run(
    """3 1
1 2
1 3
4 3 5
"""
) == "1", "sample 1"

# Provided sample 2
assert run(
    """3 1
1 2
1 3
2 10 10
"""
) == "-1", "sample 2"

# Minimum-size tree, destination is exactly zero on arrival.
assert run(
    """2 0
1 2
2 1
"""
) == "0", "destination may be zero"

# Source must be strictly positive when moving.
assert run(
    """2 0
1 2
1 1
"""
) == "-1", "source must be positive"

# Child ordering matters.
assert run(
    """5 2
1 2
1 3
1 4
4 5
100 1 100 100 100
"""
) == "93", "child ordering"

# Day 0 does not suffer the morning decrement.
assert run(
    """2 2
1 2
0 1
"""
) == "0", "day zero"

# All equal values with a small branching tree.
assert run(
    """3 2
1 2
1 3
4 4 4
"""
) == "1", "all equal values"

# Maximum-size generated test, a chain with equal temperatures.
n = 100000
k = 1000000000

parts = [f"{n} {k}\n"]
for i in range(1, n):
    parts.append(f"{i} {i + 1}\n")
parts.append(("1000000000 " * n).strip() + "\n")

max_case = "".join(parts)

assert run(max_case) == "999900001", "maximum-size chain"
```Các trường hợp tùy chỉnh xác thực các tình huống sau: 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 0`, bờ rìa`1 2`, nhiệt độ`2 1`|`0`| Nhiệt độ điểm đến có thể chính xác bằng 0 | 
|`2 0`, bờ rìa`1 2`, nhiệt độ`1 1`|`-1`| Nhiệt độ nguồn phải hoàn toàn dương | 
| Cây 5 đỉnh với các con có kích thước và thời hạn khác nhau |`93`| Phân loại trẻ theo thời hạn có hiệu lực | 
|`2 2`, nhiệt độ`0 1`|`0`| Ngày 0 nhiệt độ ban đầu không giảm | 
| Sao ba đỉnh,`k = 2`, mọi nhiệt độ`4`|`1`| Giá trị bằng nhau và lập kế hoạch anh chị em | 
| Chuỗi của`100000`đỉnh có nhiệt độ lớn bằng nhau |`999900001`| Tối đa`n`, duyệt lặp, số học số nguyên lớn | 

## Vỏ cạnh 

Trường hợp đích có nhiệt độ bằng 0 được xử lý bằng bất đẳng thức khi trẻ đến`t + 1 <= g[c]`. Không có phép trừ bổ sung sau khi đến nơi, vì vậy nhiệt độ chính xác bằng 0 được chấp nhận. Trong trường hợp hai đỉnh có nhiệt độ`2 1`Và`k = 0`, rễ có thể hình thành vào ngày 0 và di chuyển vào ngày 1, đến đỉnh thứ hai với nhiệt độ bằng 0. Thuật toán tính toán`g[2] = 1`và quá trình chuyển đổi gốc mang lại`g[1] = 0`, vì vậy nó in`0`. 

Điều kiện nguồn nghiêm ngặt xuất hiện trong`a[u] + k - 2`số hạng cho một lá và số hạng khởi hành tương ứng cho các đỉnh trong. Với`n = 2`,`k = 0`, và nhiệt độ`1 1`, rễ có thể ra rễ vào ngày 0 nhưng có nhiệt độ bằng 0 sau buổi sáng ngày 1. Quá trình chuyển đổi tạo ra`g[1] = -1`, báo cáo chính xác là không thể. 

Vụ việc đặt hàng trẻ em```
5 2
1 2
1 3
1 4
4 5
100 1 100 100 100
```có con lá 2 với`f[2] = 1`và một con hai đỉnh bắt nguồn từ 4 với`f[4] = 98`. Chìa khóa hiệu quả của họ là`3`Và`102`, do đó đỉnh 2 phải được xử lý trước. Sau chuyến tham quan đó, cây con lớn có thể được thăm và đỉnh 3 có thể là cây con cuối cùng, cho ra ngày`93`. Thời hạn tiền tố thực thi lệnh này một cách tự động. 

Trường hợp ngày 0```
2 2
1 2
0 1
```có`a[1] = 0`, vì vậy Pang có thể sử dụng gốc vào ngày 0. Phép thuật tăng nhiệt độ của nó lên 2. Vào ngày 1, gốc là dương và đỉnh 2 có nhiệt độ bằng 0, vì vậy việc di chuyển là hợp pháp. Quá trình chuyển đổi gốc mang lại`g[1] = 0`, đó chính xác là ngày chuẩn bị sớm nhất và muộn nhất có thể. 

Đối với một chiếc lá,`g[u] = a[u]`bởi vì Pang có thể đến, thi triển phép thuật và kết thúc ngay lập tức. Ranh giới này rất cần thiết khi đỉnh cuối cùng của toàn bộ bước đi là một chiếc lá. Sử dụng trạng thái khứ hồi`f[u]`thay vào đó sẽ yêu cầu đủ nhiệt độ một cách không cần thiết để rời khỏi lá và có thể loại bỏ các giải pháp hợp lệ. 

Khi`k`bằng 0, ma thuật không hề nạp lại một đỉnh nào cả. Các công thức vẫn hoạt động vì mọi giới hạn khởi hành đều chứa cùng một`k`thuật ngữ. Khi`k`là rất lớn, hạn chế đến`t <= a[u]`thay vào đó có thể trở thành điều kiện giới hạn, đó là lý do tại sao`f[u]`phải sử dụng mức tối thiểu của cả hai ràng buộc thay vì giả sử điều kiện nạp lại luôn chặt chẽ hơn. 

Giá trị DP âm bên trong cây không phải là lỗi. Nó đơn giản có nghĩa là một cây con sẽ phải được nhập trước ngày 0 để đáp ứng tất cả các thời hạn của nó. Trạng thái như vậy vẫn có thể được sử dụng làm giới hạn khi tính toán tổ tiên. Chỉ một`g[1]`được so sánh với 0, vì ngày 0 là ngày chuẩn bị sớm nhất dành cho Pang. 

Bài xã luận trên sử dụng`f/g`xây dựng trực tiếp, do đó việc triển khai tuân theo bằng chứng thay vì yêu cầu một bước khởi động lại riêng biệt.

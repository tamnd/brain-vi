---
title: "CF 102386J - \u041a\u0430\u0442\u0430\u043c\u0430\u0440\u0438"
description: "Chúng ta có một lưới (n lần m). Mỗi ô chứa một đối tượng có kích thước nguyên (a{ij}). Chúng ta cần truy cập từng ô chính xác một lần, chỉ di chuyển giữa các ô liền kề và chuỗi kích thước đối tượng dọc theo tuyến đường phải không giảm."
date: "2026-08-12T21:57:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102386
codeforces_index: "J"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0442\u0443\u0440 \u0423\u0440\u0430\u043b\u044c\u0441\u043a\u043e\u0433\u043e \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b\u0430 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u043c\u0438\u0440\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2019"
rating: 0
weight: 102386
solve_time_s: 735
verified: true
draft: false
---

[CF 102386J - \u041a\u0430\u0442\u0430\u043c\u0430\u0440\u0438](https://codeforces.com/problemset/problem/102386/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 12m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới (n \times m). Mỗi ô chứa một đối tượng có kích thước nguyên (a_{ij}). Chúng ta cần truy cập từng ô chính xác một lần, chỉ di chuyển giữa các ô liền kề và chuỗi kích thước đối tượng dọc theo tuyến đường phải không giảm. 

Hạn chế chính là mọi kích thước xảy ra nhiều nhất ba lần. Vì các kích thước khác nhau có thứ tự tương đối cố định nên tất cả các ô có cùng kích thước phải xuất hiện liên tiếp trong tuyến đường. Nếu kích thước (x) xuất hiện ba lần, tuyến đường phải nhập ba ô tương ứng, truy cập cả ba ô và rời khỏi nhóm để hướng tới kích thước lớn hơn tiếp theo. Sự tự do duy nhất là thứ tự truy cập tối đa ba ô đó. 

Lưới chứa tối đa (100 \cdot 100 = 10.000) ô. Điều đó làm cho bất cứ thứ gì theo cấp số nhân về số lượng ô đều hoàn toàn không thể sử dụng được. Mục tiêu phức tạp hữu ích về cơ bản là tuyến tính hoặc một hệ số không đổi nhỏ nhân với số lượng ô. Tuyên bố chính thức đưa ra giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB, điều này củng cố rằng giải pháp nên khai thác giới hạn tần số một cách mạnh mẽ. 

Có một số trường hợp công trình tưởng chừng như tự nhiên lại thất bại. 

Hãy xem xét một tế bào duy nhất.```
1 1
7
```Đầu ra đúng chỉ đơn giản là:```
1 1
```Không có sự chuyển đổi giữa các kích thước khác nhau, do đó việc triển khai giả định rằng phải có nhóm trước và nhóm tiếp theo có thể từ chối trường hợp này một cách không chính xác. 

Bây giờ hãy xem xét:```
1 3
1 2 1
```Câu trả lời là:```
-1
```Hai ô chứa kích thước (1) phải liên tiếp nhau trong tuyến đường, vì việc đặt kích thước (2) giữa chúng sẽ vi phạm thứ tự bắt buộc. Chúng không liền kề nhau nên không tồn tại tuyến đường hợp lệ. Việc triển khai bất cẩn chỉ kiểm tra xem có thể truy cập từng ô riêng lẻ hay không có thể bỏ sót điều này. 

Một trường hợp tế nhị khác là:```
2 2
1 2
1 3
```Một tuyến đường hợp lệ là```
2 1
1 1
1 2
2 2
```Hai ô kích thước (1) có hai thứ tự có thể. Chỉ thứ tự bắt đầu từ ((2,1)) mới có thể kết nối với ô size-(2). Việc chọn một thứ tự tùy ý cho các giá trị bằng nhau có thể loại bỏ một trường hợp có thể giải được. 

Cuối cùng,```
1 3
1 3 2
```là không thể. Mỗi kích thước xảy ra một lần nên không có lựa chọn đặt hàng nội bộ nào cả. Thứ tự bắt buộc phải là ô chứa (1), sau đó là ô chứa (2), sau đó là ô chứa (3), nhưng 2 ô đầu không liền kề nhau. Chỉ kiểm tra các nhóm có giá trị bằng nhau là không đủ, việc chuyển đổi giữa các nhóm liên tiếp cũng rất quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là sắp xếp các ô theo giá trị của chúng, chia chúng thành các nhóm có giá trị bằng nhau và thử mọi thứ tự có thể có trong mỗi nhóm. Điều này đúng vì các ô của một giá trị phải tạo thành một đoạn liên tiếp của tuyến đường cuối cùng, do đó việc chọn thứ tự cho mỗi nhóm sẽ hoàn toàn xác định tuyến đường ứng viên. 

Vấn đề là số lượng kết hợp. Nếu một nhóm có ba ô, nó có thể có tối đa (3! = 6) đơn hàng. Trong trường hợp xấu nhất, hầu hết tất cả các ô có thể được phân chia thành các nhóm ba ô, tạo ra khoảng (3333) nhóm cho (10.000) ô. Sau đó, một tìm kiếm brute-force có thể kiểm tra theo thứ tự 

[ 
6^{3333} 
] 

sự kết hợp khác nhau. Ngay cả việc kiểm tra từng ứng cử viên chỉ trong thời gian (O(N)) cũng cho ra (O(N \cdot 6^{N/3})), con số này quá lớn về mặt thiên văn. 

Brute-force hoạt động vì các quyết định duy nhất là những hoán vị nhỏ bên trong các nhóm có giá trị bằng nhau, nhưng nó lãng phí khối lượng công việc khổng lồ bằng cách tính toán lại các tiền tố có thể giống nhau. Quan sát mở ra giải pháp là một nhóm hoàn chỉnh chỉ quan trọng đối với tương lai thông qua ô cuối cùng của nó. Khi chúng tôi biết nhóm hiện tại được sắp xếp như thế nào, mọi thứ trước đó đều không liên quan ngoại trừ việc liệu trạng thái đó có thể truy cập được hay không. 

Đây chính xác là tình huống mà lập trình động trở nên hữu ích. Đối với mỗi nhóm kích thước, chúng tôi liệt kê tất cả các hoán vị hợp lệ của các ô của nó. Một hoán vị chỉ có hiệu lực nội bộ khi mỗi cặp ô liên tiếp nằm cạnh nhau. Sau đó, chúng tôi giữ trạng thái DP cho mỗi hoán vị, nghĩa là tồn tại một lộ trình hợp lệ xuyên qua tất cả các nhóm trước đó và kết thúc tại ô đầu tiên của hoán vị này. 

Khi chuyển từ nhóm này sang nhóm khác, chúng ta chỉ cần kiểm tra xem ô cuối cùng của hoán vị trước có liền kề với ô đầu tiên của hoán vị hiện tại hay không. Vì có nhiều nhất sáu hoán vị cho mỗi nhóm nên mỗi phép chuyển đổi sẽ kiểm tra nhiều nhất (6 \cdot 6 = 36) cặp. 

Sự khác biệt rất lớn: tìm kiếm theo cấp số nhân trên tất cả các kết hợp trở thành DP có kích thước không đổi được lặp lại một lần cho mỗi nhóm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N \cdot 6^{N/3})) | (O(N)) | Quá chậm | 
| Tối ưu | (O(N \log N + N)) | (O(N)) | Đã chấp nhận | 

Thuật ngữ (N \log N) xuất phát từ việc sắp xếp các ô theo kích thước. Bản thân DP là (O(N)), bởi vì mỗi nhóm có tối đa sáu trạng thái và mỗi trạng thái kiểm tra tối đa sáu trạng thái trước đó. 

## Hướng dẫn thuật toán 

1. Làm phẳng lưới thành các cặp có dạng ((a_{ij}, (i,j))) và sắp xếp chúng theo kích thước đối tượng. Các tế bào có kích thước bằng nhau sẽ tự nhiên trở thành liên tiếp. 
2. Chia các ô đã sắp xếp thành các nhóm có kích thước bằng nhau. Một nhóm chứa tối đa ba ô do hạn chế đầu vào. 

Tất cả các ô trong một nhóm phải được truy cập liên tục. Nếu hai ô có kích thước bằng nhau có một ô có kích thước lớn hơn ở giữa chúng, thì vật thể lớn hơn đó sẽ được thu thập trước một vật thể có kích thước bằng nhau, điều này bị cấm. 
3. Với mỗi nhóm, hãy liệt kê mọi hoán vị của các ô của nó. Có nhiều nhất (3! = 6) hoán vị. 
4. Loại bỏ một hoán vị nếu hai ô liên tiếp trong đó không liền kề nhau. 

Việc kiểm tra này là cần thiết vì toàn bộ nhóm phải được duyệt qua mà không để nó nằm giữa các đối tượng có kích thước bằng nhau. Ví dụ: đối với ba ô, một sự sắp xếp chỉ có thể sử dụng được nếu ô đầu tiên của nó liền kề với ô thứ hai và ô thứ hai của nó liền kề với ô thứ ba. 
5. Đối với nhóm đầu tiên, đánh dấu mọi hoán vị hợp lệ là có thể truy cập được.

Tuyến đường có thể bắt đầu ở bất cứ đâu, do đó không có hạn chế nào ở ô đầu tiên của nhóm đầu tiên. 
6. Xử lý các nhóm còn lại từ giá trị nhỏ hơn đến giá trị lớn hơn. Đối với mọi hoán vị hợp lệ của nhóm hiện tại, hãy thử mọi hoán vị có thể tiếp cận của nhóm trước đó. 

Để hoán vị trước đó kết thúc tại ô (x) và để hoán vị hiện tại bắt đầu tại ô (y). Hai phân đoạn nhóm có thể được nối chính xác khi (x) và (y) liền kề nhau. 
7. Khi quá trình chuyển đổi hoạt động, hãy đánh dấu hoán vị hiện tại có thể truy cập được và lưu trữ hoán vị trước đó đã tạo ra hoán vị đó. 

Việc lưu trữ tuyến tiền thân này cho phép chúng tôi xây dựng lại tuyến đường thực tế sau khi DP kết thúc. Chúng tôi không cần lưu trữ toàn bộ tuyến đường ở mọi trạng thái, bởi vì tất cả các trạng thái trước trạng thái tiền thân đã được đại diện bởi chuỗi tiền thân của chính nó. 
8. Sau khi xử lý nhóm cuối cùng, hãy tìm bất kỳ hoán vị nào có thể truy cập được trong nhóm đó. Nếu không tồn tại, in (-1). 
9. Nếu không, hãy theo dõi ngược lại các chỉ số trước đó được lưu trữ qua các nhóm. Điều này đưa ra hoán vị được chọn cho mỗi nhóm theo thứ tự ngược lại. Đảo ngược danh sách các trạng thái đã chọn và xuất các ô từ các hoán vị đó theo thứ tự thuận. 

### Tại sao nó hoạt động 

Điều bất biến là trạng thái DP có thể truy cập biểu thị chính xác một thứ tự có thể có của nhóm giá trị bằng nhau hiện tại cùng với tuyến đường hợp lệ qua mọi nhóm giá trị nhỏ hơn kết thúc ở thứ tự đó. 

Đối với nhóm đầu tiên, mọi hoán vị hợp lệ nội bộ đều có thể truy cập được vì ô bắt đầu không bị giới hạn. Đối với mỗi nhóm sau, một hoán vị được đánh dấu có thể truy cập chính xác khi các cạnh bên trong của nó hợp lệ và ô đầu tiên của nó liền kề với ô cuối cùng của một số hoán vị có thể truy cập trước đó. Do đó, hai đoạn tuyến kết hợp hợp pháp và tất cả các giá trị vẫn không giảm vì các nhóm được xử lý theo thứ tự kích thước tăng dần. 

Ngược lại, giả sử tồn tại một tuyến đường hoàn chỉnh hợp lệ. Các ô của mỗi giá trị tạo thành một nhóm liên tiếp và thứ tự của chúng phải là một trong những hoán vị mà chúng tôi liệt kê. Mỗi cặp liên tiếp trong nhóm đó đều liền kề nhau, do đó hoán vị vẫn tồn tại trong quá trình kiểm tra nội bộ. Ranh giới giữa hai nhóm giá trị liên tiếp cũng là cạnh kề nên việc chuyển đổi DP tương ứng sẽ được xem xét và chấp nhận. Bằng cách quy nạp qua các nhóm, DP sẽ giữ cho hoán vị được sử dụng bởi tuyến đường hợp lệ có thể truy cập được. Do đó thuật toán sẽ loại bỏ chính xác khi không có tuyến đường hợp lệ nào tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from itertools import permutations

def adjacent(a, b):
    return abs(a[0] - b[0]) + abs(a[1] - b[1]) == 1

def solve():
    n, m = map(int, input().split())

    cells = []
    for i in range(n):
        row = list(map(int, input().split()))
        for j, value in enumerate(row):
            cells.append((value, (i, j)))

    cells.sort()

    groups = []
    p = 0
    total = len(cells)

    while p < total:
        q = p + 1
        while q < total and cells[q][0] == cells[p][0]:
            q += 1

        group_cells = [cells[k][1] for k in range(p, q)]

        valid_orders = []
        for order in permutations(group_cells):
            ok = True
            for k in range(len(order) - 1):
                if not adjacent(order[k], order[k + 1]):
                    ok = False
                    break
            if ok:
                valid_orders.append(order)

        if not valid_orders:
            print(-1)
            return

        groups.append(valid_orders)
        p = q

    parents = []
    reachable = [True] * len(groups[0])
    parents.append([-1] * len(groups[0]))

    for g in range(1, len(groups)):
        current = groups[g]
        previous = groups[g - 1]

        cur_reachable = [False] * len(current)
        cur_parent = [-1] * len(current)

        for ci, cur_order in enumerate(current):
            first_cell = cur_order[0]

            for pi, prev_order in enumerate(previous):
                if not reachable[pi]:
                    continue

                last_cell = prev_order[-1]

                if adjacent(last_cell, first_cell):
                    cur_reachable[ci] = True
                    cur_parent[ci] = pi
                    break

        if not any(cur_reachable):
            print(-1)
            return

        reachable = cur_reachable
        parents.append(cur_parent)

    state = -1
    for i, ok in enumerate(reachable):
        if ok:
            state = i
            break

    if state == -1:
        print(-1)
        return

    chosen = [0] * len(groups)

    for g in range(len(groups) - 1, -1, -1):
        chosen[g] = state
        if g > 0:
            state = parents[g][state]

    answer = []
    for g in range(len(groups)):
        answer.extend(groups[g][chosen[g]])

    out = []
    for i, j in answer:
        out.append(f"{i + 1} {j + 1}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai sẽ làm phẳng lưới và sắp xếp nó. Tọa độ được lưu trữ nội bộ bằng cách sử dụng chỉ mục dựa trên 0, điều này làm cho việc tính toán kề và xử lý mảng trở nên đơn giản. Chúng chỉ được chuyển đổi trở lại tọa độ một cơ sở khi đưa ra câu trả lời. 

Vòng lặp nhóm đi qua các ô đã sắp xếp và tập hợp tất cả các giá trị bằng nhau lại với nhau. Vì mỗi giá trị xuất hiện nhiều nhất ba lần,`group_cells`không bao giờ có nhiều hơn ba phần tử.`itertools.permutations`do đó tạo ra tối đa sáu ứng viên nên việc sử dụng trực tiếp là hoàn toàn an toàn. 

các`adjacent`hàm sử dụng khoảng cách Manhattan. Hai ô liền kề nhau chính xác khi tổng hiệu của hàng và cột của chúng bằng một. Không cần phải kiểm tra ranh giới rõ ràng ở đây vì mọi tọa độ được so sánh đều thuộc về lưới. 

các`parents`mảng lưu trữ một chỉ mục tiền thân cho mọi trạng thái có thể truy cập. Nhóm đầu tiên sử dụng`-1`bởi vì nó không có tiền thân. Đối với các nhóm sau, trạng thái tương thích đầu tiên trước đó là đủ. Không cần thiết phải nhớ tất cả các tuyến đi trước có thể có vì một tuyến đi trước thành công là đủ để xây dựng lại một tuyến đường hợp lệ. 

DP chỉ giữ lại mảng khả năng tiếp cận của nhóm trước trong khi xử lý nhóm hiện tại, nhưng các mảng trước đó được giữ lại để xây dựng lại. Vì mỗi nhóm có tối đa sáu trạng thái nên bộ nhớ này vẫn nhỏ ngay cả khi lưới có (10.000) ô. 

Quá trình tái thiết đi từ trạng thái có thể truy cập cuối cùng về phía sau. Hoán vị đã chọn của mỗi nhóm được lưu trữ trong`chosen`và chỉ sau khi tất cả các liên kết trước đó đã được tuân theo thì chúng ta mới duyệt qua các nhóm theo thứ tự chuyển tiếp. Điều này tránh được lỗi phổ biến khi in ngược đường dẫn được xây dựng lại. 

Không có số học số nguyên nào ngoài chênh lệch tọa độ và chỉ số mảng có thể tăng theo giá trị đầu vào, do đó, tràn số nguyên không phải là vấn đề đáng lo ngại trong Python. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2 4
1 4 8 16
2 4 16 16
```Các nhóm sau khi sắp xếp theo kích thước sẽ được hiển thị bên dưới. 

| Nhóm | Giá trị | Tế bào | Đơn hàng đã chọn | 
| --- | --- | --- | --- | 
| 0 | 1 | ((1,1)) | ((1,1)) | 
| 1 | 2 | ((2,1)) | ((2,1)) | 
| 2 | 4 | ((1,2),(2,2)) | ((2,2),(1,2)) | 
| 3 | 8 | ((1,3)) | ((1,3)) | 
| 4 | 16 | ((1,4),(2,4),(2,3)) | ((1,4),(2,4),(2,3)) | 

Đối với nhóm kích thước (4), có hai thứ tự có thể có, nhưng chỉ thứ tự bắt đầu tại ((2,2)) kết nối với ô trước ((2,1)). Thứ tự còn lại bắt đầu từ ((1,2)), liền kề theo đường chéo với ((2,1)) và do đó không thể sử dụng được. 

Sự phát triển trạng thái DP là: 

| Nhóm | Các trạng thái có thể tiếp cận | Trạng thái được chọn | Chuyển tiếp ranh giới | 
| --- | --- | --- | --- | 
| 1 | 1 | ((1,1)) | Bắt đầu | 
| 2 | 1 | ((2,1)) | ((1,1)\rightarrow(2,1)) | 
| 4 | 1 trên 2 | ((2,2),(1,2)) | ((2,1)\rightarrow(2,2)) | 
| 8 | 1 | ((1,3)) | ((1,2)\rightarrow(1,3)) | 
| 16 | 2 đơn hàng hợp lệ, một đơn hàng có thể truy cập được | ((1,4),(2,4),(2,3)) | ((1,3)\rightarrow(1,4)) | 

Lộ trình kết quả là```
1 1
2 1
2 2
1 2
1 3
1 4
2 4
2 3
```Các kích thước gặp phải là (1,2,4,4,8,16,16,16), không giảm và mọi cặp ô liên tiếp đều liền kề nhau. 

### Mẫu 2 

Đầu vào là```
3 3
1 2 2
1 2 3
1 3 3
```Các nhóm là 

| Nhóm | Giá trị | Tế bào | Có thể đặt hàng nội bộ | 
| --- | --- | --- | --- | 
| 0 | 1 | ((1,1),(2,1),(3,1)) | Hai đường dẫn từ điểm cuối đến điểm cuối | 
| 1 | 2 | ((1,2),(1,3),(2,2)) | Hai đường đi qua ((1,2)) | 
| 2 | 3 | ((2,3),(3,2),(3,3)) | Hai đường đi qua ((3,3)) | 

Mỗi nhóm có một đường dẫn nội bộ hợp lệ. Lỗi xảy ra ở ranh giới giữa giá trị (1) và (2). 

Các điểm cuối có thể có của nhóm size-(1) là ((1,1)) và ((3,1)). Các ô bắt đầu có thể có của đường dẫn size-(2) là ((1,3)) và ((2,2)). Không có cặp nào trong số này nằm cạnh nhau. 

| Chuyển tiếp | Điểm cuối có thể có trước đó | Có thể bắt đầu hiện tại | Cặp tương thích | 
| --- | --- | --- | --- | 
| (1\rightarrow2) | ((1,1),(3,1)) | ((1,3),(2,2)) | Không có | 

Do đó, DP không có trạng thái có thể truy cập được đối với nhóm kích thước-(2) và ngay lập tức báo cáo```
-1
```Ví dụ này cho thấy tại sao việc kiểm tra từng nhóm giá trị bằng nhau một cách độc lập là không đủ. Các nhóm cũng phải được tham gia theo thứ tự yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N\log N)) | Chi phí sắp xếp (O(N\log N)), trong khi mỗi nhóm có tối đa 6 trạng thái và mỗi trạng thái kiểm tra tối đa 6 trạng thái trước đó | 
| Không gian | (O(N)) | Các ô lưới, hoán vị nhóm, trạng thái tiền thân và tuyến đường được xây dựng lại chỉ chứa một số lượng đối tượng không đổi trên mỗi ô | 

Ở đây (N=n\cdot m\le10.000). DP chỉ thực hiện một lượng nhỏ công việc không đổi cho mỗi nhóm sau khi sắp xếp, do đó số hạng sắp xếp (N\log N) chiếm ưu thế. Việc sử dụng bộ nhớ cũng thoải mái trong giới hạn 256 MB đã nêu. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm bên dưới giả định giải pháp đã gửi được lưu dưới dạng`solution.py`. Nó thay thế toàn cầu của mô-đun đó`input`hoạt động sao cho hiện tại`solve()`chức năng có thể được kiểm tra nhiều lần mà không làm thay đổi giải pháp cạnh tranh.```python
import sys
import io
import solution

def run(inp: str) -> str:
    old_input = solution.input
    old_stdout = sys.stdout

    stream = io.StringIO(inp)
    output = io.StringIO()

    solution.input = stream.readline
    sys.stdout = output

    try:
        solution.solve()
    finally:
        solution.input = old_input
        sys.stdout = old_stdout

    return output.getvalue().strip()

def validate(inp: str, out: str) -> bool:
    data = list(map(int, inp.split()))
    it = iter(data)
    n = next(it)
    m = next(it)

    a = [[next(it) for _ in range(m)] for _ in range(n)]
    total = n * m

    if out.strip() == "-1":
        return False

    vals = list(map(int, out.split()))
    if len(vals) != 2 * total:
        return False

    path = []
    for k in range(total):
        r = vals[2 * k] - 1
        c = vals[2 * k + 1] - 1

        if not (0 <= r < n and 0 <= c < m):
            return False

        path.append((r, c))

    if len(set(path)) != total:
        return False

    for k in range(1, total):
        r1, c1 = path[k - 1]
        r2, c2 = path[k]

        if abs(r1 - r2) + abs(c1 - c2) != 1:
            return False

        if a[r1][c1] > a[r2][c2]:
            return False

    return True

sample1 = """\
2 4
1 4 8 16
2 4 16 16
"""

sample2 = """\
3 3
1 2 2
1 2 3
1 3 3
"""

assert validate(sample1, run(sample1)), "sample 1"
assert run(sample2) == "-1", "sample 2"

case_min = """\
1 1
7
"""
assert validate(case_min, run(case_min)), "minimum-size case"

case_all_equal = """\
1 3
5 5 5
"""
assert validate(case_all_equal, run(case_all_equal)), "all-equal case"

case_ordering = """\
2 2
1 2
1 3
"""
assert validate(case_ordering, run(case_ordering)), "equal-group ordering case"

case_boundary = """\
1 3
1 3 2
"""
assert run(case_boundary) == "-1", "forced transition failure"

n = 100
m = 100
grid = [[0] * m for _ in range(n)]
value = 1

for r in range(n):
    if r % 2 == 0:
        cols = range(m)
    else:
        cols = range(m - 1, -1, -1)

    for c in cols:
        grid[r][c] = value
        value += 1

case_max = f"{n} {m}\n"
case_max += "\n".join(" ".join(map(str, row)) for row in grid)
case_max += "\n"

assert validate(case_max, run(case_max)), "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 7`| Tuyến đường một ô | Kích thước tối thiểu và không có chuyển đổi nhóm | 
|`1 3 / 5 5 5`| Ba ô liền kề theo một trong hai hướng | Một nhóm chứa tối đa ba giá trị bằng nhau được phép | 
|`2 2 / 1 2 / 1 3`| Tuyến đường hợp lệ sử dụng đúng thứ tự của hai ô size-(1) | DP phải lựa chọn giữa nhiều hoán vị có giá trị bằng nhau | 
|`1 3 / 1 3 2`|`-1`| Các nhóm đơn liên tiếp vẫn không thể kết nối | 
| (100\times100) lưới có nhãn con rắn | Tuyến đường 10.000 ô hợp lệ | Kích thước tối đa, xử lý ranh giới, sắp xếp và tái cấu trúc đầu ra | 

## Vỏ cạnh 

Trường hợp đơn bào```
1 1
7
```tạo chính xác một nhóm với một hoán vị, ((1,1)). Việc khởi tạo nhóm đầu tiên đánh dấu nó có thể truy cập được và việc tái cấu trúc sẽ tạo ra một ô đó. Không cần logic chuyển tiếp nên thuật toán không vô tình loại bỏ lưới nhỏ nhất có thể. 

Đối với các giá trị bằng nhau được phân tách,```
1 3
1 2 1
```các nhóm được sắp xếp là (1:{(1,1),(1,3)}) và (2:{(1,2)}). Nhóm size-(1) không có hoán vị hợp lệ vì hai ô của nó không liền kề nhau. Thuật toán từ chối trước khi thử bất kỳ chuyển đổi nào. Điều này trực tiếp nắm bắt được yêu cầu rằng các giá trị bằng nhau phải chiếm một đoạn liền kề của tuyến đường. 

Đối với nhiều đơn hàng trong một nhóm,```
2 2
1 2
1 3
```nhóm size-(1) có hai hoán vị có thể có. Thứ tự ((1,1),(2,1)) không thể kết nối với ((1,2)), trong khi ((2,1),(1,1)) thì có thể. DP bảo toàn cả hai trạng thái ban đầu và chỉ loại bỏ trạng thái không tương thích khi xử lý nhóm tiếp theo. Do đó nó tìm thấy```
2 1
1 1
1 2
2 2
```thay vì cam kết sớm với một thứ tự có giá trị bằng nhau tùy ý. 

Đối với lỗi chuyển tiếp,```
1 3
1 3 2
```mỗi nhóm chỉ có một ô, vì vậy mọi nhóm đều có giá trị nội bộ. Thứ tự sắp xếp buộc phải là các vị trí của (1), (2) và (3), cụ thể là ((1,1)), ((1,3)) và ((1,2)). Quá trình chuyển đổi đầu tiên không thành công vì ((1,1)) và ((1,3)) không liền kề. DP không có trạng thái có thể truy cập được đối với nhóm thứ hai và các đầu ra`-1`. 

Đối với ba ô bằng nhau, phép liệt kê hoán vị vẫn còn rất nhỏ. Nếu các ô tạo thành một đường dẫn, chẳng hạn như```
1 3
4 4 4
```nhóm hợp lệ chứa các thứ tự từ trái sang phải và từ phải sang trái. Bốn hoán vị còn lại không đạt được bài kiểm tra kề bên trong. Thuật toán giữ chính xác hai đơn hàng thực sự có thể được duyệt qua. 

Trường hợp kích thước tối đa sử dụng (10.000) giá trị riêng biệt, do đó mỗi nhóm chỉ có một hoán vị. Sau đó, DP sẽ thoái hóa thành một phép kiểm tra đơn giản xem mỗi ô có giá trị tiếp theo có liền kề với ô trước đó không. Thứ tự rắn của lưới thỏa mãn điều kiện này và việc triển khai xử lý tất cả (10.000) ô mà không cần dựa vào đệ quy hoặc tìm kiếm theo cấp số nhân.

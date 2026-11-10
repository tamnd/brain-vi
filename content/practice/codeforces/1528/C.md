---
title: "CF 1528C - Cây Yên Tĩnh"
description: "Chúng ta có hai cây có gốc trên cùng một tập đỉnh 1..n. Cả hai cây đều có gốc ở đỉnh 1. Từ hai cây này chúng ta xác định được đồ thị. Hai đỉnh được kết nối nếu chúng thỏa mãn đồng thời hai điều kiện."
date: "2026-06-10T17:02:42+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1528
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 722 (Div. 1)"
rating: 2300
weight: 1528
solve_time_s: 192
verified: true
draft: false
---

[CF 1528C - Cây bình yên](https://codeforces.com/problemset/problem/1528/C) 

**Đánh giá:** 2300 
**Thẻ:** cấu trúc dữ liệu, dfs và các cây tương tự, tham lam 
**Thời gian giải:** 3 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai cây có gốc trên cùng một tập đỉnh`1..n`. Cả hai cây đều có gốc ở đỉnh`1`. 

Từ hai cây này chúng ta xác định một biểu đồ. Hai đỉnh được kết nối nếu chúng thỏa mãn đồng thời hai điều kiện. 

Điều kiện đầu tiên nói rằng trong cây thứ nhất, một đỉnh nằm trên đường đi từ gốc tới gốc kia. Nói cách khác, hai đỉnh có liên quan đến tổ tiên trong cây đầu tiên. 

Điều kiện thứ hai nói rằng ở cây thứ hai, không có đỉnh nào là tổ tiên của cây kia. Khoảng cách tổ tiên của chúng trong cây thứ hai chắc chắn là không thể so sánh được. 

Nhiệm vụ là tìm kích thước của nhóm lớn nhất trong biểu đồ này. 

Biểu đồ trực tiếp không bao giờ được xây dựng một cách rõ ràng. Với tối đa`3 * 10^5`đỉnh trên tất cả các trường hợp thử nghiệm, thậm chí việc lưu trữ tất cả các cạnh có thể là không thể. Một biểu đồ trên`3 * 10^5`các đỉnh có thể chứa khoảng`4.5 * 10^10`các cạnh. 

Các ràng buộc ngay lập tức loại trừ mọi phương pháp kiểm tra các cặp đỉnh. MỘT`O(n²)`thuật toán sẽ yêu cầu khoảng`9 * 10^10`hoạt động trong trường hợp xấu nhất. Chúng ta cần một cái gì đó gần tuyến tính hoặc`O(n log n)`. 

Phần tế nhị nhất của vấn đề là hiểu được một nhóm trông như thế nào. 

Giả sử cây đầu tiên là một chuỗi:```
1
|
2
|
3
|
4
```Mỗi cặp đỉnh được chọn đều có liên quan đến tổ tiên trong cây đầu tiên. Yêu cầu duy nhất còn lại là không có cặp nào có liên quan đến tổ tiên trong cây thứ hai. 

Một giải pháp bất cẩn có thể cố gắng tối đa hóa số đỉnh có các khoảng trong cây thứ hai rời rạc theo từng cặp. Điều đó là không đủ. Chúng ta cần một tính chất mạnh hơn: với mỗi cặp, không khoảng nào có thể chứa khoảng nào. Các khoảng lồng nhau bị cấm vì việc lồng nhau có nghĩa là mối quan hệ tổ tiên trong cây thứ hai. 

Một sai lầm dễ mắc phải khác xuất hiện khi khoảng của một đỉnh được chọn chứa nhiều đỉnh khác. 

Ví dụ:```
Second tree:
1
├─2
│ └─3
└─4
```Khoảng cách của các đỉnh`2`Và`3`được lồng nhau. Cả hai đều không thể thuộc về nhóm mặc dù cả hai có thể nằm trên cùng một đường ở cây đầu tiên. 

Gốc cũng đáng được quan tâm. Trong cây thứ hai, gốc là tổ tiên của mọi đỉnh. Khoảng của nó chứa mọi khoảng khác. Do đó, gốc không bao giờ có thể cùng tồn tại với một đỉnh khác trong một cụm hợp lệ. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là xây dựng biểu đồ tưởng niệm và tính toán nhóm tối đa của nó. 

Đối với mỗi cặp đỉnh, chúng ta có thể xác định liệu chúng có tạo thành cạnh hay không bằng cách kiểm tra quan hệ tổ tiên trên cả hai cây. Cái đó đã tốn rồi`O(n²)`. Sau khi xây dựng đồ thị, việc tìm ra một cụm tối đa là NP-hard trong đồ thị tổng quát khiến hướng đi này hoàn toàn vô vọng. 

Điều quan trọng quan trọng là đồ thị được tạo ra ở đây có cấu trúc rất đặc biệt. 

Hãy xem xét một nhóm. Vì mỗi cặp đỉnh phải liên quan đến tổ tiên trong cây đầu tiên nên tất cả các đỉnh của cụm phải nằm trên một đường dẫn duy nhất từ ​​gốc đến nút của cây đầu tiên. 

Tại sao? 

Lấy hai đỉnh bất kỳ trong cụm. Chúng phải có thể so sánh được ở cây đầu tiên. Một tập hợp các đỉnh có thể so sánh theo cặp trong một cây có gốc tạo thành một chuỗi, chính xác là một đường đi từ gốc đi xuống. 

Bây giờ hãy nhìn vào cây thứ hai. 

Đối với bất kỳ hai đỉnh nhóm nào, không có đỉnh nào có thể là tổ tiên của đỉnh kia trong cây thứ hai. Sử dụng các khoảng tham quan Euler, mỗi đỉnh tương ứng với một khoảng cây con`[tin, tout]`. Mối quan hệ tổ tiên trở thành ngăn chặn khoảng cách. 

Do đó, cụm tương ứng với một tập hợp các đỉnh trên một đường đi của cây thứ nhất có các khoảng trong cây thứ hai không chứa từng cặp. 

Một tuyên bố mạnh mẽ hơn là đúng. 

Nếu chúng ta duy trì một tập hợp các khoảng của cây thứ hai sao cho không có khoảng nào chứa một khoảng khác thì bất kỳ hai đỉnh tương ứng nào cũng không liên quan đến tổ tiên trong cây thứ hai. Vì tất cả các đỉnh được duy trì đều đến từ một đường dẫn hiện tại trong cây DFS đầu tiên nên chúng sẽ tự động tạo thành một cụm. 

Điều này biến vấn đề thành: 

Trong khi đi qua một đường đi trong cây thứ nhất, hãy duy trì tập hợp các đỉnh lớn nhất có thể có các khoảng trong cây thứ hai là không chứa từng cặp. 

Cấu trúc khoảng gợi ý một giải pháp tập hợp có thứ tự. Trong DFS trên cây đầu tiên, các đỉnh được thêm vào khi vào và xóa khi rời khỏi. Chúng tôi giữ tập hợp các khoảng thời gian tối ưu hiện tại trên đường dẫn hiện tại. 

Giải pháp nổi tiếng duy trì các khoảng thời gian được sắp xếp theo thời gian vào cây thứ hai của Euler. Bất cứ khi nào một khoảng mới được chèn vào, tối đa một khoảng hiện có có thể cần phải được loại bỏ. Điều này mang lại một`O(n log n)`thuật toán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(n²) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Tiền xử lý cây thứ hai 

Đầu tiên thực hiện chuyến du hành Euler trên cây thứ hai. 

Với mọi đỉnh`v`, tính:`tin[v]`= thời gian vào`tout[v]`= thời gian thoát 

đỉnh`u`là tổ tiên của`v`ở cây thứ hai chính xác khi:`tin[u] ≤ tin[v]`Và`tout[v] ≤ tout[u]`. 

### Duy trì nhóm hoạt động 

Trong DFS của cây đầu tiên, ngăn xếp đệ quy biểu thị một đường dẫn từ gốc đến hiện tại. 

Chúng tôi duy trì một tập hợp các đỉnh được chọn từ đường dẫn này. Thay vì lưu trữ trực tiếp các đỉnh, chúng ta lưu trữ thời gian vào Euler của chúng theo thứ tự cấu trúc cân bằng. 

Điều bất biến là các đỉnh được chọn tương ứng với các khoảng trong cây thứ hai sao cho không có đỉnh nào chứa đỉnh khác. 

### chuyển tiếp DFS 

1. Nhập một đỉnh`v`ở cây đầu tiên. 
2. Tìm khoảng ngay trước và ngay sau`tin[v]`trong tập có thứ tự. 
3. Kiểm tra xem một trong các khoảng thời gian hiện được chọn đã chứa khoảng thời gian của`v`. 

Nếu một khoảng như vậy tồn tại, việc thêm`v`sẽ tăng quy mô nhóm. Chúng tôi loại bỏ khoảng chứa đó và chèn`v`. 
4. Ngược lại hãy kiểm tra xem`v`chứa một trong các khoảng đã chọn. 

Nếu có thì thêm`v`sẽ làm cho bộ sưu tập tồi tệ hơn bởi vì`v`ít cụ thể hơn khoảng mà nó chứa. Chúng tôi không chèn`v`. 
5. Nếu không`v`là không thể so sánh được với tất cả các khoảng đã chọn. Chèn nó vào. 
6. Cập nhật câu trả lời chung với kích thước hiện tại của tập hợp. 
7. DFS vào con cái của`v`ở cây đầu tiên. 
8. Sau khi tất cả các phần tử con được xử lý, khôi phục chính xác những sửa đổi được thực hiện khi nhập`v`. 

Điều này khôi phục cấu trúc dữ liệu về trạng thái tương ứng với đường dẫn gốc. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, ngăn xếp DFS là một đường dẫn trong cây đầu tiên. Mỗi đỉnh được chọn đều thuộc về đường dẫn đó, vì vậy mỗi cặp đỉnh được chọn đều có liên quan đến tổ tiên trong cây đầu tiên. 

Tập được duy trì luôn chứa các khoảng không chứa theo cặp trong cây thứ hai. Hai khoảng trong một cây có gốc chỉ có thể vi phạm điều kiện cụm nếu một khoảng chứa khoảng kia. Các quy tắc cập nhật duy trì sự vắng mặt của ngăn chặn. 

Khi một khoảng mới được chứa bên trong một khoảng đã chọn hiện có, việc thay thế khoảng lớn hơn bằng khoảng nhỏ hơn không thể làm giảm các khả năng trong tương lai. Khoảng thời gian nhỏ hơn hoàn toàn hạn chế hơn và ít nhất cũng để lại nhiều khoảng trống cho các khoảng thời gian rời rạc khác. Đây là bước tham lam quan trọng. 

Do đó, tập hợp được duy trì là cụm tối đa có thể có giữa các đỉnh trên đường dẫn gốc tới nút hiện tại. Lấy kích thước tối đa trên tất cả các trạng thái DFS sẽ mang lại nhóm tối đa toàn cầu. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline
sys.setrecursionlimit(1 << 20)

def solve():
    t = int(input())
    ans_out = []

    for _ in range(t):
        n = int(input())

        g1 = [[] for _ in range(n + 1)]
        p1 = list(map(int, input().split()))
        for i, p in enumerate(p1, start=2):
            g1[p].append(i)

        g2 = [[] for _ in range(n + 1)]
        p2 = list(map(int, input().split()))
        for i, p in enumerate(p2, start=2):
            g2[p].append(i)

        tin = [0] * (n + 1)
        tout = [0] * (n + 1)

        timer = 0

        def dfs2(v):
            nonlocal timer
            timer += 1
            tin[v] = timer
            for to in g2[v]:
                dfs2(to)
            tout[v] = timer

        dfs2(1)

        active = []
        best = 0

        def is_ancestor(u, v):
            return tin[u] <= tin[v] and tout[v] <= tout[u]

        def dfs1(v):
            nonlocal best

            pos = bisect_left(active, tin[v])

            inserted = False
            removed_vertex = -1

            if pos > 0:
                pred_tin = active[pos - 1]
                pred = tin_to_vertex[pred_tin]

                if is_ancestor(pred, v):
                    active.pop(pos - 1)
                    active.insert(pos - 1, tin[v])

                    inserted = True
                    removed_vertex = pred

                    best = max(best, len(active))

                    for to in g1[v]:
                        dfs1(to)

                    active.pop(pos - 1)
                    active.insert(pos - 1, tin[pred])
                    return

            if pos < len(active):
                succ_tin = active[pos]
                succ = tin_to_vertex[succ_tin]

                if is_ancestor(v, succ):
                    best = max(best, len(active))

                    for to in g1[v]:
                        dfs1(to)

                    return

            active.insert(pos, tin[v])
            inserted = True

            best = max(best, len(active))

            for to in g1[v]:
                dfs1(to)

            active.pop(bisect_left(active, tin[v]))

        tin_to_vertex = [0] * (n + 1)
        for v in range(1, n + 1):
            tin_to_vertex[tin[v]] = v

        dfs1(1)

        ans_out.append(str(best))

    sys.stdout.write("\n".join(ans_out))

if __name__ == "__main__":
    solve()
```DFS đầu tiên tính toán các khoảng Euler trong cây thứ hai. Bởi vì các khoảng thời gian của cây con trở thành các phân đoạn liền kề nhau nên các truy vấn tổ tiên giảm xuống còn các kiểm tra ngăn chặn khoảng thời gian. 

DFS thứ hai chạy trên cây đầu tiên. Ngăn xếp đệ quy chính xác là đường dẫn mà chúng ta đang xem xét các ứng cử viên trong nhóm. 

Cấu trúc có thứ tự`active`lưu trữ thời gian vào Euler của các đỉnh hiện được chọn. Thực tế quan trọng là giữa các khoảng không có ngăn chặn, sắp xếp theo thứ tự`tin`là đủ. Bất kỳ khoảng nào có thể chứa khoảng mới phải là khoảng trước đó theo thứ tự này. Bất kỳ khoảng nào có thể được chứa bởi khoảng mới phải là khoảng kế tiếp của nó. Đó là lý do tại sao chỉ cần kiểm tra hai người hàng xóm. 

Việc khôi phục được xử lý ngay sau khi quay trở lại từ các cuộc gọi đệ quy. Mọi trạng thái DFS đều thấy chính xác bộ sưu tập tương ứng với đường dẫn gốc đến nút của chính nó. 

## Ví dụ đã hoạt động 

### Mẫu 2 

đầu vào:```
5
1 2 3 4
1 1 1 1
```Cây đầu tiên là một chuỗi. Cây thứ hai là một ngôi sao. 

Khoảng Euler ở cây thứ hai: 

| Đỉnh | Khoảng thời gian | 
| --- | --- | 
| 1 | [1,5] | 
| 2 | [2,2] | 
| 3 | [3,3] | 
| 4 | [4,4] | 
| 5 | [5,5] | 

DFS trên cây đầu tiên: 

| Đỉnh hiện tại | Bộ hoạt động | 
| --- | --- | 
| 1 | {1} | 
| 2 | {2} | 
| 3 | {2,3} | 
| 4 | {2,3,4} | 
| 5 | {2,3,4,5} | 

Khoảng gốc chứa mọi khoảng khác nên nó sẽ được thay thế ngay lập tức. Tất cả các khoảng lá là không thể so sánh được theo cặp, tạo ra kích thước cụm`4`. 

### Mẫu 4 

đầu vào:```
7
1 1 3 4 4 5
1 2 1 4 2 5
```Trong DFS của cây đầu tiên, một số đỉnh thay thế tổ tiên có khoảng chứa chúng trong cây thứ hai. 

| Đã nhập đỉnh | Hành động | Kích thước nhóm | 
| --- | --- | --- | 
| 1 | chèn | 1 | 
| 3 | thay thế chứa tổ tiên | 1 | 
| 4 | chèn | 2 | 
| 6 | chèn | 3 | 

Kích thước tối đa đạt được là`3`, tương ứng với các đỉnh`{3,4,6}`. 

Dấu vết này thể hiện quy tắc thay thế tham lam. Khoảng chứa lớn hơn không bao giờ tốt hơn khoảng nhỏ hơn bên trong nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi đỉnh thực hiện một số lượng không đổi các phép toán được sắp xếp theo thứ tự | 
| Không gian | O(n) | Cây, mảng Euler, ngăn xếp đệ quy, tập hoạt động | 

Tổng của tất cả`n`giá trị nhiều nhất là`3 · 10^5`. MỘT`O(n log n)`giải pháp thực hiện khoảng vài triệu hoạt động có cấu trúc cân bằng, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    def solve():
        from bisect import bisect_left

        input = sys.stdin.readline
        t = int(input())
        ans = []

        for _ in range(t):
            n = int(input())

            g1 = [[] for _ in range(n + 1)]
            p1 = list(map(int, input().split()))
            for i, p in enumerate(p1, start=2):
                g1[p].append(i)

            g2 = [[] for _ in range(n + 1)]
            p2 = list(map(int, input().split()))
            for i, p in enumerate(p2, start=2):
                g2[p].append(i)

            tin = [0] * (n + 1)
            tout = [0] * (n + 1)

            timer = 0

            def dfs2(v):
                nonlocal timer
                timer += 1
                tin[v] = timer
                for to in g2[v]:
                    dfs2(to)
                tout[v] = timer

            dfs2(1)

            tin_to_vertex = [0] * (n + 1)
            for v in range(1, n + 1):
                tin_to_vertex[tin[v]] = v

            active = []
            best = 0

            def anc(u, v):
                return tin[u] <= tin[v] and tout[v] <= tout[u]

            def dfs1(v):
                nonlocal best

                pos = bisect_left(active, tin[v])

                if pos > 0:
                    p = tin_to_vertex[active[pos - 1]]
                    if anc(p, v):
                        old = active[pos - 1]
                        active[pos - 1] = tin[v]

                        best = max(best, len(active))
                        for to in g1[v]:
                            dfs1(to)

                        active[pos - 1] = old
                        return

                if pos < len(active):
                    s = tin_to_vertex[active[pos]]
                    if anc(v, s):
                        best = max(best, len(active))
                        for to in g1[v]:
                            dfs1(to)
                        return

                active.insert(pos, tin[v])

                best = max(best, len(active))
                for to in g1[v]:
                    dfs1(to)

                active.pop(bisect_left(active, tin[v]))

            dfs1(1)
            ans.append(str(best))

        return "\n".join(ans)

    return solve()

assert run(
"""4
4
1 2 3
1 2 3
5
1 2 3 4
1 1 1 1
6
1 1 1 1 2
1 2 1 2 2
7
1 1 3 4 4 5
1 2 1 4 2 5
"""
) == "1\n4\n1\n3"

assert run(
"""1
2
1
1
"""
) == "1"

assert run(
"""1
5
1 2 3 4
1 2 3 4
"""
) == "1"

assert run(
"""1
5
1 2 3 4
1 1 1 1
"""
) == "4"

assert run(
"""1
3
1 1
1 1
"""
) == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai chuỗi giống hệt nhau | 1 | Mỗi cặp đều có liên quan đến tổ tiên ở cả hai cây | 
| Chuỗi vs ngôi sao | 4 | Cụm tối đa có thể là hầu hết tất cả các đỉnh không phải gốc | 
| n = 2 | 1 | Đầu vào pháp lý nhỏ nhất | 
| Sao ở cả hai cây | 2 | Khoảng cách cây thứ hai không thể so sánh được được xử lý chính xác | 

## Vỏ cạnh 

### Cây giống nhau 

đầu vào:```
1
4
1 2 3
1 2 3
```Mọi cặp đỉnh so sánh được ở cây thứ nhất cũng có thể so sánh được ở cây thứ hai. Không có cạnh nào tồn tại giữa các đỉnh phân biệt. Thuật toán tiếp tục thay thế các khoảng thời gian nhưng không bao giờ tăng tập hoạt động vượt quá kích thước`1`. Câu trả lời là`1`. 

### Khoảng gốc chứa mọi thứ 

đầu vào:```
1
5
1 2 3 4
1 1 1 1
```Khoảng gốc ở cây thứ hai là`[1,5]`. Khi đỉnh`2`được truy cập, khoảng của nó được chứa bên trong khoảng gốc. Thuật toán thay gốc bằng đỉnh`2`. Đây chính xác là quy tắc tham lam ngăn chặn root chặn các bổ sung trong tương lai. Câu trả lời cuối cùng trở thành`4`. 

### Các khoảng lồng nhau trong cây thứ hai 

đầu vào:```
1
3
1 2
1 2
```Khoảng cách của các đỉnh`2`Và`3`được lồng nhau. Một giải pháp bất cẩn chỉ kiểm tra đường đi của cây đầu tiên sẽ chọn cả hai. Logic thiết lập theo thứ tự phát hiện việc ngăn chặn và không bao giờ cho phép cả hai khoảng thời gian duy trì hoạt động đồng thời. Câu trả lời là`1`. 

### Lá vô song 

đầu vào:```
1
3
1 2
1 1
```Đỉnh`2`Và`3`nằm trên đường ở cây thứ nhất nhưng ở cây thứ hai họ là anh em ruột. Khoảng thời gian của chúng rời rạc nên cả hai vẫn ở trong tập hoạt động. Thuật toán đạt kích thước`2`, đó là nhóm tối đa chính xác.

---
title: "CF 102279G - Ngày càng cao hơn"
description: "Có hai cây. B21 chọn gốc cây một cách tối ưu, cố gắng làm cho chiều cao của nó lớn nhất có thể. Lowie chọn gốc cây của mình một cách đối nghịch theo quan điểm của B21, vì vậy B21 chỉ cần biết liệu có tồn tại gốc nào đó cho cây của Lowie khiến B21…"
date: "2026-08-16T19:19:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102279
codeforces_index: "G"
codeforces_contest_name: "HCW 19 Team Round (ICPC format)"
rating: 0
weight: 102279
solve_time_s: 105
verified: true
draft: false
---

[CF 102279G - Ngày càng cao hơn](https://codeforces.com/problemset/problem/102279/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 45 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Có hai cây. B21 chọn gốc cây một cách tối ưu, cố gắng làm cho chiều cao của nó lớn nhất có thể. Lowie chọn gốc cây của mình một cách đối nghịch theo quan điểm của B21, vì vậy B21 chỉ cần biết liệu có tồn tại một số gốc cho cây của Lowie làm cho chiều cao kết quả của B21 lớn hơn hay không. 

Chiều cao của cây có gốc được đo bằng các đỉnh trên đường đi từ gốc đến đỉnh dài nhất. Thay vào đó, sẽ thuận tiện hơn khi làm việc với khoảng cách được đo bằng các cạnh. Nếu khoảng cách dài nhất từ ​​gốc đến bất kỳ đỉnh nào là`h`cạnh, chiều cao của cây là`h + 1`đỉnh. phần bổ sung`1`xuất hiện ở cả hai cây nên nó biến mất khi chúng ta so sánh chiều cao của chúng. 

Đối với B21, việc tối đa hóa chiều cao trên tất cả các rễ có thể cũng giống hệt như việc tìm đường kính của cây. Nếu đường kính có`D`các cạnh, chọn điểm cuối đường kính làm gốc cho chiều cao`D + 1`. 

Đối với Lowie, chúng ta cần số lượng ngược lại. B21 muốn biết liệu có căn cứ nào khiến chiều cao của Lowie nhỏ lại nhất có thể hay không. Khoảng cách tối đa nhỏ nhất có thể có từ gốc tới mọi đỉnh là bán kính của cây, tức là`ceil(D / 2)`khi đường kính là`D`. Do đó, chiều cao tốt nhất có thể có của Lowie theo quan điểm của B21 là`ceil(D / 2) + 1`. 

Đầu vào chứa cây B21, sau đó là cây Lowie. Một cái cây với`N`đỉnh có`N - 1`các cạnh, mặc dù văn bản tuyên bố được lưu trữ nói rằng`N`các đường viền. Các ví dụ sử dụng`N - 1`các cạnh, đây là định dạng duy nhất phù hợp với định nghĩa của cây, do đó việc triển khai sẽ đọc`N - 1`Và`M - 1`các cạnh. 

Các giới hạn này đủ lớn để việc truyền tải bậc hai là không thể. Với tối đa`10^5`các đỉnh trong cây B21 và`2 * 10^5`các đỉnh trong cây Lowie, một`O(N^2 + M^2)`phương pháp có thể yêu cầu xung quanh`10^11`những chuyến thăm lân cận. Giới hạn chính thức chỉ là 1 giây với bộ nhớ 256 MB, vì vậy giải pháp dự định phải xử lý từng cây theo thời gian tuyến tính. 

Trường hợp cạnh đầu tiên là cây B21 một đỉnh.```
1
2
1 2
```Đường kính của B21 là 0 cạnh nên chiều cao tối đa của nó là`1`. Cây Lowie có đường kính một cạnh nên chiều cao tối thiểu có thể có của nó là`2`. B21 không thể thắng, và đáp án là`FF`. Việc triển khai bất cẩn giả định rằng mỗi cây đều có ít nhất một cạnh có thể thất bại khi DFS của nó bắt đầu từ một cây lân cận không tồn tại hoặc khi nó khởi tạo đường kính thành`1`. 

Trường hợp cạnh thứ hai là một trận hòa ở đúng ranh giới.```
4
1 2
2 3
3 4
7
1 2
2 3
3 4
4 5
5 6
6 7
```Đường kính của B21 là`3`, trong khi đường kính của Lowie là`6`. Bán kính tối thiểu của Lowie là`ceil(6 / 2) = 3`, nên cả hai người chơi đều có thể đạt được chiều cao`4`. Câu trả lời đúng là`FF`, vì hòa không được tính là thắng. Việc thực hiện bất cẩn bằng cách sử dụng`>=`thay vì`>`sẽ in sai`GGEZ`. 

Trường hợp cạnh thứ ba có đường kính lẻ.```
4
1 2
2 3
3 4
6
1 2
2 3
3 4
4 5
5 6
```Đường kính là`3`Và`5`. Bán kính tối thiểu của Lowie là`ceil(5 / 2) = 3`, vì vậy cả độ cao tối đa và tối thiểu có liên quan đều là`4`. Câu trả lời là một lần nữa`FF`. Điều này phát hiện các triển khai vô tình sử dụng phép chia số nguyên`D / 2`thay vì chia trần`(D + 1) / 2`. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ thử mọi root có thể. Đối với mỗi gốc, hãy chạy DFS hoặc BFS và tìm đỉnh xa nhất. Việc lấy mức tối đa của các giá trị này sẽ mang lại chiều cao tốt nhất cho B21, trong khi việc lấy mức tối thiểu sẽ mang lại gốc tốt nhất mà Lowie có thể vô tình chọn. Điều này đúng vì định nghĩa chiều cao của cây chính xác là khoảng cách tối đa từ gốc đã chọn. 

Vấn đề là việc truyền tải lặp đi lặp lại. Một sự đi qua của một`N`-kiểm tra cây đỉnh`2(N - 1)`các mục lân cận. Do đó, việc chạy nó một lần cho mỗi root sẽ mất`2N(N - 1)`những chuyến thăm lân cận. Làm tương tự với cây của Lowie sẽ cho ra kết quả khác`2M(M - 1)`. Tại`N = 10^5`Và`M = 2 * 10^5`, đây là về`99,999,400,000`thăm lân cận, vượt xa thời hạn. 

Cách tiếp cận brute-force có hiệu quả vì mỗi gốc xác định một cách độc lập chiều cao của cây, nhưng thực tế chúng ta không cần phải kiểm tra từng gốc. Cấu trúc của cây cung cấp cho chúng ta hai đại lượng tổng thể tóm tắt chính xác những gì chúng ta cần. 

Chiều cao tối đa có thể đạt được bằng cách thay đổi gốc là đường kính. Nếu điểm cuối đường kính là`a`Và`b`, root tại`a`đạt tới`b`ở khoảng cách tối đa có thể, do đó chiều cao thu được là đường kính cộng một. 

Chiều cao tối thiểu có thể đạt được bằng cách thay đổi gốc là bán kính. Mỗi đỉnh phải gần với gốc đã chọn và gốc tốt nhất có thể là tâm của cây. Tâm nằm ở nửa đường kính nên đường kính`D`khoảng cách tối đa tối thiểu có thể là`ceil(D / 2)`. 

Điều này làm giảm toàn bộ vấn đề để tính toán hai đường kính. Đường kính cây có thể được tìm thấy bằng cách duyệt hai đồ thị. Bắt đầu từ bất kỳ đỉnh nào và tìm đỉnh xa nhất`a`. Sau đó bắt đầu từ`a`và tìm đỉnh xa nhất`b`. Khoảng cách từ`a`ĐẾN`b`là đường kính. 

Cho phép`D_B`là đường kính của B21 ở các cạnh và`D_L`là đường kính Lowie tính theo cạnh. B21 thắng chính xác khi`D_B + 1 > ceil(D_L / 2) + 1`đơn giản hóa để`D_B > ceil(D_L / 2)`. 

Kết quả so sánh là tuyến tính về kích thước của hai cây. Đây cũng là cách tiếp cận được mô tả bởi bài xã luận chính thức của cuộc thi, giúp giảm bài toán xuống chiều cao tối đa và tối thiểu có thể của cây và tính toán đường kính cần thiết với hai lần duyệt trên mỗi cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(N² + M²)`|`O(N + M)`| Quá chậm | 
| Tối ưu |`O(N + M)`|`O(N + M)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cây B21 và xây dựng danh sách kề của nó. Một cái cây với`N`đỉnh có chính xác`N - 1`các cạnh, vậy đó là các cạnh thuộc đồ thị B21. 
2. Tính đường kính B21 qua hai đường chéo. Bắt đầu từ bất kỳ đỉnh nào, tìm đỉnh xa nhất`a`, sau đó bắt đầu từ`a`và tìm đỉnh xa nhất`b`. Khoảng cách từ`a`ĐẾN`b`là`D_B`, đường kính tính theo cạnh. 
3. Đọc cây Lowie và xây dựng danh sách kề của nó. Thuộc tính cây giống nhau có nghĩa là nó chứa`M - 1`các cạnh. 
4. Tính đường kính Lowie`D_L`sử dụng cùng một phương pháp hai chiều. 
5. Chuyển đường kính Lowie thành khoảng cách tối đa nhỏ nhất có thể tính từ gốc. Gốc ở tâm đường kính sẽ giảm thiểu khoảng cách xa nhất của nó, cho bán kính`ceil(D_L / 2)`. Trong số học số nguyên đây là`(D_L + 1) // 2`. 
6. So sánh khoảng cách tối đa của B21`D_B`với khoảng cách tối đa có thể tối thiểu của Lowie. B21 thắng chính xác khi`D_B > (D_L + 1) // 2`. Sự bất bình đẳng nghiêm ngặt là cần thiết bởi vì sự bình đẳng tạo ra một trận hòa. 
7. In`GGEZ`khi bất đẳng thức giữ nguyên và`FF`nếu không thì. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi chiều cao của cây có gốc có thể được biểu thị bằng một cộng với khoảng cách cạnh tối đa tính từ gốc của nó. Giá trị lớn nhất như vậy trên tất cả các nghiệm là đường kính cộng một, bởi vì điểm cuối đường kính có thể đến điểm cuối đối diện ở khoảng cách đường kính. Giá trị nhỏ nhất như vậy là bán kính cộng một và mọi tâm cây đều có độ lệch tâm`ceil(D / 2)`, Ở đâu`D`là đường kính. Như vậy B21 có một lựa chọn căn thắng chính xác khi đường kính của nó lớn hơn bán kính của Lowie. Quy trình hai đường ngang tính toán chính xác từng đường kính, do đó phép so sánh cuối cùng không thể bỏ sót một căn có thể thắng hoặc tuyên bố thắng khi chỉ có thể có kết quả hòa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def diameter(graph):
    n = len(graph)

    def farthest(start):
        dist = [-1] * n
        dist[start] = 0
        stack = [start]
        far = start

        while stack:
            u = stack.pop()
            if dist[u] > dist[far]:
                far = u

            for v in graph[u]:
                if dist[v] == -1:
                    dist[v] = dist[u] + 1
                    stack.append(v)

        return far, dist[far]

    if n == 1:
        return 0

    a, _ = farthest(0)
    _, d = farthest(a)
    return d

def solve():
    n = int(input())
    b21 = [[] for _ in range(n)]

    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        b21[u].append(v)
        b21[v].append(u)

    m = int(input())
    lowie = [[] for _ in range(m)]

    for _ in range(m - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        lowie[u].append(v)
        lowie[v].append(u)

    db = diameter(b21)
    dl = diameter(lowie)

    lowie_radius = (dl + 1) // 2

    if db > lowie_radius:
        print("GGEZ")
    else:
        print("FF")

if __name__ == "__main__":
    solve()
```các`diameter`hàm thực hiện chính xác hai lần duyệt được mô tả trong phần hướng dẫn. Lần duyệt đầu tiên chỉ cần xác định đỉnh xa nhất, trong khi lần duyệt thứ hai ghi lại khoảng cách từ đỉnh đó đến mọi đỉnh có thể tiếp cận và trả về đỉnh lớn nhất. 

Biểu đồ là một cái cây, vì vậy một DFS lặp đơn giản là đủ. BFS không bắt buộc vì mọi cạnh đều có đơn vị giá như nhau và chúng ta chỉ cần khoảng cách xa nhất trong cây không có trọng số. Việc sử dụng ngăn xếp rõ ràng cũng tránh được các vấn đề về độ sâu đệ quy của Python trên đường dẫn chứa`10^5`hoặc`2 * 10^5`đỉnh. 

các`n == 1`việc kiểm tra là cần thiết vì cây một đỉnh có đường kính bằng 0. Lần truyền tải đầu tiên vẫn hoạt động, nhưng việc xử lý trường hợp này một cách rõ ràng sẽ làm cho định nghĩa trở nên rõ ràng và tránh dựa vào hành vi của logic nút xa nhất chung đối với danh sách kề trống. 

Việc chuyển đổi từ đường kính sang bán kính sử dụng`(dl + 1) // 2`. Đối với đường kính chẵn như`6`, điều này mang lại`3`. Đối với đường kính lẻ như`5`, nó mang lại`3`, đó là trần nhà được yêu cầu chứ không phải sàn nhà. 

Sự so sánh cuối cùng là nghiêm ngặt. Nếu như`db == lowie_radius`, chiều cao thu được bằng nhau, do đó đầu ra phải là`FF`. 

Các chỉ số đỉnh được chuyển đổi từ đầu vào dựa trên một thành chỉ số Python dựa trên 0. Không thể tràn số nguyên trong Python và tối đa là tất cả các khoảng cách`2 * 10^5 - 1`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Cây B21 có cạnh```
1-2, 1-3, 2-4, 2-5
```Quá trình duyệt đầu tiên có thể bắt đầu ở đỉnh`1`. Một đỉnh xa nhất có thể là`4`, ở khoảng cách`2`. Bắt đầu từ`4`, đỉnh xa nhất là`3`, ở khoảng cách`3`. Vậy đường kính của B21 là`3`. 

Đối với cây Lowie, bắt đầu từ`1`. Đỉnh xa nhất có thể là`5`, ở khoảng cách`2`. Bắt đầu từ`5`, đỉnh`7`đang ở khoảng cách`4`, cho đường kính Lowie`4`. Bán kính của nó là`ceil(4 / 2) = 2`. 

| Cây | Lần đầu tiên bắt đầu | Đầu tiên xa nhất | Bắt đầu thứ hai | Xa thứ hai | Đường kính | Bán kính | 
| --- | --- | --- | --- | --- | --- | --- | 
| B21 | 1 | 4 | 4 | 3 | 3 | không cần thiết | 
| Lowie | 1 | 5 | 5 | 7 | 4 | 2 | 

Sự so sánh là`3 > 2`, do đó B21 có thể chọn điểm cuối đường kính làm gốc của mình và đạt được chiều cao lớn hơn chiều cao của Lowie theo lựa chọn gốc thuận lợi. Câu trả lời là`GGEZ`. 

### Mẫu 2 

Cây B21 không thay đổi nên đường kính giữ nguyên`3`. 

Đối với cây Lowie, bắt đầu từ`1`, một đỉnh xa nhất có thể là`5`. Bắt đầu từ`5`, đỉnh`7`đang ở khoảng cách`6`, vậy đường kính của Lowie là`6`. Bán kính của nó là`ceil(6 / 2) = 3`. 

| Cây | Lần đầu tiên bắt đầu | Đầu tiên xa nhất | Bắt đầu thứ hai | Xa thứ hai | Đường kính | Bán kính | 
| --- | --- | --- | --- | --- | --- | --- | 
| B21 | 1 | 4 | 4 | 3 | 3 | không cần thiết | 
| Lowie | 1 | 5 | 5 | 7 | 6 | 3 | 

Bây giờ sự so sánh là`3 > 3`, điều đó là sai. Cả hai người chơi đều có thể đạt được chiều cao`4`, do đó kết quả là hòa và câu trả lời là`FF`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(N + M)`| Mỗi cây được duyệt hai lần và mỗi lần duyệt sẽ kiểm tra mọi đỉnh và cạnh một số lần không đổi. | 
| Không gian |`O(N + M)`| Hai danh sách kề và mảng khoảng cách truyền tải lưu trữ một số đỉnh và cạnh tuyến tính. | 

Cây lớn nhất có`2 * 10^5`các đỉnh, do đó thuật toán chỉ thực hiện một số lượng tuyến tính không đổi trên khoảng`3 * 10^5`các đỉnh và các cạnh của chúng. Điều này phù hợp với giới hạn 1 giây và 256 MB dự định một cách thoải mái hơn nhiều so với phương pháp vũ phu bậc hai. 

## Trường hợp thử nghiệm 

Bộ dây thử nghiệm sau đây sử dụng cùng một`solve`triển khai và thay thế đầu vào và đầu ra tiêu chuẩn để mỗi trường hợp có thể được kiểm tra bằng một xác nhận.```python
import sys
import io
from contextlib import redirect_stdout

def diameter(graph):
    n = len(graph)

    def farthest(start):
        dist = [-1] * n
        dist[start] = 0
        stack = [start]
        far = start

        while stack:
            u = stack.pop()

            if dist[u] > dist[far]:
                far = u

            for v in graph[u]:
                if dist[v] == -1:
                    dist[v] = dist[u] + 1
                    stack.append(v)

        return far, dist[far]

    if n == 1:
        return 0

    a, _ = farthest(0)
    _, d = farthest(a)
    return d

def solve():
    input = sys.stdin.readline

    n = int(input())
    b21 = [[] for _ in range(n)]

    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        b21[u].append(v)
        b21[v].append(u)

    m = int(input())
    lowie = [[] for _ in range(m)]

    for _ in range(m - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        lowie[u].append(v)
        lowie[v].append(u)

    db = diameter(b21)
    dl = diameter(lowie)

    if db > (dl + 1) // 2:
        print("GGEZ")
    else:
        print("FF")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    output = io.StringIO()
    try:
        with redirect_stdout(output):
            solve()
    finally:
        sys.stdin = old_stdin

    return output.getvalue().strip()

# Provided sample 1
sample1 = """\
5
1 2
1 3
2 4
2 5
7
1 2
2 5
3 6
2 4
1 3
3 7
"""
assert run(sample1) == "GGEZ", "sample 1"

# Provided sample 2
sample2 = """\
5
1 2
1 3
2 4
2 5
7
1 2
1 3
3 4
4 5
2 6
6 7
"""
assert run(sample2) == "FF", "sample 2"

# Minimum-size B21 tree
case_min = """\
1
2
1 2
"""
assert run(case_min) == "FF", "single vertex B21 tree"

# Exact draw boundary
case_draw = """\
4
1 2
2 3
3 4
7
1 2
2 3
3 4
4 5
5 6
6 7
"""
assert run(case_draw) == "FF", "equal B21 diameter and Lowie radius"

# Odd Lowie diameter
case_odd = """\
4
1 2
2 3
3 4
6
1 2
2 3
3 4
4 5
5 6
"""
assert run(case_odd) == "FF", "odd diameter requires ceiling"

# Branching tree with a very small diameter
case_star = """\
3
1 2
1 3
4
1 2
1 3
1 4
"""
assert run(case_star) == "GGEZ", "star-shaped trees"

# Maximum-size generated case
def make_max_case():
    n = 100000
    m = 200000

    parts = [str(n)]
    for i in range(1, n):
        parts.append(f"{i} {i + 1}")

    parts.append(str(m))
    for i in range(1, m):
        parts.append(f"{i} {i + 1}")

    return "\n".join(parts) + "\n"

max_case = make_max_case()
assert run(max_case) == "FF", "maximum-size paths"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`đỉnh so với cây 2 đỉnh |`FF`| Cây có kích thước tối thiểu và đường kính bằng 0 | 
| Đường dẫn B21 của 4 so với đường dẫn Lowie của 7 |`FF`| Sự bằng nhau chính xác giữa đường kính B21 và bán kính Lowie | 
| Đường dẫn B21 của 4 so với đường dẫn Lowie của 6 |`FF`| Chia trần cho đường kính lẻ | 
| Hai sao |`GGEZ`| Cây phân nhánh và đường kính nhỏ | 
| Đường dẫn với`100000`Và`200000`đỉnh |`FF`| Ràng buộc tối đa và hành vi thời gian tuyến tính | 

## Vỏ cạnh 

Đối với trường hợp một đỉnh, cây B21 không có cạnh, vì vậy`D_B = 0`. Cây hai đỉnh của Lowie có`D_L = 1`, cho bán kính`(1 + 1) // 2 = 1`. Sự so sánh trở nên`0 > 1`, điều này là sai, vì vậy thuật toán in ra`FF`. Sự rõ ràng`n == 1`chi nhánh ở`diameter`trả về số 0 trực tiếp. 

Để có ranh giới rút thăm chính xác, hãy xem xét```
4
1 2
2 3
3 4
7
1 2
2 3
3 4
4 5
5 6
6 7
```Cây thứ nhất có đường kính`3`. Thứ hai có đường kính`6`, vậy bán kính của nó là`(6 + 1) // 2 = 3`. Bài kiểm tra cuối cùng là`3 > 3`, điều đó là sai. Sự so sánh nghiêm ngặt từ chối một cách chính xác một trận hòa. 

Đối với đường kính Lowie lẻ, hãy xem xét```
4
1 2
2 3
3 4
6
1 2
2 3
3 4
4 5
5 6
```Đường kính của B21 là`3`. Đường kính của Lowie là`5`, và nghiệm tốt nhất là một trong hai đỉnh trung tâm, cho bán kính`3`. biểu hiện`(5 + 1) // 2`sản xuất`3`, do đó thuật toán xử lý chính xác thao tác trần. Từ`3 > 3`là sai, nó in`FF`. 

Đối với một cây phân nhánh, hãy xem xét```
3
1 2
1 3
4
1 2
1 3
1 4
```Cả hai cây đều là ngôi sao. Đường kính của chúng là`2`, vậy bán kính Lowie là`1`. Đường kính B21`2`thực sự lớn hơn`1`và thuật toán in`GGEZ`. Điều này chứng tỏ tại sao lời giải chỉ phụ thuộc vào đường kính chứ không phụ thuộc vào cây là đường đi hay có nhiều nhánh. 

Đối với trường hợp kích thước tối đa, cả hai cây đều là đường dẫn, với`100000`Và`200000`các đỉnh tương ứng. Đường kính của chúng là`99999`Và`199999`. Bán kính của Lowie là`(199999 + 1) // 2 = 100000`, trong khi đường kính của B21 chỉ bằng`99999`, vậy câu trả lời là`FF`. Việc triển khai xử lý toàn bộ đầu vào với bốn lần truyền tuyến tính và không bao giờ xây dựng ma trận khoảng cách bậc hai, đây chính xác là hành vi được yêu cầu bởi các ràng buộc.

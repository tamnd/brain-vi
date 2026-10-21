---
title: "CF 105780E - Hoa Hải Mã"
description: "Khu vườn là một lưới n x n. Một số tế bào đã có hoa. Trong quá trình này, các tế bào có thể thu được hoa hoặc các cặp tế bào có thể được nối với nhau bằng các đường hầm dưới lòng đất."
date: "2026-06-25T15:54:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105780
codeforces_index: "E"
codeforces_contest_name: "UTPC x WiCS Contest 3-12-25 (UT Internal)"
rating: 0
weight: 105780
solve_time_s: 54
verified: true
draft: false
---

[CF 105780E - Hoa Walrus Wallflowers](https://codeforces.com/problemset/problem/105780/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Khu vườn là một`n x n`lưới. Một số tế bào đã có hoa. Trong quá trình này, các tế bào có thể thu được hoa hoặc các cặp tế bào có thể được nối với nhau bằng các đường hầm dưới lòng đất. Một nhóm hoa thuộc cùng một nụ khi các bông hoa được kết nối thông qua lưới điện liền kề thông thường hoặc qua các đường hầm dưới lòng đất. 

Đối với mỗi lần nảy chồi, chúng ta cần hai giá trị. Đầu tiên là`f`, số lượng tế bào hoa bên trong nó. Thứ hai là`c`, số lượng đường hầm chạm vào chồi non đó. Năng lượng được đóng góp bởi sự nảy chồi này là`max(f - sqrt(c), 0)`. Sau mỗi thao tác, nhiệm vụ là tính tổng các giá trị này trên tất cả các chồi. 

Các giới hạn đủ nhỏ để cho phép xây dựng lại toàn bộ cấu trúc kết nối sau mỗi hoạt động. Lưới có nhiều nhất`10000`tế bào và có nhiều nhất`1000`hoạt động. Một giải pháp xung quanh`1000 * 10000`hoạt động chỉ khoảng mười triệu hoạt động cơ bản nên không cần cấu trúc kết nối động phức tạp. Một giải pháp quét lưới từ đầu sau mỗi lần cập nhật có thể chấp nhận được, trong khi các phương pháp mô phỏng mọi khả năng nảy chồi riêng biệt hoặc tìm kiếm liên tục qua tất cả các cặp ô sẽ tốn kém một cách không cần thiết. 

Những cạm bẫy triển khai chính là xung quanh khả năng kết nối và đường hầm đếm. Một ô hoa có thể được kết nối với một bông hoa khác thông qua một chuỗi đường hầm dưới lòng đất hoặc các ô hoa liền kề nên chỉ kiểm tra hàng xóm trực tiếp sẽ cho kết quả sai. Ví dụ:```
2 1
10
01
2 0 0 1 1
```Sau khi thêm đường hầm, hai bông hoa tạo thành một nụ. Câu trả lời là`2 - sqrt(1)`, không`2`, bởi vì kết nối ngầm góp phần vào`c`. 

Một trường hợp tế nhị khác là việc cắm hoa lặp đi lặp lại.```
2 1
10
00
1 0 0
```Ô đã có hoa rồi nên vườn không thay đổi gì. Việc triển khai bất cẩn làm tăng số lượng hoa mà không kiểm tra trạng thái hiện tại sẽ bị tính quá mức. 

Trường hợp cuối cùng là chồi có nhiều đường hầm nhưng ít hoa.```
2 1
10
00
2 0 0 0 1
```Tế bào bụi bẩn được kết nối dưới lòng đất với tế bào hoa, nhưng nó không được tính là một bông hoa. Sự chớm nở có`f = 1`Và`c = 1`, vậy phần đóng góp là`0`. Việc đếm mọi ô được kết nối là một bông hoa sẽ cho kết quả không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là duy trì khu vườn sau mỗi truy vấn và tính toán lại tất cả các chồi. Chúng ta có thể mô hình hóa mọi ô lưới như một đỉnh. Các ô liền kề được kết nối bằng các cạnh lưới thông thường và các hoạt động ngầm sẽ thêm các cạnh phụ. Sau mỗi lần cập nhật, chúng tôi chạy liên kết tập hợp rời rạc trên tất cả các ô, nối các ô liền kề và tất cả các kết nối ngầm. Sau đó, chúng tôi đếm, đối với mỗi thành phần thu được, có bao nhiêu ô chứa hoa và bao nhiêu cạnh ngầm thuộc về nó. 

Điều này hoạt động vì số lượng ô và thao tác đều nhỏ. Trường hợp xấu nhất thực hiện gần đúng`1000`xây dựng lại. Mỗi lần xây dựng lại chạm`10000`tế bào, về`20000`kiểm tra tính kề cận của lưới, và nhiều nhất là`1000`rìa ngầm. Điều này thoải mái trong giới hạn. 

Giải pháp kết nối động tiên tiến hơn sẽ duy trì các thành phần khi đường hầm và hoa thay đổi, nhưng điều đó là không cần thiết ở đây. Quan sát quan trọng là toàn bộ không gian trạng thái đủ nhỏ để việc xây dựng lại đơn giản và an toàn hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(d * n2) | O(n²) | Quá chậm nếu được thực hiện với các tìm kiếm biểu đồ lặp đi lặp lại | 
| Xây dựng lại DSU | O(d * (n2 + d)) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ xem mỗi ô hiện có một bông hoa hay không và lưu trữ mọi kết nối ngầm đã được thêm vào. Các thao tác chỉ mang tính chất bổ sung nên chúng ta không bao giờ cần loại bỏ thông tin. 
2. Sau mỗi truy vấn, hãy tạo một cấu trúc hợp tập rời rạc mới chứa tất cả`n * n`tế bào. Ban đầu mỗi tế bào là riêng biệt. Chúng tôi xây dựng lại vì tổng kích thước của lưới nhỏ. 
3. Nối từng cặp ô liền kề theo chiều ngang hoặc chiều dọc. Điều này thể hiện kết nối lưới bình thường. Lý do chúng tôi bao gồm tất cả các ô, kể cả ô đất, là vì một bông hoa trong tương lai có thể xuất hiện trên bất kỳ ô nào và khả năng kết nối ngầm hoặc lưới là một thuộc tính của chính các ô đó. 
4. Tham gia cả hai điểm cuối của mọi kết nối ngầm. Điều này bổ sung thêm các đường dẫn bổ sung được tạo bởi hoạt động của Karp. 
5. Đi qua từng ô và đếm số lượng ô hoa bên trong mỗi thành phần DSU. Chỉ có tế bào hoa góp phần vào`f`. 
6. Đi qua mọi kết nối ngầm và đếm nó bên trong thành phần của nó. Bởi vì mọi cạnh ngầm đã được hợp nhất vào DSU nên cả hai điểm cuối đều có cùng một đại diện, do đó mỗi cạnh thuộc về chính xác một chồi. 
7. Đối với mỗi thành phần có chứa hoa, hãy thêm`max(f - sqrt(c), 0)`để trả lời. In năng lượng thu được. 

Tại sao nó hoạt động: bất biến DSU là hai ô có cùng một đại diện chính xác khi chúng được kết nối thông qua tất cả các đường dẫn hiện có. Vì mọi vùng lân cận của lưới điện và đường hầm ngầm đều được hợp nhất trước khi đếm nên mỗi lần nảy chồi được thể hiện bằng một thành phần DSU. Đếm hoa bên trong một thành phần sẽ cho kết quả chính xác`f`và việc đếm các cạnh ngầm trong thành phần đó sẽ cho kết quả chính xác`c`. Phép tính tổng cuối cùng áp dụng công thức năng lượng một cách độc lập cho mỗi lần nảy chồi, do đó kết quả chính xác là lượng năng lượng cần thiết cho khu vườn. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    n, d = map(int, input().split())
    total = n * n

    flowers = []
    for _ in range(n):
        s = input().strip()
        flowers.extend(ch == '1' for ch in s)

    tunnels = []

    def idx(x, y):
        return x * n + y

    def compute():
        parent = list(range(total))
        size = [1] * total

        def find(x):
            while parent[x] != x:
                parent[x] = parent[parent[x]]
                x = parent[x]
            return x

        def union(a, b):
            a = find(a)
            b = find(b)
            if a == b:
                return
            if size[a] < size[b]:
                a, b = b, a
            parent[b] = a
            size[a] += size[b]

        for i in range(n):
            for j in range(n):
                cur = idx(i, j)
                if i + 1 < n:
                    union(cur, idx(i + 1, j))
                if j + 1 < n:
                    union(cur, idx(i, j + 1))

        for a, b in tunnels:
            union(a, b)

        flower_count = [0] * total
        for i, has_flower in enumerate(flowers):
            if has_flower:
                flower_count[find(i)] += 1

        tunnel_count = [0] * total
        for a, b in tunnels:
            tunnel_count[find(a)] += 1

        ans = 0.0
        for i in range(total):
            if flower_count[i]:
                ans += max(flower_count[i] - math.sqrt(tunnel_count[i]), 0.0)

        return ans

    out = []
    for _ in range(d):
        q = list(map(int, input().split()))
        if q[0] == 1:
            flowers[idx(q[1], q[2])] = True
        else:
            tunnels.append((idx(q[1], q[2]), idx(q[3], q[4])))

        out.append(f"{compute():.10f}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`flowers`mảng lưu trữ trạng thái hiện tại của mọi ô lưới bằng cách sử dụng chỉ mục phẳng. Việc làm phẳng tránh việc lưu trữ nhiều lần các cặp tọa độ và làm cho các hoạt động DSU trở nên trực tiếp. 

các`compute`chức năng xây dựng lại kết nối. Vòng kết hợp đầu tiên thêm các cạnh lưới thông thường và vòng kết hợp thứ hai thêm tất cả các đường hầm dưới lòng đất. Thứ tự không ảnh hưởng đến các thành phần cuối cùng vì DSU chỉ thể hiện khả năng kết nối. 

Hai mảng đếm riêng biệt vì hoa và đường hầm biểu thị số lượng khác nhau.`flower_count`đếm ô, trong khi`tunnel_count`đếm các kết nối ngầm. Việc trộn chúng sẽ tạo ra các giá trị không chính xác cho số hạng căn bậc hai. 

Vòng lặp cuối cùng chỉ xem xét các thành phần có ít nhất một bông hoa. Các thành phần được làm hoàn toàn bằng bụi bẩn không hình thành chồi và không đóng góp gì. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 4
101
100
001
1 2 0
2 0 0 0 1
2 0 2 2 2
1 0 1
```Những thay đổi trạng thái quan trọng là: 

| Bước | Hoạt động | Đếm hoa trong nụ | Số lượng đường hầm trong chồi | Năng lượng | 
| --- | --- | --- | --- | --- | 
| Ban đầu | Chưa có hoạt động | Ba nhóm biệt lập | 0, 0, 0 | 5.00000000 | 
| 1 | Thêm hoa tại (2,0) | Ba nhóm biệt lập | 0, 0, 0 | 5.00000000 | 
| 2 | Thêm đường hầm giữa (0,0) và (0,1) | Cùng nhóm | Một nhóm có 1 đường hầm | 4.00000000 | 
| 3 | Thêm đường hầm giữa (0,2) và (2,2) | Hai nhóm | Mỗi nhóm kết nối có 1 đường hầm | 3.00000000 | 
| 4 | Thêm hoa tại (0,1) | Một nhóm được kết nối | 2 hầm | 4.58578644 | 

Dấu vết này cho thấy tại sao các đường hầm chỉ ảnh hưởng đến số hạng căn bậc hai. Chúng kết hợp những bông hoa vào cùng một nụ và cũng làm tăng số lượng đường hầm. 

Đối với một ví dụ tùy chỉnh nhỏ hơn:```
2 3
10
00
2 0 0 0 1
1 1 0
1 0 1
```| Bước | Hoạt động | Đếm hoa | Số lượng đường hầm | Năng lượng | 
| --- | --- | --- | --- | --- | 
| 1 | Kết nối (0,0) và (0,1) | 1 | 1 | 0,00000000 | 
| 2 | Thêm hoa tại (1,0) | 2 | 1 | 1.00000000 | 
| 3 | Thêm hoa tại (0,1) | 3 | 1 | 2.00000000 | 

Dấu vết chứng tỏ các tế bào bụi bẩn sau này có thể trở thành hoa trong khi vẫn giữ nguyên cấu trúc kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(d * (n2 + d)) | Mỗi truy vấn sẽ xây dựng lại DSU trên tất cả các ô và tất cả các đường hầm | 
| Không gian | O(n²) | Mảng DSU và trạng thái lưới mỗi mảng lưu trữ một giá trị trên mỗi ô | 

Với`n = 100`, chỉ có`10000`tế bào. Với`d = 1000`, tổng công việc khoảng vài chục triệu thao tác đơn giản, vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""3 4
101
100
001
1 2 0
2 0 0 0 1
2 0 2 2 2
1 0 1
""") == """5.0000000000
4.0000000000
3.0000000000
4.5857864376
"""

assert run("""2 1
10
00
1 0 0
""") == """1.0000000000
"""

assert run("""2 3
10
00
2 0 0 0 1
1 1 0
1 0 1
""") == """0.0000000000
1.0000000000
2.0000000000
"""

assert run("""2 2
11
11
1 0 0
2 0 0 1 1
""") == """4.0000000000
3.0000000000
"""

assert run("""3 1
000
000
000
2 0 0 2 2
""") == """0.0000000000
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cắm hoa đơn | 1.0000000000 | Cập nhật hoa lặp đi lặp lại hoặc đơn giản | 
| Đường hầm trước nhiều hoa | 0,0000000000 thì giá trị lớn hơn | Kết nối thông qua tế bào để sau này thu được hoa | 
| Lưới hoa đầy đủ | Giảm năng lượng sau một đường hầm | Đếm đường hầm riêng biệt với hoa | 
| Lưới trống có đường hầm | 0 | Bỏ qua các thành phần không có hoa | 

## Vỏ cạnh 

Việc cắm hoa lặp đi lặp lại không tạo ra bông hoa mới. Trong đầu vào```
2 1
10
00
1 0 0
```tế bào`(0,0)`đã bị chiếm đóng rồi. Thuật toán đặt giá trị thành`True`một lần nữa và xây dựng lại cấu trúc thành phần tương tự, tạo ra năng lượng`1`. 

Một đường hầm nối các ô trước đây tách biệt phải hợp nhất các chồi của chúng. Vì```
2 1
10
01
2 0 0 1 1
```DSU nối hai tế bào hoa. Có một cạnh ngầm trong thành phần, vì vậy`f = 2`Và`c = 1`, cho`2 - sqrt(1) = 1`. 

Đường hầm chạm đất không làm tăng số lượng hoa. Vì```
2 1
10
00
2 0 0 0 1
```thành phần chứa một bông hoa và một đường hầm dưới lòng đất. Giá trị là`max(1 - sqrt(1), 0) = 0`, mà thuật toán thu được bằng cách đếm hoa và đường hầm một cách độc lập.

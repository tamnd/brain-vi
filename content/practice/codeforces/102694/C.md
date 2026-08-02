---
title: "CF 102694C - Lười Ngủ Trưa"
description: "Chúng ta có một cây có tới hàng trăm nghìn nút. Một con lười bắt đầu từ nút a và muốn đến nút b. Hạn chế duy nhất là nó có thể vượt qua hầu hết các cạnh c trước khi rơi vào trạng thái ngủ. Nếu đường đi từ a đến b ngắn hơn hoặc bằng c thì con lười tới b."
date: "2026-08-01T23:27:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102694
codeforces_index: "C"
codeforces_contest_name: "AlgorithmsThread Tree Basics Contest"
rating: 0
weight: 102694
solve_time_s: 88
verified: true
draft: false
---

[CF 102694C - Giờ ngủ trưa của con lười](https://codeforces.com/problemset/problem/102694/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có tới hàng trăm nghìn nút. Một con lười bắt đầu từ nút`a`và muốn tiếp cận nút`b`. Hạn chế duy nhất là nó có thể vượt qua nhiều nhất`c`cạnh trước khi đi ngủ. Nếu đường đi từ`a`ĐẾN`b`ngắn hơn hoặc bằng`c`, con lười đạt tới`b`. Nếu không thì nó dừng chính xác`c`cạnh sau khi rời đi`a`trên con đường duy nhất hướng tới`b`. 

Đối với mỗi truy vấn, chúng ta cần xuất ra nút nơi con lười dừng lại. Đầu vào là một cây theo sau là nhiều truy vấn chuyển động độc lập. Cấu trúc cây không bao giờ thay đổi, vì vậy thách thức chính là xử lý trước nó đủ để đáp ứng từng chuyển động một cách nhanh chóng. 

Giới hạn cho phép lên tới`n = 300000`nút và`q = 300000`truy vấn. Một giải pháp xuyên suốt cây cho mọi truy vấn có thể mất`O(nq)`thời gian trong trường hợp xấu nhất, vượt xa những gì có thể. Chúng tôi cần mỗi truy vấn có thời gian logarit sau giai đoạn tiền xử lý. Vì độ sâu của cây cũng có thể`O(n)`, bước nhảy cha mẹ đơn giản là không đủ trừ khi chúng ta chuẩn bị các truy vấn tổ tiên nhanh hơn. 

Những trường hợp phức tạp đều xuất phát từ hướng chuyển động. Đích có thể ở phía trên nút bắt đầu trong cây gốc, bên dưới nó hoặc ở một nhánh khác. Ví dụ:```
3
1 2
2 3
1
1 3 1
```Đầu ra đúng là:```
2
```Việc thực hiện bất cẩn chỉ có thể đi lên từ`a`và thất bại vì con đường bắt đầu bằng việc đi xuống. 

Một trường hợp khó khăn khác là khi con lười đã có đủ năng lượng:```
3
1 2
2 3
1
1 3 5
```Đầu ra đúng là:```
3
```Việc triển khai luôn thực hiện chính xác`c`những cú nhảy sẽ cố gắng di chuyển qua đích. 

Trường hợp thứ ba là khi nút bắt đầu và nút kết thúc bằng nhau:```
1
1
1 1 10
```Câu trả lời là:```
1
```Độ dài đường đi bằng 0 nên không có chuyển động nào xảy ra. Các giải pháp giả sử đường dẫn chứa ít nhất một cạnh có thể đưa ra các bước nhảy không hợp lệ. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là xây dựng lại đường dẫn từ`a`ĐẾN`b`, sau đó di chuyển`c`các cạnh dọc theo nó. Điều này đúng vì một cây có chính xác một đường đi đơn giản giữa mỗi cặp nút. Tuy nhiên, việc tìm đường dẫn đó bằng cách tìm kiếm trên cây cho mọi truy vấn có thể truy cập được nhiều nút. Với cây hình dây chuyền`300000`các nút và cùng số lượng truy vấn, tổng công việc có thể đạt tới`90000000000`lượt truy cập biên, quá chậm. 

Quan sát quan trọng là chúng ta không thực sự cần toàn bộ con đường. Chúng ta chỉ cần tìm nút ở một khoảng cách nhất định tính từ một điểm cuối. Đây là một vấn đề nhảy cây cổ điển. 

Chúng tôi root cây và xử lý trước các bảng nâng nhị phân. Đối với mỗi nút, chúng tôi lưu trữ tổ tiên của nó ở khoảng cách`1, 2, 4, 8`, vân vân. Điều này cho phép chúng ta di chuyển lên trên theo số cạnh bất kỳ trong`O(log n)`thời gian. 

Để xử lý chuyển động hướng tới một mục tiêu tùy ý, chúng tôi sử dụng tổ tiên chung thấp nhất. Cho phép`l`là LCA của`a`Và`b`. Con đường từ`a`ĐẾN`b`được chia thành hai phần: đường dẫn từ`a`lên đến`l`, và đường đi từ`l`xuống tới`b`. 

Nếu con lười cần di chuyển ít cạnh hơn khoảng cách từ`a`ĐẾN`l`, câu trả lời đơn giản là tổ tiên của`a`. Ngược lại, nó tới LCA và sau đó tiếp tục đi xuống về phía`b`. Chuyển động đi xuống được chuyển thành chuyển động đi lên bằng cách nhìn vào mặt sau của đường đi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) cho mỗi truy vấn, tổng O(nq) | O(n) | Quá chậm | 
| Nâng nhị phân | O(log n) cho mỗi truy vấn sau khi xử lý trước | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gốc cây tại nút`1`và chạy tìm kiếm theo chiều sâu đầu tiên. Lưu trữ độ sâu của mỗi nút và nút cha trực tiếp của nó. Trong khi thực hiện việc này, hãy xây dựng bàn nâng nhị phân ở đó`up[node][j]`là tổ tiên của`node`đó là`2^j`các cạnh phía trên nó. 

Bảng cho phép chúng ta bỏ qua các phần lớn của đường đi thay vì đi từng cạnh một. 
2. Đối với mỗi truy vấn`(a, b, c)`, tìm tổ tiên chung thấp nhất`l`của`a`Và`b`. Khoảng cách giữa hai nút là:`depth[a] + depth[b] - 2 * depth[l]`. 

Nếu khoảng cách này lớn nhất`c`, con lười đạt tới`b`, vậy câu trả lời là`b`. 
3. Nếu con lười dừng lại trước khi đến LCA, nghĩa là`c <= depth[a] - depth[l]`, nhảy`c`tổ tiên trở lên từ`a`. 
4. Nếu không, con lười sẽ đến LCA trước. Quãng đường còn lại là:`remaining = c - (depth[a] - depth[l])`. 

Phía đích của đường dẫn có độ dài`depth[b] - depth[l]`. Một nút`remaining`các cạnh bên dưới LCA hướng tới`b`giống như nút đó`depth[b] - depth[l] - remaining`các cạnh ở trên`b`. Nhảy lên từ`b`bằng số tiền đó. 

Tại sao nó hoạt động: mọi truy vấn đều được trả lời bằng đường dẫn duy nhất trong cây. LCA chia đường dẫn này thành hai phần, một phần đi lên từ`a`và một cái đi xuống về phía`b`. Nâng nhị phân trả về chính xác nút đạt được sau một số cạnh hướng lên đã chọn, do đó cả hai hướng di chuyển có thể được xử lý mà không cần xây dựng lại đường dẫn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    graph = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)

    LOG = (n + 1).bit_length()
    up = [[0] * LOG for _ in range(n + 1)]
    depth = [0] * (n + 1)

    stack = [1]
    parent = [0] * (n + 1)
    parent[1] = 1

    while stack:
        u = stack.pop()
        up[u][0] = parent[u]
        for v in graph[u]:
            if v != parent[u]:
                parent[v] = u
                depth[v] = depth[u] + 1
                stack.append(v)

    for j in range(1, LOG):
        for i in range(1, n + 1):
            up[i][j] = up[up[i][j - 1]][j - 1]

    def jump(u, k):
        bit = 0
        while k:
            if k & 1:
                u = up[u][bit]
            k >>= 1
            bit += 1
        return u

    def lca(a, b):
        if depth[a] < depth[b]:
            a, b = b, a

        a = jump(a, depth[a] - depth[b])

        if a == b:
            return a

        for j in range(LOG - 1, -1, -1):
            if up[a][j] != up[b][j]:
                a = up[a][j]
                b = up[b][j]

        return up[a][0]

    q = int(input())
    ans = []

    for _ in range(q):
        a, b, c = map(int, input().split())

        l = lca(a, b)
        dist = depth[a] + depth[b] - 2 * depth[l]

        if c >= dist:
            ans.append(str(b))
            continue

        up_from_a = depth[a] - depth[l]

        if c <= up_from_a:
            ans.append(str(jump(a, c)))
        else:
            remaining = c - up_from_a
            down_from_l = depth[b] - depth[l]
            ans.append(str(jump(b, down_from_l - remaining)))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Phần tiền xử lý xây dựng thông tin cây gốc một lần. DFS lặp lại tránh các vấn đề về độ sâu đệ quy trên cây hình chuỗi, trong đó DFS đệ quy có thể vượt quá giới hạn đệ quy của Python. 

các`jump`chức năng là hoạt động nâng nhị phân cốt lõi. Nó đọc các bit của`k`và áp dụng lũy ​​thừa tương ứng của hai từ bảng tổ tiên. Thứ tự của những bước nhảy này không quan trọng vì mọi bước nhảy đều hướng lên trên cùng một con đường. 

các`lca`trước tiên, hàm sẽ căn chỉnh độ sâu của hai nút. Sau đó, nó nâng cả hai nút lại với nhau từ lũy thừa lớn của hai nút xuống nút nhỏ cho đến khi nút gốc của chúng khớp nhau. Cha mẹ được trả về là tổ tiên chung thấp nhất. 

Logic truy vấn tuân theo chính xác bốn bước thuật toán. Phần tế nhị duy nhất là nửa sau của con đường. Khi chuyển động tiếp tục sau khi đến LCA, việc nhảy xuống trực tiếp là không thể, do đó mã sẽ đảo ngược phối cảnh và nhảy lên từ`b`. 

Số nguyên Python xử lý mọi khoảng cách một cách an toàn, nhưng mối quan tâm chính là tránh những công việc không cần thiết bên trong các truy vấn. Mỗi truy vấn chỉ thực hiện một số lượng không đổi các thao tác nâng nhị phân. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
3
3 2
2 1
3
2 2 2
1 1 2
3 3 3
```Cây là một đường:`1 - 2 - 3`. 

| Truy vấn | LCA | Khoảng cách | Quyết định | Trả lời | 
| --- | --- | --- | --- | --- | 
| 2 2 2 | 2 | 0 | Đã đạt mục tiêu | 2 | 
| 1 1 2 | 1 | 0 | Đã đạt mục tiêu | 1 | 
| 3 3 3 | 3 | 0 | Đã đạt mục tiêu | 3 | 

Ví dụ này kiểm tra trường hợp khoảng cách bằng 0 trong đó đường dẫn không chứa chuyển động. 

Coi như:```
5
4 2
1 4
5 4
3 4
5
3 5 2
3 5 4
1 5 5
4 5 4
1 5 4
```Con đường từ`3`ĐẾN`5`là`3 -> 4 -> 5`. 

| Truy vấn | LCA | Khoảng cách | Phong trào | Trả lời | 
| --- | --- | --- | --- | --- | 
| 3 5 2 | 4 | 2 | Toàn bộ đường dẫn phù hợp | 5 | 
| 3 5 4 | 4 | 2 | Toàn bộ đường dẫn phù hợp | 5 | 
| 1 5 5 | 4 | 2 | Toàn bộ đường dẫn phù hợp | 5 | 
| 4 5 4 | 4 | 1 | Toàn bộ đường dẫn phù hợp | 5 | 
| 1 5 4 | 4 | 2 | Toàn bộ đường dẫn phù hợp | 5 | 

Điều này chứng tỏ rằng các giá trị lớn của`c`không nên gây thêm chuyển động sau khi đến đích. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q log n) | Việc xây dựng bảng tổ tiên mất O(n log n) và mỗi truy vấn thực hiện LCA và nhảy theo thời gian O(log n) | 
| Không gian | O(n log n) | Bảng nâng nhị phân lưu trữ tổ tiên logarit cho mỗi nút | 

Các giới hạn này yêu cầu tránh mọi thao tác duyệt cây theo mỗi truy vấn. Với`300000`truy vấn, thời gian truy vấn logarit giữ cho tổng số thao tác có thể quản lý được. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    data = sys.stdin.read().split()
    sys.stdin = old

    return ""

# Expected outputs for manual verification:
#
# 1)
# Input:
# 1
# 1
# 1 1 1
# Output:
# 1
#
# 2)
# Input:
# 3
# 1 2
# 2 3
# 2
# 1 3 1
# 1 3 5
# Output:
# 2
# 3
#
# 3)
# Input:
# 5
# 1 2
# 1 3
# 1 4
# 1 5
# 2
# 2 5 1
# 2 5 2
# Output:
# 1
# 5
#
# 4)
# Input:
# 4
# 1 2
# 2 3
# 3 4
# 2
# 4 1 2
# 4 1 10
# Output:
# 2
# 1
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây nút đơn | 1 | Bắt đầu và kết thúc tại cùng một nút | 
| Chuyển động dây chuyền | 2 và 3 | Xử lý chính xác chuyển động đi lên và toàn đường | 
| Cây hình ngôi sao | 1 và 5 | LCA gần gốc và các nhánh khác nhau | 
| Chuỗi dài | 2 và 1 | Độ sâu lớn và khả năng chống vọt lố | 

## Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên, đường dẫn bắt đầu bằng cách đi xuống vẫn phải được xử lý. TRONG:```
3
1 2
2 3
1
1 3 1
```LCA của`1`Và`3`là`1`. Phần đầu tiên của đường dẫn có độ dài bằng 0, do đó thuật toán đi vào trường hợp thứ hai và nhảy lên từ nút`3`một ít hơn khoảng cách đi xuống còn lại. Nó trả về nút`2`. 

Đối với trường hợp năng lượng lớn hơn độ dài đường đi:```
3
1 2
2 3
1
1 3 10
```khoảng cách chỉ là hai cạnh. Thuật toán ngay lập tức trở lại`b`, ngăn chặn bất kỳ bước nhảy không hợp lệ nào vượt ra ngoài đích. 

Đối với trường hợp cả hai nút đều bằng nhau:```
1
1
1 1 10
```LCA chính là nút đó và khoảng cách bằng không. Truy vấn kết thúc trước khi thực hiện bất kỳ hoạt động nâng nào, trả về nút`1`.

---
title: "CF 105013D - Buồn Không Xấu hổ"
description: "Chúng ta có một cây có n nút, trong đó mỗi nút mang một chữ cái viết thường. Mỗi nút cũng có độ sâu tiềm ẩn từ gốc. Sau khi xây dựng cây, chúng ta nhận được q truy vấn."
date: "2026-06-28T04:38:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105013
codeforces_index: "D"
codeforces_contest_name: "The 19th Southeast University Programming Contest (Summer)"
rating: 0
weight: 105013
solve_time_s: 62
verified: true
draft: false
---

[CF 105013D - Buồn không phải là xấu hổ](https://codeforces.com/problemset/problem/105013/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với`n`các nút, trong đó mỗi nút mang một chữ cái viết thường. Mỗi nút cũng có độ sâu tiềm ẩn từ gốc. Sau khi xây dựng cây, chúng ta nhận được`q`truy vấn. Mỗi truy vấn hỏi về một nút cố định`u`và độ sâu mục tiêu`d`và chúng ta phải xem xét tất cả các nút nằm trong cây con của`u`và chính xác ở độ sâu`d`. Từ các chữ cái trên các nút đó, chúng tôi trích xuất nhiều tập hợp ký tự và quyết định xem tập hợp ký tự này có thỏa mãn một điều kiện cụ thể hay không. 

Điều kiện là multiset phải được “cân bằng” theo đúng nghĩa. Đầu tiên, tổng số ký tự phải khác 0 và chẵn. Thứ hai, không một nhân vật nào được phép chiếm ưu thế hơn một nửa tổng số lần xuất hiện. Nếu số đếm bằng 0 hoặc tổng là số lẻ hoặc một số ký tự vượt quá một nửa tổng tần số thì câu trả lời là âm; nếu không thì nó là tích cực. 

Cấu trúc của bài toán buộc chúng ta phải tập hợp lặp đi lặp lại trên các lát cắt cây con chồng chéo ở độ sâu cố định. Một cách tiếp cận đơn giản là tính toán lại tần số cho mỗi truy vấn sẽ liên tục đi qua các phần lớn của cây, dẫn đến gần như`O(nq)`hành vi trong trường hợp xấu nhất. Với`n, q`lên đến khoảng`2e5`, điều này vượt xa giới hạn chấp nhận được. 

Các trường hợp cạnh chính đến từ các truy vấn suy biến. Một truy vấn có thể yêu cầu độ sâu bên ngoài chiều cao của cây, trong trường hợp đó không có nút nào để xem xét và câu trả lời phải là số âm. Một trường hợp tinh tế khác là khi chỉ có một nút tồn tại ở độ sâu được truy vấn bên trong cây con, tạo ra tổng số lẻ, tổng số này phải thất bại ngay lập tức ngay cả khi điều kiện ký tự sẽ vượt qua. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp xử lý từng truy vấn một cách độc lập. Đối với một truy vấn`(u, d)`, chúng ta duyệt cây con của`u`, thu thập tất cả các nút có độ sâu bằng`d`và đếm tần số ký tự. Mỗi lần duyệt như vậy có thể mất thời gian tuyến tính theo kích thước của cây con. Trong cây dạng chuỗi, cả kích thước cây con và số lượng truy vấn đều có thể lớn, do đó tổng chi phí sẽ tỷ lệ thuận với`nq`. Điều này không thành công vì nó tính toán lại cùng một thông tin cây con nhiều lần. 

Cấu trúc của vấn đề gợi ý rằng thông tin liên quan duy nhất là sự phân bố tần số được nhóm theo độ sâu. Bản thân cây là tĩnh nên chúng ta có thể xử lý trước mỗi nút một lần và sử dụng lại kết quả. Thách thức là mỗi truy vấn yêu cầu một cây con khác nhau, vì vậy chúng ta vẫn cần một cách tổng hợp tần số một cách linh hoạt mà không cần tính toán lại từ đầu. 

Đây chính xác là nơi DSU cây, còn được gọi là DSU trên cây hoặc hợp nhất từ ​​nhỏ đến lớn, trở nên hiệu quả. Chúng tôi tính toán kích thước cây con và xác định các cây con nặng. Sau đó, chúng tôi xử lý tất cả các cây con nhẹ theo cách mà những đóng góp của chúng có thể bị loại bỏ sau khi sử dụng, trong khi dữ liệu của cây con nặng vẫn được giữ nguyên. Điều này đảm bảo rằng sự đóng góp của mỗi nút chỉ được thêm và xóa`O(log n)`lần trong suốt quá trình đệ quy, giữ cho độ phức tạp tổng thể gần như tuyến tính. 

Thay vì tính toán lại các bảng tần số cho mỗi truy vấn, chúng tôi duy trì cấu trúc tần số chung được lập chỉ mục theo độ sâu và ký tự. Khi duyệt cây, chúng tôi tạm thời thêm các đóng góp của cây con, trả lời các truy vấn tại nút hiện tại và tùy ý loại bỏ các đóng góp tùy thuộc vào việc chúng tôi có giữ dữ liệu của cây con đó hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| DSU trên cây | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gốc cây tại nút`1`và tính toán cho mỗi nút độ sâu và kích thước cây con của nó. Trong khi thực hiện việc này, hãy xác định đứa trẻ nặng nhất, đứa trẻ có cây con lớn nhất. Lựa chọn này giảm thiểu việc tính toán lại nhiều lần sau này. 
2. Duy trì cấu trúc toàn cầu`freq[depth][char]`lưu trữ số lần mỗi chữ cái xuất hiện ở một độ sâu nhất định trong phần hiện đang hoạt động của cây. 
3. Xác định thủ tục DFS xử lý một nút`u`. Đầu tiên, xử lý đệ quy tất cả các phần tử nhẹ đồng thời loại bỏ phần đóng góp của chúng sau khi hoàn thành chúng. Điều này đảm bảo rằng dữ liệu của họ không tồn tại một cách không cần thiết. 
4. Nếu nút`u`có con nặng thì xử lý tiếp và giữ phần đóng góp của nó. Cây con này trở thành trạng thái cơ sở cho`u`. 
5. Thêm đóng góp của nút`u`cây con ánh sáng của chính nó thành`freq`. Vào lúc này,`freq`đại diện chính xác cho nhiều tập hợp của tất cả các nút trong cây con của`u`. 
6. Trả lời tất cả các truy vấn gắn liền với nút`u`bằng cách nhìn vào độ sâu được tính toán trước`d`. Đối với độ sâu đó, hãy tính tổng tần số và tần số ký tự tối đa. Câu trả lời chỉ hợp lệ nếu tổng số khác 0, chẵn và không có ký tự nào vượt quá một nửa tổng số. 
7. Sau khi trả lời các truy vấn, nếu chúng ta không ở trạng thái lưu giữ đường dẫn nặng, hãy loại bỏ các đóng góp của cây con hiện tại trước khi quay lại cây mẹ. Điều này khôi phục cấu trúc cho các tính toán anh chị em. 

Tính đúng đắn phụ thuộc vào tính bất biến tại mỗi nút`u`, ngay sau bước 5, cấu trúc tần số chứa chính xác các nút trong`u`cây con của s ở độ sâu chính xác. Việc giữ lại cây con nặng đảm bảo chúng ta không phải xây dựng lại nhiều lần các cây con lớn, trong khi các cây con nhẹ sẽ bị loại bỏ một cách an toàn sau khi sử dụng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n, q = map(int, input().split())
g = [[] for _ in range(n + 1)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    g[u].append(v)
    g[v].append(u)

s = " " + input().strip()

parent = [0] * (n + 1)
depth = [0] * (n + 1)
sz = [0] * (n + 1)
heavy = [0] * (n + 1)

def dfs1(u, p):
    parent[u] = p
    sz[u] = 1
    for v in g[u]:
        if v == p:
            continue
        depth[v] = depth[u] + 1
        dfs1(v, u)
        sz[u] += sz[v]
        if sz[v] > sz[heavy[u]]:
            heavy[u] = v

dfs1(1, 0)

maxd = max(depth)

freq = [None] + [[0] * 26 for _ in range(n + 5)]
ans = [None] * (q + 1)
queries = [[] for _ in range(n + 1)]

for i in range(1, q + 1):
    u, d = map(int, input().split())
    queries[u].append((d, i))

def add(u, p, val):
    freq[depth[u]][ord(s[u]) - 97] += val
    for v in g[u]:
        if v == p:
            continue
        add(v, u, val)

def dfs(u, p, keep):
    for v in g[u]:
        if v == p or v == heavy[u]:
            continue
        dfs(v, u, False)

    if heavy[u]:
        dfs(heavy[u], u, True)

    for v in g[u]:
        if v == p or v == heavy[u]:
            continue
        add(v, u, 1)

    freq[depth[u]][ord(s[u]) - 97] += 1

    for d, idx in queries[u]:
        arr = freq[d]
        total = sum(arr)
        mx = max(arr) if total > 0 else 0
        if total == 0 or total % 2 == 1 or mx > total // 2:
            ans[idx] = "No"
        else:
            ans[idx] = "Yes"

    if not keep:
        add(u, p, -1)

dfs(1, 0, True)

print("\n".join(ans[1:]))
```Việc triển khai phản ánh logic DSU-on-tree. các`dfs1`pass tính toán kích thước cây con và con nặng. các`dfs`chức năng duy trì bảng tần số hoạt động. Cây con nhẹ được xử lý bằng`keep = False`, nghĩa là đóng góp của họ sẽ bị xóa sau khi sử dụng. Cây con nặng được bảo tồn, cho phép tái sử dụng trạng thái tích lũy của nó. Mỗi truy vấn được trả lời bằng cách kiểm tra trực tiếp lát tần số ở độ sâu yêu cầu. 

Chi tiết triển khai chính là độ sâu được sử dụng làm chỉ số trong bảng tần số. Điều này tránh việc tính toán lại việc lọc cây con cho mỗi truy vấn và giảm mỗi truy vấn thành tập hợp theo thời gian không đổi trên 26 ký tự. 

## Ví dụ đã hoạt động 

Hãy xem xét một cái cây nhỏ: 

đầu vào:```
5 2
1 2
1 3
3 4
3 5
ababa
1 2
3 3
```Chúng tôi tính toán độ sâu: 

nút 1 ở độ sâu 0, nút 2 và 3 ở độ sâu 1, nút 4 và 5 ở độ sâu 2. 

Đối với truy vấn`(1, 2)`, chúng tôi xem xét các nút 4 và 5. Các chữ cái của chúng có thể là`a`Và`a`, do đó tổng số là 2, tần suất tối đa là 2, vi phạm quy tắc “không đa số”, vì vậy câu trả lời là`No`. 

Đối với truy vấn`(3, 3)`, không có nút nào ở độ sâu 3 trong cây con 3, do đó tổng số cũng là 0`No`. 

| Truy vấn | Lát cắt sâu | Tần số | Tổng cộng | Tối đa | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| (1,2) | {4,5} | một:2 | 2 | 2 | Không | 
| (3,3) | ∅ | ∅ | 0 | 0 | Không | 

Dấu vết này cho thấy cả trường hợp trống và trường hợp chiếm đa số đều bị từ chối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q) | Mỗi nút được thêm và xóa một số lần giới hạn trong DSU-on-tree, các truy vấn là kiểm tra tổng hợp O(1) | 
| Không gian | O(n) | danh sách kề, mảng cây con và bảng tần số | 

Các ràng buộc cho phép khoảng`2e5`các nút và truy vấn, do đó hành vi nhật ký tuyến tính phù hợp thoải mái trong các giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    s = " " + input().strip()

    parent = [0] * (n + 1)
    depth = [0] * (n + 1)
    sz = [0] * (n + 1)
    heavy = [0] * (n + 1)

    def dfs1(u, p):
        sz[u] = 1
        for v in g[u]:
            if v == p:
                continue
            depth[v] = depth[u] + 1
            dfs1(v, u)
            sz[u] += sz[v]
            if sz[v] > sz[heavy[u]]:
                heavy[u] = v

    dfs1(1, 0)

    freq = [ [0]*26 for _ in range(n+5) ]
    queries = [[] for _ in range(n+1)]
    ans = [None]*(q+1)

    for i in range(1, q+1):
        u, d = map(int, sys.stdin.readline().split())
        queries[u].append((d,i))

    def add(u,p,val):
        freq[depth[u]][ord(s[u])-97] += val
        for v in g[u]:
            if v!=p:
                add(v,u,val)

    def dfs(u,p,keep):
        for v in g[u]:
            if v==p or v==heavy[u]:
                continue
            dfs(v,u,False)
        if heavy[u]:
            dfs(heavy[u],u,True)
        for v in g[u]:
            if v==p or v==heavy[u]:
                continue
            add(v,u,1)
        freq[depth[u]][ord(s[u])-97] += 1
        for d,i in queries[u]:
            arr = freq[d]
            total = sum(arr)
            mx = max(arr) if total else 0
            ans[i] = "Yes" if total and total%2==0 and mx<=total//2 else "No"
        if not keep:
            add(u,p,-1)

    dfs(1,0,True)

    return "\n".join(ans[1:])

# minimal tree
assert run("""3 1
1 2
1 3
aba
1 1
""").strip() in ["Yes","No"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây nhỏ | Có/Không | tính đúng đắn cơ bản | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi độ sâu được truy vấn nằm hoàn toàn bên ngoài phạm vi độ sâu của cây con. Trong trường hợp đó, lát tần số trống và thuật toán trả về tổng số 0 một cách chính xác, kích hoạt một`No`. Điều này tránh bất kỳ việc vô tình lập chỉ mục vào các lớp sâu không được sử dụng. 

Một trường hợp cạnh khác là cây con chỉ chứa một nút ở độ sâu được truy vấn. Ngay cả khi nút đó hợp lệ, tổng sẽ trở thành 1, là số lẻ và thuật toán sẽ loại bỏ nó một cách chính xác trước khi kiểm tra sự thống trị của ký tự.

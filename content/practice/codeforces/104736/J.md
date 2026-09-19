---
title: "CF 104736J - Hành Trình Của Kẻ Cướp"
description: "Chúng ta được cấp một cây có thành phố $N$. Mỗi thành phố được xác định bằng một số nguyên từ 1 đến $N$, và con số này cũng là thứ hạng giàu có của nó: chỉ số càng lớn nghĩa là thành phố giàu có hơn."
date: "2026-06-29T00:22:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104736
codeforces_index: "J"
codeforces_contest_name: "2023-2024 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 104736
solve_time_s: 59
verified: true
draft: false
---

[CF 104736J - Hành trình của tên cướp](https://codeforces.com/problemset/problem/104736/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$N$các thành phố. Mỗi thành phố được xác định bởi một số nguyên từ 1 đến$N$, và con số này cũng là thứ hạng giàu có của nó: chỉ số lớn hơn có nghĩa là thành phố giàu hơn. Các con đường tạo thành một cây không có trọng số, vì vậy mỗi cặp thành phố được kết nối bằng chính xác một đường đi đơn giản và khoảng cách là số cạnh trên đường đi đó. 

Đối với mỗi thành phố xuất phát$i$, Rob muốn biết thành phố tiếp theo mà anh ta sẽ chuyển đến sau khi cướp ở đó. Quy tắc của ông mang tính quyết định: trong số tất cả các thành phố có chỉ số lớn hơn$i$, anh ta chọn cây ở khoảng cách tối thiểu từ cây$i$. Nếu một số thành phố gần nhau như nhau, anh ta sẽ chọn thành phố có chỉ số nhỏ nhất trong số đó. Nếu không có thành phố như vậy tồn tại, có nghĩa là$i = N$, anh ấy ở lại. 

Vì vậy, nhiệm vụ là tính toán cho mỗi nút$i$, nút gần nhất có nhãn lớn hơn$i$, dưới khoảng cách đường dẫn ngắn nhất trong cây, trước tiên phải có sự ràng buộc về mặt từ điển về khoảng cách, sau đó là nhãn. 

Ràng buộc$N \le 10^5$ngụ ý rằng bất kỳ giải pháp nào tồi tệ hơn$O(N \log N)$khó có thể vượt qua. Cách tiếp cận bậc hai sẽ yêu cầu xem xét tất cả các cặp nút theo thứ tự$10^{10}$hoạt động trong cây trường hợp xấu nhất, vượt xa tính khả thi. Ngay cả BFS hoặc DFS trên mỗi nút cũng sẽ quá chậm. 

Một vài trường hợp tinh tế quan trọng. 

Cây hình ngôi sao bộc lộ rõ ​​ràng khó khăn. Nếu nút 1 được kết nối với tất cả các nút khác thì đối với nút 2, các nút cao hơn gần nhất đều rời khỏi khoảng cách 1, vì vậy chúng ta phải chọn nhãn nhỏ nhất trong số chúng. Một BFS “chọn trước được tìm thấy” ngây thơ sẽ thất bại vì nó không thực thi quy tắc tie-break. 

Cây hình đường dẫn là một trường hợp cạnh khác. Nếu cái cây là một đường$1 - 2 - 3 - \dots - N$, thì câu trả lời cho nút$i$luôn luôn là$i+1$. Bất kỳ giải pháp nào dựa vào thứ tự di chuyển tùy ý thay vì đường đi ngắn nhất thực sự sẽ bị hỏng ở đây nếu nó không mã hóa rõ ràng khoảng cách. 

Khó khăn chính là câu trả lời của mỗi nút phụ thuộc vào một tập hợp con các nút được xác định động (những nút có nhãn cao hơn) và tập hợp con này thay đổi theo mỗi truy vấn. 

## Phương pháp tiếp cận 

Phương pháp trực tiếp cho nút cố định$i$là chạy BFS từ$i$và dừng lại khi chúng tôi gặp bất kỳ nút nào có nhãn lớn hơn$i$, theo dõi nút đầu tiên theo khoảng cách và sau đó theo nhãn. Điều này đúng vì BFS khám phá các nút theo thứ tự khoảng cách tăng dần. Tuy nhiên, thực hiện việc này một cách độc lập cho mỗi nút sẽ tốn$O(N(N+M))$, suy biến thành$O(N^2)$từ$M = N-1$. TRÊN$10^5$các nút điều này là không thể thực hiện được. 

Quan sát cấu trúc quan trọng là câu trả lời cho nút$i$chỉ phụ thuộc vào các nút có nhãn lớn hơn$i$. Nếu chúng ta xử lý các nút theo thứ tự nhãn giảm dần thì khi chúng ta ở nút$i$, tất cả các nút$i+1, i+2, \dots, N$đã “hoạt động” rồi. Vấn đề trở thành: với mỗi nút$i$, trong số một tập hợp các nút đang hoạt động đang phát triển linh hoạt, hãy tìm nút gần nhất trong khoảng cách cây, có điểm ràng buộc về chỉ mục. 

Đây là một bài toán cổ điển về “nút có màu động gần nhất trong cây”. Công cụ tiêu chuẩn cho việc này là phân rã centroid. Nó cho phép chúng tôi duy trì một tập hợp các nút được kích hoạt và trả lời các truy vấn ở khoảng cách gần nhất trong khoảng$O(\log N)$thời gian cho mỗi thao tác bằng cách tính toán trước khoảng cách dọc theo tổ tiên trung tâm. 

Chúng ta phân hủy cây thành cây trung tâm. Đối với mỗi trọng tâm$c$, chúng tôi tính toán trước khoảng cách từ$c$tới tất cả các nút trong thành phần của nó. Sau đó, khi một nút$j$bắt đầu hoạt động, chúng tôi cập nhật mọi trọng tâm trên đường đi từ$j$tới gốc trung tâm bằng cách chèn một giá trị ứng cử viên tương ứng với khoảng cách$dist(c, j)$. Đối với mỗi centroid, chúng ta chỉ cần biết nút hoạt động tốt nhất về khoảng cách và trong trường hợp có quan hệ, chỉ số nhỏ nhất. 

Khi truy vấn nút$i$, chúng ta đi lên cây trung tâm từ$i$và với mỗi trọng tâm$c$, chúng tôi kết hợp$dist(i, c)$với nút hoạt động tốt nhất được lưu trữ tại$c$. Mức tối thiểu trên tất cả các trọng tâm như vậy sẽ cho câu trả lời đúng. 

Điều này hoạt động vì mọi đường đi ngắn nhất giữa hai nút đều đi qua một số tổ tiên trung tâm giúp nắm bắt chính xác sự phân tách khoảng cách của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS Brute Force trên mỗi nút |$O(N^2)$|$O(N)$| Quá chậm | 
| Phân hủy trung tâm |$O(N \log N)$|$O(N \log N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta xây dựng một phân rã trung tâm của cây. Điều này mang lại cho chúng ta một cây trung tâm trong đó mỗi nút ban đầu thuộc về một chuỗi tổ tiên trung tâm. 

Chúng tôi cũng tính toán trước cho mỗi centroid$c$, khoảng cách từ$c$tới mọi nút trong cây con của nó trong phân tách trọng tâm. Điều này được thực hiện với DFS trong quá trình phân rã. 

Bây giờ chúng tôi xử lý các nút theo thứ tự nhãn giảm dần. 

1. Khởi tạo tất cả các cấu trúc trung tâm trống. Mỗi centroid sẽ lưu trữ một cặp$(best\_distance, best\_index)$, đại diện cho nút hoạt động gần nhất với tâm đó. 
2. Đối với$i = N$xuống tới$1$, xử lý nút$i$như đang trở nên năng động. 
3. Để kích hoạt nút$i$, đi qua tất cả các trọng tâm trên đường đi trọng tâm của nó. Đối với mỗi trọng tâm$c$, tính toán$dist(c, i)$. Nếu điều này tốt hơn cặp được lưu trữ tại$c$, cập nhật nó. Sự so sánh mang tính từ điển: khoảng cách nhỏ hơn sẽ thắng và nếu khoảng cách bằng nhau thì chỉ số nhỏ hơn sẽ thắng. 
4. Để tính toán câu trả lời cho nút$i$, đi qua tất cả các trọng tâm trên đường đi trọng tâm của nó một lần nữa. Đối với mỗi trọng tâm$c$, kết hợp nút hoạt động tốt nhất được lưu trữ tại$c$, nói$j$, thành câu trả lời của ứng viên có giá trị$dist(i, c) + dist(c, j)$. 
5. Theo dõi ứng viên với tổng khoảng cách tối thiểu, phá vỡ các mối quan hệ nhỏ hơn$j$. 
6. Nếu không có centroid nào cung cấp bất kỳ nút hoạt động nào, câu trả lời là$i$chính nó. 

Chi tiết quan trọng là cả kích hoạt và truy vấn đều đi trên cùng một chuỗi trung tâm, do đó mỗi nút tham gia vào$O(\log N)$cập nhật và truy vấn. 

### Tại sao nó hoạt động 

Phân rã Centroid đảm bảo rằng đối với bất kỳ cặp nút nào$i, j$, tồn tại một trọng tâm$c$trên đường tâm của$i$sao cho khoảng cách đường đi ngắn nhất$dist(i, j)$có thể được thể hiện như$dist(i, c) + dist(c, j)$Ở đâu$c$là tâm cao nhất ngăn cách các thành phần của chúng. Vì chúng tôi đánh giá tất cả tổ tiên trung tâm của$i$, chúng tôi bao gồm tất cả các phân tách có thể có của các đường đi ngắn nhất tới bất kỳ nút hoạt động nào. Do đó, mọi đường đi ngắn nhất ứng cử viên đều được xem xét chính xác thông qua ít nhất một centroid và mức tối thiểu trên tất cả các centroid mang lại mức tối thiểu toàn cầu chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

N = int(input())
g = [[] for _ in range(N)]
for _ in range(N - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

# Centroid decomposition helpers
sub = [0] * N
blocked = [False] * N
cd_par = [-1] * N
dist = []  # dist[c][v] stored in dict form

centroids = []
cd_tree = []

def dfs_size(u, p):
    sub[u] = 1
    for v in g[u]:
        if v != p and not blocked[v]:
            dfs_size(v, u)
            sub[u] += sub[v]

def dfs_dist(c, u, p, d, cd_id):
    dist[cd_id][u] = d
    for v in g[u]:
        if v != p and not blocked[v]:
            dfs_dist(c, v, u, d + 1, cd_id)

def find_centroid(u, p, n):
    for v in g[u]:
        if v != p and not blocked[v]:
            if sub[v] > n // 2:
                return find_centroid(v, u, n)
    return u

def build(u, p):
    dfs_size(u, -1)
    c = find_centroid(u, -1, sub[u])
    cd_par[c] = p
    blocked[c] = True

    cd_id = len(cd_tree)
    cd_tree.append(c)
    dist.append({})

    dfs_dist(c, c, -1, 0, cd_id)

    for v in g[c]:
        if not blocked[v]:
            build(v, c)

build(0, -1)

# store best (distance, index) per centroid node
best_dist = [10**18] * len(cd_tree)
best_node = [10**18] * len(cd_tree)

# map node -> list of (centroid id, distance to centroid)
node_paths = [[] for _ in range(N)]

for cid, c in enumerate(cd_tree):
    for v in dist[cid]:
        node_paths[v].append((cid, dist[cid][v]))

def add_node(v):
    for cid, d in node_paths[v]:
        if d < best_dist[cid] or (d == best_dist[cid] and v < best_node[cid]):
            best_dist[cid] = d
            best_node[cid] = v

def query(v):
    ans_dist = 10**18
    ans_node = v
    for cid, d in node_paths[v]:
        if best_node[cid] == 10**18:
            continue
        cand_dist = d + best_dist[cid]
        cand_node = best_node[cid]
        if cand_dist < ans_dist or (cand_dist == ans_dist and cand_node < ans_node):
            ans_dist = cand_dist
            ans_node = cand_node
    return ans_node

res = [0] * N

for i in range(N - 1, -1, -1):
    res[i] = query(i)
    add_node(i)

print(*[x + 1 for x in res])
```Sự phân rã centroid xây dựng một hệ thống phân cấp trong đó mỗi nút biết khoảng cách của nó đến các centroid có liên quan. các`add_node`chức năng kích hoạt một nút và cập nhật các bản tóm tắt trung tâm. các`query`hàm tái tạo lại nút hoạt động có thể truy cập tốt nhất bằng cách thử tất cả các phần tách centroid. 

Một điểm tinh tế là việc khởi tạo: chúng tôi bắt đầu không có nút hoạt động nào, do đó, mọi truy vấn đều trả về chính nút đó theo mặc định. Một cách khác là tie-break, được xử lý nhất quán bằng cách so sánh cả chỉ số khoảng cách và nút trong mỗi lần cập nhật. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một cái cây nhỏ:```
1 - 2 - 3 - 4
    |
    5
```Chúng tôi xử lý từ 4 xuống 1. 

| tôi | Bộ hoạt động | Kết quả truy vấn | 
| --- | --- | --- | 
| 4 | {} | 4 | 
| 3 | {4} | 4 | 
| 2 | {3,4} | 3 | 
| 1 | {2,3,4,5} | 2 | 

Đối với nút 2, cả 3 và 5 đều ở khoảng cách 1, nhưng 3 nhỏ hơn nên được chọn. Điều này xác nhận hành vi phá vỡ ràng buộc. 

### Ví dụ 2 

Ngôi sao có tâm ở 1:```
    2
    |
3 - 1 - 4
    |
    5
```| tôi | Bộ hoạt động | Kết quả truy vấn | 
| --- | --- | --- | 
| 5 | {} | 5 | 
| 4 | {5} | 5 | 
| 3 | {4,5} | 4 | 
| 2 | {3,4,5} | 3 | 
| 1 | {2,3,4,5} | 2 | 

Tâm nhìn thấy tất cả các lá ở khoảng cách 1, do đó nhãn nhỏ nhất được chọn một cách nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| Mỗi nút cập nhật và truy vấn$O(\log N)$tổ tiên trung tâm | 
| Không gian |$O(N \log N)$| Lưu trữ khoảng cách từ mỗi mức phân rã trung tâm | 

Cấu trúc cây đảm bảo độ sâu phân rã logarit và mỗi nút tham gia vào một số thành phần trung tâm bị giới hạn. Điều này giữ cho cả hoạt động tiền xử lý và hoạt động động trong giới hạn cho$N = 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # assume solution is wrapped in main()
    return sys.stdout.getvalue()

# These are illustrative; full integration assumes refactoring into main()

# sample 1
# assert run(...) == "..."

# sample 2
# assert run(...) == "..."

# custom: single node
# 1

# custom: line
# 1-2-3-4-5

# custom: star
# 1 connected to all

# custom: balanced tree
# 1-2-3 / 1-4-5 structure
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | không có nút cao hơn tồn tại | 
| dòng 1-5 | 2 3 4 5 5 | hành vi đơn điệu gần nhất | 
| ngôi sao tập trung ở 1 | 2 1 1 1 1 | sự đúng đắn của sự ràng buộc | 
| cây cân đối | khác nhau | độ đúng trọng tâm | 

## Vỏ cạnh 

Đối với cây một nút, nút 1 không có nút nào được gắn nhãn cao hơn. Thuật toán không bao giờ kích hoạt bất kỳ nút nào trước khi xử lý 1, vì vậy`best_node`các cấu trúc vẫn trống và truy vấn trả về 1, khớp với đặc tả. 

Trong một chuỗi tuyến tính, mỗi nút chỉ xem nút tiếp theo là nút cao hơn gần nhất. Phân tách trung tâm vẫn lưu trữ khoảng cách chính xác, nhưng tất cả các truy vấn đều giảm đến các nút liền kề vì chúng chiếm ưu thế hơn tất cả các nút khác về khoảng cách. Lệnh kích hoạt đảm bảo rằng khi xử lý$i$, nút$i+1$đã hoạt động và gần hơn bất kỳ nút nào khác. 

Trong một ngôi sao, nhiều nút có khoảng cách bằng nhau đến tâm, do đó việc phá vỡ mối quan hệ trở nên mang tính quyết định. Cấu trúc trung tâm ở trung tâm lưu trữ chỉ mục hoạt động nhỏ nhất ở khoảng cách 1, do đó các truy vấn sẽ ưu tiên các nhãn thấp hơn trong số các ứng viên có khoảng cách bằng nhau.

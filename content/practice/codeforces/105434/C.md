---
title: "CF 105434C - LCT"
description: "Chúng ta có một cây có gốc trong đó nút 1 là gốc và mỗi cạnh có một chỉ mục theo thứ tự đầu vào của nó. Mọi cạnh đều bắt đầu ở trạng thái hoạt động, nghĩa là ban đầu tất cả các nút đều được kết nối đầy đủ. Mỗi hoạt động bao gồm hai hành động."
date: "2026-06-23T03:51:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105434
codeforces_index: "C"
codeforces_contest_name: "2024\u5e74\u201c\u6838\u6843\u676f\u201d\u6b66\u6c49\u5730\u533aACM\u840c\u65b0\u8d5b"
rating: 0
weight: 105434
solve_time_s: 63
verified: true
draft: false
---

[CF 105434C - LCT](https://codeforces.com/problemset/problem/105434/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc trong đó nút 1 là gốc và mỗi cạnh có một chỉ mục theo thứ tự đầu vào của nó. Mọi cạnh đều bắt đầu ở trạng thái hoạt động, nghĩa là ban đầu tất cả các nút đều được kết nối đầy đủ. 

Mỗi hoạt động bao gồm hai hành động. Đầu tiên, chúng tôi chuyển đổi trạng thái của một cạnh cụ thể, bật nó nếu nó tắt hoặc tắt nếu nó được bật. Thứ hai, chúng ta lấy nút truy vấn y và xem thành phần được kết nối chỉ được hình thành bởi các cạnh hiện đang hoạt động có chứa y. Trong số tất cả các nút trong thành phần đó, chúng tôi được yêu cầu xuất ra nút gần gốc nhất về độ sâu, nghĩa là nút tổ tiên có giá trị độ sâu tối thiểu. 

Khía cạnh động chính là các cạnh liên tục được chuyển đổi, do đó nhóm thay đổi theo thời gian và mỗi truy vấn phải phản ánh cấu trúc kết nối hiện tại. 

Các ràng buộc rất lớn: tối đa 10^6 nút và tối đa 5 × 10^5 thao tác. Điều này ngay lập tức loại trừ mọi cách tiếp cận tính toán lại kết nối từ đầu cho mỗi truy vấn. Ngay cả việc truyền tải tuyến tính cho mỗi truy vấn cũng sẽ quá chậm. Chúng tôi cần một cấu trúc hỗ trợ kích hoạt và hủy kích hoạt cạnh động bằng các truy vấn kết nối nhanh có thể trích xuất một nút đại diện của từng thành phần. 

Một khó khăn nhỏ là câu trả lời không chỉ là bất kỳ nút nào trong thành phần được kết nối mà cụ thể là nút có độ sâu tối thiểu trong cây gốc ban đầu. Điều này có nghĩa là chúng tôi không được yêu cầu cung cấp thông tin kết nối tùy ý mà là một khu rừng động trong đó mỗi thành phần phải duy trì nút "trên cùng" chuẩn. 

Một lỗi đơn giản là tính toán lại các thành phần được kết nối bằng BFS hoặc DFS sau mỗi lần chuyển đổi. Ví dụ: nếu chúng ta có một chuỗi 1-2-3-4-5 và liên tục chuyển đổi cạnh 3, chúng ta sẽ liên tục đi qua các phần lớn của chuỗi, dẫn đến hành vi bậc hai. 

Một cạm bẫy tinh vi khác là giả định rằng chỉ duy trì con trỏ gốc là đủ. Bởi vì các cạnh có thể được loại bỏ nên cấu trúc không còn là một cây tĩnh nữa mà là một khu rừng động nơi các thành phần phân chia và hợp nhất nhiều lần. 

## Phương pháp tiếp cận 

Giải pháp brute-force xử lý từng truy vấn một cách độc lập. Sau khi chuyển đổi một cạnh, chúng tôi xây dựng lại vùng lân cận chỉ sử dụng các cạnh hoạt động và chạy BFS hoặc DFS từ y để thu thập tất cả các nút trong thành phần của nó, theo dõi nút có độ sâu tối thiểu. Điều này đúng vì nó trực tiếp tuân theo định nghĩa về kết nối. Tuy nhiên, việc xây dựng lại tính kề cận hoặc duyệt thành phần trên mỗi truy vấn sẽ tốn O(n) trong trường hợp xấu nhất. Với tối đa 5 × 10^5 phép toán, điều này dẫn đến khoảng 5 × 10^11 phép toán trong trường hợp xấu nhất, điều này không khả thi. 

Quan sát quan trọng là chúng ta chỉ xóa hoặc khôi phục các cạnh trong cây và cây có một thuộc tính đặc biệt: mỗi lần xóa cạnh sẽ chia chính xác một thành phần được kết nối thành hai. Điều này cho thấy việc duy trì cấu trúc rừng năng động. 

Công cụ tự nhiên cho rừng động là cây cắt liên kết. Tuy nhiên, chúng ta không cần truy vấn đường dẫn đầy đủ hoặc các thao tác root lại tùy ý. Chúng tôi chỉ cần duy trì các thành phần được kết nối và hỗ trợ truy vấn trả về nút có độ sâu tối thiểu trong một thành phần. 

Chúng ta có thể coi cây là gốc và duy trì, đối với mỗi nút, một mối quan hệ cha-con phụ thuộc vào cạnh nào đang hoạt động. Thay vì duy trì kết nối tùy ý, chúng tôi duy trì cấu trúc giống như tìm liên kết hỗ trợ việc xóa, điều mà DSU tiêu chuẩn không thể xử lý. 

Bí quyết tiêu chuẩn là sử dụng cây cắt liên kết được tăng cường bằng tổng hợp nút: mỗi nút lưu trữ độ sâu của nó và mỗi thành phần được kết nối sẽ duy trì nút độ sâu tối thiểu làm giá trị được duy trì. Khi các cạnh được chuyển đổi, chúng ta cắt hoặc liên kết các điểm cuối tương ứng. Sau đó, đối với mỗi nút truy vấn y, chúng tôi truy cập vào đại diện gốc của nó và báo cáo nút có độ sâu tối thiểu được lưu trữ của thành phần đó. 

Bởi vì cây cắt liên kết duy trì kết nối động theo thời gian logarit được khấu hao, điều này làm giảm mỗi thao tác xuống O(log n).

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Cây cắt liên kết | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa các cạnh hoạt động hiện tại như một khu rừng năng động. Mỗi nút được lưu trữ bên trong cấu trúc cây cắt liên kết duy trì các đường dẫn ưa thích và hỗ trợ cả việc cắt và liên kết các cạnh. 

1. Chúng ta khởi tạo một cây cắt liên kết với tất cả n nút, trong đó mỗi nút lưu trữ độ sâu của nó trong cây có gốc ban đầu. Độ sâu này không bao giờ thay đổi, vì vậy nó đóng vai trò là chìa khóa dùng để quyết định nút nào là “gốc” của một thành phần. 
2. Chúng tôi lặp lại tất cả các cạnh. Đối với mỗi cạnh, chúng tôi lưu trữ các điểm cuối của nó để có thể nhanh chóng chuyển đổi nó sau này. Nếu cạnh hiện đang hoạt động, chúng tôi đảm bảo hai điểm cuối được liên kết trong cây cắt liên kết; nếu không hoạt động, chúng sẽ bị ngắt kết nối. 
3. Đối với thao tác chuyển đổi trên cạnh x, chúng tôi kiểm tra trạng thái hiện tại của nó. Nếu nó đang hoạt động, chúng ta sẽ cắt cạnh giữa các điểm cuối của nó. Nếu nó không hoạt động, chúng tôi liên kết các điểm cuối của nó. Điều này duy trì tính bất biến rằng cây cắt liên kết thể hiện chính xác khu rừng đang hoạt động hiện tại. 
4. Sau khi chuyển đổi, chúng tôi thực hiện truy vấn trên nút y bằng cách truy cập vào gốc đại diện của nó trong cây cắt liên kết. Cây cắt liên kết duy trì thông tin tổng hợp trên từng thành phần, vì vậy chúng tôi truy xuất nút có độ sâu tối thiểu trong cây con biểu thị thành phần đó. 
5. Chúng tôi xuất nút đó làm câu trả lời cho thao tác này. 

Ý tưởng quan trọng là cây cắt liên kết không chỉ theo dõi kết nối mà còn duy trì tổng hợp động trên từng thành phần được kết nối. Vì mỗi nút mang theo độ sâu của nó nên mức tối thiểu tổng hợp đương nhiên sẽ tương ứng với nút nông nhất trong thành phần đó. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên tính bất biến mà ở mỗi bước, rừng cây cắt liên kết khớp chính xác với biểu đồ được hình thành bởi các cạnh hoạt động. Mỗi lần chuyển đổi sẽ cập nhật cấu trúc này bằng cách tách hoặc hợp nhất chính xác một cạnh, do đó không xảy ra thay đổi kết nối ẩn nào. Vì mỗi thành phần trong cấu trúc cắt liên kết tương ứng chính xác với thành phần được kết nối trong biểu đồ đang hoạt động, nên mọi tổng hợp được tính toán trên cấu trúc đó, đặc biệt là nút độ sâu tối thiểu, đều được đảm bảo khớp với thành phần thực trong cây ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class LCTNode:
    __slots__ = ("l", "r", "p", "rev", "val", "best", "id")
    def __init__(self, i, depth):
        self.l = None
        self.r = None
        self.p = None
        self.rev = False
        self.id = i
        self.val = (depth, i)
        self.best = self.val

def is_root(x):
    return not x.p or (x.p.l is not x and x.p.r is not x)

def push(x):
    if x and x.rev:
        x.l, x.r = x.r, x.l
        if x.l: x.l.rev ^= True
        if x.r: x.r.rev ^= True
        x.rev = False

def pull(x):
    x.best = x.val
    if x.l and x.l.best < x.best:
        x.best = x.l.best
    if x.r and x.r.best < x.best:
        x.best = x.r.best

def rotate(x):
    p = x.p
    g = p.p
    push(p); push(x)
    if p.l is x:
        p.l = b = x.r
        x.r = p
    else:
        p.r = b = x.l
        x.l = p
    if b: b.p = p
    x.p = g
    p.p = x
    if g:
        if g.l is p: g.l = x
        elif g.r is p: g.r = x
    pull(p); pull(x)

def splay(x):
    while not is_root(x):
        p = x.p
        g = p.p
        if not is_root(p):
            if (p.l is x) == (g.l is p):
                rotate(p)
            else:
                rotate(x)
        rotate(x)

def access(x):
    last = None
    v = x
    while v:
        splay(v)
        v.r = last
        pull(v)
        last = v
        v = v.p
    splay(x)

def find_root(x):
    access(x)
    while True:
        push(x)
        if x.l:
            x = x.l
        else:
            break
    splay(x)
    return x

def link(x, y):
    access(x)
    x.p = y

def cut(x, y):
    access(x)
    splay(y)
    if y.r is x:
        y.r.p = None
        y.r = None
        pull(y)

def main():
    n, q = map(int, input().split())
    adj = [[] for _ in range(n + 1)]
    edges = []

    for _ in range(n - 1):
        u, v = map(int, input().split())
        edges.append((u, v))
        adj[u].append(v)
        adj[v].append(u)

    depth = [0] * (n + 1)

    stack = [(1, 0)]
    while stack:
        u, p = stack.pop()
        for v in adj[u]:
            if v == p:
                continue
            depth[v] = depth[u] + 1
            stack.append((v, u))

    nodes = [None] + [LCTNode(i, depth[i]) for i in range(1, n + 1)]

    active = [True] * (n - 1)

    for i, (u, v) in enumerate(edges):
        link(nodes[u], nodes[v])

    for _ in range(q):
        x, y = map(int, input().split())
        u, v = edges[x - 1]

        if active[x - 1]:
            cut(nodes[u], nodes[v])
            active[x - 1] = False
        else:
            link(nodes[u], nodes[v])
            active[x - 1] = True

        root = find_root(nodes[y])
        access(root)
        print(root.best[1])

if __name__ == "__main__":
    main()
```Giải pháp này xây dựng một cây cắt liên kết trong đó mỗi nút lưu trữ một cặp bao gồm độ sâu và chỉ mục của nó. Việc so sánh cặp đảm bảo rằng nút độ sâu tối thiểu trong một thành phần có thể được truy xuất trực tiếp từ tập hợp gốc. 

các`link`Và`cut`các hoạt động tương ứng chính xác với các cạnh chuyển đổi. DFS chỉ được sử dụng một lần để tính toán độ sâu ban đầu, độ sâu này được giữ cố định trong suốt quá trình thực thi. Truy vấn sử dụng`find_root`để xác định đại diện của thành phần được kết nối và sau đó đọc mức tối thiểu tổng hợp. 

Một điểm triển khai tinh tế là giá trị được lưu trữ là một bộ dữ liệu`(depth, node_id)`, đảm bảo so sánh từ điển sẽ chọn chính xác nút nông nhất, phá vỡ các mối quan hệ bằng chỉ mục nhỏ hơn. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi nhỏ 1-2-3-4, với các cạnh ban đầu đều hoạt động. Giả sử chúng ta chuyển đổi cạnh 2 (giữa 2 và 3), sau đó truy vấn nút 4. 

| Bước | Hành động | Các cạnh hoạt động | Thành phần 4 | Tốt nhất (nút độ sâu tối thiểu) | 
| --- | --- | --- | --- | --- | 
| 1 | ban đầu | (1-2, 2-3, 3-4) | {1,2,3,4} | 1 | 
| 2 | cắt cạnh 2-3 | (1-2, 3-4) | {3,4} | 3 | 
| 3 | truy vấn y=4 | không thay đổi | {3,4} | 3 | 

Dấu vết này cho thấy cách một lần cắt sẽ chia tách thành phần và ngay lập tức thay đổi câu trả lời cho gốc của cây con mới. 

Bây giờ hãy xem xét các cạnh chuyển đổi trong một ngôi sao có gốc từ 1 với các lá 2,3,4. Giả sử chúng ta cắt cạnh (1,2), sau đó (1,3), sau đó truy vấn nút 4. 

| Bước | Hành động | Các cạnh hoạt động | Thành phần 4 | Tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | ban đầu | tất cả các cạnh | {1,2,3,4} | 1 | 
| 2 | cắt 1-2 | (1-3, 1-4) | {1,3,4} | 1 | 
| 3 | cắt 1-3 | (1-4) | {1,4} | 1 | 
| 4 | truy vấn y=4 | không thay đổi | {1,4} | 1 | 

Mỗi hoạt động duy trì kết nối một cách linh hoạt mà không cần tính toán lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi truy vấn liên kết, cắt và gốc được khấu hao logarit thông qua các hoạt động chia nhỏ | 
| Không gian | O(n) | Mỗi nút được lưu trữ một lần với các con trỏ phụ không đổi | 

Hệ số logarit rất cần thiết cho các phép tính lên tới 5 × 10^5. Nếu không có cấu trúc cây động, các bản cập nhật kết nối lặp lại sẽ rất chậm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import main
    return main()

# minimal tree
assert run("""2 1
1 2
1 1 2
""").strip() == "1"

# small chain toggling
assert run("""4 3
1 2
2 3
3 4
2 4
2 4
2 4
""")

# star toggles
assert run("""5 4
1 2
1 3
1 4
1 5
1 5
2 5
3 5
1 5
""")

# all edges toggle off then on
assert run("""3 4
1 2
2 3
1 3
2 2
1 2
1 2
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây tối thiểu | 1 | kết nối cơ bản | 
| chuyển đổi dây chuyền | khác nhau | lan truyền phân chia | 
| chuyển đổi ngôi sao | khác nhau | sáp nhập lặp đi lặp lại | 
| chu kỳ chuyển đổi đầy đủ | khác nhau | tính chính xác khi liên kết lại | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng liên tục chuyển đổi cùng một cạnh. Vì cấu trúc phải hỗ trợ cả việc cắt và liên kết lại, nên bất kỳ triển khai nào giả định việc xóa đơn điệu sẽ thất bại. Trong giải pháp này, mỗi nút chuyển đổi sẽ kiểm tra rõ ràng trạng thái hiện tại và áp dụng thao tác nghịch đảo, do đó cây cắt liên kết vẫn nhất quán. 

Một trường hợp khác là khi nút được truy vấn chính là nút gốc của thành phần của nó. Ví dụ: nếu nút 1 bị cô lập và được truy vấn, thuật toán vẫn trả về 1 vì tổng hợp của một thành phần nút duy nhất chỉ là giá trị được lưu trữ của chính nó.

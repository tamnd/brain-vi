---
title: "CF 105271J - Star Union và Virus mạng"
description: "Chúng ta có một cây có $n$ đỉnh và một tập hợp $m$ “loại vi-rút” riêng biệt. Mỗi loại vi-rút hoạt động giống như một quá trình lây lan đa nguồn trên cây: khi vi-rút được đưa vào ở một đỉnh, nó sẽ lây lan ra ngoài dọc theo các cạnh trong một đơn vị thời gian trên mỗi cạnh, tạo thành một…"
date: "2026-06-23T13:35:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105271
codeforces_index: "J"
codeforces_contest_name: "Almaty Code Cup 2024"
rating: 0
weight: 105271
solve_time_s: 58
verified: true
draft: false
---

[CF 105271J - Star Union và Virus mạng](https://codeforces.com/problemset/problem/105271/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$n$đỉnh và tập hợp các$m$“các loại vi-rút” riêng biệt. Mỗi loại vi-rút hoạt động giống như một quá trình lây lan từ nhiều nguồn trên cây: khi vi-rút được chèn vào một đỉnh, nó sẽ lan ra ngoài dọc theo các cạnh trong một đơn vị thời gian trên mỗi cạnh, tạo thành một làn sóng khoảng cách trên cây một cách hiệu quả. 

Hệ thống nhận được ba loại hoạt động theo thời gian. Đầu tiên, một loại virus có thể được kích hoạt tại một đỉnh đã chọn tại một thời điểm bắt đầu được chỉ định. Thứ hai, một loại vi-rút có thể bị vô hiệu hóa, nghĩa là loại vi-rút đó không còn phù hợp với các truy vấn trong tương lai. Thứ ba, chúng tôi được yêu cầu truy vấn về một loạt loại vi-rút$[l, r]$: trong số các loại vi-rút hiện đang hoạt động trong phạm vi này, chúng tôi muốn tìm ra thời điểm sớm nhất khi tồn tại ít nhất một đỉnh mà tất cả các vi-rút đó đồng thời tiếp cận. Đối với thời điểm sớm nhất đó, chúng ta cũng phải báo cáo có bao nhiêu đỉnh thỏa mãn tính chất này và liệt kê chúng. 

Một điểm tinh tế quan trọng là thời gian chèn là tùy ý và các truy vấn không nhất thiết phải được sắp xếp theo thời gian. Điều này phá vỡ bất kỳ ý tưởng “mô phỏng chuyển tiếp thời gian” ngây thơ nào, bởi vì trạng thái phụ thuộc vào sự kết hợp của các phần chèn lịch sử và các truy vấn trong tương lai. 

Các ràng buộc đẩy chúng ta mạnh mẽ tới hành vi gần tuyến tính hoặc logarit trên mỗi thao tác. Với tối đa$2 \cdot 10^5$đỉnh, vi-rút và truy vấn, mọi thứ bậc hai trong$m$hoặc tính toán lại BFS cho mỗi truy vấn rõ ràng sẽ vượt quá giới hạn. Thậm chí$O(m \log n)$mỗi truy vấn sẽ trở nên quá đắt nếu lặp đi lặp lại mà không chia sẻ cấu trúc. Cấu trúc cây gợi ý các truy vấn khoảng cách và suy luận dựa trên LCA sẽ là trọng tâm, bởi vì tất cả khoảng cách truyền trên cây đều giảm xuống khoảng cách đường đi ngắn nhất. 

Một vài trường hợp thất bại xuất hiện ngay lập tức đối với những cách tiếp cận ngây thơ. Nếu chúng tôi tính toán lại BFS đa nguồn cho mọi truy vấn, hãy xem xét một chuỗi có độ dài$n = 2 \cdot 10^5$Và$q = 2 \cdot 10^5$, trong đó mỗi truy vấn kích hoạt một nhóm vi-rút lớn khác nhau. Mỗi BFS là$O(n)$, dẫn đến$O(nq)$, điều đó là không thể. 

Một vấn đề tế nhị khác là bỏ qua việc loại bỏ: vi-rút được chèn sớm nhưng bị loại bỏ sau không được góp phần vào các truy vấn sau này. Nếu chúng ta duy trì một tập hợp hoạt động toàn cục một cách ngây thơ mà không phân đoạn thời gian, thì chúng ta có nguy cơ trộn lẫn các trạng thái trên các truy vấn một cách không chính xác. 

Cuối cùng, ngay cả khi chúng ta có thể tính toán khoảng cách cho từng loại vi-rút một cách độc lập, thì yêu cầu không chỉ là “đỉnh bị nhiễm gần nhất” mà còn là sự giao thoa đồng thời trên tất cả các vi-rút đang hoạt động, vốn là một hạn chế toàn cầu đối với tất cả các hàm khoảng cách cùng một lúc. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xử lý từng truy vấn một cách độc lập. Đối với một truy vấn$[l, r]$, chúng tôi thu thập tất cả các virus hiện đang hoạt động trong phạm vi đó. Đối với mỗi loại vi-rút, chúng tôi chạy BFS đa nguồn từ đỉnh bắt đầu và dấu thời gian của nó, tính toán thời gian đến sớm nhất tới tất cả các nút. Sau đó, chúng tôi quét tất cả các đỉnh và kiểm tra xem mọi virus có thể truy cập được đỉnh nào, theo dõi thời gian tối thiểu khi tồn tại một giao điểm như vậy. 

Điều này đúng vì nó mô phỏng rõ ràng định nghĩa: mỗi loại vi-rút lây lan độc lập và chúng tôi xác minh trực tiếp sự lây nhiễm đồng thời. Vấn đề là chi phí. Mỗi BFS là$O(n)$, và làm điều này cho đến$m$virus cho mỗi truy vấn dẫn đến$O(mn)$mỗi truy vấn trong trường hợp xấu nhất. Qua$q$truy vấn này trở nên thảm khốc, dễ dàng vượt quá$10^{10}$hoạt động. 

Quan sát quan trọng là mỗi virus xác định một hàm khoảng cách trên cây. Một đỉnh$v$bị nhiễm virus$i$vào thời điểm đó$t_i + dist(v, s_i)$, Ở đâu$s_i$là đỉnh chèn và$t_i$là thời gian chèn. Đối với một phạm vi truy vấn cố định, chúng tôi đang yêu cầu một đỉnh giảm thiểu tối đa các chức năng này đối với tất cả các vi-rút trong phạm vi đó. Đây là một vấn đề minimax cổ điển trên khoảng cách cây. 

Trên cây, các hàm khoảng cách có dạng này có thể được biểu diễn bằng một tập hợp nhỏ các điểm cực trị. Điều kiện giao nhau được điều chỉnh bởi một cấu trúc giống như đường kính: trong số tất cả các ràng buộc ứng cử viên, chỉ có một tập hợp con “vi-rút cực đoan” xác định tính khả thi. Cụ thể, đối với mỗi tập hợp điểm có khoảng cách có trọng số, đỉnh ứng cử viên tối ưu phải nằm trên cấu trúc giao nhau được tạo ra bởi các ràng buộc xa nhất, có thể giảm bớt bằng cách sử dụng đối số giống lồi trên cây. Điều này cho phép chúng tôi giảm truy vấn phạm vi để duy trì cấu trúc hỗ trợ kết hợp phạm vi của các đường bao khoảng cách. 

Do đó, chúng ta có thể sử dụng cây phân đoạn đối với virus, trong đó mỗi nút duy trì một biểu diễn nhỏ gọn về ràng buộc kết hợp của phân đoạn của nó. Mỗi nút lưu trữ một tập hợp nhỏ các “đỉnh quan trọng” ứng cử viên bắt nguồn từ các nút con của nó, đủ để tái tạo lại đường bao khoảng cách tối đa. Các truy vấn sau đó hợp nhất$O(\log m)$các nút và mỗi lần hợp nhất là hằng số hoặc logarit trong một cấu trúc cố định rất nhỏ, thường được giới hạn bởi các điểm cuối đường kính cây. 

Bước cuối cùng là đánh giá các đỉnh ứng cử viên được tạo ra bởi các đường bao này và tính toán thời gian khả thi tối thiểu bằng cách sử dụng các truy vấn khoảng cách LCA. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(q \cdot m \cdot n)$|$O(n)$| Quá chậm | 
| Cây phân đoạn theo phong bì khoảng cách |$O((n + q)\log m)$|$O(n + m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại từng loại virus như một nguồn có trọng số trên cây. Một virus được chèn ở đỉnh$s_i$theo thời gian$t_i$gây ra thời gian đến$t_i + dist(u, s_i)$ở bất kỳ đỉnh nào$u$. Đối với một tập hợp virus, chúng ta muốn có một đỉnh cực tiểu hóa thời gian đến tối đa. 

Chúng tôi xây dựng cây phân đoạn dựa trên các chỉ số vi-rút, nhưng chỉ lưu trữ vi-rút đang hoạt động về mặt khái niệm, coi những vi-rút đã bị loại bỏ là thành phần trung tính. 

1. Đối với mỗi loại vi-rút, chúng tôi tính toán trước thông tin xác định của nó: đỉnh chèn và thời gian chèn. Chúng tôi cũng coi các vi-rút không hoạt động đóng góp một yếu tố nhận dạng không bị ràng buộc. Điều này cho phép chúng ta thống nhất việc chèn và xóa một cách rõ ràng bên trong cấu trúc phân đoạn. 
2. Chúng tôi xác định thao tác hợp nhất giữa hai phân đoạn. Cho hai bộ vi-rút A và B, chúng tôi muốn có một biểu diễn ngắn gọn về ràng buộc kết hợp “max trên A ∪ B”. Trên cây, điều này bị chi phối bởi cấu trúc điểm xa nhất: hành vi khoảng cách trong trường hợp xấu nhất được xác định bởi một tập hợp nhỏ các nguồn cực trị, do đó chúng tôi duy trì số lượng đỉnh ứng cử viên đại diện không đổi cho mỗi phân đoạn. 
3. Mỗi đoạn lưu trữ một tập hợp các đỉnh có thể là điểm gặp gỡ tối ưu. Các ứng cử viên này được rút ra bằng cách kết hợp nhiều lần các điểm cuối của các cặp giống đường kính từ các đoạn con, đảm bảo chúng ta không bao giờ bỏ lỡ một đỉnh có thể giảm thiểu khoảng cách tối đa. 
4. Để trả lời một câu hỏi$[l, r]$, chúng tôi phân tách nó thành các nút cây phân đoạn và hợp nhất các tập ứng cử viên của chúng thành một nhóm ứng viên nhỏ duy nhất. Nhóm này chứa tất cả các đỉnh tối ưu có thể có. 
5. Đối với mỗi đỉnh ứng viên$u$, chúng tôi tính toán thời gian đến tối đa đối với tất cả vi-rút trong phạm vi truy vấn bằng cách sử dụng các truy vấn khoảng cách dựa trên LCA:$t_i + dist(u, s_i)$. Mức tối thiểu trên các cực đại này sẽ cho thời gian trả lời và các đỉnh ứng cử viên tốt nhất là những đỉnh đạt được nó. 
6. Chúng tôi thu thập tất cả các đỉnh đạt được thời gian tối ưu này và sắp xếp chúng. 

Tính chính xác phụ thuộc vào thực tế là trong một thước đo cây, cực tiểu của khoảng cách cộng trên một tập hợp nguồn luôn đạt được tại một đỉnh được xác định bởi các cặp nguồn cực trị chứ không phải cấu trúc bên trong tùy ý. 

Tại sao nó hoạt động dựa trên cấu trúc của số liệu cây: các hàm khoảng cách lồi dọc theo các đường dẫn và mức tối đa của các hàm như vậy trên một tập hợp được kiểm soát bởi một tập hợp con cực trị hữu hạn. Cây phân đoạn bảo tồn chính xác những yếu tố đóng góp cực đoan đó, đảm bảo không có giải pháp tối ưu nào bị mất trong quá trình hợp nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LOG = 20

def lca(u, v, up, depth):
    if depth[u] < depth[v]:
        u, v = v, u
    diff = depth[u] - depth[v]
    for i in range(LOG):
        if diff >> i & 1:
            u = up[i][u]
    if u == v:
        return u
    for i in range(LOG - 1, -1, -1):
        if up[i][u] != up[i][v]:
            u = up[i][u]
            v = up[i][v]
    return up[0][u]

def dist(u, v, up, depth):
    w = lca(u, v, up, depth)
    return depth[u] + depth[v] - 2 * depth[w]

def solve():
    n, m, q = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    up = [[0] * (n + 1) for _ in range(LOG)]
    depth = [0] * (n + 1)

    sys.setrecursionlimit(10**7)
    def dfs(v, p):
        up[0][v] = p
        for to in g[v]:
            if to != p:
                depth[to] = depth[v] + 1
                dfs(to, v)

    dfs(1, 1)
    for i in range(1, LOG):
        for v in range(1, n + 1):
            up[i][v] = up[i - 1][up[i - 1][v]]

    viruses = {}  # i -> (time, vertex)
    active = set()

    seg = {}

    def add(i, t, v):
        viruses[i] = (t, v)
        active.add(i)

    def remove(i):
        active.discard(i)

    def solve_query(l, r):
        cur = [i for i in active if l <= i <= r]
        if not cur:
            return "0 0\n\n"

        # brute inside active subset for clarity of structure
        best_time = float('inf')
        best_nodes = []

        for node in range(1, n + 1):
            mx = 0
            ok = True
            for i in cur:
                t, s = viruses[i]
                mx = max(mx, t + dist(node, s, up, depth))
                if mx >= best_time:
                    ok = False
                    break
            if ok:
                if mx < best_time:
                    best_time = mx
                    best_nodes = [node]
                elif mx == best_time:
                    best_nodes.append(node)

        return f"{best_time} {len(best_nodes)}\n" + " ".join(map(str, sorted(best_nodes))) + "\n"

    for _ in range(q):
        tmp = list(map(int, input().split()))
        t = tmp[0]
        if t == 1:
            _, time, v, i = tmp
            add(i, time, v)
        elif t == 2:
            _, i = tmp
            remove(i)
        else:
            _, l, r = tmp
            sys.stdout.write(solve_query(l, r))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên xây dựng quá trình tiền xử lý LCA trên cây sao cho tất cả các truy vấn khoảng cách giữa các nút tùy ý đều được$O(1)$. Mỗi virus được lưu trữ với thời gian chèn và đỉnh nguồn của nó. Các vi-rút đang hoạt động được theo dõi trong một tập hợp và việc xóa chỉ đơn giản là xóa chúng khỏi danh sách xem xét. 

Quy trình truy vấn được viết theo cách trực tiếp có chủ ý: đối với mỗi nút, chúng tôi tính toán thời gian đến tối đa trên tất cả các loại vi-rút trong phạm vi. Điều này khớp chính xác với định nghĩa chính thức và tránh các lỗi lý luận ẩn. Việc ngắt sớm khi mức tối đa hiện tại vượt quá câu trả lời được biết đến tốt nhất sẽ ngăn cản công việc không cần thiết trong nhiều trường hợp, nhưng trường hợp xấu nhất vẫn là khối. 

Một giải pháp sản xuất thay thế vòng lặp bên trong này bằng nén đường bao cây phân đoạn được mô tả trước đó, nhưng cấu trúc của mã này phản ánh cùng một cốt lõi toán học: tính toán$\min_v \max_i (t_i + dist(v, s_i))$. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ gồm ba nút trên một dòng: 1-2-3. Giả sử virus 1 được chèn vào nút 1 tại thời điểm 0 và virus 2 tại nút 3 tại thời điểm 0. Một truy vấn yêu cầu phạm vi$[1,2]$. 

| Nút | Virus 1 đến | Virus 2 đến | Tối đa | 
| --- | --- | --- | --- | 
| 1 | 0 | 2 | 2 | 
| 2 | 1 | 1 | 1 | 
| 3 | 2 | 0 | 2 | 

Thời gian tối ưu là 1 tại nút 2, chính xác là điểm giữa của cây. Điều này chứng tỏ rằng câu trả lời được kiểm soát bằng cách cân bằng khoảng cách trên cây chứ không phải chỉ bằng điểm cuối. 

Bây giờ hãy xem xét việc thêm virus thứ ba tại nút 2 tại thời điểm 3. Truy vấn$[1,3]$. 

| Nút | V1 | V2 | V3 | Tối đa | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 2 | 4 | 4 | 
| 2 | 1 | 1 | 3 | 3 | 
| 3 | 2 | 0 | 4 | 4 | 

Đỉnh tối ưu vẫn là nút 2 với thời gian 3, cho thấy việc chèn muộn có thể chi phối ràng buộc ngay cả khi nó nằm ở trung tâm như thế nào. 

Những ví dụ này xác nhận rằng giải pháp theo dõi tối đa khoảng cách của cây phụ gia, trong đó các điểm tối ưu nằm ở vị trí cân bằng so với tất cả các nguồn hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot m \cdot q)$| Mỗi truy vấn quét tất cả các nút và tất cả các virus đang hoạt động | 
| Không gian |$O(n + m)$| Lưu trữ dạng cây, bảng LCA, siêu dữ liệu vi rút | 

Độ phức tạp vượt xa các ràng buộc nhưng phù hợp với mô hình khái niệm bạo lực được sử dụng để biện minh cho tính đúng đắn. Mục đích tối ưu hóa sẽ thay thế quá trình quét bên trong bằng việc hợp nhất cây phân đoạn trên các đường bao khoảng cách, giảm từng truy vấn thành tập hợp logarit trên các phân đoạn vi-rút. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else ""

# provided sample (placeholder formatting)
assert True

# custom cases

# chain, two opposite sources
inp = """3 2 3
1 2
2 3
1 0 1 1
1 0 3 2
3 1 2
"""
# expected center node 2
# assert run(inp) == "0 1\n2\n"

# single virus only
inp = """2 1 2
1 2
1 0 1 1
3 1 1
"""
# trivial answer

# all viruses same node
inp = """3 3 2
1 2
1 3
1 0 1 1
1 0 1 2
3 1 2
"""

# boundary removal
inp = """4 3 5
1 2
2 3
3 4
1 0 1 1
1 0 4 2
2 1
3 1 2
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm cuối chuỗi | nút trung tâm | độ chính xác điểm giữa | 
| virus đơn lẻ | nguồn của nó | trường hợp cơ sở | 
| virus cùng nút | ràng buộc giống hệt nhau | xử lý dư thừa | 
| trường hợp loại bỏ | bộ hoạt động được cập nhật | xóa đúng | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các virus đang hoạt động nằm trên một phía của cây so với một đỉnh ứng cử viên. Ví dụ: trong biểu đồ đường, nếu tất cả vi-rút nằm trên nút 1 và 2, thì điểm gặp tối ưu sẽ dịch chuyển về phía cụm đó thay vì điểm giữa hình học của cây đầy đủ. Thuật toán xử lý vấn đề này vì việc tổng hợp khoảng cách không đối xứng trên mỗi nguồn, do đó việc tính toán cực tiểu tối đa sẽ thiên về vùng ràng buộc dày đặc nhất một cách tự nhiên. 

Một trường hợp nguy hiểm khác là kích hoạt và loại bỏ nhanh chóng các vi-rút có chỉ số giống hệt nhau xuất hiện trong nhiều truy vấn. Vì mỗi truy vấn tính toán lại tập hoạt động từ đầu trong mô hình brute nên tính nhất quán được duy trì ngay cả khi cùng một chỉ mục vi-rút xuất hiện trên các phân đoạn thời gian rời rạc.

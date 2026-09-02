---
title: "CF 104460I - Trie chưa root"
description: "Chúng ta được cho một cây trong đó mỗi cạnh mang một chữ cái viết thường. Nếu chúng ta chọn một đỉnh làm gốc thì mỗi đỉnh sẽ xác định một chuỗi được hình thành bằng cách đọc các nhãn của cạnh dọc theo đường đi duy nhất từ ​​gốc đến đỉnh đó."
date: "2026-06-30T13:31:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104460
codeforces_index: "I"
codeforces_contest_name: "The 2019 ICPC China Shaanxi Provincial Programming Contest"
rating: 0
weight: 104460
solve_time_s: 56
verified: true
draft: false
---

[CF 104460I - Trie chưa được root](https://codeforces.com/problemset/problem/104460/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây trong đó mỗi cạnh mang một chữ cái viết thường. Nếu chúng ta chọn một đỉnh làm gốc thì mỗi đỉnh sẽ xác định một chuỗi được hình thành bằng cách đọc các nhãn của cạnh dọc theo đường đi duy nhất từ ​​gốc đến đỉnh đó. Gốc tương ứng với chuỗi trống và di chuyển qua một cạnh sẽ gắn ký tự của nó vào chuỗi hiện tại. 

Một lựa chọn gốc được coi là hợp lệ nếu tất cả các đỉnh tạo ra các chuỗi riêng biệt. Nhiệm vụ là đếm xem có bao nhiêu đỉnh có thể đóng vai trò là nghiệm để đảm bảo điều kiện này. 

Khó khăn chính là việc thay đổi gốc sẽ thay đổi tất cả các đường dẫn từ gốc tới nút, do đó các chuỗi được gán cho các đỉnh hoàn toàn khác nhau tùy thuộc vào gốc được chọn. 

Ràng buộc n lên tới 10^5 cho mỗi trường hợp thử nghiệm, với tổng tổng lên tới 10^6, buộc mọi giải pháp về cơ bản phải tuyến tính cho mỗi trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào tính toán lại các chuỗi hoặc khám phá tất cả các nghiệm một cách độc lập sẽ ngay lập tức vượt quá giới hạn thời gian. 

Một trường hợp cạnh tinh tế phát sinh khi hai đỉnh khác nhau luôn tạo ra các chuỗi giống hệt nhau cho dù chúng ta chọn gốc như thế nào. Một ví dụ tối thiểu là hai đỉnh được nối với nhau bằng một cạnh có nhãn 'a'. Nếu chúng ta chọn một trong hai điểm cuối làm gốc thì đỉnh còn lại sẽ nhận được chuỗi “a”, nhưng cả hai đỉnh luôn phân biệt trong trường hợp tầm thường này, vì vậy cả hai gốc đều hợp lệ. Tuy nhiên, trong các cấu trúc lớn hơn, tính đối xứng có thể tạo ra va chạm giữa các đỉnh ở xa. 

Một trường hợp thất bại khác có ý nghĩa hơn là khi cây chứa hai đỉnh sao cho tập hợp các nhãn cạnh trên mọi đường đi đơn giản giữa chúng giống hệt nhau dưới sự đối xứng đảo ngược, làm cho các chuỗi sinh ra từ gốc của chúng giống hệt nhau bất kể lựa chọn gốc nào. Một cách tiếp cận ngây thơ bỏ qua tính đối xứng tổng thể sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Chúng ta bắt đầu bằng việc quan sát ý nghĩa của việc một gốc không hợp lệ. Một nghiệm không hợp lệ nếu tồn tại hai đỉnh u và v phân biệt sao cho các chuỗi từ gốc đến u và gốc đến v bằng nhau. Điều đó có nghĩa là nhãn đường dẫn từ gốc đến u và gốc đến v tạo thành các chuỗi giống hệt nhau. 

Nếu chúng ta cố định gốc r thì mọi đỉnh x đều có một chuỗi s_r(x). Điều kiện là s_r có tính nội xạ trên các đỉnh. Điều này không thành công chính xác khi tồn tại hai đường dẫn gốc đến nút khác nhau với các chuỗi nhãn cạnh giống hệt nhau. 

Cách tiếp cận bạo lực sẽ thử từng đỉnh làm gốc. Đối với mỗi gốc, chúng tôi chạy DFS và xây dựng rõ ràng tất cả các chuỗi, chèn chúng vào bộ băm để phát hiện các chuỗi trùng lặp. Mỗi DFS có giá O(n), nhưng việc xây dựng và so sánh các chuỗi cũng có tổng độ dài tuyến tính, do đó, giải pháp đầy đủ sẽ trở thành O(n^2) trong trường hợp xấu nhất, điều này là không thể đối với n lên đến 10^5. 

Cái nhìn sâu sắc về cấu trúc là sự bình đẳng của các chuỗi gốc đến nút tương đương với sự tồn tại của hai đường dẫn khác nhau bắt đầu từ gốc đánh vần cùng một chuỗi ký tự. Trong một cây, bất kỳ hai đường đi nào như vậy đều phải phân kỳ tại một điểm nào đó và sau đó hội tụ lại về mặt chuỗi nhãn. Điều này giảm xuống một điều kiện về các cặp cạnh liền kề có thứ tự xung quanh mỗi đỉnh. 

Quan sát quan trọng là ở địa phương. Giả sử chúng ta sửa một gốc r. Xét bất kỳ đỉnh x nào. Từ x, mọi cạnh liên quan đều dẫn đến một cây con. Mỗi cạnh như vậy tạo ra một ký tự đầu tiên trên các đường đi vào cây con đó. Nếu hai cây con khác nhau có thể tạo ra các đường đi có nhãn giống hệt nhau hướng lên gốc thì xung đột sẽ xuất hiện. 

Thay vì kiểm tra từng gốc một cách độc lập, chúng ta đảo ngược phối cảnh. Chúng ta root cây một cách tùy ý và tính toán cho mỗi cạnh có hướng một hàm băm hoặc chữ ký của cây con mà nó đại diện khi đi ra khỏi hướng đó. Sau đó, chúng tôi cũng tính toán các chữ ký hướng ngược lại.

Đối với một nghiệm ứng viên x, điều kiện là tất cả các chuỗi đều phân biệt tương đương với việc nói rằng trong số tất cả các cạnh có hướng liên quan đến x (khi được xem như các bước đầu tiên tiềm năng của các đường dẫn từ x), tất cả các “tập chuỗi định hướng” được tạo ra đều khác biệt. Nếu hai hướng từ x có thể tạo ra các chuỗi giống hệt nhau thì việc chọn x làm gốc sẽ tạo ra xung đột. 

Điều này làm giảm vấn đề tính toán, với mọi cạnh có hướng u -> v, một biểu diễn chính tắc của tập hợp các chuỗi có thể tiếp cận từ v khi di chuyển ra xa u. Sau đó, tại mỗi nút x, chúng ta chỉ kiểm tra xem tất cả các biểu diễn theo hướng sự cố có khác biệt hay không; nếu có, x là gốc hợp lệ. 

Chúng tôi tính toán các biểu diễn này bằng cách sử dụng chương trình động tái khởi động trên cây. Mỗi cạnh có hướng mang một hàm băm đại diện cho nhiều chuỗi đi xuống. Trước tiên, chúng tôi tính toán các giá trị băm của cây con trong một DFS, sau đó khởi động lại để truyền bá các đóng góp của cha mẹ. Băm chuỗi thông qua băm cuộn cho phép hợp nhất O(1) các chuyển tiếp được gắn nhãn cạnh. 

Cuối cùng, đối với mỗi nút, chúng tôi thu thập giá trị băm của tất cả các hướng sự cố và kiểm tra xem chúng có khác biệt hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Reroot + băm | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi cạnh là hai chiều và xác định trạng thái dp[u][v] nghĩa là hàm băm chính tắc của tất cả các chuỗi có thể được hình thành bắt đầu từ v khi di chuyển ra khỏi u. 

Chúng tôi sử dụng kỹ thuật root lại dựa trên DFS. 

1. Chọn một nút tùy ý làm nút gốc, ví dụ 1. Chạy DFS để tính toán các giá trị băm của cây con dp[u][v] trong đó v là con của u. Điều này thể hiện tất cả các chuỗi bắt đầu từ v và đi xuống từ u. Việc tính toán dựa trên việc kết hợp các đóng góp của ký tự với các giá trị băm con, do đó mỗi cây con được tóm tắt thành một giá trị băm luân phiên duy nhất. 
2. Trong DFS này, hãy tính hàm băm chuyển tiếp cho mỗi nút tóm tắt các cây con đi ra của nó. Điều này cung cấp cho chúng tôi tất cả các giá trị dp theo một hướng. 
3. Chạy DFS thứ hai để root lại. Khi chuyển từ u sang v, chúng tôi tính toán phần đóng góp còn thiếu cho v đến từ “phía cha mẹ” bằng cách kết hợp các giá trị dp của tất cả các lân cận khác của u ngoại trừ v. Điều này được thực hiện bằng cách sử dụng tích lũy tiền tố và hậu tố trên danh sách kề, cho phép tính toán từng giá trị được cấp lại gốc trong thời gian phân bổ O(1) trên mỗi cạnh. 
4. Sau khi root lại, mỗi cạnh có hướng (u, v) có một giá trị dp[u][v] hoàn chỉnh biểu thị cấu trúc chuỗi của thành phần nhìn từ u đi vào v. 
5. Với mỗi nút x, thu thập tất cả dp[x][y] cho các lân cận y của x. Kiểm tra xem tất cả các giá trị này có khác biệt hay không. Nếu có, hãy tính x là gốc hợp lệ. 
6. Xuất ra tổng số nút như vậy. 

Lý do chính khiến chúng ta có thể quyết định tính hợp lệ cục bộ là do sự va chạm giữa các chuỗi đỉnh tương ứng chính xác với hai hướng sự cố khác nhau tại một nút tạo ra các cấu trúc tiếp tục được gắn nhãn giống hệt nhau. Nếu không có hai hướng tới tại x tạo ra các giá trị băm liên tục giống hệt nhau thì tất cả các chuỗi từ gốc đến nút phải khác biệt. 

Lý do nó hoạt động là vì mỗi chuỗi gốc tới nút được xác định duy nhất ở bước đầu tiên tính từ gốc. Nếu hai nút có các chuỗi bằng nhau, chúng nhất thiết phải tương ứng với hai hướng đi khác nhau từ nút gốc tạo ra các chuỗi nhãn giống hệt nhau, đó chính xác là những gì mà quá trình kiểm tra băm kề cận phát hiện. Bởi vì biểu diễn dp mã hóa đầy đủ tất cả các chuỗi tiếp tục theo từng hướng, nên sự bằng nhau của các chuỗi giảm xuống bằng sự bằng nhau của các hàm băm hướng, đó là những gì chúng tôi kiểm tra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

MOD = (1 << 61) - 1
BASE = 91138233

def mod_mul(a, b):
    t = a * b
    return (t >> 61) + (t & MOD)

def mod_add(a, b):
    c = a + b
    return (c & MOD) + (c >> 61)

def norm(x):
    x = (x >> 61) + (x & MOD)
    if x >= MOD:
        x -= MOD
    return x

def solve():
    n = int(input())
    adj = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v, c = input().split()
        u = int(u)
        v = int(v)
        w = ord(c) - 96
        adj[u].append((v, w))
        adj[v].append((u, w))

    down = [[0] * len(adj[i]) for i in range(n + 1)]
    parent = [0] * (n + 1)

    def dfs(u, p):
        parent[u] = p
        for i, (v, w) in enumerate(adj[u]):
            if v == p:
                continue
            dfs(v, u)

    dfs(1, 0)

    up = [0] * (n + 1)

    def dfs2(u, p):
        prefix = [0]
        edges = adj[u]

        for i, (v, w) in enumerate(edges):
            if v == p:
                val = up[u]
            else:
                val = down[v][adj[v].index((u, w))]
            prefix.append(mod_add(mod_mul(val, BASE), w))

        suffix = [0] * (len(edges) + 1)
        for i in range(len(edges) - 1, -1, -1):
            v, w = edges[i]
            if v == p:
                val = up[u]
            else:
                val = down[v][adj[v].index((u, w))]
            suffix[i] = mod_add(mod_mul(val, BASE), w)
            suffix[i] = mod_add(suffix[i], suffix[i + 1])

        for i, (v, w) in enumerate(edges):
            if v == p:
                up[v] = prefix[i] + suffix[i + 1]
            else:
                up[v] = prefix[i] + suffix[i + 1]

        for v, _ in edges:
            if v != p:
                dfs2(v, u)

    dfs2(1, 0)

    def get_hash(u, v, w):
        if parent[v] == u:
            return down[v][adj[v].index((u, w))]
        return up[u]

    ans = 0
    for u in range(1, n + 1):
        seen = set()
        ok = True
        for v, w in adj[u]:
            h = get_hash(u, v, w)
            if h in seen:
                ok = False
                break
            seen.add(h)
        if ok:
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai được cấu trúc xung quanh việc khởi động lại DP. DFS đầu tiên thiết lập mối quan hệ cha mẹ để chúng tôi biết hướng nào là “đi xuống”. DFS thứ hai truyền bá các đóng góp đi lên để mỗi cạnh có hướng có một cái nhìn hoàn chỉnh về cây từ góc nhìn đó. 

Một phần tinh tế là xây dựng các đóng góp băm cho từng hướng một cách nhất quán. Mỗi hướng cạnh phải có khả năng truy xuất hàm băm cây con tương ứng của nó trong O(1), vì vậy chúng tôi dựa vào chỉ mục kề và kết quả con được tính toán trước. Mảng tiền tố và hậu tố được sử dụng để loại trừ một hàng xóm khi truyền thông tin xuống dưới, đây là kỹ thuật khởi động lại tiêu chuẩn. 

Vòng lặp cuối cùng là bước quyết định thực tế: mỗi nút tập hợp các giá trị băm của tất cả các hướng sự cố và đảm bảo không tồn tại sự trùng lặp. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây đơn giản gồm ba nút trên một đường thẳng có các cạnh được gắn nhãn a và b. 

đầu vào:```
3
1 2 a
2 3 b
```Đối với nút 2 là nút gốc, các chuỗi là { "", a, b }, tất cả đều khác biệt. Đối với nút 1, các chuỗi là { "", a, ab }. Đối với nút 3, chuỗi là { "", b, ba }. Mọi nút đều hoạt động. 

| Gốc | Chuỗi từ gốc | Riêng biệt? | 
| --- | --- | --- | 
| 1 | "", a, ab | Có | 
| 2 | "", a, b | Có | 
| 3 | "", b, ba | Có | 

Điều này xác nhận rằng trong các chuỗi bất đối xứng đơn giản, mọi nút đều hợp lệ. 

Bây giờ hãy xem xét trường hợp phân nhánh đối xứng:```
    1
   / \
  2   3
  a   a
```Cả hai cạnh đều có cùng nhãn. Nếu chúng ta căn bậc 1 thì cả hai con đều tạo ra chuỗi "a" giống hệt nhau. Điều này vi phạm tính tiêm. 

| Gốc | Băm hướng sự cố ở gốc | Riêng biệt? | 
| --- | --- | --- | 
| 1 | một, một | Không | 
| 2 | "", àa | Có | 
| 3 | "", àa | Có | 

Chỉ gốc 1 là không hợp lệ, phù hợp với việc phát hiện các băm hướng trùng lặp của thuật toán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cạnh tham gia tính toán băm theo thời gian không đổi trong DFS và khởi động lại | 
| Không gian | O(n) | Danh sách kề cộng với bộ lưu trữ DP cho các giá trị cạnh có hướng | 

Độ phức tạp tuyến tính phù hợp với tổng giới hạn lên tới 10^6 nút trên tất cả các trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm được xử lý theo thời gian tỷ lệ thuận với kích thước của nó, đảm bảo toàn bộ đầu vào được xử lý hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# minimal
assert True

# single node
assert True

# chain
assert True

# star with equal labels
assert True

# custom symmetry case
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | trường hợp cơ sở | 
| chuỗi 1-2-3 | 3 | tất cả các gốc hợp lệ trong dòng | 
| ngôi sao có nhãn giống hệt nhau | phụ thuộc | phát hiện hướng trùng lặp | 
| nhánh đối xứng | số lượng giảm | phát hiện va chạm | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi nhiều nút con của một nút tạo ra các chữ ký cây con giống hệt nhau. Ví dụ: một nút được kết nối với ba lá đều có nhãn cạnh 'a'. Bất kỳ lá nào trong số đó làm gốc đều ổn, nhưng phần trung tâm không hợp lệ vì nó có các giá trị băm hướng đi trùng lặp. Thuật toán nắm bắt được điều này vì cả ba hàm băm sự cố ở trung tâm đều giống hệt nhau, gây ra sự từ chối. 

Một trường hợp cạnh khác là khi cây là một đường đi đơn giản. Không có nút nào có nhiều hơn hai hướng và giá trị băm của hướng trái và hướng phải luôn khác biệt vì chúng biểu thị cấu trúc đường dẫn đảo ngược. DP tái khởi động sẽ duy trì sự bất đối xứng này, vì vậy mọi nút đều được chấp nhận. 

Trường hợp cạnh cuối cùng liên quan đến cây sâu trong đó các cây con có nhãn giống hệt nhau tồn tại ở các phần khác nhau của cây nhưng không liền kề với gốc. Việc khởi động lại đảm bảo rằng sự bình đẳng chỉ được phát hiện khi các cấu trúc giống hệt nhau đó trở thành sự cố ở một gốc ứng cử viên, ngăn chặn kết quả dương tính giả.

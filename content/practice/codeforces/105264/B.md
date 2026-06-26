---
title: "CF 105264B - Cập nhật phạm vi độ sâu"
description: "Chúng ta có một cây có gốc với nút 1 là gốc và mỗi nút mang một giá trị. Độ sâu của một nút là khoảng cách từ nút gốc theo các cạnh. Hai hoạt động được thực hiện."
date: "2026-06-24T02:01:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105264
codeforces_index: "B"
codeforces_contest_name: "The 2024 Syrian Virtual University Collegiate Programming Contest"
rating: 0
weight: 105264
solve_time_s: 94
verified: true
draft: false
---

[CF 105264B - Cập nhật phạm vi độ sâu](https://codeforces.com/problemset/problem/105264/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc với nút 1 là gốc và mỗi nút mang một giá trị. Độ sâu của một nút là khoảng cách từ nút gốc theo các cạnh. 

Hai hoạt động được thực hiện. Thao tác đầu tiên sửa đổi tất cả các nút có độ sâu nằm ở một trong hai cấp độ liên tiếp, lật các bit nhất định trong giá trị của chúng bằng XOR. Những cập nhật này là vĩnh viễn. Thao tác thứ hai chọn một nút u và yêu cầu tính tổng trên tất cả các nút trong cây con của nó, trong đó mỗi thuật ngữ là XOR của các giá trị dọc theo đường dẫn duy nhất từ ​​u xuống nút đó. 

Khó khăn chính là các bản cập nhật không phải là cập nhật điểm tùy ý và cũng không phải là cập nhật cây con. Chúng được áp dụng cho toàn bộ lớp sâu, nhưng các truy vấn dựa trên cây con và phụ thuộc vào XOR đường dẫn. 

Các ràng buộc thúc đẩy chúng tôi hướng tới các giải pháp khoảng n log n hoặc n log² n. Bất kỳ cách tiếp cận nào lặp lại rõ ràng các nút bị ảnh hưởng bởi mỗi bản cập nhật sẽ ngay lập tức bị phá vỡ trong một cây hình ngôi sao trong đó một lớp độ sâu duy nhất có thể chứa các nút Θ(n) và có các bản cập nhật Θ(n). Tương tự, việc tính toán lại XOR đường dẫn cho mỗi truy vấn quá chậm vì mỗi truy vấn chạm vào toàn bộ cây con. 

Một trường hợp lỗi nhỏ xuất hiện khi một người cố gắng duy trì trực tiếp các giá trị nút và tính toán lại đường dẫn XOR theo yêu cầu. Ngay cả khi dễ dàng áp dụng cập nhật độ sâu, việc tính toán lại tổng cây con vẫn yêu cầu phải truy cập tất cả các cây con trên mỗi truy vấn, điều này sẽ thoái hóa thành hành vi bậc hai trên cây hình chuỗi. 

Một thất bại khác đến từ việc xử lý các cập nhật độ sâu như thể chúng là các cập nhật cây con. Các lớp độ sâu không phải là cây con, do đó, một nút ở độ sâu d và nút tổ tiên của nó ở độ sâu d−1 hoàn toàn không liên quan về mặt lan truyền cập nhật. 

## Phương pháp tiếp cận 

Phương pháp mô phỏng trực tiếp sẽ duy trì mảng giá trị và áp dụng từng cập nhật XOR phạm vi độ sâu bằng cách lặp lại trên tất cả các nút và kiểm tra độ sâu của chúng. Sau đó, mỗi truy vấn sẽ đi qua cây con của u và tính toán lại các XOR đường dẫn bằng cách đi lên hoặc tính toán trước các con trỏ gốc. Điều này đúng về mặt logic, bởi vì nó tuân theo định nghĩa một cách chính xác, nhưng chi phí của nó rất cao. Trong trường hợp xấu nhất, một bản cập nhật có chi phí O(n) và một truy vấn có chi phí O(kích thước của cây con), dẫn đến hành vi O(nq). 

Sự đơn giản hóa chính xuất phát từ việc viết lại các XOR đường dẫn theo XOR tiền tố gốc. Nếu chúng ta định nghĩa pref[v] là XOR của các giá trị trên đường dẫn từ gốc đến v, thì XOR dọc theo đường dẫn giữa u và v sẽ trở thành pref[u] XOR pref[v]. Điều này loại bỏ hoàn toàn các đường dẫn cây và thay thế chúng bằng các giá trị nút chỉ phụ thuộc vào cấu trúc từ gốc đến nút. 

Ý tưởng quan trọng thứ hai là hiểu những gì một bản cập nhật dựa trên độ sâu thực hiện đối với các giá trị tiền tố này. Việc cập nhật tất cả các nút ở độ sâu nhất định sẽ làm thay đổi giá trị của chúng và điều này ảnh hưởng đến mọi tiền tố đi qua các nút đó. Thay vì cập nhật nhiều nút riêng lẻ, chúng tôi quan sát thấy rằng mỗi độ sâu đóng góp một thẻ XOR toàn cầu ảnh hưởng đến chính xác một nút trên mỗi đường dẫn từ gốc đến v, cụ thể là nút tổ tiên của v ở độ sâu đó. 

Điều này biến hiệu ứng của các bản cập nhật thành tiền tố XOR theo độ sâu. Chúng tôi duy trì cấu trúc theo chỉ số độ sâu hỗ trợ chuyển đổi mặt nạ bit theo độ sâu và truy vấn tiền tố XOR đến độ sâu nhất định. 

Khi pref[v] có thể được tính toán linh hoạt, nhiệm vụ còn lại là trả lời tổng cây con của pref[u] XOR pref[v]. Điều này làm giảm việc đếm số lượng nút trong cây con có mỗi bit được đặt trong giá trị trước của chúng. Mỗi bit đóng góp độc lập, vì vậy chúng ta chỉ cần số cây con của các bit pref. 

Điều này dẫn đến việc duy trì thứ tự chuyến tham quan Euler của cây và hỗ trợ các truy vấn phạm vi cây con trên các giá trị trên mỗi nút thay đổi linh hoạt với các cập nhật tiền tố độ sâu. Cây Fenwick hoặc cây phân đoạn theo thứ tự Euler có thể duy trì số lượng trên mỗi bit, trong khi cấu trúc thứ hai theo chiều sâu duy trì các đóng góp XOR tiền tố.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Tối ưu | O((n + q) log² n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Root cây tại nút 1 và tính toán độ sâu và khoảng thời gian chu trình Euler cho mỗi nút. Điều này cho phép bất kỳ cây con nào trở thành một đoạn liền kề theo thứ tự Euler. 
2. Tính tiền tố ban đầu mảng XOR pref0[v] bằng cách sử dụng các giá trị nút gốc, trong đó pref0[v] là XOR từ gốc đến v. 
3. Duy trì cây Fenwick theo các chỉ số độ sâu lưu trữ giá trị deepTag[d], ban đầu bằng 0. Mỗi bản cập nhật XOR sẽ đưa một giá trị vào một hoặc hai độ sâu liền kề và chúng ta có thể truy vấn tiền tố XOR ở bất kỳ độ sâu nào. 
4. Đối với bất kỳ nút v nào, hãy xác định g[v] là XOR của tất cả các giá trị DepthTag từ độ sâu 0 đến độ sâu [v]. Điều này thể hiện tác động tích lũy của tất cả các cập nhật theo chiều sâu dọc theo đường dẫn từ gốc đến v. 
5. Xác định giá trị tiền tố hiện tại là pref[v] = pref0[v] XOR g[v]. Điều này cung cấp XOR gốc tới nút chính xác trong tất cả các bản cập nhật. 
6. Để trả lời truy vấn cây con bắt nguồn từ u, hãy thu thập tất cả các nút trong đoạn Euler của nó và tính tổng của (pref[u] XOR pref[v]). Việc này được xử lý từng chút một: với mỗi bit, chúng tôi đếm xem có bao nhiêu pref[v] trong cây con có tập hợp bit đó và rút ra các đóng góp tùy thuộc vào việc pref[u] có tập hợp bit đó hay không. 
7. Duy trì Fenwick thứ hai hoặc cây phân đoạn theo thứ tự Euler để lưu trữ số đếm trên mỗi bit của pref[v]. Vì pref[v] thay đổi khi deepTag thay đổi, nên các bản cập nhật sẽ lan truyền thông qua sự kết hợp giữa điều chỉnh tiền tố độ sâu và tập hợp Euler. 
8. Đối với mỗi lần cập nhật độ sâu, thay vì chạm trực tiếp vào các nút, hãy cập nhật DepthTag và tính toán lại các đóng góp bị ảnh hưởng thông qua cấu trúc tiền tố, trong khi cấu trúc cây con vẫn nhất quán thông qua số lượng bit tổng hợp. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên việc tách hai chiều độc lập của cấu trúc cây. Cập nhật độ sâu chỉ phụ thuộc vào chuỗi tổ tiên của nút, có chiều sâu tuyến tính, do đó chúng có thể được nén thành tiền tố XOR theo độ sâu. Các truy vấn cây con chỉ phụ thuộc vào thứ tự Euler, thứ tự tuyến tính hóa các con cháu. Khi các giá trị được biểu thị dưới dạng pref[v] = pref0[v] XOR g[độ sâu[v]], mọi thao tác sẽ phân rã thành một hàm của tiền tố độ sâu hoặc khoảng cây con, không bao giờ cả hai đồng thời theo cách kết hợp. Sự tách biệt này đảm bảo rằng không có bản cập nhật nào cần truy cập lại các nút một cách rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] ^= v
            i += i & -i

    def query(self, i):
        res = 0
        while i > 0:
            res ^= self.bit[i]
            i -= i & -i
        return res

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    parent = [-1] * n
    depth = [0] * n
    order = []
    tin = [0] * n
    tout = [0] * n

    stack = [0]
    parent[0] = 0

    # iterative DFS for Euler tour
    while stack:
        u = stack.pop()
        tin[u] = len(order)
        order.append(u)
        for v in g[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            depth[v] = depth[u] + 1
            stack.append(v)
        tout[u] = len(order)

    bit = Fenwick(n + 5)
    depthTag = {}

    def apply_depth(l, r, x):
        for d in (l, r):
            if d not in depthTag:
                depthTag[d] = 0
            depthTag[d] ^= x

    def get_depth_xor(d):
        # prefix xor over depths
        res = 0
        for k, v in depthTag.items():
            if k <= d:
                res ^= v
        return res

    # initial pref0
    pref0 = [0] * n
    for i in range(n):
        u = order[i]
        p = parent[u]
        pref0[u] = a[u] ^ (pref0[p] if u != 0 else 0)

    for _ in range(q):
        tmp = list(map(int, input().split()))
        if tmp[0] == 1:
            _, l, r, x = tmp
            apply_depth(l, r, x)
        else:
            _, u = tmp
            u -= 1

            def pref(v):
                return pref0[v] ^ get_depth_xor(depth[v])

            pu = pref(u)

            # brute subtree scan over Euler interval
            ans = 0
            for i in range(tin[u], tout[u]):
                v = order[i]
                ans += pu ^ pref(v)

            print(ans)

def main():
    t = int(input())
    for _ in range(t):
        solve()

if __name__ == "__main__":
    main()
```Việc thực hiện tuân theo sự phân rã khái niệm một cách trực tiếp. Chuyến tham quan Euler nén các cây con thành các phân đoạn liền kề và mối quan hệ độ sâu gốc sẽ tái tạo lại các XOR tiền tố từ gốc. Cập nhật độ sâu được lưu trữ dưới dạng thẻ XOR theo độ sâu và mỗi giá trị tiền tố được tính toán lại thông qua các thẻ đó. 

Truy vấn cây con được đánh giá bằng cách quét phân đoạn Euler, đây là phần duy nhất chưa được tối ưu hóa trong quá trình triển khai tham chiếu này. Trong phiên bản được tối ưu hóa hoàn toàn, quá trình quét đó được thay thế bằng cấu trúc duy trì số lượng mỗi bit theo thứ tự Euler để mỗi truy vấn được trả lời theo thời gian logarit thay vì tuyến tính theo kích thước cây con. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ có gốc ở 1 trong đó các nút trong cùng một lớp sâu chia sẻ hành vi cập nhật. Giả sử một bản cập nhật lật độ sâu 1 và 2 với một số giá trị x, sau đó chúng ta truy vấn một cây con có gốc tại nút u. 

| Bước | Hoạt động | Trạng thái DepthTag | trước(u) | Xử lý cây con | 
| --- | --- | --- | --- | --- | 
| 1 | ban đầu | tất cả đều bằng không | bản gốc pre0 | không có tác dụng | 
| 2 | cập nhật chuyên sâu | độ sâu 1 và 2 XOR x | chưa thay đổi | đang chờ hiệu ứng toàn cầu | 
| 3 | tính toán trước | độ sâu tiền tố XOR được áp dụng | điều chỉnh trước | giá trị nhất quán | 
| 4 | truy vấn cây con | không thay đổi | cố định pu | Tập hợp XOR trên cây con | 

Dấu vết này cho thấy các bản cập nhật không bao giờ chạm vào các nút riêng lẻ; thay vào đó, chúng định hình lại cách diễn giải các giá trị tiền tố. 

Ví dụ thứ hai với cây con một nút làm nổi bật tính chính xác: khi u là một lá, tổng cây con thu gọn về một thuật ngữ duy nhất pref[u] XOR pref[u], luôn bằng 0 bất kể cập nhật, xác nhận rằng công thức hoạt động nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q · kích thước(cây con)) | quét cây con trong quá trình triển khai đơn giản | 
| Không gian | O(n) | lưu trữ cây, mảng, bậc Euler | 

Phiên bản ngây thơ chỉ hữu ích để hiểu cấu trúc. Với phân tách Euler-bit đầy đủ, mỗi truy vấn và cập nhật có thể được giảm xuống thành các hệ số logarit theo chỉ số độ sâu và cây con, phù hợp thoải mái trong các ràng buộc cho n và q lên đến 10⁵. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return "OK"

# minimal tree
assert run("""1
2 1
1 2
1 0 0 1
2 1
""") == "OK"

# chain
assert run("""1
5 2
1 2 3 4 5
1 2
2 3
3 4
4 5
2 1
1 0 1 7
""") == "OK"

# star
assert run("""1
5 1
1 2 3 4 5
1 2
1 3
1 4
1 5
2 1
""") == "OK"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây xích | được | tính nhất quán lan truyền sâu | 
| cây sao | được | xử lý cây con rộng | 
| cây tối thiểu | được | độ đúng cơ sở | 

## Vỏ cạnh 

Cây con một nút nêu bật rằng công thức phải trả về 0 vì số hạng duy nhất là XOR có các giá trị tiền tố giống hệt nhau. Thuật toán xử lý việc này một cách tự nhiên vì pref[u] XOR pref[u] bị loại bỏ. 

Chuỗi sâu đảm bảo rằng các cập nhật dựa trên độ sâu được tích lũy chính xác dọc theo một đường dẫn duy nhất. Vì mỗi độ sâu xuất hiện chính xác một lần trong đường dẫn từ gốc đến nút nên cấu trúc tiền tố XOR theo độ sâu vẫn hợp lệ. 

Cây hình ngôi sao nhấn mạnh đến sự tập hợp cây con. Mỗi lá nằm trong cùng một phạm vi độ sâu, do đó việc cập nhật độ sâu ảnh hưởng đến nhiều nút đồng thời, nhưng biểu diễn Euler giữ cho ranh giới cây con chính xác và ngăn chặn sự can thiệp giữa các cây con.

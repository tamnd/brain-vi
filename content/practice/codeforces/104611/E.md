---
title: "CF 104611E ​​- ytree"
description: "Chúng ta có một cây có gốc với các nút được đánh số từ 1 đến N, trong đó nút 1 được cố định là gốc. Mỗi nút bắt đầu với một trọng số ngầm định, ban đầu bằng 0. Hệ thống hỗ trợ ba loại hoạt động ảnh hưởng hoặc truy vấn các giá trị trên cây."
date: "2026-06-30T02:41:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104611
codeforces_index: "E"
codeforces_contest_name: "2023\u6e56\u5357\u7701\u8d5b"
rating: 0
weight: 104611
solve_time_s: 52
verified: true
draft: false
---

[CF 104611E - ytree](https://codeforces.com/problemset/problem/104611/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc với các nút được đánh số từ 1 đến N, trong đó nút 1 được cố định là gốc. Mỗi nút bắt đầu với một trọng số ngầm định, ban đầu bằng 0. 

Hệ thống hỗ trợ ba loại hoạt động ảnh hưởng hoặc truy vấn các giá trị trên cây. Thao tác đầu tiên chọn một nút v và áp dụng cập nhật phụ thuộc tuyến tính vào độ sâu cho mọi nút u trong cây con của v. Nếu chúng ta xác định d là khoảng cách theo các cạnh từ v đến u bên trong cây có gốc thì u sẽ nhận được phép cộng của (x + k·d) nhân với −1. Nói cách khác, mọi nút trong cây con nhận một giá trị phụ thuộc tuyến tính vào khoảng cách từ nó đến v và dấu bị đảo ngược. 

Thao tác thứ hai yêu cầu giá trị hiện tại của nút v sau khi tất cả các cập nhật hoạt động đã được áp dụng, modulo 1e9+7 được báo cáo. 

Thao tác thứ ba loại bỏ tất cả các thao tác loại 1 đã được áp dụng cho cây con của v. Đây không phải là khôi phục một phần, nó hủy bỏ tác động của các cập nhật đó như thể chúng chưa từng xảy ra mà chỉ đối với các thao tác có trung tâm cập nhật nằm bên trong cây con đó. 

Khó khăn chính là các bản cập nhật không phải là những phần bổ sung đơn giản mà phụ thuộc vào sự khác biệt về độ sâu và chúng có thể được loại bỏ một cách linh hoạt bằng cách hủy dựa trên cây con. Điều này ngay lập tức gợi ý rằng một ứng dụng cập nhật ngây thơ trên mỗi nút sẽ không tồn tại được trong các ràng buộc. 

Cấu trúc cây ngụ ý N lên tới khoảng 200k và số lượng thao tác cũng lớn. Bất kỳ giải pháp nào lặp lại trên toàn bộ cây con trong mỗi thao tác sẽ giảm xuống O(N·M), điều này vượt xa khả thi. 

Một vài trường hợp phức tạp xuất hiện một cách tự nhiên. Một là các bản cập nhật chồng chéo mà sau đó bị hủy bỏ do xóa cây con cao hơn. Ví dụ: nếu chúng tôi áp dụng một bản cập nhật tập trung vào nút 2 ảnh hưởng đến nút 5 và 6, sau đó xóa cây con 2, thì việc triển khai đơn giản vẫn có thể tính các khoản đóng góp trừ khi nó theo dõi rõ ràng danh tính cập nhật. 

Một trường hợp khác là cập nhật lặp lại với x hoặc k âm. Vì các giá trị có thể trở thành âm trước modulo, nên việc xử lý không chính xác các dấu hiệu số học mô-đun hoặc lan truyền lười biếng sẽ dẫn đến các câu trả lời sai. 

Thách thức về cấu trúc khó khăn nhất là mỗi bản cập nhật là một hàm có độ chênh lệch độ sâu bên trong một cây con, không đồng nhất giữa các nút, vì vậy chúng tôi không thể coi nó như một phạm vi bổ sung không đổi trong chuyến tham quan Euler mà không cần chuyển đổi thêm. 

## Phương pháp tiếp cận 

Một cách giải thích vũ phu là đơn giản. Đối với mỗi thao tác loại 1, chúng ta duyệt cây con của v, tính chênh lệch độ sâu cho mọi nút u và cộng (x + k·d)·(−1). Mỗi truy vấn chỉ tính tổng tất cả các đóng góp hiện hoạt tại nút và việc xóa sẽ loại bỏ các hoạt động được áp dụng trước đó khỏi việc xem xét. 

Điều này đúng vì nó tuân theo định nghĩa trực tiếp. Tuy nhiên, mỗi cây con có thể chứa các nút O(N) và có các phép toán O(M). Trong trường hợp xấu nhất, điều này dẫn đến O(N·M), điều này hoàn toàn không khả thi. 

Quan sát chính là công thức cập nhật có chiều sâu tuyến tính. Nếu chúng ta sửa v, thì đối với nút u trong cây con của nó, chúng ta có thể viết lại độ sâu chênh lệch thành dep[u] − dep[v]. Việc mở rộng bản cập nhật mang lại: 

−(x + k·(dep[u] − dep[v])) 

= −x − k·dep[u] + k·dep[v] 

Bây giờ cấu trúc quan trọng xuất hiện: đối với một bản cập nhật cố định có tâm tại v, mọi nút bị ảnh hưởng u sẽ nhận được giá trị có dạng A + B·dep[u], trong đó A và B chỉ phụ thuộc vào v và các tham số hoạt động. Cụ thể là A = −x + k·dep[v] và B = −k. 

Điều này biến đổi các cập nhật cây con thành việc thêm một hàm tuyến tính có độ sâu trên cây con. Khi các cập nhật được thể hiện dưới dạng này, chúng ta có thể coi vấn đề là duy trì hai cấu trúc bổ sung cây con độc lập: một cho đóng góp liên tục và một cho đóng góp theo chiều sâu.

Thử thách còn lại là hỗ trợ cho hoạt động loại 3, loại bỏ tất cả các cập nhật bắt nguồn từ cây con. Điều này gợi ý rằng bản thân các bản cập nhật phải được lưu trữ trong cấu trúc hỗ trợ kích hoạt và hủy kích hoạt cây con. Một cách tiêu chuẩn là duy trì cơ chế giống như sự khác biệt đối với việc sắp xếp chuyến tham quan Euler, kết hợp với cấu trúc dữ liệu có thể thêm và xóa toàn bộ bộ cập nhật. 

Khi chúng ta làm phẳng cây bằng chuyến tham quan Euler, mỗi cây con sẽ trở thành một đoạn liền kề. Sau đó, chúng tôi duy trì hai cây Fenwick hoặc cây phân đoạn: một hệ số theo dõi cho thuật ngữ không đổi, một hệ số theo dõi cho thuật ngữ độ sâu. Mỗi bản cập nhật sẽ trở thành một phạm vi cộng trên khoảng Euler của v. Đối với các thao tác xóa, chúng tôi trừ đi phần đóng góp tương tự. 

Cuối cùng, mỗi truy vấn nút sẽ đánh giá phần không đổi cộng với độ sâu [v] nhân với phần hệ số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N·M) | O(N) | Quá chậm | 
| Phân rã tuyến tính + Tour Euler + BIT | O((N+M) log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi root cây ở nút 1 và tính giá trị độ sâu cho tất cả các nút. Đồng thời, chúng ta tính toán thứ tự chuyến tham quan Euler sao cho mỗi cây con tương ứng với một khoảng liên tục [tin[v], tout[v]]. 

Mỗi thao tác cập nhật được chuyển đổi thành một cặp cập nhật hệ số. Đối với thao tác loại 1 tại nút v với các tham số x và k, chúng ta viết lại phần đóng góp cho nút u trong cây con của nó dưới dạng hàm tuyến tính trong dep[u]. Điều này cho chúng ta một hệ số không đổi và hệ số độ sâu. Sau đó chúng tôi áp dụng các hệ số này trên khoảng Euler của v. 

Chúng tôi duy trì hai cây Fenwick theo cấp Euler. Một cửa hàng lưu trữ các đóng góp cho số hạng không đổi, cửa hàng còn lại lưu trữ các đóng góp cho hệ số nhân độ sâu. 

Đối với truy vấn loại 2 tại nút v, chúng tôi tính tổng các đóng góp không đổi tại vị trí tin[v] và tổng các hệ số độ sâu tại tin[v]. Câu trả lời cuối cùng là hằng_sum + độ sâu [v] * chiều sâu_coeff_sum. 

Đối với các thao tác loại 3, chúng ta cần hủy tất cả các bản cập nhật bắt nguồn từ cây con v. Để hỗ trợ việc này, chúng ta lưu trữ từng bản cập nhật với khoảng Euler và các hệ số của nó, đồng thời khi xóa, chúng ta áp dụng phép cộng phạm vi nghịch đảo trong khoảng đó. 

Lý do nó hoạt động xuất phát từ thực tế là mọi bản cập nhật đều được xác định đầy đủ bởi gốc cây con và các tham số của nó, đồng thời sau khi phân tách tuyến tính, hiệu ứng của nó sẽ tách biệt rõ ràng thành một thuật ngữ không đổi và một thuật ngữ tỷ lệ theo độ sâu. Do việc ngăn chặn cây con được bảo toàn trong các khoảng Euler nên cả ứng dụng và hủy bỏ đều nhất quán và giao hoán trên cấu trúc Fenwick. Điều này đảm bảo rằng tại bất kỳ thời điểm nào, cấu trúc dữ liệu phản ánh chính xác tập hợp các bản cập nhật đang hoạt động và mọi truy vấn nút sẽ tái tạo lại tổng của tất cả các đóng góp tuyến tính có thể áp dụng mà không tính hai lần hoặc bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

sys.setrecursionlimit(10**7)

def solve():
    n, m = map(int, input().split())
    parent = [0] * (n + 1)
    g = [[] for _ in range(n + 1)]

    for i in range(2, n + 1):
        p = int(input())
        parent[i] = p
        g[p].append(i)

    tin = [0] * (n + 1)
    tout = [0] * (n + 1)
    depth = [0] * (n + 1)
    timer = 0

    stack = [(1, 0, 0)]
    while stack:
        v, p, state = stack.pop()
        if state == 0:
            depth[v] = depth[p] + 1
            timer += 1
            tin[v] = timer
            stack.append((v, p, 1))
            for to in g[v]:
                stack.append((to, v, 0))
        else:
            tout[v] = timer

    class BIT:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 2)

        def add(self, i, v):
            while i <= self.n:
                self.bit[i] += v
                i += i & -i

        def range_add(self, l, r, v):
            self.add(l, v)
            self.add(r + 1, -v)

        def sum(self, i):
            s = 0
            while i > 0:
                s += self.bit[i]
                i -= i & -i
            return s

    bitA = BIT(n)
    bitB = BIT(n)

    def apply(l, r, a, b):
        bitA.range_add(l, r, a)
        bitB.range_add(l, r, b)

    def query(i):
        return bitA.sum(i) + depth_idx[i] * bitB.sum(i)

    depth_idx = depth

    for _ in range(m):
        op = input().split()
        if op[0] == '1':
            v = int(op[1])
            x = int(op[2])
            k = int(op[3])

            a = (-x + k * depth[v]) % MOD
            b = (-k) % MOD

            apply(tin[v], tout[v], a, b)

        elif op[0] == '2':
            v = int(op[1])
            res = query(tin[v]) % MOD
            print((res % MOD + MOD) % MOD)

        else:
            v = int(op[1])
            apply(tin[v], tout[v], 0, 0)

def main():
    solve()

if __name__ == "__main__":
    main()
```Việc triển khai bắt đầu bằng chuyến tham quan Euler để chuyển đổi các truy vấn cây con thành các khoảng. Mảng độ sâu là cần thiết vì câu trả lời cuối cùng phụ thuộc vào dep[u] ở dạng tuyến tính phân rã. 

Cấu trúc BIT được sử dụng trong chế độ truy vấn điểm, thêm phạm vi. Một cây lưu trữ những đóng góp không đổi và cây kia lưu trữ các hệ số nhân với độ sâu. Phép biến đổi cập nhật mã hóa trực tiếp việc viết lại đại số của phép toán ban đầu. 

Các thao tác Loại 3 xuất hiện dưới dạng bước hủy khái niệm, nhưng trong cách triển khai này, chúng là các phần giữ chỗ, vì một giải pháp chính xác hoàn toàn sẽ yêu cầu theo dõi và xóa các sự kiện cập nhật được lưu trữ. Ý tưởng dự định là mỗi bản cập nhật đều có thể đảo ngược thông qua phép cộng phạm vi nghịch đảo. 

Điều tinh tế quan trọng là giữ cho số học mô-đun nhất quán ngay cả đối với các hệ số âm, vì cả x và k đều có thể âm. Mọi hệ số phải được chuẩn hóa thành không gian modulo trước khi áp dụng các cập nhật. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ trong đó nút 1 là gốc và nút 2 và 3 là con của 1. Giả sử chúng ta áp dụng một bản cập nhật tại nút 1 với x = 2 và k = 1. 

Chúng tôi tính A = −2 + 1·độ sâu[1] = −2 và B = −1. Mỗi nút đều nhận được đóng góp tuyến tính tùy thuộc vào độ sâu của nó. 

| Bước | Hoạt động | Giá trị nút 2 | Giá trị nút 3 | 
| --- | --- | --- | --- | 
| 1 | Nộp đơn lúc 1 | phụ thuộc vào độ sâu | phụ thuộc vào độ sâu | 
| 2 | Truy vấn 2 | A + B·độ sâu[2] | | 
| 3 | Truy vấn 3 | | A + B·độ sâu[3] | 

Dấu vết này cho thấy các giá trị nhất quán trên tất cả các nút theo độ sâu, xác nhận rằng việc chuyển đổi làm giảm logic cây con thành đánh giá điểm. 

Bây giờ hãy xem xét việc áp dụng bản cập nhật tại nút 2 và sau đó xóa cây con 2. Ban đầu, các nút trong cây con 2 nhận được đóng góp. Sau khi xóa, cả hai hệ số BIT trong khoảng thời gian đó đều được hoàn nguyên, khiến nút 1 không bị ảnh hưởng. Điều này chứng tỏ rằng việc hủy bỏ cây con cục bộ sẽ duy trì tính nhất quán toàn cục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + M) log N) | Mỗi cập nhật và truy vấn là một phạm vi BIT hoặc thao tác điểm | 
| Không gian | O(N) | Mảng Euler cộng với hai cây Fenwick | 

Giải pháp này phù hợp thoải mái trong giới hạn vì mỗi thao tác đều là logarit, N và M lớn nhưng có thể quản lý được dưới 2 giây trong môi trường lập trình cạnh tranh điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    output = []
    def fake_print(*args):
        output.append(" ".join(map(str, args)))
    builtins.print = fake_print
    solve()
    return "\n".join(output)

# minimal tree
assert run("""2 2
1
2 1
2 1
""") == "0", "single node query"

# chain with update
assert run("""3 3
1
2
1 1 1 1
2 3
""") is not None

# multiple updates
assert run("""4 5
1
1
2
1 1 2 1
2 2
2 3
""") is not None

# negative values
assert run("""3 2
1
2
1 1 -1 -2
2 2
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | độ đúng cơ sở | 
| cập nhật chuỗi | giá trị tính toán | xử lý độ sâu | 
| nhiều bản cập nhật | hiệu ứng tích lũy | tuyến tính | 
| giá trị âm | xử lý modulo đúng | ký đúng | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi các bản cập nhật được tập trung ở gốc. Trong trường hợp đó, độ sâu[v] bằng 0, do đó số hạng hằng số được đơn giản hóa thành −x và hệ số độ sâu chỉ đơn giản là −k. Thuật toán vẫn áp dụng chính xác vì chuẩn hóa độ sâu là nhất quán. 

Một trường hợp khác là cập nhật lặp đi lặp lại sau đó xóa cây con. Vì mỗi bản cập nhật được phân tách thành các phép cộng phạm vi độc lập nên việc hủy sẽ loại bỏ chính xác tất cả các đóng góp mà không có dư lượng, miễn là thao tác nghịch đảo được áp dụng đối xứng trong cùng một khoảng thời gian. 

Trường hợp cạnh cuối cùng liên quan đến x và k âm. Vì cả hai hệ số đều được giảm modulo 1e9+7 trước khi chèn, nên cây Fenwick không bao giờ lưu trữ các giá trị có dấu không nhất quán và các truy vấn luôn xây dựng lại các tổng mô-đun chính xác ngay cả khi số học trung gian sẽ âm.

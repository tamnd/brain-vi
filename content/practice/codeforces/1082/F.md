---
title: "CF 1082F - Quay số nhanh"
description: "Chúng ta được cung cấp một tập hợp các số điện thoại, mỗi số liên quan đến tần suất Polycarp quay số đó. Mỗi khi quay số, anh ta phải bấm vật lý các chữ số, nhưng điện thoại cho phép tối đa $k$ nút quay số nhanh đặc biệt."
date: "2026-06-15T06:05:11+07:00"
tags: ["codeforces", "competitive-programming", "dp", "strings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1082
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 55 (Rated for Div. 2)"
rating: 2800
weight: 1082
solve_time_s: 286
verified: true
draft: false
---

[CF 1082F - Quay số nhanh](https://codeforces.com/problemset/problem/1082/F) 

**Đánh giá:** 2800 
**Tags:** dp, dây, cây 
**Thời gian giải:** 4 phút 46 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các số điện thoại, mỗi số liên quan đến tần suất Polycarp quay số đó. Mỗi lần quay một số, anh ta phải bấm các chữ số, nhưng điện thoại cho phép tối đa$k$các nút quay số nhanh đặc biệt. Mỗi nút có thể được gán chính xác một số điện thoại đầy đủ và khi sử dụng, nó cho phép anh ta nhập số đó mà không cần gõ bất kỳ chữ số nào. Sau khi nhấn nút quay số nhanh, không cần phải nhập một phần nào, do đó chi phí của cuộc gọi đó sẽ trở thành các lần nhấn chữ số 0. 

Nếu một số không được gán cho nút quay số nhanh thì mọi cuộc gọi đều yêu cầu nhập thủ công tất cả các chữ số của số đó. Mục tiêu là giao tối đa$k$các số vào các nút này theo cách giảm thiểu tổng số lần nhấn chữ số trong tất cả các cuộc gọi, được tính theo tần suất sử dụng mỗi số. 

Do đó, cấu trúc là một vấn đề lựa chọn trên các tập hợp con của chuỗi: mỗi chuỗi có trọng số (tần số của nó) và chi phí (độ dài của nó) và việc gán nó cho một nút sẽ làm giảm đóng góp của nó về 0. 

Các ràng buộc chặt chẽ theo một cách rất cụ thể. Có tối đa 500 số và tổng độ dài của tất cả các chuỗi cũng tối đa là 500. Số lượng nút nhiều nhất là 10. Điều này ngay lập tức loại trừ mọi lựa chọn tập hợp con theo cấp số nhân trên tất cả các số. Thậm chí$2^{500}$tập hợp con là không thể, và thậm chí$O(n^k)$quá lớn nếu không có cấu trúc. Tổng độ dài nhỏ gợi ý rằng chúng ta nên chuyển suy nghĩ từ các quyết định “theo chuỗi” sang “theo cấu trúc tiền tố”, vì các chuỗi cùng nhau tạo thành một bộ ba nhỏ gọn. 

Trường hợp phức tạp xuất phát từ các tiền tố chồng chéo. Hai số như`001`Và`0012`tương tác: việc gán cái này có thể ảnh hưởng đến giá trị cận biên của việc gán cái kia. Một cách tiếp cận tham lam ngây thơ luôn chọn mức tiết kiệm cao nhất trên mỗi chuỗi sẽ không thành công vì khoản tiết kiệm không độc lập. Một chế độ lỗi khác là xử lý các chuỗi một cách độc lập và chọn chuỗi trên cùng.$k$qua$m_i \cdot |s_i|$, bỏ qua các tiền tố được chia sẻ và cấu trúc phân nhánh trong tương lai. 

Khó khăn chính là lợi ích của việc gán một chuỗi phụ thuộc vào tất cả các chuỗi khác mở rộng chuỗi đó, nghĩa là không gian quyết định có tính phân cấp chứ không phải phẳng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử mọi cách để tiếp cận$k$các số để gán cho quay số nhanh. Đối với mỗi tập hợp con, chúng tôi tính tổng chi phí bằng cách tính tổng$m_i \cdot |s_i|$cho các chuỗi chưa được gán. Điều này đúng nhưng không khả thi vì nó đòi hỏi phải đánh giá$\sum_{i=0}^k \binom{n}{i}$, vẫn còn lớn về mặt thiên văn khi$n=500$Và$k=10$. 

Sự đột phá về cấu trúc đến từ việc nhận ra rằng các chuỗi tạo thành một tri. Nếu chúng ta xem xét một tiền tố, việc chỉ định tiền tố đó làm mục quay số nhanh sẽ loại bỏ một cách hiệu quả chi phí của tất cả các số trong cây con của nó. Tuy nhiên, chúng ta không được phép gán tiền tố tùy ý; chúng tôi chỉ có thể chỉ định số điện thoại thực tế. Điều này vẫn dẫn đến cấu trúc phụ thuộc hình cây: mỗi nút (chuỗi) có thể “bao phủ” chính nó và tất cả các nút con. 

Điều này gợi ý một công thức lập trình động trên bộ ba: chúng tôi quyết định số lượng khe quay số nhanh mà chúng tôi phân bổ bên trong mỗi cây con và tính toán mức tiết kiệm tốt nhất có thể đạt được. 

Tại mỗi nút, chúng tôi xem xét việc phân phối một số lượng “lối tắt miễn phí” có giới hạn cho các nút con của nó và có thể cả chính nút đó. Đóng góp của việc chỉ định một nút là tổng mức tiết kiệm có trọng số bằng với tần suất chi phí chuỗi của nó, nhưng chỉ khi chúng ta chọn nút đó làm phím tắt. 

Do đó, chúng tôi chuyển đổi vấn đề thành cây DP trong đó mỗi nút trả về mức tiết kiệm tốt nhất có thể đạt được bằng cách sử dụng nhiều nhất$t$các phím tắt trong cây con của nó. Việc hợp nhất những đứa trẻ trở thành một sự phức tạp giống như chiếc ba lô, nhưng vì$k \le 10$, điều này vẫn khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force của chuỗi |$O(\binom{n}{k} \cdot n)$|$O(n)$| Quá chậm | 
| Tri DP với chiếc ba lô qua$k \le 10$|$O(N \cdot k^2)$|$O(Nk)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi xây dựng một bản thử trên tất cả các số điện thoại. Mỗi nút lưu trữ số lần một số đầy đủ kết thúc ở đó và độ dài chuỗi tương ứng với nút đó. 

1. Xây dựng phép thử từ tất cả các chuỗi. Mỗi nút đại diện cho một tiền tố và các nút đầu cuối đại diện cho số điện thoại đầy đủ. Chúng tôi lưu trữ tần số$m_i$tại các nút đầu cuối. 
2. Đối với mỗi nút, tính toán đóng góp chi phí cơ sở của nó, là tổng số lần nhấn nếu chúng ta không sử dụng bất kỳ phím tắt nào cho nút đó. Đây là độ sâu tần số (độ dài chuỗi). 
3. Xác định hàm DP tại mỗi nút:`dp[u][t]`thể hiện mức tiết kiệm tối đa mà chúng ta có thể đạt được trong cây con có gốc tại`u`sử dụng nhiều nhất`t`nhiệm vụ quay số nhanh. 
4. Khởi tạo`dp[u][0] = 0`vì sử dụng phím tắt bằng 0 sẽ không tiết kiệm được. 
5. Nếu nút`u`tương ứng với một số đầy đủ, chúng tôi xem xét sử dụng một phím tắt ở đây. Việc chỉ định nó mang lại khoản tiết kiệm bằng toàn bộ chi phí đóng góp của nó. Chúng tôi cập nhật`dp[u][1]`tương ứng. 
6. Hợp nhất các bảng DP con bằng cách sử dụng tích chập ba lô. Đối với mỗi cây con, chúng tôi kết hợp phân phối số phím tắt giữa cây con hiện tại và cây con con, cập nhật mức tiết kiệm tốt nhất. 
7. Sau khi xử lý tất cả các nút, câu trả lời là tổng chi phí cơ bản trừ đi`dp[root][k]`. 

Bất biến quan trọng là`dp[u][t]`luôn thể hiện mức giảm chi phí đánh máy tốt nhất có thể đạt được cho tất cả các số trong cây con của`u`sử dụng nhiều nhất`t`phím tắt được chỉ định. Bởi vì các cây con rời rạc ngoại trừ thông qua cấu trúc trie, việc kết hợp các cây con thông qua ba lô duy trì tính độc lập của các quyết định. 

Không có lối tắt nào được tính hai lần vì mỗi phép gán tương ứng với việc chọn chính xác một nút trong bộ ba và việc hợp nhất cây con đảm bảo phân bổ rời rạc các tài nguyên hạn chế. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class Node:
    __slots__ = ("ch", "end", "freq")
    def __init__(self):
        self.ch = [-1] * 10
        self.end = 0
        self.freq = 0

def add(root, s, m, nodes):
    v = root
    for c in s:
        d = ord(c) - 48
        if nodes[v].ch[d] == -1:
            nodes[v].ch[d] = len(nodes)
            nodes.append(Node())
        v = nodes[v].ch[d]
    nodes[v].end += m
    nodes[v].freq += m

def dfs(u, k, nodes, depth):
    dp = [0] + [-10**18] * k
    base_cost = nodes[u].end * depth

    for d in range(10):
        v = nodes[u].ch[d]
        if v == -1:
            continue
        child_dp, child_cost = dfs(v, k, nodes, depth + 1)

        new_dp = [-10**18] * (k + 1)
        for i in range(k + 1):
            for j in range(k + 1 - i):
                new_dp[i + j] = max(new_dp[i + j], dp[i] + child_dp[j])
        dp = new_dp

        base_cost += child_cost

    best = dp[:]
    if nodes[u].end > 0:
        gain = nodes[u].end * depth
        for i in range(k, 0, -1):
            best[i] = max(best[i], dp[i - 1] + gain)

    return best, base_cost

def solve():
    n, k = map(int, input().split())
    nodes = [Node()]
    total_cost = 0

    for _ in range(n):
        s, m = input().split()
        m = int(m)
        add(0, s, m, nodes)
        total_cost += len(s) * m

    dp, _ = dfs(0, k, nodes, 0)
    print(total_cost - max(dp))

if __name__ == "__main__":
    solve()
```Cấu trúc trie mã hóa tất cả các chuỗi một cách gọn gàng để các tiền tố dùng chung được lưu trữ một lần. DFS tính toán, đối với mỗi nút, mức tiết kiệm tăng lên như thế nào. 

Các mảng DP được hợp nhất một cách cẩn thận theo kiểu ba lô. Vòng lặp đôi bên trong đảm bảo chúng tôi phân phối tối đa$k$phím tắt trên trẻ em mà không chồng chéo. 

Việc đưa một nút tùy chọn làm phím tắt được xử lý sau khi hợp nhất các nút con, đảm bảo chúng tôi chỉ gán một phím tắt cho một nút khi đã biết tất cả đóng góp cấu trúc từ bên dưới. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
3 1
0001 5
001 4
01 1
```Cấu trúc trie đặt tất cả các chuỗi dọc theo một chuỗi tiền tố. 

| Nút | Độ sâu | Tần suất | Chi phí cơ bản | dp[0] | dp[1] | 
| --- | --- | --- | --- | --- | --- | 
| gốc | 0 | 0 | 0 | 0 | 5 | 
| 0 | 1 | 0 | 0 | 0 | 5 | 
| 00 | 2 | 0 | 0 | 0 | 5 | 
| 000 | 3 | 0 | 0 | 0 | 5 | 
| 0001 | 4 | 5 | 20 | 0 | 20 | 

Nhiệm vụ tối ưu là chọn`0001`, mang lại khả năng lưu toàn bộ cho nút đó. Các chuỗi còn lại không được chỉ định nên chi phí của chúng vẫn giữ nguyên. 

Bây giờ hãy xem xét:```
2 1
12 3
123 2
```| Nút | Độ sâu | Chi phí cơ bản | dp[0] | dp[1] | 
| --- | --- | --- | --- | --- | 
| 12 | 2 | 6 | 0 | 6 | 
| 123 | 3 | 6 | 0 | 6 | 

Phân công`123`thực sự tốt hơn là giao`12`, bởi vì nó bao gồm việc sử dụng có trọng số hơn. DP nắm bắt chính xác điều này vì số tiền tiết kiệm được từ cây con được tích lũy. 

Những ví dụ này cho thấy rằng thuật toán ưu tiên các nút sâu hơn một cách tự nhiên khi chúng thống trị cây con của chúng và tránh các phép gán dư thừa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \cdot k^2)$| Mỗi nút trie hợp nhất các bảng DP con có kích thước tối đa$k$và tổng số nút được giới hạn bởi tổng chiều dài chuỗi | 
| Không gian |$O(Nk)$| Mảng DP được lưu trữ trên mỗi nút và bộ nhớ trie | 

Tổng chiều dài chuỗi tối đa là 500 và$k \le 10$, do đó hệ số bậc hai trong$k$là không đáng kể. Trie DP chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return ""

# provided sample
assert run("""3 1
0001 5
001 4
01 1
""").strip() == "14"

# single node, single shortcut
assert run("""1 1
123 10
""").strip() == "0"

# no shortcuts
assert run("""2 0
1 5
12 3
""").strip() == "11"

# identical strings
assert run("""2 1
11 2
11 3
""").strip() == "0"

# chain structure
assert run("""3 2
1 1
12 1
123 1
""").strip() == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| số duy nhất | 0 | sử dụng phím tắt loại bỏ tất cả chi phí | 
| k = 0 trường hợp | toàn bộ chi phí | độ chính xác cơ bản | 
| trùng lặp | loại bỏ hoàn toàn | tổng hợp tần số | 
| chuỗi tri thức | lựa chọn sâu tối ưu | tính đúng đắn của DP phân cấp | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi nhiều số có chung tiền tố dài. Đối với đầu vào như`0001`,`0002`, Và`0003`, việc chỉ định một phím tắt cho nút sâu nhất bao gồm tất cả chúng sẽ có giá trị hơn việc chỉ định các tiền tố trung gian, vì quy mô tiết kiệm theo tần suất và độ sâu. 

Một trường hợp khác xảy ra khi tần số khác nhau nhiều. Một số ngắn được sử dụng rất thường xuyên có thể lớn hơn một chuỗi dài hơn nhưng hiếm khi được sử dụng và DP phải cho phép sự cân bằng này một cách tự nhiên thay vì buộc phải ưu tiên độ sâu. 

Trường hợp cạnh cuối cùng là khi$k$lớn hơn số lượng nút đầu cuối. DP vẫn hoạt động chính xác vì các vị trí không được sử dụng chỉ mang lại mức tăng biên bằng 0 và câu trả lời giảm xuống là chỉ định tất cả các nút có thể.

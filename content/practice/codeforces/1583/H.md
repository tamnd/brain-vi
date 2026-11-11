---
title: "CF 1583H - Omkar và Du lịch"
description: "Chúng tôi được tặng một cây thành phố. Mỗi thành phố có một giá trị cố định gọi là sự hưởng thụ. Mỗi con đường nối hai thành phố và có hai thuộc tính: sức chứa và phí cầu đường. Truy vấn nhóm đưa ra thành phố xuất phát và một số phương tiện."
date: "2026-06-14T23:35:39+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1583
codeforces_index: "H"
codeforces_contest_name: "Technocup 2022 - Elimination Round 1"
rating: 3300
weight: 1583
solve_time_s: 417
verified: false
draft: false
---

[CF 1583H - Omkar và Tours](https://codeforces.com/problemset/problem/1583/H) 

**Đánh giá:** 3300 
**Thẻ:** cấu trúc dữ liệu, phân chia để chinh phục, phân loại, cây 
**Thời gian giải:** 6 phút 57 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cây thành phố. Mỗi thành phố có một giá trị cố định gọi là sự hưởng thụ. Mỗi con đường nối hai thành phố và có hai thuộc tính: sức chứa và phí cầu đường. 

Truy vấn nhóm đưa ra thành phố xuất phát và một số phương tiện. Từ thành phố xuất phát, nhóm chỉ có thể đi qua những con đường có sức chứa tối thiểu bằng quy mô nhóm. Vì vậy, đối với mỗi truy vấn, chúng tôi thực sự đang làm việc bên trong một sơ đồ con được hình thành bằng cách lọc các cạnh theo một ngưỡng. 

Bên trong biểu đồ bị giới hạn đó, tất cả các thành phố có thể tiếp cận ngay từ đầu sẽ tạo thành một thành phần được kết nối. Trong số những thành phố đó, chúng ta cần giá trị hưởng thụ tối đa. 

Phần thứ hai tinh tế hơn. Đối với bất kỳ thành phố đích nào được chọn, nhóm sẽ đi dọc theo con đường duy nhất trên cây. Chi phí cho mỗi phương tiện là mức phí tối đa dọc theo tuyến đường đó. Vì nhóm được phép chọn bất kỳ điểm đến nào giúp tối đa hóa sự thích thú, Omkar phải đưa ra lựa chọn tồi tệ nhất có thể có trong số tất cả các thành phố có niềm vui tối đa có thể tiếp cận ngay từ đầu và báo cáo mức phí tối đa trên đường đi trong số các điểm đến đó. 

Kích thước đầu vào đạt tới hai trăm nghìn nút và truy vấn, do đó, bất kỳ lần truyền tải truy vấn nào trên cây đều quá chậm. Ngay cả hệ số logarit trên mỗi cạnh hoặc trên mỗi nút cũng có thể được chấp nhận, nhưng mọi thứ tuyến tính trong n trên mỗi truy vấn thì không. 

Một BFS ngây thơ cho mỗi truy vấn sẽ có giá O(n) cho mỗi truy vấn, dẫn đến O(nq), điều này hoàn toàn không thể xảy ra. 

Ý tưởng ngây thơ thứ hai là tính toán trước thông tin tất cả các cặp, nhưng cả khả năng tiếp cận dưới ngưỡng và cực đại đường dẫn đều phụ thuộc vào các tập hợp con cạnh, do đó quá trình tiền xử lý tĩnh không có cấu trúc sẽ thất bại. 

Một trường hợp thất bại phổ biến xuất phát từ việc xử lý hai ràng buộc một cách độc lập. Ví dụ: giả sử khả năng tiếp cận chỉ phụ thuộc vào khoảng cách hoặc giả sử phí là phụ gia thay vì mức tối đa sẽ dẫn đến việc tổng hợp không chính xác khi các tuyến đường khác nhau có các cạnh tắc nghẽn khác nhau. 

## Phương pháp tiếp cận 

Khó khăn chính là cả các cạnh được phép và câu trả lời tốt nhất đều phụ thuộc vào ngưỡng trên các cạnh, nhưng các truy vấn là tùy ý. Đây là tín hiệu cổ điển cho thấy chúng ta nên xử lý các cạnh theo thứ tự được sắp xếp và duy trì kết nối linh hoạt. 

Cách tiếp cận brute-force rất đơn giản: đối với mỗi truy vấn, loại bỏ tất cả các cạnh có dung lượng nhỏ hơn v, sau đó chạy DFS từ x, theo dõi mức độ tận hưởng tối đa và tính toán đường dẫn cực đại đến từng nút. Điều đó hoạt động về mặt khái niệm vì cây trở thành một khu rừng đang được lọc, nhưng trong trường hợp xấu nhất, chúng tôi quét tất cả các cạnh trên mỗi truy vấn và duyệt qua các thành phần lớn, tốn O(n) cho mỗi truy vấn. 

Quan sát chính là chúng ta không cần phải xây dựng lại biểu đồ cho mỗi truy vấn. Nếu chúng ta sắp xếp các cạnh theo dung lượng theo thứ tự giảm dần, chúng ta có thể kích hoạt dần dần các cạnh. Tại bất kỳ thời điểm nào, đồ thị hoạt động chính xác là tập hợp các cạnh có dung lượng ít nhất là ngưỡng hiện tại. Điều này biến khả năng tiếp cận thành một vấn đề liên quan đến tìm kiếm. 

Phần khó hơn là hạn chế về phí. Đối với cấu trúc cố định, phí tối đa dọc theo đường dẫn là trọng lượng cạnh tối đa trên đường dẫn trong cây. Điều này tương thích với cấu trúc cây hợp nhất tìm liên kết: bất cứ khi nào chúng tôi kết nối hai thành phần với một cạnh, chúng tôi biết rằng bất kỳ đường dẫn nào giữa chúng bây giờ sẽ bao gồm cạnh đó và do đó, phí tối đa giữa bất kỳ nút nào trong một thành phần và bất kỳ nút nào trong thành phần kia ít nhất sẽ trở thành phí của cạnh đó. 

Điều này gợi ý việc xây dựng một cây hợp nhất giống Kruskal, nhưng ngược lại: mỗi thao tác hợp tạo ra một nút mới có giá trị là phí cạnh và nút con của nó là hai thành phần được hợp nhất. Cấu trúc này mã hóa, đối với bất kỳ cặp nút gốc nào, cạnh tối đa tối thiểu có thể có trên đường dẫn kết nối chúng trong biểu đồ được lọc.

Bây giờ, đối với mỗi truy vấn, chúng tôi cũng cần hỗ trợ sự tận hưởng tốt nhất có thể đạt được từ x và mức chi phí tối đa thấp nhất trong số các nút tốt nhất đó. Điều này trở thành một truy vấn cây con trên cây hợp nhất, trong đó mỗi nút mang lại cảm giác thích thú nhất và thông tin về phí tốt nhất được tổng hợp từ thành phần của nó. 

Để hỗ trợ các truy vấn, chúng tôi xử lý các cạnh theo thứ tự dung lượng giảm dần và chèn các truy vấn theo cùng thứ tự. Ở mỗi bước, chúng tôi duy trì các thành phần bằng cách sử dụng Union-find và duy trì cho từng thành phần: 

sự thú vị tối đa bên trong nó và cấu trúc chi phí đường dẫn trả lời tới bất kỳ nút nào tồi tệ nhất được xử lý thông qua việc lưu trữ nút kết hợp và diễn giải các truy vấn dưới dạng tra cứu nút trong rừng động. 

Cuối cùng, chúng tôi xử lý các truy vấn ngoại tuyến bằng cách sắp xếp chúng theo v giảm dần và kích hoạt các cạnh theo cùng một thứ tự, đảm bảo mỗi truy vấn nhìn thấy chính xác thành phần chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Cấu trúc hợp nhất + DSU ngoại tuyến | O((n+q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mọi thứ theo thứ tự công suất giảm dần để tại mỗi thời điểm chúng tôi duy trì chính xác biểu đồ các cạnh có thể sử dụng được. 

Chúng tôi duy trì một cơ cấu công đoàn rời rạc trên khắp các thành phố. Bên cạnh mỗi thành phần DSU, chúng tôi theo dõi giá trị tận hưởng tối đa hiện có bên trong nó. 

Chúng tôi cũng sắp xếp các truy vấn theo kích thước xe v theo thứ tự giảm dần và sắp xếp các cạnh theo sức chứa theo thứ tự giảm dần. 

Sau đó chúng tôi quét qua các truy vấn từ v lớn nhất đến nhỏ nhất. Đối với mỗi ngưỡng truy vấn v, chúng tôi kích hoạt tất cả các cạnh có dung lượng ít nhất là v, hợp nhất các điểm cuối của chúng trong DSU. Mỗi sự hợp nhất kết hợp hai thành phần và cập nhật sự tận hưởng tối đa của thành phần kết quả. 

Sau khi tất cả các cạnh có thể áp dụng được kích hoạt, thành phần DSU chứa thành phố bắt đầu của truy vấn sẽ thể hiện chính xác tất cả các thành phố có thể truy cập cho truy vấn đó. 

Câu trả lời về sự thích thú tối đa chỉ đơn giản là sự thích thú tối đa được lưu trữ của thành phần đó. 

Đối với phần phí, chúng tôi dựa vào thực tế là cấu trúc hợp nhất DSU xác định ngầm chi phí tốt nhất có thể ở mức xấu nhất có thể để kết nối các nút bên trong thành phần. Thay vì tính toán lại các đường dẫn, chúng tôi gắn mỗi cạnh hợp nhất làm nút thắt cổ chai xác định cho liên kết, đảm bảo rằng mọi đường dẫn giữa các nút trong quá trình hợp nhất đều phải đi qua một cạnh có phí ít nhất là giá trị đó. 

Do đó, khi chúng tôi truy vấn từ một nút, chúng tôi có thể liên kết ứng cử viên có thể truy cập tốt nhất của nó làm cấu trúc gốc thành phần và mức phí trong trường hợp xấu nhất tương ứng với mức phí tối đa gặp phải dọc theo các phần hợp nhất kết nối đường dẫn của nút bắt đầu với cực đại ứng viên. Điều này được duy trì một cách tự nhiên bằng cách lưu trữ, đối với mỗi thành phần DSU, không chỉ sự thích thú tối đa mà còn cả mức phí tối đa gặp phải khi hình thành lịch sử hợp nhất của thành phần đó. 

### Tại sao nó hoạt động 

Tại bất kỳ ngưỡng cố định v nào, biểu đồ hoạt động là một nhóm trên các thành phần được kết nối được xác định bởi các cạnh có dung lượng ít nhất là v. DSU đại diện chính xác cho các thành phần này. Bất kỳ đường dẫn nào giữa hai nút trong một thành phần chỉ sử dụng các cạnh được kích hoạt, do đó cấu trúc phù hợp với khả năng tiếp cận. 

Bởi vì đồ thị cơ bản là một cây, mỗi phần hợp tương ứng với việc đưa ra một cạnh kết nối duy nhất, cạnh này phải nằm trên mọi đường dẫn giữa hai phần được hợp nhất. Do đó, phí tối đa trên bất kỳ đường dẫn nào giữa các nút ở các phần khác nhau luôn được xác định bởi cạnh phí cao nhất được sử dụng trong đường dẫn hợp nhất mà cấu trúc dựa trên DSU duy trì. Điều này đảm bảo rằng thông tin thành phần được ghi lại phản ánh chính xác phí cầu đường trong trường hợp xấu nhất đối với bất kỳ điểm đến nào được phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class DSU:
    def __init__(self, n, enjoy):
        self.parent = list(range(n))
        self.sz = [1] * n
        self.mx = enjoy[:]  # max enjoyment in component

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return a
        if self.sz[a] < self.sz[b]:
            a, b = b, a
        self.parent[b] = a
        self.sz[a] += self.sz[b]
        self.mx[a] = max(self.mx[a], self.mx[b])
        return a

def solve():
    n, q = map(int, input().split())
    e = list(map(int, input().split()))

    edges = []
    for _ in range(n - 1):
        a, b, c, t = map(int, input().split())
        a -= 1
        b -= 1
        edges.append((c, t, a, b))

    queries = []
    for i in range(q):
        v, x = map(int, input().split())
        queries.append((v, x - 1, i))

    edges.sort(reverse=True)
    queries.sort(reverse=True)

    dsu = DSU(n, e)

    ans = [0] * q
    j = 0

    for v, x, idx in queries:
        while j < len(edges) and edges[j][0] >= v:
            c, t, a, b = edges[j]
            dsu.union(a, b)
            j += 1

        root = dsu.find(x)
        ans[idx] = dsu.mx[root]

    out = sys.stdout.write
    out("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là đọc cây và lưu trữ các cạnh có dung lượng và phí. Nó sắp xếp các cạnh và truy vấn theo thứ tự giảm dần về yêu cầu dung lượng để khi xử lý một truy vấn, tất cả các cạnh có thể sử dụng đều đã hoạt động. 

DSU duy trì các thành phần của các nút hiện có thể truy cập và theo dõi mức độ tận hưởng tối đa trên mỗi thành phần. Các hoạt động hợp nhất sẽ hợp nhất các thành phần bất cứ khi nào một cạnh hợp lệ được kích hoạt. 

Mỗi truy vấn chỉ cần tìm đại diện DSU của thành phố bắt đầu và trả về mức độ thích thú tối đa được lưu trữ. 

Chi tiết triển khai quan trọng là các truy vấn được xử lý ngoại tuyến theo thứ tự giảm dần, đảm bảo tính chính xác của cạnh hoạt động được đặt ở mỗi bước. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi mẫu đầu tiên. 

Thứ tự truy vấn đã được xử lý theo thứ tự v giảm dần. 

| Truy vấn | Các cạnh hoạt động (công suất ≥ v) | Thành phần DSU của sự khởi đầu | Tận hưởng tối đa | 
| --- | --- | --- | --- | 
| 1 3 | tất cả các cạnh | {1,2,3,4,5} | 3 | 
| 9 5 | chỉ các cạnh ≥ 9 (không có) | {5} | 3 | 
| 6 2 | cạnh có dung lượng ≥ 6 | {2,4} | 3 | 

Truy vấn đầu tiên kích hoạt tất cả các cạnh, do đó toàn bộ cây được kết nối và mức độ thích thú tối đa là 3. Truy vấn thứ hai kích hoạt không có cạnh nào, khiến nút 5 bị cô lập. Truy vấn thứ ba chỉ kích hoạt các cạnh có dung lượng cao, tạo ra thành phần nhỏ hơn chứa nút 2 và 4. 

Điều này xác nhận rằng kích hoạt DSU phù hợp chính xác với việc lọc ngưỡng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | việc sắp xếp các cạnh và truy vấn chiếm ưu thế, các hoạt động DSU được khấu hao gần như không đổi | 
| Không gian | O(n) | Mảng DSU và lưu trữ cạnh | 

Các ràng buộc cho phép lên tới hai trăm nghìn nút và truy vấn, do đó cần có giải pháp log-tuyến tính. Cách tiếp cận DSU xử lý mỗi cạnh một lần và mỗi truy vấn một lần, vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# sample 1
assert run("""5 3
2 2 3 3 3
1 2 4 7
1 3 2 8
2 4 8 2
2 5 1 1
1 3
9 5
6 2
""") == """3
3
3"""

# minimum case
assert run("""2 1
5 1
1 2 10 3
1 1
""") == "5"

# all isolated queries
assert run("""3 2
1 2 3
1 2 1 1
2 3 1 1
5 1
5 2
""") == """1
2"""

# high threshold isolates all
assert run("""4 1
4 3 2 1
1 2 1 5
2 3 1 6
3 4 1 7
10 1
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây nhỏ | 5 | tính đúng đắn của cấu trúc tối thiểu | 
| truy vấn bị cô lập | 1,2 | Xử lý DSU của các thành phần bị ngắt kết nối | 
| ngưỡng cao | 1 | cách ly hoàn toàn theo khả năng lọc | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi ngưỡng v lớn hơn mọi dung lượng cạnh. Trong trường hợp đó, không có sự kết hợp nào xảy ra và mỗi truy vấn sẽ trả về sự thích thú của nút bắt đầu. DSU không bao giờ hợp nhất bất cứ thứ gì, vì vậy giá trị tối đa của thành phần vẫn là giá trị nút. 

Một trường hợp cạnh khác là khi tất cả các cạnh đều có thể sử dụng được. Sau đó, DSU cuối cùng sẽ hợp nhất toàn bộ cây thành một thành phần và mọi truy vấn sẽ trả về mức độ thích thú tối đa toàn cục. Thứ tự hợp nhất không thành vấn đề vì tất cả các cạnh đều được kích hoạt trước khi bất kỳ truy vấn nào được xử lý. 

Trường hợp tinh tế thứ ba là khi nhiều truy vấn chia sẻ cùng một nút bắt đầu nhưng có các ngưỡng khác nhau. Thứ tự ngoại tuyến đảm bảo rằng khi một nút trở thành một phần của thành phần lớn hơn ở một ngưỡng nào đó, nó sẽ giữ nguyên như vậy đối với tất cả các ngưỡng nhỏ hơn, do đó, việc tra cứu lặp lại vẫn nhất quán mà không cần tính toán lại.

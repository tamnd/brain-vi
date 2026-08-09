---
title: "CF 104009D - Lưỡng bên"
description: "Chúng ta được cho một chuỗi các cạnh của đồ thị vô hướng, được trình bày theo một thứ tự cố định. Chúng ta không được phép sắp xếp lại các cạnh nhưng có thể cắt dãy này thành các khối liên tiếp. Mỗi khối tạo thành biểu đồ độc lập của riêng nó bằng cách sử dụng chính xác các cạnh bên trong nó."
date: "2026-07-02T05:24:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104009
codeforces_index: "D"
codeforces_contest_name: "AGM 2022, Final Round, Day 1"
rating: 0
weight: 104009
solve_time_s: 49
verified: true
draft: false
---

[CF 104009D - Bipartite](https://codeforces.com/problemset/problem/104009/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các cạnh của đồ thị vô hướng, được trình bày theo một thứ tự cố định. Chúng ta không được phép sắp xếp lại các cạnh nhưng có thể cắt dãy này thành các khối liên tiếp. Mỗi khối tạo thành biểu đồ độc lập của riêng nó bằng cách sử dụng chính xác các cạnh bên trong nó. 

Đối với mỗi khối, chúng tôi kiểm tra xem biểu đồ được tạo bởi các cạnh đó có phải là lưỡng cực hay không. Một phân vùng hợp lệ là phân vùng trong đó mỗi khối là hai phần và chúng tôi muốn chia danh sách cạnh ban đầu thành số lượng khối như vậy nhỏ nhất có thể. 

Vì vậy, nhiệm vụ về cơ bản là phân đoạn trực tuyến của luồng cạnh: chúng tôi quét các cạnh từ trái sang phải và bất cứ khi nào khối hiện tại không còn là lưỡng cực, chúng tôi sẽ cắt nó và bắt đầu một khối mới. 

Hạn chế chính là cả số lượng nút và cạnh đều có thể lên tới hai trăm nghìn, do đó, bất kỳ phương pháp nào tính toán lại tính lưỡng cực từ đầu cho mọi phân đoạn có thể đều quá chậm. Một nỗ lực O(M²) ngây thơ đối với tất cả các điểm cuối của phân đoạn sẽ liên quan đến việc kiểm tra tính lưỡng cực nhiều lần trên các đồ thị con lớn, vượt xa giới hạn. 

Một trường hợp lỗi tinh vi xuất hiện khi một biểu đồ trở thành không lưỡng cực do một chu trình lẻ trải dài trên nhiều cạnh. Ví dụ: các cạnh (1,2), (2,3), (3,1) tạo thành một hình tam giác. Nếu một cách tiếp cận ngây thơ làm trì hoãn việc phát hiện và cố gắng “sửa chữa” nó sau đó, thì nó có thể hợp nhất các cạnh thành một phân đoạn một cách không chính xác mặc dù điều kiện lưỡng cực đã bị vi phạm tại thời điểm cạnh thứ ba được thêm vào. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là duy trì phân khúc hiện tại và sau mỗi cạnh mới, xây dựng lại biểu đồ và chạy kiểm tra lưỡng cực bằng BFS hoặc DFS. Điều này hoạt động một cách hợp lý, bởi vì tính lưỡng cực rất dễ được xác minh bằng cách tô màu hai bên. Tuy nhiên, nếu chúng ta thực hiện điều này cho mọi cạnh thì tổng chi phí sẽ trở thành O(M × (N + M)) trong trường hợp xấu nhất, vì mỗi BFS có thể đi qua gần như toàn bộ phân đoạn và chúng ta lặp lại nó M lần. Với M lên tới 200000 thì điều này là không khả thi. 

Nhận xét quan trọng là chúng ta không bao giờ cần phải xem xét lại các quyết định trong quá khứ một khi đã cắt một đoạn. Bên trong một đoạn, chúng ta chỉ cần biết liệu việc thêm một cạnh mới có tạo ra mâu thuẫn trong cách tô màu hai bên hay không. Đây chính xác là mục đích của cấu trúc Disjoint Set Union với tính chẵn lẻ (còn được gọi là DSU với theo dõi lưỡng cực). 

Chúng tôi duy trì một DSU trong đó mỗi nút được chia thành hai trạng thái đại diện cho màu của nó. Mỗi phép toán hợp bắt buộc rằng các điểm cuối của một cạnh phải có màu đối lập nhau. Nếu tại bất kỳ thời điểm nào điều kiện này mâu thuẫn với các phép gán trước đó thì phân đoạn hiện tại trở nên không hợp lệ, vì vậy chúng ta phải cắt ở đây. 

Vì vậy, chúng tôi tham lam mở rộng phân khúc hiện tại càng xa càng tốt trong khi nó vẫn là lưỡng đảng. Khi xảy ra mâu thuẫn, chúng tôi bắt đầu một phân đoạn mới và đặt lại DSU. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại BFS trên mỗi phân đoạn | O(M²) | O(N) | Quá chậm | 
| DSU với tính chẵn lẻ + phân đoạn tham lam | O(M α(N)) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các cạnh từ trái sang phải trong khi duy trì DSU theo dõi các ràng buộc lưỡng cực bên trong phân đoạn hiện tại. 

1. Chúng tôi khởi tạo một DSU mới cho phân đoạn hiện tại, trong đó mỗi nút ban đầu nằm trong tập hợp riêng của nó mà không có ràng buộc chẵn lẻ. 
2. Đối với mỗi cạnh (u, v), chúng ta cố gắng hợp nhất u và v với tính chẵn lẻ đối diện. Điều này buộc u và v phải thuộc các lớp màu khác nhau trong màu lưỡng cực của phân đoạn hiện tại. 
3. Nếu u và v đã thuộc cùng một thành phần DSU với cùng yêu cầu chẵn lẻ, thì việc thêm cạnh này sẽ tạo ra một chu kỳ lẻ. Tại thời điểm này, phân đoạn hiện tại không còn là phân đoạn lưỡng cực nữa, vì vậy chúng tôi hoàn thiện phân đoạn kết thúc ở cạnh trước đó. 
4. Khi một phân đoạn kết thúc, chúng tôi ghi lại nó, xóa trạng thái DSU và bắt đầu một phân đoạn mới bắt đầu ở cạnh hiện tại. 
5. Sau khi xử lý tất cả các cạnh, chúng ta xuất ra số đoạn.

Phần không tầm thường là cách lưu trữ tính chẵn lẻ. Mỗi nút DSU giữ một con trỏ cha và một giá trị chẵn lẻ, nghĩa là màu của nút đó có khác với màu của nút cha hay không. Nén đường dẫn cập nhật cả cấu trúc và tính chẵn lẻ một cách nhất quán, đảm bảo chúng ta luôn có thể tính toán xem hai nút phải có màu bằng nhau hay đối diện nhau. 

### Tại sao nó hoạt động 

Bên trong mỗi phân đoạn, chúng tôi đang xây dựng màu lưỡng cực một phần một cách hiệu quả khi các cạnh xuất hiện. Bất biến DSU là đối với mọi cạnh được xử lý, các ràng buộc được thực thi là nhất quán và không tồn tại chu kỳ lẻ. Nếu mâu thuẫn xuất hiện, nó tương ứng chính xác với việc phát hiện ra một chu trình lẻ trong đoạn đó, đó là lý do duy nhất khiến đồ thị không thể lưỡng cực. Vì chúng tôi cắt ngay lập tức khi điều này xảy ra nên mọi phân khúc được sản xuất đều được đảm bảo duy trì tính lưỡng cực và tính tối đa của từng phân khúc sẽ tuân theo vì chúng tôi chỉ cắt khi không thể mở rộng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
        self.parity = [0] * n  # parity to parent

    def find(self, x):
        if self.parent[x] == x:
            return x
        px = self.parent[x]
        root = self.find(px)
        self.parity[x] ^= self.parity[px]
        self.parent[x] = root
        return root

    def get_parity(self, x):
        self.find(x)
        return self.parity[x]

    def union(self, a, b):
        ra = self.find(a)
        rb = self.find(b)

        pa = self.get_parity(a)
        pb = self.get_parity(b)

        if ra == rb:
            return (pa ^ pb) == 1

        if self.rank[ra] < self.rank[rb]:
            ra, rb = rb, ra
            a, b = b, a
            pa, pb = pb, pa

        self.parent[rb] = ra
        self.parity[rb] = pa ^ pb ^ 1

        if self.rank[ra] == self.rank[rb]:
            self.rank[ra] += 1

        return True

def solve():
    n, m = map(int, input().split())
    edges = [tuple(map(int, input().split())) for _ in range(m)]

    dsu = DSU(n + 1)
    res = 1

    for u, v in edges:
        if not dsu.union(u, v):
            res += 1
            dsu = DSU(n + 1)
            dsu.union(u, v)

    print(res)

if __name__ == "__main__":
    solve()
```DSU lưu trữ thông tin chẵn lẻ liên quan đến các liên kết gốc. Hàm hợp trả về việc thêm cạnh có còn hiệu lực hay không. Nếu nó trả về Sai, điều đó có nghĩa là đã tìm thấy mâu thuẫn và chúng tôi ngay lập tức bắt đầu một phân đoạn mới. 

Một chi tiết tinh tế là việc đặt lại hoàn toàn DSU khi chúng tôi cắt. Vì các phân đoạn là các biểu đồ độc lập nên không có thông tin nào được truyền tải giữa chúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
1 3
1 2
2 3
```Chúng tôi xử lý các cạnh một cách tuần tự. 

| Bước | Cạnh | Bang DSU hợp lệ? | Phân đoạn | 
| --- | --- | --- | --- | 
| 1 | (1,3) | Có | [1] | 
| 2 | (1,2) | Có | [1,2] | 
| 3 | (2,3) | Không | cắt trước cái này | 

Vì vậy, chúng tôi nhận được hai phân đoạn: [1,2] và [3]. 

Điều này cho thấy tam giác buộc phải phân chia chính xác như thế nào khi cạnh thứ ba được xử lý. 

### Ví dụ 2 

đầu vào:```
4 4
1 2
2 3
3 4
4 1
```| Bước | Cạnh | Bang DSU hợp lệ? | Phân đoạn | 
| --- | --- | --- | --- | 
| 1 | (1,2) | Có | [1] | 
| 2 | (2,3) | Có | [1,2] | 
| 3 | (3,4) | Có | [1,2,3] | 
| 4 | (4,1) | Không | cắt | 

Cạnh cuối cùng đóng một chu kỳ lẻ trên cấu trúc hiện tại, tạo thành đoạn thứ hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M α(N)) | Mỗi cạnh kích hoạt tối đa một số thao tác DSU với tính năng nén đường dẫn | 
| Không gian | O(N) | Mảng DSU cho cấp độ gốc, cấp bậc, chẵn lẻ | 

Các ràng buộc cho phép lên tới 200000 cạnh và giải pháp dựa trên DSU xử lý từng cạnh trong thời gian khấu hao gần như không đổi, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    class DSU:
        def __init__(self, n):
            self.parent = list(range(n))
            self.rank = [0]*n
            self.parity = [0]*n

        def find(self, x):
            if self.parent[x] == x:
                return x
            px = self.parent[x]
            r = self.find(px)
            self.parity[x] ^= self.parity[px]
            self.parent[x] = r
            return r

        def get_parity(self, x):
            self.find(x)
            return self.parity[x]

        def union(self, a, b):
            ra, rb = self.find(a), self.find(b)
            pa, pb = self.get_parity(a), self.get_parity(b)
            if ra == rb:
                return (pa ^ pb) == 1
            if self.rank[ra] < self.rank[rb]:
                ra, rb = rb, ra
                pa, pb = pb, pa
            self.parent[rb] = ra
            self.parity[rb] = pa ^ pb ^ 1
            if self.rank[ra] == self.rank[rb]:
                self.rank[ra] += 1
            return True

    n, m = map(int, inp.splitlines()[0].split())
    edges = [tuple(map(int, x.split())) for x in inp.splitlines()[1:]]

    dsu = DSU(n+1)
    ans = 1

    for u, v in edges:
        if not dsu.union(u, v):
            ans += 1
            dsu = DSU(n+1)
            dsu.union(u, v)

    return str(ans)

# sample
assert run("3 3\n1 3\n1 2\n2 3\n") == "2"

# all independent edges
assert run("4 3\n1 2\n3 4\n1 3\n") == "1"

# triangle forces split
assert run("3 3\n1 2\n2 3\n3 1\n") == "2"

# chain remains bipartite
assert run("5 4\n1 2\n2 3\n3 4\n4 5\n") == "1"

# repeated cycle pattern
assert run("4 5\n1 2\n2 3\n3 4\n4 1\n1 3\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tam giác | 2 | phát hiện chu kỳ lẻ đầu tiên | 
| chuỗi | 1 | phân khúc lưỡng cực hoàn toàn | 
| cạnh hỗn hợp | 1 | thành phần độc lập | 
| hợp âm bổ sung | 2 | xử lý vi phạm muộn | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi cạnh đầu tiên đã tạo ra mâu thuẫn ở trạng thái DSU hiện tại. Trong tình huống đó, thuật toán ngay lập tức bắt đầu một phân đoạn mới chỉ chứa cạnh đó và DSU được đặt lại đảm bảo không có thông tin chẵn lẻ cũ nào bị rò rỉ về phía trước. 

Một trường hợp khác là đồ thị có nhiều chu kỳ lẻ chồng lên nhau. Ví dụ: cấu trúc dày đặc giống như hình tam giác có thể kích hoạt các đoạn cắt lặp đi lặp lại. Mỗi lần cắt sẽ đặt lại tất cả các ràng buộc, do đó, ngay cả khi các cạnh trước đó hình thành các xung đột phức tạp thì chỉ phân đoạn hoạt động hiện tại mới quan trọng. 

Trường hợp tinh tế cuối cùng là một chuỗi dài trong đó cạnh cuối cùng đóng một chu kỳ. Thuật toán trì hoãn lỗi một cách chính xác cho đến khi cạnh chính xác hoàn thành mâu thuẫn, bởi vì tính chẵn lẻ DSU chỉ phát hiện sự không nhất quán khi yêu cầu tính chẵn lẻ đối lập của cùng một thành phần bị vi phạm.

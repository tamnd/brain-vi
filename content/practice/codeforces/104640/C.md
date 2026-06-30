---
title: "CF 104640C - \u041f\u0440\u044b\u0436\u043a\u0438 \u043c\u0435\u0436\u0434\u0443 \u0432\u0441\u0435\u043b\u0435\u043d\u043d\u044b\u043c\u0438"
description: "Chúng ta được cung cấp một biểu đồ trong đó các đỉnh biểu thị các vũ trụ và các cạnh biểu thị các cổng. Mỗi cổng có yêu cầu năng lượng tối thiểu, nghĩa là bạn chỉ có thể đi qua cổng đó nếu mức năng lượng hiện tại của bạn ít nhất là một ngưỡng nhất định. Năng lượng bắt đầu từ con số không."
date: "2026-06-29T16:49:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104640
codeforces_index: "C"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2023-2024, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 104640
solve_time_s: 89
verified: true
draft: false
---

[CF 104640C - \u041f\u0440\u044b\u0436\u043a\u0438 \u043c\u0435\u0436\u0434\u0443 \u0432\u0441\u0435\u043b\u0435\u043d\u043d\u044b\u043c\u0438](https://codeforces.com/problemset/problem/104640/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ trong đó các đỉnh biểu thị các vũ trụ và các cạnh biểu thị các cổng. Mỗi cổng có yêu cầu năng lượng tối thiểu, nghĩa là bạn chỉ có thể đi qua cổng đó nếu mức năng lượng hiện tại của bạn ít nhất là một ngưỡng nhất định. Năng lượng bắt đầu từ con số không. 

Bất cứ khi nào bạn bước vào vũ trụ lần đầu tiên, năng lượng của bạn sẽ tăng vĩnh viễn theo giá trị được gán cho vũ trụ đó. Bạn bắt đầu từ một vũ trụ cố định và từ đó bạn có thể du hành dọc theo bất kỳ chuỗi cổng nào có thể sử dụng được, tăng dần năng lượng khi bạn khám phá các vũ trụ mới. 

Nhiệm vụ là xác định tổng năng lượng tối đa mà bạn có thể tích lũy khi kết thúc quá trình này, tương đương với tổng giá trị của tất cả các vũ trụ có thể tiếp cận được dưới những ràng buộc này. 

Kích thước ràng buộc ngay lập tức loại trừ mọi giải pháp mô phỏng lặp đi lặp lại tất cả các đường dẫn có thể. Với tối đa một trăm nghìn vũ trụ và hai trăm nghìn cổng, bất kỳ cách tiếp cận nào khám phá các trạng thái có dạng “nút hiện tại cộng với tập hợp đã truy cập” đều không khả thi. Ngay cả thuật toán kiểu đường dẫn ngắn nhất trên các trạng thái mở rộng cũng sẽ bùng nổ về mặt tổ hợp vì năng lượng phụ thuộc vào thứ tự các nút được truy cập. 

Một khó khăn tinh tế đến từ sự phụ thuộc vòng tròn. Một cổng có thể sớm không sử dụng được vì yêu cầu của nó quá cao, nhưng một khi chúng ta thu thập đủ năng lượng từ các vũ trụ khác, nó sẽ có thể sử dụng được và có thể mở khóa nhiều vũ trụ hơn. Điều này có nghĩa là khả năng tiếp cận không cố định. 

Một cạm bẫy phổ biến là coi đây là một vấn đề về khả năng tiếp cận tiêu chuẩn và chỉ cần bỏ qua các trọng số hoặc chạy Dijkstra trên các nút có trọng số cạnh. Điều đó không thành công vì ràng buộc không mang tính cộng tính dọc theo các cạnh, nó là ngưỡng phát triển toàn cầu. 

Một trường hợp lỗi khác xuất hiện khi một nút cung cấp năng lượng lớn chỉ có thể truy cập được thông qua cạnh ngưỡng cao. Một phép duyệt tham lam ngây thơ không bao giờ xem lại các cạnh bị chặn sẽ bỏ lỡ các nút như vậy mặc dù chúng rất quan trọng để mở khóa phần còn lại của biểu đồ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là mô phỏng tất cả các cách có thể để ghé thăm các vũ trụ. Bắt đầu từ vũ trụ ban đầu, ở mỗi bước, chúng tôi thử mọi cổng thông tin hiện có thể truy cập, di chuyển qua nó nếu được phép và tiếp tục đệ quy. Mỗi trạng thái sẽ cần phải nhớ những vũ trụ nào đã được viếng thăm và năng lượng hiện tại. Yếu tố phân nhánh phát triển nhanh chóng vì mỗi vũ trụ mới vừa thay đổi năng lượng vừa thay đổi khả năng tiếp cận trong tương lai. Trong trường hợp xấu nhất, điều này dẫn đến việc thăm dò theo cấp số nhân trên các tập hợp con của nút. 

Quan sát quan trọng là điều duy nhất quan trọng để mở khóa các cạnh là tổng năng lượng hiện tại, chính xác là tổng giá trị của các nút đã truy cập. Khi một nút có thể truy cập được, việc truy cập nút đó chỉ làm tăng năng lượng, điều này chỉ có thể mở khóa nhiều cạnh hơn. Không có cơ chế làm giảm tính linh hoạt. 

Cấu trúc đơn điệu này gợi ý một quy trình tập hợp ngày càng có thể tiếp cận được. Nếu chúng ta biết tập hợp cuối cùng của các nút có thể truy cập được thì tổng tổng của chúng sẽ xác định năng lượng cuối cùng và chính năng lượng đó sẽ xác định những cạnh nào lẽ ra có thể sử dụng được. Sự phụ thuộc vòng tròn này có thể được giải quyết bằng cách liên tục mở rộng những gì có thể tiếp cận được bằng cách sử dụng ước tính năng lượng hiện tại. 

Chúng ta có thể duy trì tập hợp các cạnh có yêu cầu đã được thỏa mãn và hợp nhất các điểm cuối của chúng thành các thành phần được kết nối. Khi một thành phần chứa nút bắt đầu trở nên đủ lớn, tổng giá trị nút của nó sẽ tăng năng lượng, có khả năng mở khóa nhiều cạnh hơn, điều này có thể hợp nhất nhiều thành phần hơn vào thành phần bắt đầu. Điều này tạo ra một quá trình điểm cố định tự nhiên.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng trạng thái đầy đủ | Hàm mũ | Hàm mũ | Quá chậm | 
| Tuyên truyền ngưỡng với DSU + kích hoạt lặp lại | O((n + m) log m) hoặc O((n + m) α(n)) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cấu trúc liên kết rời rạc trên các vũ trụ và chúng tôi dần dần kích hoạt các cổng khi năng lượng tăng lên. Mỗi lần kích hoạt có thể hợp nhất các thành phần và chỉ thành phần chứa vũ trụ ban đầu mới góp phần vào sự tăng trưởng năng lượng trong tương lai. 

1. Khởi tạo một liên kết tập hợp rời rạc trên tất cả các vũ trụ. Mỗi vũ trụ bắt đầu như một thành phần riêng của nó và mỗi thành phần lưu trữ tổng giá trị nút của nó. Vũ trụ ban đầu được coi là đã được viếng thăm ngay lập tức, do đó năng lượng ban đầu bằng giá trị của nó. 
2. Sắp xếp tất cả các cổng theo năng lượng cần thiết theo thứ tự tăng dần. Điều này cho phép chúng ta kích hoạt chúng dần dần khi năng lượng đủ lớn. 
3. Giữ con trỏ trên các cổng được sắp xếp. Kích hoạt liên tục tất cả các cổng có yêu cầu tối đa là năng lượng hiện tại. Kích hoạt có nghĩa là thực hiện các hoạt động kết hợp trong DSU. 
4. Sau khi kích hoạt tất cả các cổng hiện có thể sử dụng được, hãy tìm thành phần chứa vũ trụ ban đầu. Tính tổng số tiền của nó, đại diện cho năng lượng tốt nhất có thể đạt được với cấu trúc hiện đã được mở khóa. 
5. Nếu năng lượng mới này lớn hơn giá trị trước đó, hãy cập nhật năng lượng và lặp lại quy trình, vì năng lượng cao hơn có thể mở khóa các cổng bổ sung đã bị chặn trước đó. 
6. Dừng khi việc duyệt toàn bộ danh sách cổng thông tin không kích hoạt bất kỳ cổng mới nào hoặc không tăng kích thước thành phần có thể truy cập. 

Ý tưởng chính là mỗi khi năng lượng tăng lên, chúng tôi sẽ mở rộng tập hợp các cạnh có thể sử dụng và mỗi lần mở rộng như vậy chỉ có thể tăng khả năng kết nối của thành phần khởi đầu chứ không bao giờ giảm nó. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, DSU được xây dựng từ tất cả các cạnh được kích hoạt thể hiện chính xác khả năng kết nối theo ràng buộc mà tất cả các cổng được sử dụng đều yêu cầu tối đa năng lượng hiện tại. Thành phần của nút bắt đầu trong cấu trúc này đại diện cho tất cả các vũ trụ thực sự có thể tiếp cận được ở mức năng lượng đó. Vì năng lượng được định nghĩa là tổng giá trị của các vũ trụ đã ghé thăm nên việc tính toán lại nó từ thành phần này phù hợp với quá trình ghé thăm từng vũ trụ một lần khi nó có thể tiếp cận được. Vì cả năng lượng và tập hợp các cạnh được kích hoạt chỉ tăng theo thời gian nên quá trình phải hội tụ đến một điểm cố định nơi không có cạnh mới nào có thể được kích hoạt và không có nút mới nào có thể tham gia thành phần bắt đầu. Điểm cố định đó là năng lượng tối đa có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n, val):
        self.parent = list(range(n))
        self.size = [1] * n
        self.sum = val[:]  # component sums

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return False
        if self.size[a] < self.size[b]:
            a, b = b, a
        self.parent[b] = a
        self.size[a] += self.size[b]
        self.sum[a] += self.sum[b]
        return True

def solve():
    n, m, s = map(int, input().split())
    s -= 1

    a = list(map(int, input().split()))

    edges = []
    for _ in range(m):
        u, v, w = map(int, input().split())
        edges.append((w, u - 1, v - 1))

    edges.sort()

    dsu = DSU(n, a)

    energy = a[s]
    prev_energy = -1

    i = 0
    while energy != prev_energy:
        prev_energy = energy

        while i < m and edges[i][0] <= energy:
            _, u, v = edges[i]
            dsu.union(u, v)
            i += 1

        root = dsu.find(s)
        energy = dsu.sum[root]

    print(energy)

if __name__ == "__main__":
    solve()
```DSU lưu trữ cả kết nối và tổng giá trị bên trong mỗi thành phần, điều này tránh tính toán lại tổng sau mỗi lần hợp nhất. Con trỏ`i`đảm bảo mỗi cạnh chỉ được xử lý một lần, vì khi ngưỡng của nó được thỏa mãn, nó sẽ duy trì hoạt động mãi mãi. 

Vòng ngoài rất cần thiết vì các cạnh kích hoạt có thể phóng to thành phần khởi động, làm tăng năng lượng, có thể mở khóa nhiều cạnh hơn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 4 1
1 1 1 1 1
1 2 2
1 3 1
1 4 3
1 5 5
```Chúng ta bắt đầu với năng lượng 1 từ vũ trụ 1. 

| Bước | Năng lượng | Các cạnh được kích hoạt (w ≤ năng lượng) | Bắt đầu thành phần | Năng lượng mới | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | (1-3) | {1, 3} | 2 | 
| 2 | 2 | (1-2) | {1, 2, 3} | 3 | 
| 3 | 3 | (1-4) | {1, 2, 3, 4} | 4 | 
| 4 | 4 | không có gì mới | không thay đổi | 4 | 

Khi năng lượng đạt tới 4, cạnh 5 vẫn cần 5 nên nó không bao giờ được kích hoạt. Quá trình ổn định ở 4. 

Điều này chứng tỏ sự tăng trưởng năng lượng và kích hoạt cạnh xen kẽ nhau cho đến khi không có ngưỡng mới nào được thỏa mãn. 

### Mẫu 2 

đầu vào:```
4 3 1
3 2 1 10
1 2 3
2 3 5
1 3 4
```Năng lượng ban đầu là 3. 

| Bước | Năng lượng | Các cạnh được kích hoạt (w ≤ năng lượng) | Bắt đầu thành phần | Năng lượng mới | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | (1-2) | {1, 2} | 5 | 
| 2 | 5 | (1-3), (2-3) | {1, 2, 3} | 6 | 
| 3 | 6 | không có gì mới | không thay đổi | 6 | 

Nút có giá trị 10 không thể truy cập được vì không có đường dẫn nào thỏa mãn điều kiện kích hoạt cần thiết trước khi nó bị cô lập sau các ngưỡng cao hơn. 

Điều này cho thấy kết nối trung gian quan trọng hơn giá trị nút thô như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) α(n)) | Mỗi cạnh được xử lý một lần, mỗi liên kết/tìm gần như được khấu hao gần như không đổi | 
| Không gian | O(n + m) | Mảng DSU cộng với lưu trữ cạnh | 

Thuật toán phù hợp thoải mái trong giới hạn vì mỗi cổng được kích hoạt tối đa một lần và mỗi lần kích hoạt chỉ kích hoạt các hoạt động DSU gần như không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve() if solve() is not None else "").strip()

# provided samples
assert run("""5 4 1
1 1 1 1 1
1 2 2
1 3 1
1 4 3
1 5 5
""") == "4"

assert run("""4 3 1
3 2 1 10
1 2 3
2 3 5
1 3 4
""") == "6"

# minimal case
assert run("""1 0 1
7
""") == "7"

# disconnected graph
assert run("""3 0 1
5 6 7
""") == "5"

# chain unlocking
assert run("""4 3 1
1 1 1 1
1 2 0
2 3 0
3 4 0
""") == "4"

# high threshold blocking
assert run("""3 2 1
10 1 1
1 2 100
2 3 100
""") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút | 7 | khởi tạo một nút | 
| không có cạnh | 5 | xử lý ngắt kết nối | 
| xích không trọng lượng | 4 | tuyên truyền đầy đủ | 
| ngưỡng cao | 10 | mở rộng bị chặn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các cạnh có ngưỡng lớn hơn năng lượng ban đầu. Trong tình huống đó, thuật toán thực hiện chính xác không có sự kết hợp nào ngoài nút bắt đầu và câu trả lời vẫn chỉ là giá trị của vũ trụ ban đầu. 

Một trường hợp cạnh khác xảy ra khi một cạnh có ngưỡng thấp duy nhất kết nối với một chuỗi lớn các nút có giá trị cao hơn. Thuật toán chỉ mở khóa chuỗi sau khi năng lượng tăng đủ và vòng lặp đảm bảo rằng sau khi kết nối đầu tiên được thực hiện, các lần mở rộng tiếp theo sẽ được kích hoạt một cách tự nhiên mà không cần truy cập lại logic trước đó theo cách thủ công. 

Trường hợp tinh tế cuối cùng là khi các thành phần hợp nhất gián tiếp thông qua các cạnh sẽ hoạt động muộn hơn nhiều. DSU xử lý việc này một cách rõ ràng vì khi một cạnh được kích hoạt, tác dụng của nó là vĩnh viễn và việc tăng năng lượng sau này không yêu cầu xử lý lại các cạnh cũ hoặc đưa ra quyết định đảo ngược.

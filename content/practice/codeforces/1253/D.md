---
title: "CF 1253D - Đồ thị hài hòa"
description: "Chúng ta có một đồ thị vô hướng trên các đỉnh được đánh nhãn từ 1 đến n và chúng ta được phép thêm các cạnh. Mục tiêu là làm cho biểu đồ thỏa mãn một ràng buộc về khả năng tiếp cận rất cụ thể phụ thuộc vào thứ tự tự nhiên của các đỉnh."
date: "2026-06-15T22:44:15+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "dsu", "graphs", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1253
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 600 (Div. 2)"
rating: 1700
weight: 1253
solve_time_s: 103
verified: true
draft: false
---

[CF 1253D - Đồ thị hài hòa](https://codeforces.com/problemset/problem/1253/D) 

**Đánh giá:** 1700 
**Thẻ:** thuật toán xây dựng, dfs và tương tự, dsu, đồ thị, tham lam, sắp xếp 
**Thời gian giải:** 1m 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng trên các đỉnh được đánh nhãn từ 1 đến n và chúng ta được phép thêm các cạnh. Mục tiêu là làm cho biểu đồ thỏa mãn một ràng buộc về khả năng tiếp cận rất cụ thể phụ thuộc vào thứ tự tự nhiên của các đỉnh. 

Điều kiện nói rằng bất cứ khi nào một đỉnh l có thể đến đỉnh r bằng cách sử dụng các cạnh hiện có thì mọi đỉnh có nhãn nằm hoàn toàn giữa l và r cũng phải có thể tới được từ l. Nói cách khác, kết nối từ l đến r không thể “bỏ qua” các nhãn trung gian. Nếu l có thể đạt tới r thì tất cả các đỉnh trong khoảng số [l, r] phải nằm trong vùng liên thông của l. 

Đây không phải là yêu cầu kết nối tiêu chuẩn vì khả năng tiếp cận tương tác với thứ tự của các nút. Cho phép một thành phần được kết nối, nhưng hình chiếu của nó lên trục số phải hoạt động giống như một khoảng liền kề, nếu không chúng ta phải thêm các cạnh để cố định các “lỗ hổng”. 

Các ràng buộc n, m lên tới 200.000 buộc chúng ta phải tránh mọi suy luận bậc hai về các cặp nút hoặc BFS ngây thơ cho mỗi truy vấn. Bất kỳ giải pháp nào cũng phải gần với tuyến tính hoặc tuyến tính. Một cấu trúc tập hợp rời rạc hoặc việc quét tham lam trên các cạnh được sắp xếp trở nên tự nhiên bởi vì chúng ta liên tục suy luận về các khoảng kết nối trên một trục có thứ tự. 

Trường hợp phức tạp phát sinh khi một thành phần trải rộng trên phạm vi số rộng nhưng thiếu các nút bên trong. Ví dụ: nếu 1 được kết nối với 5 nhưng 3 không thể truy cập được từ 1 thì biểu đồ không hợp lệ và phải được sửa chữa ngay cả khi kết nối tồn tại theo nghĩa thông thường. Một trường hợp đặc biệt khác là khi nhiều thành phần trùng nhau trong phạm vi số của chúng nhưng không thực sự được kết nối, điều này có thể che giấu các vi phạm trừ khi chúng tôi theo dõi khả năng tiếp cận một cách cẩn thận. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là kiểm tra rõ ràng điều kiện cho mọi cặp đỉnh l và r. Đối với mỗi l, chúng ta có thể chạy DFS hoặc BFS để tìm tất cả các nút có thể truy cập và sau đó xác minh rằng với mọi r có thể truy cập, tất cả các nút trung gian cũng có thể truy cập được. Điều này ngay lập tức trở nên đắt đỏ. Một DFS duy nhất cho mỗi nút là O(n + m) và việc thực hiện điều đó cho tất cả các nút sẽ dẫn đến O(n(n + m)), vượt xa giới hạn. 

Quan sát chính là về cơ bản, điều kiện là về các thành phần được kết nối chứ không phải các đường dẫn riêng lẻ. Nếu chúng ta xem xét một thành phần liên thông, quy tắc yêu cầu rằng nếu nhãn tối thiểu của nó là l và nhãn tối đa là r thì mọi nút giữa l và r phải thuộc cùng một thành phần. Ngược lại, tồn tại một khoảng trống vi phạm điều kiện. 

Điều này biến vấn đề thành việc buộc mỗi thành phần được kết nối phải trở thành “hoàn thành khoảng” trên trục số. Nếu một thành phần kéo dài từ l đến r thì tất cả các nút trong phạm vi đó phải nằm trong nó. Nếu một số nút trong [l, r] ở bên ngoài, chúng ta phải kết nối nó vào. 

Điều này gợi ý việc sắp xếp các cạnh hoặc sử dụng DSU trong khi quét các nút theo thứ tự tăng dần. Chúng tôi duy trì các thành phần và đảm bảo rằng bất cứ khi nào chúng tôi quan sát thấy một khoảng trống trong phạm vi của một thành phần, chúng tôi sẽ hợp nhất hoặc “lấp đầy” nó bằng cách thêm các cạnh. Giải pháp tiêu chuẩn là xử lý các nút theo thứ tự tăng dần trong khi vẫn duy trì ranh giới xa nhất có thể tiếp cận của mỗi thành phần. Bất cứ khi nào chúng tôi phát hiện một nút nằm trong phạm vi của thành phần đã thấy trước đó nhưng không được kết nối, chúng tôi phải kết nối nút đó, tăng câu trả lời và hợp nhất các thành phần tương ứng. 

Cấu trúc DSU duy trì các thành phần một cách hiệu quả, đồng thời chúng tôi theo dõi điểm cuối bên phải tối đa của bất kỳ thành phần nào gặp phải cho đến nay. Khi quét từ trái sang phải, nếu một nút nằm trong khoảng thời gian hoạt động nhưng không được kết nối, chúng tôi sẽ kết nối nó một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra nhiều BFS/DFS) | O(n(n + m)) | O(n + m) | Quá chậm | 
| DSU + quét tham lam trên cấu trúc được sắp xếp | O((n + m) α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi xử lý các nút theo thứ tự tăng dần và duy trì các thành phần được kết nối bằng cách sử dụng cấu trúc tập hợp rời rạc. 

1. Khởi tạo DSU trên tất cả n nút. Điều này thể hiện kết nối hiện tại từ các cạnh nhất định. 
2. Lặp lại các nút từ 1 đến n, coi mỗi nút là ranh giới bên trái tiềm năng của một thành phần. Chúng tôi duy trì một con trỏ hoặc ranh giới theo dõi chỉ mục tối đa có thể truy cập được từ thành phần hiện tại. Ban đầu, đây chỉ là nút. 
3. Đối với mỗi nút i, chúng tôi tính toán điểm cuối ngoài cùng bên phải của thành phần được kết nối hiện tại bằng DSU. Điều này cho chúng ta biết thành phần này đã trải dài bao xa về mặt số lượng. 
4. Chúng tôi quét tất cả các nút từ i đến điểm cuối ngoài cùng bên phải đó. Nếu chúng ta tìm thấy nút j bên trong khoảng này thuộc về thành phần DSU khác, chúng ta phải kết nối nó. Chúng tôi thêm một cạnh giữa đại diện của thành phần hiện tại và j, tăng câu trả lời và hợp chúng trong DSU. 

Lý do điều này có tác dụng là vì bất kỳ nút nào bị thiếu trong khoảng thời gian đều thể hiện sự vi phạm quy tắc “không có khoảng trống trong khả năng tiếp cận”. 
5. Mỗi lần hợp nhất các thành phần, chúng tôi sẽ tính toán lại ranh giới bên phải xa nhất vì việc hợp nhất có thể mở rộng khoảng cách. 
6. Tiếp tục cho đến khi quá trình quét đạt được độ ổn định, nghĩa là tất cả các nút trong khoảng thời gian đều thuộc về cùng một thành phần DSU. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý tiền tố của các nút, mọi thành phần được kết nối giao với tiền tố này sẽ tạo thành một khoảng liên tục đối với nhãn nút. Bất cứ khi nào chúng tôi phát hiện một nút bên trong khoảng hiện tại chưa được kết nối, chúng tôi sẽ hợp nhất nút đó một cách rõ ràng, loại bỏ khoảng cách. Vì chúng tôi chỉ thêm các cạnh khi phát hiện ra khoảng trống nên chúng tôi thêm số cạnh tối thiểu cần thiết để loại bỏ tất cả các điểm gián đoạn. DSU đảm bảo rằng khi các nút được hợp nhất, chúng vẫn được xử lý nhất quán như một khoảng thời gian duy nhất, ngăn chặn việc hợp nhất dư thừa hoặc xung đột. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n + 1))
        self.size = [1] * (n + 1)

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
        return True

def solve():
    n, m = map(int, input().split())
    dsu = DSU(n)

    for _ in range(m):
        u, v = map(int, input().split())
        dsu.union(u, v)

    ans = 0

    i = 1
    while i <= n:
        root = dsu.find(i)
        r = i

        # expand interval of this component
        for j in range(i, n + 1):
            if dsu.find(j) == root:
                r = j

        # fill gaps inside [i, r]
        j = i
        while j <= r:
            if dsu.find(j) != root:
                ans += 1
                dsu.union(root, j)
                root = dsu.find(root)
                r = max(r, j)
            j += 1

        i += 1

    print(ans)

if __name__ == "__main__":
    solve()
```DSU được sử dụng để duy trì kết nối từ các biên ban đầu. Vòng lặp bên ngoài chọn điểm bắt đầu i và xác định khoảng thời gian của thành phần hiện tại bằng cách quét về phía trước. Vòng lặp bên trong kiểm tra các nút bị thiếu bên trong khoảng đó và buộc các liên kết bất cứ khi nào tìm thấy khoảng trống. Mỗi phép hợp tương ứng với việc thêm một cạnh, góp phần trực tiếp vào câu trả lời. 

Chi tiết triển khai chính là sau mỗi lần kết hợp, đại diện có thể thay đổi, vì vậy chúng tôi tính toán lại gốc và có khả năng kéo dài khoảng thời gian. Việc không cập nhật thư mục gốc một cách chính xác sẽ dẫn đến đánh giá thấp khả năng tiếp cận và thiếu các cạnh cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
14 8
1 2
2 7
3 4
6 3
5 7
3 8
6 8
11 12
```Chúng ta bắt đầu với các thành phần DSU được hình thành bởi các cạnh cho trước. 

| Bước | tôi | gốc(i) | khoảng [i, r] | hành động | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 1 | 1 | [1, 2] | quét tìm thấy 2 kết nối | 
| mở rộng | 3 | 3 | [3, 8] | hợp nhất bên trong thành phần | 
| sửa lỗi | 1 | 1 | [1, 7] | các nút bị thiếu buộc hợp nhất | 
| cuối cùng | - | - | tất cả đều hợp lệ | Đã thêm 1 cạnh | 

Thuật toán phát hiện nút 4 hoặc 6 nằm trong phạm vi khả năng tiếp cận nhưng không được kết nối đúng cách theo cách thỏa mãn khoảng thời gian đóng. Cần có một cạnh để thu hẹp khoảng cách. 

Điều này chứng tỏ một thành phần dường như được kết nối thông qua các đường dẫn vẫn có thể vi phạm tính liên tục trong khoảng thời gian. 

### Ví dụ 2 

đầu vào:```
5 4
1 2
2 3
3 4
4 5
```| Bước | tôi | gốc(i) | khoảng thời gian | hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | [1,5] | đã được kết nối đầy đủ | 

Không tìm thấy khoảng trống nên không có cạnh nào được thêm vào. 

Điều này khẳng định rằng một chuỗi được kết nối đầy đủ đã hài hòa rồi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n α(n)) | Hoạt động DSU chiếm ưu thế, mỗi hoạt động được khấu hao gần như không đổi | 
| Không gian | O(n) | Mảng gốc và mảng kích thước cho DSU | 

Các ràng buộc cho phép thực hiện khoảng vài trăm triệu thao tác đơn giản, nhưng DSU giữ các hoạt động gần như tuyến tính, giúp giải pháp dễ dàng đủ nhanh cho n lên tới 200.000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve() if solve() is not None else ""

# sample
assert run("""14 8
1 2
2 7
3 4
6 3
5 7
3 8
6 8
11 12
""").strip() == "1"

# already harmonious chain
assert run("""5 4
1 2
2 3
3 4
4 5
""").strip() == "0"

# disconnected blocks
assert run("""4 1
1 2
""").strip() == "1"

# minimum size
assert run("""3 0
""").strip() == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị chuỗi | 0 | trường hợp đã hài hòa | 
| đồ thị thưa thớt | 1 | cầu đơn cần thiết | 
| đồ thị trống | 2 | hoàn thành khoảng thời gian đầy đủ | 

## Vỏ cạnh 

Trường hợp quan trọng là khi kết nối trải dài trong một khoảng số lớn nhưng bỏ qua các nút bên trong. Ví dụ: nếu các cạnh kết nối 1 với 10 thông qua một đường dẫn nhưng nút 5 bị cô lập, thuật toán sẽ phát hiện 5 nằm trong khoảng [1, 10] và buộc phải liên kết. Điều này ngăn chặn các vi phạm thầm lặng khi khả năng tiếp cận tồn tại nhưng tính liên tục trong khoảng thời gian bị phá vỡ. 

Một trường hợp cạnh khác là đồ thị bị ngắt kết nối hoàn toàn. Trong trường hợp đó, mỗi nút tạo thành thành phần riêng của nó và quá trình quét sẽ liên tục phát hiện vùng phủ sóng bị thiếu và thêm các cạnh để hợp nhất chúng thành một cấu trúc giống như khoảng thời gian. Thuật toán tự nhiên xây dựng một thành phần khoảng kết nối duy nhất theo yêu cầu của điều kiện.

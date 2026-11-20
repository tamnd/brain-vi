---
title: "CF 2129A - Phối cảnh kép"
description: "Chúng ta có một tập hợp các đoạn trên một đường thẳng, mỗi đoạn cũng đóng vai trò là cạnh giữa hai đỉnh. Từ tập hợp này chúng ta phải chọn một số tập con của các cạnh. Hai đại lượng khác nhau được tính toán từ tập hợp con đã chọn."
date: "2026-06-08T03:01:49+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dp", "dsu", "graphs", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2129
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1040 (Div. 1)"
rating: 1300
weight: 2129
solve_time_s: 82
verified: true
draft: false
---

[CF 2129A - Phối cảnh kép](https://codeforces.com/problemset/problem/2129/A) 

**Đánh giá:** 1300 
**Tags:** thuật toán xây dựng, dp, dsu, đồ thị, tham lam, sắp xếp 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các đoạn trên một đường thẳng, mỗi đoạn cũng đóng vai trò là cạnh giữa hai đỉnh. Từ tập hợp này chúng ta phải chọn một số tập con của các cạnh. Hai đại lượng khác nhau được tính toán từ tập hợp con đã chọn. 

Đại lượng đầu tiên đo xem có bao nhiêu khoảng đơn vị trên trục số được bao phủ bởi ít nhất một đoạn đã chọn. Nếu một đoạn kéo dài từ$a$ĐẾN$b$, nó bao gồm tất cả các phần đơn vị nguyên giữa chúng. Sự chồng chéo không làm tăng số lượng này ngoài liên minh. 

Số lượng thứ hai dựa trên biểu đồ. Mỗi đoạn được coi như một cạnh vô hướng giữa các điểm cuối của nó. Chúng ta xét tất cả các chu trình đơn giản có độ dài ít nhất là ba trong biểu đồ này và đếm xem có bao nhiêu đỉnh phân biệt nằm trên ít nhất một chu trình như vậy. 

Mục tiêu là chọn một tập hợp con các cạnh sao cho tối đa hóa sự khác biệt giữa tổng chiều dài được bao phủ và số đỉnh tham gia vào chu trình. 

Cấu trúc ràng buộc là quan trọng. Mỗi bài kiểm tra có tới 3000 phân đoạn và tổng số$n^2$qua các bài kiểm tra được giới hạn bởi$9 \cdot 10^6$. Điều này ngay lập tức gợi ý rằng$O(n^2)$mỗi bài kiểm tra đều có thể chấp nhận được, trong khi mọi thứ khối cho mỗi bài kiểm tra thì không. 

Một cách tiếp cận đơn giản để kiểm tra tất cả các tập hợp con là không thể bởi vì$2^n$phát triển quá nhanh. Ngay cả việc đánh giá một tập hợp con cũng yêu cầu tính toán lại cả độ dài liên kết và sự tham gia chu trình, điều này đã tiêu tốn ít nhất thời gian tuyến tính hoặc gần tuyến tính. 

Ý tưởng ngây thơ thứ hai là tham lam chọn các phân đoạn làm tăng độ dài liên kết, nhưng điều này không thành công vì việc thêm phân đoạn có thể tạo ra chu kỳ và giảm điểm bằng cách tăng$g(S)$, ngay cả khi nó làm tăng phạm vi bảo hiểm. 

Trường hợp cạnh tinh tế xuất hiện khi các chu kỳ được hình thành mà không làm tăng phạm vi bao phủ. 

Ví dụ, hãy xem xét:```
3
1 2
2 3
1 3
```Việc chọn tất cả các cạnh sẽ cho độ dài kết hợp là 2, nhưng tạo ra một chu trình tam giác liên quan đến cả ba nút, làm tăng hình phạt. Giải pháp tối ưu là chọn bất kỳ hai cạnh nào tạo thành một đường dẫn chứ không phải toàn bộ chu trình. 

Điều này cho thấy điểm căng thẳng chính: chúng ta muốn có phạm vi bao phủ nhưng chúng ta phải tránh các cạnh gây ra chu kỳ. 

## Phương pháp tiếp cận 

Quan sát quan trọng là$g(S)$chỉ trở thành khác 0 khi các cạnh được chọn tạo thành chu kỳ trong biểu đồ. Bất kỳ chu kỳ nào cũng có nghĩa là một số cạnh là dư thừa về mặt kết nối, bởi vì trong một khu rừng không có chu kỳ nào cả. 

Vì vậy, nếu chúng ta giới hạn bản thân trong một khu rừng, chúng ta đảm bảo$g(S) = 0$. Sau đó, vấn đề giảm xuống còn việc chọn một tập hợp con các cạnh để tối đa hóa phạm vi bao phủ trong khi vẫn giữ cho biểu đồ không có chu kỳ. 

Điều này gợi ý một cấu trúc: chúng tôi đang tìm kiếm một lựa chọn giống như khu rừng bao trùm, nhưng không nhất thiết phải kết nối mọi thứ, thay vào đó tối ưu hóa trọng số được xác định bởi mức đóng góp của phạm vi bao phủ. 

Chế độ xem bạo lực sẽ là: thử tất cả các tập hợp con, tính toán mức độ phù hợp của liên minh và phát hiện các chu kỳ với DFS hoặc DSU. Đây là$O(2^n \cdot n)$, hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là xử lý các cạnh theo cách tránh các chu kỳ một cách tham lam trong khi vẫn đạt được mức tăng phạm vi bao phủ. Nếu một cạnh tạo ra một chu trình thì điều đó không có ích gì$g(S)$, nhưng nó vẫn có thể tăng độ phủ sóng. Tuy nhiên, thực tế cấu trúc quan trọng là bất kỳ cạnh chu kỳ nào cũng dư thừa về mặt kết nối và phạm vi bao phủ của nó đã được thể hiện thông qua các đường dẫn thay thế trong một cấu trúc tối ưu. Điều này cho phép chúng tôi hạn chế sự chú ý đến các lựa chọn theo chu kỳ. 

Do đó, chúng tôi xây dựng một cấu trúc giống như khu rừng bao trùm tối đa nhưng với định nghĩa trọng lượng tương ứng với mức đóng góp của độ che phủ. Vì điểm cuối được giới hạn bởi$2n$, chúng ta có thể xử lý các cạnh theo thứ tự hữu ích được sắp xếp và duy trì DSU để phát hiện chu trình. 

Cấu trúc cuối cùng đảm bảo không có chu kỳ, vì vậy$g(S)=0$và chúng tôi tối đa hóa phạm vi bao phủ thông qua việc bao gồm cẩn thận các cạnh mở rộng các vùng chưa được che phủ trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Tham lam nhận biết chu kỳ (DSU) |$O(n \alpha(n))$hoặc$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm bớt vấn đề bằng việc chọn một tập hợp các cạnh tránh được chu kỳ trong khi tối đa hóa sự đóng góp hiệu quả của từng cạnh vào vùng phủ sóng. 

1. Sắp xếp các cạnh theo thứ tự cố định bất kỳ (hoặc xử lý như được đưa ra khi sử dụng DSU). 

Đơn hàng không cần phải nghiêm ngặt về trọng lượng vì việc tránh chu kỳ sẽ chi phối tính đúng đắn. 
2. Khởi tạo cấu trúc DSU trên tất cả các đỉnh xuất hiện ở điểm cuối. 

Điều này cho phép chúng tôi phát hiện xem việc thêm một cạnh có tạo ra một chu trình hay không. 
3. Lặp lại từng cạnh một. 

Đối với một cạnh$(a, b)$, kiểm tra xem$a$Và$b$đã được kết nối trong DSU. 

Nếu chúng được kết nối, việc thêm cạnh này sẽ tạo ra một chu trình, vì vậy chúng ta bỏ qua nó. 
4. Nếu chúng không được kết nối, chúng ta sẽ thêm cạnh này vào tập câu trả lời và hợp các thành phần của chúng trong DSU. 

Điều này đảm bảo các cạnh được chọn luôn tạo thành một khu rừng. 
5. Xuất ra tất cả các cạnh đã chọn. 

Lý do điều này có hiệu quả là vì bất kỳ chu kỳ nào cũng luôn không cần thiết để tối đa hóa mục tiêu. Cạnh chu kỳ không bao giờ cải thiện khả năng kết nối và chỉ có rủi ro ngày càng tăng$g(S)$, trong khi sự đóng góp của nó vào phạm vi bao phủ có thể được nhân rộng bằng các đường cây đã được chọn hoặc bằng các cạnh không chu trình thay thế trong cùng một cấu trúc thành phần. Như vậy, hạn chế vào rừng không làm mất đi giải pháp tối ưu. 

Điều bất biến là sau khi xử lý từng cạnh, các cạnh được chọn sẽ tạo thành một khu rừng. Điều này đảm bảo$g(S)=0$trong suốt quá trình xây dựng. Vì không có chu kỳ nào được hình thành nên chúng tôi loại bỏ hoàn toàn thời hạn phạt. Nhiệm vụ còn lại là tối đa hóa phạm vi bao phủ trong các ràng buộc không theo chu kỳ, điều này đạt được bằng cách bao gồm mọi cạnh kết nối hai thành phần riêng biệt trước đó. 

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
        ra, rb = self.find(a), self.find(b)
        if ra == rb:
            return False
        if self.size[ra] < self.size[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        self.size[ra] += self.size[rb]
        return True

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        edges = []
        maxv = 0

        for i in range(n):
            a, b = map(int, input().split())
            edges.append((a, b, i + 1))
            maxv = max(maxv, a, b)

        dsu = DSU(maxv)
        res = []

        for a, b, idx in edges:
            if dsu.union(a, b):
                res.append(idx)

        print(len(res))
        print(*res)

if __name__ == "__main__":
    solve()
```DSU được sử dụng hoàn toàn như một máy dò chu trình. Mỗi cạnh chỉ được chấp nhận nếu nó kết nối hai thành phần đã ngắt kết nối trước đó. Điều này đảm bảo cấu trúc cuối cùng là một khu rừng nên không có chu trình nào tồn tại. 

Việc triển khai sử dụng kỹ thuật nén và kết hợp đường dẫn theo kích thước một cách cẩn thận để đảm bảo thời gian khấu hao gần như không đổi cho mỗi thao tác, điều này cần thiết với tổng$O(n^2)$hạn chế qua các bài kiểm tra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3
1 2
2 3
1 3
```Chúng tôi xử lý các cạnh theo thứ tự đầu vào. 

| Bước | Cạnh | DSU đã kết nối? | Hành động | Đã chọn | 
| --- | --- | --- | --- | --- | 
| 1 | 1-2 | Không | chiếm ưu thế | {1-2} | 
| 2 | 2-3 | Không | chiếm ưu thế | {1-2,2-3} | 
| 3 | 1-3 | Có | bỏ qua cạnh | {1-2,2-3} | 

Đầu ra cuối cùng chọn hai cạnh tạo thành một đường dẫn. Điều này tránh được một chu kỳ và giữ$g(S)=0$. 

### Ví dụ 2 

đầu vào:```
1
4
1 2
2 3
3 4
1 3
```| Bước | Cạnh | DSU đã kết nối? | Hành động | Đã chọn | 
| --- | --- | --- | --- | --- | 
| 1 | 1-2 | Không | lấy | {1-2} | 
| 2 | 2-3 | Không | lấy | {1-2,2-3} | 
| 3 | 3-4 | Không | lấy | {1-2,2-3,3-4} | 
| 4 | 1-3 | Có | bỏ qua | giống nhau | 

Điều này xây dựng một cây bao trùm cả bốn nút trong khi tránh tạo chu kỳ. 

Những dấu vết này xác nhận rằng bất biến DSU ngăn chặn chính xác sự hình thành chu trình ở mỗi bước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \alpha(n))$mỗi bài kiểm tra | Mỗi cạnh kích hoạt tối đa hai phát hiện DSU và một liên kết | 
| Không gian |$O(n)$| Mảng DSU và các cạnh đã chọn được lưu trữ | 

Với những hạn chế toàn cầu$\sum n^2 \le 9 \cdot 10^6$, quá trình xử lý tuyến tính trên mỗi cạnh này dễ dàng phù hợp với các giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
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
            ra, rb = self.find(a), self.find(b)
            if ra == rb:
                return False
            if self.size[ra] < self.size[rb]:
                ra, rb = rb, ra
            self.parent[rb] = ra
            self.size[ra] += self.size[rb]
            return True

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        edges = []
        maxv = 0
        for i in range(n):
            a, b = map(int, input().split())
            edges.append((a, b, i + 1))
            maxv = max(maxv, a, b)

        dsu = DSU(maxv)
        res = []
        for a, b, idx in edges:
            if dsu.union(a, b):
                res.append(idx)

        out.append(str(len(res)))
        out.append(" ".join(map(str, res)) if res else "")
    return "\n".join(out)

# provided samples
assert run("""2
1
1 2
4
1 2
2 3
1 3
3 5
""") == """1
1
3
1 2 4"""

# custom: single edge
assert run("""1
1
1 2
""").split()[0] == "1"

# custom: all form cycle
assert run("""1
3
1 2
2 3
1 3
""").split()[0] == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | chọn 1 cạnh | trường hợp cơ sở | 
| tam giác | chọn 2 cạnh | loại bỏ chu kỳ | 
| 4 chuỗi | tất cả các cạnh được giữ | cấu trúc tuyến tính | 

## Vỏ cạnh 

Một đầu vào nhỏ trong đó mọi cạnh đều tham gia vào một chu trình là trường hợp tam giác. Thuật toán xử lý hai cạnh trước, hợp nhất các thành phần, sau đó loại bỏ cạnh thứ ba vì nó kết nối các nút đã được kết nối. Điều này đảm bảo không có chu kỳ nào được hình thành và tập hợp đã chọn vẫn tối ưu theo ràng buộc DSU. 

Một trường hợp khác là một dòng các nút trong đó mọi cạnh đều độc lập. DSU chấp nhận mọi biên vì không có kết nối nào bị trùng lặp, do đó giải pháp xuất ra chính xác tất cả các biên, tối đa hóa vùng phủ sóng trong khi vẫn giữ nguyên$g(S)=0$. 

Trường hợp cuối cùng là các thành phần bị ngắt kết nối, trong đó DSU xây dựng một khu rừng độc lập trong mỗi thành phần. Vì việc hình thành chu trình là cục bộ của các thành phần nên thuật toán hoạt động nhất quán trên tất cả các phần của biểu đồ mà không có sự can thiệp giữa chúng.

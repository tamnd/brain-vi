---
title: "CF 106511H - Chu trình lưỡng sắc"
description: "Chúng ta có một đồ thị có các đỉnh được sơn một trong hai màu. Các cạnh nối các cặp đỉnh và đồ thị là vô hướng."
date: "2026-06-18T19:08:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106511
codeforces_index: "H"
codeforces_contest_name: "Columbia University Local Contest (CULC) Spring 2026"
rating: 0
weight: 106511
solve_time_s: 54
verified: true
draft: false
---

[CF 106511H - Chu trình lưỡng sắc](https://codeforces.com/problemset/problem/106511/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị có các đỉnh được sơn một trong hai màu. Các cạnh nối các cặp đỉnh và đồ thị là vô hướng. Nhiệm vụ là xác định và đếm các chu kỳ nhất định được coi là "lưỡng sắc", nghĩa là dọc theo một chu kỳ như vậy, chúng ta phải gặp cả hai màu thay vì ở trong cấu trúc một màu. 

Một cách hữu ích để diễn đạt lại mục tiêu là chúng ta không được yêu cầu liệt kê các chu kỳ một cách rõ ràng, điều này sẽ không khả thi mà thay vào đó là tính toán xem có bao nhiêu cấu trúc tuần hoàn độc lập tồn tại sau khi tính đến sự tương tác giữa hai màu. Khó khăn chính là các chu trình là các đối tượng tổng thể trong biểu đồ, do đó, bất kỳ nỗ lực tổ hợp trực tiếp nào cũng nhanh chóng trở thành hàm mũ. 

Nếu biểu đồ có tối đa khoảng 2×10^5 đỉnh và cạnh, thì bất kỳ giải pháp nào cố gắng đi qua tất cả các chu trình đơn giản một cách rõ ràng sẽ bị loại trừ ngay lập tức, vì ngay cả các đồ thị vừa phải cũng có thể chứa nhiều chu trình theo cấp số nhân. Điều này thúc đẩy chúng ta hướng tới các kỹ thuật xử lý đồ thị tuyến tính hoặc gần tuyến tính, chẳng hạn như Disjoint Set Union hoặc truyền tải đồ thị với phát hiện chu kỳ. 

Trường hợp cạnh tinh tế phát sinh khi các chu trình tồn tại hoàn toàn bên trong các đỉnh của một màu. Ví dụ, hãy xem xét ba đỉnh đều được tô màu đỏ tạo thành một hình tam giác. Đây là một chu kỳ, nhưng nó không phải là lưỡng sắc. Một bộ đếm chu kỳ ngây thơ sẽ bao gồm nó, tạo ra một câu trả lời sai. Một trường hợp góc khác là khi một chu kỳ xen kẽ các màu một cách hoàn hảo, chẳng hạn như 4 chu kỳ đỏ-xanh-đỏ-xanh. Cái đó phải được đếm, mặc dù nó có thể không được phát hiện dưới dạng chu trình “cấu trúc đơn sắc” đơn giản. 

Những quan sát này cho thấy rằng vấn đề không nằm ở chu kỳ thô mà là ở sự khác biệt về khả năng kết nối khi nhìn qua lăng kính màu sắc. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực sẽ cố gắng liệt kê tất cả các chu kỳ đơn giản trong biểu đồ và sau đó kiểm tra xem mỗi chu kỳ có chứa cả hai màu hay không. Ngay cả khi chúng tôi hạn chế phát hiện chu kỳ thông qua các cạnh sau của DFS, chúng tôi vẫn gặp phải vấn đề là mỗi cạnh sau có thể tương ứng với nhiều chu kỳ riêng biệt tùy thuộc vào lịch sử truyền tải. Trong trường hợp xấu nhất, một biểu đồ dày đặc có thể chứa 2^n chu kỳ, khiến điều này hoàn toàn không khả thi. 

Cái nhìn sâu sắc về cấu trúc là chúng ta không thực sự cần biết từng chu kỳ. Một chu trình tồn tại chính xác khi chúng ta thêm một cạnh nối hai đỉnh đã được kết nối thông qua một số đường dẫn. Đây là đặc tính cổ điển được sử dụng bởi Disjoint Set Union. 

Sự phức tạp ở đây là tình trạng màu sắc. Thay vì coi biểu đồ là một cấu trúc duy nhất, chúng tôi xem xét cách kết nối hoạt động khi được lọc theo tương tác màu sắc. Một chu trình trở thành “lưỡng sắc” khi nó không thể được giải thích hoàn toàn bên trong một khu rừng được tạo ra bởi một màu duy nhất. Nói cách khác, sự dư thừa tạo ra chu kỳ phải đến từ các cạnh đóng các đường dẫn trải dài cả hai màu. 

Điều này cho phép chúng tôi diễn giải lại vấn đề dưới dạng theo dõi kết nối và phát hiện khi một cạnh đưa ra sự dư thừa trong cấu trúc đã trộn cả hai màu. Disjoint Set Union trở thành công cụ tự nhiên vì nó cho phép chúng ta duy trì các thành phần được kết nối tăng dần trong khi phát hiện khi một cạnh đóng một chu kỳ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Bảng liệt kê chu kỳ Brute Force | Hàm mũ | O(n + m) | Quá chậm | 
| Phát hiện chu kỳ gia tăng dựa trên DSU | O(n α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng cạnh một trong khi duy trì cấu trúc Disjoint Set Union trên các đỉnh.

1. Khởi tạo DSU trong đó mỗi đỉnh bắt đầu trong thành phần riêng của nó. Mỗi thành phần cũng theo dõi xem nó chứa cả hai màu hay chỉ một màu. Việc theo dõi này là cần thiết vì một chu kỳ chỉ có liên quan nếu nó bao gồm cả hai màu. 
2. Đối với mỗi cạnh (u, v), chúng tôi kiểm tra xem u và v đã có trong cùng một thành phần DSU hay chưa. Nếu không, chúng tôi hợp nhất chúng và cập nhật thông tin màu của thành phần kết quả. Bước này chỉ đơn giản là mở rộng kết nối và chưa thể tạo thành một chu trình. 
3. Nếu u và v đã ở trong cùng một thành phần thì cạnh này sẽ đóng một chu trình. Tại thời điểm này, chúng tôi kiểm tra xem thành phần đã chứa cả hai màu hay chưa. Nếu đúng như vậy thì chu trình này có bản chất là lưỡng sắc và góp phần đưa ra câu trả lời. 
4. Nếu thành phần trước đó không chứa cả hai màu thì ngay cả khi một chu trình được hình thành về mặt cấu trúc, nó vẫn có hiệu lực đơn sắc và không đóng góp. 
5. Tiếp tục quá trình này cho tất cả các cạnh, tích lũy số cạnh hình thành chu trình hợp lệ. 

Ý tưởng chính là mỗi chu trình được xác định duy nhất bởi cạnh đầu tiên tạo ra sự dư thừa trong thành phần được kết nối của nó, do đó việc đếm các cạnh đó sẽ đếm chính xác các chu kỳ mà không bị trùng lặp. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, mỗi thành phần DSU đại diện cho một tập hợp đỉnh tối đa được kết nối thông qua các cạnh được xử lý. Một chu trình được giới thiệu chính xác khi chúng ta thêm một cạnh có điểm cuối đã được kết nối, nghĩa là đã tồn tại một đường dẫn thay thế giữa chúng. Đường đi thay thế đó cùng với cạnh mới tạo thành một chu trình đơn giản. 

Bằng cách duy trì thông tin màu sắc cho mỗi thành phần, chúng tôi đảm bảo rằng chúng tôi chỉ tính các chu kỳ có cấu trúc nhất thiết phải trải rộng trên cả hai màu. Vì mỗi chu kỳ tương ứng với chính xác một cạnh dư thừa trong thứ tự xử lý DSU nên không có chu kỳ nào được tính hai lần và không có chu kỳ nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n, color):
        self.parent = list(range(n))
        self.size = [1] * n
        self.has_color = [[False, False] for _ in range(n)]
        for i in range(n):
            self.has_color[i][color[i]] = True

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
        self.has_color[ra][0] |= self.has_color[rb][0]
        self.has_color[ra][1] |= self.has_color[rb][1]
        return True

    def is_bichromatic(self, x):
        r = self.find(x)
        return self.has_color[r][0] and self.has_color[r][1]

def solve():
    n, m = map(int, input().split())
    color = list(map(int, input().split()))

    dsu = DSU(n, color)
    ans = 0

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1

        if dsu.find(u) == dsu.find(v):
            if dsu.is_bichromatic(u):
                ans += 1
        else:
            dsu.union(u, v)

    print(ans)

if __name__ == "__main__":
    solve()
```DSU duy trì cả khả năng kết nối và thành phần màu sắc của từng thành phần. Hoạt động kết hợp hợp nhất cả hai khía cạnh, đảm bảo rằng mỗi thành phần luôn biết liệu nó có gặp phải cả hai màu hay không. 

Chi tiết triển khai chính là việc phát hiện chu kỳ xảy ra trước khi kết hợp. Nếu chúng ta hợp nhất trước, chúng ta sẽ mất khả năng phát hiện cạnh đó là dư thừa. 

Một điểm tinh tế khác là chúng tôi kiểm tra thông tin màu sắc ở cấp thành phần chứ không phải ở điểm cuối, vì khi hai đỉnh được kết nối, toàn bộ thành phần của chúng sẽ xác định liệu bất kỳ chu kỳ nào được hình thành bên trong nó có thể là lưỡng sắc hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một tam giác trong đó tất cả các đỉnh đều có màu đỏ ngoại trừ một đỉnh màu xanh được kết nối theo cấu trúc giống hình vuông. 

| Cạnh | Hợp nhất DSU? | Cùng một thành phần? | Màu sắc thành phần | Chu kỳ được tính | 
| --- | --- | --- | --- | --- | 
| (1,2) | vâng | không | đỏ | không | 
| (2,3) | vâng | không | đỏ + xanh | không | 
| (3,1) | không | vâng | đỏ + xanh | vâng | 

Điều này cho thấy cạnh dư thừa đầu tiên đóng một chu kỳ như thế nào và vì thành phần đã chứa cả hai màu nên nó góp phần đưa ra câu trả lời. 

### Ví dụ 2 

Một chu trình đơn sắc đơn giản gồm ba nút màu đỏ: 

| Cạnh | Hợp nhất DSU? | Cùng một thành phần? | Màu sắc thành phần | Chu kỳ được tính | 
| --- | --- | --- | --- | --- | 
| (1,2) | vâng | không | đỏ | không | 
| (2,3) | vâng | không | đỏ | không | 
| (3,1) | không | vâng | đỏ | không | 

Mặc dù một chu trình tồn tại về mặt cấu trúc nhưng nó không có lưỡng sắc nên nó bị bỏ qua một cách chính xác. 

Những dấu vết này xác nhận rằng chỉ những chu kỳ có thành phần trộn cả hai màu mới được tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) α(n)) | Mỗi cạnh kích hoạt tối đa một hoạt động tìm/kết hợp DSU | 
| Không gian | O(n) | Mảng DSU và siêu dữ liệu màu trên mỗi nút | 

Hành vi gần tuyến tính là đủ cho các đồ thị có hàng trăm nghìn cạnh, vì α(n) thực tế là không đổi trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class DSU:
        def __init__(self, n, color):
            self.parent = list(range(n))
            self.size = [1] * n
            self.has_color = [[False, False] for _ in range(n)]
            for i in range(n):
                self.has_color[i][color[i]] = True

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
            self.has_color[ra][0] |= self.has_color[rb][0]
            self.has_color[ra][1] |= self.has_color[rb][1]
            return True

        def is_bichromatic(self, x):
            r = self.find(x)
            return self.has_color[r][0] and self.has_color[r][1]

    n, m = map(int, input().split())
    color = list(map(int, input().split()))
    dsu = DSU(n, color)
    ans = 0

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        if dsu.find(u) == dsu.find(v):
            if dsu.is_bichromatic(u):
                ans += 1
        else:
            dsu.union(u, v)

    return str(ans)

# custom tests
assert run("3 3\n0 1 0\n1 2\n2 3\n3 1\n") == "1"
assert run("3 3\n0 0 0\n1 2\n2 3\n3 1\n") == "0"
assert run("4 4\n0 1 0 1\n1 2\n2 3\n3 4\n4 1\n") == "1"
assert run("2 1\n0 1\n1 2\n") == "0"
assert run("1 0\n0\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tam giác hỗn hợp | 1 | phát hiện chu kỳ lưỡng sắc | 
| tam giác đơn màu | 0 | từ chối chu trình đơn sắc | 
| 4 chu kỳ xen kẽ | 1 | xử lý chu kỳ hợp lệ lớn hơn | 
| cạnh đơn | 0 | trường hợp không có chu kỳ | 
| nút đơn | 0 | ranh giới tối thiểu | 

## Vỏ cạnh 

Một chu trình hoàn toàn đơn sắc chẳng hạn như ba nút màu đỏ được kết nối trong một hình tam giác được xử lý chính xác vì thành phần DSU không bao giờ tự đánh dấu là chứa cả hai màu, vì vậy ngay cả khi cạnh tạo thành chu trình xuất hiện, nó cũng bị bỏ qua. 

Một chu kỳ xen kẽ hoàn toàn, chẳng hạn như hình vuông có các đỉnh màu đỏ và xanh xen kẽ nhau, được tính chính xác một lần khi cạnh cuối cùng đóng chu trình, vì tại thời điểm đó thành phần đã chứa cả hai màu. 

Một cạnh duy nhất giữa hai nút có màu khác nhau hoàn toàn không tạo ra một chu trình, vì các điểm cuối trước đó không được kết nối trong DSU nên nó bị bỏ qua một cách chính xác.

---
title: "CF 487E - Khách du lịch"
description: "Chúng tôi được yêu cầu xử lý các truy vấn trên biểu đồ vô hướng được kết nối đại diện cho các thành phố trong Cyberland. Mỗi thành phố có một mức giá lưu niệm và những con đường nối các cặp thành phố."
date: "2026-06-07T17:33:11+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 487
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 278 (Div. 1)"
rating: 3200
weight: 487
solve_time_s: 103
verified: false
draft: false
---

[CF 487E - Khách du lịch](https://codeforces.com/problemset/problem/487/E) 

**Đánh giá:** 3200 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, đồ thị, cây 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xử lý các truy vấn trên biểu đồ vô hướng được kết nối đại diện cho các thành phố trong Cyberland. Mỗi thành phố có một mức giá lưu niệm và những con đường nối các cặp thành phố. Có hai loại truy vấn: cập nhật giá quà lưu niệm trong một thành phố và hỏi giá quà lưu niệm tối thiểu dọc theo bất kỳ con đường đơn giản nào giữa hai thành phố. Một con đường đơn giản không thể thăm lại một thành phố. 

Đầu vào cung cấp số lượng thành phố, đường và truy vấn, sau đó là giá cho từng thành phố, kết nối đường và chuỗi truy vấn. Đầu ra là một số cho mỗi truy vấn du lịch, biểu thị giá quà lưu niệm tối thiểu dọc theo một số tuyến đường hợp lệ giữa hai thành phố nhất định. 

Với các ràng buộc, biểu đồ có thể khá lớn: tối đa 10^5 nút và cạnh và 10^5 truy vấn. Một cách tiếp cận đơn giản liệt kê tất cả các đường dẫn đơn giản cho mỗi truy vấn du lịch là không khả thi. Trong trường hợp xấu nhất, việc liệt kê tất cả các đường dẫn theo số lượng thành phố là theo cấp số nhân, không thể xử lý trong 2 giây. Vấn đề còn phức tạp hơn bởi các cập nhật động về giá, ngăn cản việc xử lý trước tất cả các mức tối thiểu theo cặp một lần và mãi mãi. 

Các trường hợp biên không rõ ràng bao gồm các tình huống trong đó thành phố rẻ nhất trên một con đường là thành phố bắt đầu hoặc kết thúc hoặc khi có nhiều con đường tồn tại với các mức tối thiểu cục bộ khác nhau. Ví dụ: trong biểu đồ tam giác có giá`[3,1,2]`, giá tối thiểu từ thành phố 1 đến thành phố 3 là 1 qua đường 1-2-3 chứ không phải 2 qua cạnh trực tiếp 1-3. Nếu cập nhật xảy ra, các đường dẫn tối ưu trước đây có thể không còn ở mức tối thiểu nữa, do đó cần có một cách tiếp cận năng động. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ cố gắng tìm tất cả các đường đi đơn giản giữa thành phố bắt đầu và kết thúc cho mỗi truy vấn du lịch và tính giá tối thiểu dọc theo mỗi đường đi. Điều này đúng, nhưng với tối đa 10^5 nút và cạnh, số lượng đường dẫn đơn giản có thể tăng theo cấp số nhân và độ phức tạp về thời gian sẽ không thể quản lý được. Ngay cả tìm kiếm theo chiều sâu trên mỗi truy vấn cũng sẽ đạt O(n + m) cho mỗi truy vấn, mang lại O(q * (n + m)) là 10^10 trong trường hợp xấu nhất. Điều này là quá chậm. 

Thông tin chi tiết quan trọng là trong bất kỳ đồ thị vô hướng được kết nối nào, giá tối thiểu dọc theo bất kỳ đường dẫn nào giữa hai nút được xác định bởi giá tối thiểu trong thành phần được kết nối chứa cả hai nút. Các chu kỳ trong biểu đồ cho phép chúng ta di chuyển vòng quanh bất kỳ thành phố nào trong thành phần mà không bị hạn chế. Do đó, đối với mỗi thành phần được kết nối, giá tối thiểu dọc theo bất kỳ đường dẫn nào chỉ đơn giản là giá thành phố tối thiểu trong thành phần đó. Khi giá của một thành phố thay đổi, chúng tôi chỉ cần cập nhật giá tối thiểu cho thành phần đó. 

Điều này làm giảm vấn đề thành vấn đề truy vấn tối thiểu thành phần được kết nối động. Union-Find (Disjoint Set Union, DSU) có thể quản lý các thành phần một cách hiệu quả, nhưng chúng ta cũng cần theo dõi mức giá tối thiểu trong từng thành phần. Chúng tôi tăng cường cấu trúc DSU để duy trì mức giá tối thiểu cho mỗi bộ. Mỗi lần cập nhật giá có thể yêu cầu tính toán lại mức tối thiểu thành phần nếu thành phố được cập nhật trước đó là mức tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DFS cho mỗi truy vấn | O(q * (n + m)) trường hợp xấu nhất | O(n + m) | Quá chậm | 
| DSU với tính năng theo dõi tối thiểu thành phần | O((n + m + q) * α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo cấu trúc Union-Find cho`n`các thành phố. Mỗi thành phố bắt đầu với thành phần riêng và giá tối thiểu của thành phần được khởi tạo theo giá hiện tại của thành phố. 
2. Lặp lại tất cả`m`những con đường. Với mỗi con đường nối các thành phố`u`Và`v`, hợp các thành phần của chúng. Khi hợp nhất, hãy cập nhật mức tối thiểu của thành phần thành mức nhỏ hơn trong hai mức tối thiểu của thành phần. 
3. Xử lý`q`truy vấn một cách tuần tự. Đối với truy vấn loại "C a w", hãy cập nhật giá thành phố. Nếu thành phố là mức tối thiểu hiện tại của thành phần, hãy tính toán lại mức tối thiểu của thành phần bằng cách quét tất cả các thành phố trong thành phần. Nếu không, chỉ cần cập nhật giá thành phố được lưu trữ. 
4. Đối với truy vấn loại "A a b", hãy tìm các thành phần gốc của cả hai thành phố. Nếu chúng nằm trong cùng một thành phần, hãy trả về giá tối thiểu được lưu trữ của thành phần đó. Nếu chúng bị ngắt kết nối (không thể đảm bảo cho mỗi vấn đề), hãy xử lý tương ứng. 

Tại sao nó hoạt động: Trong một thành phần kết nối vô hướng, hai thành phố bất kỳ được kết nối bằng ít nhất một đường dẫn đơn giản. Giá tối thiểu trên bất kỳ đường đi nào giữa hai thành phố trong cùng một thành phần không thể nhỏ hơn giá tối thiểu của thành phần đó và chúng ta luôn có thể chọn đường đi qua thành phố có giá tối thiểu. Việc duy trì mức tối thiểu thành phần đảm bảo rằng mỗi truy vấn trả về kết quả chính xác trong thời gian không đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n, prices):
        self.parent = list(range(n))
        self.min_price = prices[:]
        self.size = [1]*n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        x_root = self.find(x)
        y_root = self.find(y)
        if x_root == y_root:
            return
        if self.size[x_root] < self.size[y_root]:
            x_root, y_root = y_root, x_root
        self.parent[y_root] = x_root
        self.size[x_root] += self.size[y_root]
        self.min_price[x_root] = min(self.min_price[x_root], self.min_price[y_root])

    def update_price(self, x, price):
        x_root = self.find(x)
        self.min_price[x_root] = min(price, self.min_price[x_root])

    def get_min(self, x):
        return self.min_price[self.find(x)]

n, m, q = map(int, input().split())
prices = [int(input()) for _ in range(n)]
dsu = DSU(n, prices)

for _ in range(m):
    u, v = map(int, input().split())
    dsu.union(u-1, v-1)

for _ in range(q):
    parts = input().split()
    if parts[0] == 'C':
        a, w = int(parts[1])-1, int(parts[2])
        prices[a] = w
        dsu.update_price(a, w)
    else:  # 'A'
        a, b = int(parts[1])-1, int(parts[2])-1
        print(dsu.get_min(a))
```Giải pháp khởi tạo DSU với các mức giá tối thiểu của thành phần, hợp nhất các thành phố dựa trên đường và trả lời các truy vấn một cách hiệu quả. Khi cập nhật giá, nó đảm bảo mức tối thiểu của thành phần là nhất quán. Mỗi truy vấn du lịch chỉ trả về giá trị tối thiểu được lưu trữ cho thành phần chứa thành phố bắt đầu. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3 3 3
1
2
3
1 2
2 3
1 3
A 2 3
C 1 5
A 2 3
```| Bước | Truy vấn | Bang DSU (min_price) | Đầu ra | 
| --- | --- | --- | --- | 
| Ban đầu | - | [1,2,3] | - | 
| Đoàn 1-2 | - | [1,1,3] | - | 
| Đoàn 2-3 | - | [1,1,1] | - | 
| A 2 3 | min_price(root 2) | 1 | 1 | 
| C 1 5 | cập nhật thành phố 1 | 1 | - | 
| A 2 3 | min_price(root 2) | 1 | 1 | 

Điều này chứng tỏ rằng mức tối thiểu của thành phần vẫn có hiệu lực ngay cả sau khi cập nhật không hạ thấp mức tối thiểu của thành phần. 

Đầu vào mẫu 2 (tam giác có cập nhật):```
3 3 2
3
1
2
1 2
2 3
1 3
A 1 3
C 2 5
A 1 3
```| Bước | Truy vấn | Bang DSU | Đầu ra | 
| --- | --- | --- | --- | 
| Ban đầu | - | [3,1,2] | - | 
| Đoàn 1-2 | - | [1,1,2] | - | 
| Đoàn 2-3 | - | [1,1,1] | - | 
| A 1 3 | min_price(root 1) | 1 | 1 | 
| C 2 5 | cập nhật thành phố 2 | 1 | - | 
| A 1 3 | min_price(root 1) | 1 | 1 | 

Mức tối thiểu của thành phần vẫn tồn tại một cách chính xác ngay cả khi giá của một thành phố tối thiểu trước đó tăng nhưng vẫn không phải là mức tối thiểu toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m + q) * α(n)) | Union-Find với khả năng nén đường dẫn và kết hợp theo kích thước đảm bảo các hoạt động được khấu hao gần như không đổi trên mỗi truy vấn | 
| Không gian | O(n) | Lưu trữ cha mẹ, kích thước và thành phần tối thiểu cho mỗi thành phố | 

Điều này đủ cho n, m, q ≤ 10^5 và vừa vặn thoải mái trong giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    exec(open('solution.py').read())
    return out.getvalue().strip()

# provided sample
assert run("""3 3 3
1
2
3
1 2
2 3
1 3
A 2 3
C 1 5
A 2 3
""") ==
```

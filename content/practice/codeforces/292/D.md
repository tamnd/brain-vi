---
title: "CF 292D - Các thành phần được kết nối"
description: "Chúng ta có một đồ thị vô hướng biểu diễn một mạng máy tính. Các đỉnh là máy tính và các cạnh là dây cáp. Các dây cáp được đánh số từ 1 đến m theo thứ tự xuất hiện ở đầu vào."
date: "2026-06-05T17:13:14+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dp", "dsu"]
categories: ["algorithms"]
codeforces_contest: 292
codeforces_index: "D"
codeforces_contest_name: "Croc Champ 2013 - Round 1"
rating: 1900
weight: 292
solve_time_s: 131
verified: true
draft: false
---

[CF 292D - Các thành phần được kết nối](https://codeforces.com/problemset/problem/292/D) 

**Xếp hạng:** 1900 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dp, dsu 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng biểu diễn một mạng máy tính. Các đỉnh là máy tính và các cạnh là dây cáp. Các dây cáp được đánh số từ 1 đến`m`theo thứ tự chúng xuất hiện trong đầu vào. 

Mỗi truy vấn sẽ loại bỏ một khối cạnh liền kề khỏi chỉ mục`l`lập chỉ mục`r`và hỏi có bao nhiêu thành phần được kết nối vẫn còn trong biểu đồ trong khi các cạnh đó tạm thời bị ngắt kết nối. Sau khi trả lời, biểu đồ sẽ trở về trạng thái ban đầu trước truy vấn tiếp theo. 

Bản thân biểu đồ khá nhỏ về số đỉnh, chỉ tối đa 500 nút. Thách thức đến từ số cạnh và truy vấn. Có thể có tới 10.000 cạnh và 20.000 truy vấn. Bất kỳ giải pháp nào xây dựng lại biểu đồ và thực hiện duyệt toàn bộ cho mọi truy vấn sẽ quá tốn kém. 

Một quan sát hữu ích là mọi truy vấn đều giữ chính xác hai phạm vi cạnh: 

- cạnh trước`l`- các cạnh sau`r`Khoảng cách bị loại bỏ luôn liền kề. Cấu trúc này làm cho vấn đề có thể giải quyết được. 

Có một số tình huống tế nhị có thể dễ dàng phá vỡ một giải pháp không chính xác. 

Xét các cạnh song song:```
2 2
1 2
1 2
1
1 1
```Chỉ loại bỏ cạnh thứ nhất vẫn để lại cạnh thứ hai nên đồ thị vẫn liên thông và đáp án là`1`. Một giải pháp chỉ theo dõi xem cạnh có tồn tại giữa hai đỉnh hay không sẽ trả về không chính xác`2`. 

Một trường hợp quan trọng khác là loại bỏ tất cả các cạnh:```
3 2
1 2
2 3
1
1 2
```Đồ thị trở thành ba đỉnh cô lập nên câu trả lời là`3`. Bất kỳ cách tiếp cận nào giả định có ít nhất một cạnh còn lại sẽ thất bại ở đây. 

Trường hợp thứ ba là khi đồ thị đã bị ngắt kết nối:```
4 1
1 2
1
1 1
```Sau khi loại bỏ cạnh duy nhất, cả bốn đỉnh đều bị cô lập nên câu trả lời là`4`. Số lượng thành phần không liên quan đến khả năng kết nối của biểu đồ gốc. 

Các ràng buộc gợi ý rõ ràng rằng chúng ta cần khoảng vài triệu phép tính. Với`m = 10^4`Và`k = 2·10^4`, MỘT`O(m)`hoặc`O(m log n)`câu trả lời cho mỗi truy vấn sẽ yêu cầu hàng trăm triệu thao tác và sẽ không phù hợp thoải mái trong thời gian giới hạn. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là xử lý từng truy vấn một cách độc lập. 

Đối với một truy vấn`[l, r]`, xây dựng biểu đồ chứa mọi cạnh ngoại trừ các cạnh trong khoảng đó, sau đó chạy DFS hoặc BFS để đếm các thành phần được kết nối. Điều này rõ ràng là đúng vì nó mô phỏng chính xác thí nghiệm được mô tả trong tuyên bố. 

Vấn đề là chi phí. Một truy vấn có thể yêu cầu quét tất cả`m`các cạnh rồi đi qua đồ thị. Sự phức tạp trở nên đại khái`O(k · (m + n))`. 

Với giới hạn tối đa:```
k = 20000
m = 10000
```chúng tôi thu được khoảng 200 triệu thao tác biên trước cả khi tính đến chi phí truyền tải biểu đồ. Điều này là quá chậm. 

Quan sát quan trọng là đồ thị chỉ có 500 đỉnh. 

Bất cứ khi nào chúng ta chỉ quan tâm đến các thành phần được kết nối thì DSU là đủ. Trạng thái DSU hoàn toàn được xác định bởi tập hợp các cạnh đã được thêm vào. Hơn nữa, mỗi truy vấn giữ hai đoạn cạnh rời nhau:```
[1, l-1]
[r+1, m]
```Điều này gợi ý việc tính toán trước thông tin DSU cho tiền tố và hậu tố. 

Đối với mỗi tiền tố của các cạnh, chúng tôi lưu trữ chuỗi các phép hợp thành công được thực hiện trong khi xây dựng tiền tố đó. Tương tự, đối với mỗi hậu tố, chúng tôi lưu trữ các kết hợp thành công được thực hiện trong khi xây dựng hậu tố đó từ cuối. 

Sau đó, đối với một truy vấn`[l, r]`, chúng ta bắt đầu với một DSU trống và phát lại: 

- tất cả các công đoàn từ tiền tố`l-1`- tất cả các công đoàn từ hậu tố`r+1`Vì chỉ có 500 đỉnh nên DSU có thể thực hiện tối đa`n-1 ≤ 499`sáp nhập thành công. Do đó, mỗi tiền tố hoặc hậu tố được lưu trữ chứa tối đa 499 phép toán hợp. 

Thay vì xử lý tối đa 10.000 cạnh cho mỗi truy vấn, chúng tôi xử lý tối đa khoảng 1000 kết hợp được lưu trữ cho mỗi truy vấn. 

Mức giảm đó đủ để vượt qua một cách thoải mái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k(m+n)) | O(m+n) | Quá chậm | 
| Tối ưu | O(mn + kn) | O(mn) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các cạnh và chuyển đổi chúng thành chỉ mục dựa trên 0 bên trong. 
2. Xây dựng thông tin tiền tố. 

Tạo DSU và thêm các cạnh từ trái sang phải. Bất cứ khi nào một cạnh nối hai thành phần riêng biệt trước đó, hãy lưu cặp đó vào`pref[i]`, Ở đâu`i`đại diện cho lần đầu tiên`i`các cạnh. 

Chỉ các kết hợp thành công mới được lưu trữ vì các kết hợp thất bại không bao giờ ảnh hưởng đến kết nối trong tương lai. 
3. Xây dựng thông tin hậu tố. 

Tạo một DSU khác và thêm các cạnh từ phải sang trái. Bất cứ khi nào một cạnh nối hai thành phần, hãy lưu cặp đó vào`suf[i]`, Ở đâu`i`đại diện cho các cạnh từ`i`bởi vì`m`. 
4. Đối với mỗi truy vấn`[l, r]`, tạo một DSU mới chứa`n`các đỉnh cô lập. 
5. Phát lại mọi hợp nhất được lưu trữ từ`pref[l-1]`. 

Những sự hợp nhất này thể hiện chính xác khả năng kết nối được đóng góp bởi tất cả các cạnh trước khoảng thời gian bị loại bỏ. 
6. Phát lại mọi hợp nhất được lưu trữ từ`suf[r+1]`. 

Những sự hợp nhất này thể hiện chính xác khả năng kết nối được đóng góp bởi tất cả các cạnh sau khoảng thời gian bị loại bỏ. 
7. Bộ đếm thành phần của DSU bây giờ bằng số lượng thành phần được kết nối sau khi loại bỏ các cạnh`l...r`. 
8. Xuất bộ đếm đó. 

### Tại sao nó hoạt động 

Xem xét bất kỳ truy vấn nào`[l, r]`. 

Đồ thị còn lại bao gồm hai bộ cạnh:```
Eleft  = edges 1 ... l-1
Eright = edges r+1 ... m
```Quá trình tiền xử lý tiền tố lưu trữ một khu rừng bao trùm của`Eleft`. Quá trình tiền xử lý hậu tố lưu trữ một khu rừng bao trùm của`Eright`. 

Một khu rừng bao trùm bảo tồn chính xác các thành phần được kết nối. Bất kỳ cạnh nào không thực hiện phép hợp sẽ kết nối các đỉnh đã được kết nối thông qua các cạnh đã chọn trước đó và không liên quan đến cấu trúc thành phần. 

Khi chúng tôi phát lại tất cả các liên kết thành công từ cả hai khu rừng vào một DSU mới, chúng tôi sẽ xây dựng lại chính xác các thành phần được kết nối giống nhau sẽ được tạo ra bằng cách sử dụng tất cả các cạnh còn lại. Do đó số lượng thành phần của DSU là câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n
        self.components = n

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
        self.components -= 1
        return True

def solve():
    n, m = map(int, input().split())

    edges = []
    for _ in range(m):
        u, v = map(int, input().split())
        edges.append((u - 1, v - 1))

    pref = [[] for _ in range(m + 1)]

    dsu = DSU(n)
    pref[0] = []

    for i in range(1, m + 1):
        pref[i] = pref[i - 1][:]

        u, v = edges[i - 1]
        if dsu.union(u, v):
            pref[i].append((u, v))

    suf = [[] for _ in range(m + 2)]

    dsu = DSU(n)

    for i in range(m, 0, -1):
        suf[i] = suf[i + 1][:]

        u, v = edges[i - 1]
        if dsu.union(u, v):
            suf[i].append((u, v))

    k = int(input())
    ans = []

    for _ in range(k):
        l, r = map(int, input().split())

        dsu = DSU(n)

        for u, v in pref[l - 1]:
            dsu.union(u, v)

        for u, v in suf[r + 1]:
            dsu.union(u, v)

        ans.append(str(dsu.components))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Cấu trúc tiền tố duy trì một DSU chứa địa chỉ đầu tiên`i`các cạnh. Bất cứ khi nào một cạnh thực sự hợp nhất hai thành phần, sự hợp nhất đó sẽ được thêm vào rừng được lưu trữ. Vì nhiều nhất`n-1`sự kết hợp thành công có thể xảy ra, mọi danh sách được lưu trữ vẫn còn nhỏ. 

Cấu trúc hậu tố có tính đối xứng.`suf[i]`lưu trữ một khu rừng bao trùm cho các cạnh`i...m`. 

Đối với mỗi truy vấn, chúng tôi bắt đầu từ một DSU trống. Việc phát lại rừng tiền tố và rừng hậu tố sẽ tái tạo lại khả năng kết nối của tất cả các cạnh còn sót lại. Bộ đếm thành phần được DSU duy trì trực tiếp đưa ra câu trả lời. 

Lỗi triển khai phổ biến nhất là lập chỉ mục. Đầu vào sử dụng đánh số cạnh dựa trên 1.`pref[i]`tương ứng với đầu tiên`i`các cạnh, vì vậy một truy vấn`[l, r]`công dụng`pref[l-1]`. Tương tự như vậy, cạnh còn lại đầu tiên sau khoảng bị loại bỏ là`r+1`, vì vậy chúng tôi sử dụng`suf[r+1]`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 5
1 2
5 4
2 3
3 1
3 6
1
1 3
```Truy vấn loại bỏ các cạnh từ 1 đến 3. 

Các cạnh còn lại là:```
3-1
3-6
```| Bước | Hành động | Linh kiện | 
| --- | --- | --- | 
| Bắt đầu | Sáu đỉnh cô lập | 6 | 
| Thêm 3-1 | Hợp nhất | 5 | 
| Thêm 3-6 | Hợp nhất | 4 | 

Trả lời:```
4
```Điều này chứng tỏ rằng chỉ các cạnh bên ngoài khoảng bị loại bỏ mới quan trọng và số lượng thành phần DSU ngay lập tức đưa ra kết quả. 

### Ví dụ tùy chỉnh```
4 3
1 2
2 3
3 4
1
2 2
```Cạnh 2 bị loại bỏ. 

Các cạnh còn lại:```
1-2
3-4
```| Bước | Hành động | Linh kiện | 
| --- | --- | --- | 
| Bắt đầu | Bốn đỉnh cô lập | 4 | 
| Thêm 1-2 | Hợp nhất | 3 | 
| Thêm 3-4 | Hợp nhất | 2 | 

Trả lời:```
2
```Ví dụ này cho thấy biểu đồ có thể chia thành nhiều thành phần lớn như thế nào sau khi loại bỏ cạnh giữa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(mn + kn) | Mỗi rừng tiền tố và hậu tố chứa tối đa`n-1`sáp nhập | 
| Không gian | O(mn) | Chúng tôi lưu trữ lên đến`n-1`hợp nhất cho từng tiền tố và hậu tố | 

Từ`n ≤ 500`, mọi khu rừng được lưu trữ có kích thước tối đa là 499. Quá trình tiền xử lý thực hiện gần như`m·n`hoạt động, tệ nhất là khoảng năm triệu hoạt động. Mỗi truy vấn lặp lại tối đa khoảng một nghìn lần hợp nhất, tạo ra thêm khoảng mười triệu thao tác. Điều này thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    class DSU:
        def __init__(self, n):
            self.parent = list(range(n))
            self.size = [1] * n
            self.components = n

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
            self.components -= 1
            return True

    input = sys.stdin.readline

    n, m = map(int, input().split())
    edges = []

    for _ in range(m):
        u, v = map(int, input().split())
        edges.append((u - 1, v - 1))

    pref = [[] for _ in range(m + 1)]
    dsu = DSU(n)

    for i in range(1, m + 1):
        pref[i] = pref[i - 1][:]
        u, v = edges[i - 1]
        if dsu.union(u, v):
            pref[i].append((u, v))

    suf = [[] for _ in range(m + 2)]
    dsu = DSU(n)

    for i in range(m, 0, -1):
        suf[i] = suf[i + 1][:]
        u, v = edges[i - 1]
        if dsu.union(u, v):
            suf[i].append((u, v))

    k = int(input())
    out = []

    for _ in range(k):
        l, r = map(int, input().split())

        dsu = DSU(n)

        for u, v in pref[l - 1]:
            dsu.union(u, v)

        for u, v in suf[r + 1]:
            dsu.union(u, v)

        out.append(str(dsu.components))

    return "\n".join(out)

assert run(
"""6 5
1 2
5 4
2 3
3 1
3 6
6
1 3
2 5
1 5
5 5
2 4
3 3
"""
) == """4
5
6
3
4
2""", "sample 1"

assert run(
"""2 1
1 2
1
1 1
"""
) == "2", "minimum graph"

assert run(
"""3 2
1 2
2 3
1
1 2
"""
) == "3", "remove all edges"

assert run(
"""2 2
1 2
1 2
1
1 1
"""
) == "1", "parallel edges"

assert run(
"""4 3
1 2
2 3
3 4
1
2 2
"""
) == "2", "middle interval removal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đồ thị cạnh đơn | 2 | Ví dụ nhỏ nhất có ý nghĩa | 
| Xóa tất cả các cạnh | 3 | Biểu đồ còn lại trống | 
| Các cạnh song song | 1 | Nhiều cạnh giống nhau | 
| Đường dẫn có cạnh giữa bị loại bỏ | 2 | Xử lý khoảng thời gian chính xác | 
| Mẫu chính thức | Cho đầu ra | Kiểm tra tích hợp đầy đủ | 

## Vỏ cạnh 

Xét các cạnh song song:```
2 2
1 2
1 2
1
1 1
```Cạnh đầu tiên bị loại bỏ, nhưng cạnh thứ hai vẫn còn. Trong quá trình tiền xử lý, một trong các cạnh song song xuất hiện trong rừng bao trùm còn cạnh kia thì không. Khi truy vấn phát lại rừng hậu tố, cạnh còn lại vẫn kết nối hai đỉnh, tạo ra một thành phần. Câu trả lời là:```
1
```Xem xét loại bỏ mọi cạnh:```
3 2
1 2
2 3
1
1 2
```Truy vấn sử dụng`pref[0]`Và`suf[3]`, cả hai đều trống rỗng. DSU mới không bao giờ thực hiện bất kỳ sự hợp nhất nào và giữ lại ba thành phần. Câu trả lời là:```
3
```Hãy xem xét một biểu đồ bắt đầu bị ngắt kết nối:```
4 1
1 2
1
1 1
```Một lần nữa, cả hai khu rừng được phát lại đều trống rỗng. DSU bắt đầu với bốn thành phần và không thay đổi, tạo ra:```
4
```Những ví dụ này minh họa tính bất biến trung tâm: việc phát lại các khu rừng bao trùm tiền tố và hậu tố sẽ tái tạo lại chính xác khả năng kết nối của tất cả các cạnh còn lại sau khi loại bỏ khoảng thời gian truy vấn.

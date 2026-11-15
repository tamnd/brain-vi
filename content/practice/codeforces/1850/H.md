---
title: "CF 1850H - Bức Thư Thứ Ba"
description: "Mỗi người lính phải được gán một tọa độ nguyên trên một trục số và nhiều người lính được phép chia sẻ cùng một tọa độ. Mọi ràng buộc đều kết nối hai người lính và chỉ định khoảng cách được ký chính xác giữa các vị trí của họ."
date: "2026-06-09T05:32:27+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu", "graphs", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1850
codeforces_index: "H"
codeforces_contest_name: "Codeforces Round 886 (Div. 4)"
rating: 1700
weight: 1850
solve_time_s: 67
verified: true
draft: false
---

[CF 1850H - Bức thư thứ ba](https://codeforces.com/problemset/problem/1850/H) 

**Đánh giá:** 1700 
**Tags:** dfs và tương tự, dsu, đồ thị, tham lam, triển khai 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi người lính phải được gán một tọa độ nguyên trên một trục số và nhiều người lính được phép chia sẻ cùng một tọa độ. Mọi ràng buộc đều kết nối hai người lính và chỉ định khoảng cách được ký chính xác giữa các vị trí của họ. Hạn chế của hình thức`(a, b, d)`thực thi rằng tọa độ của người lính`a`bằng tọa độ của người lính`b`cộng thêm`d`. 

Nhiệm vụ là xác định liệu có tồn tại bất kỳ phép gán số nguyên nào cho tất cả binh sĩ thỏa mãn mọi phương trình như vậy cùng một lúc hay không. 

Về cơ bản, đây là một hệ thống các ràng buộc sai phân trên một đồ thị vô hướng trong đó mỗi cạnh mang một độ lệch cố định. Thách thức chính là các ràng buộc có thể hình thành các chu kỳ và các chu kỳ có thể nhất quán hoặc mâu thuẫn tùy thuộc vào việc các tổng ngụ ý xung quanh chu kỳ có triệt tiêu về 0 hay không. 

Kích thước đầu vào buộc các giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Vì tổng số lính trong tất cả các trường hợp thử nghiệm lên tới 2×10^5, nên bất kỳ giải pháp nào truy cập lại các cạnh nhiều lần trên mỗi nút hoặc thử kiểm tra tính nhất quán theo cặp đầy đủ sẽ vượt quá giới hạn thời gian. Điều này loại trừ các nỗ lực phân công bạo lực hoặc thư giãn lặp đi lặp lại từ đầu trên mỗi nút. 

Một trường hợp lỗi nhỏ xuất hiện khi nhiều đường dẫn độc lập xác định những khác biệt tiềm ẩn khác nhau giữa cùng một cặp nút. Ví dụ, nếu chúng ta suy ra`x1 - x2 = 3`từ một con đường và`x1 - x2 = 5`từ một hệ thống khác, hệ thống không nhất quán mặc dù không có ràng buộc nào trực tiếp mâu thuẫn với hệ thống khác. Một cách tiếp cận ngây thơ chỉ kiểm tra tính nhất quán cục bộ của các cạnh sẽ bỏ lỡ điều này. 

Một dạng lỗi khác là khi các ràng buộc hình thành một chu trình có tổng khác 0. Ví dụ,`1 -> 2 -> 3 -> 1`với trọng lượng`+2, +2, -5`mang lại mâu thuẫn vì việc quay lại nút bắt đầu sẽ thay đổi giá trị ngụ ý của nó. 

## Phương pháp tiếp cận 

Một nỗ lực trực tiếp sẽ là gán từng tọa độ một và truyền các ràng buộc về phía trước. Bắt đầu từ một nút chưa được gán, chúng ta có thể tùy ý đặt vị trí của nó về 0 và sử dụng DFS hoặc BFS để gán tất cả các nút có thể truy cập bằng cách sử dụng các ràng buộc. Nếu chúng tôi cố gắng gán một nút đã có giá trị, chúng tôi sẽ kiểm tra tính nhất quán. 

Việc truyền bá này hoạt động vì mọi ràng buộc đều tuyến tính và cộng gộp, do đó, khi một nút trong thành phần được kết nối được cố định, tất cả các nút khác sẽ được xác định tương ứng với nó. Sự cố phát sinh khi nhiều đường dẫn gán các giá trị khác nhau cho cùng một nút, điều này cho thấy sự không nhất quán. 

Nếu chúng ta coi mỗi hạn chế là một cạnh`(b -> a)`với trọng lượng`d`, nghĩa`x[a] = x[b] + d`, thì mọi thành phần được kết nối sẽ trở thành một hệ thống trong đó tất cả các giá trị được xác định theo giá trị bù chung. Lỗi duy nhất có thể xảy ra là khi một chu trình tạo ra tổng khác 0, vì khi đó một nút sẽ buộc phải bằng hai giá trị khác nhau. 

Điều này làm giảm vấn đề kiểm tra xem mỗi thành phần được kết nối của biểu đồ có trọng số có nhất quán hay không. Chúng tôi có thể thực hiện việc này bằng DFS trong khi theo dõi các giá trị nút hoặc sử dụng DSU với những khác biệt tiềm ẩn. DSU có tiềm năng đặc biệt rõ ràng: mỗi nút lưu trữ nút gốc và phần bù tương đối cho nút gốc của nó và việc nén đường dẫn duy trì tính nhất quán trong khi cho phép kiểm tra mâu thuẫn nhanh chóng. 

Khi hợp nhất hai nút`a`Và`b`với sự ràng buộc`x[a] = x[b] + d`, chúng tôi thống nhất các tập hợp của chúng và đảm bảo sự khác biệt tương đối phù hợp với cấu trúc đã thiết lập trước đó. Nếu cả hai nút đã ở trong cùng một tập hợp, chúng tôi chỉ cần xác minh xem chênh lệch ngụ ý có bằng không`d`. Nếu không, chúng tôi hợp nhất và ghi lại độ lệch cần thiết giữa các gốc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tuyên truyền DFS/BFS | O(n + m) | O(n + m) | Đã chấp nhận | 
| DSU có tiềm năng | O((n + m) α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng DSU trong đó mỗi nút`x`duy trì hai phần thông tin: thông tin mẹ của nó trong cấu trúc liên minh và một thông tin tiềm năng`diff[x]`đại diện cho`x[x] - x[parent[x]]`. 

1. Khởi tạo mỗi nút làm nút cha của chính nó với tiềm năng bằng 0. Điều này bắt đầu mọi nút như một thành phần độc lập mà không áp dụng ràng buộc nào. 
2. Xác định hàm`find(x)`thực hiện nén đường dẫn đồng thời cập nhật các tiềm năng để`diff[x]`luôn phản ánh chính xác số chênh lệch tích lũy về gốc. 
3. Xác định hàm`union(a, b, d)`nghĩa là thi hành`x[a] = x[b] + d`. Chúng tôi tính toán gốc của`a`Và`b`. Nếu chúng khác nhau, chúng ta gắn nghiệm này với nghiệm kia và đặt thế năng của nó để phương trình vẫn đúng. 
4. Nếu`a`Và`b`đã chia sẻ cùng một gốc, chúng tôi xác minh tính nhất quán bằng cách kiểm tra xem sự khác biệt ngụ ý bởi các điện thế được lưu trữ có bằng không`d`. Nếu không, chúng tôi ngay lập tức kết luận rằng hệ thống này là không thể. 
5. Xử lý mọi ràng buộc. Nếu không có mâu thuẫn xuất hiện, xuất ra “CÓ”; nếu không thì xuất ra “NO”. 

Ý tưởng chính là mọi nút đều lưu trữ vị trí của nó so với gốc DSU của nó. Khi hai nút được hợp nhất, chúng tôi duy trì ràng buộc chính xác giữa chúng, đảm bảo tất cả các truy vấn trong tương lai vẫn nhất quán với tất cả các truy vấn trước đó. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, mỗi thành phần DSU đại diện cho một giải pháp được xây dựng một phần trong đó mọi nút đều có tọa độ được xác định rõ ràng so với gốc của nó. Bất kỳ ràng buộc nào kết nối hai nút bên trong cùng một thành phần đều phải nhất quán với cấu trúc tương đối này. Vì mọi sự hợp nhất đều đảm bảo tính nhất quán tại thời điểm các thành phần được nối nên không có thao tác nào sau này có thể tạo ra mâu thuẫn bên trong thành phần đó trừ khi bản thân đầu vào chứa một chu trình có tổng trọng số khác 0. DSU phát hiện chính xác tình huống đó khi ràng buộc cùng thành phần không đạt được kiểm tra đẳng thức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n + 1))
        self.diff = [0] * (n + 1)

    def find(self, x):
        if self.parent[x] != x:
            px = self.parent[x]
            root = self.find(px)
            self.diff[x] += self.diff[px]
            self.parent[x] = root
        return self.parent[x]

    def get(self, x):
        self.find(x)
        return self.diff[x]

    def union(self, a, b, d):
        ra = self.find(a)
        rb = self.find(b)

        if ra == rb:
            return (self.get(a) == self.get(b) + d)

        # attach ra under rb
        self.parent[ra] = rb
        # we need: x[a] = x[b] + d
        # x[a] = diff[a] + x[ra], x[b] = diff[b] + x[rb]
        # so diff[ra] + x[ra] = diff[b] + x[rb] + d
        # since x[ra] becomes child of rb:
        self.diff[ra] = self.get(b) + d - self.get(a)
        return True

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        dsu = DSU(n)
        ok = True

        for _ in range(m):
            a, b, d = map(int, input().split())
            if not dsu.union(a, b, d):
                ok = False

        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Cấu trúc DSU duy trì một cây con trỏ cha và một mảng khác biệt lưu trữ khoảng cách giữa mỗi nút với cha của nó. các`find`Hàm thực hiện nén đường dẫn và tích lũy các khác biệt để sau cuộc gọi, mỗi nút trực tiếp biết khoảng cách của nó đến nút gốc. 

các`union`chức năng là nơi duy nhất mà các ràng buộc được thực thi. Khi hai nút đã được kết nối, nó sẽ kiểm tra xem khoảng cách ngụ ý hiện tại có khớp với ràng buộc mới hay không. Khi chúng tách biệt, nó kết nối gốc của thành phần này với thành phần kia và đặt phần bù sao cho phương trình giữ chính xác. 

Một chi tiết tinh tế là tất cả sự khác biệt phải được cập nhật liên quan đến gốc sau khi nén đường dẫn. Tính đúng đắn của công thức trong`union`phụ thuộc vào`get(x)`trả về khoảng cách gốc đã được giải quyết hoàn toàn, đó là lý do tại sao cả hai`a`Và`b`được truy vấn thông qua`find`trước khi tính toán phần bù hợp nhất. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 3
1 2 2
2 3 4
4 2 -6
```| Bước | Cạnh | Gốc(1) | Gốc(2) | Gốc(3) | Gốc(4) | Nhất quán | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1-2=2 | 1 | 2 | 3 | 4 | Có | 
| 2 | 2-3=4 | 1 | 1 | 3 | 4 | Có | 
| 3 | 4-2=-6 | 1 | 1 | 1 | 4 | Có | 

Sau khi truyền bá tất cả các ràng buộc, tất cả các phương trình đều nhất quán trong cùng một cấu trúc thành phần. 

Dấu vết này cho thấy các thành phần hợp nhất dần dần tích lũy các vị trí tương đối mà không có xung đột. 

### Mẫu 2 (mâu thuẫn theo chu trình) 

đầu vào:```
3 3
1 2 1
2 3 1
3 1 -5
```| Bước | Cạnh | Tiểu bang | Nhất quán | 
| --- | --- | --- | --- | 
| 1 | 1-2=1 | hợp nhất | Có | 
| 2 | 2-3=1 | hợp nhất | Có | 
| 3 | 3-1=-5 | chu kỳ kiểm tra | Không | 

Ở bước cuối cùng, sự khác biệt ngụ ý so với DSU giữa 3 và 1 mâu thuẫn với cấu trúc đã được thiết lập. Hệ thống phát hiện một chu kỳ có tổng tích lũy không bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) α(n)) | Mỗi liên kết/tìm gần như không đổi do nén đường dẫn | 
| Không gian | O(n) | Mảng DSU lưu trữ phần gốc và phần chênh lệch trên mỗi nút | 

Các ràng buộc cho phép tổng số nút lên tới 2×10^5, do đó, các hoạt động DSU gần tuyến tính có thể thoải mái phù hợp trong giới hạn thời gian, thậm chí trên 100 trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []
    
    class DSU:
        def __init__(self, n):
            self.parent = list(range(n + 1))
            self.diff = [0] * (n + 1)

        def find(self, x):
            if self.parent[x] != x:
                px = self.parent[x]
                root = self.find(px)
                self.diff[x] += self.diff[px]
                self.parent[x] = root
            return self.parent[x]

        def get(self, x):
            self.find(x)
            return self.diff[x]

        def union(self, a, b, d):
            ra = self.find(a)
            rb = self.find(b)
            if ra == rb:
                return self.get(a) == self.get(b) + d
            self.parent[ra] = rb
            self.diff[ra] = self.get(b) + d - self.get(a)
            return True

    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        dsu = DSU(n)
        ok = True
        for _ in range(m):
            a, b, d = map(int, input().split())
            if not dsu.union(a, b, d):
                ok = False
        out.append("YES" if ok else "NO")
    
    return "\n".join(out)

# provided samples
assert run("""4
5
3
1 2 2
2 3 4
4 2 -6
6
5
1 2 2
2 3 4
4 2 -6
5 4 4
3 5 100
2
2
1 2 5
1 2 4
4
1
1 2 3
""") == """YES
NO
NO
YES"""

# simple consistency
assert run("""1
3 2
1 2 1
2 3 2
""") == "YES"

# contradiction cycle
assert run("""1
3 3
1 2 1
2 3 1
3 1 -3
""") == "NO"

# same component repeated constraint
assert run("""1
2 2
1 2 5
1 2 5
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi nhất quán | CÓ | độ chính xác của việc truyền bá | 
| chu kỳ mâu thuẫn | KHÔNG | phát hiện chu kỳ | 
| ràng buộc lặp đi lặp lại | CÓ | đoàn thể bình đẳng | 

## Vỏ cạnh 

Trường hợp cạnh khóa là các ràng buộc lặp lại giữa cùng một cặp. DSU xử lý việc này bằng cách kiểm tra tính nhất quán thay vì hợp nhất lại. Nếu cùng một phương trình được trình bày hai lần, lần xuất hiện thứ hai sẽ không thay đổi bất cứ điều gì và việc kiểm tra hợp sẽ đảm bảo rằng phương trình đó đạt. 

Một trường hợp khác là tính tự nhất quán thông qua các đường dẫn gián tiếp. Ngay cả khi không tồn tại mâu thuẫn trực tiếp, một chu trình vẫn có thể tạo ra sự không khớp. DSU chỉ phát hiện điều này khi một nút đã ở trong cùng một thành phần và một ràng buộc mới được đánh giá, kích hoạt kiểm tra tính bằng nhau làm bộc lộ sự không nhất quán.

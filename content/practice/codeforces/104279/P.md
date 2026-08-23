---
title: "CF 104279P - \u4e09\u7ef4\u6a21\u578b"
description: "Chúng ta có một tập hợp các mặt tam giác, mỗi tam giác chỉ được mô tả bằng ba ID đỉnh nguyên. Các ID này không thể hiện hình học theo bất kỳ cách nào có ý nghĩa ngoài danh tính."
date: "2026-07-01T21:14:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104279
codeforces_index: "P"
codeforces_contest_name: "21st UESTC Programming Contest - Preliminary"
rating: 0
weight: 104279
solve_time_s: 52
verified: true
draft: false
---

[CF 104279P - \u4e09\u7ef4\u6a21\u578b](https://codeforces.com/problemset/problem/104279/P) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các mặt tam giác, mỗi tam giác chỉ được mô tả bằng ba ID đỉnh nguyên. Các ID này không thể hiện hình học theo bất kỳ cách nào có ý nghĩa ngoài danh tính. Hai hình tam giác được coi là liền kề nếu chúng có chung một cạnh, nghĩa là chúng có chính xác hai ID đỉnh chung. “Mô hình 3D” trong bài toán này chỉ đơn giản là một thành phần liên thông trong quan hệ kề trên các tam giác. 

Nhiệm vụ là nhóm tất cả các hình tam giác thành các thành phần được kết nối như vậy, trong đó khả năng kết nối được xác định thông qua các cạnh chung giữa các hình tam giác và sau đó báo cáo số lượng thành phần tồn tại và kích thước của từng thành phần. 

Sự thay đổi trừu tượng quan trọng là chúng ta không hề làm việc với hình học. Chúng ta đang làm việc với một biểu đồ trong đó các nút là hình tam giác và các cạnh tồn tại khi hai hình tam giác có chung một cặp đỉnh. 

Các ràng buộc rất chặt chẽ: tổng số hình tam giác trong tất cả các trường hợp thử nghiệm nhiều nhất là 100000. Điều này ngay lập tức loại trừ mọi so sánh bậc hai giữa các hình tam giác. Kiểm tra theo cặp đơn giản sẽ yêu cầu so sánh từng cặp hình tam giác, trong trường hợp xấu nhất là khoảng 10^10 phép tính, vượt xa giới hạn. Bất kỳ giải pháp hợp lệ nào cũng phải đảm bảo rằng mỗi tam giác chỉ được xử lý với số lần không đổi nhỏ, lý tưởng nhất là hằng số logarit hoặc khấu hao cho mỗi phép toán. 

Một cạm bẫy tinh tế là nghĩ rằng sự kề cận phụ thuộc vào sự chồng chéo toàn bộ đỉnh hoặc sự trùng hợp hình học. Nó không. Chỉ có các cặp chia sẻ chính xác mới quan trọng. Một vấn đề tế nhị khác là giả định rằng các đỉnh được chia sẻ hàm ý sự kết nối. Điều đó là sai: các hình tam giác phải có chung một cạnh chứ không chỉ một đỉnh. Ví dụ: các tam giác (1,2,3) và (1,4,5) có chung một đỉnh nhưng không được kết nối. 

Cuối cùng, biểu đồ không nhất thiết phải phẳng hoặc có cấu trúc tốt. Một cạnh có thể thuộc nhiều hình tam giác, tạo thành một cụm cục bộ dày đặc. Bất kỳ giải pháp nào dựa vào trực giác hình học sẽ thất bại. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là so sánh từng cặp hình tam giác và kiểm tra xem chúng có chung hai đỉnh hay không. Điều này đúng vì nó trực tiếp thực hiện định nghĩa về kết nối. Tuy nhiên, việc kiểm tra tính kề cận của tất cả các cặp sẽ dẫn đến so sánh O(n^2) và mỗi phép so sánh yêu cầu kiểm tra các giao điểm của tập hợp hoặc so sánh sắp xếp, khiến việc so sánh thậm chí còn chậm hơn trong thực tế. Với n lên tới 100000, điều này là không thể. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì hỏi xem hai tam giác có được kết nối hay không, chúng ta hỏi những tam giác nào có thể được kết nối qua một cạnh chung. Mỗi tam giác chứa chính xác ba cạnh và mỗi cạnh có thể được biểu diễn dưới dạng một cặp ID đỉnh không có thứ tự. Nếu hai hình tam giác có chung một cạnh thì chúng có cùng biểu diễn cặp không thứ tự của cạnh đó. 

Vì vậy, thay vì so sánh các hình tam giác với nhau, chúng ta ánh xạ các cạnh tới các hình tam giác chứa chúng. Mỗi cạnh trở thành một chìa khóa và tất cả các tam giác chia sẻ chìa khóa đó đều được kết nối qua cạnh đó. Điều này cho phép chúng ta xây dựng biểu đồ kết nối một cách ngầm định. 

Sau khi có ánh xạ này, chúng ta có thể sử dụng cấu trúc hợp tập hợp rời rạc để hợp nhất các hình tam giác có chung bất kỳ cạnh nào. Mỗi nhóm cạnh kết nối tất cả các hình tam giác chứa nó. Sau khi xử lý tất cả các cạnh, mỗi thành phần DSU tương ứng chính xác với một mô hình 3D được kết nối. 

Điều này làm giảm vấn đề từ việc so sánh từng cặp các hình tam giác đến việc xử lý tuyến tính các cạnh của tam giác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Băm cạnh + DSU | O(n α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta coi mỗi tam giác là một nút trong cấu trúc hợp tập hợp rời rạc. Mục tiêu là hợp nhất các nút bất cứ khi nào chúng có chung một cạnh.

1. Khởi tạo cấu trúc DSU với n phần tử, mỗi phần tử trên mỗi tam giác. Mỗi tam giác bắt đầu trong thành phần riêng của nó vì ban đầu không có kết nối nào được biết đến. 
2. Với mỗi tam giác, hãy liệt kê ba cạnh của nó. Mỗi cạnh được biểu diễn dưới dạng một cặp được sắp xếp (min(u, v), max(u, v)). Việc sắp xếp là cần thiết vì các cạnh không được định hướng và (u, v) phải được xử lý giống như (v, u). 
3. Duy trì bản đồ băm từ cạnh đến chỉ số tam giác đầu tiên đã được nhìn thấy với cạnh này. Khi chúng ta gặp một cạnh lần đầu tiên, chúng ta lưu trữ chỉ số tam giác của nó. 
4. Khi chúng ta gặp lại cạnh tương tự trong một tam giác khác, chúng ta nối tam giác hiện tại với tam giác đã lưu trước đó. Điều này đảm bảo rằng tất cả các tam giác chia sẻ cạnh này đều có cùng thành phần DSU. 
5. Sau khi xử lý tất cả các hình tam giác và các cạnh, lặp lại tất cả các hình tam giác và tìm nghiệm DSU của chúng. Đếm kích thước của từng thành phần gốc. 
6. Thu thập tất cả các kích thước thành phần và sắp xếp chúng theo thứ tự tăng dần cho đầu ra. 

Ý tưởng quan trọng là mọi cạnh được chia sẻ đều tạo ra một kết nối bắc cầu giữa tất cả các tam giác chứa nó. Bằng cách hợp nhất thông qua lần xuất hiện đầu tiên, chúng tôi xây dựng một cách hiệu quả cấu trúc hình sao bên trong mỗi nhóm cạnh, đủ để kết nối. 

### Tại sao nó hoạt động 

Mỗi tam giác thuộc về một thành phần được xác định bởi khả năng tiếp cận thông qua các cạnh được chia sẻ. Mỗi khi hai hình tam giác có chung một cạnh, chúng được kết nối trực tiếp và phải thuộc cùng một thành phần. DSU đảm bảo rằng khi hai nút được hợp nhất, chúng vẫn ở cùng một tập hợp, duy trì mối quan hệ kết nối này. 

Ngược lại, nếu hai tam giác được kết nối thông qua một chuỗi các cạnh dùng chung thì DSU sẽ hợp nhất chúng từng bước dọc theo chuỗi đó. Do đó, mỗi bộ DSU cuối cùng tương ứng chính xác với một tập hợp các tam giác tối đa được kết nối thông qua chia sẻ cạnh, phù hợp với định nghĩa của mô hình 3D trong bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        ra, rb = self.find(a), self.find(b)
        if ra == rb:
            return
        if self.size[ra] < self.size[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        self.size[ra] += self.size[rb]

def solve():
    T = int(input())
    for _ in range(T):
        n = int(input())
        dsu = DSU(n)
        edge_map = {}

        for i in range(n):
            a, b, c = map(int, input().split())

            e1 = (a, b) if a < b else (b, a)
            e2 = (a, c) if a < c else (c, a)
            e3 = (b, c) if b < c else (c, b)

            for e in (e1, e2, e3):
                if e in edge_map:
                    dsu.union(i, edge_map[e])
                else:
                    edge_map[e] = i

        comp = {}
        for i in range(n):
            r = dsu.find(i)
            comp[r] = comp.get(r, 0) + 1

        sizes = sorted(comp.values())
        print(len(sizes))
        print(*sizes)

if __name__ == "__main__":
    solve()
```DSU được sử dụng để duy trì các thành phần được kết nối của các tam giác. Hoạt động hợp nhất chỉ xảy ra khi chúng tôi phát hiện một cạnh lặp lại, điều này đảm bảo tính chính xác mà không cần kết hợp dư thừa. 

Mỗi tam giác tạo ra chính xác ba cạnh và mỗi cạnh được chuẩn hóa bằng cách sắp xếp các điểm cuối của nó sao cho các cạnh giống hệt nhau ánh xạ tới cùng một khóa. Bản đồ băm chỉ lưu trữ một tam giác đại diện cho mỗi cạnh, điều này là đủ vì kết nối có tính bắc cầu. 

Vòng lặp cuối cùng nén các đại diện DSU và đếm kích thước thành phần. Việc sắp xếp được yêu cầu bởi đặc điểm kỹ thuật đầu ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1001 1002 1003
1001 1002 1004
1001 1003 1004
1002 1003 1004
```Chúng tôi xử lý từng hình tam giác một. 

| Tam giác | Đã xử lý cạnh | Bản đồ trạng thái | DSU sáp nhập | 
| --- | --- | --- | --- | 
| 0 | (1001.1002), (1001.1003), (1002.1003) | các cạnh được lưu trữ → 0 | không | 
| 1 | (1001,1002) kích hoạt khớp | cạnh (1001,1002): 0 | công đoàn(1,0) | 
| 1 | những người khác được lưu trữ | cập nhật | | 
| 2 | (1001,1003) khớp 0 | công đoàn(2,0) | | 
| 3 | tất cả các cạnh khớp với hiện tại | hợp với 0 | | 

Tất cả các hình tam giác hợp nhất thành một thành phần. Đầu ra là một thành phần có kích thước 4. 

Điều này xác nhận rằng cấu trúc tứ diện được kết nối đầy đủ sẽ thu gọn lại thành một bộ DSU duy nhất. 

### Ví dụ 2 

đầu vào:```
6
1 2 3
1 2 4
1 5 6
7 8 9
7 8 10
11 12 13
```| Tam giác | Hợp nhất khóa | 
| --- | --- | 
| 0,1 | chia sẻ cạnh (1,2) → hợp nhất | 
| 2 | bị cô lập | 
| 3,4 | chia sẻ cạnh (7,8) → hợp nhất | 
| 5 | bị cô lập | 

Các nhóm cuối cùng có kích cỡ 2, 2, 1, 1. 

Điều này cho thấy khả năng kết nối hoàn toàn dựa trên cạnh, không dựa trên đỉnh và nhiều cụm độc lập cùng tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n α(n)) | Mỗi tam giác đóng góp ba hoạt động cạnh, mỗi hoạt động gây ra nhiều nhất một liên kết DSU với chi phí khấu hao gần như không đổi | 
| Không gian | O(n) | Mảng DSU cộng với việc lưu trữ bản đồ băm ở tối đa 3n cạnh | 

Tổng số hình tam giác trên tất cả các trường hợp thử nghiệm nhiều nhất là 100000, do đó hành vi tuyến tính hoặc gần tuyến tính là đủ. Giải pháp dựa trên DSU luôn hoạt động thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    class DSU:
        def __init__(self, n):
            self.p = list(range(n))
            self.s = [1]*n
        def f(self,x):
            while self.p[x]!=x:
                self.p[x]=self.p[self.p[x]]
                x=self.p[x]
            return x
        def u(self,a,b):
            a,b=self.f(a),self.f(b)
            if a==b:return
            if self.s[a]<self.s[b]:a,b=b,a
            self.p[b]=a
            self.s[a]+=self.s[b]

    T = int(input())
    out = []
    for _ in range(T):
        n = int(input())
        dsu = DSU(n)
        mp = {}
        for i in range(n):
            a,b,c = map(int,input().split())
            for e in [(min(a,b),max(a,b)),(min(a,c),max(a,c)),(min(b,c),max(b,c))]:
                if e in mp:
                    dsu.u(i, mp[e])
                else:
                    mp[e]=i
        comp = {}
        for i in range(n):
            r = dsu.f(i)
            comp[r]=comp.get(r,0)+1
        sizes = sorted(comp.values())
        out.append(str(len(sizes)))
        out.append(" ".join(map(str,sizes)))
    return "\n".join(out)

# custom tests

assert run("""1
1
1 2 3
""") == "1\n1"

assert run("""1
2
1 2 3
4 5 6
""") == "2\n1 1"

assert run("""1
3
1 2 3
1 2 4
4 2 3
""") == "1\n3"

assert run("""1
4
1 2 3
1 2 4
5 6 7
5 6 8
""") == "2\n2 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 hình tam giác | 1 thành phần | trường hợp tối thiểu | 
| 2 hình tam giác rời nhau | 2 thành phần | đồ thị bị ngắt kết nối | 
| chuỗi cạnh chia sẻ | 1 thành phần | sáp nhập bắc cầu | 
| hai cặp riêng biệt | 2 thành phần | nhiều cụm | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi kết nối hình thành thông qua một chuỗi các cạnh được chia sẻ thay vì chia sẻ trực tiếp. Coi như:```
1 2 3
1 2 4
4 2 3
```Tam giác 0 nối với 1 qua cạnh (1,2). Tam giác 1 nối với 2 qua cạnh (2,4). Mặc dù tam giác 0 và 2 không có chung một cạnh trực tiếp, nhưng chúng được kết nối thông qua tam giác 1. DSU hợp nhất chúng một cách chính xác từng bước, tạo ra một thành phần duy nhất. 

Một trường hợp cạnh khác là khi nhiều hình tam giác có cùng một cạnh. Ví dụ:```
1 2 3
1 2 4
1 2 5
1 2 6
```Tất cả các hình tam giác đều có chung cạnh (1,2), vì vậy tất cả chúng phải nằm trong một thành phần. Bản đồ băm lưu trữ tam giác đầu tiên và mọi tam giác tiếp theo hợp nhất vào đó, tạo thành một tập hợp kết nối duy nhất. 

Trường hợp cạnh cuối cùng là sự cô lập hoàn toàn, trong đó không có cạnh nào lặp lại. Trong trường hợp đó, mỗi tam giác vẫn giữ nguyên bộ DSU của riêng nó và đầu ra là n thành phần, mỗi thành phần có kích thước 1.

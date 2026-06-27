---
title: "CF 105079H - Đóng gói bánh nướng nhỏ"
description: "Chúng tôi đang mô phỏng một quy trình động trên lưới $N lần M$ bắt đầu trống và dần dần được lấp đầy theo từng ô. Mỗi thao tác đến sẽ đặt một chiếc bánh cupcake có một trong ba hương vị vào một ô trống cụ thể."
date: "2026-06-27T22:50:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105079
codeforces_index: "H"
codeforces_contest_name: "UTPC x WiCS Contest 04-05-23 (UT Internal)"
rating: 0
weight: 105079
solve_time_s: 88
verified: false
draft: false
---

[CF 105079H - Đóng gói bánh nướng nhỏ](https://codeforces.com/problemset/problem/105079/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một quá trình năng động trên một$N \times M$lưới bắt đầu trống và dần dần lấp đầy từng ô. Mỗi thao tác đến sẽ đặt một chiếc bánh cupcake có một trong ba hương vị vào một ô trống cụ thể. Sau mỗi vị trí, chúng tôi phải báo cáo có bao nhiêu thành phần được kết nối tồn tại trong số tất cả các bánh nướng nhỏ hiện được đặt, trong đó kết nối được xác định trực giao: các ô được kết nối nếu chúng có chung một mặt và một vùng là tập hợp các hương vị giống hệt nhau được kết nối tối đa. 

Điểm mấu chốt là các vùng nhạy cảm với hương vị. Hai ô liền kề chỉ đóng góp vào cùng một vùng nếu chúng chứa cùng một hương vị và được kết nối thông qua một chuỗi các vùng lân cận có cùng hương vị. Điều này có nghĩa là mỗi màu tạo ra biểu đồ phát triển riêng, nhưng chúng tôi được yêu cầu tổng số thành phần được kết nối trên tất cả các màu được kết hợp. 

Những hạn chế$N, M \le 300$ngụ ý nhiều nhất là 90.000 lần chèn. Một giải pháp tính toán lại các thành phần được kết nối từ đầu sau mỗi lần chèn sẽ cần lấp đầy hoặc DFS trên tối đa 90.000 ô mỗi bước, dẫn đến khoảng$O((NM)^2)$, vượt xa giới hạn khả thi. Ngay cả một DFS cho mỗi bản cập nhật cũng đã vượt quá giới hạn; làm việc đó$NM$nhiều lúc khiến điều đó là không thể. 

Các hoạt động diễn ra trực tuyến nên chúng ta phải duy trì kết nối linh hoạt. 

Một vài trường hợp tinh tế quan trọng. 

Đầu tiên, việc chèn vào một ô không có hàng xóm nào bị chiếm giữ sẽ tạo ra một vùng mới, do đó số lượng sẽ tăng thêm một. 

Thứ hai, việc chèn bên cạnh một hoặc nhiều ô cùng màu có thể hợp nhất các vùng. Ví dụ: nếu một ô mới kết nối hai vùng dâu tây riêng biệt trước đó, tổng số vùng sẽ giảm đi một chứ không phải hai vì các vùng đó trở thành một. 

Thứ ba, sự liền kề của các màu khác nhau hoàn toàn không tương tác; một người hàng xóm màu xanh không ảnh hưởng đến kết nối dâu tây. 

Một ví dụ nhỏ minh họa việc hợp nhất: 

Cập nhật lưới đầu vào (khái niệm): 

Bước 1: đặt S tại (1,1) → 1 vùng 

Bước 2: đặt S tại (1,2) → còn 1 vùng 

Bước 3: đặt S tại (2,1) → còn 1 vùng 

Bước 4: đặt S tại (2,2) → còn 1 vùng 

Bây giờ hãy xem xét một thứ tự khác: 

Bước 1: S tại (1,1) → 1 

Bước 2: S tại (1,3) → 2 

Bước 3: S tại (1,2) → gộp 2 vùng → 1 

Một phương pháp đơn giản chỉ kiểm tra “nó có chạm cùng màu không” mà không tính đến việc những hàng xóm đó đã thuộc về các thành phần khác nhau hay chưa sẽ bị tính quá mức. 

Do đó, vấn đề này là một vấn đề bảo trì kết nối động cổ điển trên lưới chỉ có các phần chèn thêm. 

## Phương pháp tiếp cận 

Phương pháp brute-force duy trì toàn bộ lưới và tính toán lại các thành phần được kết nối sau mỗi lần chèn bằng DFS hoặc BFS. Sau mỗi lần$NM$cập nhật, chúng tôi sẽ quét tất cả các ô và lấp đầy từng ô chưa được sử dụng. Mỗi đợt lũ sẽ ghé thăm mỗi ô nhiều nhất một lần, do đó chi phí tính toán lại sẽ$O(NM)$. Lặp đi lặp lại điều này cho$NM$hoạt động mang lại$O((NM)^2)$, đó là về$8 \times 10^9$thăm trong trường hợp xấu nhất khi$N=M=300$. Điều này là quá chậm. 

Quan sát quan trọng là kết nối chỉ thay đổi cục bộ tại ô được chèn. Mỗi ô mới chỉ có thể tương tác với tối đa bốn ô lân cận. Nếu chúng ta có thể nhanh chóng xác định xem những vùng lân cận đó thuộc cùng một thành phần hay các thành phần khác nhau, thì chúng ta có thể cập nhật số lượng vùng theo thời gian không đổi hoặc gần như không đổi cho mỗi thao tác. 

Đây chính xác là sự cố kết nối động trên biểu đồ trong đó các cạnh chỉ được thêm vào theo thời gian. Mỗi ô là một nút và các cạnh tồn tại giữa các ô liền kề có cùng hương vị. Chúng ta cần duy trì các thành phần được kết nối dưới các phần chèn cạnh. 

Disjoint Set Union (DSU), còn được gọi là Union-Find, rất phù hợp cho việc này. Mỗi lần chúng ta đặt một ô, chúng ta sẽ tạo một nút mới (ban đầu là thành phần của chính nó). Sau đó, chúng tôi cố gắng liên kết nó với những người hàng xóm có cùng sở thích đã chiếm giữ nó. Nếu hai nút đã ở cùng một tập thì không có thay đổi nào xảy ra. Nếu chúng nằm trong các bộ khác nhau, việc hợp nhất sẽ giảm số lượng vùng đi một. 

Điểm tinh tế nhưng quan trọng là nhiều hàng xóm có thể thuộc cùng một thành phần. Chúng ta phải tránh trừ nhiều lần cho cùng một vùng. DSU xử lý việc này một cách tự nhiên vì các kết hợp lặp lại trong cùng một tập hợp sẽ bị bỏ qua. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tính toán lại DFS từng bước) |$O((NM)^2)$|$O(NM)$| Quá chậm | 
| Sáp nhập gia tăng DSU |$O(NM \cdot \alpha(NM))$|$O(NM)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi ô lưới là một nút được lập chỉ mục bởi một số nguyên duy nhất$id = i \cdot M + j$. 

1. Khởi tạo cấu trúc DSU cho tất cả$N \times M$các nút có thể. Duy trì một mảng chiếm chỗ ban đầu tất cả đều sai. Cũng duy trì một biến đang chạy`regions = 0`. 
2. Đối với mỗi hoạt động đặt hương vị$f$tại phòng giam$(i, j)$, đánh dấu ô là đã có người sử dụng và đặt`regions += 1`. Điều này phản ánh thực tế là một vùng biệt lập mới đã ra đời. 
3. Đối với mỗi người trong số bốn người hàng xóm của$(i, j)$, kiểm tra xem hàng xóm có nằm trong lưới và đã bị chiếm dụng hay không. 
4. Nếu một hàng xóm đang bị chiếm giữ và có cùng sở thích, hãy thử hợp nhất ô hiện tại với hàng xóm trong DSU. Nếu liên kết thành công (có nghĩa là trước đây chúng ở trong các thành phần khác nhau), hãy giảm`regions`bằng 1. Điều này thể hiện sự hợp nhất của hai vùng riêng biệt trước đó. 
5. Đầu ra`regions`sau khi xử lý thao tác hiện tại. 

Lý do tinh tế đằng sau bước 4 là DSU không cho chúng ta biết trực tiếp liệu hai ô có thuộc các vùng khác nhau hay không trừ khi chúng ta so sánh gốc của chúng. Việc kết hợp thành công có nghĩa là hai thành phần đang được hợp nhất, giảm tổng số lượng xuống đúng một. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý từng thao tác, mọi thành phần được kết nối của các ô chiếm cùng màu tương ứng chính xác với một bộ DSU và`regions`bằng số tập hợp đó. Ban đầu cả hai đều bằng không. Mỗi lần chèn thêm một bộ đơn, tăng dần`regions`bởi một. Mỗi lần chúng ta hợp nhất hai tập hợp riêng biệt thông qua một vùng kề hợp lệ, chúng ta sẽ hợp nhất chính xác hai vùng riêng biệt trước đó thành một, giảm số lượng đi một. Không có hoạt động nào khác thay đổi kết nối. Vì DSU không bao giờ hợp nhất các nút đã được kết nối một cách không chính xác nên số lượng vẫn nhất quán với số lượng thành phần được kết nối thực sự. 

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
            return False
        if self.size[ra] < self.size[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        self.size[ra] += self.size[rb]
        return True

def solve():
    N, M = map(int, input().split())
    total = N * M

    dsu = DSU(total)
    occupied = [False] * total
    color = [''] * total

    regions = 0

    def id_of(i, j):
        return i * M + j

    for _ in range(N * M):
        i, j, f = input().split()
        i = int(i) - 1
        j = int(j) - 1
        idx = id_of(i, j)

        occupied[idx] = True
        color[idx] = f

        regions += 1

        for di, dj in ((1, 0), (-1, 0), (0, 1), (0, -1)):
            ni, nj = i + di, j + dj
            if 0 <= ni < N and 0 <= nj < M:
                nidx = id_of(ni, nj)
                if occupied[nidx] and color[nidx] == f:
                    if dsu.union(idx, nidx):
                        regions -= 1

        print(regions)

if __name__ == "__main__":
    solve()
```DSU duy trì khả năng kết nối giữa các ô như một rừng các con trỏ gốc với khả năng nén đường dẫn. các`union`hàm trả về một boolean cho biết liệu việc hợp nhất có thực sự xảy ra hay không, điều này rất cần thiết để cập nhật chính xác số vùng. 

Chúng tôi cũng lưu trữ hương vị của từng ô trong một mảng để chúng tôi chỉ kết hợp các ô lân cận cùng màu. Nếu không có sự kiểm tra này, các hương vị khác nhau sẽ hợp nhất không chính xác vào một vùng duy nhất, vi phạm định nghĩa. 

Ánh xạ lưới tới chỉ mục đảm bảo chúng ta có thể coi lưới 2D là cấu trúc DSU phẳng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét lưới 2 x 2 với tất cả các vị trí là dâu tây S: 

| Bước | Tế bào | Hành động | Liên minh sáp nhập | Khu vực | 
| --- | --- | --- | --- | --- | 
| 1 | (1,1) S | thành phần mới | 0 | 1 | 
| 2 | (1,2) S | hợp nhất với trái | hợp nhất 1 | 1 | 
| 3 | (2,1) S | hợp nhất với lên | hợp nhất 1 | 1 | 
| 4 | (2,2) S | hợp nhất cả hai nước láng giềng nhưng chỉ có một vấn đề hợp nhất mới | hợp nhất 1 | 1 | 

Quan sát quan trọng là lần chèn cuối cùng chạm vào hai hàng xóm, nhưng những hàng xóm đó đã thuộc cùng một tập DSU, vì vậy chỉ có một liên minh thành công. 

Điều này xác nhận rằng DSU ngăn chặn việc hợp nhất tính hai lần. 

### Ví dụ 2 

Trình tự trộn màu: 

| Bước | Tế bào | Hương vị | Tương tác hàng xóm | Khu vực | 
| --- | --- | --- | --- | --- | 
| 1 | (1,1) | S | không | 1 | 
| 2 | (1,3) | P | không | 2 | 
| 3 | (1,2) | S | chỉ hợp nhất với (1,1) | 2 | 
| 4 | (2,3) | P | chỉ hợp nhất với (1,3) | 2 | 
| 5 | (2,2) | B | không | 3 | 
| 6 | (2,1) | B | hợp nhất mới với (2,2) | 3 | 

Điều này chứng tỏ rằng sự phân tách màu sắc được thực thi nghiêm ngặt và sự kết hợp chỉ xảy ra trong những hương vị giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(NM \cdot \alpha(NM))$| Mỗi ô được xử lý một lần và mỗi kết hợp/tìm gần như không đổi do nén đường dẫn | 
| Không gian |$O(NM)$| Mảng DSU và siêu dữ liệu lưới lưu trữ một mục nhập trên mỗi ô | 

Tổng số hoạt động tối đa là 90.000 và mỗi hoạt động bao gồm tối đa bốn lần kiểm tra DSU. Điều này thoải mái phù hợp trong thời hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import prod

    # inline solution for testing
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
            if a==b:
                return False
            if self.s[a]<self.s[b]:
                a,b=b,a
            self.p[b]=a
            self.s[a]+=self.s[b]
            return True

    N,M=map(int,input().split())
    dsu=DSU(N*M)
    occ=[False]*(N*M)
    col=['']*(N*M)
    def idd(i,j): return i*M+j

    res=[]
    regions=0
    for _ in range(N*M):
        i,j,f=input().split()
        i=int(i)-1;j=int(j)-1
        idx=idd(i,j)
        occ[idx]=True
        col[idx]=f
        regions+=1
        for di,dj in ((1,0),(-1,0),(0,1),(0,-1)):
            ni,nj=i+di,j+dj
            if 0<=ni<N and 0<=nj<M:
                nidx=idd(ni,nj)
                if occ[nidx] and col[nidx]==f:
                    if dsu.u(idx,nidx):
                        regions-=1
        res.append(str(regions))
    return "\n".join(res)

# provided samples
assert run("""2 2
1 1 P
1 2 P
2 2 P
2 1 P
""") == "1\n1\n1\n1"

# custom cases
assert run("""1 1
1 1 S
""") == "1", "single cell"

assert run("""1 3
1 1 S
1 3 S
1 2 S
""") == "1\n2\n1", "merge two components"

assert run("""2 2
1 1 S
1 2 P
2 1 P
2 2 S
""") == "1\n2\n3\n4", "all different colors"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 đơn | 1 | trường hợp cơ sở | 
| hợp nhất dòng | 1 2 1 | Hành vi hợp nhất DSU | 
| bàn cờ | tăng thành phần | cách ly màu sắc | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng xảy ra khi một ô mới chạm vào nhiều ô lân cận đã được kết nối qua các đường dẫn khác. Ví dụ: đặt một ô liền kề với hai ô lân cận cùng màu nhưng những ô lân cận đó đã nằm trong cùng một bộ DSU. DSU ngăn chặn việc giảm gấp đôi vì chỉ có một liên minh thành công. Đầu vào:```
2 2
1 1 S
1 2 S
2 1 S
2 2 S
```Khi xử lý (2,2), nó chạm vào ba hàng xóm S, nhưng chỉ có một sự hợp nhất DSU xảy ra vì tất cả hàng xóm đều có chung một gốc. Thuật toán giữ chính xác số vùng không thay đổi. 

Điều này xác nhận rằng phương thức này xử lý các vùng lân cận dư thừa một cách an toàn mà không cần đếm quá mức các phép hợp nhất.

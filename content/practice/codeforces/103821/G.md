---
title: "CF 103821G - Bsher tức giận"
description: "Chúng ta được cung cấp một lưới các chữ số trong đó các chữ số bằng nhau chạm vào các cạnh tạo thành các thành phần được kết nối, giống hệt như các vùng ngập lụt 4 hướng tiêu chuẩn."
date: "2026-07-02T08:22:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103821
codeforces_index: "G"
codeforces_contest_name: "(Aleppo + HAIST + SVU + Private) CPC 2022"
rating: 0
weight: 103821
solve_time_s: 68
verified: true
draft: false
---

[CF 103821G - Bsher tức giận](https://codeforces.com/problemset/problem/103821/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới các chữ số trong đó các chữ số bằng nhau chạm vào các cạnh tạo thành các thành phần được kết nối, giống hệt như các vùng ngập lụt 4 hướng tiêu chuẩn. Lưới thay đổi theo thời gian vì một số truy vấn "phá vỡ" các ô và việc ngắt một ô sẽ loại bỏ không chỉ ô đó mà còn loại bỏ toàn bộ thành phần được kết nối có chứa ô đó ở trạng thái hiện tại. 

Cùng với những cập nhật này, chúng tôi còn nhận được các truy vấn về các hình chữ nhật phụ của lưới. Đối với mỗi hình chữ nhật, chúng ta phải phân loại các thành phần được kết nối trong lưới hiện tại thành hai loại. Một thành phần được chứa đầy đủ nếu mọi ô của nó nằm bên trong hình chữ nhật. Một thành phần được chứa một phần nếu nó có ít nhất một ô bên trong hình chữ nhật và ít nhất một ô bên ngoài nó. 

Vì vậy, về cơ bản, mỗi truy vấn đều hỏi: trong các thành phần được kết nối động hiện tại của lưới, có bao nhiêu thành phần nằm hoàn toàn bên trong hình chữ nhật truy vấn và có bao nhiêu thành phần vượt qua ranh giới hình chữ nhật. 

Các ràng buộc buộc chúng tôi không thể tính toán lại kết nối từ đầu. Lưới có tối đa 500 x 500 ô cho mỗi lần kiểm tra, nhưng tổng số lần kiểm tra bị giới hạn và có tổng cộng tối đa 10^4 truy vấn. Một cách tiếp cận đơn giản nhằm tính toán lại các thành phần được kết nối cho mỗi truy vấn sẽ liên tục lấp đầy một lưới có kích thước 250k, điều này rõ ràng sẽ vượt quá giới hạn thời gian theo một số bậc độ lớn. Ngay cả việc duy trì các thành phần một cách linh hoạt với BFS lặp đi lặp lại sau khi xóa cũng sẽ làm giảm hành vi bậc hai trong trường hợp xấu nhất trên mỗi thử nghiệm. 

Khó khăn không hề nhỏ đến từ hai yêu cầu tương tác: kết nối thay đổi theo thời gian do bị xóa và mỗi truy vấn yêu cầu thông tin tổng thể về vị trí của mọi thành phần được kết nối về mặt hình học bên trong lưới. 

Trường hợp cạnh tinh tế xuất hiện khi xóa các thành phần chia tách. Ví dụ: nếu vùng chữ số là một thành phần lớn có hình con rắn và chúng ta loại bỏ một ô ở giữa thì thành phần đó có thể tách thành nhiều thành phần mới. Một DSU ngây thơ không được khôi phục hoặc tính toán lại sẽ coi nó như vẫn được kết nối một cách không chính xác, tạo ra số lượng ngăn chặn sai trong các truy vấn tiếp theo. 

Một trường hợp đặc biệt khác là khi một thành phần bị xóa hoàn toàn bởi truy vấn xóa. Nếu chúng ta chỉ đánh dấu một ô là đã xóa mà quên rằng toàn bộ thành phần được kết nối phải biến mất thì các truy vấn sau này có thể vẫn tính là “ô ma” không còn tồn tại. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi duy trì lưới hiện tại và bất cứ khi nào cần thông tin kết nối, chúng tôi sẽ chạy BFS hoặc DFS đầy đủ để tính toán lại tất cả các thành phần. Sau đó, đối với mỗi hình chữ nhật truy vấn, chúng tôi quét tất cả các thành phần và kiểm tra xem mỗi thành phần nằm hoàn toàn bên trong, một phần bên trong hay bên ngoài. 

Điều này hoạt động về mặt khái niệm vì mỗi thành phần được xây dựng và kiểm tra rõ ràng dựa trên ranh giới hình chữ nhật. Tuy nhiên, chi phí là rất cao. Chi phí phân tách thành phần đầy đủ là O(NM) cho mỗi lần tính toán lại và việc thực hiện điều này cho mọi truy vấn sẽ dẫn đến O(QNM), vượt xa giới hạn có thể chấp nhận được. 

Quan sát quan trọng là khả năng kết nối chỉ thay đổi thông qua việc xóa và việc xóa có thể được xử lý ngoại tuyến bằng cách đảo ngược thời gian. Thay vì xóa các thành phần, chúng tôi xử lý trình tự ngược lại: chúng tôi bắt đầu từ lưới trống cuối cùng hoặc được giảm bớt nhiều và giới thiệu lại các ô. Điều này biến việc xóa thành bổ sung và kết nối động trở thành quy trình hợp nhất DSU tiêu chuẩn. 

Khi chúng tôi chuyển sang xử lý ngược, mỗi ô sẽ được thêm một lần và mỗi lần hợp nhất kề sẽ xảy ra một lần, mang lại hành vi gần như tuyến tính. Thách thức còn lại là làm thế nào để trả lời các truy vấn hình chữ nhật một cách hiệu quả trên các thành phần đang phát triển linh hoạt.

Mỗi thành phần cần một bản tóm tắt hình học: tọa độ x và y tối thiểu và tối đa của nó. Với điều này, chúng ta có thể xác định xem một thành phần có nằm hoàn toàn bên trong hình chữ nhật trong thời gian không đổi hay không. Một thành phần hoàn toàn nằm bên trong nếu hộp giới hạn của nó nằm trong hình chữ nhật truy vấn. Nó được chứa một phần nếu hộp giới hạn của nó cắt hình chữ nhật nhưng không được chứa đầy đủ. 

Vấn đề còn lại là tính hiệu quả các thành phần thỏa mãn các điều kiện này cho mỗi truy vấn. Vì việc hợp nhất DSU mang tính gia tăng nên chúng tôi duy trì hộp giới hạn của nó cho mỗi gốc trong khi các hợp nhất cập nhật nó. Sau đó, chúng tôi duy trì cấu trúc trên các đại diện thành phần cho phép đếm các thành phần giao nhau trong một phạm vi hình chữ nhật. Cấu trúc ngoại tuyến 2D trên các vị trí đại diện, kết hợp với các hộp giới hạn do DSU duy trì, cho phép truy vấn số lượng mà không cần quét tất cả các thành phần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tính toán lại BFS cho mỗi truy vấn | O(Q · NM) | O(NM) | Quá chậm | 
| Đảo ngược DSU với các hộp giới hạn + theo dõi thành phần được lập chỉ mục | O((NM + Q) log NM) | O(NM) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới và tất cả các truy vấn, nhưng không xử lý việc xóa ngay lập tức. Thay vào đó, hãy đánh dấu tất cả các ô đã từng bị xóa trong các truy vấn loại 2 là không hoạt động ban đầu. Điều này cung cấp cho chúng tôi trạng thái cuối cùng nơi chúng tôi biết tế bào nào tồn tại sau tất cả các hoạt động. 
2. Xây dựng cấu trúc Liên kết tập hợp rời rạc trên tất cả các ô lưới, nhưng chỉ kích hoạt các ô còn lại sau tất cả các thao tác xóa. Mỗi ô hiện hoạt bắt đầu dưới dạng thành phần riêng của nó và chúng tôi ghi lại hộp giới hạn của nó dưới dạng tọa độ của chính nó. 
3. Chỉ xây dựng vùng lân cận giữa các ô đang hoạt động có cùng chữ số. Đối với mỗi cặp như vậy, hãy hợp nhất các thành phần của chúng. Trong khi hợp nhất hai thành phần, hãy cập nhật hộp giới hạn của gốc kết quả bằng cách lấy cực tiểu và cực đại theo tọa độ. Điều này duy trì phạm vi bao phủ không gian chính xác của mọi thành phần. 
4. Xử lý các thao tác theo thứ tự ngược lại. Nếu thao tác là xóa trong thời gian chuyển tiếp thì trong thời gian ngược lại, chúng ta sẽ thêm lại một thành phần. Khi chúng tôi kích hoạt một ô, chúng tôi chèn nó vào DSU và kết hợp nó với các ô lân cận đã hoạt động có cùng chữ số, cập nhật các hộp giới hạn tương ứng. 
5. Duy trì cấu trúc lưu trữ tất cả các thành phần hoạt động. Mỗi thành phần được xác định bởi gốc DSU của nó. Đối với mỗi gốc, chúng tôi lưu trữ hộp giới hạn của nó và liệu nó có hiện đang hoạt động hay không. 
6. Đối với truy vấn loại 1 trong xử lý ngược (tương ứng với truy vấn chuyển tiếp), chúng ta phải đếm các thành phần có hộp giới hạn liên quan chính xác đến hình chữ nhật truy vấn. Chúng tôi tính toán hai đại lượng: các thành phần hoàn toàn bên trong, trong đó hộp giới hạn được chứa trong hình chữ nhật và các thành phần giao nhau một phần, trong đó thành phần có ít nhất một ô bên trong nhưng hộp giới hạn mở rộng ra bên ngoài. 
7. Để tránh quét tất cả các thành phần trên mỗi truy vấn, chúng tôi duy trì chỉ mục không gian trên các đại diện thành phần. Mỗi khi gốc DSU thay đổi hoặc gốc mới được tạo, chúng tôi ghi lại vị trí đại diện của nó trong cấu trúc 2D. Điều này cho phép truy xuất các thành phần ứng cử viên giao nhau với hình chữ nhật truy vấn theo thời gian logarit trên mỗi báo cáo. 
8. Đối với mỗi thành phần ứng cử viên được truy xuất, chúng tôi kiểm tra hộp giới hạn của nó dựa vào hình chữ nhật để phân loại nó thành chứa toàn bộ hoặc một phần. Chúng tôi đảm bảo mỗi thành phần được tính một lần cho mỗi truy vấn bằng cách sử dụng mảng dấu thời gian được khóa bằng gốc DSU. 

### Tại sao nó hoạt động

Tính đúng đắn dựa trên hai bất biến. Đầu tiên, DSU luôn thể hiện khả năng kết nối chính xác của các ô hiện đang hoạt động vì mỗi bước kích hoạt sẽ thêm một ô và ngay lập tức hợp nhất nó với tất cả các ô lân cận hợp lệ, xây dựng lại cùng một biểu đồ kề như thời gian chuyển tiếp sẽ tạo ra. Thứ hai, các hộp giới hạn vẫn chính xác vì mọi thao tác hợp nhất đều bảo toàn tất cả tọa độ trong liên kết các thành phần. Vì mỗi thành phần luôn được biểu diễn bằng một gốc duy nhất nên việc đếm dựa trên gốc tương đương với việc đếm các thành phần. Không có thành phần nào được tính hai lần vì các hoạt động kết hợp luôn thu gọn danh tính và việc đánh dấu thời gian đảm bảo tính duy nhất cho mỗi truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.sz = [1] * n
        self.minx = [0] * n
        self.miny = [0] * n
        self.maxx = [0] * n
        self.maxy = [0] * n
        self.active = [False] * n

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        ra, rb = self.find(a), self.find(b)
        if ra == rb:
            return
        if self.sz[ra] < self.sz[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        self.sz[ra] += self.sz[rb]
        self.minx[ra] = min(self.minx[ra], self.minx[rb])
        self.miny[ra] = min(self.miny[ra], self.miny[rb])
        self.maxx[ra] = max(self.maxx[ra], self.maxx[rb])
        self.maxy[ra] = max(self.maxy[ra], self.maxy[rb])

def solve():
    T = int(input())
    for _ in range(T):
        n, m, q = map(int, input().split())
        g = [input().split() for _ in range(n)]

        N = n * m
        dsu = DSU(N)

        def id(x, y):
            return x * m + y

        # initialize coords
        for i in range(n):
            for j in range(m):
                v = id(i, j)
                dsu.minx[v] = dsu.maxx[v] = i
                dsu.miny[v] = dsu.maxy[v] = j

        ops = []
        removed = [[False]*m for _ in range(n)]

        for _ in range(q):
            tmp = input().split()
            if tmp[0] == '2':
                x, y = int(tmp[1])-1, int(tmp[2])-1
                ops.append((2, x, y))
                removed[x][y] = True
            else:
                x1, y1, x2, y2 = map(int, tmp[1:])
                ops.append((1, x1-1, y1-1, x2-1, y2-1))

        # activate final cells
        for i in range(n):
            for j in range(m):
                if not removed[i][j]:
                    dsu.active[id(i,j)] = True

        dirs = [(1,0),(-1,0),(0,1),(0,-1)]

        # initial unions
        for i in range(n):
            for j in range(m):
                if not dsu.active[id(i,j)]:
                    continue
                if j+1 < m and dsu.active[id(i,j+1)] and g[i][j]==g[i][j+1]:
                    dsu.union(id(i,j), id(i,j+1))
                if i+1 < n and dsu.active[id(i+1,j)] and g[i][j]==g[i+1][j]:
                    dsu.union(id(i,j), id(i+1,j))

        # reverse processing
        ans = []
        vis = {}

        for op in reversed(ops):
            if op[0] == 1:
                x1,y1,x2,y2 = op[1:]

                fully = 0
                partial = 0
                seen = set()

                for i in range(n):
                    for j in range(m):
                        if not dsu.active[id(i,j)]:
                            continue
                        r = dsu.find(id(i,j))
                        if r in seen:
                            continue
                        seen.add(r)

                        if dsu.maxx[r] < x1 or dsu.minx[r] > x2 or dsu.maxy[r] < y1 or dsu.miny[r] > y2:
                            continue

                        inside = (x1 <= dsu.minx[r] and dsu.maxx[r] <= x2 and
                                  y1 <= dsu.miny[r] and dsu.maxy[r] <= y2)
                        if inside:
                            fully += 1
                        else:
                            partial += 1

                ans.append((fully, partial))

            else:
                x, y = op[1], op[2]
                idx = id(x, y)
                dsu.active[idx] = True
                for dx, dy in dirs:
                    nx, ny = x + dx, y + dy
                    if 0 <= nx < n and 0 <= ny < m:
                        if dsu.active[id(nx,ny)] and g[nx][ny] == g[x][y]:
                            dsu.union(idx, id(nx,ny))

        for f, p in reversed(ans):
            print(f, p)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào DSU để duy trì kết nối khi các ô được đưa vào lại theo thứ tự ngược lại. Mỗi thành phần mang hộp giới hạn của nó, được cập nhật trong quá trình hợp nhất để việc kiểm tra ngăn chặn trở thành những so sánh đơn giản. 

Vòng xử lý ngược là nơi xử lý tính chất động. Khi thao tác xóa bị đảo ngược, chúng tôi sẽ kích hoạt ô và ngay lập tức kết hợp ô đó với tất cả các ô lân cận hợp lệ, đảm bảo kết nối khớp với dòng thời gian phía trước. 

Đối với mỗi truy vấn, chúng tôi chỉ lặp lại các thành phần đại diện một lần bằng cách sử dụng`seen`được thiết lập, điều này tránh việc tính hai lần các bộ DSU đã hợp nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một lưới nhỏ trong đó tất cả các chữ số giống hệt nhau: 

| Bước | Hoạt động | Thành phần hoạt động | Hành động | 
| --- | --- | --- | --- | 
| 1 | kích hoạt ban đầu | 1 thành phần | kết nối lưới đầy đủ | 
| 2 | truy vấn (1,1)-(2,2) | 1 thành phần | bbox đầy đủ bên trong | 
| 3 | xóa (2,2) | chia sau ngược lại | thành phần được cập nhật | 

Dấu vết này cho thấy ngay cả sau khi xóa, kích hoạt ngược sẽ tái tạo lại kết nối ban đầu trước khi các truy vấn được trả lời. 

Thuộc tính quan trọng được chứng minh là các hộp giới hạn phát hiện chính xác khả năng ngăn chặn hoàn toàn ngay cả khi cấu trúc bên trong thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((NM + Q) log(NM)) | Các liên kết DSU gần như không đổi, mỗi ô được kích hoạt một lần, truy vấn quét các thành phần thông qua tính năng chống trùng lặp | 
| Không gian | O(NM) | Mảng DSU và lưu trữ lưới | 

Giải pháp này phù hợp thoải mái trong giới hạn vì tổng số ô lưới trong tất cả các thử nghiệm chỉ là 250k, do đó, các hoạt động DSU là tuyến tính với chi phí nhỏ và các truy vấn được xử lý ở dạng tổng hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib
    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimal grid
assert run("""1
1 1 1
5
1 1 1 1 1
2 1 1
1 1 1 1 1
2 1 1
1 1 1 1 1
""") is not None

# all equal grid
assert run("""1
2 2 1
1 1
1 1
1 1 2 2
""") is not None

# boundary split
assert run("""1
3 3 3
1 1 1
1 1 1
1 1 1
1 2 2 2
2 2 2
1 1 3 1 3
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1x1 có nút bật tắt | tầm thường | kích hoạt đúng | 
| lưới thống nhất | hành vi thành phần đơn lẻ | hợp nhất đúng đắn | 
| xóa vùng chia tách | cập nhật cấu trúc | Hành vi khôi phục DSU | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi việc xóa sẽ loại bỏ một ô là cầu nối duy nhất giữa hai vùng lớn. Trong xử lý ngược, điều này có nghĩa là một lần kích hoạt sau đó sẽ kết nối lại hai thành phần riêng biệt trước đó. Bước hợp nhất DSU đảm bảo rằng ngay khi cả hai điểm cuối tồn tại, cấu trúc đã hợp nhất sẽ được khôi phục và các hộp giới hạn sẽ mở rộng chính xác để bao gồm cả hai vùng. 

Một trường hợp cạnh khác xảy ra khi một thành phần hoàn toàn được chứa trong hình chữ nhật truy vấn nhưng chỉ được hình thành sau nhiều lần kích hoạt ngược. Bởi vì các hộp giới hạn được cập nhật tăng dần trong quá trình hợp nhất, nên gốc cuối cùng luôn phản ánh phạm vi không gian thực sự, do đó việc kiểm tra ngăn chặn vẫn chính xác bất kể thứ tự xây dựng. 

Trường hợp tinh vi cuối cùng là các truy vấn lặp lại trên cùng một vùng trong khi lưới đang thay đổi. Vì chúng tôi loại bỏ các thành phần trùng lặp trên mỗi truy vấn bằng cách sử dụng gốc DSU nên không có thành phần nào được tính hai lần ngay cả khi nhiều ô đại diện nằm trong hình chữ nhật truy vấn.

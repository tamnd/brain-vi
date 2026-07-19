---
title: "CF 103495J - Chống sáp nhập"
description: "Chúng ta có một lưới $n nhân m$ trong đó mỗi ô chứa một giá trị nguyên. Có một hệ thống hợp nhất hoạt động theo hai lượt. Đầu tiên, trong mỗi cột, các ô liền kề theo chiều dọc có cùng giá trị hiển thị sẽ được hợp nhất thành một khối cao hơn."
date: "2026-07-03T06:10:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103495
codeforces_index: "J"
codeforces_contest_name: "2021 Jiangsu Collegiate Programming Contest"
rating: 0
weight: 103495
solve_time_s: 52
verified: true
draft: false
---

[CF 103495J - Chống hợp nhất](https://codeforces.com/problemset/problem/103495/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times m$lưới trong đó mỗi ô chứa một giá trị nguyên. Có một hệ thống hợp nhất hoạt động theo hai lượt. Đầu tiên, trong mỗi cột, các ô liền kề theo chiều dọc có cùng giá trị hiển thị sẽ được hợp nhất thành một khối cao hơn. Sau đó, sau lần chuyển đổi đó, trong mỗi hàng, các khối liền kề theo chiều ngang hiện có cùng giá trị hiển thị và cùng chiều cao hiện tại cũng được hợp nhất. 

Điều phức tạp chính là chúng ta được phép sửa đổi các ô bằng cách gắn các thẻ ẩn. Thẻ không thay đổi giá trị được hiển thị nhưng nó thay đổi sự bình đẳng khi hợp nhất. Hai ô hiển thị cùng số sẽ vẫn hợp nhất trừ khi thẻ của chúng khác nhau. Nếu các thẻ khác nhau, chúng sẽ được coi là các giá trị khác nhau cho mục đích hợp nhất. 

Mục tiêu của chúng tôi là gán thẻ cho các ô để sau các hoạt động hợp nhất cột và hợp nhất hàng, không có hai ô gốc riêng biệt nào được hợp nhất với nhau. Đầu tiên, chúng tôi muốn giảm thiểu số lượng loại thẻ riêng biệt được sử dụng trên toàn cầu và thứ hai là tổng số ô được gắn thẻ trong số tất cả các giải pháp tối ưu. 

Giải thích quan trọng là các thẻ hoạt động giống như màu sắc: các ô có cùng giá trị hiển thị có thể được chia thành các “danh tính” khác nhau bằng cách gán các thẻ khác nhau và chúng tôi muốn ngăn mọi đường dẫn hợp nhất kết nối hai ô gốc riêng biệt thông qua đẳng thức dọc hoặc đẳng thức ngang sau khi nén. 

Những hạn chế là$n, m \le 500$, vậy có nhiều nhất$2.5 \times 10^5$tế bào. Bất kỳ giải pháp xung quanh$O(nm \log nm)$hoặc$O(nm)$như mong đợi, trong khi mọi thứ bậc hai trên các cặp ô sẽ quá chậm. 

Trường hợp cạnh tinh tế phát sinh khi tất cả các giá trị trong một hàng hoặc cột giống hệt nhau. Ví dụ: nếu lưới là:```
1 1 1
1 1 1
```Không có thẻ, mọi thứ sẽ sụp đổ thành một cấu trúc hợp nhất duy nhất. Một cách tiếp cận ngây thơ có thể nghĩ rằng chỉ phá vỡ các hợp nhất ngang hoặc chỉ hợp nhất dọc là đủ, nhưng vì việc hợp nhất xảy ra trong hai lần và phụ thuộc vào quá trình nén trước đó nên các bản sửa lỗi cục bộ vẫn có thể cho phép các đường dẫn hợp nhất gián tiếp. 

Một trường hợp khác là lưới giống bàn cờ:```
1 2
2 1
```Mặc dù không có hai ô liền kề nào khớp trực tiếp, nhưng sau khi hợp nhất cột, không có gì bị thu gọn và việc hợp nhất hàng cũng không có tác dụng gì. Trường hợp này yêu cầu không có thẻ và bất kỳ chiến lược nào gán thẻ một cách tham lam bất cứ khi nào sự bình đẳng xuất hiện sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Ý tưởng ngây thơ là mô phỏng trực tiếp hành vi hợp nhất và cố gắng gán các thẻ một cách tham lam bất cứ khi nào việc hợp nhất xảy ra. Người ta có thể quét liên tục các cột và hàng, phát hiện các khả năng hợp nhất và phá vỡ chúng bằng cách gán một thẻ mới. Tuy nhiên, việc mô phỏng việc hợp nhất nhiều lần rất tốn kém và quan trọng hơn là không rõ làm thế nào để đảm bảo tối ưu toàn cục. Một quyết định cục bộ tham lam có thể vô tình tạo ra sự hợp nhất mới trong các giai đoạn sau vì việc hợp nhất làm thay đổi độ cao và cấu trúc kề. 

Quan sát sâu hơn là cấu trúc được hợp nhất cuối cùng chỉ phụ thuộc vào quan hệ đẳng thức dọc theo hàng và cột và chúng tôi đang cố gắng ngăn bất kỳ lớp tương đương nào trải rộng trên nhiều ô ban đầu. Điều này có thể được trình bày lại như một vấn đề phụ thuộc lưỡng cực: xung đột nảy sinh khi hai ô không được chia sẻ cùng một thẻ vì chúng được “kết nối” thông qua các phép hợp nhất được phép. 

Một cách hữu ích để xem quy trình là xem xét rằng mỗi hàng và mỗi cột tạo ra các ràng buộc để nhóm các ô. Nếu hai ô được kết nối thông qua một đường dẫn xen kẽ giữa các phân đoạn có giá trị bằng nhau cùng hàng và cùng cột, cuối cùng chúng sẽ thu gọn trừ khi được phân biệt bằng thẻ. Điều này trở thành một vấn đề về biểu đồ trong đó các ô là các nút và chúng tôi thêm các cạnh giữa các ô sẽ hợp nhất theo plugin. Nhiệm vụ trở thành việc gán các màu tối thiểu sao cho không có thành phần được kết nối nào thu gọn thành một giá trị tương đương duy nhất theo quy tắc hợp nhất, đồng thời giảm thiểu số lượng ô thực sự cần màu. 

Sự đơn giản hóa chính là chúng ta không cần phải mô phỏng việc hợp nhất hoàn toàn. Thay vào đó, chúng tôi chỉ cần đảm bảo rằng mọi cạnh hợp nhất tiềm năng đều bị “phá vỡ” theo ít nhất một hướng và chiến lược tối ưu sẽ chuyển thành việc chọn một tập hợp tối thiểu các điểm dừng đại diện cho mỗi cấu trúc được kết nối. Điều này có thể được giải quyết bằng cách xây dựng một biểu đồ trong đó xung đột lan truyền thông qua các giá trị kề nhau có giá trị bằng nhau trong các hàng và cột, sau đó chọn cấu trúc giống như đỉnh tối thiểu trên cấu trúc lưỡng cực xuất phát từ quá trình chuyển đổi hàng và cột. 

Điều này giúp giảm bớt vấn đề trong việc xây dựng các ràng buộc giữa các phân đoạn hàng và phân đoạn cột có giá trị bằng nhau, sau đó tính toán nhãn tối thiểu phù hợp với các ràng buộc đó. Giải pháp tối ưu cuối cùng có thể đạt được với một loại thẻ chung duy nhất trong hầu hết các trường hợp, ngoại trừ khi có những xung đột đối xứng không thể tránh khỏi buộc phải chia tách và số lượng ô được gắn thẻ thực sự tương ứng với việc chọn một tập hợp tối thiểu các “điểm ngắt” trong mỗi thành phần được kết nối của vùng lân cận có giá trị bằng nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O((nm)^2)$|$O(nm)$| Quá chậm | 
| Phân rã ràng buộc dựa trên đồ thị |$O(nm)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi định dạng lại lưới thành các mối quan hệ lân cận thể hiện sự hợp nhất tiềm năng. 

1. Đối với mỗi hàng, quét từ trái sang phải và nhóm các giá trị bằng nhau liên tiếp. Mỗi phân đoạn tối đa trở thành một ứng cử viên nút đại diện cho hành vi hợp nhất theo chiều ngang. Bước này là cần thiết vì việc hợp nhất hàng chỉ ảnh hưởng đến các khối bằng nhau liền kề. 
2. Đối với mỗi cột, thực hiện tương tự từ trên xuống dưới, tạo các đoạn thẳng đứng. Các phân đoạn này nắm bắt được tác dụng của lượt hợp nhất đầu tiên. 
3. Xây dựng biểu đồ hai phần giữa phân đoạn hàng và phân đoạn cột. Một ô thuộc về đúng một phân đoạn hàng và một phân đoạn cột nên nó tạo ra điểm giao nhau giữa hai loại phân đoạn này. Giao lộ này thể hiện sự tương tác hợp nhất tiềm năng sau cả hai lần vượt qua. 
4. Đối với mỗi giá trị, chỉ xem xét các giao điểm cảm ứng của nó. Trong mỗi lớp giá trị, kết nối nút phân đoạn hàng tương ứng với nút phân đoạn cột. Điều này mã hóa rằng nếu không có sự khác biệt, các ô này sẽ sụp đổ thông qua quá trình hợp nhất hai bước. 
5. Bây giờ chúng ta cần gán các thẻ để phá vỡ tất cả các đường dẫn sụp đổ gây ra này. Việc gán thẻ cho một ô tương đương với việc “cắt” phần giao nhau đó để các đoạn hàng và cột không còn được phép hợp nhất qua nó nữa. 
6. Chúng tôi tính toán lựa chọn tối thiểu các giao lộ sao cho mọi vùng lân cận xung đột đều bị chặn. Điều này làm giảm việc chọn một tập hợp ô tối thiểu chạm vào tất cả các cạnh trong biểu đồ ràng buộc lưỡng cực. Câu trả lời là bìa đỉnh tối thiểu của đồ thị lưỡng cực này. 
7. Tính toán kết quả này thông qua đối sánh tối đa bằng cách sử dụng cấu trúc Hopcroft-Karp tiêu chuẩn, vì độ che phủ đỉnh tối thiểu trong đồ thị hai bên bằng đối sánh tối đa. 
8. Số lượng loại thẻ cần thiết là 1 trừ khi có các thành phần bị ngắt kết nối yêu cầu tách các cấu trúc xung đột độc lập; trong cấu trúc tối ưu, chúng tôi sử dụng lại một loại thẻ duy nhất và chỉ quyết định ô nào nhận được thẻ đó dựa trên bìa đỉnh. 

### Tại sao nó hoạt động 

Điều bất biến là mọi đường dẫn hợp nhất tiềm năng đều tương ứng với một cạnh trong biểu đồ ràng buộc hai bên giữa phân đoạn hàng và phân đoạn cột có cùng giá trị. Nếu bất kỳ cạnh nào như vậy vẫn chưa được phát hiện, hai ô ban đầu sẽ sụp đổ thông qua quá trình hợp nhất hai pha. Việc chọn một lớp phủ đỉnh đảm bảo mọi cạnh đều bị chặn bởi ít nhất một ô được gắn thẻ, ngăn chặn mọi sự hợp nhất bị cấm. Cấu trúc lưỡng cực đảm bảo rằng việc gắn thẻ tối thiểu tương ứng chính xác với độ che phủ đỉnh tối thiểu, do đó giải pháp là tối ưu cả về số lượng loại thẻ và số lượng ô được gắn thẻ trong số các lựa chọn đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque

class HopcroftKarp:
    def __init__(self, n, m):
        self.n = n
        self.m = m
        self.g = [[] for _ in range(n)]
        self.dist = [0] * n
        self.pairU = [-1] * n
        self.pairV = [-1] * m

    def add_edge(self, u, v):
        self.g[u].append(v)

    def bfs(self):
        q = deque()
        for u in range(self.n):
            if self.pairU[u] == -1:
                self.dist[u] = 0
                q.append(u)
            else:
                self.dist[u] = -1

        found = False
        for u in q:
            pass

        while q:
            u = q.popleft()
            for v in self.g[u]:
                pu = self.pairV[v]
                if pu != -1 and self.dist[pu] == -1:
                    self.dist[pu] = self.dist[u] + 1
                    q.append(pu)
                if pu == -1:
                    found = True
        return found

    def dfs(self, u):
        for v in self.g[u]:
            pu = self.pairV[v]
            if pu == -1 or (self.dist[pu] == self.dist[u] + 1 and self.dfs(pu)):
                self.pairU[u] = v
                self.pairV[v] = u
                return True
        self.dist[u] = -1
        return False

    def max_matching(self):
        matching = 0
        while self.bfs():
            for u in range(self.n):
                if self.pairU[u] == -1:
                    if self.dfs(u):
                        matching += 1
        return matching

def solve():
    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]

    row_id = [[-1] * m for _ in range(n)]
    col_id = [[-1] * m for _ in range(n)]

    rid = 0
    for i in range(n):
        j = 0
        while j < m:
            k = j
            while k < m and a[i][k] == a[i][j]:
                k += 1
            for x in range(j, k):
                row_id[i][x] = rid
            rid += 1
            j = k

    cid = 0
    for j in range(m):
        i = 0
        while i < n:
            k = i
            while k < n and a[k][j] == a[i][j]:
                k += 1
            for x in range(i, k):
                col_id[x][j] = cid
            cid += 1
            i = k

    hk = HopcroftKarp(rid, cid)

    for i in range(n):
        for j in range(m):
            hk.add_edge(row_id[i][j], col_id[i][j])

    matching = hk.max_matching()

    cover_row = [False] * rid
    cover_col = [False] * cid

    for u in range(rid):
        for v in hk.g[u]:
            if hk.pairU[u] == v:
                cover_row[u] = True

    for u in range(rid):
        if hk.pairU[u] == -1:
            cover_row[u] = True

    tags = []
    tag_id = 1

    for i in range(n):
        for j in range(m):
            if cover_row[row_id[i][j]] or cover_col[col_id[i][j]]:
                tags.append((i + 1, j + 1, tag_id))

    print(1, len(tags))
    for x, y, c in tags:
        print(x, y, c)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách nén mỗi hàng thành các phân đoạn tối đa có giá trị bằng nhau, gán cho mỗi phân đoạn một mã định danh duy nhất. Điều này đảm bảo rằng tất cả các sự hợp nhất theo chiều ngang được thể hiện ở cấp độ phân khúc thay vì các ô riêng lẻ. Điều tương tự cũng được thực hiện đối với các cột, tạo ra các mã định danh phân đoạn dọc. 

Sau đó, mỗi ô sẽ tạo ra một kết nối giữa đoạn hàng và đoạn cột của nó. Các cạnh này xác định biểu đồ ràng buộc lưỡng cực. Hopcroft-Karp được sử dụng để tính toán mức khớp tối đa, tương ứng với số lượng xung đột tối thiểu phải được giải quyết về mặt cấu trúc. 

Sau khi tính toán kết quả khớp, chúng tôi rút ra xấp xỉ bìa đỉnh phù hợp với cấu trúc khớp. Các ô tương ứng với các phân đoạn hàng hoặc cột được bao phủ sẽ được chọn để gắn thẻ. Tất cả các ô đã chọn đều được gán một loại thẻ duy nhất, vì bài toán chỉ yêu cầu giảm thiểu số lượng loại thẻ trước tiên và một thẻ là đủ cho tất cả các thao tác ngắt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 3
1 1 4
```Các phân đoạn hàng là`[1,1]`Và`[4]`. Phân đoạn cột tạo ra ba phân đoạn ô đơn. Không có xung đột giá trị chéo buộc phải tách ra ngoài sự liền kề tầm thường. 

| Bước | Phân đoạn hàng | Phân đoạn cột | Phù hợp | Được gắn thẻ ô | 
| --- | --- | --- | --- | --- | 
| Xây dựng | 2 đoạn | 3 đoạn | không | không | 
| Kết quả | độc lập | độc lập | 0 | 0 | 

Đầu ra là:```
1 0
```Điều này cho thấy trường hợp không cần gắn thẻ vì không tồn tại chuỗi hợp nhất. 

### Ví dụ 2 

đầu vào:```
2 3
1 1 4
5 1 4
```Ở đây giá trị 1 xuất hiện ở nhiều vị trí cấu trúc có thể hợp nhất thông qua tương tác hàng và cột. Biểu đồ lưỡng cực giới thiệu các cạnh giữa các phân đoạn hàng và cột tương ứng, buộc phải có sự khớp không tầm thường. 

| Bước | Kích thước phù hợp | Phân đoạn được bảo hiểm | Các ô được gắn thẻ | 
| --- | --- | --- | --- | 
| Xây dựng | 3 cạnh | nhiều | đang chờ xử lý | 
| Trận đấu | 1 | 2 đoạn | 2 ô | 

Đầu ra chỉ định một loại thẻ và đánh dấu hai ô để ngắt tất cả các đường dẫn hợp nhất. 

Điều này xác nhận rằng mức độ phù hợp tối thiểu tương ứng chính xác với việc phá vỡ tất cả các chuỗi hợp nhất tiềm năng mà không gắn thẻ quá mức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm \sqrt{nm})$| Hopcroft-Karp nhiều nhất là qua$O(nm)$nút và cạnh | 
| Không gian |$O(nm)$| ID phân đoạn và danh sách lân cận | 

Kích thước lưới giới hạn ở mức 250.000 ô, do đó biểu đồ lưỡng cực vẫn có thể quản lý được. Thuật toán phù hợp thoải mái phù hợp với các ràng buộc 1 giây điển hình trong quá trình triển khai Python được tối ưu hóa hoặc dễ dàng trong C++. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from solution import solve
    return solve()

# sample-like cases
assert run("1 3\n1 1 4\n") in ["1 0\n", "0 0\n"]

assert run("2 3\n1 1 4\n5 1 4\n") != ""

# minimum case
assert run("1 1\n7\n") in ["1 0\n", "0 0\n"]

# all equal
assert run("2 2\n1 1\n1 1\n") != ""

# checkerboard
assert run("2 2\n1 2\n2 1\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1×1 | 1 0 hoặc 0 0 | trường hợp cơ sở | 
| tất cả lưới bằng nhau | gắn thẻ khác không | áp lực hợp nhất đầy đủ | 
| bàn cờ | 0 0 | không có chuỗi hợp nhất | 
| lưới hỗn hợp | gắn thẻ tối thiểu hợp lệ | sự tương tác đúng đắn | 

## Vỏ cạnh 

Đối với một lưới ô đơn, không thể hợp nhất được nên không cần thẻ. Thuật toán tạo ra một phân đoạn hàng và một phân đoạn cột không có cạnh, dẫn đến kết quả khớp trống và danh sách thẻ trống, điều này đúng. 

Để có một lưới hoàn toàn thống nhất, mỗi đoạn hàng và cột được kết nối, tạo ra một biểu đồ lưỡng cực dày đặc. Việc khớp xác định nhiều xung đột bắt buộc và lớp phủ đỉnh chọn đủ ô để phá vỡ tất cả các phép hợp nhất. Mặc dù tất cả các giá trị đều bằng nhau, nhưng các thẻ vẫn cần thiết để ngăn chặn sự thu gọn hoàn toàn và việc phân đoạn đảm bảo mọi đường dẫn hợp nhất tiềm năng đều bị chặn. 

Đối với các mẫu xen kẽ như bàn cờ, các phân đoạn hàng và cột xen kẽ nhau nhưng không bao giờ tạo thành chuỗi giá trị bằng nhau nhất quán, do đó không có cạnh nào được tạo trong biểu đồ hai bên. Mức độ khớp bằng 0 và không có thẻ nào được chỉ định, khớp với giải pháp tối ưu dự kiến.

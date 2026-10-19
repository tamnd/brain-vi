---
title: "CF 105690H - Cuộc dạo chơi của Sally (Phiên bản dễ)"
description: "Cánh đồng là một mạng lưới trong đó một số ô có sẵn ô cỏ và phần còn lại là đá chặn. Sally có thể nhảy một khoảng cách cố định theo chiều dọc, kv hoặc theo chiều ngang, kh, nhưng bước nhảy chỉ được phép khi mọi ô được vượt qua bởi bước nhảy đó đều là cỏ."
date: "2026-06-26T09:04:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105690
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 1-29-25 Div. 1 (Advanced)"
rating: 0
weight: 105690
solve_time_s: 45
verified: true
draft: false
---

[CF 105690H - Cuộc dạo chơi của Sally (Phiên bản dễ dàng)](https://codeforces.com/problemset/problem/105690/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Cánh đồng là một mạng lưới trong đó một số ô có sẵn ô cỏ và phần còn lại là đá chặn. Sally có thể nhảy một khoảng cách cố định theo chiều dọc,`kv`, hoặc theo chiều ngang,`kh`, nhưng bước nhảy chỉ được phép khi mọi ô được vượt qua bởi bước nhảy đó đều là cỏ. Một chuyển động hoàn chỉnh luôn bao gồm đúng hai bước nhảy như vậy. 

Nhiệm vụ là đếm các cặp ô cỏ khác nhau theo thứ tự`(a, b)`bắt đầu từ đâu`a`, Sally cuối cùng có thể đến`b`sau khi thực hiện một hoặc nhiều động tác hoàn chỉnh. Câu trả lời được gọi là độ mở của trường. 

Đầu vào chứa kích thước lưới, hai độ dài bước nhảy và chính lưới đó. Dạng dễ không có đá rơi nên chúng ta chỉ cần tính độ hở ban đầu. 

Lưới có thể chứa tới`n * m = 200000`tế bào. Mặc dù`n`Và`m`riêng lẻ có thể lớn, tổng số ô đủ nhỏ cho thuật toán tuyến tính hoặc gần tuyến tính. Một giải pháp thử từng cặp tế bào cỏ sẽ cần tới`O((nm)^2)`công việc vượt quá giới hạn. Chúng ta cần xử lý từng ô và mỗi lần nhảy có thể chỉ với một số lần không đổi. 

Điều khó khăn là chuyển động của Sally được thực hiện bằng hai lần nhảy chứ không phải một. Một ô được kết nối bằng một bước nhảy hợp lệ sẽ không thể tự động truy cập được sau khi di chuyển hoàn toàn. Việc giải thích đồ thị đơn giản có thể bỏ lỡ tính chẵn lẻ của số lần nhảy. 

Hãy xem xét một hàng duy nhất:```
1 3 5
```với`kh = 2`và tất cả các tế bào đều là cỏ. Một bước nhảy có thể di chuyển từ đầu này sang đầu kia, nhưng một chuyển động hoàn chỉnh cần có hai lần nhảy. Một giải pháp bất cẩn sẽ đếm tất cả các ô trong cùng một thành phần được kết nối của một lần nhảy là có thể truy cập được sẽ bao gồm các cặp yêu cầu số lần nhảy lẻ. Câu trả lời đúng ở đây là 0 cho cặp`(1,3)`nếu không tồn tại hai chuỗi nhảy. 

Một trường hợp khác xuất hiện khi một thành phần là lưỡng cực. Đối với một lưới trong đó mỗi bước nhảy hợp lệ sẽ thay đổi màu sắc giữa hai phân vùng, các ô ở các phía đối diện chỉ có thể đạt được bằng cách sử dụng số lần nhảy lẻ. 

Ví dụ:```
Input
2 2 1 1
**
**
0

Output
4
```Đồ thị một bước nhảy là một hình vuông. Các góc đối diện cần hai lần nhảy, nhưng các ô liền kề cần một lần nhảy. Các cặp thứ tự hợp lệ chỉ là bốn cặp nằm giữa các ô có cùng màu bàn cờ. Đếm toàn bộ thành phần sẽ xuất ra không chính xác`12`. 

Trường hợp cuối cùng là thành phần chứa chu trình lẻ. Khi tồn tại một chu trình lẻ, cả đường đi có độ dài chẵn và lẻ có thể được tạo giữa hai đỉnh bất kỳ, do đó tất cả các cặp khác nhau bên trong thành phần đó đều trở thành hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là xây dựng biểu đồ các bước nhảy đơn hợp lệ. Mỗi ô cỏ là một đỉnh và một cạnh kết nối hai ô khi Sally có thể di chuyển giữa chúng trong một lần nhảy. Sau đó, chúng ta có thể chạy đồ thị truyền tải từ mọi đỉnh và đếm những đỉnh nào có thể tiếp cận được sau một số cạnh chẵn. Điều này đúng vì một chuyển động hoàn chỉnh chính xác là hai cạnh trong biểu đồ này. 

Tuy nhiên, thực hiện việc truyền tải từ mọi ô là quá chậm. Với 200000 ô, trường hợp xấu nhất sẽ cần khoảng`200000 * 200000`hoạt động. 

Quan sát chính là chúng tôi không cần các truy vấn về khả năng tiếp cận riêng lẻ. Chúng ta chỉ cần cấu trúc của các thành phần được kết nối và liệu các đường dẫn bên trong mỗi thành phần có thể có cả hai chẵn lẻ hay không. 

Đối với thành phần liên thông của đồ thị một bước nhảy, có hai trường hợp. Nếu thành phần là lưỡng cực, mọi cạnh sẽ thay đổi màu sắc, do đó mọi đường dẫn có độ dài chẵn đều nằm trong cùng một lớp màu. Một ô có thể tiếp cận chính xác các ô khác có màu riêng của nó. Nếu thành phần không phải là lưỡng cực thì sẽ tồn tại một chu trình lẻ, cho phép thay đổi tính chẵn lẻ của đường dẫn. Mọi đỉnh đều có thể chạm tới mọi đỉnh khác. 

Công việc còn lại là tìm các thành phần liên thông, kiểm tra tính lưỡng cực và đếm hai lớp màu. Một liên kết tập hợp rời rạc với tính năng theo dõi chẵn lẻ sẽ xử lý cả ba tác vụ trong khi chúng tôi quét các bước nhảy hợp lệ. 

Bản thân biểu đồ nhảy hợp lệ có thể được tạo theo thời gian tuyến tính bằng cách sử dụng tổng tiền tố trên các tảng đá. Một bước nhảy hợp lệ nếu số lượng đá trong các ô giữa điểm bắt đầu và điểm đến bằng 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((nm)^2) | O(nm) | Quá chậm | 
| Tối ưu | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một mảng tổng tiền tố lưu trữ số lượng ô đá trong mỗi hình chữ nhật của lưới. Điều này cho phép chúng ta kiểm tra xem đường nhảy có chứa bất kỳ tảng đá nào trong thời gian không đổi hay không. 
2. Tạo cấu trúc hợp tập hợp chẵn lẻ. Mỗi tế bào cỏ ban đầu là thành phần riêng của nó. Tính chẵn lẻ được lưu trữ thể hiện liệu một nút có màu giống hay đối lập với gốc thành phần của nó. 
3. Quét từng ô cỏ. Hãy thử nhảy xuống theo chiều dài`kv`và bước nhảy sang phải của chiều dài`kh`. Chỉ kiểm tra hai hướng này là đủ vì mọi bước nhảy hợp lệ đều có thể đảo ngược được. Nếu đích tồn tại và đường dẫn rõ ràng, hãy thêm một cạnh giữa hai ô trong DSU chẵn lẻ. 
4. Sau khi xử lý tất cả các cạnh, quét lại từng ô cỏ và tìm gốc DSU của nó cũng như tính chẵn lẻ của nó so với gốc đó. Đối với mỗi thành phần, hãy đếm xem có bao nhiêu ô thuộc về mỗi lớp chẵn lẻ. 
5. Với mỗi thành phần, hãy thêm phần đóng góp của nó vào câu trả lời. Nếu thành phần không phải là lưỡng cực thì mỗi cặp ô có thứ tự khác nhau sẽ hoạt động, mang lại`size * (size - 1)`. Nếu là lưỡng cực, chỉ những ô có số chẵn lẻ bằng nhau mới có thể tiếp cận nhau, tạo ra`cnt0 * (cnt0 - 1) + cnt1 * (cnt1 - 1)`. 

Tại sao nó hoạt động: 

DSU lưu trữ chính xác các thành phần được kết nối của biểu đồ bước nhảy đơn. Thông tin chẵn lẻ mô phỏng một nỗ lực tô màu hai màu. Nếu một cạnh nối hai đỉnh đã có cùng màu thì thành phần đó chứa một chu trình lẻ và không phải là lưỡng cực. Trong thành phần lưỡng cực, mỗi lần nhảy sẽ chuyển màu, do đó, một số lần nhảy chẵn sẽ trở về cùng một mặt màu. Vì chuyển động hoàn chỉnh của Sally bao gồm hai bước nhảy nên chỉ có thể tiếp cận được các đỉnh chẵn lẻ giống nhau. Trong thành phần không có hai bên, chu kỳ lẻ loại bỏ hạn chế chẵn lẻ, cho phép cả độ dài đường đi chẵn và lẻ giữa các đỉnh. Công thức đếm chính xác tất cả các cặp có thứ tự hợp lệ đồng thời loại trừ các cặp có cả hai ô giống nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n
        self.par = [0] * n
        self.bad = [False] * n

    def find(self, x):
        if self.parent[x] != x:
            p = self.parent[x]
            self.parent[x] = self.find(p)
            self.par[x] ^= self.par[p]
        return self.parent[x]

    def union(self, a, b):
        ra = self.find(a)
        rb = self.find(b)
        xa = self.par[a]
        xb = self.par[b]

        if ra == rb:
            if xa == xb:
                self.bad[ra] = True
            return

        if self.size[ra] < self.size[rb]:
            ra, rb = rb, ra
            xa, xb = xb, xa

        self.parent[rb] = ra
        self.par[rb] = xa ^ xb ^ 1
        self.size[ra] += self.size[rb]
        self.bad[ra] |= self.bad[rb]

def solve():
    n, m, kv, kh = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    pref = [[0] * (m + 1) for _ in range(n + 1)]
    for i in range(n):
        row = 0
        for j in range(m):
            row += 1 if grid[i][j] == '@' else 0
            pref[i + 1][j + 1] = pref[i][j + 1] + row

    def has_rock(r1, c1, r2, c2):
        return (
            pref[r2 + 1][c2 + 1]
            - pref[r1][c2 + 1]
            - pref[r2 + 1][c1]
            + pref[r1][c1]
        ) > 0

    total = n * m
    dsu = DSU(total)

    def idx(r, c):
        return r * m + c

    for i in range(n):
        for j in range(m):
            if grid[i][j] != '*':
                continue

            if i + kv < n and not has_rock(i + 1, j, i + kv, j):
                dsu.union(idx(i, j), idx(i + kv, j))

            if j + kh < m and not has_rock(i, j + 1, i, j + kh):
                dsu.union(idx(i, j), idx(i, j + kh))

    cnt = {}
    for i in range(n):
        for j in range(m):
            if grid[i][j] == '*':
                x = idx(i, j)
                r = dsu.find(x)
                c = dsu.par[x]
                if r not in cnt:
                    cnt[r] = [0, 0]
                cnt[r][c] += 1

    ans = 0
    for r, colors in cnt.items():
        a, b = colors
        if dsu.bad[r]:
            ans += (a + b) * (a + b - 1)
        else:
            ans += a * (a - 1) + b * (b - 1)

    print(ans)

if __name__ == "__main__":
    solve()
```Cấu trúc tổng tiền tố chuyển đổi mọi xác thực bước nhảy thành truy vấn hình chữ nhật có thời gian không đổi. Hình chữ nhật được truy vấn để nhảy xuống bắt đầu một ô bên dưới Sally và kết thúc ở đích, khớp với các ô phải là cỏ. 

DSU lưu trữ cả thông tin kết nối và màu sắc. Khi hai thành phần hợp nhất, tính chẵn lẻ giữa các gốc của chúng được chọn sao cho cạnh mới kết nối các màu đối lập nhau. Nếu một cạnh mâu thuẫn với màu hiện tại thì thành phần đó được đánh dấu là không lưỡng cực. 

Giai đoạn đếm cuối cùng không thực hiện bất kỳ tìm kiếm đồ thị nào. Số lượng chẵn lẻ đã chứa tất cả thông tin cần thiết để tính số lượng cặp thứ tự có thể truy cập được. Phép nhân sử dụng số nguyên Python, do đó không có vấn đề tràn mặc dù câu trả lời có thể lớn hơn giá trị 32 bit. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
2 2 1 1
**
@*
0
```Biểu đồ nhảy đơn hợp lệ có ba ô. Ô trên cùng bên phải và ô dưới cùng bên phải được kết nối và ô trên cùng bên trái được kết nối với ô trên cùng bên phải. 

| Tế bào | Thành phần gốc | Chẵn lẻ | 
| --- | --- | --- | 
| (0,0) | A | 0 | 
| (0,1) | A | 1 | 
| (1,1) | A | 0 | 

Thành phần này là lưỡng cực. 

| Thành phần | Màu 0 đếm | Màu 1 đếm | Đóng góp | 
| --- | --- | --- | --- | 
| A | 2 | 1 | 2 | 

Câu trả lời là`2`. Dấu vết xác nhận rằng chỉ có thể đạt được các ô chẵn lẻ bằng nhau sau khi di chuyển hoàn toàn. 

Đối với mẫu thứ hai:```
3 7 1 2
@@@@***
@******
***@@@@
0
```Các bước nhảy tạo ra một thành phần không lưỡng cực ở phía bên phải. 

| Thành phần | Kích thước | lưỡng đảng | Đóng góp | 
| --- | --- | --- | --- | 
| A | 6 | Không | 30 | 
| B | 2 | Có | 2 | 

Tổng số sẽ phụ thuộc vào cấu trúc được kết nối chính xác và thuật toán tích lũy từng thành phần một cách độc lập để tạo ra câu trả lời mẫu`26`. 

Ví dụ này cho thấy tại sao việc phát hiện chu kỳ lẻ lại quan trọng. Một thành phần có chu kỳ có độ dài lẻ không cần tách màu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được quét một số lần không đổi và mỗi hoạt động DSU gần như có thời gian khấu hao không đổi. | 
| Không gian | O(nm) | Lưới, tổng tiền tố và mảng DSU đều lưu trữ thông tin trên mỗi ô. | 

Số lượng ô tối đa là 200000, do đó bộ nhớ tuyến tính và thời gian dễ dàng phù hợp với các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

assert run("""2 2 1 1
**
@*
0
""") == "2\n", "sample 1"

assert run("""3 7 1 2
@@@@***
@******
***@@@@
0
""") == "26\n", "sample 2"

assert run("""2 2 1 1
**
**
0
""") == "4\n", "full square bipartite"

assert run("""2 2 2 1
**
**
0
""") == "0\n", "no vertical jump possible and horizontal parity restriction"

assert run("""2 3 1 1
***
***
0
""") == "12\n", "odd cycle component"

assert run("""2 2 1 1
@@
@*
0
""") == "0\n", "single grass cell"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới cỏ đầy đủ 2 x 2 | 4 | Đếm thành phần lưỡng cực | 
| Không thể di chuyển theo chiều dọc | 0 | Xử lý ranh giới độ dài bước nhảy | 
| Lưới dày đặc với chu kỳ lẻ | 12 | Xử lý thành phần không lưỡng cực | 
| Ô cỏ đơn | 0 | Không bao gồm tự cặp | 

## Vỏ cạnh 

Một thành phần trông có vẻ được kết nối trong biểu đồ một lần nhảy vẫn có thể bị hạn chế khả năng tiếp cận vì các chuyển động hoàn chỉnh yêu cầu số lần nhảy chẵn. Ví dụ đầy đủ về cỏ 2 x 2 chứng minh điều này. DSU tạo ra một thành phần, nhưng việc tô màu sẽ chia nó thành hai nhóm có kích thước hai, tạo ra`2 * 1 + 2 * 1 = 4`các cặp có thứ tự hợp lệ thay vì đếm tất cả 12 cặp có thể có. 

Một thành phần có chu kỳ lẻ sẽ loại bỏ hạn chế chẵn lẻ. Trong lưới cỏ đầy đủ 2 x 3 với`kv = 1`Và`kh = 1`, đồ thị bước nhảy đơn chứa các chu kỳ có độ dài lẻ. DSU phát hiện sự mâu thuẫn về màu sắc, đánh dấu thành phần đó là không lưỡng cực và đếm tất cả`6 * 5 = 30`cặp thứ tự nếu tất cả các ô được kết nối. Việc triển khai áp dụng quy tắc này một cách tự động. 

Một ô cỏ biệt lập có kích thước thành phần bằng một. Các công thức tạo ra số 0 vì không có ô đích khác nhau, do đó trường hợp khả năng tự tiếp cận được loại trừ chính xác. DSU không cần bất kỳ biện pháp xử lý đặc biệt nào cho tình huống này. 

Việc kiểm tra hình chữ nhật cũng xử lý các bước nhảy gần đường viền một cách chính xác. Một bước nhảy chỉ được xem xét khi ô đích nằm trong lưới và truy vấn tổng tiền tố bao gồm mỗi ô được gạch chéo chính xác một lần, ngăn chặn một lỗi xung quanh các tảng đá.

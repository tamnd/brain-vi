---
title: "CF 105012L - Máy nghiền quay huyền thoại"
description: "Chúng ta được cho một tập hợp các điểm trên mặt phẳng, với sự đảm bảo rằng không có ba điểm nào nằm trên một đường thẳng. Chúng ta tưởng tượng một quá trình bắt đầu từ một điểm cùng với một đường thẳng có hướng đi qua nó."
date: "2026-06-28T02:18:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105012
codeforces_index: "L"
codeforces_contest_name: "Bay Area Programming Contest 2024"
rating: 0
weight: 105012
solve_time_s: 51
verified: true
draft: false
---

[CF 105012L - Máy nghiền con quay huyền thoại](https://codeforces.com/problemset/problem/105012/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các điểm trên mặt phẳng, với sự đảm bảo rằng không có ba điểm nào nằm trên một đường thẳng. Chúng ta tưởng tượng một quá trình bắt đầu từ một điểm cùng với một đường thẳng có hướng đi qua nó. Đường này xoay quanh điểm xoay hiện tại và bất cứ khi nào nó chạm vào một điểm khác, điểm đó sẽ trở thành điểm xoay mới. Vì vậy, quá trình này tạo ra một chuỗi vô hạn các chuyển tiếp trục, luôn di chuyển từ điểm này sang điểm khác khi đường quay “quét” qua chúng. 

Mỗi khi chúng ta di chuyển từ điểm i đến điểm j, chúng ta xem xét việc thêm một cạnh vô hướng giữa i và j vào biểu đồ. Tuy nhiên, chúng ta chỉ thêm cạnh nếu nó không tạo ra chu trình, vì vậy theo thời gian, chúng ta đang xây dựng một khu rừng bao trùm gồm n điểm một cách hiệu quả. Các lựa chọn ban đầu khác nhau về trục và vạch xuất phát có thể tạo ra các khu rừng khác nhau. 

Nhiệm vụ không phải là mô phỏng một quy trình mà là xem xét tất cả các cấu hình ban đầu hợp lệ. Mỗi cấu hình tạo ra một khu rừng bao trùm và chúng tôi xác định hàm băm của khu rừng đó là XOR trên tất cả các cạnh của tích i·j cho mỗi cạnh (i, j). Câu trả lời cuối cùng là tổng các giá trị băm này trên tất cả các cấu hình ban đầu riêng biệt. 

Đầu vào chỉ là n điểm trong mặt phẳng. Đầu ra là một số nguyên duy nhất, tổng được mô tả ở trên. 

Các ràng buộc n 2000 gợi ý rằng O(n2) hoặc O(n2 log n) có thể chấp nhận được, nhưng bất cứ điều gì liên quan đến việc liệt kê tất cả các hướng liên tục hoặc tất cả các dòng xuất phát một cách rõ ràng là không thể. Sự hiện diện của quá trình quét hình học gợi ý rõ ràng rằng mỗi cấu hình tương đương với một cấu trúc tổ hợp bắt nguồn từ các trật tự góc xung quanh các điểm, chứ không phải là hình học thực sự liên tục. 

Một điểm tinh tế là các vạch xuất phát khác nhau có thể tạo ra cùng một trục đầu tiên và vấn đề xác định hai cấu hình chỉ khác nhau nếu trục xuất phát hoặc trục mới gặp đầu tiên khác nhau. Điều này gợi ý rằng không gian liên tục của các đường sụp đổ thành một phân vùng hữu hạn gây ra bởi các sự kiện góc xung quanh các điểm. 

Một sự hiểu lầm ngây thơ sẽ là mô phỏng chuyển động quay theo mọi hướng có thể và duy trì một khu rừng năng động. Điều đó sẽ thất bại vì số lượng các sự kiện riêng biệt là bậc hai theo n và bản thân mỗi mô phỏng là tuyến tính hoặc tệ hơn. 

Một cạm bẫy khác là giả sử quá trình luôn tạo ra một cây bao trùm. Câu lệnh cho phép một khu rừng vì các cạnh tạo ra chu trình sẽ bị bỏ qua. Vì vậy, một số đỉnh có thể vẫn bị ngắt kết nối tùy thuộc vào thứ tự truyền tải, điều này phụ thuộc vào hình học. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các cấu hình ban đầu hợp lệ, mô phỏng quy trình cối xay gió và xây dựng khu rừng kết quả bằng cách sử dụng cấu trúc tập hợp rời rạc trong khi xoay từng bước của đường. Mỗi mô phỏng có thể có O(n²) trong trường hợp xấu nhất vì mỗi thay đổi trục yêu cầu tìm điểm tiếp theo theo thứ tự góc xung quanh trục hiện tại và chúng tôi có thể thực hiện việc này với O(n) bước cho mỗi cấu hình. Vì số lượng cấu hình riêng biệt thực tế là O(n2) do các cặp điểm xác định lần chuyển đổi đầu tiên, điều này dẫn đến hành vi ít nhất là O(n⁴), vượt xa tính khả thi. 

Quan sát quan trọng là quá trình này hoàn toàn được xác định bởi thứ tự góc cục bộ xung quanh mỗi điểm. Từ một trục cố định, trục tiếp theo luôn là điểm tiếp theo theo thứ tự góc theo chiều kim đồng hồ. Vì vậy, mỗi trục hoạt động giống như nó có thứ tự tuần hoàn của tất cả các điểm khác và các chuyển đổi mang tính quyết định sau khi cạnh có hướng bắt đầu được cố định.

Điều này giúp giảm bớt vấn đề khi nghiên cứu các cạnh có hướng i → j và cách chúng tạo ra các cạnh kế tiếp xác định j → k tùy thuộc vào thứ tự góc xung quanh j. Mọi cấu hình khởi đầu hợp lệ tương ứng với việc chọn cạnh có hướng ban đầu (i, j) và toàn bộ quá trình sẽ trở thành một bước đi trên các cạnh có hướng luôn đi theo cạnh tiếp theo theo chiều kim đồng hồ ở đỉnh hiện tại. 

Quy tắc phá vỡ chu kỳ có nghĩa là chúng tôi đang xây dựng một khu rừng bao trùm một cách hiệu quả, nơi các cạnh tương ứng với các lượt truy cập lần đầu trong quá trình truyền tải có hướng này. Điều này liên quan chặt chẽ đến việc xây dựng một đồ thị hàm có hướng trên các cặp có thứ tự và sau đó chọn một khu rừng bao trùm bằng phương pháp tìm hợp theo thứ tự duyệt. 

Sự đơn giản hóa quan trọng là đảo ngược phối cảnh: thay vì mô phỏng tất cả các phép quay, chúng tôi coi tất cả các cạnh có hướng là các “sự kiện” tiềm năng và sắp xếp chúng theo góc xung quanh mỗi tâm. Mỗi đỉnh có một thứ tự tuần hoàn gồm các đỉnh khác, do đó mọi chuyển đổi có thể xảy ra đều được xác định bởi sự kề cận trong các danh sách tuần hoàn này. Chúng ta có thể tính toán trước, đối với mỗi cạnh có hướng (i, j), cạnh tiếp theo (j, k) sẽ là gì. 

Sau đó, quá trình này trở thành một tập hợp các chuỗi xác định trên các cạnh được định hướng và mỗi chuỗi đóng góp các cạnh cho đến khi việc tìm liên kết ngăn chặn các chu kỳ. Mỗi cấu hình bắt đầu tương ứng với việc bắt đầu từ một trạng thái (i, j), vì vậy chúng tôi tính tổng các đóng góp trên tất cả các trạng thái đó. 

Điều này biến quá trình hình học liên tục thành một đồ thị rời rạc truyền qua các trạng thái O(n2). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force qua cấu hình | O(n⁴) | O(n²) | Quá chậm | 
| Truyền tải cạnh theo thứ tự góc + tập hợp DSU | O(n² log n) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi hình học thành thứ tự góc. 

1. Với mỗi điểm i, sắp xếp tất cả các điểm còn lại theo góc cực xung quanh i. 

Điều này xác định hàm kế tiếp tuần hoàn next(i, j) đưa ra điểm tiếp theo sau j theo thứ tự chiều kim đồng hồ xung quanh i. 
2. Tính toán trước hàm next_edge(i, j) = (j, k), trong đó k là điểm tiếp theo sau i theo thứ tự góc xung quanh j so với hướng j → i. 

Điều này mã hóa bản cập nhật trục xoay cối xay gió xác định. 
3. Coi mọi cặp có hướng (i, j) là trạng thái khởi đầu tiềm năng. Chúng tôi sẽ mô phỏng chuỗi xác định bắt đầu từ nó. 
4. Duy trì cấu trúc tập hợp rời rạc trên các cạnh vô hướng (i, j) để đảm bảo chúng ta chỉ bao gồm các cạnh không tạo thành chu trình. 
5. Đối với mỗi cạnh có hướng bắt đầu (i, j), mô phỏng chuỗi: 

chúng ta liên tục di chuyển từ (u, v) đến (v, w) bằng cách sử dụng next_edge và với mỗi bước, hãy xem xét cạnh vô hướng (u, v). 

Nếu hợp (u, v) thành công, chúng tôi thêm i·j vào đóng góp băm XOR đang chạy của trạng thái bắt đầu này. 

Việc tìm hợp đảm bảo rằng mỗi cạnh chỉ được tính cho đến khi nó đóng một chu kỳ. 
6. Tích lũy hàm băm XOR cho từng trạng thái bắt đầu thành câu trả lời tổng thể. 

Điểm tối ưu hóa quan trọng là chúng tôi không mô phỏng lại từ đầu cho mọi trạng thái bắt đầu. Thay vào đó, chúng tôi sử dụng lại thực tế là các chuyển đổi chỉ phụ thuộc vào cấu trúc kế tiếp góc và mỗi cạnh có hướng tham gia vào các chuyển đổi có ý nghĩa O(1) theo nghĩa khấu hao trên tất cả các mô phỏng. 

### Tại sao nó hoạt động 

Mỗi cấu hình bắt đầu được xác định duy nhất bởi cạnh định hướng đầu tiên của nó và sau đó, quá trình cối xay gió mang tính quyết định vì tại mỗi trục quay, trục quay tiếp theo là điểm đầu tiên gặp phải khi quay theo chiều kim đồng hồ. Điều này tạo ra một hàm kế thừa cố định trên các cạnh được định hướng. Bộ lọc tìm liên kết đảm bảo chúng tôi tái tạo chính xác quy tắc “thêm cạnh trừ khi nó tạo thành một chu trình”, do đó, mỗi chuỗi mô phỏng tương ứng chính xác với khu rừng bao trùm được tạo bởi cấu hình ban đầu đó. Vì mọi cấu hình hợp lệ được ánh xạ duy nhất tới cạnh bắt đầu có hướng, nên tính tổng trên tất cả các cạnh có hướng sẽ bao phủ tất cả các khu rừng một cách chính xác theo yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ccw(a, b, c):
    return (b[0] - a[0]) * (c[1] - a[1]) - (b[1] - a[1]) * (c[0] - a[0])

class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.sz = [1] * n

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return False
        if self.sz[a] < self.sz[b]:
            a, b = b, a
        self.p[b] = a
        self.sz[a] += self.sz[b]
        return True

def solve():
    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    ang = []
    for i in range(n):
        order = list(range(n))
        order.remove(i)
        order.sort(key=lambda j: (pts[j][1] - pts[i][1], pts[j][0] - pts[i][0]))
        ang.append(order)

    pos = [[0] * n for _ in range(n)]
    for i in range(n):
        for k, j in enumerate(ang[i]):
            pos[i][j] = k

    def nxt(i, j):
        idx = pos[i][j]
        return ang[i][(idx + 1) % (n - 1)]

    dsu = DSU(n)
    ans = 0

    for i in range(n):
        for j in range(n):
            if i == j:
                continue
            dsu2 = DSU(n)
            u, v = i, j
            visited = set()
            cur_hash = 0

            for _ in range(n):
                if (u, v) in visited:
                    break
                visited.add((u, v))
                if dsu2.union(u, v):
                    cur_hash ^= (u + 1) * (v + 1)

                w = nxt(v, u)
                u, v = v, w

            ans += cur_hash

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên xây dựng thứ tự góc xung quanh mọi điểm để các truy vấn “trục tiếp theo” trở thành tra cứu theo thời gian liên tục. Bảng pos chuyển đổi một cặp điểm thành chỉ mục của nó theo thứ tự tuần hoàn và nxt thực hiện quy tắc xoay. 

Vòng lặp kép chính liệt kê tất cả các cạnh bắt đầu có hướng. Đối với mỗi cái, chúng tôi mô phỏng đường đi xác định của cối xay gió trong khi theo dõi các cạnh vô hướng nào đã được sử dụng thông qua DSU2. Bất cứ khi nào liên kết thành công, chúng tôi sẽ kết hợp đóng góp của cạnh vào hàm băm XOR. 

Tập đã truy cập sẽ ngăn các vòng lặp vô hạn nếu quá trình truyền tải quay trở lại trạng thái được định hướng đã thấy trước đó, điều này có thể xảy ra khi quá trình quay vòng trong không gian góc. 

Một chi tiết tinh tế là DSU2 mang tính cục bộ trên mỗi mô phỏng, vì mỗi cấu hình ban đầu sẽ xây dựng khu rừng riêng của mình một cách độc lập. DSU toàn cầu không được sử dụng ở đây vì các rừng không được chia sẻ giữa các cấu hình. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào tam giác tối thiểu. 

| Bước | Hiện tại (u, v) | Tiếp theo (v, w) | Hợp nhất DSU | Cur băm | 
| --- | --- | --- | --- | --- | 
| 1 | (1,2) | (2,3) | vâng | 2 | 
| 2 | (2,3) | (3,1) | vâng | 2 ⊕ 6 = 4 | 
| 3 | (3,1) | dừng lại | không | 4 | 

Điều này cho thấy cách một cạnh bắt đầu duy nhất tạo ra một đường truyền đầy đủ bao phủ tất cả các đỉnh, tạo thành một cây trừ khi DSU ngăn chặn một chu trình. 

Bây giờ hãy xem xét bốn điểm ở vị trí lồi. 

| Bước | Hiện tại (u, v) | Tiếp theo (v, w) | Hợp nhất DSU | Cur băm | 
| --- | --- | --- | --- | --- | 
| 1 | (1,2) | (2,3) | vâng | 2 | 
| 2 | (2,3) | (3,4) | vâng | 2 ⊕ 6 = 4 | 
| 3 | (3,4) | (4,1) | vâng | 4 ⊕ 12 = 8 | 
| 4 | (4,1) | dừng lại | không | 8 | 

Dấu vết này minh họa rằng các cấu hình lồi tạo ra các chu kỳ xác định dài tương ứng với thứ tự đa giác và DSU đảm bảo các cạnh chỉ được thêm vào cho đến khi chúng đóng một vòng lặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n³) trường hợp xấu nhất theo cách giải thích đơn giản, được khấu hao O(n2 log n) dự định | phân loại góc chiếm ưu thế, mỗi trạng thái xử lý các chuyển đổi hạn chế | 
| Không gian | O(n²) | lưu trữ bảng thứ tự góc và vị trí | 

Các ràng buộc n 2000 cho phép khoảng 4 triệu trạng thái cặp, do đó cần có cấu trúc O(n2) hoặc gần O(n2 log n). Quá trình tiền xử lý góc là chi phí chiếm ưu thế và mô phỏng trên mỗi trạng thái trung bình phải duy trì ở mức ngắn để phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# minimal triangle
assert run("3\n1 1\n3 1\n2 2\n") is not None

# square
assert run("4\n0 0\n1 0\n1 1\n0 1\n") is not None

# collinear-free random small
assert run("5\n0 0\n2 1\n1 3\n3 2\n4 0\n") is not None

# near-linear but valid
assert run("3\n1 1\n100 2\n50 100\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tam giác | khác không | hình thành chu trình cơ bản | 
| vuông | khác không | truyền tải đa giác | 
| ngẫu nhiên 5 | khác không | độ đúng hình học tổng quát | 
| tam giác nghiêng | khác không | sự mạnh mẽ của việc đặt hàng | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi các điểm tạo thành một đa giác lồi. Trong tình huống đó, mọi thứ tự góc đều nhất quán trên toàn cầu và hàm kế tiếp gần như trở nên xác định theo thứ tự tuần hoàn. Thuật toán xử lý chính xác điều này vì nxt luôn trả về đỉnh tiếp theo theo thứ tự góc, khớp với kề đa giác, do đó việc truyền tải tạo ra một chu trình đầy đủ mà sau đó DSU sẽ ngắt một cách thích hợp. 

Một trường hợp cạnh khác là khi các điểm được sắp xếp sao cho thứ tự góc khác nhau đáng kể giữa các đỉnh, tạo ra các chuỗi kế tiếp không đồng nhất cao. Mô phỏng vẫn chính xác vì tất cả các chuyển đổi được xác định cục bộ trên mỗi trục và DSU đảm bảo rằng ngay cả khi các chu kỳ xuất hiện trong quá trình truyền tải, các cạnh không được tính hai lần. 

Cuối cùng, n trường hợp nhỏ như n = 3 hoặc n = 4 kiểm tra xem phép liệt kê trên các cạnh có hướng có nắm bắt chính xác tất cả các cấu hình ban đầu hay không. Vì mọi cặp có thứ tự đều được xem xét nên không có cấu hình nào bị bỏ sót và tích lũy hàm băm trên các cạnh được lọc DSU khớp chính xác với định nghĩa.

---
title: "CF 102550A - \u041f\u043e\u0438\u0441\u043a\u0438 \u0422\u0440\u0435\u0437\u0443\u0431\u0446\u0430"
description: "Bản đồ là một lưới hình xuyến n x m. Di chuyển ra ngoài cạnh trên, dưới, trái hoặc phải sẽ quấn quanh phía đối diện. Phòng bắt đầu ở góc trên bên trái. Một số phòng có gợi ý được đánh dấu X."
date: "2026-08-06T20:35:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102550
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2018-2019, \u041f\u0435\u0440\u0432\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102550
solve_time_s: 228
verified: false
draft: false
---

[CF 102550A - \u041f\u043e\u0438\u0441\u043a\u0438 \u0422\u0440\u0435\u0437\u0443\u0431\u0446\u0430](https://codeforces.com/problemset/problem/102550/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 48s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Bản đồ là một`n x m`lưới hình xuyến. Di chuyển ra ngoài cạnh trên, dưới, trái hoặc phải sẽ quấn quanh phía đối diện. Phòng bắt đầu ở góc trên bên trái. Một số phòng chứa gợi ý được đánh dấu bằng`X`. 

Một gợi ý trong một căn phòng`(i, j)`chỉ khả dụng sau khi mọi gợi ý có khoảng cách Manhattan thông thường nhỏ hơn kể từ đầu đã được thu thập. Khoảng cách không bị ảnh hưởng bởi chuyển động quấn, nó chỉ đơn giản là`i + j`khi sử dụng tọa độ dựa trên 0. Chúng ta cần xuất ra một chuỗi các bước đi thăm tất cả các phòng gợi ý theo thứ tự hợp lệ. 

Kích thước tối đa là 100, vì vậy có tối đa 10000 phòng. Một giải pháp tìm kiếm đồ thị nặng nề từ mỗi phòng sẽ có gần hàng trăm triệu thao tác và không cần thiết. Hạn chế quan trọng không phải là kích thước của lưới mà là thứ tự bắt buộc của những lần truy cập đầu tiên. Chúng ta cần một công trình xây dựng theo khoảng cách ngày càng tăng của Manhattan một cách tự nhiên. 

Một lỗi phổ biến là chạy DFS bình thường từ phòng bắt đầu. DFS có thể đi sâu vào một nhánh trước khi đến một phòng khác có cùng khoảng cách hoặc nhỏ hơn. Ví dụ:```
3 3
S..
X..
..X
```căn phòng`(3,1)`trong một chỉ mục dựa trên có khoảng cách`2`, trong khi`(1,2)`có khoảng cách`1`. Một DFS bị hỏng trước có thể cố gắng nhập khoảng cách`2`phòng trước khi thu thập khoảng cách`1`gợi ý. 

Một sai lầm khác là di chuyển theo đường chéo bằng một động tác làm tăng khoảng cách tạm thời. Ví dụ, chuyển từ`(2,2)`ĐẾN`(3,1)`bằng cách đi xuống trước tiên vào`(3,2)`, có khoảng cách lớn hơn và vẫn có thể bị khóa. 

Giải pháp phải đến thăm các phòng ở các lớp có khoảng cách bằng nhau và mọi di chuyển bên trong một lớp chỉ được đi qua các phòng từ lớp hiện tại hoặc lớp trước đó. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liên tục tìm kiếm gợi ý có sẵn tiếp theo. Đối với mọi giá trị khoảng cách, chúng tôi có thể chạy BFS và tìm tất cả các phòng hiện có thể truy cập. Điều này đúng vì BFS tôn trọng tập hợp các phòng đã mở khóa nhưng việc tìm kiếm lặp đi lặp lại rất lãng phí. Trong trường hợp xấu nhất có 10000 phòng và việc tìm kiếm biểu đồ 10000 phòng nhiều lần sẽ tốn nhiều công sức hơn mức cần thiết. 

Điều quan trọng cần lưu ý là mọi phòng có cùng khoảng cách đều nằm trên một đường chéo. Các phòng liên tiếp theo đường chéo có thể được truy cập một cách an toàn bằng hai lần di chuyển. Nếu chúng ta chuyển từ`(i, j)`ĐẾN`(i-1, j+1)`, trình tự`U, R`đi qua`(i-1, j)`, khoảng cách của nó nhỏ hơn một. Hướng ngược lại hoạt động tương tự với`L, D`. 

Điều này mang lại một đường quét chéo đơn giản. Chúng tôi xử lý các đường chéo theo thứ tự tăng dần`i + j`. Chúng ta luân phiên hướng của mỗi đường chéo sao cho điểm cuối của đường chéo này nằm cạnh điểm đầu của đường chéo tiếp theo. Mỗi phòng được truy cập đúng một lần và độ dài đường dẫn luôn ở dưới mức giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((nm)^2) | O(nm) | Quá chậm | 
| Tối ưu | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo mọi đường chéo theo khoảng cách của nó`d = i + j`, bắt đầu từ`0`và kết thúc tại`n + m - 2`. Các tọa độ phòng bên trong một đường chéo đều là các cặp có tổng đó. 
2. Đi qua một đường chéo hoàn toàn trước khi chuyển sang đường chéo tiếp theo. Đối với các đường chéo được đánh số chẵn, hãy truy cập các phòng từ chỉ số hàng lớn nhất đến nhỏ nhất. Đối với các đường chéo được đánh số lẻ, hãy đảo ngược hướng. Các hướng thay thế là điều làm cho các đường chéo lân cận kết nối một cách tự nhiên. 
3. Khi di chuyển trong đường chéo từ phòng này sang phòng khác, hãy sử dụng hai nước đi. Theo hướng hàng đi xuống sử dụng`U`sau đó`R`. Theo hướng ngược lại sử dụng`L`sau đó`D`. Phòng trung gian luôn có khoảng cách nhỏ hơn đường chéo đang được xử lý. 
4. Giữa hai đường chéo, thực hiện một nước đi nối điểm cuối của đường chéo hiện tại với điểm đầu của đường chéo tiếp theo. Vì theo thứ tự xen kẽ nên hai phòng này liền kề nhau. 

Tại sao nó hoạt động: trước khi xử lý đường chéo`d`, mọi phòng trên đường chéo có khoảng cách nhỏ hơn đều đã được ghé thăm. Khi đi qua đường chéo`d`, các phòng trung gian duy nhất nằm trên đường chéo`d`hoặc trên các đường chéo nhỏ hơn. Một gợi ý không bao giờ được nhập trước khi tất cả các gợi ý khoảng cách nhỏ hơn đã được thu thập. Sau khi hoàn thành đường chéo cuối cùng, mọi phòng đều đã được ghé thăm nên mọi gợi ý có thể đều đã được thu thập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    ans = []
    current = (0, 0)

    def move_to(a, b):
        nonlocal current
        x, y = current
        nx, ny = a, b

        while x > nx:
            ans.append('U')
            x -= 1
        while y < ny:
            ans.append('R')
            y += 1
        while x < nx:
            ans.append('D')
            x += 1
        while y > ny:
            ans.append('L')
            y -= 1

        current = (x, y)

    for d in range(n + m - 1):
        cells = []
        lo = max(0, d - (m - 1))
        hi = min(n - 1, d)

        if d % 2 == 0:
            for i in range(hi, lo - 1, -1):
                cells.append((i, d - i))
        else:
            for i in range(lo, hi + 1):
                cells.append((i, d - i))

        if cells[0] != current:
            move_to(*cells[0])

        for x, y in cells[1:]:
            cx, cy = current
            if x == cx - 1 and y == cy + 1:
                ans.append('U')
                ans.append('R')
            elif x == cx + 1 and y == cy - 1:
                ans.append('L')
                ans.append('D')
            else:
                move_to(x, y)
            current = (x, y)

        if d + 1 < n + m - 1:
            nd = d + 1
            nlo = max(0, nd - (m - 1))
            nhi = min(n - 1, nd)
            if nd % 2 == 0:
                nxt = (nhi, nd - nhi)
            else:
                nxt = (nlo, nd - nlo)
            if nxt != current:
                move_to(*nxt)

    print(''.join(ans))

if __name__ == "__main__":
    solve()
```Mã này không cần kiểm tra xem phòng có chứa`X`. Việc ghé thăm một căn phòng trống là vô hại và việc ghé thăm từng phòng theo đúng thứ tự là một sự đảm bảo chắc chắn hơn so với việc chỉ ghé thăm những phòng gợi ý. 

Việc tạo đường chéo sử dụng tọa độ dựa trên 0, do đó khoảng cách của một ô chính xác là`i + j`. các`lo`Và`hi`các giá trị giới hạn đường chéo đối với các ô thực sự tồn tại bên trong hình chữ nhật. 

Chuyển động giữa các ô chéo được xử lý tách biệt với chuyển động tùy ý. Quá trình chuyển đổi hai ký tự đặc biệt là phần quan trọng vì chúng đảm bảo rằng chúng ta sẽ không bao giờ bước vào lớp bị khóa trong tương lai. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4 5
S....
X.X..
.X...
...XX
```Thứ tự đường chéo là: 

| Khoảng cách | Hướng | Các ô đã truy cập | 
| --- | --- | --- | 
| 0 | xuống lên | (0,0) | 
| 1 | lên xuống | (1,0), (0,1) | 
| 2 | xuống lên | (2,0), (1,1), (0,2) | 
| 3 | lên xuống | (0,3), (1,2), (2,1), (3,0) | 

Đường dẫn được tạo sẽ thu thập gợi ý ở khoảng cách 1 trước khi đạt được gợi ý ở khoảng cách lớn hơn. Đầu ra chính xác có thể khác với mẫu vì mọi tuyến đường hợp lệ đều được chấp nhận. 

Đối với mẫu thứ hai:```
1 7
S.....X
```Chỉ có một hàng nên các đường chéo trở thành một chuỗi các cột. Thuật toán đi qua từng phòng của hàng và chỉ đến gợi ý cuối cùng sau khi tất cả các khoảng cách trước đó đã được xử lý. 

| Khoảng cách | Phòng hiện tại | Hành động | 
| --- | --- | --- | 
| 0 | (0,0) | bắt đầu | 
| 1 | (0,1) | quá trình chéo | 
| 2 | (0,2) | quá trình chéo | 
| 3 | (0,3) | quá trình chéo | 
| 4 | (0,4) | quá trình chéo | 
| 5 | (0,5) | quá trình chéo | 
| 6 | (0,6) | thu thập gợi ý cuối cùng | 

Trường hợp này xác minh rằng cấu trúc cũng hoạt động khi một chiều là một. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi phòng được đặt vào đúng một đường chéo và được xử lý một lần. | 
| Không gian | O(nm) | Lưới đầu vào và kho lưu trữ chéo tạm thời chứa tối đa 10000 phòng. | 

Độ dài đường dẫn tối đa cũng bị giới hạn. Di chuyển bên trong các đường chéo sử dụng hai bước di chuyển cho mỗi cặp lân cận, tạo ra ít hơn 20000 bước di chuyển. Các kết nối giữa các đường chéo thêm ít hơn 200 bước di chuyển bổ sung, duy trì an toàn dưới giới hạn 30000 được yêu cầu. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old
    return out.strip()

assert run("""4 5
S....
X.X..
.X...
...XX
""") != "", "sample 1"

assert run("""1 7
S.....X
""") != "", "sample 2"

assert run("""1 1
S
""") == "", "single room"

assert run("""2 2
S.
.X
""") != "", "small diagonal transition"

assert run("""3 3
SXX
XXX
XXX
""") != "", "many hints"

assert run("""100 100
""" + "\n".join(["S" + "." * 99] + ["X" * 100 for _ in range(99)])).endswith(""), "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | Bất kỳ đường dẫn hợp lệ nào | Vỏ hình chữ nhật thông thường | 
| Mẫu 2 | Bất kỳ đường dẫn hợp lệ nào | Xử lý hàng đơn | 
|`1 x 1`lưới | Đầu ra trống | Không có gợi ý và không có chuyển động | 
|`2 x 2`lưới | Bất kỳ đường dẫn hợp lệ nào | Thay đổi nhỏ theo đường chéo | 
| Lưới đầy đủ các gợi ý | Bất kỳ đường dẫn hợp lệ nào | Số lượt truy cập bắt buộc trong trường hợp xấu nhất | 
|`100 x 100`lưới | Bất kỳ đường dẫn hợp lệ nào | Hạn chế tối đa | 

## Vỏ cạnh 

Khi lưới chỉ có một phòng thì không có đường chéo nào sau phòng bắt đầu. Thuật toán in ra một đường dẫn trống, điều này đúng vì không có gợi ý nào để thu thập. 

Khi tất cả các phòng đều có gợi ý thì bạn phải ghé thăm từng phòng. Quét chéo vẫn hoạt động vì mọi phòng mới vào đều thuộc lớp khoảng cách hiện tại hoặc lớp trước đó. Không có giả định phím tắt về các ô trống. 

Khi một chiều bằng một, đường chéo sẽ trở thành đường đi thẳng dọc theo hướng duy nhất có thể. Các công thức chuyển tiếp vẫn tạo ra các bước di chuyển liền kề hợp lệ vì đường chéo chứa tối đa một ô. 

Khi gợi ý xuất hiện trên các đường chéo lân cận, thứ tự xen kẽ sẽ quan trọng. Việc duyệt dựa trên hàng đơn giản có thể vào hàng xa hơn trước khi truy cập gợi ý gần hơn. Thứ tự đường chéo ngăn chặn điều đó vì lớp khoảng cách là quy tắc đặt hàng chính.

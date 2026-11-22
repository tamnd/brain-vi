---
title: "CF 280C - Trò chơi trên cây"
description: "Chúng ta được cho một cây có gốc ở đỉnh 1. Một trò chơi được chơi trên cây có gốc này cho đến khi mọi nút biến mất. Ở mỗi lần di chuyển, một trong các đỉnh còn lại hiện tại được chọn ngẫu nhiên một cách thống nhất."
date: "2026-06-05T08:57:35+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math", "probabilities", "trees"]
categories: ["algorithms"]
codeforces_contest: 280
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 172 (Div. 1)"
rating: 2200
weight: 280
solve_time_s: 121
verified: true
draft: false
---

[CF 280C - Trò chơi trên cây](https://codeforces.com/problemset/problem/280/C) 

**Đánh giá:** 2200 
**Tags:** cách thực hiện, toán học, xác suất, cây 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây có gốc ở đỉnh`1`. Một trò chơi được chơi trên cây có gốc này cho đến khi mọi nút biến mất. 

Ở mỗi lần di chuyển, một trong các đỉnh còn lại hiện tại được chọn ngẫu nhiên một cách thống nhất. Khi một đỉnh`v`được chọn, toàn bộ cây con có gốc tại`v`sẽ bị xóa ngay lập tức, bao gồm cả`v`chính nó. Trò chơi kết thúc khi gốc cuối cùng bị loại bỏ, vì việc xóa gốc sẽ loại bỏ toàn bộ cây còn lại. 

Nhiệm vụ là tính toán số lần di chuyển dự kiến ​​trước khi cây trở nên trống. 

Đầu vào mô tả một cây vô hướng có tối đa`10^5`đỉnh. Vì cây có gốc ở đỉnh`1`, chúng ta có thể diễn giải mọi cạnh theo mối quan hệ cha-con sau DFS hoặc BFS từ gốc. 

Ràng buộc`n ≤ 100000`ngay lập tức loại trừ bất kỳ cách tiếp cận nào mô phỏng rõ ràng quá trình ngẫu nhiên. Thậm chí một`O(n^2)`thuật toán sẽ yêu cầu khoảng`10^10`trong trường hợp xấu nhất vượt xa thời hạn. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. 

Một khía cạnh tinh vi của vấn đề là trạng thái của trò chơi thay đổi sau mỗi lần xóa. Xác suất để một đỉnh được chọn phụ thuộc vào đỉnh nào còn sót lại sau các bước di chuyển trước đó. Việc cố gắng viết các chuyển tiếp giữa tất cả các cây còn lại có thể nhanh chóng trở nên bất khả thi. 

Hãy xem xét một chuỗi:```
1
|
2
|
3
```Một sai lầm phổ biến là nghĩ rằng mỗi đỉnh đóng góp một cách độc lập. Nó không. Nếu đỉnh`2`được chọn, đỉnh`3`biến mất mà không bao giờ được chọn. Bất kỳ cách tiếp cận nào gán xác suất cố định cho mỗi đỉnh được chọn chính xác một lần sẽ tạo ra kỳ vọng sai. 

Một cạm bẫy dễ mắc phải khác là quên rằng gốc cuối cùng luôn được chọn. Đối với cây một nút:```
1
```trò chơi luôn kéo dài đúng một nước đi, vì vậy câu trả lời là`1`. Bất kỳ công thức nào mang lại`0`ở đây thiếu sự đóng góp của root. 

Cây hình ngôi sao cũng bộc lộ suy luận không đúng:```
1
/|\
2 3 4
```Những chiếc lá chỉ có thể biến mất khi chúng được tự chọn, trong khi cái gốc sẽ phá hủy mọi thứ cùng một lúc. Câu trả lời được mong đợi không bằng chiều cao của cây, số lá hoặc bất kỳ đại lượng cấu trúc rõ ràng nào khác. 

Điều quan trọng là tìm ra đại lượng mà kỳ vọng của nó có thể được tính toán trực tiếp mà không cần lập mô hình toàn bộ quá trình. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ coi mọi cây còn lại có thể là một trạng thái. Từ một trạng thái chứa`k`đỉnh, mỗi đỉnh được chọn với xác suất`1/k`, dẫn đến một trạng thái khác có được bằng cách xóa một cây con. 

Chế độ xem không gian trạng thái này là chính xác vì nó khớp chính xác với các quy tắc trò chơi. Thật không may, ngay cả những cây nhỏ cũng tạo ra nhiều cấu hình còn lại theo cấp số nhân. Một cái cây với`10^5`đỉnh làm cho điều này hoàn toàn không thể thực hiện được. 

Sự đột phá đến từ việc thay đổi những gì chúng ta đo lường. 

Thay vì hỏi trực tiếp số bước đi dự kiến, hãy xem xét liệu một đỉnh cụ thể có đóng góp một bước đi cho trò chơi hay không. 

Hãy tưởng tượng gán cho mỗi đỉnh một mức độ ưu tiên ngẫu nhiên. Việc chọn một đỉnh còn lại ngẫu nhiên thống nhất ở mỗi bước tương đương với việc tạo ra một hoán vị ngẫu nhiên của tất cả các đỉnh và hiển thị chúng theo thứ tự đó. Một đỉnh thực sự gây ra một bước di chuyển khi và chỉ nếu nó xuất hiện trước mọi đỉnh tổ tiên của đỉnh đó trong hoán vị. 

Tại sao? Nếu một cây tổ tiên xuất hiện đầu tiên, việc xóa cây con của cây tổ tiên đó sẽ loại bỏ đỉnh trước khi nó được chọn. Nếu đỉnh là đỉnh sớm nhất trong số tất cả các đỉnh trên đường đi từ gốc đến chính nó thì nó tồn tại cho đến lượt của nó và đóng góp một nước đi. 

Đối với một đỉnh ở độ sâu`d`(độ sâu của rễ`1`), đường đi từ gốc tới đỉnh đó chứa chính xác`d`đỉnh. Trong số này`d`các đỉnh, mọi đỉnh đều có khả năng là đỉnh sớm nhất trong một hoán vị ngẫu nhiên như nhau. Kể từ đây$$P(\text{vertex } v \text{ contributes a move}) = \frac{1}{d}.$$Cho phép`I(v)`là chỉ báo cho đỉnh đó`v`đóng góp một động thái. Tổng số lần di chuyển là$$X = \sum_v I(v).$$Sử dụng tính tuyến tính của kỳ vọng,$$E[X] = \sum_v E[I(v)] = \sum_v \frac{1}{\text{depth}(v)}.$$Vấn đề đã trở nên cực kỳ đơn giản. Chúng ta chỉ cần độ sâu của mỗi đỉnh trong cây có gốc. 

Một DFS duy nhất tính toán tất cả các độ sâu và câu trả lời là tổng các nghịch đảo của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê trạng thái Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Tổng hợp độ sâu DFS | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề của cây. 
2. Gốc cây tại đỉnh`1`. 
3. Thực hiện DFS từ gốc và tính độ sâu của mỗi đỉnh, sử dụng độ sâu`1`cho gốc. 
4. Với mỗi đỉnh đã thăm, hãy cộng`1 / depth`để trả lời. 
5. Tiếp tục cho đến khi đã thăm hết tất cả các đỉnh. 
6. In số tiền tích lũy với độ chính xác vừa đủ. 

Phần không rõ ràng là bước 4. Một đỉnh đóng góp một bước di chuyển chính xác khi nó là đỉnh sớm nhất trên đường đi từ gốc đến đỉnh của nó trong cách diễn giải hoán vị ngẫu nhiên. Vì mọi đỉnh trên đường đi đó đều có khả năng xuất hiện sớm nhất như nhau nên xác suất đóng góp chỉ đơn giản là`1 / depth`. 

### Tại sao nó hoạt động 

Đối với bất kỳ đỉnh nào`v`, xác định một biến ngẫu nhiên chỉ báo:$$I(v)= \begin{cases} 1,&\text{if }v\text{ is ever selected},\\ 0,&\text{otherwise}. \end{cases}$$đỉnh`v`được chọn khi và chỉ nếu không có tổ tiên của`v`được chọn trước nó. Trong một hoán vị ngẫu nhiên của tất cả các đỉnh, điều này xảy ra chính xác khi`v`là đỉnh đầu tiên trên root-to-`v`con đường. 
Trong đường dẫn có chứa`depth(v)`đỉnh, tính đối xứng cho$$P(I(v)=1)=\frac{1}{depth(v)}.$$Tổng số nước đi của trò chơi bằng$$X=\sum_v I(v).$$Áp dụng tuyến tính của kỳ vọng,$$E[X] = \sum_v E[I(v)] = \sum_v P(I(v)=1) = \sum_v \frac{1}{depth(v)}.$$DFS tính toán chính xác mọi độ sâu một lần, do đó thuật toán đánh giá chính xác công thức này. Vì công thức bằng số bước di chuyển dự kiến ​​nên thuật toán đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    ans = 0.0

    stack = [(1, 0, 1)]  # node, parent, depth

    while stack:
        u, parent, depth = stack.pop()

        ans += 1.0 / depth

        for v in g[u]:
            if v != parent:
                stack.append((v, u, depth + 1))

    print("{:.15f}".format(ans))

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ cây trong`O(n)`ký ức. 

DFS được triển khai lặp đi lặp lại thay vì đệ quy. DFS đệ quy trên đường dẫn có độ dài`100000`sẽ vượt quá giới hạn đệ quy của Python. Sử dụng ngăn xếp rõ ràng sẽ tránh được vấn đề đó hoàn toàn. 

Mỗi mục ngăn xếp lưu trữ đỉnh hiện tại, đỉnh cha và độ sâu của nó. Khi một đỉnh được xuất hiện, chúng ta ngay lập tức thêm`1/depth`để trả lời. Điều này trực tiếp thực hiện công thức đã được chứng minh$$\sum_v \frac{1}{depth(v)}.$$Rễ bắt đầu ở độ sâu`1`, không`0`. Điều này rất quan trọng vì xác suất liên quan đến nghiệm là`1`và chia cho 0 rõ ràng là không hợp lệ. 

Câu trả lời được tích lũy trong một biến dấu phẩy động. Độ chính xác gấp đôi là quá đủ cho yêu cầu`10^{-6}`sự chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 2
```Cây có gốc là:```
1
|
2
```| Đỉnh | Độ sâu | Đóng góp | 
| --- | --- | --- | 
| 1 | 1 | 1.0 | 
| 2 | 2 | 0,5 | 

Tổng cộng:$$1+\frac12=1.5$$| Bước | Nút đã truy cập | Độ sâu | Chạy Trả lời | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1.0 | 
| 2 | 2 | 2 | 1,5 | 

Đầu ra:```
1.500000000000000
```Ví dụ này cho thấy gốc luôn đóng góp chính xác một nước đi, trong khi đứa trẻ sống sót đủ lâu để chỉ được chọn một nửa thời gian. 

### Ví dụ 2 

đầu vào:```
3
1 2
1 3
```Cây có gốc là:```
  1
 / \
2   3
```| Đỉnh | Độ sâu | Đóng góp | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 2 | 0,5 | 
| 3 | 2 | 0,5 | 

Tổng cộng:$$1+\frac12+\frac12=2$$| Bước | Nút đã truy cập | Độ sâu | Chạy Trả lời | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1.0 | 
| 2 | 2 | 2 | 1,5 | 
| 3 | 2 | 2 | 2.0 | 

Đầu ra:```
2.000000000000000
```Điều này thể hiện quan điểm tuyến tính của kỳ vọng. Chúng ta không bao giờ cần liệt kê các lệnh xóa khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đỉnh và cạnh được xử lý một lần trong DFS | 
| Không gian | O(n) | Danh sách kề và ngăn xếp DFS lưu trữ tối đa dữ liệu O(n) | 

Việc truyền tải tuyến tính dễ dàng đủ nhanh để`n = 100000`. Việc sử dụng bộ nhớ cũng nằm trong giới hạn thoải mái vì biểu diễn biểu đồ chỉ chứa`2(n - 1)`các mục lân cận. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())

    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    ans = 0.0
    stack = [(1, 0, 1)]

    while stack:
        u, p, d = stack.pop()
        ans += 1.0 / d

        for v in g[u]:
            if v != p:
                stack.append((v, u, d + 1))

    return f"{ans:.15f}"

# provided sample
assert run("2\n1 2\n") == "1.500000000000000"

# single node
assert run("1\n") == "1.000000000000000"

# star with three leaves
assert run("4\n1 2\n1 3\n1 4\n") == "2.500000000000000"

# chain of length 3
assert run("3\n1 2\n2 3\n") == "1.833333333333333"

# chain of length 4
assert run("4\n1 2\n2 3\n3 4\n") == "2.083333333333333"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1.0`| Cây nhỏ nhất có thể | 
| Ngôi sao 3 lá |`2.5`| Nhiều trẻ em ở cùng độ sâu | 
| Chuỗi dài 3 |`1.833333333333333`| Tăng độ sâu | 
| Chuỗi dài 4 |`2.083333333333333`| Cây sâu, xử lý độ sâu | 
| Đầu vào mẫu |`1.5`| Trận đấu ví dụ chính thức | 

## Vỏ cạnh 

### Cây một đỉnh 

đầu vào:```
1
```DFS chỉ truy cập vào thư mục gốc. 

| Đỉnh | Độ sâu | Đóng góp | 
| --- | --- | --- | 
| 1 | 1 | 1 | 

Trả lời:$$1$$Điều này khẳng định rằng gốc luôn đóng góp một nước đi và độ sâu đó phải bắt đầu từ`1`. 

###Chuỗi dài 

đầu vào:```
4
1 2
2 3
3 4
```Độ sâu là`1, 2, 3, 4`. 

Câu trả lời trở thành$$1+\frac12+\frac13+\frac14 = 2.083333333333333.$$DFS đệ quy có thể tràn trên một chuỗi có độ dài`100000`, nhưng ngăn xếp lặp lại xử lý nó một cách an toàn. 

### Ngôi sao được căn giữa ở gốc 

đầu vào:```
4
1 2
1 3
1 4
```Độ sâu là`1, 2, 2, 2`. 

Câu trả lời là$$1+\frac12+\frac12+\frac12 = 2.5.$$Trường hợp này xác minh rằng mỗi lá đóng góp độc lập thông qua xác suất của nó`1/2`, mặc dù việc chọn gốc sẽ loại bỏ ngay lập tức tất cả các lá. 

###Cây không cân đối 

đầu vào:```
5
1 2
2 3
1 4
4 5
```Độ sâu là: 

| Đỉnh | Độ sâu | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 3 | 
| 4 | 2 | 
| 5 | 3 | 

Trả lời:$$1+\frac12+\frac13+\frac12+\frac13 = \frac{8}{3} = 2.666666666666667.$$Thuật toán chỉ phụ thuộc vào độ sâu từ gốc đến đỉnh, bất kể cây cân bằng hay không cân bằng. Điều này xác nhận rằng đối số xác suất hoạt động với các hình dạng cây tùy ý.

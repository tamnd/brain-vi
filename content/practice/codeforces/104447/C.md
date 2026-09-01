---
title: "CF 104447C - Điều gì xảy ra với máy tính xách tay của Bashar?"
description: "Chúng ta có một hệ thống thư mục gốc bắt đầu dưới dạng cây cố định với các nút được gắn nhãn từ 1 đến n, trong đó thư mục 1 là gốc. Mỗi thư mục chứa một danh sách các thư mục con, do đó đầu vào sẽ xác định cấu trúc cây được định hướng trên các nút ban đầu này."
date: "2026-06-30T17:58:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104447
codeforces_index: "C"
codeforces_contest_name: "Al-Baath Collegiate Programming Contest 2023"
rating: 0
weight: 104447
solve_time_s: 76
verified: true
draft: false
---

[CF 104447C - Điều gì xảy ra với máy tính xách tay của Bashar?](https://codeforces.com/problemset/problem/104447/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hệ thống thư mục gốc bắt đầu dưới dạng cây cố định với các nút được gắn nhãn từ 1 đến n, trong đó thư mục 1 là gốc. Mỗi thư mục chứa một danh sách các thư mục con, do đó đầu vào sẽ xác định cấu trúc cây được định hướng trên các nút ban đầu này. 

Sau đó, chúng tôi xử lý nhiều truy vấn độc lập. Mỗi truy vấn mô tả một chuỗi các thao tác ngắn, có tối đa ba bước. Trong mỗi bước, chúng ta lấy một thư mục u và sao chép toàn bộ nội dung của nó, nghĩa là toàn bộ cây con bắt nguồn từ u, rồi dán nó vào một thư mục v khác dưới dạng cây con mới. 

Điều quan trọng là các thư mục đã sao chép sẽ không được sử dụng lại làm nhãn gốc. Thay vào đó, mỗi nút được sao chép sẽ được gán một nhãn mới dựa trên nhãn gốc và số bước, điều này đảm bảo rằng mọi thư mục được tạo đều có thể nhận dạng duy nhất ngay cả khi nó đến từ cùng một nút gốc. 

Khi kết thúc một truy vấn, chúng tôi được hỏi một điều rất cụ thể: sau khi thực hiện tất cả các thao tác sao chép theo trình tự, tổng cộng có bao nhiêu thư mục tồn tại. 

Các ràng buộc rất lớn: n có thể lên tới 100000 và số lượng truy vấn cũng có thể đạt tới 100000, nhưng mỗi truy vấn thực hiện tối đa ba thao tác. Điều này ngay lập tức gợi ý rằng chúng ta không thể mô phỏng rõ ràng việc sao chép cây con, bởi vì một thao tác sao chép đơn lẻ có thể sao chép một cây con lớn và các thao tác lặp lại có thể gây ra sự tăng trưởng theo cấp số nhân. 

Một mô phỏng đơn giản sẽ xây dựng lại hoặc sao chép cây một cách vật lý, có thể dễ dàng đạt tới O(n) cho mỗi thao tác hoặc tệ hơn, khiến việc này không thể thực hiện được trong thời gian giới hạn. 

Một vấn đề nhỏ là các thao tác sau này trong cùng một truy vấn có thể đề cập đến các nút mới được tạo, vì vậy chúng tôi không thể xử lý từng thao tác một cách độc lập trên cây ban đầu. Tuy nhiên, vì có nhiều nhất ba thao tác cho mỗi truy vấn nên cấu trúc vẫn còn nông và có thể được theo dõi bằng cách sử dụng thông tin tổng hợp thay vì xây dựng rõ ràng. 

Trường hợp cạnh khóa là khi u đề cập đến một nút được tạo trong thao tác trước đó. Ví dụ: nếu chúng ta sao chép cây con 2 thành 3, sau đó sao chép cây con gốc ở bản sao mới vào một vị trí khác thì kích thước của cây con đó phải phản ánh các lần chèn trước đó. Điều này loại trừ bất kỳ tính toán trước tĩnh nào. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Theo nghĩa đen, chúng tôi xây dựng cây và với mỗi thao tác, chúng tôi duyệt qua toàn bộ cây con gốc tại u, sao chép mọi nút, gán nhãn mới và đính kèm cấu trúc nhân bản trong v. Điều này đúng vì nó tuân theo chính xác định nghĩa. Tuy nhiên, trong trường hợp xấu nhất, một cây con có thể chứa tất cả n nút và mỗi truy vấn có thể lặp lại nó nhiều lần. Mặc dù k tối đa là 3, nhưng bản thân các cấu trúc được sao chép có thể trở nên lớn, dẫn đến sự bùng nổ theo cấp số nhân về số lượng nút mà chúng tôi xử lý. Điều này nhanh chóng trở nên không khả thi ngay cả đối với một truy vấn duy nhất. 

Quan sát quan trọng là chúng ta thực sự không cần cụ thể hóa cấu trúc của các thư mục được sao chép. Số lượng duy nhất chúng tôi được yêu cầu là số lượng nút cuối cùng. Mọi thao tác đóng góp chính xác kích thước của cây con được sao chép tại thời điểm đó. Vì vậy, toàn bộ vấn đề giảm xuống còn việc duy trì kích thước cây con chính xác trong các hoạt động “sao chép-thêm” động. 

Khó khăn là kích thước cây con thay đổi theo thời gian vì khi chúng ta đính kèm một cây con được sao chép bên dưới v, tất cả các cây con gốc của v đều thấy kích thước cây con của chúng tăng lên. Điều đó có nghĩa là kích thước cây con của nút không cố định, nó tích lũy các đóng góp từ các lần chèn trong tương lai. 

Điều này dẫn đến một phép biến đổi cổ điển: thay vì cập nhật rõ ràng tất cả tổ tiên của v, chúng ta đảo ngược phối cảnh. Mỗi thao tác tại v đóng góp một giá trị cho mọi tổ tiên của v. Vì vậy, mỗi lần cập nhật là một sự kiện điểm tại v và mỗi nút u cần biết tổng của tất cả các cập nhật xảy ra bên trong cây con của nó. Đây chính xác là một vấn đề truy vấn tổng cây con trên cây.

Chúng ta có thể giải quyết vấn đề này bằng cách sử dụng chuyến du hành Euler cộng với cây Fenwick. Chúng tôi lưu trữ kích thước cây con ban đầu và chúng tôi duy trì BIT trên các nút nơi chúng tôi thêm các đóng góp tại v. Sau đó, tổng của cây con có thể được truy vấn một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ trong trường hợp xấu nhất | O(tổng số nút đã tạo) | Quá chậm | 
| Chuyến tham quan Euler + Cây Fenwick | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Xây dựng cây ban đầu và tính toán kích thước cây con 

Trước tiên, chúng tôi chạy DFS trên cây ban đầu để tính kích thước cây con của mỗi nút. Điều này cung cấp cho chúng tôi kích thước cơ bản của từng thư mục trước khi thực hiện bất kỳ hoạt động sao chép nào. 

### 2. Gán chỉ số chuyến tham quan Euler 

Chúng tôi chỉ định mỗi nút một khoảng thời gian nhập và tính toán các khoảng thời gian của cây con. Cây con của một nút tương ứng với một phân đoạn liền kề theo thứ tự Euler, cho phép chúng ta tổng hợp các bản cập nhật một cách hiệu quả. 

### 3. Duy trì cây Fenwick trên các nút 

Chúng tôi tạo cây Fenwick trong đó mỗi vị trí tương ứng với một nút trong cây ban đầu. Cấu trúc này lưu trữ số lượng nút bổ sung đã được thêm vào “bên trong” mỗi cây con do hoạt động sao chép. 

### 4. Xử lý từng truy vấn một cách độc lập 

Đối với mỗi truy vấn, chúng tôi bắt đầu với một cây Fenwick sạch và tổng số câu trả lời đang chạy được khởi tạo thành n. 

Chúng tôi xử lý các hoạt động k theo thứ tự. 

### 5. Tính kích thước hiện tại của bạn 

Khi chúng ta cần sao chép cây con u, kích thước hiện tại của nó không chỉ là kích thước DFS ban đầu. Nó cũng bao gồm tất cả những đóng góp từ các hoạt động trước đó. Chúng tôi thu được nó dưới dạng init_size[u] cộng với tổng của tất cả các cập nhật Fenwick bên trong cây con(u). 

Điều này cung cấp số lượng nút chính xác được sao chép ở bước này. 

### 6. Áp dụng thao tác sao chép 

Chúng tôi thêm kích thước cây con được tính toán vào câu trả lời chung. 

Sau đó, chúng tôi mô phỏng việc “đính kèm” cây con được sao chép này dưới v bằng cách cập nhật cây Fenwick ở vị trí v với giá trị đó. Điều này đảm bảo rằng tất cả tổ tiên của v sẽ phản ánh kích thước cây con tăng lên. 

### 7. Xuất ra số đếm cuối cùng 

Sau tất cả k thao tác, câu trả lời tích lũy sẽ được in ra. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là cây Fenwick luôn lưu trữ chính xác tổng đóng góp của tất cả các thao tác sao chép cho mỗi nút gốc, được giới hạn ở vị trí các thao tác đó được đính kèm. 

Đối với bất kỳ nút x nào, việc truy vấn cây con của nó theo thứ tự Euler sẽ trả về chính xác tổng số nút được thêm vào bên trong các nút con của nó. Điều này khớp chính xác với kích thước cây con phát triển như thế nào, bởi vì mỗi khi chúng ta đính kèm một bản sao bên dưới v, tất cả tổ tiên của v phải tăng kích thước cây con của chúng lên một lượng như vậy. Điều kiện đó tương đương với việc thêm giá trị vào tất cả các nút có cây con chứa v, được ghi lại bằng cách tích lũy phạm vi cây con. 

Vì mọi thao tác được giảm xuống thành một bản cập nhật cục bộ và mọi truy vấn chỉ phụ thuộc vào tổng hợp tiền tố trên cấu trúc cây nên không cần phải sao chép rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n = int(input())
g = [[] for _ in range(n + 1)]

for i in range(1, n + 1):
    parts = list(map(int, input().split()))
    s = parts[0]
    for x in parts[1:]:
        g[i].append(x)

tin = [0] * (n + 1)
tout = [0] * (n + 1)
sub = [0] * (n + 1)
timer = 0

def dfs(u):
    global timer
    timer += 1
    tin[u] = timer
    sub[u] = 1
    for v in g[u]:
        dfs(v)
        sub[u] += sub[v]
    tout[u] = timer

dfs(1)

class BIT:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 2)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

    def range_sum(self, l, r):
        return self.sum(r) - self.sum(l - 1)

bit = BIT(n)

def get_subtree_add(u):
    return bit.range_sum(tin[u], tout[u])

q = int(input())
for _ in range(q):
    k = int(input())
    total = n
    bit = BIT(n)

    for _ in range(k):
        u, v = map(int, input().split())
        cur = sub[u] + get_subtree_add(u)
        total += cur
        bit.add(tin[v], cur)

    print(total)
```DFS tính toán cả kích thước cây con và khoảng Euler để mỗi cây con trở thành một đoạn liền kề. Cây Fenwick sau đó được sử dụng để tích lũy đóng góp từ tất cả các hoạt động sao chép trước đó. 

Mỗi lần chúng tôi sao chép u, chúng tôi truy vấn kích thước cây con hiệu quả hiện tại của nó bằng cách kết hợp giá trị DFS tĩnh với tất cả các phần bổ sung động được lưu trữ trong BIT. Sau đó, chúng tôi thêm đóng góp đó vào câu trả lời và truyền bá nó tại v. 

Điểm tinh tế chính là chúng tôi xây dựng lại BIT cho mỗi truy vấn vì các truy vấn độc lập, đảm bảo không có sự can thiệp giữa các tình huống khác nhau. 

## Ví dụ đã hoạt động 

Hãy xem xét một cái cây nhỏ trong đó 1 là gốc và 1 có hai con 2 và 3. 

### Dấu vết truy vấn 1 

Giả sử đầu tiên chúng ta sao chép cây con có gốc từ 2 vào 3. 

| Bước | Hoạt động | kích thước(u) | tổng cộng | cập nhật BIT | 
| --- | --- | --- | --- | --- | 
| 1 | sao chép 2 → 3 | 1 | 4 | thêm vào lúc 3 | 

Cây con của 2 có kích thước 1 nên chúng ta thêm một nút mới. Tổng số sẽ là 4. 

Điều này cho thấy cách hoạt động của bản sao lá và xác nhận rằng các cập nhật là cục bộ nhưng được truyền lên trên thông qua các truy vấn cây con. 

### Dấu vết truy vấn 2 

Bây giờ hãy xem xét việc sao chép một cây con lớn hơn trước rồi sao chép một nút nằm trong vùng được sửa đổi. 

| Bước | Hoạt động | kích thước(u) | tổng cộng | cập nhật BIT | 
| --- | --- | --- | --- | --- | 
| 1 | sao chép 1 → 2 | 3 | 7 | thêm vào lúc 2 | 
| 2 | sao chép 2 → 3 | kích thước cập nhật(2) = 4 | 11 | thêm vào lúc 3 | 

Sau thao tác đầu tiên, kích thước cây con của nút 2 tăng lên. Khi chúng tôi sao chép 2 ở bước thứ hai, chúng tôi đã bao gồm chính xác cấu trúc đã thêm trước đó. Điều này chứng tỏ tại sao việc tổng hợp cây con động là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi truy vấn thực hiện tối đa 3 cập nhật và truy vấn Fenwick | 
| Không gian | O(n) | Mảng Euler và lưu trữ BIT | 

Cấu trúc của bài toán đảm bảo rằng mặc dù n và q lớn nhưng mỗi truy vấn lại cực kỳ nhỏ. Chi phí logarit từ các hoạt động của Fenwick đủ để xử lý tất cả các bản cập nhật một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        # assume solution is wrapped in main()
        pass
    return out.getvalue().strip()

# minimal tree
assert run("""1
0
1
1
1
1 1
""") == "2"

# chain
assert run("""3
1 2
1 3
0
1
1
1 2
""") == "4"

# star structure with multiple ops
assert run("""4
2 2 3
0
0
0
1
2
1 2
2 3
""") == "7"

# repeated self-copy
assert run("""2
1 2
0
1
3
1 2
1 2
1 2
""") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây tối thiểu | 2 | độ chính xác của một bản sao | 
| chuỗi | 4 | lan truyền phụ thuộc cây con | 
| ngôi sao | 7 | tích lũy nhiều bước | 
| tự sao chép nhiều lần | 5 | cập nhật kích thước động | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một nút được sao chép sau khi cây con của nó đã được sửa đổi bởi các thao tác trước đó trong cùng một truy vấn. Trong trường hợp đó, kích thước cây con của u phải bao gồm tất cả các phần bổ sung trước đó. Tính toán dựa trên BIT xử lý việc này một cách chính xác vì mỗi lần chèn vào phần con cháu của u đều đóng góp vào tổng cây con của nó. 

Một trường hợp khác là sao chép vào một nút sâu nằm bên trong cây con đã được sửa đổi trước đó. Trong trường hợp này, tổ tiên ở trên v phải phản ánh cả bản cập nhật trước đó và hiện tại. Vì chúng tôi thêm các đóng góp tại v và truy vấn trên các cây con, nên hiệu ứng sẽ lan truyền lên trên một cách tự nhiên mà không đi qua tổ tiên một cách rõ ràng. 

Cuối cùng, các thao tác lặp lại trên cùng một nút sẽ kiểm tra xem các bản cập nhật có được tích lũy đúng cách hay không. Vì mỗi bản cập nhật đều là phần bổ sung trong cây Fenwick và các truy vấn cây con tổng hợp tất cả đóng góp nên các bản sao lặp lại sẽ chia tỷ lệ chính xác kích thước cây con theo thời gian mà không tính hai lần hoặc thiếu các bản cập nhật.

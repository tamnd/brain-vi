---
title: "CF 1213G - Truy vấn đường dẫn"
description: "Chúng ta được cấp một cây có trọng số trong đó mỗi cạnh đều có giá trị và chúng ta được hỏi nhiều câu hỏi về đường đi giữa các cặp đỉnh có thể “đắt” như thế nào. Với hai đỉnh $u$ và $v$ bất kỳ, có chính xác một đường đi đơn giữa chúng vì đồ thị là một cây."
date: "2026-06-13T17:18:52+07:00"
tags: ["codeforces", "competitive-programming", "divide-and-conquer", "dsu", "graphs", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1213
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 582 (Div. 3)"
rating: 1800
weight: 1213
solve_time_s: 271
verified: true
draft: false
---

[CF 1213G - Truy vấn đường dẫn](https://codeforces.com/problemset/problem/1213/G) 

**Đánh giá:** 1800 
**Thẻ:** chia để trị, dsu, đồ thị, phân loại, cây 
**Thời gian giải:** 4 phút 31 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có trọng số trong đó mỗi cạnh đều có giá trị và chúng ta được hỏi nhiều câu hỏi về đường đi giữa các cặp đỉnh có thể “đắt” như thế nào. 

Với hai đỉnh bất kỳ$u$Và$v$, có chính xác một đường đi đơn giữa chúng vì đồ thị là một cây. Dọc theo con đường đó, chúng ta xem xét tất cả các trọng số của cạnh và lấy giá trị lớn nhất. Truy vấn hỏi: trong số tất cả các cặp đỉnh phân biệt, có bao nhiêu cặp có trọng số cạnh tối đa trên đường kết nối của chúng không vượt quá một ngưỡng nhất định$q$. 

Vì vậy, mỗi truy vấn về cơ bản là hỏi: liệu chúng ta chỉ được phép “sử dụng” các cạnh có trọng số tối đa$q$, có bao nhiêu cặp nút được kết nối chỉ qua các cạnh như vậy. 

Giải thích trực tiếp là chúng ta đang đếm các cặp đỉnh nằm trong cùng một thành phần liên thông của đồ thị được hình thành bằng cách chỉ giữ lại các cạnh có trọng số tối đa.$q$. 

Các ràng buộc đi lên đến$2 \cdot 10^5$đỉnh, cạnh và truy vấn. Điều này ngay lập tức loại trừ việc tính toán lại kết nối từ đầu cho mỗi truy vấn. Bất kỳ cách tiếp cận nào cố gắng chạy DFS hoặc BFS cho mỗi truy vấn sẽ dẫn đến gần như$O(m(n + n))$, vượt xa giới hạn khả thi. 

Vấn đề tế nhị thứ hai là các cặp được tính độc lập trên tất cả các truy vấn. Các câu trả lời không tăng dần theo nghĩa ngây thơ trừ khi chúng ta khai thác cấu trúc sắp xếp và tái sử dụng. 

Một sai lầm ngây thơ là diễn giải từng truy vấn riêng biệt và mỗi lần xây dựng lại biểu đồ đã lọc. Ví dụ: nếu tất cả các cạnh đều khác biệt và các truy vấn cũng lớn thì việc truyền tải lặp lại sẽ hết thời gian ngay cả đối với đầu vào vừa phải. 

Một cạm bẫy phổ biến khác là cố gắng tính toán đường dẫn tối đa cho mỗi cặp bằng cách sử dụng tiền xử lý LCA. Trong khi LCA có thể tính toán cạnh tối đa trên một đường đi trong$O(\log n)$, làm điều này cho tất cả$O(n^2)$cặp là không thể. 

## Phương pháp tiếp cận 

Quan điểm vũ phu bắt đầu từ định nghĩa. Đối với mỗi truy vấn$q$, chúng tôi sẽ kiểm tra từng cặp$(u, v)$, tính trọng lượng cạnh tối đa trên đường đi của chúng và đếm các cặp hợp lệ. Ngay cả với quá trình tiền xử lý LCA, mỗi truy vấn đều tốn$O(n^2 \log n)$, điều này hoàn toàn không thể thực hiện được đối với$n = 2 \cdot 10^5$. 

Một chế độ xem có cấu trúc hơn đến từ việc lật ngược phối cảnh. Thay vì hỏi “đối với mỗi cặp, cạnh tối đa trên đường đi của nó là bao nhiêu”, chúng tôi hỏi “đối với mỗi ngưỡng, có bao nhiêu cặp được kết nối khi các cạnh đạt đến ngưỡng đó được kích hoạt”. 

Khi chúng ta tăng trọng lượng cho phép, các cạnh sẽ xuất hiện theo thứ tự tăng dần. Mỗi lần chúng ta thêm một cạnh, nó có thể hợp nhất hai thành phần riêng biệt trước đó. Khi hai thành phần có kích thước$a$Và$b$hợp nhất, họ đóng góp$a \cdot b$các cặp hợp lệ mới, bởi vì mọi đỉnh trong một thành phần giờ đây có thể chạm tới mọi đỉnh trong thành phần kia bằng cách sử dụng các cạnh có trọng số tối đa hiện tại. 

Quan sát này chuyển đổi vấn đề thành một quá trình kết nối động trên các cạnh đã được sắp xếp, đó chính xác là điều mà DSU (kết hợp tập hợp rời rạc) duy trì một cách hiệu quả. Thay vì tính toán lại các thành phần cho mỗi truy vấn, chúng tôi xử lý các cạnh theo trọng số tăng dần và duy trì số lượng cặp kết nối tăng dần. 

Các truy vấn cũng có thể được sắp xếp theo ngưỡng, cho phép chúng ta quét qua cả hai cạnh và các truy vấn theo thứ tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(m n^2)$hoặc tệ hơn |$O(n)$| Quá chậm | 
| DSU + phân loại |$O((n + m)\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các cạnh theo trọng lượng tăng dần trong khi trả lời các truy vấn theo thứ tự tăng dần$q$. 

1. Sắp xếp tất cả các cạnh theo trọng lượng theo thứ tự không giảm. 

Điều này đảm bảo rằng khi chúng ta xử lý một cạnh, tất cả các cạnh được xử lý trước đó đều không nặng hơn cạnh đó. 
2. Sắp xếp truy vấn nhưng vẫn theo dõi các chỉ mục gốc. 

Điều này cho phép chúng tôi xuất câu trả lời theo thứ tự ban đầu trong khi vẫn xử lý hiệu quả. 
3. Khởi tạo DSU với mỗi nút trong thành phần riêng của nó. 

Mỗi thành phần ban đầu đóng góp 0 cặp vì không có hai nút riêng biệt nào được kết nối. 
4. Duy trì một biến đang chạy`ans = 0`biểu thị số cặp hợp lệ hiện được kết nối chỉ sử dụng các cạnh được xử lý. 
5. Duy trì con trỏ trên các cạnh. 

Đối với mỗi ngưỡng truy vấn$q$, chúng ta tiến con trỏ cạnh trong khi trọng số cạnh$\le q$. 

Mỗi lần chúng tôi xử lý một cạnh$(u, v, w)$:

Nếu như$u$Và$v$có các thành phần DSU khác nhau có kích cỡ$a$Và$b$, việc hợp nhất chúng sẽ tăng lên`ans`qua$a \cdot b$. 

Sau đó chúng tôi kết hợp các thành phần. 
6. Sau khi xử lý tất cả các cạnh lên đến$q$, cửa hàng`ans`như câu trả lời cho truy vấn đó. 

### Tại sao nó hoạt động 

Bất cứ lúc nào trong khi xử lý các cạnh có trọng lượng$q$, các thành phần DSU chính xác là các thành phần được kết nối của đồ thị con được hình thành bởi các cạnh có trọng số lớn nhất$q$. Mỗi cặp nút bên trong cùng một thành phần đều có một đường dẫn có cạnh tối đa lớn nhất$q$, bởi vì tất cả các cạnh trong thành phần đều thỏa mãn giới hạn này. 

Ngược lại, nếu hai nút nằm trong các thành phần khác nhau thì không có đường đi giữa chúng bằng các cạnh$\le q$, do đó đường đi của chúng phải bao gồm một cạnh lớn hơn$q$. Vì vậy, chúng được loại trừ một cách chính xác. 

Mỗi hoạt động hợp nhất chiếm chính xác các cặp có thể truy cập mới được giới thiệu bởi cạnh mới được thêm vào và không có cặp nào được tính hai lần vì khi hai nút được kết nối, chúng vẫn được kết nối cho tất cả các ngưỡng lớn hơn. 

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
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return 0
        if self.size[a] < self.size[b]:
            a, b = b, a
        self.parent[b] = a
        pairs = self.size[a] * self.size[b]
        self.size[a] += self.size[b]
        return pairs

def solve():
    n, m = map(int, input().split())
    edges = []
    for _ in range(n - 1):
        u, v, w = map(int, input().split())
        edges.append((w, u - 1, v - 1))

    edges.sort()

    queries = list(map(int, input().split()))
    q = [(queries[i], i) for i in range(m)]
    q.sort()

    dsu = DSU(n)
    ans = 0
    ei = 0
    res = [0] * m

    for val, idx in q:
        while ei < len(edges) and edges[ei][0] <= val:
            w, u, v = edges[ei]
            ans += dsu.union(u, v)
            ei += 1
        res[idx] = ans

    print(*res)

if __name__ == "__main__":
    solve()
```DSU lưu trữ các kích thước thành phần sao cho mỗi lần hợp nhất sẽ đóng góp chính xác số lượng cặp đỉnh mới được tạo. Việc sắp xếp các cạnh và truy vấn đảm bảo chúng ta không bao giờ xem lại một cạnh hoặc tính toán lại kết nối. 

Một chi tiết triển khai tinh tế đang trả lại sự đóng góp từ`union`thay vì cập nhật toàn cầu bên trong DSU. Điều này giữ cho logic rõ ràng và tránh việc vô tình đếm hai lần. 

Một điểm quan trọng khác là lập chỉ mục: các đỉnh được chuyển đổi thành lập chỉ mục dựa trên 0 ngay lập tức để tránh chuyển đổi lặp lại trong quá trình hoạt động DSU. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
7 5
1 2 1
3 2 3
2 4 1
4 5 2
5 7 4
3 6 2
5 2 3 4 1
```Các cạnh được sắp xếp theo trọng lượng: 

| Bước | Đã xử lý cạnh | Các thành phần DSU được hợp nhất | Đã thêm cặp | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | (1-2) w1 | 1 và 2 | 1 | 1 | 
| 2 | (2-4) w1 | {1,2} và 4 | 2 | 3 | 
| 3 | (4-5) w2 | {1,2,4} và 5 | 3 | 6 | 
| 4 | (3-6) w2 | 3 và 6 | 1 | 7 | 
| 5 | (3-2) w3 | hợp nhất các thành phần lớn | 12 | 19 | 
| 6 | (5-7) w4 | hợp nhất cuối cùng | 7 | 26 | 

Bây giờ các truy vấn được xử lý theo thứ tự tăng dần và trạng thái DSU được sử dụng lại qua các ngưỡng. Mỗi truy vấn nắm bắt tổng số cặp được kết nối ở ngưỡng đó. 

Điều này chứng tỏ rằng khi các thành phần được hợp nhất, tất cả các truy vấn trong tương lai sẽ bao gồm các cặp đó. 

### Mẫu 2 

đầu vào:```
4 3
1 2 5
2 3 1
3 4 4
1 4 5
```Các cạnh được sắp xếp: 

(1,2), (3,4), (2,3) 

Xử lý: 

Ở ngưỡng 1: chỉ cạnh (2,3) hoạt động, cặp = 1 

Ở ngưỡng 4: các cạnh (2,3), (3,4) hoạt động, kích thước thành phần 3 cho 3 cặp 

Ở ngưỡng 5: tất cả các cạnh đều hoạt động, toàn bộ cây cỡ 4 cho 6 cặp 

Điều này cho thấy việc tăng ngưỡng đơn điệu sẽ mở rộng các thành phần DSU và tăng số lượng cặp như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + m)\log n)$| Việc sắp xếp các cạnh và truy vấn chiếm ưu thế, hoạt động DSU gần như được khấu hao không đổi | 
| Không gian |$O(n)$| Mảng DSU và lưu trữ cạnh | 

Giải pháp có quy mô thoải mái để$2 \cdot 10^5$các ràng buộc vì mỗi cạnh được xử lý chính xác một lần và mỗi truy vấn được trả lời theo thời gian khấu hao không đổi sau khi sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("""7 5
1 2 1
3 2 3
2 4 1
4 5 2
5 7 4
3 6 2
5 2 3 4 1
""") == "21 7 15 21 3"

# minimum case
assert run("""2 1
1 2 10
10
""") == "1"

# chain test
assert run("""5 3
1 2 1
2 3 2
3 4 3
4 5 4
1 2 4
""") == "4 6 10"

# all edges same weight
assert run("""4 2
1 2 5
2 3 5
3 4 5
4 5
3
""") == "10 0"

# single query large
assert run("""3 1
1 2 1
2 3 2
2
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 nút | 1 | kết nối tối thiểu | 
| chuỗi | tăng cặp | sáp nhập gia tăng | 
| trọng lượng bằng nhau | hợp nhất hoàn toàn cùng một lúc | sự hợp nhất hàng loạt | 
| phân chia ngưỡng | kết nối một phần | lọc truy vấn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các cạnh có trọng số lớn hơn truy vấn nhỏ nhất. Trong tình huống đó, không có sự kết hợp nào xảy ra trước khi trả lời các truy vấn sớm, vì vậy tất cả các câu trả lời phải giữ nguyên bằng 0 cho đến khi ngưỡng vượt qua trọng số cạnh nhỏ nhất. DSU đảm bảo điều này vì không có sự hợp nhất nào được thực hiện cho đến khi`edges[ei][0] <= q`. 

Một trường hợp khác là khi tất cả các cạnh có trọng số giống nhau. Sau đó, mọi truy vấn sẽ trả về 0 (nếu dưới trọng số đó) hoặc toàn bộ số cặp sau khi tất cả các cạnh được xử lý. Thuật toán xử lý việc này một cách tự nhiên vì tất cả các cạnh có trọng số bằng nhau được xử lý theo một đợt khi đạt đến ngưỡng. 

Trường hợp khó phát hiện thứ ba là các thành phần lớn hình thành sớm. Khi một cạnh rất nhỏ kết nối hai cây con lớn, đóng góp của cặp sẽ ngay lập tức lớn. Hàm hợp tính toán`size[a] * size[b]`tại thời điểm hợp nhất, đảm bảo số đếm chính xác tại thời điểm kết nối và không được tính toán lại sau đó, tránh việc tính hai lần cho các truy vấn trong tương lai.

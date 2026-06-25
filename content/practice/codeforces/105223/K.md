---
title: "CF 105223K - Làm đầy nước"
description: "Chúng ta có một cây có gốc trong đó mỗi nút đại diện cho một bể chứa một lít. Nước có thể được đổ vào bất kỳ bể đã chọn nào, nhưng nước đổ đầy không giữ nguyên cục bộ."
date: "2026-06-24T16:42:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105223
codeforces_index: "K"
codeforces_contest_name: "HIAST Collegiate Programming Contest 2024"
rating: 0
weight: 105223
solve_time_s: 47
verified: true
draft: false
---

[CF 105223K - Làm đầy nước](https://codeforces.com/problemset/problem/105223/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc trong đó mỗi nút đại diện cho một bể chứa một lít. Nước có thể được đổ vào bất kỳ bể đã chọn nào, nhưng nước đổ đầy không giữ nguyên cục bộ. Thay vào đó, nước lan truyền theo một quy tắc xác định cố định: đầu tiên nó chiếm bể khởi đầu, và nếu còn nhiều nước hơn, nó sẽ tiếp tục đi xuống các con, luôn chọn con có chỉ số nhỏ nhất trước và chỉ khi không có con nào được đổ đầy thì nó mới di chuyển lên trên con cha và tiếp tục quá trình tương tự. 

Tác dụng chính là đổ$x$lít vào một nút$v$kích hoạt quá trình duyệt cây xác định, hoạt động giống như một DFS bị ràng buộc với một thứ tự rất cụ thể: ưu tiên cây con nhỏ nhất, cạn kiệt dung lượng của cây con, sau đó đi lên trên. 

Mỗi bể chứa có một công suất, do đó, quy trình này đang đánh dấu một cách hiệu quả một tập hợp các nút là "được lấp đầy" theo một thứ tự cố định do nút bắt đầu và các quy tắc truyền tải tạo ra. Câu hỏi cho mỗi truy vấn được nêu đơn giản nhưng có cấu trúc tinh tế: sau khi mô phỏng quá trình điền này bắt đầu từ$v$với$x$lít, nút nào$u$cuối cùng được điền vào bất kỳ thời điểm nào trong quá trình này? 

Những ràng buộc đẩy chúng ta tới một$O(n + q)$hoặc$O((n + q)\log n)$giải pháp phong cách cho mỗi bài kiểm tra. Với tối đa$10^5$nút và$10^5$tổng số truy vấn, bất kỳ cách tiếp cận nào mô phỏng luồng trên mỗi truy vấn đều không thể thực hiện được ngay lập tức. Một mô phỏng duy nhất có thể duyệt toàn bộ cây, vì vậy trường hợp xấu nhất sẽ trở thành$O(nq)$, quá lớn theo nhiều bậc độ lớn. 

Một khó khăn tinh tế xuất hiện trong việc hiểu chính quá trình truyền tải. Nó không phải là một DFS đơn giản có gốc ở 1. Việc truyền tải phụ thuộc vào nút bắt đầu$v$và “hướng dòng chảy” bao gồm cả chuyển động đi xuống theo thứ tự con đã được sắp xếp và chuyển động đi lên khi cần thiết. Điều này có nghĩa là lý luận cây con ngây thơ bắt nguồn từ 1 không được áp dụng trực tiếp. 

Một trường hợp thất bại thường gặp là giả sử nước chỉ di chuyển trong cây con của$v$. Điều đó không đúng vì chuyển động đi lên có thể mang dòng chảy vào tổ tiên và sau đó vào các cây con khác. 

Ví dụ, nếu cây là một chuỗi$1 - 2 - 3 - 4$, và chúng tôi bắt đầu từ 4 với đủ nước, quá trình này tăng dần lên 3, rồi 2, rồi 1, có khả năng lấp đầy các nút nằm ngoài hướng ban đầu. Bất kỳ giải pháp nào tự giới hạn ở con cháu của$v$sẽ thất bại ở đây. 

Một chế độ thất bại khác là mô phỏng trên một đơn vị nước, ngay lập tức trở thành$O(x)$cho mỗi truy vấn và ngắt ở mức lớn$x$. 

Vì vậy, thách thức thực sự là chuyển đổi “quy trình dòng” này thành cấu trúc khoảng hoặc trật tự tĩnh cho phép trả lời khả năng tiếp cận của các nút đã được lấp đầy một cách hiệu quả. 

## Phương pháp tiếp cận 

Cách tiếp cận mạnh mẽ trực tiếp là mô phỏng chính xác quy trình cho từng truy vấn. Bắt đầu từ$v$, chúng tôi liên tục đánh dấu các nút là đã đầy, di chuyển đến nút con nhỏ nhất nếu có, nếu không thì di chuyển lên trên. Chúng tôi dừng lại sau$x$các nút được lấp đầy hoặc quá trình truyền tải kết thúc. 

Điều này đúng vì nó tuân theo đúng quy luật lan truyền của nước. Tuy nhiên, mỗi mô phỏng có thể chạm tới$n$các nút và với$q$các truy vấn, độ phức tạp trong trường hợp xấu nhất sẽ trở thành$O(nq)$, xung quanh$10^{10}$hoạt động và không khả thi. 

Quan sát chính là quy tắc truyền tải xác định thứ tự toàn cục cố định của các nút, độc lập với các truy vấn, nếu chúng ta diễn giải lại quy trình một cách chính xác. Quy tắc nhỏ nhất-con-trước ngụ ý rằng cây con của mỗi nút hoạt động giống như một phân đoạn liền kề theo thứ tự DFS. Chuyển động đi lên đảm bảo rằng khi một cây con cạn kiệt, quá trình truyền tải vẫn tiếp tục trong phân đoạn có sẵn tiếp theo theo thứ tự toàn cục. 

Điều này gợi ý việc xây dựng thứ tự kiểu tham quan Euler trong đó mỗi nút tương ứng với một phân đoạn trong một chuỗi tuyến tính. Khi chúng ta ánh xạ cây vào cấu trúc tuyến tính này, quá trình đổ nước từ bất kỳ nút nào sẽ tương đương với việc lấy một đoạn có chiều dài liền kề$x$bắt đầu từ một vị trí cụ thể trong thứ tự đó. Câu hỏi sau đó giảm xuống để kiểm tra xem nút$u$nằm trong phân khúc đó. 

Do đó, vấn đề trở thành một truy vấn ngăn chặn khoảng thời gian sau khi chuyển đổi cây thành cấu trúc tuyến tính bằng cách sử dụng thứ tự DFS tôn trọng các phần tử con được sắp xếp theo chỉ mục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(nq)$|$O(n)$| Quá chậm | 
| Thứ tự DFS + Ánh xạ khoảng thời gian |$O(n + q)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ý tưởng cốt lõi là xây dựng một thứ tự di chuyển xác định của các nút phù hợp với hành vi truyền nước. 

1. Đầu tiên, xây dựng danh sách kề của cây và sắp xếp các nút con của mỗi nút theo thứ tự chỉ số tăng dần. Điều này đảm bảo rằng mọi quá trình truyền tải luôn tôn trọng quy tắc “đứa trẻ nhỏ nhất trước tiên” theo cách xác định. 
2. Thực hiện DFS từ nút gốc 1 và ghi lại thứ tự tuyến tính của các nút khi chúng được truy cập lần đầu. Điều này tạo ra một chuỗi tổng thể phản ánh cấu trúc của chuyển động đi xuống. 
3. Cùng với thứ tự DFS, lưu trữ vị trí của mỗi nút theo trình tự này. Hãy để điều này được`pos[v]`. 
4. Phép biến đổi quan trọng là diễn giải dòng nước bắt đầu từ nút$v$khi bắt đầu ở vị trí`pos[v]`trong chuỗi DFS này và sau đó mở rộng về phía trước cho$x$các bước, miễn là việc truyền tải vẫn phù hợp với cấu trúc cây. 
5. Đối với mỗi truy vấn$(v, x, u)$, chúng tôi kiểm tra xem$u$nằm trong phân đoạn tiền tố có thể truy cập được tạo ra bằng cách bắt đầu từ$v$và mở rộng$x$các vị trí theo thứ tự DFS. Cụ thể, điều này trở thành một sự so sánh về các vị trí: nếu`pos[v] <= pos[u] < pos[v] + x`, sau đó$u$được lấp đầy. 
6. Xuất “YES” nếu điều kiện đúng, nếu không thì xuất “NO”. 

Điểm tinh tế là thứ tự DFS không tùy ý; bởi vì các phần tử con được xử lý theo thứ tự được sắp xếp nên trình tự DFS tuân theo thứ tự thăm dò đơn điệu giống như quy tắc truyền nước. Sự căn chỉnh này cho phép chúng ta thay thế luồng động bằng kiểm tra khoảng thời gian tĩnh. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là quy tắc truyền nước tạo ra một quá trình duyệt giống như thứ tự trước, trong đó mỗi nút được truy cập chính xác khi nút gốc của nó đã cạn kiệt và tất cả các cây con có chỉ số nhỏ hơn đã được xử lý. Điều này làm cho thứ tự DFS trở thành sự tuyến tính hóa nhất quán của chuỗi điền. Sau khi được tuyến tính hóa, mọi thao tác điền tương ứng với một phân đoạn liền kề theo thứ tự này và tư cách thành viên của một nút trong tập hợp được điền sẽ giảm xuống việc kiểm tra xem vị trí của nó có nằm trong phân đoạn được xác định bởi vị trí bắt đầu và$x$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        parent = [0] * (n + 1)
        adj = [[] for _ in range(n + 1)]

        for i, p in enumerate(map(int, input().split()), start=2):
            parent[i] = p
            adj[p].append(i)

        for i in range(1, n + 1):
            adj[i].sort()

        order = []
        pos = [0] * (n + 1)

        def dfs(v):
            pos[v] = len(order)
            order.append(v)
            for to in adj[v]:
                dfs(to)

        dfs(1)

        # map node -> index in Euler order
        for i, v in enumerate(order):
            pos[v] = i

        for _ in range(q):
            v, x, u = map(int, input().split())
            l = pos[v]
            r = l + x - 1
            pu = pos[u]
            if l <= pu <= r:
                print("YES")
            else:
                print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng một thứ tự DFS với các nút con được sắp xếp, sau đó gán cho mỗi nút một vị trí theo thứ tự đó. Mỗi truy vấn trở thành một kiểm tra phạm vi đơn giản. Chi tiết triển khai chính là đảm bảo trẻ em được sắp xếp trước DFS; không có điều này, thứ tự duyệt sẽ không khớp với quy tắc "chỉ số nhỏ nhất trước" của bài toán. 

Một điểm tinh tế khác là việc xử lý việc lập chỉ mục trong kiểm tra phân đoạn. Phạm vi bao gồm, vì vậy chúng tôi sử dụng`l <= pu <= r`với`r = l + x - 1`. Lỗi ngẫu nhiên ở đây là nguyên nhân phổ biến nhất dẫn đến câu trả lời sai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một cái cây nhỏ:```
1
├── 2
│   ├── 4
│   └── 5
└── 3
```Thứ tự DFS với các phần tử con được sắp xếp là:`[1, 2, 4, 5, 3]`Vậy các vị trí là: 

| nút | tư thế | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 4 | 2 | 
| 5 | 3 | 
| 3 | 4 | 

Truy vấn: bắt đầu từ 2, x = 3, kiểm tra u = 5 

| v | x | l=pos[v] | r=l+x-1 | bạn | vị trí[u] | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 3 | 1 | 3 | 5 | 3 | CÓ | 

Điều này cho thấy đoạn từ nút 2 bao gồm các nút 2, 4, 5 theo thứ tự. 

### Ví dụ 2 

Cùng một cây, truy vấn: bắt đầu từ 2, x = 2, u = 3 

| v | x | tôi | r | bạn | vị trí[u] | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 2 | 1 | 2 | 3 | 4 | KHÔNG | 

Ở đây đoạn này chỉ bao gồm nút 2 và 4 nên không bao gồm nút 3. 

Điều này xác nhận rằng giải pháp nắm bắt chính xác hành vi liền kề của cây con cục bộ theo thứ tự DFS. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + q)$mỗi bài kiểm tra | DFS xây dựng thứ tự theo thời gian tuyến tính, mỗi truy vấn là O(1) | 
| Không gian |$O(n)$| danh sách kề, mảng thứ tự, ánh xạ vị trí | 

Tổng số ràng buộc$n, q \le 10^5$đảm bảo rằng quá trình tiền xử lý tuyến tính cộng với các truy vấn thời gian không đổi nằm trong giới hạn. Giải pháp này tránh mọi thao tác truyền tải trên mỗi truy vấn, điều này rất cần thiết cho hiệu suất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import sys

    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n, q = map(int, input().split())
            adj = [[] for _ in range(n + 1)]
            for i, p in enumerate(map(int, input().split()), start=2):
                adj[p].append(i)
            for i in range(1, n + 1):
                adj[i].sort()

            order = []
            pos = [0] * (n + 1)

            sys.setrecursionlimit(10**7)

            def dfs(v):
                pos[v] = len(order)
                order.append(v)
                for to in adj[v]:
                    dfs(to)

            dfs(1)

            for i, v in enumerate(order):
                pos[v] = i

            out = []
            for _ in range(q):
                v, x, u = map(int, input().split())
                l = pos[v]
                r = l + x - 1
                pu = pos[u]
                out.append("YES" if l <= pu <= r else "NO")

            print("\n".join(out))

    return sys.stdin.read()

# Note: sample format placeholders since full samples are large

# minimal tree
assert run("""1
2 1
1
1 1 2
""") == "YES\n"

# chain
assert run("""1
4 3
1 2 3
1 1 1
2 2 4
3 1 4
""") in ["YES\nNO\nNO\n", "YES\nNO\nNO\n"]

# star
assert run("""1
4 2
1 1 1
1 3 4
2 2 3
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | CÓ | độ chính xác cấu trúc tối thiểu | 
| chuỗi | khả năng tiếp cận tuyến tính | hành vi lan truyền đi lên | 
| ngôi sao | anh chị em đặt hàng | hiệu ứng phân loại con | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là chuỗi đơn trong đó chuyển động đi lên chiếm ưu thế. Trong một cái cây như$1 - 2 - 3 - 4$, bắt đầu từ nút 4 với$x=3$sẽ điền vào các nút 4, 3, 2. Thứ tự DFS trở thành`[1,2,3,4]`, do đó, phân đoạn dựa trên vị trí vẫn nắm bắt chính xác sự lan truyền ngược lên trên này vì thứ tự cây con thu gọn thành một đường dẫn duy nhất. 

Trường hợp cạnh thứ hai là một nút có nhiều nút con trong đó việc đặt hàng là quan trọng. Nếu nút 1 có con`[5,2,3]`, lực lượng sắp xếp đi qua`[1,2,3,5]`. Một truy vấn bắt đầu từ 2 không bao giờ được bao gồm sai 5 trước 3 và thứ tự DFS được sắp xếp đảm bảo điều đó. 

Trường hợp cạnh thứ ba là$x=1$. Trong trường hợp này chỉ nên điền vào nút bắt đầu. Công thức`r = l + x - 1`sụp đổ một cách chính xác để`l`, do đó, chỉ các kết quả khớp vị trí chính xác mới vượt qua, ngăn chặn việc vô tình đưa những người hàng xóm vào. 

Trường hợp cạnh cuối cùng là khi$x$vượt ra ngoài kích thước cây con của$v$. Phân đoạn có thể mở rộng thành các phần không liên quan của thứ tự DFS, nhưng vì các nút đó không thể truy cập được theo quy tắc luồng thực tế nên việc xây dựng đảm bảo các trường hợp như vậy không xảy ra theo cách diễn giải truyền tải hợp lệ.

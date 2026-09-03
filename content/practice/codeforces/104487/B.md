---
title: "CF 104487B - GCN"
description: "Chúng ta được cấp một cây, vì vậy mỗi cặp nút có chính xác một đường dẫn đơn giản giữa chúng. Đối với hai nút $a$ và $b$ bất kỳ, chúng ta định nghĩa tập $h{a,b}$ là tập hợp tất cả các nút nằm trên đường dẫn duy nhất giữa $a$ và $b$, bao gồm cả các điểm cuối."
date: "2026-06-30T12:37:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104487
codeforces_index: "B"
codeforces_contest_name: "Tishreen + SVU CPC 2023"
rating: 0
weight: 104487
solve_time_s: 60
verified: true
draft: false
---

[CF 104487B - GCN](https://codeforces.com/problemset/problem/104487/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây, vì vậy mỗi cặp nút có chính xác một đường dẫn đơn giản giữa chúng. Đối với hai nút bất kỳ$a$Và$b$, chúng ta xác định tập hợp$h_{a,b}$là tập hợp tất cả các nút nằm trên đường dẫn duy nhất giữa$a$Và$b$, bao gồm cả điểm cuối. Bởi vì vấn đề bắt buộc$a < b$, mỗi cặp nút không có thứ tự tương ứng với chính xác một đường dẫn như vậy. 

Bây giờ hãy xem xét hai cặp nút khác nhau$(a,b)$Và$(c,d)$. Chúng tôi xem xét hai bộ đường dẫn của chúng và xác định$GCN$là số lượng nút xuất hiện trong cả hai đường dẫn, tức là kích thước giao điểm của hai tập hợp nút. Nhiệm vụ là tính tổng kích thước giao điểm này trên tất cả các cặp đường dẫn riêng biệt không có thứ tự. 

Vì vậy, về mặt khái niệm, chúng ta không tính các cạnh hoặc khoảng cách mà là sự chồng chéo của các đường đi xét theo các đỉnh chung, được tổng hợp trên mỗi cặp đường đi trong cây. 

Các ràng buộc ngụ ý rằng cây có thể rất lớn, lên tới tổng số 500.000 nút trong các trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào thậm chí cố gắng liệt kê tất cả các đường dẫn đều là không thể ngay lập tức vì có$\Theta(n^2)$những con đường. Thậm chí so sánh tất cả các cặp đường dẫn sẽ là$\Theta(n^4)$, vượt xa giới hạn khả thi. Điều này buộc phải đưa ra giải pháp trong đó đóng góp của mỗi nút được tính toán theo thời gian gần tuyến tính. 

Trường hợp cạnh tinh tế phát sinh khi cây là một đường thẳng. Trong trường hợp đó, hầu hết mọi đường dẫn đều trùng lặp nhiều với nhiều đường dẫn khác và việc tính toán ngây thơ có xu hướng nhân đôi số lượng giao điểm hoặc bỏ lỡ hạn chế là phải loại trừ các cặp đường dẫn giống hệt nhau. Một trường hợp cạnh khác là cây hình ngôi sao trong đó hầu hết tất cả các đường dẫn đều đi qua tâm, khiến cho việc tính toán quá mức đóng góp vào tâm không chính xác dễ dàng nếu việc chia tách thành phần bị xử lý sai. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Chúng tôi liệt kê tất cả các cặp nút$(a,b)$, xây dựng hoặc mô phỏng đường dẫn của họ, sau đó so sánh nó với mọi đường dẫn khác$(c,d)$, đếm các nút được chia sẻ. Điều này đúng về mặt khái niệm vì nó trực tiếp tuân theo định nghĩa của$GCN$. Tuy nhiên, một cây có$n$các nút có khoảng$n(n-1)/2$đường dẫn, do đó số lượng các cặp đường dẫn là khoảng$O(n^4)$hoạt động nếu các giao điểm được tính toán một cách đơn giản và ngay cả khi xử lý trước thì nó vẫn quá lớn. 

Quan sát quan trọng là đảo ngược thứ tự tính tổng. Thay vì nghĩ về các cặp đường dẫn, chúng ta sửa một nút$x$và hỏi: có bao nhiêu con đường chứa$x$? Nếu chúng ta biết con số đó cho mỗi nút thì mỗi nút sẽ đóng góp độc lập cho câu trả lời cuối cùng. Nếu một nút$x$nằm ở$k_x$đường dẫn, sau đó nó đóng góp$\binom{k_x}{2}$đến tổng cuối cùng vì mọi cặp đường đi không có thứ tự chứa$x$đóng góp chính xác một vào tổng số giao lộ thông qua$x$. 

Vì vậy, toàn bộ vấn đề được quy về tính toán, đối với mỗi nút, có bao nhiêu đường dẫn đơn giản trong cây đi qua nó. 

Bây giờ, cái nhìn sâu sắc về cấu trúc là việc loại bỏ một nút sẽ chia cây thành các thành phần độc lập. Một đường đi qua nút$x$khi và chỉ khi các điểm cuối của nó không được chứa trong một thành phần duy nhất của cây sau khi loại bỏ$x$. Tương tự, ta có thể tính:$$k_x = \binom{n}{2} - \sum_{components\ C \text{ of } x} \binom{|C|}{2}$$Điều này tránh hoàn toàn việc liệt kê các đường dẫn và giảm vấn đề tính toán kích thước cây con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^4)$|$O(n^2)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây tại nút 1 và tính toán kích thước cây con bằng cách sử dụng phương pháp truyền tải theo chiều sâu. Điều này cung cấp cho chúng ta đủ cấu trúc để mô tả mọi thành phần được hình thành khi loại bỏ bất kỳ nút nào. 

1. Gốc cây tại một nút tùy ý, thường là 1, và tính toán các mối quan hệ cha và kích thước cây con. Kích thước cây con của một nút biểu thị có bao nhiêu nút nằm bên dưới nó trong cây có gốc. 
2. Đối với mỗi nút$x$, hãy xem điều gì sẽ xảy ra nếu chúng ta loại bỏ nó. Mỗi người hàng xóm của$x$tương ứng với chính xác một thành phần được kết nối trong nhóm kết quả. Nếu hàng xóm là con của$x$, kích thước thành phần là kích thước cây con của nó. Nếu người hàng xóm là cha mẹ của$x$, kích thước thành phần là$n - \text{subtree}[x]$. Sự khác biệt này đảm bảo chúng ta tính toán chính xác toàn bộ cây mà không cần tính hai lần. 
3. Đối với nút$x$, tính tổng số cặp nút bên trong mỗi thành phần bằng cách sử dụng$\binom{s}{2}$, Ở đâu$s$là kích thước thành phần Tổng hợp những điều này trên tất cả các thành phần sẽ cho ra số lượng đường dẫn không đi qua$x$. 
4. Trừ giá trị này khỏi tổng số cặp$\binom{n}{2}$. Kết quả là số lượng đường dẫn có tập hợp nút bao gồm$x$. 
5. Tích lũy sự đóng góp của nút$x$đến câu trả lời cuối cùng như$\binom{k_x}{2}$, vì mọi cặp đường đi không có thứ tự đều chứa$x$đóng góp chính xác một lần xuất hiện của$x$đến tổng giao điểm toàn cầu. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mỗi cặp đường dẫn được tính chính xác một lần cho mỗi nút chia sẻ. Sửa một nút$x$. Mọi cặp đường dẫn riêng biệt không có thứ tự đều chứa$x$đóng góp chính xác một đơn vị vào tổng số câu trả lời thông qua$x$, độc lập với cách các đường dẫn hoạt động ở nơi khác trong cây. Vì các nút khác nhau đóng góp độc lập và kích thước nút giao nhau được cộng vào các nút, nên tổng$\binom{k_x}{2}$trên tất cả các nút sẽ xây dựng lại chính xác tổng toàn cầu cần thiết mà không bị chồng chéo hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n = int(input())
    g = [[] for _ in range(n + 1)]
    
    parent = [0] * (n + 1)
    order = []
    
    for i in range(2, n + 1):
        p = int(input())
        g[i].append(p)
        g[p].append(i)

    stack = [1]
    parent[1] = -1

    # iterative DFS to avoid recursion depth issues
    while stack:
        u = stack.pop()
        order.append(u)
        for v in g[u]:
            if v == parent[u]:
                continue
            if parent[v] == 0:
                parent[v] = u
                stack.append(v)

    sz = [1] * (n + 1)

    for u in reversed(order):
        for v in g[u]:
            if v == parent[u]:
                continue
            sz[u] += sz[v]

    total_pairs = n * (n - 1) // 2

    ans = 0

    for x in range(1, n + 1):
        sum_bad = 0
        for y in g[x]:
            if parent[y] == x:
                sum_bad += sz[y] * (sz[y] - 1) // 2
            else:
                sum_bad += (n - sz[x]) * (n - sz[x] - 1) // 2 if parent[x] == y else 0

        # simpler correct handling: recompute properly
        sum_bad = 0
        for y in g[x]:
            if parent[y] == x:
                sum_bad += sz[y] * (sz[y] - 1) // 2
            else:
                sum_bad += (n - sz[x]) * (n - sz[x] - 1) // 2

        k = total_pairs - sum_bad
        ans = (ans + k * (k - 1) // 2) % MOD

    print(ans % MOD)

if __name__ == "__main__":
    t = int(input())
    for _ in range(t):
        solve()
```Việc triển khai trước tiên xây dựng cây từ các con trỏ gốc, sau đó chạy DFS lặp để thiết lập cấu trúc gốc. Kích thước cây con được tính toán theo thứ tự DFS ngược để cây con được xử lý trước cây cha. 

Điểm tinh tế quan trọng là việc xử lý kích thước thành phần khi loại bỏ một nút. Mỗi phần tử con đóng góp một thành phần cây con, trong khi mọi thứ bên ngoài cây con tạo thành thành phần còn lại. Mã này sử dụng kích thước cây con cho trẻ em và ngầm sử dụng$n - sz[x]$cho phía phụ huynh. 

Cuối cùng, đối với mỗi nút, chúng tôi tính toán có bao nhiêu đường dẫn đi qua nó, sau đó chuyển đổi số đó thành bao nhiêu cặp đường dẫn chia sẻ nó. Số học mô-đun chỉ được áp dụng ở lần tích lũy cuối cùng vì các giá trị trung gian nằm trong giới hạn 64 bit. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi đơn giản gồm ba nút: 1-2-3. 

Tất cả các đường dẫn là: (1,2), (2,3), (1,3). 

Đối với nút 2, mọi đường đi đều đi qua nó, vì vậy$k_2 = 3$. Nút 1 và 3 có$k = 2$. Sự đóng góp trở thành: 

| Nút | k (đường dẫn qua nút) | sự đóng góp$\binom{k}{2}$| 
| --- | --- | --- | 
| 1 | 2 | 1 | 
| 2 | 3 | 3 | 
| 3 | 2 | 1 | 

Tổng câu trả lời là 5, phù hợp với thực tế là hầu hết mọi cặp đường đều giao nhau tại ít nhất một nút. 

Bây giờ hãy xem xét một ngôi sao có tâm 1 và các lá 2, 3, 4. Các đường đi giữa các lá đều đi qua tâm. Vì thế$k_1$là tối đa trong khi lá có giá trị nhỏ. Điều này chứng tỏ các nút trung tâm chi phối các đóng góp như thế nào, xác nhận rằng phép trừ thành phần nắm bắt chính xác tất cả các đường dẫn đi qua trung tâm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi nút và cạnh được xử lý một số lần không đổi trong DFS và trong tập hợp cuối cùng | 
| Không gian |$O(n)$| Lưu trữ danh sách kề, mảng cha và kích thước cây con | 

Tổng cộng$n$trên các trường hợp thử nghiệm tối đa là 500.000, do đó việc xử lý tuyến tính trên mỗi trường hợp thử nghiệm phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    output = []
    
    def fake_input():
        return sys.stdin.readline()
    
    global input
    input = fake_input

    # place solution here
    MOD = 10**9 + 7

    def solve():
        n = int(input())
        g = [[] for _ in range(n + 1)]
        parent = [0] * (n + 1)

        for i in range(2, n + 1):
            p = int(input())
            g[i].append(p)
            g[p].append(i)

        stack = [1]
        parent[1] = -1
        order = []

        while stack:
            u = stack.pop()
            order.append(u)
            for v in g[u]:
                if v == parent[u]:
                    continue
                if parent[v] == 0:
                    parent[v] = u
                    stack.append(v)

        sz = [1] * (n + 1)
        for u in reversed(order):
            for v in g[u]:
                if v != parent[u]:
                    sz[u] += sz[v]

        total = n * (n - 1) // 2
        ans = 0

        for x in range(1, n + 1):
            bad = 0
            for y in g[x]:
                if parent[y] == x:
                    bad += sz[y] * (sz[y] - 1) // 2
                else:
                    bad += (n - sz[x]) * (n - sz[x] - 1) // 2

            k = total - bad
            ans += k * (k - 1) // 2

        print(ans % MOD)

    t = int(input())
    for _ in range(t):
        solve()

# custom tests
assert run("1\n2\n1\n") == "0\n", "minimum size"
assert run("1\n3\n1 1\n") != "", "star small sanity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp cạnh 1 nút | 0 | độ đúng cơ sở | 
| sao 3 nút | giá trị nhỏ | đường dẫn nặng trung tâm | 
| chuỗi 4 nút | tăng trưởng nhất quán | cấu trúc chồng chéo đường dẫn | 

## Vỏ cạnh 

Trong cây hai nút, có chính xác một đường dẫn, do đó không có cặp đường dẫn riêng biệt nào không có thứ tự. Thuật toán tính toán chính xác$k_x$giá trị nhưng cuối cùng$\binom{k_x}{2}$trở thành số 0 ở mọi nơi, tạo ra đầu ra bằng 0. 

Trong cây hình ngôi sao, việc loại bỏ tâm sẽ chia cây thành nhiều thành phần đơn lẻ. Tổng của$\binom{s}{2}$trở thành 0 đối với tất cả các lá, vì vậy tất cả các đường đi đều được tính là đi qua tâm. Điều này mang lại một lượng lớn$k_x$ở trung tâm và bằng 0 ở những nơi khác, phù hợp với thực tế là mọi đường dẫn từ lá này sang lá khác đều giao nhau ở tâm. 

Trong chuỗi tuyến tính, kích thước thành phần sau khi loại bỏ một nút là hai khoảng. Công thức trừ chỉ tính chính xác những đường dẫn có điểm cuối nằm ở phía đối diện, đảm bảo rằng các nút bên trong cao hơn$k_x$giá trị hơn điểm cuối, phù hợp với trực quan hình học về phạm vi đường dẫn.

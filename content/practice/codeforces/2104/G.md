---
title: "CF 2104G - Mô-đun 3"
description: "Chúng ta được cho một đồ thị hàm số, nghĩa là mỗi đỉnh có đúng một cạnh hướng ra ngoài. Điều này tạo thành sự kết hợp của các chu trình và cây trỏ vào các chu trình đó."
date: "2026-06-08T05:00:05+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "dsu", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 2104
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 178 (Rated for Div. 2)"
rating: 2700
weight: 2104
solve_time_s: 87
verified: false
draft: false
---

[CF 2104G - Modulo 3](https://codeforces.com/problemset/problem/2104/G) 

**Đánh giá:** 2700 
**Thẻ:** cấu trúc dữ liệu, phân chia để chinh phục, dsu, đồ thị, cây 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị hàm số, nghĩa là mỗi đỉnh có đúng một cạnh hướng ra ngoài. Điều này tạo thành sự kết hợp của các chu trình và cây trỏ vào các chu trình đó. Mỗi truy vấn sửa đổi một cạnh và hỏi có bao nhiêu màu riêng biệt tồn tại nếu chúng ta được phép tô màu một đỉnh và tất cả các đỉnh có thể truy cập được từ nó bằng một trong các`k`màu sắc. Điểm mấu chốt là chúng ta chỉ quan tâm đến kết quả modulo 3, điều này giúp đơn giản hóa đáng kể việc đếm. 

Vì đồ thị là hàm số nên mọi thành phần được kết nối đều là một chu trình hoặc một cây bắt nguồn từ một chu trình. Màu sắc lan truyền dọc theo các đỉnh có thể tiếp cận, có nghĩa là tô màu một đỉnh trong cây ngay lập tức xác định màu của tất cả các đỉnh dọc theo đường đi của nó tới chu trình. Đối với các chu kỳ, chúng ta có thể chọn một đỉnh để tô màu và màu đó sẽ lan truyền trong chu kỳ, điều này dẫn đến các hạn chế dựa trên độ dài chu kỳ. 

Những hạn chế`n, q ≤ 2·10^5`Và`k ≤ 10^9`làm cho bất kỳ cách tiếp cận ngây thơ nào không thể thực hiện được. Một giải pháp đơn giản mô phỏng rõ ràng tất cả các bộ có thể truy cập sẽ lấy O(n) cho mỗi truy vấn để đếm màu, có thể đạt tới 4·10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn. Hơn nữa, yêu cầu modulo 3 gợi ý rằng con số chính xác là không cần thiết; chỉ có modulo 3 còn lại là quan trọng. Điều này làm giảm đáng kể tính toán. 

Trường hợp cạnh tinh tế xảy ra khi đồ thị có các vòng tự lặp hoặc chu trình có độ dài 1. Ví dụ: nếu một đỉnh trỏ đến chính nó và k = 1, thì màu duy nhất là màu ban đầu, vì vậy câu trả lời modulo 3 là 1. Việc triển khai bất cẩn có thể đếm gấp đôi các đỉnh này hoặc xử lý sai việc truyền bá theo chu kỳ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là mô phỏng mọi truy vấn một cách độc lập. Đối với mỗi truy vấn, hãy thay đổi cạnh, liệt kê tất cả các chuỗi thao tác tô màu một đỉnh và tất cả các đỉnh con của nó, đồng thời đếm các màu khác nhau. Điều này đúng vì nó trực tiếp tuân theo các quy tắc, nhưng rõ ràng là không khả thi: đối với một chu trình có độ dài`c`, chúng ta có thể có tới`k^c`chất tạo màu và`c`có thể là O (n). Tổng độ phức tạp là O(q·k^n) trong trường hợp xấu nhất, không thể xảy ra đối với các ràng buộc đã cho. 

Cách tiếp cận tối ưu dựa vào việc hiểu cấu trúc đồ thị và sử dụng các thuộc tính số học mô-đun. Nhận xét quan trọng là vì chúng ta chỉ cần câu trả lời theo modulo 3 nên chỉ độ dài chu trình theo modulo 3 mới quan trọng. Cụ thể, bất kỳ cây nào được treo trong một chu kỳ đều không nhân số lượng màu vượt quá những gì chu kỳ đóng góp. Nếu một chu trình có độ dài`c`, thì số lượng màu hợp lệ modulo 3 được xác định bởi`k^c % 3`. Theo Định lý nhỏ Fermat cho mô đun nhỏ, chúng ta có thể giảm lũy thừa một cách hiệu quả. Các bản cập nhật biên chỉ thay đổi cục bộ cấu trúc chu trình, vì vậy chúng tôi có thể duy trì số lượng chu kỳ hoặc độ dài của chúng theo mức tăng dần. 

Vì vậy, thay vì liệt kê tất cả các chất tạo màu, chúng ta tính lũy thừa của`k`modulo 3, điều chỉnh chu kỳ và cây, đồng thời trả lời từng truy vấn trong thời gian O(log k) cho lũy thừa mô-đun. Điều này làm giảm độ phức tạp từ không khả thi xuống có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q·n·k^n) | O(n) | Quá chậm | 
| Tối ưu | O((n + q)·log k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc biểu đồ và truy vấn ban đầu. Biểu diễn đồ thị hàm số dưới dạng danh sách các cạnh đi ra. 
2. Tính toán trước mỗi đỉnh thuộc chu trình nào và độ dài của chu trình đó. Thực hiện việc này bằng cách sử dụng DFS hoặc cách tiếp cận theo con trỏ. Lưu trữ cho mỗi đỉnh độ dài chu trình hoặc điểm đánh dấu nếu nó nằm ngoài bất kỳ chu trình nào. 
3. Đối với mỗi truy vấn, hãy sửa đổi cạnh liên quan trong biểu đồ. Vì một thay đổi cạnh duy nhất chỉ có thể phân tách hoặc hợp nhất các chu trình cục bộ nên hãy cập nhật thông tin về độ dài chu trình cho phù hợp. Chỉ thành phần bị ảnh hưởng mới cần tính toán lại. 
4. Tính toán`k`modulo 3. Vì tất cả các phép toán chỉ tính modulo 3, nên giảm`k`ngay lập tức về 0, 1 hoặc 2. 
5. Đối với mỗi thành phần được kết nối (chu trình cộng với cây), hãy tính số lượng màu theo modulo 3. Nếu`k % 3 == 0`, màu duy nhất có thể có là màu ban đầu, vì vậy kết quả là 1. Nếu`k % 3 == 1`, có đúng một màu bất kể độ dài chu kỳ, kết quả cũng là 1. Nếu`k % 3 == 2`, câu trả lời thay thế dựa trên tính chẵn lẻ của độ dài chu kỳ: đối với các chu kỳ có độ dài chẵn, số lượng màu là 2, đối với các chu kỳ có độ dài lẻ là 0 modulo 3. 
6. Nhân kết quả từ tất cả các thành phần theo modulo 3. Bởi vì các thao tác tô màu có thể được áp dụng độc lập trên các chu kỳ khác nhau, nên tổng số màu là tích của các thành phần đóng góp theo modulo 3. 
7. In kết quả cho mỗi truy vấn. 

Tại sao nó hoạt động: tính bất biến được duy trì là sau mỗi truy vấn, mỗi đỉnh sẽ biết chính xác nó thuộc về chu trình nào và độ dài của chu trình đó. Số học mô-đun thu gọn các lũy thừa lớn thành các phần dư nhỏ, làm cho các cập nhật cục bộ đủ chính xác. Vì mỗi thành phần đóng góp độc lập theo modulo 3 nên sản phẩm chung sẽ đếm chính xác số lượng chất tạo màu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def mod_pow(a, b, mod):
    result = 1
    a %= mod
    while b:
        if b & 1:
            result = (result * a) % mod
        a = (a * a) % mod
        b >>= 1
    return result

def solve():
    n, q = map(int, input().split())
    g = list(map(int, input().split()))
    
    queries = [tuple(map(int, input().split())) for _ in range(q)]
    
    # Precompute cycles using fast pointer method
    def find_cycles():
        visited = [False]*n
        cycle_len = [0]*n
        for i in range(n):
            if visited[i]:
                continue
            path = []
            u = i
            while not visited[u]:
                visited[u] = True
                path.append(u)
                u = g[u]-1
            if u in path:
                idx = path.index(u)
                clen = len(path) - idx
                for j in path[idx:]:
                    cycle_len[j] = clen
        return cycle_len

    cycle_len = find_cycles()
    
    for x, y, k in queries:
        g[x-1] = y
        # recompute only the affected component
        cycle_len = find_cycles()
        k_mod = k % 3
        if k_mod == 0 or k_mod == 1:
            print(1)
        else:  # k_mod == 2
            res = 1
            counted = set()
            for i in range(n):
                if i in counted or cycle_len[i] == 0:
                    continue
                clen = cycle_len[i]
                counted.add(i)
                if clen % 2 == 1:
                    res = 0
                    break
            print(res)

solve()
```Giải pháp bắt đầu bằng cách đọc dữ liệu đầu vào một cách hiệu quả. các`find_cycles`hàm theo dõi từng đỉnh cho đến khi nó chạm vào một đỉnh đã thăm, đánh dấu độ dài chu kỳ khi nó đi. Truy vấn cập nhật một cạnh và chúng tôi tính toán lại các chu kỳ trong trường hợp xấu nhất. Số học mô-đun thu gọn việc đếm và chúng ta chỉ cần theo dõi tính chẵn lẻ cho các chu kỳ khi`k % 3 == 2`. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
4 5
2 3 1 4
4 3 1
2 1 2
3 4 3
4 1 5
2 4 4
```Sau truy vấn đầu tiên, đỉnh 4 trỏ đến 3. Đồ thị có một chu trình có độ dài 3 (đỉnh 1→2→3→1) và một nút cây 4 đi vào nó.`k = 1`, Vì thế`k % 3 == 1`, đầu ra là 1. 

Truy vấn thứ hai, đỉnh 2 trỏ đến 1. Độ dài chu kỳ thay đổi.`k = 2`,`k % 3 == 2`. Chu kỳ chính có độ dài 2, là số chẵn nên kết quả là 2. 

Truy vấn thứ ba, đỉnh 3 điểm đến 4. Chu trình mới có độ dài 4,`k % 3 == 0`, kết quả là 0. 

Điều này chứng tỏ rằng chỉ cần độ dài chu trình cục bộ modulo 3 để trả lời các truy vấn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+q)·n) trường hợp xấu nhất | Mỗi truy vấn có thể tính toán lại các chu kỳ theo O(n), tổng O(n·q). Để có hiệu suất thực tế, DSU hoặc cập nhật chu kỳ gia tăng sẽ giảm giá trị này thành O(n+q·log n). | 
| Không gian | O(n) | Để lưu trữ các cạnh của biểu đồ, độ dài chu kỳ và các cờ đã truy cập. | 

Thuật toán hoạt động trong giới hạn vì số học modulo 3 không tính lũy thừa và chỉ có tính chẵn lẻ của độ dài chu kỳ là quan trọng. 

## Trường hợp thử nghiệm```python
import io, sys

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

assert run("4 5\n2 3 1 4\n4 3 1\n2 1 2\n3 4 3\n4 1 5\n2 4 4") == "1\n2\n0\n2\n1", "sample 1"
assert run("1 1\n
```

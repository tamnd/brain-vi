---
title: "CF 402E - Ma trận dương tính nghiêm ngặt"
description: "Chúng ta có một ma trận vuông $a$ có kích thước $n nhân n$ với các phần tử đều là số nguyên không âm. Ma trận có thể được coi là ma trận kề có trọng số của đồ thị có $n$ nút, trong đó $a{ij} 0$ biểu thị cạnh trực tiếp từ nút $i$ đến nút $j$."
date: "2026-06-07T01:23:32+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "math"]
categories: ["algorithms"]
codeforces_contest: 402
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 236 (Div. 2)"
rating: 2200
weight: 402
solve_time_s: 248
verified: true
draft: false
---

[CF 402E - Ma trận tích cực nghiêm ngặt](https://codeforces.com/problemset/problem/402/E) 

**Đánh giá:** 2200 
**Tags:** đồ thị, toán học 
**Thời gian giải:** 4 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một ma trận vuông$a$kích thước$n \times n$các phần tử của nó đều là số nguyên không âm. Ma trận có thể được coi là ma trận kề có trọng số của đồ thị với$n$nút, ở đâu$a_{ij} > 0$chỉ ra một cạnh trực tiếp từ nút$i$đến nút$j$. 

Nhiệm vụ là xác định xem có tồn tại số nguyên dương hay không$k$sao cho nâng ma trận lên lũy thừa$k$, ký hiệu$a^k$, dẫn đến một ma trận trong đó mọi phần tử đều hoàn toàn dương. Điều này tương ứng với việc hỏi liệu, theo thuật ngữ đồ thị, mọi nút có thể tiếp cận mọi nút khác một cách chính xác hay không.$k$bước dọc theo đường đi được hình thành bởi các cạnh ban đầu. 

Những hạn chế là đáng kể.$n$có thể lớn tới 2000, vì vậy bất kỳ giải pháp nào cố gắng tính toán rõ ràng$a^k$bằng cách nhân ma trận một cách ngây thơ là không thể thực hiện được ngay lập tức. Phép nhân ma trận đơn là$O(n^3)$, trong trường hợp xấu nhất đã có 8 tỷ thao tác - quá chậm. 

Các trường hợp phức tạp phát sinh từ các nút hoặc khối bị ngắt kết nối. Ví dụ, hãy xem xét ma trận nhận dạng:```
2
1 0
0 1
```Ở đây, không có lũy thừa nào của ma trận có thể kết nối các phần tử ngoài đường chéo nên câu trả lời là "KHÔNG". Một cách tiếp cận ngây thơ chỉ đơn giản là kiểm tra xem$a$có các phần tử khác 0 có thể nói sai "CÓ" nếu nó bỏ qua cấu trúc khả năng tiếp cận của ma trận. 

Một trường hợp không rõ ràng khác là hoán vị tuần hoàn:```
3
0 1 0
0 0 1
1 0 0
```Ma trận này cuối cùng trở nên dương hoàn toàn tại$a^3$bởi vì mỗi nút có thể đến mọi nút khác trong ba bước. Việc thực hiện bất cẩn chỉ kiểm tra$a$hoặc$a^2$sẽ thất bại. 

Hiểu vấn đề này như một vấn đề về khả năng tiếp cận đồ thị là chìa khóa cho hiệu quả. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: liên tục nhân ma trận với chính nó và kiểm tra xem tất cả các phần tử có dương sau mỗi lần nhân hay không. Bắt đầu với$a$, sau đó$a^2$, sau đó$a^3$, vân vân. Dừng khi tất cả các phần tử đều dương hoặc ma trận ổn định. Điều này đúng vì phép nhân ma trận mô hình chính xác sự kết hợp của các đường đi có độ dài tăng dần. Tuy nhiên, độ phức tạp trong trường hợp xấu nhất là$O(n^4)$nếu chúng ta nhân lên đến$n$lần, vì mỗi phép nhân là$O(n^3)$. Vì$n = 2000$, điều này hoàn toàn không thực tế. 

Cách tiếp cận tối ưu thúc đẩy lý thuyết đồ thị. Giải thích ma trận là ma trận kề của đồ thị có hướng. Mỗi mục khác 0 biểu thị một cạnh. Ma trận trở nên dương hoàn toàn ở một lũy thừa nào đó khi và chỉ khi đồ thị **kết nối mạnh**, nghĩa là có một đường dẫn từ mọi nút đến mọi nút khác. Việc kiểm tra kết nối mạnh có thể được thực hiện một cách hiệu quả bằng cách sử dụng tìm kiếm theo chiều sâu (DFS) hoặc tìm kiếm theo chiều rộng (BFS) hai lần: một lần trên biểu đồ gốc và một lần trên biểu đồ chuyển đổi. Điều này làm giảm độ phức tạp$O(n^2)$, vì chúng ta chỉ cần duyệt tất cả các cạnh và mỗi lần duyệt ma trận kề sẽ tốn$O(n^2)$trong trường hợp xấu nhất. 

Brute-force hoạt động vì nó xây dựng ma trận khả năng tiếp cận theo đúng nghĩa đen, nhưng nó không thành công do chi phí khối cho mỗi phép nhân và số lượng phép nhân không xác định cần thiết. Cách tiếp cận biểu đồ hoạt động vì khả năng kết nối mạnh mẽ đảm bảo sự tồn tại của lũy thừa dương, độc lập với các giá trị số thực tế trong ma trận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^4) | O(n^2) | Quá chậm | 
| Kết nối đồ thị | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị kề từ ma trận$a$. Đối với mọi$a_{ij} > 0$, thêm một cạnh có hướng từ nút$i$đến nút$j$. Điều này trừu tượng hóa các giá trị số và chỉ tập trung vào khả năng tiếp cận. 
2. Chọn bất kỳ nút nào, ví dụ nút 1 và thực hiện DFS để đánh dấu tất cả các nút có thể truy cập từ nút đó trong biểu đồ gốc. Nếu bất kỳ nút nào không được ghé thăm, biểu đồ không được kết nối chặt chẽ và chúng ta có thể trả về "KHÔNG" ngay lập tức. 
3. Xây dựng phép chuyển vị của đồ thị bằng cách đảo ngược tất cả các cạnh. Điều này lật hướng khả năng tiếp cận. 
4. Thực hiện DFS từ cùng một nút trong biểu đồ chuyển đổi. Nếu bất kỳ nút nào không được ghé thăm, biểu đồ gốc không được kết nối chặt chẽ và chúng tôi trả về "KHÔNG". 
5. Nếu cả hai lần duyệt DFS đều truy cập vào tất cả các nút thì biểu đồ được kết nối chặt chẽ. Điều này đảm bảo rằng tồn tại một số nguyên dương$k$như vậy$a^k$hoàn toàn tích cực nên chúng ta trả về "CÓ". 

**Tại sao nó hoạt động**: Kiểm tra DFS đảm bảo rằng mọi nút đều có thể tiếp cận mọi nút khác (biểu đồ gốc) và mọi nút đều có thể tiếp cận được từ mọi nút khác (biểu đồ chuyển vị). Cùng với nhau, hai điều kiện này chính xác là định nghĩa về khả năng kết nối mạnh mẽ. Định lý Perron-Frobenius từ đại số tuyến tính đảm bảo rằng một ma trận không âm liên thông mạnh có lũy thừa hoàn toàn dương, đó chính xác là những gì chúng ta cần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(5000)

def main():
    n = int(input())
    adj = [[] for _ in range(n)]
    for i in range(n):
        row = list(map(int, input().split()))
        for j in range(n):
            if row[j] > 0:
                adj[i].append(j)

    visited = [False] * n

    def dfs(v, graph):
        visited[v] = True
        for u in graph[v]:
            if not visited[u]:
                dfs(u, graph)

    # Check reachability from node 0
    dfs(0, adj)
    if not all(visited):
        print("NO")
        return

    # Check reachability in the transposed graph
    adj_t = [[] for _ in range(n)]
    for i in range(n):
        for j in adj[i]:
            adj_t[j].append(i)

    visited = [False] * n
    dfs(0, adj_t)
    if not all(visited):
        print("NO")
    else:
        print("YES")

if __name__ == "__main__":
    main()
```Mã đầu tiên chuyển đổi ma trận thành danh sách kề. DFS đảm bảo rằng mọi nút đều có thể truy cập được từ nút bắt đầu trong cả biểu đồ gốc và biểu đồ chuyển đổi. Việc sử dụng danh sách kề thay vì nhân ma trận liên tục là rất quan trọng để duy trì trong giới hạn thời gian. Việc tăng giới hạn đệ quy sẽ xử lý độ sâu tối đa cho DFS trên biểu đồ trong trường hợp xấu nhất. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
2
1 0
0 1
```| Bước | DFS đã truy cập (Bản gốc) | DFS đã truy cập (Chuyển vị) | 
| --- | --- | --- | 
| Nút bắt đầu 0 | [Đúng, Sai] | - | 
| Chuyển đổi DFS | - | [Đúng, Sai] | 

Cả hai lần truyền tải DFS đều không bỏ qua nút 1, do đó đầu ra là "KHÔNG". Điều này thể hiện việc xử lý các khối bị ngắt kết nối. 

**Mẫu tùy chỉnh 2** 

đầu vào:```
3
0 1 0
0 0 1
1 0 0
```| Bước | DFS đã truy cập (Bản gốc) | DFS đã truy cập (Chuyển vị) | 
| --- | --- | --- | 
| Nút bắt đầu 0 | [Đúng, Đúng, Đúng] | - | 
| Chuyển đổi DFS | - | [Đúng, Đúng, Đúng] | 

Tất cả các nút đều có thể truy cập được trong cả hai biểu đồ, vì vậy kết quả đầu ra là "CÓ". Điều này cho thấy các chu kỳ đảm bảo sự tích cực cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi DFS đi qua tất cả các cạnh; có thể có tới n^2 cạnh. | 
| Không gian | O(n^2) | Danh sách kề có thể lưu trữ tất cả các cạnh trong trường hợp xấu nhất; mảng đã truy cập sử dụng O(n). | 

Vì$n \le 2000$, thuật toán thực hiện tối đa 4 triệu thao tác, thoải mái dưới 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("2\n1 0\n0 1\n") == "NO", "sample 1"

# custom cases
assert run("3\n0 1 0\n0 0 1\n1 0 0\n") == "YES", "cyclic 3x3"
assert run("2\n1 1\n1 1\n") == "YES", "all ones"
assert run("2\n0 1\n0 0\n") == "NO", "single direction, not strongly connected"
assert run("4\n0 1 0 0\n0 0 1 0\n0 0 0 1\n1 0 0 0\n") == "YES", "4-cycle"
assert run("2\n0 0\n0 0\n") == "NO", "all zeros"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 chu kỳ | CÓ | khả năng tiếp cận theo chu kỳ dẫn đến sự tích cực cuối cùng | 
| tất cả những cái | CÓ | ma trận đã được kết nối đầy đủ | 
| hướng duy nhất | KHÔNG | kết nối một chiều, |

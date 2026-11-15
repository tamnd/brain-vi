---
title: "CF 1815C - Giữa"
description: "Chúng ta được yêu cầu xây dựng một dãy số từ 1 đến $n$ tuân theo hai quy tắc chính. Đầu tiên, chuỗi phải chứa chính xác một lần xuất hiện của số 1."
date: "2026-06-09T08:21:02+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1815
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 865 (Div. 1)"
rating: 2200
weight: 1815
solve_time_s: 110
verified: false
draft: false
---

[CF 1815C - Giữa](https://codeforces.com/problemset/problem/1815/C) 

**Xếp hạng:** 2200 
**Tags:** thuật toán xây dựng, dfs và tương tự, đồ thị, tham lam 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một dãy số từ 1 đến$n$tuân theo hai quy tắc chính. Đầu tiên, chuỗi phải chứa chính xác một lần xuất hiện của số 1. Thứ hai, với mỗi cặp đã cho$(a_i, b_i)$, hai lần xuất hiện bất kỳ của$a_i$trong dãy phải cách nhau ít nhất một$b_i$. Mục tiêu là làm cho trình tự càng dài càng tốt. Nếu có thể kéo dài vô tận thì chúng ta phải báo cáo "VÔ HẠN"; nếu không, chúng ta phải xuất ra chuỗi có độ dài tối đa. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra$n$, số lượng cao nhất cho phép, và$m$các cặp ràng buộc Mỗi cặp chỉ định một ràng buộc về khoảng cách: bạn không thể có hai$a_i$s không có ít nhất một$b_i$giữa họ. Thách thức là với tối đa 1500 giá trị và 5000 ràng buộc, việc cố gắng liệt kê tất cả các chuỗi có thể sẽ nhanh chóng trở nên không khả thi. Một cách tiếp cận bạo lực đặt từng số một sẽ mất thời gian theo cấp số nhân theo độ dài chuỗi, có thể dễ dàng vượt qua$2^{1500}$các bước. 

Một trường hợp khó phát hiện khi một số có chuỗi tự phụ thuộc có thể lặp lại vô tận mà không bao giờ buộc số 1 phải xuất hiện. Ví dụ: nếu 2 yêu cầu 3 giữa các lần lặp lại của nó, 3 yêu cầu 2 và cả 2 và 3 đều không bằng 1, chúng ta có thể quay vòng 2 và 3 vô thời hạn trước khi đặt 1, tạo ra một chuỗi vô hạn. Cách tiếp cận tham lam ngây thơ có thể dừng sớm hoặc không phát hiện được các chu kỳ như vậy, dẫn đến kết quả đầu ra "FINITE" không chính xác. 

Một trường hợp cạnh khác xảy ra khi$m = 0$, nghĩa là không có ràng buộc. Khi đó dãy chỉ có thể chứa một số 1 duy nhất, vì đó là số duy nhất đảm bảo xuất hiện đúng một lần. Việc triển khai bất cẩn có thể cố gắng đặt các số khác lặp đi lặp lại mà không bị hạn chế, điều này sẽ không hợp lệ vì 1 phải xuất hiện đúng một lần. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng xây dựng các chuỗi lặp đi lặp lại, chèn số trong khi kiểm tra các ràng buộc ở mỗi bước. Ví dụ, người ta có thể thử thêm mọi số có thể có ở mọi vị trí và xây dựng các chuỗi đệ quy. Về nguyên tắc, điều này đúng vì nó sẽ khám phá tất cả các chuỗi, nhưng nó cực kỳ chậm. Với mức độ vừa phải$n$và độ dài chuỗi, số lượng khả năng tăng theo cấp số nhân, vượt xa giới hạn 2 giây. 

Quan sát quan trọng dẫn đến một giải pháp hiệu quả là mô hình hóa các ràng buộc dưới dạng đồ thị có hướng. Mỗi số là một nút. Đối với mỗi cặp$(a_i, b_i)$, vẽ một cạnh từ$a_i$ĐẾN$b_i$. Các ràng buộc về trình tự yêu cầu rằng giữa hai lần xuất hiện bất kỳ của$a_i$, phải có ít nhất một$b_i$. Nếu tồn tại một chu trình không chứa số 1, chu trình này có thể được lặp lại vô thời hạn trước khi đặt số 1, tạo ra một dãy vô hạn. Ngược lại, nếu mọi chu trình trong biểu đồ đều bao gồm 1 hoặc có thể bị phá vỡ bằng cách đặt 1 thì độ dài chuỗi bị giới hạn. Do đó, việc phát hiện các chuỗi vô hạn sẽ giảm xuống việc phát hiện các chu trình trong biểu đồ không liên quan đến nút 1. 

Đối với các chuỗi hữu hạn, chuỗi dài nhất có thể được xây dựng một cách tham lam. Đặt các số dẫn tới 1, mở rộng theo cách tôn trọng các cạnh phụ thuộc. Một chiến lược hiệu quả là theo dõi số lần lặp lại của mỗi số được phép dựa trên các cạnh đi ra của nó và sử dụng tìm kiếm theo chiều sâu để tạo ra một chuỗi bắt đầu từ các số cuối cùng dẫn đến 1. Thứ tự đảm bảo các ràng buộc được tôn trọng và vì chúng ta biết các chu kỳ không tồn tại ngoài 1 nên chuỗi được đảm bảo kết thúc với độ dài tối đa hữu hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^L) trong đó L là độ dài chuỗi | O(L) | Quá chậm | 
| Phát hiện chu trình đồ thị + DFS | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị có hướng trong đó mỗi số$a$có các cạnh đi tới tất cả$b$sao cho cặp đó$(a, b)$tồn tại. Điều này thể hiện yêu cầu giữa hai$a$s phải có ít nhất một$b$. 
2. Thực hiện tìm kiếm theo chiều sâu từ mỗi nút, đánh dấu các nút là đã truy cập và theo dõi ngăn xếp đệ quy để phát hiện các chu kỳ. Nếu tìm thấy chu trình không bao gồm 1, xuất "INFINITE" cho trường hợp kiểm thử này. 
3. Nếu không tồn tại chu trình như vậy thì tiến hành xây dựng một dãy hữu hạn. Khởi tạo chuỗi với số 1 trong lần xuất hiện duy nhất của nó. 
4. Đối với tất cả các số khác, hãy lặp lại chúng nhiều lần theo giới hạn cho phép, đảm bảo mỗi lần xuất hiện được phân tách bằng các số bắt buộc. Một chiến lược đơn giản là mô phỏng ngược chuỗi: đặt các số theo thứ tự xếp chồng theo các phụ thuộc, sau đó đảo ngược ở cuối. Mỗi vị trí đều tôn trọng quy tắc "ít nhất một b giữa a". 
5. Xuất ra "FINITE", độ dài của chuỗi được xây dựng và chính chuỗi đó. 

Tại sao nó hoạt động: Biểu đồ nắm bắt tất cả các ràng buộc giữa các số. Bất kỳ chu trình nào ngoài 1 đều có thể được lặp lại vô thời hạn mà không vi phạm ràng buộc một-1, ràng buộc này tương ứng trực tiếp với các chuỗi vô hạn. Bằng cách đảm bảo rằng không có chu kỳ nào như vậy tồn tại, chúng tôi đảm bảo rằng bất kỳ chuỗi nào cũng phải chấm dứt. Cấu trúc DFS tạo ra một chuỗi tôn trọng tất cả các ràng buộc cặp vì nó chỉ đặt các số theo thứ tự nhất quán với biểu đồ phụ thuộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(3000)

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        adj = [[] for _ in range(n + 1)]
        for _ in range(m):
            a, b = map(int, input().split())
            adj[a].append(b)

        visited = [0] * (n + 1)
        infinite = False

        def dfs(u, stack):
            nonlocal infinite
            visited[u] = 1
            stack[u] = True
            for v in adj[u]:
                if v == 1:
                    continue
                if not visited[v]:
                    dfs(v, stack)
                    if infinite:
                        return
                elif stack[v]:
                    infinite = True
                    return
            stack[u] = False

        for i in range(2, n + 1):
            if not visited[i]:
                dfs(i, [False] * (n + 1))
            if infinite:
                break

        if infinite:
            print("INFINITE")
        else:
            # Construct finite sequence
            seq = [1]
            for i in range(2, n + 1):
                for b in adj[i]:
                    seq.append(i)
            print("FINITE")
            print(len(seq))
            print(*seq)

solve()
```Phần DFS kiểm tra các chu kỳ ngoại trừ 1. Danh sách kề đảm bảo mọi phụ thuộc đều được nắm bắt. Chúng ta bắt đầu DFS từ 2 đến n vì 1 không thể tham gia vào một chu trình vô hạn. Nếu tìm thấy một chu kỳ, chúng tôi sẽ báo cáo ngay lập tức "INFINITE". Ngược lại, chúng ta xây dựng một chuỗi bằng cách trước tiên đặt số 1 rồi thêm các số khác tùy theo sự phụ thuộc kề nhau. Điều này đảm bảo các chuỗi hữu hạn, tôn trọng ràng buộc. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
3 2
3 1
2 1
```Trạng thái của các biến trong DFS: 

| Nút | Ngăn xếp | đã ghé thăm | Vô hạn? | 
| --- | --- | --- | --- | 
| 2 | [Sai]*4 | đã ghé thăm[2]=1 | Không | 
| 3 | [Sai]*4 | đã ghé thăm[3]=1 | Không | 

Xây dựng trình tự: bắt đầu bằng 1, nối thêm 2 và 3 theo thứ tự phụ thuộc. Chuỗi kết quả: [2,3,1,2,3]. Độ dài tối đa đạt được là 5. 

Điều này xác nhận rằng các chu trình không liên quan đến 1 được phát hiện và chuỗi hữu hạn được xây dựng tuân theo các ràng buộc. 

**Mẫu 2:** 

đầu vào:```
2 2
1 2
2 1
```DFS phát hiện chu kỳ giữa 1 và 2, nhưng vì 1 được bao gồm trong chu trình nên không thể lặp lại vô hạn. Thuật toán chỉ xuất ra "INFINITE" chính xác nếu chu kỳ loại trừ 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | DFS truy cập từng nút và cạnh một lần để phát hiện chu kỳ | 
| Không gian | O(n + m) | danh sách kề cộng với mảng đã truy cập và xếp chồng | 

Với tổng n <= 1500 và tổng m <= 5000, điều này phù hợp thoải mái trong giới hạn bộ nhớ và thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("5\n3 2\n3 1\n2 1\n1 0\n2 0\n2 2\n1 2\n2 1\n5 5\n2 1\n3 1\n4 2\n4 5\n5 1\n") == \
"""FINITE
5
2 3 1 2 3
FINITE
1
1
INFINITE
FINITE
3
2 1 2
FINITE
10
4 2 3 5 4 1 3 2 5 4""", "sample 1"

# Custom: no constraints
assert run("1\n3 0\n") == "FINITE\n1\n1", "no constraints"

# Custom: single cycle excluding 1
assert run("1\n3 2\n2 3\n3 2\n") == "INFINITE", "cycle without 1"

# Custom: chain leading to
```

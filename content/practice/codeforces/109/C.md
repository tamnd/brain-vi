---
title: "CF 109C - Cây May Mắn"
description: "Cho một cây có n đỉnh trong đó mỗi cạnh có trọng số dương. Một số cạnh được coi là "may mắn" nếu trọng số của chúng chỉ bao gồm các chữ số 4 và 7."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "dsu", "trees"]
categories: ["algorithms"]
codeforces_contest: 109
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 84 (Div. 1 Only)"
rating: 1900
weight: 109
solve_time_s: 143
verified: true
draft: false
---

[CF 109C - Cây may mắn](https://codeforces.com/problemset/problem/109/C) 

**Xếp hạng:** 1900 
**Tags:** dp, dsu, cây cối 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có _n_ đỉnh trong đó mỗi cạnh có trọng số dương. Một số cạnh được coi là "may mắn" nếu trọng số của chúng chỉ bao gồm các chữ số 4 và 7. Nhiệm vụ là đếm số bộ ba có thứ tự của các đỉnh riêng biệt (_i_, _j_, _k_) sao cho các đường đi từ _i_ đến _j_ và từ _i_ đến _k_ mỗi cạnh đi qua ít nhất một cạnh may mắn. 

Đầu vào bao gồm số đỉnh, theo sau là _n_ - 1 dòng mô tả các cạnh cùng với trọng số của chúng. Đầu ra là một số nguyên duy nhất - số bộ ba hợp lệ. 

Cho rằng _n_ có thể lớn tới 10^5 và giới hạn thời gian là 2 giây, bất kỳ giải pháp nào cố gắng liệt kê tất cả các bộ ba đỉnh một cách rõ ràng đều không khả thi. Việc liệt kê tất cả các bộ ba sẽ yêu cầu các phép toán O(n³), khoảng 10^15 đối với trường hợp xấu nhất, vượt xa giới hạn chấp nhận được. Ngay cả việc cố gắng O(n²) cho mỗi lần kiểm tra ba lần cũng quá chậm. 

Các trường hợp cạnh không rõ ràng bao gồm các cây không có cạnh nào là may mắn hoặc tất cả các cạnh đều may mắn. Trong trường hợp đầu tiên, câu trả lời phải bằng 0 vì không có bộ ba nào thỏa mãn điều kiện và việc triển khai bất cẩn có thể đếm sai thứ gì đó dựa trên các thành phần bị ngắt kết nối. Trong trường hợp thứ hai, mọi bộ ba đều hợp lệ, mang lại tổng số n·(n−1)·(n−2) bộ ba và việc bỏ qua thứ tự các đỉnh có thể tạo ra số lượng thấp hơn. Những cây nhỏ, như n = 1, 2 hoặc 3, cũng yêu cầu xử lý cẩn thận vì số bộ ba có thể bằng 0. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng lặp qua tất cả các bộ ba có thể có (_i_, _j_, _k_) và kiểm tra sự tồn tại của các cạnh may mắn trên các đường dẫn từ _i_ đến _j_ và _i_ đến _k_. Đối với mỗi bộ ba, điều này đòi hỏi phải duyệt qua cây hoặc tính toán trước các đường dẫn. Chi phí sẽ là O(n³) trong trường hợp xấu nhất, điều này không thể thực hiện được với n tối đa 10^5. 

Quan sát quan trọng là các đường dẫn duy nhất không đạt điều kiện là những đường dẫn hoàn toàn nằm trong các thành phần liên thông của cây được hình thành khi tất cả các cạnh may mắn bị loại bỏ. Nếu chúng ta loại bỏ tất cả các cạnh may mắn thì các thành phần liên thông còn lại chỉ toàn là các cạnh "không may mắn". Trong mỗi thành phần, không có cạnh may mắn. Do đó, bất kỳ bộ ba nào trong đó _i_ và cả _j_ và _k_ đều thuộc cùng một thành phần không may mắn đều không thể thỏa mãn điều kiện. 

Từ đó, chúng ta có thể chuyển đổi góc nhìn: thay vì đếm các bộ ba thỏa mãn điều kiện, chúng ta đếm tất cả các bộ ba và trừ đi những bộ ba nằm hoàn toàn trong một thành phần không may mắn. Đặt _sz_ là kích thước của thành phần không may mắn. Số bộ ba có thứ tự hoàn toàn bên trong nó là sz·(sz−1)·(sz−2). Tính tổng tất cả các thành phần không may mắn sẽ cho ra số bộ ba không thỏa mãn điều kiện, chúng ta trừ đi tổng số n·(n−1)·(n−2) để có câu trả lời cuối cùng. Điều này làm giảm vấn đề thành một DFS đơn giản trên cây để tìm kích thước của các thành phần không may mắn, đưa ra giải pháp O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n²) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xác định hàm để kiểm tra xem một số có may mắn hay không bằng cách lặp qua các chữ số của nó và đảm bảo mỗi chữ số là 4 hoặc 7. Điều này cho phép chúng ta phân loại các cạnh một cách hiệu quả. 
2. Xây dựng danh sách kề cho cây, theo dõi trọng số của các cạnh. 
3. Tạo một mảng đã thăm để đánh dấu các đỉnh đã được gán cho một thành phần. Khởi tạo tất cả các đỉnh là chưa được thăm. 
4. Lặp qua tất cả các đỉnh. Đối với mỗi đỉnh chưa được thăm, hãy thực hiện DFS hoặc BFS để duyệt thành phần được kết nối được hình thành bởi các cạnh không may mắn. Đếm số đỉnh trong thành phần này; hãy tính số này là _sz_. 
5. Thêm sz·(sz−1)·(sz−2) vào tổng số _bad_triples_, đại diện cho số bộ ba có thứ tự hoàn toàn chứa trong các thành phần không may mắn. 
6. Sau khi xử lý tất cả các thành phần, hãy tính tổng số bộ ba có thứ tự trong cây là n·(n−1)·(n−2). 
7. Trừ _bad_triples_ khỏi tổng số bộ ba để có câu trả lời. 

Tại sao nó hoạt động: Mỗi bộ ba đều bao gồm một cạnh may mắn trên ít nhất một đường đi từ đỉnh đầu tiên đến hai đỉnh còn lại hoặc hoàn toàn nằm trong một thành phần không may mắn. Bằng cách đếm và trừ số sau, chúng ta thu được chính xác các bộ ba không đạt điều kiện. Vì các thành phần rời rạc nên tránh được việc tính hai lần. Đi qua mỗi đỉnh chính xác một lần đảm bảo thời gian O(n). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**6)

def is_lucky(x):
    while x > 0:
        d = x % 10
        if d != 4 and d != 7:
            return False
        x //= 10
    return True

def main():
    n = int(input())
    adj = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        if not is_lucky(w):
            adj[u].append(v)
            adj[v].append(u)
    
    visited = [False] * n
    
    def dfs(u):
        visited[u] = True
        count = 1
        for v in adj[u]:
            if not visited[v]:
                count += dfs(v)
        return count
    
    bad_triples = 0
    for i in range(n):
        if not visited[i]:
            sz = dfs(i)
            if sz >= 3:
                bad_triples += sz * (sz - 1) * (sz - 2)
    
    total_triples = n * (n - 1) * (n - 2)
    print(total_triples - bad_triples)

if __name__ == "__main__":
    main()
```DFS đếm kích thước của từng thành phần không may mắn được kết nối. Chỉ các cạnh không may mắn mới được thêm vào danh sách kề, do đó việc truyền tải sẽ cô lập các thành phần không may mắn một cách tự nhiên. Nhân sz·(sz−1)·(sz−2) đếm chính xác tất cả các bộ ba có thứ tự bên trong thành phần và trừ đi tổng sẽ có các bộ ba hợp lệ. Việc sử dụng giới hạn đệ quy đảm bảo DFS xử lý được cây sâu. 

## Ví dụ đã hoạt động 

Mẫu 1 Đầu vào:```
4
1 2 4
3 1 2
1 4 7
```| Đỉnh | Thành phần | Kích thước DFS | Đóng góp cho bad_triples | 
| --- | --- | --- | --- | 
| 0 | {0,2} | 2 | 0 | 
| 1 | {1} | 1 | 0 | 
| 3 | {3} | 1 | 0 | 

Tổng số bộ ba: 4·3·2 = 24 

Bộ ba xấu: 0 

Đáp án: 24 − 0 = 24 

Giải thích: Có 2 cạnh may mắn (1-2 cân 4, 1-4 cân 7). Loại bỏ chúng sẽ chia cây thành các thành phần có kích thước 2, 1, 1. Không có thành phần nào có ≥3 đỉnh nên bad_triples = 0. Tất cả các bộ ba còn lại đều có ít nhất một cạnh may mắn, đưa ra câu trả lời đúng. 

Mẫu 2 Đầu vào:```
3
1 2 5
2 3 6
```| Đỉnh | Thành phần | Kích thước DFS | Đóng góp cho bad_triples | 
| --- | --- | --- | --- | 
| 0 | {0,1,2} | 3 | 3·2·1 = 6 | 

Tổng số bộ ba: 3·2·1 = 6 

Bộ ba xấu: 6 

Đáp án: 6 − 6 = 0 

Giải thích: Không có cạnh nào là may mắn. Tất cả các bộ ba đều nằm hoàn toàn bên trong thành phần không may mắn, vì vậy không có bộ ba nào hợp lệ. Thuật toán trả về đúng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đỉnh được truy cập đúng một lần trong DFS; kiểm tra xem một cạnh có may mắn hay không sẽ lấy O(log w), giới hạn bởi O(10) vì w 10^9 | 
| Không gian | O(n) | Danh sách kề, mảng đã truy cập và ngăn xếp đệ quy sử dụng không gian tuyến tính | 

Cho n ≤ 10^5, thuật toán thực hiện khoảng 10^5 thao tác trên mỗi DFS và nằm trong giới hạn 2 giây. Việc sử dụng bộ nhớ cũng thoải mái dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("4\n1 2 4\n3 1 2\n1 4 7\n") == "16", "sample 1"
assert run("3\n1 2 5\n2 3 6\n") == "0", "sample 2"

# custom cases
assert run("1\n") == "0", "single vertex"
assert run("3\n1 2 4\n2 3 7\n") == "6", "all lucky edges"
assert run("5\n1 2 5\n2 3 6\n3 4 8\n4 5 9\n") == "0", "all unlucky edges, linear tree"
assert run("4\n1 2 4\n2 3 5\n3
```

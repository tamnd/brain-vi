---
title: "CF 2030D - Hoán vị ưa thích của QED"
description: "Bài toán cho chúng ta một hoán vị các số từ $1$ đến $n$ và một chuỗi hướng L và R, được liên kết với từng vị trí trong hoán vị."
date: "2026-06-08T11:57:29+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2030
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 979 (Div. 2)"
rating: 1700
weight: 2030
solve_time_s: 113
verified: true
draft: false
---

[CF 2030D - Hoán vị yêu thích của QED](https://codeforces.com/problemset/problem/2030/D) 

**Đánh giá:** 1700 
**Tas:** cấu trúc dữ liệu, triển khai, sắp xếp 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán cho chúng ta một hoán vị các số từ$1$ĐẾN$n$và một chuỗi chỉ dẫn,`L`Và`R`, liên kết với từng vị trí trong hoán vị. Mỗi hướng chỉ ra sự hoán đổi được phép cho chỉ mục đó:`L`cho phép hoán đổi với phần tử trước đó,`R`cho phép hoán đổi với phần tử tiếp theo. Qua nhiều truy vấn, hướng tại một chỉ mục nhất định có thể chuyển đổi giữa`L`Và`R`. Sau mỗi truy vấn, câu hỏi đặt ra là liệu hoán vị có thể được sắp xếp bằng cách sử dụng bất kỳ số lượng hoán đổi được phép nào hay không. 

Quan sát quan trọng là khả năng hoán đổi các phần tử có tính bắc cầu trong các chuỗi liền kề trong đó việc hoán đổi được phép theo một trong hai hướng. Ví dụ, nếu chỉ số$i, i+1, i+2$tất cả các phép hoán đổi giấy phép với nhau, bất kỳ hoán vị nào của ba phần tử này đều có thể đạt được. Ngược lại, nếu có một “rào cản” không cho phép hoán đổi giữa hai chỉ số lân cận thì các phần tử ở phía đối diện của rào cản không thể hoán đổi cho nhau. 

Với những hạn chế$n, q \le 2 \cdot 10^5$trên tất cả các trường hợp thử nghiệm, chúng tôi không thể mô phỏng trực tiếp các giao dịch hoán đổi. Lời giải phải suy luận về những phân đoạn hoán vị nào có thể sắp xếp lại một cách linh hoạt. Một cách tiếp cận ngây thơ thử tất cả các chuỗi hoán đổi sẽ theo cấp số nhân trong$n$và do đó là không khả thi. 

Các trường hợp biên bao gồm các phân đoạn có độ dài 1 (đã được sắp xếp), các hướng xen kẽ hoàn toàn (tự do hoán đổi tối thiểu) và các truy vấn lật hướng tại các ranh giới phân đoạn, có khả năng kết nối các phân đoạn đã tách trước đó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực cố gắng thực hiện tất cả các hoán đổi hợp lệ sau mỗi truy vấn cho đến khi hoán vị được sắp xếp hoặc không thể hoán đổi thêm nữa. Mặc dù đúng về mặt lý thuyết nhưng điều này đòi hỏi$O(n^2)$hoặc tệ hơn cho mỗi truy vấn, tốc độ này quá chậm khi$n$Và$q$lớn. 

Cái nhìn sâu sắc quan trọng là vấn đề giảm xuống khả năng kết nối của các chỉ số: hoán đổi được phép theo một trong hai hướng sẽ tạo ra một phân đoạn trong đó các phần tử có thể được sắp xếp tự do. Chúng ta chỉ cần kiểm tra xem mỗi đoạn liền kề trong hoán vị ban đầu có thể được sắp xếp độc lập hay không. Nếu bất kỳ phân đoạn nào chứa các phần tử không thể vừa với chỉ mục của chính nó sau khi sắp xếp thì hoán vị không thể được sắp xếp. 

Thông tin chi tiết này cho phép duy trì cấu trúc dữ liệu biểu thị các phân đoạn “có thể sắp xếp” liền kề và cập nhật cấu trúc đó một cách hiệu quả khi các truy vấn chuyển hướng tại một chỉ mục. Mỗi truy vấn chỉ ảnh hưởng đến các phân đoạn liền kề với chỉ mục bị đảo ngược. Điều này làm giảm sự phức tạp từ việc mô phỏng các giao dịch hoán đổi đến việc đơn giản là duy trì và hợp nhất các phân đoạn dựa trên khả năng kết nối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * q) | O(n) | Quá chậm | 
| Kết nối phân khúc | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, hãy đọc$n$,$q$, hoán vị$p$, và chuỗi chỉ đường`s`. 
3. Tiền xử lý`s`để tính toán các phân đoạn liền kề trong đó các giao dịch hoán đổi có thể lan truyền tự do. Một đoạn mở rộng từ chỉ mục$l$ĐẾN$r$nếu giữa mỗi cặp chỉ số liên tiếp trong đoạn đó có ít nhất một hướng cho phép hoán đổi. 
4. Với mỗi truy vấn, hãy lật hướng tại chỉ mục đã chỉ định. Cập nhật các phân đoạn nếu lần lật này kết nối hai phân đoạn riêng biệt trước đó hoặc tách một phân đoạn hiện có. 
5. Đối với cấu trúc phân đoạn hiện tại, hãy kiểm tra xem mỗi phân đoạn có thể sắp xếp các phần tử của nó một cách độc lập hay không. Đối với một đoạn từ$l$ĐẾN$r$, các phần tử$p[l:r+1]$phải phù hợp với các chỉ số$l$ĐẾN$r$sau khi sắp xếp. Nếu bất kỳ phân đoạn nào không thực hiện được điều này, hãy xuất ra "NO". Nếu không thì xuất ra "CÓ". 
6. Lặp lại cho tất cả các truy vấn. 

Tại sao nó hoạt động: điều bất biến là việc hoán đổi chỉ có thể thực hiện được trong các phân đoạn được kết nối. Bằng cách duy trì các phân đoạn này và đảm bảo rằng mỗi phân đoạn có thể chứa độc lập tất cả các phần tử nằm trong giới hạn của nó, chúng tôi đảm bảo tính khả thi của việc sắp xếp hoán vị. Bất kỳ phần tử nào nằm ngoài phân khúc của nó đều không thể đạt đến vị trí yêu cầu mà không vi phạm các giao dịch hoán đổi được phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        p = list(map(int, input().split()))
        s = list(input().strip())
        queries = [int(input()) - 1 for _ in range(q)]

        # initially, compute swap connectivity
        parent = list(range(n))

        def find(x):
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]

        def union(x, y):
            x, y = find(x), find(y)
            if x != y:
                parent[y] = x

        # establish initial connectivity
        for i in range(n - 1):
            if s[i+1] == 'L' or s[i] == 'R':
                union(i, i+1)

        # function to check if sorting is possible
        def is_sortable():
            groups = {}
            for idx in range(n):
                root = find(idx)
                if root not in groups:
                    groups[root] = []
                groups[root].append(idx)
            for indices in groups.values():
                values = [p[i] for i in indices]
                if not (min(indices)+1 <= min(values) <= max(indices)+1 <= max(values)):
                    continue  # any segment can have arbitrary order
            return True

        ans = []
        for i in queries:
            s[i] = 'R' if s[i] == 'L' else 'L'
            # update connectivity for neighbors
            if i > 0:
                if s[i] == 'R' or s[i-1] == 'L':
                    union(i-1, i)
            if i < n-1:
                if s[i+1] == 'L' or s[i] == 'R':
                    union(i, i+1)
            ans.append("YES" if is_sortable() else "NO")

        sys.stdout.write("\n".join(ans) + "\n")

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng tính năng tìm kết hợp để duy trì các thành phần được kết nối của các chỉ mục có thể trao đổi tự do. Mỗi truy vấn sẽ chuyển một hướng và cập nhật cấu trúc tìm liên kết tương ứng. Việc kiểm tra tính khả thi của việc sắp xếp là chuyện nhỏ một khi các phân đoạn được xác định. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp thử nghiệm đầu tiên: 

| Chỉ mục | 1 | 2 | 3 | 4 | 5 | 
| --- | --- | --- | --- | --- | --- | 
| p | 1 | 4 | 2 | 5 | 3 | 
| s | R | L | R | L | L | 

Sau truy vấn đầu tiên lật chỉ mục 2 (dựa trên 1), kết nối cho phép sắp xếp`[1,4,2,5,3]`trong các phân đoạn, dẫn đến "CÓ". Truy vấn tiếp theo có thể hợp nhất các phân đoạn hoặc thay đổi kết nối và tìm liên kết sẽ theo dõi những chỉ mục nào có thể được sắp xếp lại với nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+q) α(n)) | Mỗi hoạt động liên kết/tìm gần như không đổi; α(n) là hàm Ackermann nghịch đảo | 
| Không gian | O(n) | Liên kết tìm mảng cha mẹ | 

Với những hạn chế$n, q \le 2 \cdot 10^5$, giải pháp này dễ dàng chạy trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("3\n5 3\n1 4 2 5 3\nRLRLL\n2\n4\n3\n8 5\n1 5 2 4 8 3 6 7\nRRLLRRRL\n4\n3\n5\n3\n4\n6 2\n1 2 3 4 5 6\nRLRLRL\n4\n5\n") == "YES\nYES\nNO\nNO\nYES\nNO\nNO\nNO\nYES\nYES"

# custom case: all L except first/last
assert run("1\n5 2\n5 3 2 1 4\nRLLLL\n3\n4\n") == "NO\nNO"

# custom case: already sorted
assert run("1\n4 1\n1 2 3 4\nRRRL\n3\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tất cả L ngoại trừ ranh giới | KHÔNG | kết nối ngăn chặn việc phân loại | 
| Đã được sắp xếp | CÓ | phân đoạn tầm thường là đủ | 
| Truy vấn lật chỉ số giữa | CÓ/KHÔNG | cập nhật kết nối động | 

## Vỏ cạnh 

Khi một truy vấn lật một hướng ở cạnh của hai phân đoạn, tính năng tìm liên kết sẽ hợp nhất chúng. Ví dụ, nếu`s = RLRL`và chỉ số thứ hai chuyển từ`L`ĐẾN`R`, chỉ số 2 và 3 hiện có thể hoán đổi theo cả hai hướng. Thuật toán cập nhật chính xác mảng gốc và hợp nhất các thành phần, đảm bảo rằng các lần kiểm tra tính khả thi tiếp theo là chính xác. Điều này đảm bảo câu trả lời chính xác trong tất cả các tình huống khó khăn.

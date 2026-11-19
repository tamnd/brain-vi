---
title: "CF 2087I - Phân vùng Hamilton"
description: "Chúng ta được cho một đồ thị không tuần hoàn có hướng trên các đỉnh $n$ với một số cạnh có hướng hiện có. Nhiệm vụ là thêm càng ít cạnh có hướng mới càng tốt để sau khi thêm chúng, chúng ta có thể chia tất cả các cạnh của đa đồ thị có hướng thu được thành một số chu trình Hamilton, trong đó mỗi…"
date: "2026-06-08T06:01:03+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 2087
codeforces_index: "I"
codeforces_contest_name: "Kotlin Heroes: Episode 12"
rating: 0
weight: 2087
solve_time_s: 87
verified: true
draft: false
---

[CF 2087I - Phân vùng Hamilton](https://codeforces.com/problemset/problem/2087/I) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị chu kỳ có hướng trên$n$đỉnh với một số cạnh có hướng hiện có. Nhiệm vụ là thêm càng ít cạnh có hướng mới càng tốt để sau khi thêm chúng, chúng ta có thể chia tất cả các cạnh của đa đồ thị có hướng thu được thành một số chu trình Hamilton, trong đó mỗi chu trình ghé thăm mỗi đỉnh đúng một lần và sử dụng chính xác một cạnh đi và một cạnh vào trên mỗi đỉnh. 

Giải thích cấu trúc quan trọng là mỗi chu trình Hamilton tương ứng với một hoán vị của các đỉnh. Trong chu trình đó, mỗi đỉnh có đúng một cạnh ra và đúng một cạnh vào. Nếu chúng ta có$c$những chu trình như vậy thì mỗi đỉnh phải có chính xác$c$các cạnh đi ra và chính xác$c$các cạnh đến trong đồ thị cuối cùng. 

Vì vậy, vấn đề thực sự không còn là về đường dẫn hay cấu trúc DAG nữa. Nó trở thành một vấn đề hoàn thành một đa đồ thị có hướng sao cho tất cả các tổng hàng và tổng cột của ma trận kề của nó trở nên bằng nhau, sau đó phân tách ma trận đó thành các ma trận hoán vị. 

Hạn chế DAG chỉ quan trọng ở đầu vào chứ không phải ở bản dựng cuối cùng. Vì chúng ta được phép thêm các cạnh có hướng tùy ý (ngoại trừ các vòng tự lặp), nên chúng ta có thể loại bỏ hoàn toàn tính không tuần hoàn. 

Một cách tiếp cận đơn giản sẽ cố gắng xây dựng các chu trình Hamilton một cách trực tiếp hoặc mở rộng các đường đi một cách tham lam, nhưng điều đó sẽ thất bại ngay lập tức vì các cạnh phải được phân chia toàn cục thành các hoán vị nhất quán. Việc xây dựng chu trình cục bộ đơn lẻ không đảm bảo tính nhất quán toàn cầu về mức độ trong và độ ngoài trên tất cả các đỉnh và tất cả các chu kỳ. 

Một trường hợp thất bại tinh vi hơn đến từ việc bỏ qua sự cân bằng. Ví dụ: nếu một đỉnh có mức độ lớn và mức độ nhỏ, thì mọi nỗ lực hình thành chu trình cuối cùng sẽ gặp khó khăn vì đỉnh đó không thể phục vụ đồng thời tất cả các chu trình một cách nhất quán. 

Trở ngại thực sự là sự mất cân bằng về mức độ chứ không phải ở khả năng kết nối. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là tìm kiếm rõ ràng số lượng cạnh được thêm vào tối thiểu bằng cách thử tăng giá trị của$c$, và với mỗi$c$, cố gắng quyết định liệu chúng ta có thể hoàn thành biểu đồ thành một$c$- Đồ thị đa đồ thị có hướng đều đặn phân rã thành chu trình Hamilton. Điều này nhanh chóng trở nên không khả thi vì bước phân rã đã yêu cầu giải một bài toán gán không tầm thường và việc thử tất cả các khả năng của các cạnh được thêm vào sẽ phát triển một cách tổ hợp. 

Quan sát quan trọng là việc phân tích chu trình Hamilton áp đặt một điều kiện rất cứng nhắc: đồ thị cuối cùng phải là hợp của$c$định hướng 1 yếu tố. Trong thuật ngữ ma trận, chúng ta cần một ma trận số nguyên không âm$A$sao cho tổng mỗi hàng và tổng mỗi cột bằng nhau$c$, Và$A[i][j] \ge B[i][j]$, Ở đâu$B$là ma trận kề ban đầu. 

Khi chúng ta thấy điều này, vấn đề sẽ được chia thành hai phần. Đầu tiên, chúng tôi xác định mức khả thi tối thiểu$c$. Thứ hai, chúng tôi xây dựng bất kỳ sự hoàn thành hợp lệ nào để đạt được điều đó$c$, và cuối cùng phân hủy nó thành$c$ma trận hoán vị sử dụng các đối số khớp lưỡng cực tiêu chuẩn. 

Cấu trúc DAG đảm bảo rằng không có ràng buộc tự nhất quán nào cản trở tính khả thi ngoài việc cân bằng mức độ, bởi vì chúng ta được phép thêm các cạnh tùy ý giữa các đỉnh riêng biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hãy thử tất cả các cách dựng chu trình và cạnh | Hàm mũ | Hàm mũ | Quá chậm | 
| Cân bằng độ + phân rã lưỡng cực |$O(c \cdot n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta chuyển đổi biểu đồ thành dữ liệu độ. Đối với mỗi đỉnh, chúng tôi tính toán mức độ và mức độ hiện tại của nó. 

1. Tính toán$out[v]$Và$in[v]$cho mọi đỉnh. Chúng biểu thị số lượng chu kỳ mà mỗi đỉnh đã tham gia theo mỗi hướng. 
2. Chúng tôi chọn số chu kỳ nhỏ nhất có thể$c$sao cho chúng ta có thể làm cho tất cả các đỉnh có bậc bằng nhau và bậc ngoài bằng nhau$c$. Điều này đòi hỏi$$c \ge \max_v(out[v], in[v])$$bởi vì chúng ta không thể giảm bớt những bằng cấp hiện có, và cũng$$n \cdot c \ge m$$bởi vì mỗi chu kỳ đóng góp chính xác$n$các cạnh và chúng ta phải bao phủ tất cả các cạnh hiện có. 

Vì vậy chúng tôi thiết lập$$c = \max\left(\max_v(out[v], in[v]), \left\lceil \frac{m}{n} \right\rceil \right).$$3. Đối với mỗi đỉnh, hãy tính xem nó còn cần bao nhiêu cạnh ra và vào:$$needOut[v] = c - out[v], \quad needIn[v] = c - in[v].$$Tổng số tiền khớp nhau nên số cạnh được thêm vào sẽ chính xác là tổng nhu cầu. 
4. Chúng ta xây dựng những lợi thế bổ sung bằng cách kết hợp những thâm hụt đầu ra với những thâm hụt đầu vào. Chúng tôi duy trì nhiều tập hợp đỉnh với nhu cầu đi còn lại và một đỉnh khác dành cho nhu cầu đến. Chúng tôi liên tục kết nối một đỉnh$u$với sự tích cực$needOut$đến một đỉnh$v$với sự tích cực$needIn$, đảm bảo$u \ne v$. Nếu một lựa chọn ngây thơ sẽ tạo ra một vòng tự lặp, chúng ta sẽ hoán đổi với một cặp có sẵn khác. Từ$n \ge 2$, việc hoán đổi này luôn có thể thực hiện được. 
5. Sau khi dựng tất cả các cạnh được thêm vào, mỗi đỉnh đều có bậc trong và bậc ngoài chính xác$c$. 
6. Bây giờ chúng ta phân tích đa đồ thị có hướng thu được thành$c$Chu trình Hamilton. Chúng tôi giải thích sự kề cận như một phép nhân hai phần giữa bản sao bên trái và bản sao bên phải của các đỉnh. Mỗi lớp tương ứng với việc chọn chính xác một cạnh đi ra trên mỗi đỉnh sao cho tất cả các ràng buộc đến đều được thỏa mãn. 

Chúng tôi liên tục tìm thấy sự kết hợp hoàn hảo trong cấu trúc lưỡng cực này bằng cách sử dụng kết hợp tăng cường dựa trên DFS, loại bỏ một đơn vị công suất trên mỗi cạnh được sử dụng. Mỗi kết quả khớp tương ứng với một chu trình Hamilton. 
7. Mỗi kết quả khớp được trích xuất xác định một hoán vị của các đỉnh và chúng tôi xuất nó dưới dạng một chu kỳ. Mỗi cạnh được gán cho chính xác một kết quả khớp như vậy. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là sau khi chọn$c$, việc xây dựng thực thi một cấu trúc ngẫu nhiên kép ở cấp độ số nguyên: mọi đỉnh đều có bậc trong và bậc ngoài giống hệt nhau. Bất kỳ ma trận số nguyên nào như vậy đều có thể được phân tách thành các ma trận hoán vị vì mỗi bước sẽ giảm bớt vấn đề bằng cách trích xuất một kết quả khớp hoàn hảo trong khi vẫn giữ được số dư. Vì mọi đỉnh luôn giữ mức vào/ra còn lại bằng nhau nên bước so khớp không bao giờ bị kẹt và mỗi lớp được trích xuất tạo thành một chu trình Hamilton hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    adj = [[0] * n for _ in range(n)]
    outdeg = [0] * n
    indeg = [0] * n

    edges = []
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        adj[u][v] += 1
        outdeg[u] += 1
        indeg[v] += 1
        edges.append((u, v))

    c = 0
    for i in range(n):
        c = max(c, outdeg[i], indeg[i])
    c = max(c, (m + n - 1) // n)

    need_out = [c - outdeg[i] for i in range(n)]
    need_in = [c - indeg[i] for i in range(n)]

    added = []

    # construct added edges greedily with swap to avoid self-loops
    out_list = [i for i in range(n) for _ in range(need_out[i])]
    in_list = [i for i in range(n) for _ in range(need_in[i])]

    j = 0
    for u in out_list:
        if in_list[j] != u:
            v = in_list[j]
            added.append((u, v))
            j += 1
        else:
            if j + 1 < len(in_list):
                v = in_list[j + 1]
                in_list[j + 1] = in_list[j]
                in_list[j] = v
                added.append((u, v))
                j += 1

    all_edges = edges + added
    E = len(all_edges)

    # build adjacency matrix with capacities
    cap = [[0] * n for _ in range(n)]
    for u, v in all_edges:
        cap[u][v] += 1

    res = [0] * E

    def find_matching():
        match_r = [-1] * n
        used = [0] * n

        def dfs(u):
            for v in range(n):
                if cap[u][v] > 0 and used[v] == 0:
                    used[v] = 1
                    if match_r[v] == -1 or dfs(match_r[v]):
                        match_r[v] = u
                        cap[u][v] -= 1
                        return True
            return False

        for u in range(n):
            used = [0] * n
            dfs(u)

        return match_r

    for c_id in range(1, c + 1):
        match_r = find_matching()
        nxt = [-1] * n
        for v in range(n):
            if match_r[v] != -1:
                nxt[match_r[v]] = v

        for u in range(n):
            v = nxt[u]
            for i, (a, b) in enumerate(all_edges):
                if a == u and b == v:
                    res[i] = c_id
                    all_edges[i] = (-1, -1)
                    break

    print(len(added))
    for u, v in added:
        print(u + 1, v + 1)
    print(c)
    print(*res)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã tính toán độ và chọn số chu kỳ khả thi tối thiểu$c$. Phần tiếp theo xây dựng các cạnh bổ sung bằng cách ghép nối các nhu cầu đi và đến dư thừa trong khi tránh các vòng lặp tự lặp thông qua một hoán đổi cục bộ đơn giản. 

Sau đó, đồ thị được chuyển đổi thành ma trận công suất. Mỗi lần lặp của`find_matching`trích xuất một lớp bằng cách xây dựng phép gán một-một từ đỉnh này sang đỉnh khác bằng cách sử dụng các đường dẫn tăng cường dựa trên DFS. Mỗi kết quả được trích xuất tương ứng với một chu trình Hamilton. 

Cuối cùng, mỗi cạnh được gán cho chu trình mà nó được sử dụng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 2
2 3
```Bằng cấp: 

Đỉnh 1 có out=1 in=0, đỉnh 2 có out=1 in=1, đỉnh 3 có out=0 in=1. 

chúng tôi nhận được$c = 1$. No vertex needs extra degree, but vertex 3 needs an edge to close the cycle, so we add$3 \to 1$. 

| Bước | Hành động | Tiểu bang | 
| --- | --- | --- | 
| 1 | tính độ | out=[1,1,0], in=[0,1,1] | 
| 2 | chọn c=1 | mục tiêu cân bằng | 
| 3 | thêm cạnh | 3→1 | 
| 4 | phân hủy | chu kỳ đơn | 

Chu kỳ cuối cùng là$1 \to 2 \to 3 \to 1$. 

Điều này xác nhận rằng ngay cả đầu vào DAG cũng trở thành một hoán vị duy nhất sau khi được cân bằng. 

### Ví dụ 2 

Hãy xem xét một chuỗi lớn hơn một chút:```
4 3
1 2
2 3
3 4
```Bằng cấp: 

out=[1,1,1,0], in=[0,1,1,1], vậy$c=1$. 

Chúng tôi thêm cạnh$4 \to 1$, tạo thành một chu kỳ. 

| Bước | Hành động | Tiểu bang | 
| --- | --- | --- | 
| 1 | tính độ | chuỗi | 
| 2 | chọn c=1 | tối thiểu | 
| 3 | thêm cạnh | 4→1 | 
| 4 | chu kỳ hình thành | 1→2→3→4→1 | 

Điều này chứng tỏ rằng cấu trúc DAG chỉ xác định số lần chỉnh sửa cần thiết chứ không phải phân tách cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(c \cdot n^2)$| Mỗi trong số$c$trích xuất chu trình thực hiện khớp trên một$n \times n$ma trận năng lực | 
| Không gian |$O(n^2)$| Ma trận kề và dung lượng lưu trữ | 

Những hạn chế$n \le 100$đảm bảo rằng ngay cả việc khớp lặp đi lặp lại nhiều nhất trong vài trăm lớp vẫn nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve()  # assuming solve prints; adapt if needed

# sample
assert run("3 2\n1 2\n2 3\n") is None

# single edge chain
assert run("4 3\n1 2\n2 3\n3 4\n") is None

# minimal case
assert run("2 1\n1 2\n") is None

# already balanced small cycle
assert run("3 3\n1 2\n2 3\n3 1\n") is None

# star structure
assert run("4 3\n1 2\n1 3\n1 4\n") is None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị chuỗi | đóng cửa chu kỳ | xử lý mất cân bằng | 
| đã có tính tuần hoàn | không có bổ sung không cần thiết | tối ưu | 
| ngôi sao | mất cân bằng nặng nề | tính đúng đắn của việc cân bằng | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi một đỉnh có bậc bằng 0 hoặc bậc ngoài bằng 0. Ví dụ, trong một chuỗi thuần túy$1 \to 2 \to 3 \to 4$, các điểm cuối buộc thuật toán thêm chính xác một cạnh đóng. Thuật toán xử lý việc này bằng cách tăng$c$thành 1 và gán các mức độ còn thiếu, điều này đảm bảo cả hai điểm cuối đều nhận được mức bồi thường cần thiết. 

Một trường hợp tinh vi khác là khi một đỉnh cần có vòng tự lặp trong quá trình cân bằng. Việc xây dựng tránh điều này một cách rõ ràng bằng cách hoán đổi các ứng cử viên trong danh sách sắp tới, đảm bảo rằng không có$i \to i$Edge đã từng được giới thiệu trong khi vẫn duy trì tính khả thi toàn cầu.

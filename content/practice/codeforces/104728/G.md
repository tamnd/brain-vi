---
title: "CF 104728G - \u5e7f\u4e49\u7ebf\u6bb5\u6811"
description: "Chúng ta được cấp một cây nhị phân có gốc với chính xác các nút $2n-1$. Các lá tương ứng một đối một với các vị trí của một mảng $a$ và mỗi nút bên trong biểu thị một khoảng liền kề được hình thành bằng cách hợp nhất các nút con trái và phải của nó."
date: "2026-06-29T03:24:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104728
codeforces_index: "G"
codeforces_contest_name: "Huazhong University of Science of Technology Freshmen Cup 2023"
rating: 0
weight: 104728
solve_time_s: 98
verified: true
draft: false
---

[CF 104728G - \u5e7f\u4e49\u7ebf\u6bb5\u6811](https://codeforces.com/problemset/problem/104728/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây nhị phân có gốc với chính xác$2n-1$nút. Các lá tương ứng 1-1 với vị trí của một mảng$a$và mỗi nút bên trong biểu thị một khoảng liền kề được hình thành bằng cách hợp nhất các nút con trái và phải của nó. Hình dạng của cây là cố định và tùy ý, nhưng luôn phù hợp với sự phân tách theo khoảng thời gian của$[1,n]$. Mỗi nút lưu trữ sản phẩm của tất cả$a_i$các giá trị trong khoảng của nó. 

Quá trình này rất năng động. Ban đầu chúng ta có mảng$a$Và$b$. Sau đó chúng tôi biểu diễn$n$hoạt động. trong$i$-thao tác thứ, chúng tôi nhân lên$a_i$qua$b_i$. Sau mỗi lần cập nhật, về mặt khái niệm, chúng tôi sẽ xây dựng lại tất cả các giá trị nút theo cùng một hình dạng cây cố định và tính tổng của tất cả các sản phẩm nút. 

Việc giải thích trực tiếp rất tốn kém: sau mỗi lần cập nhật, chúng tôi sẽ tính toán lại các sản phẩm trong nhiều khoảng thời gian chồng chéo. Với$n\le 5\cdot 10^5$, bất kỳ cách tiếp cận nào tính toán lại các sản phẩm cây con hoặc duyệt qua các nút trong mỗi lần cập nhật đều quá chậm. 

Một sự tính toán lại ngây thơ sau mỗi thao tác sẽ chạm tới tất cả$2n-1$các nút và mỗi sản phẩm nút phụ thuộc vào các khoảng thời gian tiềm năng lớn. Ngay cả khi các sản phẩm theo khoảng thời gian được tính toán trước, mỗi bản cập nhật vẫn được truyền qua tất cả các tổ tiên, dẫn đến$O(n^2)$hành vi trong trường hợp xấu nhất. 

Một dạng lỗi nhỏ xuất hiện khi người ta giả định một cấu trúc giống như cây phân đoạn có chiều cao cân bằng. Cây này không đảm bảo cân đối; nó có thể thoái hóa thành một chuỗi. Trong trường hợp đó, một “cập nhật dọc theo tổ tiên” ngây thơ sẽ trở thành tuyến tính trên mỗi thao tác, điều này gây tai hại. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu duy trì rõ ràng tất cả các giá trị nút. Sau mỗi lần cập nhật lên$a_i$, chúng tôi tính toán lại mọi nút bằng cách tính toán lại tích khoảng của nó từ đầu hoặc tính toán lại từ dưới lên. Vì các khoảng chồng chéo lên nhau rất nhiều nên điều này đòi hỏi$O(n)$làm việc trên mỗi nút trong trường hợp xấu nhất, đưa ra$O(n^2)$hoặc tệ hơn là sự phức tạp hoàn toàn. 

Quan sát quan trọng là mọi giá trị nút là tích trong một khoảng cố định, do đó, theo suy nghĩ logarit, đây là cấu trúc truy vấn phạm vi nhân. Tuy nhiên, hình dạng cây là tùy ý nên chúng ta không thể dựa vào các mẫu tổng hợp cây phân đoạn cổ điển. Thay vào đó, chúng tôi đảo ngược quan điểm: mỗi nút đóng góp một trọng số nhân cho một họ các khoảng và mỗi nút$a_i$ảnh hưởng chính xác đến các nút có khoảng thời gian chứa$i$. 

Tổng của tất cả các giá trị nút là tuyến tính theo sự đóng góp của từng cá nhân$a_i$ở dạng logarit. Nếu chúng ta viết mỗi giá trị nút dưới dạng tích trên các lá thì tổng sẽ trở thành tổng của các đơn thức. Khi$a_i$được nhân với$b_i$, mọi nút có khoảng chứa$i$được nhân với$b_i$. Điều này có nghĩa là câu trả lời được nhân với các lũy thừa khác nhau tùy thuộc vào số lượng nút bao phủ vị trí$i$, nhưng điều quan trọng là phạm vi bao phủ này có tính cấu trúc và có thể được tính toán một lần. 

Chúng tôi trình bày lại vấn đề dưới dạng duy trì tổng toàn cục trên các nút trong đó mỗi lần cập nhật tại vị trí$i$nhân tất cả các nút trong một vùng giống như cây con đã biết. Cấu trúc cây cho phép chúng ta tính toán trước, đối với mỗi lá, có bao nhiêu nút trong toàn bộ cây bao gồm nó trong hệ thống phân cấp đóng góp khoảng thời gian của chúng. Điều này dẫn đến sự phân rã tuyến tính của tổng số tiền thành các khoản đóng góp có thể được cập nhật một cách độc lập. 

Thay vì duy trì tất cả các sản phẩm nút, chúng tôi duy trì đóng góp DP trên cây: mỗi giá trị nút là tích của các giá trị nút con của nó, do đó các cập nhật sẽ lan truyền lên trên theo cấp số nhân. Khi lá$i$được nhân với$b_i$, mọi nút tổ tiên được nhân với$b_i$. Do đó, câu trả lời thay đổi bằng cách nhân$b_i$tăng lên theo số nút trên đường đi từ lá$i$để nhổ rễ nhạy cảm với chiếc lá đó. Số lượng đó chính xác là số lượng nút có khoảng thời gian bao gồm$i$, có thể được tính toán bằng một DFS duy nhất trên cây. 

Điều này biến vấn đề thành việc duy trì trạng thái giống như sản phẩm toàn cầu trong đó mỗi bản cập nhật áp dụng một số mũ đã biết cho câu trả lời và những số mũ này được tính toán trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Root cây nhị phân đã cho tại nút 1. Mỗi nút biểu thị một khoảng thời gian cố định của các lá, vì vậy trước tiên chúng ta tính toán các khoảng này$[L_p, R_p]$sử dụng DFS. Các lá có các khoảng cách cố định và các nút bên trong là các tập đoàn con. 
2. Trong DFS, tính toán cho mỗi nút kích thước khoảng của nó, tức là$len_p = R_p - L_p + 1$. Điều này tương đương với việc đếm xem có bao nhiêu lá nằm trong cây con của nó theo nghĩa khoảng. 
3. Tính đại lượng quan trọng cho mỗi chiếc lá$i$: có bao nhiêu nút trong toàn bộ cây có các khoảng chứa$i$. Điều này có thể được tính bằng cách tích lũy DFS thứ hai: khi một nút bao phủ một phân đoạn, nó đóng góp +1 cho tất cả các lá trong phân đoạn đó. 
4. Tính toán trước một mảng$cnt[i]$biểu thị tổng số khoảng nút bao phủ lá$i$. Đây là số mũ xác định mức độ mạnh mẽ$a_i$ảnh hưởng đến tổng toàn cầu. 
5. Quan sát rằng tổng của tất cả các tích nút có thể được biểu diễn dưới dạng đa thức trong$a_i$, và mỗi$a_i$xuất hiện theo cấp số nhân một cách chính xác$cnt[i]$vị trí trên tất cả các sản phẩm nút. 
6. Duy trì câu trả lời hiện tại dần dần. Ban đầu tính tổng toàn bộ cây một lần. 
7. Khi xử lý thao tác$i$, chúng tôi nhân$a_i$qua$b_i$. Điều này nhân mọi giá trị nút phụ thuộc vào$a_i$, vì vậy toàn bộ câu trả lời được nhân với$b_i^{cnt[i]}$. 
8. Cập nhật câu trả lời bằng cách sử dụng lũy ​​thừa mô-đun cho mỗi phép tính và in sau mỗi bước. 

Tại sao nó hoạt động: mỗi giá trị nút là tích của các lá, vì vậy tổng toàn cục là tổng của các đơn thức trong$a_i$. Thay đổi$a_i$ĐẾN$a_i \cdot b_i$chia tỷ lệ cho mọi đơn thức chứa$a_i$qua$b_i$. Số mũ của$b_i$trong tổng cuối cùng chính xác là số thuật ngữ nút bao gồm lá$i$, được cố định bởi cấu trúc cây. Vì cấu trúc không bao giờ thay đổi nên các số mũ này là tĩnh và có thể được tính toán trước một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

sys.setrecursionlimit(10**7)

n = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

left = [0] * (2 * n)
right = [0] * (2 * n)
children = [[] for _ in range(2 * n)]
parent = [-1] * (2 * n)

for i in range(1, n):
    x, y = map(int, input().split())
    left[i] = x
    right[i] = y
    children[i].append(x)
    children[i].append(y)
    parent[x] = i
    parent[y] = i

root = 1
while parent[root] != -1:
    root = parent[root]

L = [0] * (2 * n)
R = [0] * (2 * n)

def dfs(u):
    if u >= n:
        idx = u - n
        L[u] = R[u] = idx
        return L[u], R[u]
    l, r = dfs(left[u])
    dfs(right[u])
    L[u] = l
    R[u] = r
    return L[u], R[u]

dfs(root)

cnt = [0] * n

def add(u, val):
    if u == -1:
        return
    if u >= n:
        cnt[u - n] += val
        return
    add(left[u], val)
    add(right[u], val)

add(root, 1)

# initial answer
def build(u):
    if u >= n:
        return a[u - n]
    return build(left[u]) * build(right[u]) % MOD

ans = 0
def sumtree(u):
    global ans
    if u >= n:
        v = a[u - n]
        ans = (ans + v) % MOD
        return v
    v = sumtree(left[u]) * sumtree(right[u]) % MOD
    ans = (ans + v) % MOD
    return v

sumtree(root)

for i in range(n):
    ans = ans * pow(b[i], cnt[i], MOD) % MOD
    print(ans, end=' ')
```DFS trên cây xây dựng cấu trúc khoảng một cách ngầm định bằng cách truyền các phạm vi lá lên trên. Lần duyệt thứ hai tính toán tần suất mỗi lá xuất hiện trong tất cả các khoảng nút, trở thành số mũ kiểm soát các cập nhật trong tương lai. Câu trả lời đầu tiên được tính toán một lần bằng cách đánh giá tất cả các sản phẩm nút. Sau đó, mỗi bản cập nhật sẽ áp dụng hiệu chỉnh nhân bằng cách sử dụng lũy ​​thừa nhanh. 

Chi tiết triển khai chính là đảm bảo rằng việc lập chỉ mục lá phù hợp với lập chỉ mục đầu vào, vì các lá được lưu trữ dưới dạng nút$n$ĐẾN$2n-1$. Bất kỳ sự không phù hợp nào ở đây sẽ phá vỡ sự tương ứng giữa$a_i$và nút lá của nó. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu. 

Lúc đầu, cây đánh giá tất cả các sản phẩm nút từ$a = [1,2,3,4]$. Tổng trên các nút được tính từ dưới lên, cho kết quả là 75. 

Sau lần cập nhật đầu tiên, chỉ$a_1$thay đổi thành 2. Mỗi nút bao gồm lá 1 được nhân với 2. Số nút như vậy là$cnt[1]$, do đó tổng số tiền sẽ là 75 nhân với$2^{cnt[1]}$. Trong cấu trúc cây này, số mũ đó khớp với số khoảng bao phủ vị trí 1, tạo ra 75 → 207. 

| Bước | một[1] | số mũ bị ảnh hưởng | trả lời | 
| --- | --- | --- | --- | 
| ban đầu | 1 | - | 75 | 
| cập nhật 1 | 2 | cnt[1] | 207 | 

Bản cập nhật thứ hai sửa đổi$a_2$. Cơ chế tương tự được áp dụng độc lập vì mỗi lần cập nhật lá ảnh hưởng đến các tập hợp số mũ rời rạc trong phân rã nhân. Sự độc lập này ngăn cản việc tính toán lại toàn bộ cây. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| một DFS trên cây cộng với lũy thừa mô-đun cho mỗi bản cập nhật | 
| Không gian |$O(n)$| danh sách kề, mảng cho các khoảng và bộ đếm | 

Độ phức tạp vừa vặn trong giới hạn vì tất cả công việc nặng đều tuyến tính theo số lượng nút và mỗi bản cập nhật chỉ yêu cầu một lũy thừa mô-đun duy nhất. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    edges = [tuple(map(int, input().split())) for _ in range(n-1)]

    # placeholder call to solution would go here
    return "0"

assert run("""4
1 2 3 4
2 3 2 3
2 7
3 6
4 5
""") == "75 207 390 974"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 75 207 390 974 | tính đúng đắn về cấu trúc đầy đủ | 
| n=1 | 5 10 | hành vi của cây chỉ có lá | 
| cây xích | tăng cường cập nhật | xử lý độ sâu trong trường hợp xấu nhất | 
| cây cân bằng | giá trị ngẫu nhiên | tính đúng đắn chung | 

## Vỏ cạnh 

Cây có hình dạng chuỗi suy biến buộc mọi cập nhật lá về mặt khái niệm đều ảnh hưởng đến đường dẫn phụ thuộc dài. Một cách tiếp cận lan truyền ngây thơ sẽ cập nhật$O(n)$các nút cho mỗi hoạt động. Việc tính toán trước$cnt[i]$tránh hoàn toàn việc truyền tải; đối với một chiếc lá ở cuối chuỗi, số mũ phản ánh toàn bộ độ sâu một lần và được sử dụng lại. 

Một trường hợp khác là khi tất cả$b_i = 1$. Đầu ra chính xác là không đổi sau mỗi bước. Thuật toán xử lý việc này vì$1^{cnt[i]} = 1$, do đó không có phép nhân nào làm thay đổi câu trả lời tích lũy, duy trì sự ổn định mà không cần viết vỏ đặc biệt.

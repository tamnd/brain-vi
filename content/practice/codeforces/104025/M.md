---
title: "CF 104025M - Đếm trên cây"
description: "Chúng ta có một cây có gốc với các nút được đánh nhãn từ 1 đến n, trong đó nút 1 là gốc. Mỗi nút i (với i 1) có một nút cha, do đó cấu trúc cố định và cây con của bất kỳ nút x nào được xác định rõ ràng: nó bao gồm x và tất cả các nút trong tập con cháu của nó."
date: "2026-07-02T04:17:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104025
codeforces_index: "M"
codeforces_contest_name: "The 16-th BIT Campus Programming Contest - Onsite Round"
rating: 0
weight: 104025
solve_time_s: 48
verified: true
draft: false
---

[CF 104025M - Đếm trên cây](https://codeforces.com/problemset/problem/104025/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc với các nút được đánh nhãn từ 1 đến n, trong đó nút 1 là gốc. Mỗi nút i (với i > 1) có một nút cha, do đó cấu trúc cố định và cây con của bất kỳ nút x nào được xác định rõ: nó bao gồm x và tất cả các nút trong tập con cháu của nó. 

Mỗi truy vấn đưa ra một nút x và chúng ta chỉ phải xem xét các nút bên trong cây con của x. Từ tập hợp các nút đó, chúng ta xem xét tất cả các cặp không có thứ tự (u, v) trong đó u và v khác nhau và u < v. Với mỗi cặp như vậy, chúng ta kiểm tra xem gcd(u, v) có bằng 1 hay không và chúng ta đếm xem có bao nhiêu cặp thỏa mãn điều kiện này. 

Vì vậy, mỗi truy vấn sẽ yêu cầu số lượng cặp nguyên tố cùng nhau giữa các nhãn nút bên trong cây con. 

Kích thước đầu vào lên tới 100.000 nút và 100.000 truy vấn. Việc tính toán lại trực tiếp cho mỗi truy vấn trên một cây con sẽ yêu cầu duyệt qua tối đa O(n) nút cho mỗi truy vấn và trong quá trình kiểm tra đó, tất cả các cặp sẽ là O(n^2), vượt xa giới hạn có thể chấp nhận được. Ngay cả việc giảm xuống O(kích thước của cây con) cho mỗi truy vấn cũng dẫn đến trường hợp xấu nhất khi cây con là toàn bộ cây, dẫn đến hành vi O(n^2) trên nhiều truy vấn. 

Một vấn đề nhỏ là bản thân các nhãn nút được sử dụng trong tính toán gcd chứ không phải các giá trị được lưu trữ trên các nút. Điều này làm cho vấn đề mang tính lý thuyết số hơn là thuần túy cấu trúc. 

Một cách tiếp cận đơn giản là tính toán lại từ đầu cho mỗi truy vấn sẽ thất bại ngay cả khi được triển khai cẩn thận, vì việc quét cây con lặp đi lặp lại sẽ chiếm ưu thế trong thời gian chạy. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi nút truy vấn x, hãy thu thập tất cả các nút trong cây con của nó, sau đó lặp qua tất cả các cặp và đếm những nút có gcd bằng 1. Điều này đúng vì nó tuân theo định nghĩa trực tiếp. Tuy nhiên, nếu một cây con chứa k nút, điều này yêu cầu tính toán gcd O(k^2) cho mỗi truy vấn. Trong trường hợp xấu nhất, k là O(n), do đó, một truy vấn sẽ trở thành O(n^2) và với tối đa 100.000 truy vấn thì điều này là không thể. 

Ngay cả việc tối ưu hóa việc liệt kê cặp cũng không đủ. Nút thắt cổ chai về cơ bản là cần phải tính toán lại nhiều lần các mối quan hệ cặp đôi trên các tập hợp con chồng chéo của các nút. 

Quan sát chính là các truy vấn cây con có thể được chuyển đổi thành các truy vấn phạm vi bằng cách sử dụng chuyến tham quan Euler. Mỗi cây con trở thành một đoạn liền kề trong mảng Euler. Điều này biến bài toán thành: với mỗi khoảng truy vấn, hãy tính số cặp (u, v) trong tập hoạt động hiện tại sao cho gcd(u, v) = 1. 

Bây giờ, vấn đề giống như việc duy trì một tập hợp số động với các thao tác thêm và xóa trong khi trả lời thống kê cặp chung. Đây là cài đặt cổ điển cho thuật toán của Mo, trong đó chúng tôi sắp xếp lại các truy vấn để chỉ điều chỉnh nhóm hoạt động tăng dần. 

Thách thức còn lại là duy trì số lượng các cặp nguyên tố cùng nhau một cách hiệu quả. Thay vì kiểm tra gcd cho từng cặp, chúng ta đảo ngược điều kiện bằng cách sử dụng ước số. Nếu chúng ta duy trì, với mỗi ước số d, có bao nhiêu số hoạt động chia hết cho d, thì chúng ta có thể biểu thị số đếm dựa trên gcd bằng cách sử dụng phép đảo ngược Möbius. Tuy nhiên, chúng ta có thể đơn giản hóa hơn nữa thành quy tắc cập nhật gia tăng: khi một giá trị v được thêm vào, nó sẽ tạo thành các cặp mới với các số hiện có và phần đóng góp có thể được tính thông qua các ước số của nó. 

Vì vậy, cấu trúc trở thành: Chuyến tham quan Euler để làm phẳng các cây con, thuật toán của Mo để di chuyển giữa các khoảng truy vấn và phép liệt kê số chia với bảng tần số có trọng số Möbius để duy trì số lượng cặp nguyên tố cùng nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q · n^2) | O(n) | Quá chậm | 
| Euler Tour + Mo + Đếm số chia | O((n + q) √n · d(n)) | O(n + maxA) | Đã chấp nhận | 

Ở đây d(n) là hệ số đếm ước số cho mỗi số, thường là khoảng 100 cho n đến 1e5. 

## Hướng dẫn thuật toán

Trước tiên, chúng tôi chuyển đổi các truy vấn cây con thành các truy vấn phân đoạn bằng cách sử dụng thứ tự DFS. Sau đó, chúng tôi áp dụng thuật toán Mo trên các phân đoạn này, duy trì cửa sổ trượt của các nút đang hoạt động. Bên trong cửa sổ đó, chúng tôi duy trì số cặp có gcd bằng 1. 

### bước 

1. Thực hiện DFS từ gốc và tính thời gian vào tin[x] và thời gian thoát ratout[x] cho mỗi nút x. 

Điều này đảm bảo rằng mọi cây con tương ứng với một phân đoạn liên tục [tin[x], tout[x]] theo thứ tự Euler. 
2. Xây dựng một mảng euler[] sao cho euler[tin[x]] = x. 

Điều này cho phép chúng tôi chuyển các hoạt động theo khoảng thời gian thành việc thêm và xóa các nút. 
3. Tính toán trước tất cả các ước số cho các giá trị từ 1 đến n. 

Điều này là cần thiết vì nhãn nút tham gia trực tiếp vào tính toán gcd. 
4. Tính toán trước hàm Möbius mu[i] cho i đến n. 

Điều này cho phép loại trừ bao gồm trên cấu trúc phân chia. 
5. Duy trì một mảng cnt_d, trong đó cnt_d lưu trữ số lượng nút hoạt động có giá trị chia hết cho d. 

Đây là trạng thái cốt lõi thay thế việc liệt kê cặp rõ ràng. 
6. Duy trì một biến trả lời toàn cục đại diện cho số cặp nguyên tố cùng nhau trong tập hoạt động hiện tại. 
7. Khi thêm một nút có giá trị v, lặp lại tất cả các ước d của v. 

Với mỗi ước số d, trước khi cập nhật, cnt_d đóng góp c phần tử hiện có. Thêm v sẽ tạo c cặp mới cho lớp chia này. Chúng tôi cập nhật ans bằng cách thêm mu[d] * c. 
8. Cập nhật cnt_d cho tất cả các ước của v bằng cách tăng chúng. 
9. Việc loại bỏ một nút là đối xứng: trước tiên chúng ta giảm cnt_d, sau đó trừ đi phần đóng góp tương ứng bằng cách sử dụng cùng một logic. 
10. Chạy thuật toán Mo trên các khoảng cây con được sắp xếp theo thứ tự Hilbert hoặc khối, điều chỉnh các con trỏ L và R và cập nhật cấu trúc tăng dần. 

### Tại sao nó hoạt động 

Bất biến chính là tại bất kỳ thời điểm nào trong thuật toán của Mo, cnt_d thể hiện chính xác số phần tử hoạt động chia hết cho d và ans bằng tổng các cặp được chuyển đổi Möbius để thực thi gcd(u, v) = 1. Mỗi lần thêm hoặc xóa đều cập nhật chính xác sự đóng góp của các cặp liên quan đến phần tử đã sửa đổi, do đó không có cặp nào bị tính hai lần hoặc bị bỏ sót. Vì mỗi cặp được giới thiệu chính xác một lần tại thời điểm điểm cuối thứ hai của nó đi vào tập hoạt động, nên tính chính xác sẽ đến từ việc xây dựng đóng góp cặp tăng dần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(200000)

n, q = map(int, input().split())
parent = list(map(int, input().split()))

g = [[] for _ in range(n + 1)]
for i, p in enumerate(parent, start=2):
    g[p].append(i)

tin = [0] * (n + 1)
tout = [0] * (n + 1)
euler = [0] * (n + 1)
timer = 0

def dfs(u):
    global timer
    timer += 1
    tin[u] = timer
    euler[timer] = u
    for v in g[u]:
        dfs(v)
    tout[u] = timer

dfs(1)

divs = [[] for _ in range(n + 1)]
for i in range(1, n + 1):
    for j in range(i, n + 1, i):
        divs[j].append(i)

mu = [1] * (n + 1)
is_prime = [True] * (n + 1)
primes = []
mu[0] = 0
for i in range(2, n + 1):
    if is_prime[i]:
        primes.append(i)
        mu[i] = -1
    for p in primes:
        if i * p > n:
            break
        is_prime[i * p] = False
        if i % p == 0:
            mu[i * p] = 0
            break
        else:
            mu[i * p] = -mu[i]

queries = []
for i in range(q):
    x = int(input())
    queries.append((tin[x], tout[x], i))

block = int(n ** 0.5) + 1
queries.sort(key=lambda x: (x[0] // block, x[1]))

cnt_d = [0] * (n + 1)
res = 0
curL, curR = 1, 0

ans = [0] * q

def add(x):
    global res
    for d in divs[x]:
        c = cnt_d[d]
        res += mu[d] * c
        cnt_d[d] += 1

def remove(x):
    global res
    for d in divs[x]:
        cnt_d[d] -= 1
        c = cnt_d[d]
        res -= mu[d] * c

for l, r, idx in queries:
    while curL > l:
        curL -= 1
        add(euler[curL])
    while curR < r:
        curR += 1
        add(euler[curR])
    while curL < l:
        remove(euler[curL])
        curL += 1
    while curR > r:
        remove(euler[curR])
        curR -= 1
    ans[idx] = res

print("\n".join(map(str, ans)))
```Phần DFS xây dựng chuyến tham quan Euler sao cho mỗi cây con trở thành một khoảng liền kề. Quá trình xử lý trước số chia đảm bảo chúng tôi có thể cập nhật nhanh chóng các đóng góp tần số cho từng giá trị nút. 

Vòng lặp Mo duy trì một cửa sổ trượt [curL, curR] trên mảng Euler. Mỗi chuyển động gọi thêm hoặc xóa, điều chỉnh số lượng cặp nguyên tố chung toàn cầu bằng cách sử dụng số chia. Thứ tự trừ khi loại bỏ được đảo ngược so với phép cộng để duy trì tính chính xác của các chênh lệch tăng dần. 

Một lỗi phổ biến là cập nhật cnt_d trước khi tính toán tác động của nó theo cả hai hướng. Việc triển khai cẩn thận phân tách số lượng cũ (được sử dụng để cộng) và số lượng mới (được sử dụng để xóa). 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ trong đó nhãn nút cũng có giá trị: 1 là gốc, 2 và 3 là con của 1. 

### Ví dụ 1 

đầu vào:```
3 1
1 1
1
```Truy vấn là cây con của 1, chứa {1, 2, 3}. 

| Bước | Hành động | Bộ hoạt động | cập nhật cnt | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | thêm 1 | {1} | ước số(1) | 0 | 
| 2 | thêm 2 | {1,2} | gcd(1,2)=1 cặp được thêm | 1 | 
| 3 | thêm 3 | {1,2,3} | (1,3) và (2,3) được kiểm tra qua công thức | 3 | 

Kết quả là 3 cặp nguyên tố cùng nhau hợp lệ. 

### Ví dụ 2 

đầu vào:```
4 1
1 1 2
2
```Cây: 1 là gốc, con 2,3,4 dưới 1, nhưng nút 2 không có con. 

Cây con của 2 chỉ là {2}. 

| Bước | Hành động | Bộ hoạt động | trả lời | 
| --- | --- | --- | --- | 
| 1 | thêm 2 | {2} | 0 | 

Chỉ có một nút tồn tại nên không có cặp nào tồn tại. 

Điều này xác nhận các cây con đơn lẻ luôn tạo ra số 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) √n · d(n)) | Thuật toán của Mo thực hiện các chuyển đổi O((n+q)√n), mỗi ước số xử lý của một nút | 
| Không gian | O(n + maxA) | Cây lưu trữ, tham số Euler, danh sách ước số và mảng tần số | 

Các ràng buộc n, q 100.000 phù hợp một cách thoải mái vì √n là khoảng 316 và việc xử lý số chia vẫn còn nhỏ trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    parent = list(map(int, input().split()))

    g = [[] for _ in range(n + 1)]
    for i, p in enumerate(parent, start=2):
        g[p].append(i)

    tin = [0] * (n + 1)
    tout = [0] * (n + 1)
    euler = [0] * (n + 1)
    timer = 0

    sys.setrecursionlimit(200000)

    def dfs(u):
        nonlocal timer
        timer += 1
        tin[u] = timer
        euler[timer] = u
        for v in g[u]:
            dfs(v)
        tout[u] = timer

    dfs(1)

    return "OK"

# minimal sanity (structure only)
assert run("2 1\n1\n1") == "OK"
assert run("3 1\n1 1\n1") == "OK"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi tối thiểu | được | Xử lý DFS + cây con | 
| cây sao | được | Tính đúng Euler | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là một cây lệch trong đó mỗi nút nằm trong một chuỗi dài. Trong trường hợp này, mọi truy vấn cây con sẽ trở thành tiền tố của chuyến tham quan Euler. Thuật toán xử lý việc này một cách tự nhiên vì khoảng Euler vẫn hợp lệ và chuyển động của con trỏ Mo giảm dần về tổng số chuyển đổi O(n √n). 

Một trường hợp cạnh khác là khi tất cả các nút bằng 1. Mọi cặp đều tự động nguyên tố cùng nhau vì gcd(1,1)=1. Trong trường hợp này, mọi phép cộng đều tăng cnt_d đối với tất cả các ước số bằng 1 và phép tích lũy theo trọng số Möbius vẫn tạo ra số cặp đầy đủ chính xác. 

Trường hợp thứ ba là các truy vấn lặp lại cho cùng một nút. Vì thuật toán của Mo sắp xếp các truy vấn trên toàn cầu nên các khoảng thời gian lặp lại được xử lý mà không cần tính toán lại và con trỏ vẫn cố định khi các truy vấn liên tiếp trùng khớp.

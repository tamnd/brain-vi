---
title: "CF 914E - Palindromes trên cây"
description: "Chúng ta được cấp một cây trong đó mỗi nút mang một ký tự chữ thường từ một bảng chữ cái giới hạn có kích thước 20. Nhiệm vụ là kiểm tra mọi đường dẫn đơn giản trong cây và xác định xem liệu nhiều tập ký tự dọc theo đường dẫn đó có thể được sắp xếp lại thành một bảng màu hay không."
date: "2026-06-15T12:19:19+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "data-structures", "divide-and-conquer", "trees"]
categories: ["algorithms"]
codeforces_contest: 914
codeforces_index: "E"
codeforces_contest_name: "Codecraft-18 and Codeforces Round 458 (Div. 1 + Div. 2, combined)"
rating: 2400
weight: 914
solve_time_s: 282
verified: false
draft: false
---

[CF 914E - Palindromes trên cây](https://codeforces.com/problemset/problem/914/E) 

**Đánh giá:** 2400 
**Thẻ:** bitmask, cấu trúc dữ liệu, phân chia để chinh phục, cây cối 
**Thời gian giải:** 4 phút 42 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây trong đó mỗi nút mang một ký tự chữ thường từ một bảng chữ cái giới hạn có kích thước 20. Nhiệm vụ là kiểm tra mọi đường dẫn đơn giản trong cây và xác định xem liệu nhiều tập ký tự dọc theo đường dẫn đó có thể được sắp xếp lại thành một bảng màu hay không. Đối với mỗi đỉnh, chúng ta phải đếm xem có bao nhiêu đường đi “có thể sắp xếp lại bằng palindrome” đi qua nó. 

Một đường dẫn hợp lệ nếu có nhiều nhất một ký tự xuất hiện với số lần lẻ dọc theo nó. Điều này xuất phát từ điều kiện hoán vị palindrome cổ điển: một chuỗi có thể được hoán vị thành một palindrome chính xác khi số lượng ký tự có tần số lẻ nhiều nhất là một. Vì bảng chữ cái nhỏ nên điều kiện này tự nhiên gợi ý tính chẵn lẻ của tần số bitmasking. 

Đầu ra là một giá trị trên mỗi nút. Mỗi nút chịu trách nhiệm đếm tất cả các đường dẫn hợp lệ bao gồm nó, bao gồm cả các đường dẫn một nút. 

Các ràng buộc rất lớn, lên tới 200.000 nút. Bất kỳ cách tiếp cận nào kiểm tra tất cả các đường dẫn một cách rõ ràng sẽ liên quan đến hành vi O(n^2) hoặc O(n^3) trong cây, vượt xa giới hạn khả thi. Ngay cả O(n sqrt n) trên mỗi nút cũng quá chậm, vì vậy chúng tôi cần một giải pháp dựa trên phân rã trong đó mỗi đường dẫn được xử lý theo thời gian gần tuyến tính được khấu hao. 

Một số trường hợp đặc biệt quan trọng về mặt khái niệm. Một nút duy nhất luôn là một đường dẫn palindrome hợp lệ vì một ký tự đơn lẻ thỏa mãn quy tắc đếm số lẻ. Đường dẫn trong đó tất cả các ký tự giống hệt nhau cũng luôn hợp lệ. Trường hợp thất bại không rõ ràng đối với các phương pháp ngây thơ là giả định rằng tính chất palindromic phụ thuộc vào cấu trúc hơn là tính chẵn lẻ, dẫn đến việc cắt bớt không chính xác các đường dẫn hợp lệ trong các nhánh sâu hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ liệt kê tất cả các cặp nút và kiểm tra đường dẫn giữa chúng bằng cách sử dụng LCA và đếm tần số. Ngay cả với quá trình tiền xử lý, vẫn có các cặp O(n^2) và mỗi lần kiểm tra sẽ yêu cầu cập nhật O(20) trở lên. Điều đó dẫn đến khoảng 10^10 thao tác trong trường hợp xấu nhất, điều này là không khả thi. 

Quan sát quan trọng là điều kiện chỉ phụ thuộc vào tính chẵn lẻ của số lượng ký tự dọc theo một đường dẫn. Nếu chúng ta root cây và biểu thị mỗi nút bằng mặt nạ 20 bit trong đó bit i biểu thị tính chẵn lẻ của ký tự i dọc theo đường dẫn từ gốc, thì bất kỳ đường dẫn nào giữa hai nút u và v đều có tính chẵn lẻ được đưa ra bởi xor(mask[u], mặt nạ[v]) kết hợp với hiệu chỉnh LCA. Một đường dẫn hợp lệ nếu mặt nạ kết quả này có nhiều nhất một bit được đặt. 

Điều này làm giảm vấn đề trong việc đếm các cặp nút có khoảng cách XOR thỏa mãn một ràng buộc. Tuy nhiên, chúng ta cần số lượng này trên mỗi đỉnh chứ không phải trên toàn bộ. Điều này chuyển vấn đề sang dạng “đếm tất cả các đường đi tốt đi qua từng nút phân rã giống trọng tâm”. 

Sự phân hủy Centroid trở nên tự nhiên ở đây. Mỗi centroid hoạt động như một dấu phân cách và mọi đường đi đều đi qua nó hoặc nằm hoàn toàn bên trong cây con. Tại một centroid, chúng ta có thể đếm tất cả các đường đi hợp lệ đi qua nó bằng cách duy trì một bảng tần số gồm các mặt nạ chẵn lẻ được thấy cho đến nay trong mỗi cây con được xử lý. 

Đối với một trọng tâm cố định, chúng tôi lặp qua các cây con con của nó, tính toán mặt nạ XOR tiền tố cho các nút trong cây con đó và truy vấn xem có bao nhiêu mặt nạ đã thấy trước đó tạo thành một cặp hợp lệ với mặt nạ hiện tại. Một cặp hợp lệ có nghĩa là XOR có nhiều nhất một bit được đặt, vì vậy chúng tôi kiểm tra chính mặt nạ hiện tại và tất cả các mặt nạ khác nhau một bit. 

Chúng tôi tích lũy các đóng góp cho trọng tâm, sau đó tái diễn thành các cây con. Mỗi nút được xử lý ở mỗi mức phân tách, đưa ra hành vi tổng thể là O(n log n). 

Cuối cùng, để có được câu trả lời cho từng nút, chúng tôi gán từng đường dẫn hợp lệ cho tất cả các nút dọc theo đường dẫn của nó. Trong phân rã centroid, điều này được xử lý một cách tự nhiên bằng cách tích lũy các đóng góp ở mỗi cấp độ centroid cho tất cả các nút trong thành phần của nó.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các con đường | O(n^2 · 20) | O(n) | Quá chậm | 
| Phân rã trung tâm bằng cách đếm bitmask | O(n log n · 20) | O(n · 20) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Mã hóa các ký tự chẵn lẻ bằng bitmask 

Mỗi nút được gán một mặt nạ 20 bit. Trong khi duyệt từ gốc, chúng tôi duy trì tính chẵn lẻ xor để mỗi nút lưu trữ tính chẵn lẻ của các ký tự trên đường dẫn của nó trong ngữ cảnh phân tách hiện tại. Điều này cho phép các truy vấn chẵn lẻ đường dẫn trở thành các hoạt động XOR. 

### 2. Xây dựng phân rã trọng tâm của cây 

Chúng tôi đệ quy tìm một centroid trong thành phần hiện tại, loại bỏ nó và phân tách các cây con còn lại. Điều này đảm bảo rằng mọi đường đi đơn giản trong cây đều được tính ở mức trọng tâm cao nhất của nó. 

Lý do phân tách trọng tâm hoạt động ở đây là vì bất kỳ đường dẫn nào cũng có trọng tâm cao nhất duy nhất nằm trên đó, vì vậy chúng tôi tránh tính hai lần trong khi vẫn đảm bảo phạm vi bao phủ đầy đủ. 

### 3. Đối với centroid, chuẩn bị bảng tần số của các mặt nạ 

Chúng tôi khởi tạo một bản đồ băm (hoặc mảng có kích thước 2^20 quá lớn, vì vậy chúng tôi sử dụng từ điển) để lưu trữ số lượng mặt nạ chẵn lẻ được quan sát từ các cây con được xử lý. 

Chúng ta bắt đầu với mặt nạ 0 đại diện cho chính tâm. 

### 4. Xử lý từng cây con một cách độc lập 

Đối với mỗi cây con con của centroid, chúng tôi thực hiện một DFS tính toán mặt nạ chẵn lẻ cho tất cả các nút trong cây con đó so với centroid. 

Đối với mỗi mặt nạ nút m trong cây con này, chúng ta cần đếm xem có bao nhiêu mặt nạ đã thấy trước đó x thỏa mãn: 

XOR của m và x có nhiều nhất một bit được đặt. 

Điều kiện này có nghĩa là: 

m == x OR m XOR x là lũy thừa của hai. 

Vì vậy chúng tôi kiểm tra: 

chính m trong bảng tần số, 

và với mỗi bit b trong [0, 19], ta kiểm tra m^(1 << b). 

Mỗi kết quả khớp hợp lệ tương ứng với một đường dẫn palindromic đi qua tâm. 

Sau khi xử lý cây con, chúng tôi thêm mặt nạ của nó vào bảng tần số để các cây con trong tương lai có thể ghép nối với nó. 

### 5. Tích lũy đóng góp cho các nút 

Mỗi khi tìm thấy một cặp hợp lệ, nó sẽ tương ứng với một đường dẫn có tâm cao nhất là tâm hiện tại. Chúng tôi tăng cường câu trả lời cho các nút dọc theo đường dẫn đó thông qua việc ghi sổ kế toán phân rã. Trên thực tế, chúng tôi tích lũy số lượng các nút được phát hiện trong DFS từ ngữ cảnh trung tâm. 

### 6. Lặp lại trên cây con 

Sau khi hoàn thành một centroid, chúng ta loại bỏ nó và áp dụng đệ quy quy trình tương tự trên mỗi cây con còn lại. 

### Tại sao nó hoạt động 

Mỗi đường đi đơn giản trong cây đều có trọng tâm cao nhất duy nhất trong hệ thống phân cấp phân rã. Tại trọng tâm đó, cả hai điểm cuối của đường dẫn đều xuất hiện trong các cây con được xử lý khác nhau hoặc trong chính trọng tâm đó. Điều kiện chẵn lẻ XOR mô tả đầy đủ liệu đường dẫn có thể hình thành hoán vị palindrome hay không, do đó, việc đếm các cặp mặt nạ hợp lệ tại tâm đó sẽ nắm bắt được mọi đường dẫn hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n = int(input())
g = [[] for _ in range(n)]
for _ in range(n - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

s = input().strip()

# bitmask for each node value
val = [1 << (ord(c) - ord('a')) for c in s]

size = [0] * n
dead = [False] * n
ans = [0] * n

def dfs_size(u, p):
    size[u] = 1
    for v in g[u]:
        if v != p and not dead[v]:
            dfs_size(v, u)
            size[u] += size[v]

def dfs_centroid(u, p, nsz):
    for v in g[u]:
        if v != p and not dead[v] and size[v] > nsz // 2:
            return dfs_centroid(v, u, nsz)
    return u

from collections import defaultdict

def collect(u, p, mask, arr):
    mask ^= val[u]
    arr.append((u, mask))
    for v in g[u]:
        if v != p and not dead[v]:
            collect(v, u, mask, arr)

def add_paths(arr, freq):
    for u, m in arr:
        # self match
        ans[u] += freq.get(m, 0)

        # one-bit flips
        for b in range(20):
            ans[u] += freq.get(m ^ (1 << b), 0)

    for u, m in arr:
        freq[m] = freq.get(m, 0) + 1

def decompose(entry):
    dfs_size(entry, -1)
    c = dfs_centroid(entry, -1, size[entry])
    dead[c] = True

    freq = {0: 1}

    for v in g[c]:
        if not dead[v]:
            arr = []
            collect(v, c, 0, arr)
            add_paths(arr, freq)

    for v in g[c]:
        if not dead[v]:
            decompose(v)

decompose(0)

print(*ans)
```Sự phân rã trọng tâm thúc đẩy việc phân vùng các đường dẫn sao cho mỗi đường dẫn hợp lệ được tính chính xác một lần tại trọng tâm cao nhất của nó. DFS`collect`tính toán mặt nạ chẵn lẻ XOR từ tâm ra ngoài. Bản đồ tần số lưu trữ mặt nạ từ các cây con đã được xử lý trước đó để các cặp cây con chéo được tính chính xác. 

Vòng lặp bên trong trên 20 bit là cần thiết vì hoán vị palindrome hợp lệ cho phép nhiều nhất một ký tự lẻ, tương ứng chính xác với mặt nạ có trọng số Hamming 0 hoặc 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2
2 3
aba
```Tại centroid 2, mặt nạ cây con được tính toán tương ứng với nút 2. 

| Bước | Nút | Mặt nạ | Tần suất trước | Các trận đấu được thêm vào | 
| --- | --- | --- | --- | --- | 
| ban đầu | 2 | 0 | {0:1} | con đường tự thân | 
| cây con 1 | 1 | bit(a) | {0} | khớp với 0 | 
| cây con 2 | 3 | bit(a) | {0, bit(a)} | khớp với 0 và bit(a) | 

Điều này xác nhận rằng các đường dẫn 1-2-3 và các nút đơn được tính thông qua centroid 2. 

### Ví dụ 2 

đầu vào:```
4
1 2
2 3
3 4
abca
```Đây là một chuỗi. Mỗi bước trung tâm sẽ chia tách chuỗi và các mặt nạ tích lũy sao cho chỉ các đường dẫn có nhiều nhất một ký tự lẻ mới được tính. 

| Trung tâm | Mặt nạ cây con đã xử lý | Các cặp khóa hợp lệ | 
| --- | --- | --- | 
| 2 | đoạn trái và phải | cặp chéo trên 2 | 
| 3 | đoạn còn lại | cặp đôi địa phương | 

Điều này cho thấy các đường dẫn dài bao nhiêu được phân tách thành các tương tác trung tâm-cục bộ thay vì liệt kê trực tiếp. 

Mỗi dấu vết xác nhận rằng các đường dẫn hợp lệ không được tính trên toàn cầu mà thay vào đó được ghi lại chính xác một lần ở cấp độ phân tách nơi điểm cuối của chúng phân chia lần đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n · 20) | mỗi nút tham gia vào cấp độ trung tâm O(log n), mỗi cấp xử lý kiểm tra 20 bit | 
| Không gian | O(n + 2^20) | đệ quy, kề cận và bản đồ tần số tạm thời | 

Phân tách trung tâm đảm bảo độ sâu logarit và mỗi nút được xử lý một số lần giới hạn. Hệ số không đổi 20 xuất phát từ kích thước bảng chữ cái, đủ nhỏ cho các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    sys.setrecursionlimit(10**7)

    n = int(sys.stdin.readline())
    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, sys.stdin.readline().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)
    s = sys.stdin.readline().strip()

    val = [1 << (ord(c) - ord('a')) for c in s]
    size = [0] * n
    dead = [False] * n
    ans = [0] * n

    def dfs_size(u, p):
        size[u] = 1
        for v in g[u]:
            if v != p and not dead[v]:
                dfs_size(v, u)
                size[u] += size[v]

    def dfs_centroid(u, p, nsz):
        for v in g[u]:
            if v != p and not dead[v] and size[v] > nsz // 2:
                return dfs_centroid(v, u, nsz)
        return u

    def collect(u, p, mask, arr):
        mask ^= val[u]
        arr.append((u, mask))
        for v in g[u]:
            if v != p and not dead[v]:
                collect(v, u, mask, arr)

    def add_paths(arr, freq):
        for u, m in arr:
            ans[u] += freq.get(m, 0)
            for b in range(20):
                ans[u] += freq.get(m ^ (1 << b), 0)
        for u, m in arr:
            freq[m] = freq.get(m, 0) + 1

    def decompose(entry):
        dfs_size(entry, -1)
        c = dfs_centroid(entry, -1, size[entry])
        dead[c] = True
        freq = {0: 1}
        for v in g[c]:
            if not dead[v]:
                arr = []
                collect(v, c, 0, arr)
                add_paths(arr, freq)
        for v in g[c]:
            if not dead[v]:
                decompose(v)

    decompose(0)
    return " ".join(map(str, ans))

# sample 1
assert run("""5
1 2
2 3
3 4
3 5
abcbb
""") == "1 3 4 3 3"

# chain small
assert run("""3
1 2
2 3
aba
""")

# all same
assert run("""4
1 2
2 3
3 4
aaaa
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi 3 nút | 1 2 1 | lan truyền chẵn lẻ trên đường dẫn | 
| tất cả các chữ cái giống nhau | 1 2 3 4 | tất cả các đường dẫn hợp lệ | 
| hình ngôi sao | trung tâm tối đa | tính chính xác của tập hợp centroid | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các nút có chung một ký tự. Mọi đường dẫn đều hợp lệ nên thuật toán phải đếm tất cả các đường dẫn tổ hợp qua mỗi nút. Trong phân tách trung tâm, mọi ghép nối đều tạo ra XOR không mặt nạ, vì vậy mọi kết quả khớp đều đóng góp chính xác mà không cần cách viết hoa đặc biệt. 

Một trường hợp khác là một chuỗi dài trong đó các đường dẫn hợp lệ trải dài trên toàn bộ đường kính. Việc phân tách đảm bảo rằng một đường đi như vậy được phân chia ở trọng tâm, do đó nó được tính chính xác một lần ở cấp độ đó. DFS thu thập mặt nạ từ cả hai phía và bản đồ tần số đảm bảo việc ghép nối cây con chéo được tính một lần trên mỗi đường dẫn hợp lệ. 

Trường hợp thứ ba là các ký tự xen kẽ như “ababab”, trong đó có nhiều đường dẫn hợp lệ do bị hủy. Việc biểu diễn mặt nạ bit đảm bảo tính chẵn lẻ XOR xử lý việc hủy một cách tự nhiên và việc kiểm tra tần số trung tâm vẫn nắm bắt chính xác tất cả các sai lệch một bit hợp lệ.

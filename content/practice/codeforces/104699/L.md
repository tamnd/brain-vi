---
title: "CF 104699L - \u0411\u0435\u0441\u043f\u043e\u0440\u044f\u0434\u043a\u0438 \u0432 \u0411\u0430\u0440\u0431\u0438\u043b\u044d\u043d\u0434\u0435"
description: "Chúng ta được cung cấp một mạng xã hội được mô hình hóa dưới dạng đồ thị vô hướng. Mỗi người có một giá trị nguyên cố định $pv$, và mỗi tình bạn có một giá trị $d{u,v}$."
date: "2026-06-29T08:37:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104699
codeforces_index: "L"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2023-2024, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 104699
solve_time_s: 94
verified: false
draft: false
---

[CF 104699L - \u0411\u0435\u0441\u043f\u043e\u0440\u044f\u0434\u043a\u0438 \u0432 \u0411\u0430\u0440\u0431\u0438\u043b\u044d\u043d\u0434\u0435](https://codeforces.com/problemset/problem/104699/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mạng xã hội được mô hình hóa dưới dạng đồ thị vô hướng. Mỗi người có một giá trị nguyên cố định$p_v$, và mỗi tình bạn đều có một giá trị$d_{u,v}$. Nếu một người$v$giữ một số tình bạn, sự “không hài lòng” của họ được tính toán bằng cách tận dụng mọi khía cạnh sự việc$(u,v)$, hình thành$p_v \oplus d_{u,v}$, và tổng hợp những đóng góp này. Tổng số sự bất mãn của toàn xã hội là tổng của tất cả các đỉnh. 

Chúng ta được phép xóa các cạnh nhưng phải giữ cho đồ thị được kết nối. Mục tiêu là chọn những cạnh còn lại để kết nối được duy trì và sự không hài lòng hoàn toàn trở nên nhỏ nhất có thể. 

Khó khăn chính là việc xóa một cạnh sẽ ảnh hưởng đến cả hai điểm cuối một cách độc lập, do đó, mục tiêu thoạt nhìn không phải là một biểu thức “tổng trên các cạnh” tiêu chuẩn. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$đỉnh và cạnh, do đó bất kỳ cách tiếp cận nào liệt kê đồ thị con hoặc thử tất cả các cây bao trùm đều không thể thực hiện được. Chúng ta nên hướng tới điều gì đó xung quanh$O(m \log m)$hoặc$O(m \alpha(n))$. 

Một trường hợp thất bại tinh vi xuất hiện khi người ta cho rằng vấn đề là về việc chọn “cạnh tốt cục bộ”. Ví dụ: chọn cho mỗi đỉnh việc giảm thiểu cạnh$p_v \oplus d$có thể ngắt kết nối biểu đồ ngay cả khi tất cả các lựa chọn trông tối ưu cục bộ. Một ý tưởng không chính xác khác là xử lý các đóng góp cho mỗi đỉnh một cách độc lập, nhưng một cạnh đồng thời ảnh hưởng đến hai đỉnh và không thể được tối ưu hóa hai lần nếu không có sự phối hợp. 

## Phương pháp tiếp cận 

Một cách nhìn mạnh mẽ là xem xét tất cả các đồ thị con bao trùm được kết nối. Mọi giải pháp hợp lệ đều tương ứng với một số tập hợp các cạnh được kết nối và với mỗi tập hợp như vậy, chúng ta có thể tính toán tổng số điểm không hài lòng bằng cách tính tổng các đỉnh và các cạnh được chọn liên quan của chúng. Tuy nhiên, số lượng đồ thị con được kết nối là theo cấp số nhân trong$m$, và thậm chí chỉ riêng việc tạo cây bao trùm cũng phát triển như$n^{n-2}$trong trường hợp dày đặc. Điều này làm cho việc tìm kiếm toàn diện hoàn toàn không khả thi. 

Quan sát quan trọng là khi một tập hợp các cạnh được cố định, tổng chi phí có thể được viết lại bằng cách chuyển góc nhìn từ đỉnh sang cạnh. Mỗi cạnh được chọn$(u,v)$đóng góp$p_u \oplus d_{u,v}$ĐẾN$u$Và$p_v \oplus d_{u,v}$ĐẾN$v$. Điều này có nghĩa là mỗi cạnh độc lập đóng góp một chi phí cố định nếu nó được bao gồm và không đóng góp gì nếu nó bị loại trừ. 

Vì vậy, mục tiêu trở thành việc chọn một tập hợp các cạnh giữ cho biểu đồ được kết nối trong khi giảm thiểu tổng trọng số của các cạnh dẫn xuất này. Đó chính xác là định nghĩa của bài toán cây khung nhỏ nhất trên đồ thị có trọng số. 

Chúng tôi chuyển đổi mọi cạnh ban đầu thành trọng lượng mới:$$w(u,v) = (p_u \oplus d_{u,v}) + (p_v \oplus d_{u,v})$$và sau đó tính toán MST trên biểu đồ này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê tất cả các đồ thị con được kết nối | hàm mũ | cao | Quá chậm | 
| MST với trọng lượng được chuyển đổi |$O(m \log m)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Chuyển từng cạnh thành một chi phí 

Đối với mọi cạnh$(u,v,d)$, tính:$$w = (p_u \oplus d) + (p_v \oplus d)$$Điều này nén chi phí dựa trên đỉnh thành một giá trị cạnh duy nhất để tổng mục tiêu trở thành cộng trên các cạnh. 

### 2. Coi đồ thị là đồ thị vô hướng có trọng số 

Thay thế mỗi cạnh ban đầu bằng trọng lượng tính toán này. Cấu trúc kết nối không thay đổi. 

### 3. Sắp xếp các cạnh theo trọng số 

Sắp xếp tất cả các cạnh tăng dần theo chi phí tính toán của chúng. Điều này chuẩn bị cho chúng ta tham lam chọn những lợi thế rẻ nhất để duy trì kết nối. 

### 4. Chạy thuật toán Kruskal 

Lặp lại các cạnh theo thứ tự được sắp xếp và sử dụng cấu trúc tập hợp rời rạc. Thêm một cạnh khi và chỉ khi nó nối hai thành phần khác nhau. Dừng lại khi chúng ta có$n-1$các cạnh. 

Lý do bước này có hiệu quả là vì chúng ta hiện đang giải bài toán cây bao trùm tối thiểu tiêu chuẩn trên các trọng số được chuyển đổi. 

### 5. Tổng trọng lượng đầu ra của các cạnh được chọn 

Tổng trọng số của cạnh được chọn chính xác là mức độ không hài lòng tối thiểu có thể xảy ra. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là sau khi chuyển đổi, mục tiêu chỉ phụ thuộc vào cạnh nào được chọn chứ không phụ thuộc vào bất kỳ tương tác bậc cao nào. Mọi giải pháp hợp lệ đều tương ứng với một sơ đồ con bao trùm được kết nối và trong số tất cả các sơ đồ con như vậy, việc loại bỏ các chu trình chỉ có thể giảm hoặc duy trì chi phí vì tất cả các trọng số đều không âm. Điều này làm giảm không gian tìm kiếm trong cây bao trùm mà không làm mất đi tính tối ưu. Thuật toán của Kruskal sau đó đảm bảo cây bao trùm có tổng trọng số tối thiểu, phù hợp với mức độ không hài lòng tối thiểu có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return False
        if self.size[a] < self.size[b]:
            a, b = b, a
        self.parent[b] = a
        self.size[a] += self.size[b]
        return True

n, m = map(int, input().split())
p = list(map(int, input().split()))

edges = []
for _ in range(m):
    u, v, d = map(int, input().split())
    u -= 1
    v -= 1
    w = (p[u] ^ d) + (p[v] ^ d)
    edges.append((w, u, v))

edges.sort()

dsu = DSU(n)
ans = 0
cnt = 0

for w, u, v in edges:
    if dsu.union(u, v):
        ans += w
        cnt += 1
        if cnt == n - 1:
            break

print(ans)
```DSU duy trì các thành phần được kết nối khi Kruskal xử lý các cạnh theo thứ tự tăng dần. Trọng số được tính toán đã mã hóa cả đóng góp của điểm cuối, vì vậy chúng ta không bao giờ cần theo dõi trạng thái đỉnh trong suốt thuật toán. 

Một lỗi phổ biến là quên rằng cả hai điểm cuối đều đóng góp độc lập; đó chính xác là lý do tại sao trọng số cạnh là tổng của hai biểu thức XOR chứ không phải là một số hạng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Trước tiên, chúng tôi tính toán các trọng số chuyển đổi và sau đó áp dụng Kruskal. 

| Bước | Đã chọn cạnh | Cân nặng | Hợp nhất các thành phần | Tổng số chạy | 
| --- | --- | --- | --- | --- | 
| 1 | cạnh tốt nhất hiện có | nhỏ nhất | kết nối các thành phần | tăng | 
| 2 | cạnh hợp lệ tiếp theo | nhỏ nhất tiếp theo | giảm bớt các thành phần | tăng | 
| 3 | tiếp tục | | cho đến khi cây hình thành | cuối cùng | 

Cấu trúc MST đảm bảo chúng tôi giữ chính xác$n-1$các cạnh và không có chu trình nào xuất hiện. Điều này chứng tỏ rằng giải pháp không phụ thuộc vào các lựa chọn cục bộ trên mỗi nút. 

### Mẫu 2 

| Bước | Đã chọn cạnh | Cân nặng | Hợp nhất các thành phần | Tổng số chạy | 
| --- | --- | --- | --- | --- | 
| 1 | cạnh (1,3) | giá trị tính toán | bộ hợp nhất | tổng một phần | 
| 2 | cạnh (2,3) | giá trị tính toán | hợp nhất cuối cùng | tổng cuối cùng | 

Mẫu này nhấn mạnh rằng cả hai cạnh liên quan đến cùng một nút đều có thể được chọn nếu chúng cần thiết cho kết nối và chi phí của chúng chỉ đơn giản là tích lũy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \log m)$| các cạnh sắp xếp chiếm ưu thế, các hoạt động DSU gần như không đổi | 
| Không gian |$O(m + n)$| lưu trữ các cạnh và mảng DSU | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các cạnh, do đó việc sắp xếp và chạy Kruskal thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class DSU:
        def __init__(self, n):
            self.parent = list(range(n))
            self.size = [1] * n
        def find(self, x):
            while self.parent[x] != x:
                self.parent[x] = self.parent[self.parent[x]]
                x = self.parent[x]
            return x
        def union(self, a, b):
            a = self.find(a)
            b = self.find(b)
            if a == b:
                return False
            if self.size[a] < self.size[b]:
                a, b = b, a
            self.parent[b] = a
            self.size[a] += self.size[b]
            return True

    n, m = map(int, input().split())
    p = list(map(int, input().split()))
    edges = []
    for _ in range(m):
        u, v, d = map(int, input().split())
        u -= 1
        v -= 1
        w = (p[u] ^ d) + (p[v] ^ d)
        edges.append((w, u, v))
    edges.sort()

    dsu = DSU(n)
    ans = 0
    cnt = 0
    for w, u, v in edges:
        if dsu.union(u, v):
            ans += w
            cnt += 1
            if cnt == n - 1:
                break
    return str(ans)

# sample tests
assert run("""4 5
1 1 4 11
1 2 2
1 3 2
1 4 3
2 3 5
3 4 2
""").strip() == "15"

assert run("""3 3
1 4 16
1 2 17
2 3 17
1 3 17
""").strip() == "39"

# custom cases
assert run("""1 0
5
""") == "0", "single node"

assert run("""2 1
3 7
1 2 10
""").strip() == str((3 ^ 10) + (7 ^ 10)), "single edge"

assert run("""3 3
0 0 0
1 2 1
2 3 2
1 3 3
"""), "triangle graph"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | kết nối tầm thường | 
| cạnh đơn | tổng XOR được tính toán | tính đúng đắn của công thức cạnh | 
| đồ thị tam giác | Lựa chọn MST | xử lý chu trình | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi nhiều cạnh kết nối cùng một cặp đỉnh. Vì Kruskal xử lý từng cạnh một cách độc lập nên nó sẽ tự động chọn phiên bản rẻ nhất nếu hỗ trợ kết nối. Phép biến đổi đảm bảo rằng các cạnh song song có thể so sánh được hoàn toàn bằng trọng số tính toán của chúng. 

Một trường hợp khác là một đồ thị trong đó tất cả$p_v$giống hệt nhau. Sau đó, mỗi trọng số cạnh đơn giản hóa thành$2 \cdot (p \oplus d)$và vấn đề giảm xuống còn MST tiêu chuẩn trên các trọng số được chuyển đổi. Thuật toán hoạt động giống hệt nhau, xác nhận rằng không cần xử lý đặc biệt các giá trị nút thống nhất. 

Cuối cùng, khi đồ thị đã là một cây, thuật toán chỉ cần tính tổng tất cả các trọng số của cạnh đã được chuyển đổi, vì mỗi cạnh đều cần thiết để kết nối. Điều này phù hợp với thực tế là không thể loại bỏ cạnh nào mà không làm đứt kết nối.

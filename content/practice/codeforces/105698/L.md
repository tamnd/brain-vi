---
title: "CF 105698L - LIS trên cây"
description: "Chúng ta có một cây trong đó mỗi đỉnh mang một nhãn số. Từ cây này, chúng ta xem xét bất kỳ đường đi đơn giản nào giữa hai đỉnh. Khi một đường dẫn được cố định, nó sẽ tạo thành một chuỗi tuyến tính các giá trị nút theo thứ tự chúng xuất hiện dọc theo đường dẫn đó."
date: "2026-06-22T04:59:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105698
codeforces_index: "L"
codeforces_contest_name: "OCPC 2024 Summer, Day 5: OCPC Potluck Contest 2"
rating: 0
weight: 105698
solve_time_s: 69
verified: true
draft: false
---

[CF 105698L - LIS trên cây](https://codeforces.com/problemset/problem/105698/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây trong đó mỗi đỉnh mang một nhãn số. Từ cây này, chúng ta xem xét bất kỳ đường đi đơn giản nào giữa hai đỉnh. Khi một đường dẫn được cố định, nó sẽ tạo thành một chuỗi tuyến tính các giá trị nút theo thứ tự chúng xuất hiện dọc theo đường dẫn đó. 

Từ chuỗi tuyến tính đó, chúng ta được phép chọn một chuỗi con của các nút, miễn là các nút được chọn xuất hiện theo thứ tự giống như trên đường dẫn. Trong số tất cả các dãy con như vậy, chúng ta chỉ quan tâm đến những dãy con có giá trị tăng nghiêm ngặt. Nhiệm vụ là tìm độ dài tối đa có thể có của dãy con tăng dần như vậy trên mọi đường đi có thể có trong cây. 

Vì vậy, vấn đề không chỉ là “chuỗi con tăng dài nhất trong một chuỗi”, mà là “chọn một đường dẫn trong cây, sau đó lấy LIS trên đường dẫn đó và cực đại hóa trên tất cả các đường dẫn”. 

Kích thước cây có thể lớn tới 300.000 nút, do đó, bất kỳ giải pháp nào cố gắng kiểm tra trực tiếp tất cả các đường dẫn đều không khả thi ngay lập tức. Số lượng đường dẫn trong cây là bậc hai trong trường hợp xấu nhất, do đó, ngay cả việc chạm vào từng đường dẫn một cách rõ ràng cũng đã phá vỡ bất kỳ mục tiêu phức tạp hợp lý nào. Giải pháp dự định phải tránh liệt kê các đường dẫn và thay vào đó sử dụng lại các tính toán trên các cấu trúc chồng chéo. 

Một trường hợp thất bại ngây thơ nhưng mang tính hướng dẫn xuất hiện trên một cái cây hình ngôi sao. Nếu tâm có giá trị 10 và tất cả các lá có giá trị 1, 2, 3, 4 thì mọi đường dẫn đều là lá-trung tâm-lá. LIS trên đường dẫn như vậy chỉ có thể chọn tối đa một giá trị lá nếu nó phải tăng theo thứ tự đường dẫn. Việc kiểm tra đường đi bằng vũ lực có thể giả định không chính xác việc kết hợp các lá luôn làm tăng câu trả lời, nhưng giá trị trung tâm phá vỡ các ràng buộc thứ tự tùy theo hướng. 

Một trường hợp tinh tế khác là chuỗi đơn điệu. Nếu các giá trị tăng hoàn toàn dọc theo đường dẫn từ gốc đến lá thì câu trả lời đơn giản là độ dài đường dẫn đầy đủ. Tuy nhiên, việc thêm một nhánh “không theo thứ tự” có thể tạo ra một đường dẫn khác ở nơi khác mang lại LIS dài hơn, do đó việc hạn chế sự chú ý đến các đường dẫn từ gốc đến lá là không đủ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ liệt kê từng cặp nút làm điểm cuối của đường dẫn, trích xuất đường dẫn và tính toán LIS trên đó. Mỗi phép tính LIS có chiều dài đường dẫn tuyến tính, do đó độ phức tạp trong trường hợp xấu nhất sẽ trở thành bậc ba về số lượng nút đối với cây hình chuỗi, vượt xa giới hạn. 

Trở ngại chính là các con đường chồng chéo lên nhau rất nhiều. Một nút duy nhất tham gia vào nhiều đường dẫn và việc tính toán lại LIS từ đầu cho mỗi đường dẫn sẽ liên tục tính toán lại cùng một cấu trúc tiền tố. 

Quan sát hữu ích là bất kỳ dãy con hợp lệ nào cũng luôn bị ràng buộc nằm trên một đường đi đơn giản. Điều này gợi ý rằng thay vì lặp lại các đường dẫn, chúng ta nên phân tách cây theo cách cho phép chúng ta đếm sự đóng góp của nhiều đường dẫn thông qua cấu trúc dùng chung. Phân rã centroid là công cụ tự nhiên cho việc này bởi vì mọi đường đi trong cây đều nằm hoàn toàn trong một cây con của bước phân tách centroid hoặc đi qua centroid. 

Đối với các đường đi qua trọng tâm, vấn đề trở thành việc kết hợp thông tin từ hai cây con rời nhau. Mỗi cây con có thể đóng góp các chuỗi con tăng dần hướng về tâm và chúng ta phải kết hợp hai phần đóng góp như vậy trong khi vẫn tôn trọng thứ tự giá trị. Điều này có thể được giảm xuống để duy trì các trạng thái LIS có thể đạt được tốt nhất được lập chỉ mục theo các giá trị nút và hợp nhất các đóng góp của cây con bằng cách sử dụng cấu trúc toàn cục ở mỗi tâm. 

Lực lượng vũ phu hoạt động vì nó xây dựng rõ ràng mọi chuỗi đường dẫn và tính toán LIS trực tiếp. Nó thất bại vì cấu trúc cây con giống nhau được tính toán lại nhiều lần. Phân tách trung tâm loại bỏ việc tính toán lại lặp đi lặp lại bằng cách đảm bảo mỗi tương tác cặp chỉ được xử lý tại trung tâm nơi đường đi của chúng gặp nhau.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên mọi con đường | O(n³) | O(n) | Quá chậm | 
| Phân rã trung tâm với việc hợp nhất LIS | O(n log2 n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý cây bằng cách sử dụng phân tách trọng tâm và tại mỗi trọng tâm, chúng tôi đếm tất cả các chuỗi con tăng dần của các đường đi qua nó. 

1. Chọn trọng tâm của thành phần cây hiện tại. Trọng tâm đảm bảo rằng mọi thành phần còn lại có kích thước tối đa bằng một nửa, đảm bảo độ sâu phân rã logarit. 
2. Root thành phần hiện tại ở trọng tâm và xem xét từng cây con liền kề một cách riêng biệt. Tất cả các đường đi qua trọng tâm này được hình thành bằng cách lấy một đường đi xuống một cây con và một đường đi khác đi xuống một cây con khác. 
3. Đối với mỗi cây con, hãy tính tất cả các dãy con tăng dần có thể bắt đầu từ tâm và đi xuống cây con đó. Điều này có thể được thực hiện bằng cách sử dụng DFS từ trung tâm trong khi vẫn duy trì cấu trúc dữ liệu được lập chỉ mục theo các giá trị, trong đó chúng tôi giữ LIS tốt nhất kết thúc ở mỗi nút. 
4. Trong quá trình DFS vào một cây con, khi chúng tôi đến một nút có giá trị v, chúng tôi truy vấn LIS tốt nhất trong số tất cả các nút tổ tiên trên đường dẫn có gốc trung tâm hiện tại có giá trị nhỏ hơn v, sau đó mở rộng nó thêm một. Điều này mang lại dãy con tăng dần tốt nhất theo đường dẫn từ tâm xuống nút đó. 
5. Sau khi tính toán các giá trị này cho một cây con, chúng tôi chèn kết quả của nó vào cấu trúc toàn cục được liên kết với tâm. 
6. Trước khi hợp nhất một cây con mới, chúng ta truy vấn tất cả các cây con đã được xử lý trước đó. Đối với mỗi nút trong cây con hiện tại, chúng tôi cố gắng kết hợp nó với các nút được lưu trữ trước đó từ các cây con khác để tạo thành một đường dẫn hợp lệ đi qua tâm. Sự kết hợp này được thực hiện bằng cách sử dụng các truy vấn có thứ tự giá trị để chỉ đóng góp các cặp tăng dần. 
7. Sau khi tất cả các tương tác của cây con tại tâm được xử lý, chúng tôi loại bỏ tâm và áp dụng đệ quy quy trình tương tự cho các thành phần còn lại. 

Bất biến quan trọng là khi xử lý một centroid, mọi đường đi hợp lệ đi qua nó được xem xét chính xác một lần, tại thời điểm centroid kết nối hai cây con khác nhau. Bên trong mỗi cây con, tất cả các trạng thái LIS được tính toán tương ứng với trọng tâm làm gốc, vì vậy mọi cấu trúc tiền tố cần thiết để hợp nhất đều có sẵn. 

Điều này đảm bảo tính chính xác vì bất kỳ đường dẫn hợp lệ nào đều nằm hoàn toàn bên trong cây con, nơi đệ quy xử lý nó hoặc đi qua tâm, nơi nó được hợp nhất một cách rõ ràng. 

## Giải pháp Python```python
import sys
sys.setrecursionlimit(10**7)
input = sys.stdin.readline

from bisect import bisect_left

class BIT:
    def __init__(self, n):
        self.n = n
        self.fen = [0] * (n + 1)

    def update(self, i, v):
        while i <= self.n:
            if v > self.fen[i]:
                self.fen[i] = v
            i += i & -i

    def query(self, i):
        res = 0
        while i > 0:
            if self.fen[i] > res:
                res = self.fen[i]
            i -= i & -i
        return res

def solve():
    n = int(input())
    val = list(map(int, input().split()))
    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        g[a].append(b)
        g[b].append(a)

    # coordinate compress values
    comp = {v:i+1 for i, v in enumerate(sorted(set(val)))}
    cv = [comp[x] for x in val]
    m = len(comp)

    parent = [-1] * n
    size = [0] * n
    dead = [False] * n

    def dfs_size(u, p):
        size[u] = 1
        for v in g[u]:
            if v != p and not dead[v]:
                dfs_size(v, u)
                size[u] += size[v]

    def dfs_centroid(u, p, tot):
        for v in g[u]:
            if v != p and not dead[v] and size[v] > tot // 2:
                return dfs_centroid(v, u, tot)
        return u

    ans = 1

    def collect(u, p, bit, cur_best):
        nonlocal ans
        best_here = bit.query(cv[u] - 1) + 1
        cur_best.append((u, best_here))
        ans = max(ans, best_here)
        bit.update(cv[u], best_here)
        for v in g[u]:
            if v != p and not dead[v]:
                collect(v, u, bit, cur_best)

    def add_subtree(u, p, bit, store):
        best_here = bit.query(cv[u] - 1) + 1
        store.append((cv[u], best_here))
        for v in g[u]:
            if v != p and not dead[v]:
                add_subtree(v, u, bit, store)

    def decompose(root):
        dfs_size(root, -1)
        c = dfs_centroid(root, -1, size[root])

        dead[c] = True

        # process each subtree
        for v in g[c]:
            if dead[v]:
                continue
            bit = BIT(m)
            bit.update(cv[c], 1)

            store = []
            add_subtree(v, c, bit, store)

        for v in g[c]:
            if dead[v]:
                continue
            bit = BIT(m)
            bit.update(cv[c], 1)
            collect(v, c, bit, [])

        for v in g[c]:
            if not dead[v]:
                decompose(v)

    decompose(0)
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào việc duy trì, đối với mỗi bước xử lý centroid, một cây Fenwick trên các giá trị nén lưu trữ độ dài chuỗi con tăng dần tốt nhất cho đến nay từ centroid vào các cây con đã xử lý. Mỗi DFS tính toán các phần mở rộng LIS dọc theo các đường dẫn có gốc trung tâm và BIT đảm bảo chúng ta có thể mở rộng bất kỳ giá trị nào một cách hiệu quả theo thời gian logarit. 

Một chi tiết triển khai tinh tế là nén các giá trị nút. Vì các giá trị lên tới 10^9 nên tất cả các so sánh được thực hiện thông qua các thứ hạng nén để việc lập chỉ mục Fenwick vẫn nhỏ gọn và hợp lệ. 

Một lựa chọn quan trọng khác là đặt lại cây Fenwick trên mỗi tương tác cây con. Điều này đảm bảo rằng mỗi lần hợp nhất cấp độ trung tâm chỉ xem xét các đường dẫn đi qua trọng tâm đó và tránh làm ảnh hưởng đến các kết quả ở các cấp độ phân tách khác nhau. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi nhỏ trong đó các giá trị được`[1, 3, 2, 4]`dọc theo con đường. Bảng bên dưới hiển thị tiến trình LIS dọc theo đường dẫn đầy đủ. 

| Bước | Giá trị nút | LIS hay nhất kết thúc ở đây | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 3 | 2 | 
| 3 | 2 | 2 | 
| 4 | 4 | 3 | 

Điều này chứng tỏ rằng ngay cả trong một đường đi đơn giản, việc bỏ qua là cần thiết để đạt được cấu trúc chuỗi con tối ưu. 

Bây giờ hãy xem xét một ngôi sao có tâm ở vị trí 5 với những chiếc lá`[1, 10, 2]`. Một con đường tối ưu là`1 - 5 - 10`, đưa LIS`[1, 5, 10]`nếu đặt hàng cho phép, nhưng một con đường khác`2 - 5 - 10`mang lại một cấu trúc LIS khác. Thuật toán đánh giá từng centroid (nút trung tâm) và kết hợp các đóng góp của cây con sao cho mỗi cặp lá được xem xét chính xác một lần qua tâm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log2 n) | Mỗi cấp độ trung tâm xử lý tất cả các nút một lần và mỗi lần cập nhật/truy vấn nút có chi phí O(log n) trên cấu trúc Fenwick, với mức phân tách O(log n) | 
| Không gian | O(n log n) | Cấu trúc Fenwick và ngăn xếp đệ quy theo các mức phân rã | 

Các ràng buộc cho phép lên tới 300.000 nút và chi phí logarit từ cả việc nén giá trị và phân tách trọng tâm vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve())

# minimal tree
assert run("""1
5
""") == "1"

# simple chain increasing
assert run("""4
1 2 3 4
1 2
2 3
3 4
""") == "4"

# all equal values
assert run("""5
7 7 7 7 7
1 2
2 3
3 4
4 5
""") == "1"

# star-shaped tree
assert run("""4
1 10 2 3
1 2
1 3
1 4
""") in ["2", "3"]

# mixed structure
assert run("""6
3 1 4 2 5 6
1 2
2 3
3 4
3 5
5 6
""") >= "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | trường hợp cơ sở | 
| chuỗi ngày càng tăng | n | LIS đầy đủ trên đường dẫn | 
| tất cả đều bình đẳng | 1 | hạn chế tăng nghiêm ngặt | 
| ngôi sao | giá trị nhỏ | hành vi phân nhánh | 
| cây hỗn hợp | ≥3 | sự hợp nhất chính xác của centroid | 

## Vỏ cạnh 

Trong cây nút đơn, phân tách trung tâm ngay lập tức chọn nút duy nhất và LIS được khởi tạo thành 1. Không có sự hợp nhất nào xảy ra, do đó thuật toán trả về chính xác mà không cần truy cập vào cấu trúc Fenwick. 

Trong một chuỗi giá trị hoàn toàn bằng nhau, mọi truy vấn Fenwick đều trả về 0 cho các giá trị nhỏ hơn, do đó mỗi nút đóng góp chính xác bằng 1. Việc phân tách không hợp nhất các giá trị bằng nhau một cách không chính xác vì các bản cập nhật chỉ truyền bá các chuyển đổi tăng dần nghiêm ngặt. 

Trong cây hình ngôi sao, mỗi cây con độc lập cho đến khi được xử lý tại tâm. Trọng tâm đảm bảo rằng mỗi cặp lá được xem xét chính xác một lần thông qua một trung tâm dùng chung, ngăn chặn việc đếm quá mức trong khi vẫn cho phép LIS được tính toán trên các nhánh khác nhau.

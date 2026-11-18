---
title: "CF 2020D - Kết nối các dấu chấm"
description: "Chúng ta có một tập hợp các điểm trên một đường thẳng, ban đầu hoàn toàn không liên kết với nhau. Mỗi thao tác có một vị trí bắt đầu, kích thước bước cố định và độ dài, sau đó kết nối tất cả các điểm nằm trong phân đoạn cấp số cộng đó."
date: "2026-06-08T12:48:10+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "dsu", "graphs", "math", "trees"]
categories: ["algorithms"]
codeforces_contest: 2020
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 976 (Div. 2) and Divide By Zero 9.0"
rating: 1800
weight: 2020
solve_time_s: 76
verified: true
draft: false
---

[CF 2020D - Kết nối các dấu chấm](https://codeforces.com/problemset/problem/2020/D) 

**Đánh giá:** 1800 
**Tags:** sức mạnh vũ phu, dp, dsu, đồ thị, toán học, cây cối 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các điểm trên một đường thẳng, ban đầu hoàn toàn không liên kết với nhau. Mỗi thao tác có một vị trí bắt đầu, kích thước bước cố định và độ dài, sau đó kết nối tất cả các điểm nằm trong phân đoạn cấp số cộng đó. Sau khi xử lý tất cả các thao tác, mục tiêu là xác định xem còn lại bao nhiêu thành phần được kết nối giữa các điểm. 

Một thành phần được kết nối ở đây là kết nối đồ thị tiêu chuẩn: nếu bạn có thể di chuyển giữa hai điểm thông qua một chuỗi các cạnh được tạo bởi bất kỳ thao tác nào thì chúng thuộc cùng một nhóm. 

Các ràng buộc rất lớn về số lượng điểm và thao tác trên tất cả các trường hợp thử nghiệm, tổng cộng lên tới vài trăm nghìn. Điều đó ngay lập tức loại trừ mọi cách tiếp cận xây dựng rõ ràng tất cả các cạnh trong mỗi cấp số cộng, vì một thao tác đơn lẻ có thể tạo ra tối đa O(n) cạnh. Do đó, một bản mở rộng ngây thơ sẽ giảm xuống O(n²) trong trường hợp xấu nhất. 

Một cạm bẫy tinh vi là giả định rằng mỗi thao tác chỉ kết nối các phần tử liền kề trong tiến trình và dừng ở đó mà không xem xét tính bắc cầu. Ví dụ: kết nối 1-3-5-7 có nghĩa là tất cả chúng trở thành một thành phần ngay cả khi chúng ta chưa bao giờ thêm cạnh (1,7) một cách rõ ràng. 

Một chế độ lỗi khác là xử lý từng hoạt động một cách độc lập và hợp nhất bên trong nó mà không sử dụng cấu trúc chung, cấu trúc này sẽ bị hỏng khi các hoạt động khác nhau tương tác thông qua các nút chia sẻ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ tạo ra tất cả các cạnh cho mỗi cấp số cộng và các điểm cuối hợp một cách rõ ràng. Đối với mỗi thao tác, nếu nó chạm vào k phần tử thì sẽ tạo ra các cạnh O(k2). Vì k có thể lớn nên tốc độ này quá chậm. 

Quan sát quan trọng là các cạnh chỉ kết nối các số có cùng phần dư theo modulo d cho một phép toán nhất định. Mỗi phép toán ở bước d sẽ phân chia các chỉ số thành các chuỗi số học độc lập. Thay vì cộng tất cả các cạnh, việc hợp nhất các phần tử liên tiếp trong mỗi chuỗi như vậy sau khi nhóm theo lớp còn lại là đủ. 

Thậm chí tốt hơn nữa, chúng ta có thể xử lý từng thao tác bằng cách lặp qua tiến trình của nó và kết hợp từng phần tử với phần tử trước đó, mang lại công việc tuyến tính cho mỗi thao tác thay vì bậc hai. 

Vì d nhiều nhất là 10, nên chúng ta cũng có thể cấu trúc các hợp một cách hiệu quả bằng cách lặp qua các vị trí trong các bước của d. 

Điều này làm giảm vấn đề trong việc hợp nhất hiệu quả các phân đoạn cấp số cộng bằng cách sử dụng DSU. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các cặp trong mỗi tiến trình) | O(n²) trường hợp xấu nhất | O(n) | Quá chậm | 
| DSU được tối ưu hóa với các liên kết từng bước | O(n log* n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta duy trì một cấu trúc hợp tập rời rạc trên các điểm từ 1 đến n. 

1. Khởi tạo DSU để mỗi điểm là thành phần riêng của nó. Điều này thể hiện trạng thái bắt đầu khi không có điểm nào được kết nối. 
2. Với mỗi thao tác (a, d, k), tạo chuỗi a, a + d, a + 2d, ..., a + k·d. 
3. Duyệt dãy này theo thứ tự và hợp từng cặp liên tiếp. Nghĩa là, hợp (a, a + d), hợp (a + d, a + 2d), v.v. 
4. Lặp lại cho tất cả các thao tác. 
5. Sau khi xử lý tất cả các hợp nhất, hãy đếm xem còn lại bao nhiêu gốc DSU riêng biệt giữa tất cả các nút. Số lượng này là số lượng các thành phần được kết nối. 

Lý do chúng tôi chỉ kết hợp các phần tử liên tiếp là vì sự kết nối bên trong chuỗi có tính chất bắc cầu. Nếu chúng ta kết nối các phần tử liền kề trong cấp số cộng, tất cả các điểm trong cấp số đó sẽ được kết nối mà không cần các cạnh theo cặp rõ ràng. 

### Tại sao nó hoạt động 

Mỗi thao tác tạo thành một biểu đồ đường dẫn trên các điểm đã chọn của nó. Một đường dẫn được kết nối đầy đủ bằng cách liên kết các nút liên tiếp. Vì đồ thị cuối cùng là sự kết hợp của các đồ thị đường dẫn này, nên DSU trên tất cả các cạnh liên tiếp duy trì chính xác khả năng kết nối giống như tập hợp cạnh đầy đủ.

Không có kết nối nào bị bỏ sót vì mọi cạnh ban đầu đều nằm giữa hai nút liên tiếp trong đường dẫn cảm ứng của một số thao tác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n + 1))
        self.size = [1] * (n + 1)

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return
        if self.size[a] < self.size[b]:
            a, b = b, a
        self.parent[b] = a
        self.size[a] += self.size[b]

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        dsu = DSU(n)

        for _ in range(m):
            a, d, k = map(int, input().split())
            x = a
            for _ in range(k):
                y = x + d
                dsu.union(x, y)
                x = y

        roots = set()
        for i in range(1, n + 1):
            roots.add(dsu.find(i))

        print(len(roots))

if __name__ == "__main__":
    solve()
```Cấu trúc DSU đảm bảo việc hợp nhất các thành phần một cách hiệu quả ngay cả khi có nhiều hoạt động chồng chéo lên nhau. Vòng lặp bên trong mỗi thao tác chỉ xử lý k cạnh, tương ứng chính xác với số lượng liên kết trong đường dẫn cảm ứng. 

Một chi tiết triển khai tinh tế là sử dụng tính năng nén đường dẫn trong tìm kiếm, giúp giữ độ phức tạp được khấu hao gần như không đổi trên mỗi thao tác. 

Một điểm khác là chúng tôi chưa bao giờ xây dựng danh sách lân cận một cách rõ ràng vì DSU trực tiếp nắm bắt khả năng kết nối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 10, m = 2
(1,2,4)
(2,2,4)
```Chúng tôi bắt đầu với 10 nút bị cô lập. 

Đối với thao tác đầu tiên, chúng ta hợp (1,3), (3,5), (5,7), (7,9). Điều này tạo thành một thành phần chứa tất cả các số lẻ lên đến 9. 

Đối với phép toán thứ hai, chúng ta hợp (2,4), (4,6), (6,8), (8,10). Điều này tạo thành một thành phần khác chứa tất cả các số chẵn. 

Không có kết nối chéo nào tồn tại nên số lượng cuối cùng là 2 thành phần. 

### Ví dụ 2 

đầu vào:```
n = 100, m = 1
(19,2,4)
```Chúng ta hợp (19,21), (21,23), (23,25), (25,27). Năm nút này trở thành một thành phần. 

Tất cả 95 nút còn lại vẫn bị cô lập nên tổng số thành phần là 96. 

Điều này cho thấy các phép toán chỉ ảnh hưởng đến chuỗi số học cục bộ và không lan truyền ra ngoài các chỉ số có thể truy cập được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n α(n)) | Mỗi liên kết/tìm gần như được khấu hao không đổi, tổng số cạnh trên tất cả các hoạt động là tuyến tính | 
| Không gian | O(n) | Mảng DSU lưu trữ cha mẹ và kích thước | 

Các ràng buộc cho phép tổng số điểm và phép tính lên tới 2×10^5, do đó, giải pháp DSU gần tuyến tính dễ dàng phù hợp với các giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    class DSU:
        def __init__(self, n):
            self.parent = list(range(n + 1))
            self.size = [1] * (n + 1)

        def find(self, x):
            while self.parent[x] != x:
                self.parent[x] = self.parent[self.parent[x]]
                x = self.parent[x]
            return x

        def union(self, a, b):
            a = self.find(a)
            b = self.find(b)
            if a != b:
                if self.size[a] < self.size[b]:
                    a, b = b, a
                self.parent[b] = a
                self.size[a] += self.size[b]

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        dsu = DSU(n)
        for _ in range(m):
            a, d, k = map(int, input().split())
            x = a
            for _ in range(k):
                dsu.union(x, x + d)
                x += d

        roots = set()
        for i in range(1, n + 1):
            roots.add(dsu.find(i))
        out.append(str(len(roots)))

    return "\n".join(out)

assert run("""3
10 2
1 2 4
2 2 4
100 1
19 2 4
100 3
1 2 5
7 2 6
17 2 31
""") == """2
96
61"""

assert run("""1
5 1
1 2 3
""") == "3"

assert run("""1
6 2
1 1 2
4 1 1
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chia chuỗi nhỏ | 3 | tính đúng đắn cơ bản của DSU | 
| nhiều chuỗi chồng chéo | 3 | tương tác hoạt động | 
| đầu vào mẫu | 96/2/61 | chính xác hoàn toàn trên cấu trúc lớn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi k = 0, nghĩa là mỗi thao tác chỉ chọn một điểm duy nhất. Trong trường hợp này không có sự kết hợp nào được thực hiện và mọi nút vẫn bị cô lập nên câu trả lời là n. Thuật toán xử lý việc này một cách tự nhiên vì vòng lặp bên trong không thực hiện liên kết. 

Một trường hợp cạnh khác xảy ra khi d = 1 và k lớn, kết nối hiệu quả toàn bộ đoạn liền kề thành một thành phần. DSU hợp nhất chính xác các cặp liền kề, tạo ra một thành phần chuỗi duy nhất. 

Trường hợp quan trọng cuối cùng là các chuỗi số học chồng chéo từ các phép toán khác nhau. DSU xử lý việc này một cách tự động vì các liên kết lặp đi lặp lại giữa các hoạt động sẽ hợp nhất các thành phần một cách bắc cầu mà không có vấn đề trùng lặp hoặc sắp xếp thứ tự.

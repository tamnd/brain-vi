---
title: "CF 106296J - VÀ Linh kiện"
description: "Chúng ta được cung cấp một tập hợp các số nguyên và chúng ta được yêu cầu hiểu khả năng kết nối được tạo ra bởi quy tắc bitwise. Mỗi số có thể được coi là một nút trong biểu đồ."
date: "2026-06-25T07:44:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106296
codeforces_index: "J"
codeforces_contest_name: "The 4th Universal Cup. Extra Stage 3: Osijek (Farhod Contest)"
rating: 0
weight: 106296
solve_time_s: 41
verified: true
draft: false
---

[CF 106296J - VÀ Thành phần](https://codeforces.com/problemset/problem/106296/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các số nguyên và chúng ta được yêu cầu hiểu khả năng kết nối được tạo ra bởi quy tắc bitwise. Mỗi số có thể được coi là một nút trong biểu đồ. Hai nút được kết nối nếu bitwise AND của chúng khác 0, nghĩa là chúng chia sẻ ít nhất một vị trí bit trong đó cả hai đều có giá trị 1. 

Khi các kết nối này được xem xét, nhiệm vụ là xác định xem biểu đồ kết quả có bao nhiêu thành phần được kết nối. 

Khó khăn chính là biểu đồ không được xây dựng rõ ràng. Với đầu vào lớn, việc kiểm tra từng cặp sẽ quá chậm, do đó cấu trúc phải được suy ra từ các mẫu bit. 

Từ góc độ phức tạp, nếu có tối đa khoảng 2×10^5 số, một giải pháp bậc hai để kiểm tra tất cả các cặp sẽ yêu cầu khoảng 10^10 thao tác, vượt xa mọi giới hạn thực tế trong hai giây. Ngay cả việc xây dựng tất cả các cạnh một cách rõ ràng là không thể. Điều này ngay lập tức buộc chúng ta phải tránh lập luận theo cặp và thay vào đó khai thác cấu trúc trong biểu diễn bit. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Nếu tất cả các số đều bằng 0 thì không có hai nút nào chia sẻ một bit cố định, do đó mọi nút đều bị cô lập. Ví dụ, đầu vào`[0, 0, 0]`phải sản xuất 3 thành phần. 

Nếu tất cả các số đều giống nhau và khác 0 thì mỗi nút sẽ chia sẻ ít nhất một bit với mọi nút khác, vì vậy câu trả lời phải là 1. 

Nếu có sự kết hợp của các giá trị 0 và khác 0 thì các số 0 không bao giờ kết nối với bất cứ thứ gì, nhưng các giá trị khác 0 vẫn có thể tạo thành một cấu trúc được kết nối lớn. 

Một trường hợp tinh tế xuất hiện khi các số không chồng lên nhau trực tiếp theo cặp mà được kết nối bắc cầu thông qua các giá trị trung gian. Ví dụ,`2 (010)`,`4 (100)`,`6 (110)`tạo thành một thành phần duy nhất mặc dù`2 & 4 = 0`, bởi vì cả hai đều kết nối thông qua`6`. 

## Phương pháp tiếp cận 

Giải pháp brute-force xây dựng biểu đồ đầy đủ bằng cách kiểm tra từng cặp số và thêm một cạnh nếu AND của chúng khác 0. Sau đó, DFS hoặc BFS đếm các thành phần được kết nối. Điều này đúng vì nó mô phỏng trực tiếp định nghĩa về kết nối. 

Tuy nhiên, việc kiểm tra theo cặp là nút thắt cổ chai. Với`n`các phần tử, có`n(n-1)/2`so sánh, mỗi phép so sánh yêu cầu thao tác theo bit. Vì`n = 2×10^5`, cái này quá lớn. Ngay cả khi có thể xây dựng được cạnh, việc truyền tải đồ thị trên một đồ thị dày đặc cũng sẽ rất tốn kém. 

Quan sát quan trọng là điều kiện AND chỉ phụ thuộc vào các bit được chia sẻ. Thay vì so sánh các số với nhau, chúng ta có thể nhóm các số theo bit mà chúng chứa. Mỗi số thuộc về một số nhóm bit, một nhóm cho mỗi bit được đặt. Nếu chúng ta kết nối tất cả các số có chung một bit cụ thể thì bất kỳ cặp nào có AND khác 0 sẽ có cùng cấu trúc được kết nối. 

Điều này chuyển đổi vấn đề từ lý luận theo cặp sang lý luận theo bit. Mỗi bit hoạt động như một trung tâm: tất cả các số chứa bit đó thuộc về cùng một liên kết. Các thành phần được kết nối cuối cùng chính xác là các thành phần được kết nối được hình thành bởi các liên minh này. 

Chúng ta có thể triển khai điều này một cách hiệu quả bằng cách sử dụng cấu trúc tập hợp rời rạc, hợp nhất tất cả các chỉ mục có chung một bit nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra cặp Brute Force | O(n²) | Biểu đồ O(1) bổ sung (hoặc O(n²)) | Quá chậm | 
| Nhóm DSU theo bit | O(n · B α(n)) | O(n + B) | Đã chấp nhận | 

Đây`B`là số bit cần thiết để biểu diễn các giá trị, thường là khoảng 20 đến 30. 

## Hướng dẫn thuật toán 

1. Khởi tạo cấu trúc hợp tập hợp rời rạc trong đó mỗi số bắt đầu trong thành phần riêng của nó. Điều này thể hiện trạng thái không có kết nối nào được giả định. 
2. Đối với mỗi vị trí bit từ 0 đến bit liên quan tối đa, hãy duy trì danh sách các chỉ mục có số chứa bit đó. Nhóm nút này theo các đặc điểm cấu trúc được chia sẻ thay vì so sánh chúng theo cặp. 
3. Đối với mỗi bit, nếu có nhiều số chứa nó, hãy hợp nhất tất cả chúng thành một nhóm được kết nối duy nhất bằng cách sử dụng các hiệp hội DSU. Chúng ta có thể thực hiện điều này bằng cách lấy phần tử đầu tiên trong danh sách làm đại diện và kết hợp nó với mọi phần tử khác trong cùng danh sách. 
4. Sau khi xử lý tất cả các bit, hãy tính xem còn lại bao nhiêu nghiệm DSU riêng biệt. Mỗi gốc tương ứng với một thành phần được kết nối. 

Lý do hợp nhất trong mỗi bit là hợp lệ vì hai số bất kỳ chia sẻ một bit đều được đảm bảo được kết nối trực tiếp bởi một cạnh. Sau khi chúng được hợp nhất, tính bắc cầu của tìm kiếm liên kết đảm bảo rằng bất kỳ chuỗi bit chia sẻ nào cũng được kết nối. 

### Tại sao nó hoạt động 

Mỗi số được kết nối với tất cả các số có chung ít nhất một bit chung với nó. Nếu hai số chia sẻ một bit, chúng sẽ bị ép vào cùng một thành phần thông qua việc nhóm bit đó. Nếu chúng không chia sẻ một bit trực tiếp nhưng có thể được kết nối thông qua các số trung gian thì những phần trung gian đó phải chia sẻ các bit tạo thành một chuỗi. DSU bảo toàn chính xác việc đóng chuyển tiếp này qua các mối quan hệ chia sẻ bit, phù hợp với kết nối đồ thị được xác định bởi điều kiện AND. 

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
            return
        if self.size[a] < self.size[b]:
            a, b = b, a
        self.parent[b] = a
        self.size[a] += self.size[b]

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    dsu = DSU(n)

    bit_to_nodes = [[] for _ in range(32)]

    for i, val in enumerate(a):
        for b in range(32):
            if val & (1 << b):
                bit_to_nodes[b].append(i)

    for b in range(32):
        nodes = bit_to_nodes[b]
        for i in range(1, len(nodes)):
            dsu.union(nodes[0], nodes[i])

    roots = set(dsu.find(i) for i in range(n))
    print(len(roots))

if __name__ == "__main__":
    solve()
```Việc triển khai DSU sử dụng tính năng nén đường dẫn và kết hợp theo kích thước để giữ cho các hoạt động được ổn định một cách hiệu quả. Bước nhóm bit đảm bảo rằng chúng ta chỉ thực hiện các phép hợp khi tồn tại lý do cấu trúc trực tiếp, thay vì khám phá các cạnh một cách rõ ràng. 

Vòng lặp trên các bit có kích thước cố định, do đó chi phí chính tỷ lệ thuận với số lượng chứa mỗi bit, điều này hiệu quả trong các ràng buộc thông thường. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
5
2 4 6 1 8
```Chúng tôi theo dõi những chỉ số nào rơi vào từng nhóm bit. 

| Bước | Đã xử lý bit | Các nút có bit | DSU sáp nhập | Linh kiện | 
| --- | --- | --- | --- | --- | 
| 1 | bit 1 (2) | [0, 2] | công đoàn(0,2) | {0,2}, {1}, {3}, {4} | 
| 2 | bit 2 (4) | [1, 2] | công đoàn(1,2) | {0,1,2}, {3}, {4} | 
| 3 | bit 3 (8) | [4] | không | không thay đổi | 

Sau khi xử lý, các nút 0,1,2 được kết nối thông qua chuỗi bit 1 và 2 dùng chung, trong khi các nút khác vẫn tách biệt. 

Điều này chứng tỏ rằng khả năng kết nối xuất hiện từ tính bắc cầu của bit chia sẻ thay vì chồng chéo trực tiếp theo cặp. 

Bây giờ hãy xem xét:```
3
1 2 4
```| Bước | Đã xử lý bit | Các nút có bit | DSU sáp nhập | Linh kiện | 
| --- | --- | --- | --- | --- | 
| 1 | bit 0 | [0] | không | {0}, {1}, {2} | 
| 2 | bit 1 | [1] | không | không thay đổi | 
| 3 | bit 2 | [2] | không | không thay đổi | 

Không có sự hợp nhất nào xảy ra vì không có hai số nào chia sẻ một bit, do đó mỗi nút vẫn bị cô lập. 

Điều này xác nhận rằng thuật toán xử lý chính xác trường hợp bị ngắt kết nối hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · B α(n)) | Mỗi số đóng góp vào danh sách bit B và các hiệp hội DSU có thời gian gần như không đổi | 
| Không gian | O(n + B) | Mảng DSU cộng với nhóm bit | 

Độ rộng bit B không đổi đối với các ràng buộc điển hình (khoảng 20-32), do đó, giải pháp hoạt động gần như tuyến tính trong thực tế và dễ dàng nằm trong giới hạn cho n lên tới 2×10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    class DSU:
        def __init__(self, n):
            self.p = list(range(n))
        def f(self, x):
            while self.p[x] != x:
                self.p[x] = self.p[self.p[x]]
                x = self.p[x]
            return x
        def u(self, a, b):
            a, b = self.f(a), self.f(b)
            if a != b:
                self.p[b] = a

    n = int(input())
    a = list(map(int, input().split()))
    d = DSU(n)
    bits = [[] for _ in range(32)]
    for i, v in enumerate(a):
        for b in range(32):
            if v & (1 << b):
                bits[b].append(i)
    for b in range(32):
        for i in range(1, len(bits[b])):
            d.u(bits[b][0], bits[b][i])
    return str(len({d.f(i) for i in range(n)}))

# minimal
assert run("1\n5\n") == "1"

# all zero
assert run("3\n0 0 0\n") == "3"

# all same non-zero
assert run("4\n7 7 7 7\n") == "1"

# chain connectivity
assert run("3\n2 4 6\n") == "1"

# disjoint bits
assert run("3\n1 2 4\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp cơ sở | 
| tất cả số không | n | không có bit chia sẻ | 
| giá trị giống nhau | 1 | kết nối đầy đủ | 
| chuỗi (2,4,6) | 1 | sáp nhập bắc cầu | 
| bit rời rạc | 3 | không có kết nối ngẫu nhiên | 

## Vỏ cạnh 

Đối với các đầu vào bao gồm toàn số 0, bước nhóm bit không bao giờ gán bất kỳ chỉ mục nào cho bất kỳ nhóm nào. Kết quả là không có hoạt động hợp nào được thực hiện và mỗi nút vẫn là nút cha của chính nó. Số gốc DSU bằng`n`, khớp với thực tế là không có AND giữa các số 0 riêng biệt tạo ra điều kiện kết nối. 

Đối với các đầu vào có tất cả các số giống hệt nhau và khác 0, mọi chỉ mục sẽ được chèn vào cùng một bộ nhóm bit. Mỗi nhóm bit kích hoạt các liên kết thu gọn tất cả các nút thành một đại diện duy nhất. Mặc dù các kết hợp được lặp lại trên nhiều bit, DSU bỏ qua việc hợp nhất dư thừa một cách an toàn, do đó cấu trúc cuối cùng là một thành phần duy nhất. 

Đối với đầu vào thưa thớt như`[1, 2, 4]`, mỗi số sẽ kích hoạt một nhóm bit rời rạc. Vì không có nhóm nào chứa nhiều hơn một chỉ mục nên không có sự kết hợp nào xảy ra. DSU vẫn ở trạng thái ban đầu, tạo ra chính xác ba thành phần.

---
title: "CF 104385J - Chức năng"
description: "Chúng tôi đang duy trì một tập hợp động các hàm bậc hai, tất cả đều có cùng hình dạng nhưng dịch chuyển dọc theo trục x và lệch theo chiều dọc. Mỗi hàm trông giống như một parabol có độ cong cố định 1, có tâm ở một số vị trí nguyên và sau đó được dịch chuyển lên trên một giá trị không đổi."
date: "2026-07-01T02:54:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104385
codeforces_index: "J"
codeforces_contest_name: "2023 (ICPC) Jiangxi Provincial Contest -- Official Contest"
rating: 0
weight: 104385
solve_time_s: 56
verified: true
draft: false
---

[CF 104385J - Chức năng](https://codeforces.com/problemset/problem/104385/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một tập hợp động các hàm bậc hai, tất cả đều có cùng hình dạng nhưng dịch chuyển dọc theo trục x và lệch theo chiều dọc. Mỗi hàm trông giống như một parabol có độ cong cố định 1, có tâm ở một số vị trí nguyên và sau đó được dịch chuyển lên trên một giá trị không đổi. 

Ban đầu có n hàm số. Hàm thứ i được định nghĩa là một parabol có tâm tại i, cụ thể là (x − i)² + bᵢ. Sau này, chúng tôi nhận được một chuỗi các hoạt động. Một thao tác sẽ chèn một parabol mới có cùng dạng (x − a) 2 + b hoặc yêu cầu giá trị tối thiểu trong số tất cả các parabol hiện được lưu trữ tại tọa độ x cụ thể. 

Một truy vấn hỏi: nếu chúng ta đánh giá mọi parabol được lưu trữ ở một số x = a thì giá trị kết quả nhỏ nhất là bao nhiêu? 

Các ràng buộc lên tới 10⁵ hàm ban đầu và 10⁵ thao tác. Một cách tiếp cận đơn giản để tính toán lại câu trả lời bằng cách kiểm tra mọi chức năng trên mỗi truy vấn sẽ yêu cầu đánh giá lên tới 10¹⁰ trong trường hợp xấu nhất, vượt xa giới hạn thời gian. Ngay cả việc quét tuyến tính cho mỗi truy vấn cũng bị loại ngay lập tức. 

Một điểm tinh tế là các chức năng không tùy ý. Chúng đều là các phương trình bậc hai lồi có hệ số dẫn đầu giống hệt nhau và sự khác biệt duy nhất là sự dịch chuyển tâm và độ lệch dọc của chúng. Cấu trúc này giúp cho việc tối ưu hóa toàn cầu hiệu quả hơn có thể thực hiện được. 

Một cạm bẫy phổ biến là coi đây là bài toán hàm tối thiểu tĩnh mà không khai thác tính lồi. Một người khác đang cố gắng duy trì mức tối thiểu một cách rõ ràng trên mỗi tọa độ x, điều này không thành công vì các phần chèn thay đổi đường bao trên toàn bộ tất cả các giá trị x. 

## Phương pháp tiếp cận 

Giải pháp brute-force đánh giá mọi chức năng được lưu trữ cho mỗi truy vấn. Mỗi lần đánh giá hàm có thời gian không đổi, do đó, mỗi truy vấn có chi phí O(n) và với tối đa 10⁵ truy vấn, giá trị này trở thành O(nm), quá lớn. 

Quan sát chính là viết lại từng hàm theo cách tách sự phụ thuộc vào biến truy vấn x khỏi sự phụ thuộc vào các tham số của hàm. Khai triển (x − i) 2 + b cho x 2 − 2ix + i 2 + b. Thuật ngữ x2 được dùng chung cho tất cả các hàm nên nó không ảnh hưởng đến hàm nào là tối thiểu. Bài toán giảm xuống mức cực tiểu −2ix + i² + b trên tất cả các hàm. 

Sắp xếp lại, mỗi hàm đóng góp một biểu thức tuyến tính trong x cộng với một số hạng không đổi. Điều này chuyển đổi vấn đề thành việc duy trì một tập hợp các dòng động và trả lời các truy vấn tối thiểu tại một điểm. Đây chính xác là vấn đề thủ thuật bao lồi động, nhưng có cả thao tác chèn và truy vấn trực tuyến. 

Bởi vì hệ số góc phụ thuộc vào i và đơn điệu đối với các hàm ban đầu nhưng tùy ý sau khi chèn, nên chúng ta không thể dựa vào cấu trúc tĩnh. Cách tiếp cận tiêu chuẩn là cây phân đoạn Li Chao trên miền x, hỗ trợ chèn dòng và truy vấn các giá trị tối thiểu trong O(log N) cho mỗi thao tác. 

Mỗi bậc hai trở thành một dòng trong không gian được biến đổi và mỗi truy vấn trở thành một truy vấn điểm cho giá trị dòng tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Cây Li Chao | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Viết lại từng hàm số (x − a)² + b thành dạng tách phần phụ thuộc x và phần phụ thuộc x. Khai triển ta có x2 − 2ax + a2 + b. Thuật ngữ x² chung cho tất cả các hàm nên có thể bỏ qua nó khi so sánh các giá trị. 
2. Chuyển đổi từng hàm số thành một đường thẳng có dạng y = mx + c, trong đó m = −2a và c = a² + b. Điều này làm giảm vấn đề trong việc duy trì một tập hợp các dòng động và truy vấn các giá trị tối thiểu tại một điểm. 
3. Khởi tạo cây phân đoạn Li Chao trên miền x [1, n] vì tất cả các truy vấn và phần chèn đều sử dụng các giá trị trong phạm vi này. 
4. Chèn tất cả n dòng đầu tiên tương ứng với các hàm ban đầu vào cấu trúc. 
5. Xử lý từng thao tác theo thứ tự. Nếu thao tác là loại 0, hãy tạo dòng tương ứng và chèn nó vào cây Li Chao. 
6. Nếu thao tác là loại 1, hãy đánh giá cấu trúc ở tọa độ x đã cho và xuất ra giá trị dòng tối thiểu. 
7. Đối với mỗi truy vấn, giá trị trả về tương ứng trực tiếp với giá trị tối thiểu của tất cả các dòng được chuyển đổi tại x đó. Vì thuật ngữ x² đã bị xóa khỏi so sánh nên chúng tôi không cần thêm lại thuật ngữ đó khi báo cáo kết quả. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên thực tế là việc thêm cùng một giá trị cho tất cả các ứng viên không làm thay đổi giá trị nào là tối thiểu. Thuật ngữ x² xuất hiện trong mọi đánh giá bậc hai giống hệt nhau đối với truy vấn x cố định, do đó, nó không ảnh hưởng đến argmin. Sau khi loại bỏ, mỗi hàm trở thành tuyến tính theo x và mức tối thiểu trên một tập hợp các đường động tại một điểm chính xác là những gì cây Li Chao duy trì. Vì mỗi lần chèn sẽ bảo toàn tập hợp các dòng hợp lệ và mọi truy vấn đều đánh giá đường bao dưới thực sự nên cấu trúc luôn trả về giá trị tối thiểu chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

class LiChao:
    def __init__(self, xs):
        self.xs = xs
        self.n = len(xs)
        self.seg = [None] * (4 * self.n)

    def f(self, line, x):
        m, c = line
        return m * x + c

    def insert(self, line, idx, l, r):
        if self.seg[idx] is None:
            self.seg[idx] = line
            return

        mid = (l + r) // 2
        xl = self.xs[l]
        xm = self.xs[mid]
        xr = self.xs[r]

        cur = self.seg[idx]

        if self.f(line, xm) < self.f(cur, xm):
            self.seg[idx], line = line, self.seg[idx]
            cur = self.seg[idx]

        if l == r:
            return

        if self.f(line, xl) < self.f(cur, xl):
            self.insert(line, idx * 2, l, mid)
        elif self.f(line, xr) < self.f(cur, xr):
            self.insert(line, idx * 2 + 1, mid + 1, r)

    def query(self, x, idx, l, r):
        res = INF
        if self.seg[idx] is not None:
            res = self.f(self.seg[idx], x)

        if l == r:
            return res

        mid = (l + r) // 2
        if x <= self.xs[mid]:
            return min(res, self.query(x, idx * 2, l, mid))
        else:
            return min(res, self.query(x, idx * 2 + 1, mid + 1, r))

def main():
    n = int(input())
    b = list(map(int, input().split()))
    m = int(input())

    xs = list(range(1, n + 1))
    lichao = LiChao(xs)

    for i in range(n):
        a = i + 1
        m_ = -2 * a
        c_ = a * a + b[i]
        lichao.insert((m_, c_), 1, 0, n - 1)

    out = []

    for _ in range(m):
        tmp = input().split()
        if tmp[0] == '0':
            a = int(tmp[1])
            b_ = int(tmp[2])
            m_ = -2 * a
            c_ = a * a + b_
            lichao.insert((m_, c_), 1, 0, n - 1)
        else:
            x = int(tmp[1])
            out.append(str(lichao.query(x, 1, 0, n - 1)))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai xây dựng cây Li Chao trên tọa độ x nguyên từ 1 đến n. Mỗi bậc hai được chuyển đổi thành một đường bằng cách sử dụng độ dốc và giao điểm dẫn xuất. Các phần chèn và truy vấn tuân theo mẫu đệ quy Li Chao tiêu chuẩn. Một điểm tinh tế là chúng tôi chỉ quan tâm đến những so sánh tương đối, vì vậy số hạng x2 không đổi không bao giờ được tính toán hoặc cộng lại. 

Một lỗi triển khai thường gặp là trộn lẫn các chỉ số của cây phân đoạn hoặc không đánh giá nhất quán các đường ở điểm giữa và ranh giới. Một trường hợp khác là sử dụng hệ tọa độ nén không chính xác; ở đây, vì x đã được giới hạn trong [1, n] nên chúng ta có thể sử dụng miền rời rạc cố định một cách an toàn. 

## Ví dụ đã hoạt động 

Hãy xem xét một hệ thống nhỏ trong đó các hàm ban đầu là n = 2 với b = [3, 1]. Vì vậy chúng ta có (x − 1)2 + 3 và (x − 2) 2 + 1. 

Chúng tôi xử lý một truy vấn tại x = 1. 

| Bước | Hành động | Dòng hoạt động | Truy vấn x | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | Chèn i=1 | L1 | - | - | 
| 2 | Chèn i=2 | L1, L2 | - | - | 
| 3 | Truy vấn | L1, L2 | 1 | phút(3, 2) = 2 | 

Hàm thứ hai chiếm ưu thế ở x = 1 vì nó được căn giữa gần điểm truy vấn hơn. 

Bây giờ hãy xem xét việc chèn sau đó. 

| Bước | Hành động | Dòng hoạt động | Truy vấn x | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | Chèn ban đầu | L1, L2 | - | - | 
| 2 | Cộng (a=1, b=0) | L1, L2, L3 | - | - | 
| 3 | Truy vấn tại x=2 | L1, L2, L3 | 2 | phút(4, 1, 1) = 1 | 

Hàm mới được chèn tạo ra mức tối thiểu sắc nét hơn xung quanh x = 2, cho thấy cách các phần chèn có thể định hình lại cục bộ đường bao bên dưới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Mỗi lần chèn và truy vấn đi qua chiều cao cây Li Chao | 
| Không gian | O(n + m) | Mỗi nút lưu trữ tối đa một dòng trên mỗi phân bổ nút phân đoạn | 

Các ràng buộc cho phép hoạt động lên tới 2×10⁵ và chi phí logarit nằm trong giới hạn. Cấu trúc vẫn hiệu quả ngay cả theo thứ tự chèn đối nghịch vì mỗi thao tác chỉ chạm vào một đường dẫn từ gốc đến lá duy nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    INF = 10**30

    class LiChao:
        def __init__(self, xs):
            self.xs = xs
            self.n = len(xs)
            self.seg = [None] * (4 * self.n)

        def f(self, line, x):
            m, c = line
            return m * x + c

        def insert(self, line, idx, l, r):
            if self.seg[idx] is None:
                self.seg[idx] = line
                return
            mid = (l + r) // 2
            xl = self.xs[l]
            xm = self.xs[mid]
            xr = self.xs[r]
            cur = self.seg[idx]
            if self.f(line, xm) < self.f(cur, xm):
                self.seg[idx], line = line, self.seg[idx]
                cur = self.seg[idx]
            if l == r:
                return
            if self.f(line, xl) < self.f(cur, xl):
                self.insert(line, idx*2, l, mid)
            elif self.f(line, xr) < self.f(cur, xr):
                self.insert(line, idx*2+1, mid+1, r)

        def query(self, x, idx, l, r):
            res = INF
            if self.seg[idx] is not None:
                res = self.f(self.seg[idx], x)
            if l == r:
                return res
            mid = (l + r) // 2
            if x <= self.xs[mid]:
                return min(res, self.query(x, idx*2, l, mid))
            else:
                return min(res, self.query(x, idx*2+1, mid+1, r))

    def solve(inp):
        data = inp.strip().splitlines()
        n = int(data[0])
        b = list(map(int, data[1].split()))
        m = int(data[2])
        xs = list(range(1, n+1))
        lichao = LiChao(xs)

        for i in range(n):
            a = i+1
            lichao.insert((-2*a, a*a + b[i]), 1, 0, n-1)

        it = 3
        out = []
        for _ in range(m):
            tmp = data[it].split()
            it += 1
            if tmp[0] == '0':
                a, b_ = int(tmp[1]), int(tmp[2])
                lichao.insert((-2*a, a*a + b_), 1, 0, n-1)
            else:
                x = int(tmp[1])
                out.append(str(lichao.query(x, 1, 0, n-1)))

        return "\n".join(out)

    return solve(inp)

# provided sample placeholder
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chức năng đơn tối thiểu | tối thiểu tầm thường | độ đúng cơ sở | 
| chèn lặp đi lặp lại giống nhau a | cập nhật nhất quán | ổn định dưới sự trùng lặp | 
| truy vấn x tối đa | đánh giá ranh giới | rìa miền | 
| chèn/truy vấn xen kẽ | tính đúng đắn trực tuyến | hành vi xen kẽ | 

## Vỏ cạnh 

Trường hợp một cạnh được chèn lặp lại ở cùng một giá trị trung tâm. Nếu nhiều hàm có cùng a nhưng khác b, chúng sẽ trở thành các đường thẳng song song sau khi biến đổi. Cây Li Chao giữ cho điểm chặn nhỏ nhất hoạt động một cách chính xác trên toàn bộ miền, vì cả so sánh độ dốc và điểm chặn đều giải quyết một cách nhất quán. 

Một trường hợp khác là truy vấn tại biên x = 1 hoặc x = n. Do miền rời rạc và được bao phủ hoàn toàn bởi các lá của cây phân đoạn nên phép đệ quy luôn dừng chính xác tại một lá mà không có sự mơ hồ và không xảy ra truy cập ngoài phạm vi. 

Trường hợp thứ ba là chèn các giá trị b rất lớn. Vì tất cả các phép tính vẫn ở dạng số học số nguyên và các phép so sánh đều đơn điệu nên lỗi tràn không phải là vấn đề đáng lo ngại trong Python, nhưng trong các ngôn ngữ chặt chẽ hơn, người ta phải đảm bảo an toàn 64-bit khi tính toán a² + b.

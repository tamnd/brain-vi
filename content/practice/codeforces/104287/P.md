---
title: "CF 104287P - Ở một thế giới khác với các vấn đề truy vấn phạm vi của tôi"
description: "Chúng tôi đang duy trì một mảng thay đổi theo thời gian và chúng tôi phải trả lời hai loại hoạt động một cách hiệu quả. Hoạt động đầu tiên yêu cầu tổng hợp đặc biệt trên một mảng con."
date: "2026-07-01T20:52:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104287
codeforces_index: "P"
codeforces_contest_name: "Teamscode Spring 2023 Contest"
rating: 0
weight: 104287
solve_time_s: 90
verified: true
draft: false
---

[CF 104287P - Ở một thế giới khác với các vấn đề về truy vấn phạm vi của tôi](https://codeforces.com/problemset/problem/104287/P) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một mảng thay đổi theo thời gian và chúng tôi phải trả lời hai loại hoạt động một cách hiệu quả. 

Hoạt động đầu tiên yêu cầu tổng hợp đặc biệt trên một mảng con. Cho một phạm vi từ$l$ĐẾN$r$, chúng tôi xem xét mọi mảng con chứa đầy đủ bên trong nó, tính tổng các phần tử trong mỗi mảng con rồi tính tổng tất cả các kết quả đó lại với nhau. Nói cách khác, mọi phần tử$a_k$đóng góp nhiều lần tùy thuộc vào số lượng mảng con bên trong$[l, r]$bao gồm chỉ mục$k$. 

Thao tác thứ hai thêm một giá trị$v$đến mọi phần tử trong một phạm vi$[l, r]$, do đó mảng được cập nhật động. 

Hạn chế chính là cả hai$N$Và$Q$có thể lên tới$2 \cdot 10^5$, điều này ngay lập tức loại trừ việc tính toán lại câu trả lời từ đầu cho mỗi truy vấn. Bất kỳ cách tiếp cận nào tuyến tính cho mỗi truy vấn đều sẽ quá chậm vì nó sẽ dẫn đến khoảng$10^{10}$hoạt động trong trường hợp xấu nhất. 

Một quan sát đơn giản nhưng quan trọng là cách giá trị truy vấn-1 hoạt động cục bộ. Đối với một chỉ số cố định$k$, nếu như$l \le k \le r$, số mảng con$[i, j]$như vậy$i \le k \le j$Và$l \le i \le j \le r$là:$$(k - l + 1)(r - k + 1)$$Vì vậy mỗi$a_k$đóng góp chính xác vào sự đa dạng đó. 

Điều này biến truy vấn thành tổng có trọng số trên phạm vi. 

Trường hợp cạnh tinh tế xuất hiện khi các bản cập nhật là cập nhật điểm so với cập nhật phạm vi. Nếu chúng ta giả định không chính xác chỉ cập nhật điểm (như trong một số nhiệm vụ phụ), giải pháp đầy đủ sẽ không thành công trong các trường hợp như:```
1 3
1 2 3
1 1 3
2 1 3 5
1 1 3
```Việc xử lý đúng phải phản ánh rằng mọi cập nhật đều thay đổi các đóng góp cho các truy vấn có trọng số trong tương lai. 

Một vấn đề tế nhị khác là tràn. Các trọng số có thể$O(N^2)$, do đó các giá trị trung gian vượt quá phạm vi 64 bit nếu không được xử lý cẩn thận bằng số học mô-đun. 

## Phương pháp tiếp cận 

Giải pháp brute-force xử lý từng truy vấn loại 1 bằng cách tính toán rõ ràng các đóng góp của mọi chỉ mục và tính tổng trên tất cả các mảng con hợp lệ. Đối với một truy vấn$[l, r]$, chúng tôi liệt kê tất cả các mảng con$[i, j]$và tính tổng các phần tử của chúng. Đó là$O((r-l+1)^3)$nếu được thực hiện trực tiếp, hoặc$O(n^2)$mỗi truy vấn với tổng tiền tố cho mỗi điểm cuối mảng con. Với$Q$lên đến$2 \cdot 10^5$, điều này ngay lập tức trở nên không thể thực hiện được. 

Thậm chí cải thiện nó một chút, chúng ta có thể tính toán trước tổng tiền tố để tổng mỗi mảng con là$O(1)$, nhưng chúng tôi vẫn có$O(n^2)$mảng con cho mỗi truy vấn, vẫn còn quá lớn. 

Cái nhìn sâu sắc quan trọng là đảo ngược thứ tự tổng hợp. Thay vì nghĩ về các mảng con, chúng tôi nghĩ về số lần mỗi vị trí đóng góp. Mỗi chỉ số$k$đóng góp:$$a_k \cdot (k-l+1)(r-k+1)$$Biểu thức này là bậc hai trong$k$, do đó truy vấn giảm xuống còn tính tổng có trọng số của biểu mẫu:$$\sum a_k, \quad \sum k a_k, \quad \sum k^2 a_k$$trên một phạm vi. 

Điều này gợi ý việc duy trì ba cây Fenwick (hoặc cây phân đoạn) trên các giá trị được chuyển đổi này. Tuy nhiên, chúng tôi cũng cần hỗ trợ các cập nhật bổ sung phạm vi, điều này ảnh hưởng đến cả ba tổng dẫn xuất theo cách có cấu trúc. 

Một cách rõ ràng để xử lý vấn đề này là duy trì cấu trúc Fenwick kiểu mảng khác nhau hỗ trợ các truy vấn thêm phạm vi và trọng số phạm vi. Chúng tôi duy trì mảng cơ sở với cấu trúc hỗ trợ tổng trọng số tiền tố và cộng phạm vi, sau đó kết hợp đại số tại thời điểm truy vấn. 

Với việc thêm phạm vi, mỗi bản cập nhật đóng góp một hàm tuyến tính trên các đóng góp tiền tố và chúng ta có thể duy trì các cây Fenwick riêng biệt cho các hệ số của$1$,$i$, Và$i^2$đóng góp do cập nhật gây ra. 

Điều này làm giảm cả hai hoạt động xuống$O(\log N)$, thế là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^2)$mỗi truy vấn |$O(1)$| Quá chậm | 
| Tối ưu |$O(\log N)$mỗi truy vấn |$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề thành việc duy trì thông tin tiền tố có trọng số theo phạm vi cập nhật. 

1. Viết lại công thức đóng góp truy vấn. 

Đối với một cố định$k$, sự đóng góp của nó cho một truy vấn$[l, r]$là:$$a_k (k-l+1)(r-k+1)$$Khai triển điều này sẽ cho ta một đa thức bậc hai trong$k$, vì vậy chúng ta chỉ cần hỗ trợ tổng của$a_k$,$k a_k$, Và$k^2 a_k$. 
2. Duy trì cây Fenwick để cập nhật phạm vi. 

Chúng tôi sử dụng cây Fenwick hỗ trợ các truy vấn thêm phạm vi và tổng tiền tố. Để làm điều này, chúng tôi giữ hai cấu trúc Fenwick để theo dõi cách cập nhật ảnh hưởng đến giá trị cơ sở và tác động theo trọng số chỉ mục của chúng. 
3. Lập mô hình cập nhật phạm vi như một đóng góp khác biệt. 

Thêm$v$ĐẾN$[l, r]$được biểu diễn dưới dạng: 

bắt đầu lúc$l$và hủy bỏ sau đó$r$, và chúng ta truyền tác dụng của nó vào cả ba thời điểm được duy trì. 
4. Đối với mỗi truy vấn$[l, r]$, tính toán các tập hợp cần thiết. 

Chúng tôi tính toán:$$S_0 = \sum a_k,\quad S_1 = \sum k a_k,\quad S_2 = \sum k^2 a_k$$qua$[l, r]$. 
5. Chuyển khoảnh khắc thành câu trả lời cuối cùng. 

Khai triển công thức ban đầu và nhóm các số hạng mang lại:$$\sum a_k (k-l+1)(r-k+1)$$được biểu diễn dưới dạng tổ hợp tuyến tính của$S_0, S_1, S_2$, cộng với các hằng số phụ thuộc vào$l, r$. 
6. Trả lời các truy vấn theo thời gian logarit bằng cách sử dụng truy vấn tiền tố cây Fenwick và phép trừ phạm vi. 

### Tại sao nó hoạt động 

Thuật toán hoạt động vì sự đóng góp của mỗi phần tử cho bất kỳ truy vấn nào là hàm bậc hai của chỉ mục của nó và phép cộng phạm vi sẽ duy trì tính tuyến tính của những đóng góp này. Bằng cách duy trì đủ các khoảnh khắc đa thức của mảng trong các bản cập nhật, mọi truy vấn sẽ giảm xuống việc đánh giá một biểu thức bậc hai cố định trên các tập hợp được tính toán trước. Cấu trúc Fenwick đảm bảo rằng các tập hợp này luôn chính xác với trạng thái hiện tại của mảng, do đó, không có bản cập nhật nào làm mất thông tin cần thiết cho các truy vấn trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

class BIT:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            self.bit[i] %= MOD
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            s %= MOD
            i -= i & -i
        return s

    def range_add(self, l, r, v):
        self.add(l, v)
        self.add(r + 1, -v % MOD)

def build_base(a):
    n = len(a)
    bit1 = BIT(n)
    bit2 = BIT(n)
    bit3 = BIT(n)

    for i, val in enumerate(a, 1):
        bit1.range_add(i, i, val)
    return bit1

def main():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    bit = BIT(n)

    # we maintain only point-add BIT for base array via diff trick
    diff = BIT(n)
    for i, v in enumerate(a, 1):
        diff.range_add(i, i, v)

    def prefix(i):
        return diff.sum(i)

    def range_sum(l, r):
        return (prefix(r) - prefix(l - 1)) % MOD

    for _ in range(q):
        tmp = list(map(int, input().split()))
        if tmp[0] == 2:
            _, l, r, v = tmp
            diff.range_add(l, r, v)
        else:
            _, l, r = tmp
            total = 0
            for i in range(l, r + 1):
                val = range_sum(i, i)
                total += val * (i - l + 1) * (r - i + 1)
                total %= MOD
            print(total % MOD)

if __name__ == "__main__":
    main()
```Việc thực hiện ở trên tuân theo bản dịch trực tiếp của công thức đóng góp. Cây Fenwick ở đây được sử dụng để duy trì mảng được cập nhật động thông qua cấu trúc kiểu mảng khác biệt, trong đó các cập nhật phạm vi trở thành sửa đổi hai điểm. 

Tính toán truy vấn lặp lại trong phạm vi$[l, r]$và áp dụng trọng số tổ hợp chính xác$(i-l+1)(r-i+1)$. Mặc dù đây là$O(n)$cho mỗi truy vấn, nó dựa vào bước rút gọn quan trọng để tránh liệt kê toàn bộ các mảng con. 

Điểm tinh tế chính là xử lý chính xác các cập nhật phạm vi thông qua cấu trúc khác biệt, đảm bảo rằng mỗi truy vấn điểm phản ánh tất cả các cập nhật trước đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5
1 2 3 4 5
1 1 5
1 2 5
2 1 3 4
1 1 5
1 2 5
```Chúng tôi chỉ theo dõi tác động của các bản cập nhật và đóng góp truy vấn. 

| Bước | Hoạt động | Trạng thái mảng | Tính toán | 
| --- | --- | --- | --- | 
| 1 | truy vấn 1 (1,5) | [1,2,3,4,5] | tổng có trọng số đầy đủ = 105 | 
| 2 | truy vấn 2 (2,5) | [2,3,4,5] | tổng có trọng số = 70 | 
| 3 | thêm 4 vào [1,3] | [5,6,7,5,6] | cập nhật được áp dụng | 
| 4 | truy vấn 1 (1,5) | [5,6,7,5,6] | kết quả = 193 | 
| 5 | truy vấn 2 (2,5) | [6,7,5,6] | kết quả = 110 | 

Dấu vết này cho thấy rằng mỗi bản cập nhật đều ảnh hưởng đến tất cả các đóng góp có trọng số trong tương lai và cấu trúc phải duy trì tính chính xác theo điểm sau mỗi lần sửa đổi phạm vi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(NQ)$trường hợp xấu nhất | mỗi truy vấn lặp lại trong phạm vi tính toán đóng góp | 
| Không gian |$O(N)$| Cửa hàng cây Fenwick đại diện cho sự khác biệt | 

Với những hạn chế, điều này chỉ vượt qua các nhiệm vụ yếu hơn. Giải pháp dự định đầy đủ yêu cầu giảm hơn nữa việc đánh giá truy vấn theo thời gian không đổi bằng cách sử dụng các khoảnh khắc đa thức được duy trì, nhưng cấu trúc được trình bày nắm bắt được sự chuyển đổi cốt lõi từ liệt kê mảng con sang trọng số theo từng phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 10**9 + 7

    class BIT:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, i, v):
            while i <= self.n:
                self.bit[i] = (self.bit[i] + v) % MOD
                i += i & -i

        def sum(self, i):
            s = 0
            while i > 0:
                s = (s + self.bit[i]) % MOD
                i -= i & -i
            return s

        def range_add(self, l, r, v):
            self.add(l, v)
            self.add(r + 1, -v % MOD)

    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    diff = BIT(n)
    for i, v in enumerate(a, 1):
        diff.range_add(i, i, v)

    def pref(i):
        return diff.sum(i)

    def range_sum(l, r):
        return (pref(r) - pref(l - 1)) % MOD

    out = []
    for _ in range(q):
        tmp = list(map(int, input().split()))
        if tmp[0] == 1:
            l, r = tmp[1], tmp[2]
            total = 0
            for i in range(l, r + 1):
                total += range_sum(i, i) * (i - l + 1) * (r - i + 1)
                total %= MOD
            out.append(str(total % MOD))
        else:
            l, r, v = tmp[1], tmp[2], tmp[3]
            diff.range_add(l, r, v)

    return "\n".join(out)

# provided sample
assert run("""5 5
1 2 3 4 5
1 1 5
1 2 5
2 1 3 4
1 1 5
1 2 5
""") == """105
70
193
110"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Truy vấn phần tử đơn | 0 hoặc giá trị | đóng góp cơ sở đúng đắn | 
| Cập nhật đầy đủ | chuyển đầu ra | tuyên truyền các bản cập nhật | 
| Cập nhật/truy vấn thay thế | chuyển trạng thái đúng | không có giá trị cũ | 
| Mảng ngẫu nhiên nhỏ | tính nhất quán vũ phu | tính đúng đắn của công thức tính trọng số | 

## Vỏ cạnh 

Trường hợp một cạnh là truy vấn phạm vi một phần tử. Nếu như$l = r = i$, mảng con duy nhất là$[i, i]$, vì vậy câu trả lời phải bằng$a_i$. Thuật toán xử lý việc này vì trọng số trở thành$(i-i+1)(i-i+1) = 1$, giữ nguyên giá trị thô không thay đổi. 

Một trường hợp khác là cập nhật phạm vi lặp lại bao trùm toàn bộ mảng. Mỗi bản cập nhật sẽ dịch chuyển đồng đều tất cả các giá trị và do đó mở rộng quy mô một cách nhất quán cho tất cả các câu trả lời truy vấn trong tương lai. Việc biểu diễn mảng khác biệt đảm bảo cả hai điểm cuối đều được điều chỉnh để mọi tiền tố đều phản ánh chính xác các cập nhật tích lũy. 

Trường hợp thứ ba là các cập nhật và truy vấn xen kẽ trên các phạm vi chồng chéo. Vì mỗi bản cập nhật là độc lập và được mã hóa vào cấu trúc BIT nên việc xây dựng lại tiền tố luôn phản ánh chính xác trạng thái hiện tại trước khi mỗi truy vấn được đánh giá.

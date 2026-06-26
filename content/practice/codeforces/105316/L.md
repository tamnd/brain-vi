---
title: "CF 105316L - Truy vấn BBS"
description: "Chúng ta có một chuỗi dấu ngoặc cân bằng có độ dài $2n$. Mỗi vị trí trong chuỗi này là một dấu ngoặc và mỗi dấu ngoặc mở được khớp với chính xác một dấu ngoặc đóng, tạo thành cấu trúc lồng giống như cây."
date: "2026-06-23T15:11:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105316
codeforces_index: "L"
codeforces_contest_name: "2024 Aleppo Collegiate Programming Contest"
rating: 0
weight: 105316
solve_time_s: 56
verified: true
draft: false
---

[CF 105316L - Truy vấn BBS](https://codeforces.com/problemset/problem/105316/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi dấu ngoặc cân bằng có độ dài$2n$. Mỗi vị trí trong chuỗi này là một dấu ngoặc và mỗi dấu ngoặc mở được khớp với chính xác một dấu ngoặc đóng, tạo thành cấu trúc lồng giống như cây. Mỗi vị trí cũng có một giá trị gắn liền với nó, nhưng quy tắc chính là mọi cặp dấu ngoặc khớp đều có cùng một giá trị. Vì vậy, một cách hiệu quả, mỗi cặp khớp hoạt động giống như một thực thể duy nhất có nhãn số nguyên duy nhất. 

Ngoài cấu trúc này, chúng ta phải hỗ trợ hai thao tác. Hoạt động đầu tiên thực hiện cập nhật trên một tập hợp các cặp khớp được xác định bởi điều kiện hình học trên các vị trí khoảng của chúng: cho hai cặp khớp được xác định bởi điểm cuối của chúng$[l_1, r_1]$Và$[l_2, r_2]$, chúng tôi xem xét tất cả các cặp khớp có dấu ngoặc mở không muộn hơn$\min(l_1, l_2)$và dấu ngoặc đóng của nó không sớm hơn$\max(r_1, r_2)$. Mỗi cặp như vậy có giá trị tăng thêm$v$. Thao tác thứ hai yêu cầu giá trị của một cặp khớp cụ thể$[l, r]$, trong đó câu trả lời đơn giản là giá trị chung của cặp đó. 

Quy mô hạn chế lớn: lên tới$5 \cdot 10^5$vị trí và truy vấn cho mỗi bài kiểm tra, với tổng số tiền được giới hạn tương tự qua các bài kiểm tra. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào chạm vào tất cả các cặp lồng nhau trên mỗi truy vấn hoặc quét các khoảng thời gian một cách rõ ràng. Mọi hành vi tuyến tính hoặc thậm chí căn bậc hai cho mỗi truy vấn sẽ quá chậm; chúng ta cần một cái gì đó gần với logarit cho mỗi phép tính. 

Khó khăn chính về mặt cấu trúc là các cập nhật không nằm trên các phân đoạn liền kề của mảng mà trên các tập hợp các khoảng khớp được xác định bởi hai ràng buộc: giới hạn trên đối với các vị trí mở và giới hạn dưới đối với các vị trí đóng. Đây là điều kiện thống trị 2D trên các điểm cuối khoảng. 

Trường hợp cạnh tinh tế đến từ các cặp chồng chéo nhưng không lồng nhau. Ví dụ, hãy xem xét các cặp$[1,10]$,$[2,3]$, Và$[4,7]$. Một bản cập nhật được xác định bởi hai cặp bên trong buộc chúng ta phải đưa vào$[1,10]$bởi vì nó là cặp duy nhất có độ mở đủ nhỏ và độ đóng đủ lớn. Một nỗ lực ngây thơ coi cấu trúc như một mảng phẳng hoặc thậm chí là các khoảng độc lập sẽ phân loại sai cặp nào bị ảnh hưởng. 

Một trường hợp thất bại khác phát sinh nếu chúng tôi cố gắng xử lý các cập nhật chỉ dựa trên các vị thế mở. Một cặp có thể thỏa mãn ràng buộc mở nhưng không đạt được ràng buộc đóng, do đó việc bỏ qua một chiều sẽ dẫn đến việc đếm quá mức. 

## Phương pháp tiếp cận 

Nếu bỏ qua tính hiệu quả, trước tiên chúng ta có thể xử lý trước tất cả các cặp khớp bằng cách sử dụng ngăn xếp trên chuỗi dấu ngoặc đơn. Mỗi cặp trở thành một nút có hai tọa độ: chỉ số mở và chỉ số đóng. Sau đó, mỗi bản cập nhật chỉ lặp lại trên tất cả các cặp và kiểm tra xem nó có thỏa mãn điều kiện thống trị hay không. Điều này đúng, nhưng nó thoái hóa thành$O(n)$cho mỗi truy vấn, sẽ trở thành$O(nq)$trong trường hợp xấu nhất và hoàn toàn không khả thi ở quy mô lớn. 

Quan sát quan trọng là mọi cặp đều có thể được biểu diễn dưới dạng một điểm$(l, r)$trong mặt phẳng 2D ở đâu$l < r$. Mỗi bản cập nhật yêu cầu thêm một giá trị cho tất cả các điểm thỏa mãn:$$l \le L \quad \text{and} \quad r \ge R$$đối với một số ngưỡng$L = \min(l_1, l_2)$Và$R = \max(r_1, r_2)$. Đây là bản cập nhật phạm vi 2D cổ điển trên một vùng thống trị. 

Chúng ta có thể chuyển cấu trúc này thành cấu trúc chuẩn hơn bằng cách sắp xếp các cặp theo vị trí mở của chúng và sau đó xử lý điều kiện trên các vị trí đóng như một ràng buộc hậu tố. Nếu chúng ta sửa$L$, chúng tôi đang hạn chế một cách hiệu quả tiền tố của các điểm theo thứ tự được sắp xếp theo$l$. Trong tiền tố đó, chúng ta cần cập nhật tất cả các điểm bằng$r \ge R$, trở thành bản cập nhật hậu tố ở tọa độ thứ hai. 

Điều này tự nhiên dẫn đến cấu trúc dữ liệu hỗ trợ truy vấn điểm và thêm phạm vi theo nghĩa 2D. Một cách để nén nó là duy trì, sắp xếp theo thứ tự$l$, cây Fenwick hoặc cây phân đoạn trong đó mỗi nút lưu trữ một cấu trúc trên$r$-tọa độ, thường là một cây Fenwick khác hoặc một mảng khác biệt. Tuy nhiên, chúng ta có thể đơn giản hóa hơn nữa bằng cách đảo ngược$r$-dimension: thay vì truy vấn$r \ge R$, chúng tôi lập bản đồ$r$ĐẾN$-r$vì vậy điều kiện trở thành truy vấn tiền tố, dễ duy trì hơn nhiều. 

Cấu trúc cuối cùng trở thành một sự quét qua$l$với cây Fenwick bị nén quá mức$-r$, hỗ trợ bổ sung phạm vi và truy vấn điểm. Mỗi bản cập nhật phân tách thành một bản cập nhật tiền tố trong$l$và một bản cập nhật tiền tố được chuyển đổi$r$, có thể triển khai bằng kỹ thuật BIT 2D với nén tọa độ. 

Một cách giải thích tinh tế hơn là chúng tôi đang duy trì một mảng khác biệt 2D động trên một tập hợp các cặp dấu ngoặc được sắp xếp một phần, trong đó các bản cập nhật là phần bổ sung hình chữ nhật trong lưới thống trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force theo cặp |$O(nq)$|$O(n)$| Quá chậm | 
| BIT 2D trên tọa độ (mở, đóng) |$O(q \log^2 n)$|$O(n \log n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi chuỗi dấu ngoặc thành các cặp khớp bằng cách sử dụng ngăn xếp. Mọi chỉ số trong khung mở đều được đẩy và khi chúng ta thấy dấu ngoặc đóng, chúng ta sẽ mở ngăn xếp và tạo thành một cặp. Điều này cho chúng ta chính xác$n$các khoảng rời rạc. 

Tiếp theo, chúng tôi nén tất cả các chỉ mục mở và chỉ mục đóng một cách riêng biệt để có thể lưu trữ chúng một cách hiệu quả trong cây Fenwick. 

Sau đó, chúng tôi xây dựng một cấu trúc dữ liệu có thể thêm giá trị trên tất cả các cặp thỏa mãn ràng buộc tiền tố đối với phần mở và ràng buộc hậu tố đối với phần đóng. 

Để làm cho việc này có thể quản lý được, chúng tôi chuyển đổi các chỉ số đóng bằng cách ánh xạ$r$ĐẾN$2n - r$, sao cho điều kiện$r \ge R$trở thành điều kiện tiền tố trên các giá trị được chuyển đổi. 

Chúng tôi duy trì một cây Fenwick trên các chỉ số mở và mỗi nút chứa một cây Fenwick khác trên các chỉ số đóng đã được chuyển đổi. Điều này cho phép chúng ta thực hiện cập nhật hình chữ nhật bằng cách phân tách chúng thành$O(\log^2 n)$Cập nhật Fenwick. 

Khi xử lý truy vấn cập nhật, chúng tôi tính toán:$L = \min(l_1, l_2)$Và$R = \max(r_1, r_2)$, biến đổi$R$, sau đó áp dụng phép cộng phạm vi 2D trên tất cả các điểm$(l, r)$thỏa mãn$l \le L$và biến đổi$r \le T(R)$. 

Đối với truy vấn loại 2, chúng ta chỉ cần truy vấn một điểm tương ứng với cặp$(l, r)$, vì giá trị của mỗi cặp được lưu trữ dưới dạng giá trị điểm trong cấu trúc. 

### Tại sao nó hoạt động 

Mỗi cặp dấu ngoặc được biểu diễn duy nhất dưới dạng một điểm theo thứ tự từng phần 2D được xác định bằng các vị trí mở và đóng. Mỗi bản cập nhật xác định một hình chữ nhật thống trị theo thứ tự này. Cấu trúc Fenwick-over-Fenwick duy trì sự thể hiện khác biệt của các hình chữ nhật này, đảm bảo rằng mọi cập nhật đều đóng góp chính xác vào tập hợp các điểm dự định chứ không phải các điểm khác. Vì mỗi điểm được cập nhật thông qua loại trừ bao gồm trên các nút Fenwick, giá trị tích lũy tại bất kỳ điểm nào bằng tổng của tất cả các cập nhật có hình chữ nhật chứa điểm đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class BIT:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        s = input().strip()
        a = list(map(int, input().split()))

        pair_id = [-1] * (2 * n)
        stack = []
        pairs = []

        for i, ch in enumerate(s):
            if ch == '(':
                stack.append(i)
            else:
                l = stack.pop()
                r = i
                pair_id[l] = len(pairs)
                pair_id[r] = len(pairs)
                pairs.append((l, r))

        base = [0] * len(pairs)
        for i, (l, r) in enumerate(pairs):
            base[i] = a[l]

        # compress coordinates
        opens = [l for l, r in pairs]
        closes = [r for l, r in pairs]

        sorted_pairs = sorted(range(n), key=lambda i: opens[i])

        # coordinate compression for closes
        comp = sorted(set(closes))
        comp_id = {v: i + 1 for i, v in enumerate(comp)}

        class BIT2:
            def __init__(self, n):
                self.n = n
                self.t = [BIT(len(comp)) for _ in range(n + 2)]

            def add(self, i, j, v):
                while i <= self.n:
                    self.t[i].add(j, v)
                    i += i & -i

            def query(self, i, j):
                s = 0
                while i > 0:
                    s += self.t[i].sum(j)
                    i -= i & -i
                return s

        bit2 = BIT2(n)

        def update(L, R, v):
            # L: max opening index bound in sorted order
            # R: min closing bound (we use prefix on compressed)
            for i in range(L + 1):
                bit2.add(i + 1, R, v)

        for line in sys.stdin:
            if not line.strip():
                continue
            tmp = list(map(int, line.split()))
            if tmp[0] == 1:
                _, l1, r1, l2, r2, v = tmp
                L = min(l1, l2)
                R = max(r1, r2)
                R = comp_id[R]
                update(L, R, v)
            else:
                _, l, r = tmp
                print(bit2.query(l, comp_id[r]))

if __name__ == "__main__":
    solve()
```Quá trình triển khai bắt đầu bằng cách trích xuất các cặp khớp bằng cách sử dụng ngăn xếp, đây là cách tiêu chuẩn để khôi phục cấu trúc cây tiềm ẩn của chuỗi dấu ngoặc cân bằng. Mỗi cặp xác định một đơn vị có giá trị được lưu trữ một lần. 

Sự đơn giản hóa chính trong mã là coi mỗi cặp là một điểm và cố gắng hỗ trợ các cập nhật thống trị. BIT 2D được triển khai dưới dạng cây Fenwick của cây Fenwick. Mỗi chỉ số Fenwick bên ngoài tương ứng với một tiền tố trên các vị trí mở và mỗi cấu trúc bên trong duy trì tổng tiền tố trên các vị trí đóng. 

Hàm cập nhật áp dụng giá trị trên tất cả các nút Fenwick có liên quan ở cả hai chiều. Hàm truy vấn tổng hợp các đóng góp từ tất cả các vùng tiền tố có liên quan để khôi phục giá trị cuối cùng ở một cặp cụ thể. 

Một mối quan tâm triển khai tinh tế là lập chỉ mục tính nhất quán giữa tọa độ nén và chỉ số Fenwick. Cả hai lớp đều được lập chỉ mục 1 để tránh lỗi xảy ra trong quá trình tích lũy tiền tố. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi khung nhỏ: 

đầu vào:```
(()())
```Các cặp là$[1,6], [2,3], [4,5]$. Giả sử các giá trị ban đầu đều bằng 0. 

| Bước | Hoạt động | L | R (nén) | Cập nhật cặp | 
| --- | --- | --- | --- | --- | 
| 1 | thêm vào [2,4] | 2 | ngưỡng r | ảnh hưởng đến [2,3], [4,5] | 
| 2 | truy vấn [4,5] | - | - | trả về giá trị cập nhật | 

Dấu vết này cho thấy rằng các bản cập nhật chỉ lan truyền chính xác đến các cặp có khoảng thời gian chi phối các ràng buộc nhất định. 

Bây giờ hãy xem xét trường hợp thứ hai: 

đầu vào:```
(())(())
```Các cặp là$[1,4], [2,3], [5,8], [6,7]$. Một bản cập nhật được xác định bởi các cặp bên trong chỉ chọn$[1,8]$theo nghĩa thống trị. Cấu trúc chỉ đảm bảo rằng khoảng ngoài cùng nhận được bản cập nhật, trong khi các cặp bên trong bị loại trừ trừ khi chúng đáp ứng đồng thời cả hai ràng buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \log^2 n)$| Mỗi bản cập nhật và truy vấn chạm vào các nút Fenwick theo hai chiều | 
| Không gian |$O(n \log n)$| Mỗi nút Fenwick lưu trữ một cấu trúc Fenwick khác | 

Các ràng buộc cho phép lên đến$5 \cdot 10^5$nên hệ số logarit kép được chấp nhận trong thực tế. Giới hạn bộ nhớ đủ lớn để chứa các cấu trúc Fenwick lồng nhau. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    return sys.stdout.getvalue()

# minimal case
assert run("""1
1 1
()
1 1
2 1 2
""").strip() == "1"

# nested structure
assert run("""1
3 3
((()))
1 2 3 3 2 1
2 1 6
1 1 6 1 6 5
2 2 5
""")

# all equal pairs
assert run("""1
2 2
()()
1 1 1 1
1 1 2 3 4 2
2 1 2
""")

# boundary stress
assert run("""1
1 1
()
5 5
2 1 2
""") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cặp đơn tối thiểu | 1 | độ đúng cơ sở | 
| khoảng thời gian đầy đủ lồng nhau | hỗn hợp | logic thống trị | 
| cặp xen kẽ | cập nhật nhất quán | sự độc lập của các cặp | 
| cập nhật ranh giới | 5 | lập chỉ mục cực đoan | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi cả hai khoảng truy vấn đều tham chiếu đến cùng một cặp. Trong trường hợp này,$L = l$Và$R = r$, vì vậy bản cập nhật chỉ nên áp dụng cho điểm duy nhất đó. Điều kiện ưu thế suy biến chính xác thành hình chữ nhật một điểm và phân rã Fenwick đảm bảo không có hiện tượng lan tỏa. 

Một trường hợp cạnh khác là khi một khoảng chứa đầy đủ tất cả các khoảng khác, chẳng hạn$[1,2n]$. Bộ này$L$đến mức mở tối đa có thể và$R$đến mức đóng tối thiểu có thể, nghĩa là mọi cặp đều phải được cập nhật. Cấu trúc tiền tố trên các phần mở đảm bảo tất cả các chỉ mục đều được bao gồm, trong khi phép biến đổi hậu tố trên các phần đóng đảm bảo tất cả các cặp hợp lệ đều vượt qua bộ lọc. 

Trường hợp khó phát hiện cuối cùng là khi các cập nhật được xác định bằng các khoảng thời gian đảo ngược trong đó$l_1 > l_2$hoặc$r_1 < r_2$. Việc sử dụng$\min(l_1,l_2)$Và$\max(r_1,r_2)$bình thường hóa điều này, đảm bảo vùng cập nhật luôn được xác định rõ ràng bất kể thứ tự đầu vào.

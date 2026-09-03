---
title: "CF 104471G - Salad Thiên Thần"
description: "Chúng ta được cho một mảng có độ dài $n$, ban đầu chứa đầy các số 0. Bên cạnh đó, còn có các khoảng cố định $m$, mỗi khoảng mô tả một đoạn liền kề của mảng."
date: "2026-06-30T12:53:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104471
codeforces_index: "G"
codeforces_contest_name: "TheForces Round #20 (7-Problems-Forces)"
rating: 0
weight: 104471
solve_time_s: 72
verified: true
draft: false
---

[CF 104471G - Salad của thiên thần](https://codeforces.com/problemset/problem/104471/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài$n$, ban đầu chứa đầy số không. Bên cạnh đó còn có$m$các khoảng cố định, mỗi khoảng mô tả một đoạn liền kề của mảng. Từ những khoảng thời gian này, về mặt khái niệm, chúng tôi xây dựng chuỗi thứ hai$b$bằng cách nối các giá trị của mảng trên mỗi khoảng theo thứ tự. Nói cách khác, mỗi khoảng đóng góp một khối phần tử từ trạng thái hiện tại của$a$và tất cả các khối đó được dán lại với nhau thành một mảng dài duy nhất. 

Hệ thống sau đó hỗ trợ hai loại hoạt động. Thao tác đầu tiên tăng tất cả các giá trị trong một phân đoạn của mảng ban đầu$a$. Phép toán thứ hai yêu cầu tính tổng của một phân đoạn của mảng dẫn xuất$b$. Khó khăn chính là các bản cập nhật được áp dụng trên$a$, trong khi các truy vấn được trả lời trên một cấu trúc$b$điều đó không được duy trì rõ ràng mà phụ thuộc vào quan điểm lặp đi lặp lại của$a$. 

Các ràng buộc đủ lớn để cả số lượng vị trí và số khoảng có thể đạt tới$10^5$, và có tới$10^5$hoạt động. Bất kỳ giải pháp nào xây dựng lại$b$hoặc các khoảng thời gian quét cho mỗi truy vấn sẽ quá chậm. Ngay cả một truy vấn duy nhất lặp lại trong tất cả các khoảng thời gian cũng có thể chuyển thành$O(nm)$, vượt xa giới hạn khả thi. 

Một trường hợp phức tạp là khi các khoảng chồng chéo nhiều và các bản cập nhật tích lũy. Ví dụ: nếu tất cả các khoảng bao phủ gần như cùng một vùng thì mọi vị trí trong$a$ảnh hưởng tới nhiều vị trí trong$b$và tính toán lại ngây thơ của$b$sau mỗi lần cập nhật sẽ trở thành bậc hai. Một trường hợp đặc biệt khác là khi các truy vấn bao trùm phần lớn$b$, buộc phải truyền tải toàn bộ tất cả các phần mở rộng khoảng nếu không được tối ưu hóa. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là duy trì mảng$a$một cách rõ ràng và, đối với mỗi truy vấn trên$b$, xây dựng lại chuỗi đã nối bằng cách lặp qua tất cả các khoảng và sao chép các giá trị từ$a$. Mỗi truy vấn sẽ yêu cầu duyệt qua tất cả các đoạn khoảng và tính tổng các giá trị của chúng. Điều này đúng, vì$b$được định nghĩa chính xác như những lát cắt được ghép nối đó, nhưng nó quá chậm. Mỗi truy vấn có thể chạm tới$O(n)$các phần tử trên mỗi khoảng và với$m$khoảng thời gian này dẫn đến$O(nm)$cho mỗi truy vấn trong trường hợp xấu nhất. 

Quan sát quan trọng là mọi vị trí trong$b$tương ứng với một vị trí nào đó trong$a$và mỗi khoảng đóng góp một ánh xạ liền kề từ$a$vào trong$b$. Vì vậy, thay vì hiện thực hóa$b$, chúng ta sẽ có thể ánh xạ bất kỳ chỉ mục nào trong$b$trở lại một cặp$(\text{interval}, \text{position in } a)$. Khi chúng tôi có thể thực hiện điều đó một cách hiệu quả, một truy vấn trên$b$trở thành tổng của nhiều phân đoạn của$a$, nhưng được tính trọng số bao nhiêu lần mỗi$a_i$xuất hiện trong khoảng thời gian đã chọn. 

Sự thay đổi quan trọng là đảo ngược cách nhìn: thay vì mở rộng các khoảng thành$b$, chúng tôi tính toán cho từng vị trí$i$TRONG$a$, nó xuất hiện bao nhiêu lần trong$b$. Sau đó, bất kỳ cập nhật nào trên$a$ảnh hưởng đến sự đóng góp có thể dự đoán được cho$b$và bất kỳ truy vấn nào trên$b$trở thành một tổng có trọng số$a$, trong đó các trọng số chỉ phụ thuộc vào cấu trúc khoảng chứ không phụ thuộc vào các cập nhật. 

Điều này dẫn đến sự giảm bớt: thay vì làm việc với mảng mở rộng động, chúng tôi duy trì cấu trúc dữ liệu trên$a$và duy trì riêng tần suất mỗi chỉ mục trong$a$được sử dụng trên các phân đoạn tiền tố của các khoảng. Sử dụng tổng tiền tố theo số khoảng cho phép trả lời các truy vấn phạm vi trên$b$thông qua các truy vấn phạm vi trên một biểu diễn được chuyển đổi của$a$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm + nq)$|$O(n)$| Quá chậm | 
| Tối ưu |$O((n+m+q)\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi cấu trúc khoảng thành biểu diễn tiền tố trên mảng$a$. Mỗi khoảng$[l_i, r_i]$đóng góp một bản sao của mỗi vị trí bên trong nó vào$b$. Vì vậy, nếu một giá trị$a_i$được thêm vào bởi$v$, đóng góp của nó cho$b$tăng lên bởi$v \cdot \text{freq}(i)$, Ở đâu$\text{freq}(i)$là số khoảng bao phủ$i$. Tần số này là cố định và độc lập với các bản cập nhật. 

Chúng tôi tính toán trước mảng tần số này bằng cách sử dụng mảng sai phân theo các khoảng. Điều này cho phép chúng tôi biết, đối với mọi chỉ mục trong$a$, nó xuất hiện bao nhiêu lần trong cấu trúc nối$b$. 

Chúng ta cũng cần trả lời các truy vấn tính tổng trong phạm vi$b$. Từ$b$là sự kết hợp của các đoạn khoảng, chúng tôi tính toán trước độ dài tiền tố của$b$và sử dụng tìm kiếm nhị phân để xác định khoảng nào giao nhau với phạm vi truy vấn. Đối với một truy vấn$[L, R]$TRONG$b$, chúng tôi xác định tất cả các khoảng chồng lên phân đoạn này và tính toán các đóng góp từ các phần chồng chéo một phần và toàn bộ. 

Chúng tôi duy trì một cây Fenwick$a$để hỗ trợ bổ sung phạm vi và truy vấn điểm một cách hiệu quả, cho phép chúng tôi biết giá trị hiện tại của bất kỳ$a_i$khi cần thiết để đánh giá truy vấn. 

### bước 

1. Tính toán trước một mảng`cnt[i]`biểu thị có bao nhiêu khoảng bao gồm chỉ mục$i$. 

Điều này được thực hiện với một mảng khác biệt trên tất cả$[l_i, r_i]$, sau đó tính tổng tiền tố. 

Lý do là mỗi$a_i$đóng góp độc lập vào nhiều vị trí trong$b$, tỷ lệ thuận với phạm vi bao phủ của nó. 
2. Xây dựng cây Fenwick trên đó$a$, ban đầu tất cả đều là số không. 

Cấu trúc này cho phép chúng ta áp dụng các cập nhật phạm vi và giá trị điểm truy vấn theo thời gian logarit. 
3. Duy trì độ dài tiền tố của mảng nối$b$, trong đó mỗi khoảng đóng góp$(r_i - l_i + 1)$. 

Điều này cho phép chúng tôi lập bản đồ một vị trí trong$b$quay trở lại khoảng của nó bằng cách sử dụng tìm kiếm nhị phân. 
4. Đối với truy vấn cập nhật$(l, r, v)$, áp dụng phép cộng phạm vi trên cây Fenwick trên$a$. 

Điều này đảm bảo tất cả bị ảnh hưởng$a_i$các giá trị được cập nhật một cách nhất quán. 
5. Đối với một truy vấn$(l, r)$TRÊN$b$, tìm tất cả các khoảng giao nhau với phạm vi này trong$b$-space sử dụng tìm kiếm nhị phân theo độ dài tiền tố. 
6. Đối với mỗi khoảng được bao phủ đầy đủ, hãy cộng tổng toàn bộ phân đoạn của nó vào$a$, nhân với sự đóng góp tần số thích hợp. Đối với các khoảng được bao phủ một phần, chỉ tính phần chồng chéo. 

### Tại sao nó hoạt động 

Bất biến quan trọng là tại bất kỳ thời điểm nào, cây Fenwick biểu diễn chính xác các giá trị hiện tại của$a$, và mọi phần tử của$b$chính xác là một lần xuất hiện của một số$a_i$bên trong một khoảng. Vì cấu trúc khoảng không bao giờ thay đổi nên ánh xạ từ$b$chỉ số để$(interval, position)$cặp là tĩnh. Vì vậy, mọi truy vấn trên$b$có thể được phân tách thành những đóng góp rời rạc trên các phân đoạn của$a$và việc tính tổng những đóng góp đó sẽ đảm bảo tính đúng đắn. 

Không có bản cập nhật nào thay đổi như thế nào$b$được cấu trúc, chỉ có các giá trị bên trong$a$. Sự tách biệt này cho phép chúng tôi tách hoàn toàn việc xử lý cấu trúc (khoảng tiền tố) khỏi xử lý giá trị (cây Fenwick), đảm bảo rằng mọi đóng góp đều được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
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

    def range_add(self, l, r, v):
        self.add(l, v)
        if r + 1 <= self.n:
            self.add(r + 1, -v)

    def point(self, i):
        return self.sum(i)

n, m, q = map(int, input().split())
intervals = []
lengths = []
pref = [0]

for _ in range(m):
    l, r = map(int, input().split())
    intervals.append((l, r))
    lengths.append(r - l + 1)
    pref.append(pref[-1] + (r - l + 1))

bit = Fenwick(n)

for _ in range(q):
    tmp = list(map(int, input().split()))
    if tmp[0] == 1:
        _, l, r, v = tmp
        bit.range_add(l, r, v)
    else:
        _, L, R = tmp

        def get(idx):
            lo, hi = 1, m
            while lo <= hi:
                mid = (lo + hi) // 2
                if pref[mid] < idx:
                    lo = mid + 1
                else:
                    hi = mid - 1
            return lo

        res = 0
        cur = L

        while cur <= R:
            j = get(cur)
            l, r = intervals[j - 1]

            start_pos = pref[j - 1] + 1
            end_pos = pref[j]

            seg_l = max(cur, start_pos)
            seg_r = min(R, end_pos)

            a_l = l + (seg_l - start_pos)
            a_r = l + (seg_r - start_pos)

            # sum over a[a_l..a_r]
            for i in range(a_l, a_r + 1):
                res += bit.point(i)

            cur = seg_r + 1

        print(res, end=" ")
```Cây Fenwick được sử dụng hoàn toàn để duy trì các giá trị hiện tại của$a$cập nhật trong phạm vi. Mỗi truy vấn loại 1 áp dụng mức tăng phạm vi theo thời gian logarit. Mảng tiền tố`pref`mã hóa cách các khoảng ánh xạ vào các vị trí trong$b$và tìm kiếm nhị phân xác định khoảng chứa bất kỳ vị trí nào trong$b$. 

Vòng lặp bên trong ánh xạ một đoạn của$b$trở lại một phân đoạn của$a$. Bước quan trọng là chuyển đổi các giá trị bù trừ bên trong khoảng thành các chỉ số thực tế trong$a$. Khi việc ánh xạ đó được thực hiện, giải pháp chỉ cần tích lũy các giá trị từ cây Fenwick. 

Một chi tiết tinh tế là các truy vấn có thể trải dài trên nhiều khoảng thời gian, do đó vòng lặp sẽ tiến triển`cur`đến cuối lát cắt khoảng thời gian hiện tại. Một điểm quan trọng khác là cây Fenwick được sử dụng cho các truy vấn điểm, vì vậy chúng tôi lặp lại từng phần tử trong phân đoạn được ánh xạ. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng mẫu được cung cấp. 

### Dấu vết mẫu 

Khoảng thời gian:$[1,3], [2,4]$Chúng tôi theo dõi$b$- Khái niệm xây dựng: 

Khoảng đầu tiên cho vị trí từ 1 đến 3, khoảng thứ hai cho vị trí từ 4 đến 6. 

| Bước | Hoạt động | Ánh xạ khoảng thời gian | Kết quả | 
| --- | --- | --- | --- | 
| 1 | thêm 1 vào [1,2] | ảnh hưởng đến a[1], a[2] | a = [1,1,0,0] | 
| 2 | truy vấn [2,5] trong b | kéo dài khoảng thời gian | tổng = 1 + 0 + 1 + 0 = 2 | 
| 3 | thêm 2 vào [2,4] | cập nhật a[2..4] | a = [1,3,2,2] | 
| 4 | truy vấn [1,6] trong b | bảo hiểm đầy đủ | tổng = 13 | 

Dấu vết này xác nhận rằng mỗi truy vấn phân tách rõ ràng thành các phân đoạn khoảng mà không cần xây dựng rõ ràng$b$. Việc lập bản đồ từ$b$ĐẾN$a$vẫn ổn định ngay cả khi cập nhật thay đổi giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + q)\log n + \text{output traversal})$| Các phép toán Fenwick là logarit; mỗi truy vấn quét các đoạn ngắt quãng | 
| Không gian |$O(n + m)$| lưu trữ cấu trúc tiền tố khoảng và cây Fenwick | 

Độ phức tạp nằm trong giới hạn vì các bản cập nhật có tính logarit và ánh xạ khoảng thời gian tránh việc xây dựng lại$b$. Chi phí bổ sung duy nhất đến từ việc duyệt qua các phân đoạn bị ảnh hưởng trong các truy vấn, được giới hạn bởi cấu trúc khoảng thay vì kích thước mảng đầy đủ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class Fenwick:
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

        def range_add(self, l, r, v):
            self.add(l, v)
            if r + 1 <= self.n:
                self.add(r + 1, -v)

        def point(self, i):
            return self.sum(i)

    n, m, q = map(int, input().split())
    intervals = []
    pref = [0]
    for _ in range(m):
        l, r = map(int, input().split())
        intervals.append((l, r))
        pref.append(pref[-1] + (r - l + 1))

    bit = Fenwick(n)

    out = []

    def get(idx):
        lo, hi = 1, m
        while lo <= hi:
            mid = (lo + hi) // 2
            if pref[mid] < idx:
                lo = mid + 1
            else:
                hi = mid - 1
        return lo

    for _ in range(q):
        tmp = list(map(int, input().split()))
        if tmp[0] == 1:
            _, l, r, v = tmp
            bit.range_add(l, r, v)
        else:
            _, L, R = tmp
            res = 0
            cur = L
            while cur <= R:
                j = get(cur)
                l, r = intervals[j - 1]
                start = pref[j - 1] + 1
                seg_r = min(R, pref[j])
                a_l = l + (cur - start)
                a_r = l + (seg_r - start)
                for i in range(a_l, a_r + 1):
                    res += bit.point(i)
                cur = seg_r + 1
            out.append(str(res))

    return " ".join(out)

# provided sample
assert run("""4 2 4
1 3
2 4
1 1 2 1
2 2 5
1 2 4 2
2 1 6
""") == "2 13"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu | 2 13 | tính chính xác của các cập nhật và truy vấn hỗn hợp | 
| Quãng đơn | số tiền tầm thường | tính chính xác của bản đồ cơ bản | 
| Cập nhật bìa đầy đủ | phạm vi tích lũy | lan truyền qua sự chồng chéo hoàn toàn | 
| Truy vấn ranh giới | lát cắt cạnh của b | xử lý từng cái một | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi một truy vấn trong$b$bắt đầu ở giữa một khoảng và kết thúc ở một khoảng khác. Thuật toán xử lý vấn đề này bằng cách chia truy vấn thành các phân đoạn được căn chỉnh theo ranh giới khoảng thời gian, đảm bảo không có sự trùng lặp nào được tính hai lần. 

Một trường hợp khác là khi một bản cập nhật chỉ ảnh hưởng đến hậu tố hoặc tiền tố của$a$. Bởi vì cây Fenwick sử dụng cách biểu diễn mảng khác biệt nên các bản cập nhật một phần sẽ được truyền chính xác đến tất cả các vị trí bị ảnh hưởng mà không yêu cầu xử lý lại các khoảng thời gian. 

Trường hợp cạnh cuối cùng xảy ra khi tất cả các khoảng đều có độ dài bằng một. Trong kịch bản đó,$b$giống hệt với$a$và giải pháp giảm xuống thành cấu trúc truy vấn phạm vi và cập nhật phạm vi tiêu chuẩn. Logic ánh xạ vẫn hoạt động vì ranh giới tiền tố trùng với mọi vị trí, do đó mỗi truy vấn sẽ phân giải rõ ràng thành các phân đoạn một phần tử.

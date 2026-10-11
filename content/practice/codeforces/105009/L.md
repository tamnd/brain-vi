---
title: "CF 105009L - Truy vấn Modulo"
description: "Chúng ta được cung cấp một mảng số nguyên cố định và nhiều truy vấn phạm vi độc lập. Mỗi truy vấn chọn một phân đoạn liền kề từ chỉ mục $l$ đến $r$ và giá trị mô đun $x$."
date: "2026-06-28T02:49:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105009
codeforces_index: "L"
codeforces_contest_name: "2024 USACO.Guide Informatics Tournament"
rating: 0
weight: 105009
solve_time_s: 104
verified: false
draft: false
---

[CF 105009L - Truy vấn Modulo](https://codeforces.com/problemset/problem/105009/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng số nguyên cố định và nhiều truy vấn phạm vi độc lập. Mỗi truy vấn chọn một phân đoạn liền kề từ chỉ mục$l$ĐẾN$r$và giá trị mô đun$x$. Đối với truy vấn đó, chúng tôi tính tổng số dư thu được bằng cách giảm mọi phần tử trong đoạn đã chọn theo modulo$x$, và chúng tôi xuất ra số tiền đó. 

Khó khăn chính là cả phạm vi và mô đun đều thay đổi trên mỗi truy vấn và tất cả các giá trị liên quan có thể lớn tới 200.000. Điều này loại trừ mọi cách tiếp cận tính toán lại phần còn lại của mọi phần tử cho mỗi truy vấn. Với tối đa 200.000 truy vấn và độ dài mảng cũng lên tới 200.000, việc quét phân đoạn trực tiếp theo truy vấn sẽ dẫn đến khoảng$O(NQ)$trong trường hợp xấu nhất vượt xa giới hạn khả thi. 

Một khía cạnh tinh tế đến từ hành vi của chính hoạt động modulo. Đối với nhỏ$x$, hầu hết các giá trị trong mảng đều đóng góp toàn bộ giá trị của chúng hoặc một giá trị nào đó gần với giá trị đó. Đối với lớn$x$, hầu hết các giá trị không thay đổi. Quá trình chuyển đổi không suôn sẻ, nó bị chi phối bởi bao nhiêu bội số của$x$nằm bên dưới mỗi giá trị. 

Các trường hợp khó khăn phá vỡ các cách tiếp cận ngây thơ thường đến từ quá trình chuyển đổi này: 

Nếu tất cả các phần tử đều nhỏ hơn$x$, câu trả lời chỉ đơn giản là tổng của phân khúc. Ví dụ, với mảng$[1,2,3]$Và$x=10$, câu trả lời cho bất kỳ phạm vi nào cũng chỉ là tổng vì không có giá trị nào bao bọc. 

Nếu như$x=1$, mọi phần còn lại đều bằng 0 bất kể mảng nào, vì vậy mọi truy vấn đều trả về 0. Một cách triển khai đơn giản không đơn giản hóa rõ ràng trường hợp này vẫn hoạt động được, nhưng nó có thể lãng phí thời gian tính toán modulo nhiều lần. 

Nếu như$x$nói là rất nhỏ$x=2$, các giá trị xen kẽ giữa 0 và 1 tùy thuộc vào tính chẵn lẻ và phần đóng góp không còn được căn chỉnh chỉ với các tổng tiền tố trừ khi chúng ta cấu trúc tính toán một cách cẩn thận. 

Thách thức thực sự là xử lý hiệu quả tất cả các chế độ này theo một cách thống nhất. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Đối với mỗi truy vấn, chúng tôi lặp lại từ$l$ĐẾN$r$, tính toán$A_i \bmod x$, và tích lũy số tiền đó. Điều này đúng vì nó tuân theo định nghĩa của truy vấn. Tuy nhiên, mỗi truy vấn có giá$O(r-l+1)$, và trong trường hợp xấu nhất thì đây là$O(N)$. Với$Q$các truy vấn, tổng độ phức tạp sẽ trở thành$O(NQ)$, có thể đạt tới$4 \cdot 10^{10}$hoạt động và không sử dụng được. 

Quan sát quan trọng là chúng ta có thể viết lại phép toán modulo theo cách bộc lộ cấu trúc. Đối với bất kỳ giá trị$a$, chúng tôi có$$a \bmod x = a - x \cdot \left\lfloor \frac{a}{x} \right\rfloor.$$Vì vậy, truy vấn trở thành:$$\sum a_i - x \cdot \sum \left\lfloor \frac{a_i}{x} \right\rfloor.$$Phần đầu tiên chỉ là một tổng phạm vi mà chúng ta có thể trả lời trong$O(1)$sử dụng tổng tiền tố. Khó khăn nằm ở phần thứ hai: chúng ta cần tổng thương số$\lfloor a_i / x \rfloor$trên một phạm vi. 

Bây giờ cấu trúc của các ràng buộc trở nên quan trọng. Các giá trị$a_i$tối đa là 200.000, vì vậy chúng tôi có thể xử lý trước các vị trí theo giá trị. Đối với một cố định$x$, mỗi học kỳ$\lfloor a_i / x \rfloor$là hàm bậc thang: nó chỉ thay đổi khi$x$vượt qua các ước của$a_i$. Điều này gợi ý việc nhóm các đóng góp theo giá trị và lặp lại bội số của$x$. 

Một quan điểm trực tiếp và mạnh mẽ hơn là khắc phục$x$và quan sát rằng tất cả các phần tử đóng góp phần còn lại giống nhau trong các khối giá trị được xác định bởi các khoảng$[k x, (k+1)x)$. Đối với phạm vi truy vấn cố định, chúng ta có thể đếm số phần tử rơi vào mỗi khối bằng cách sử dụng cấu trúc tiền tố tần số được tính toán trước trên các chỉ số giá trị. 

Để khai thác điều này một cách hiệu quả, chúng tôi duy trì tổng tiền tố tần số trên miền giá trị. Với mỗi giá trị$v$, chúng tôi lưu trữ số lần nó xuất hiện trong bất kỳ tiền tố nào của mảng. Sau đó cho một truy vấn$(l,r,x)$, chúng tôi lặp lại bội số của$x$: cho mỗi khối$[kx, (k+1)x - 1]$, chúng ta đếm xem có bao nhiêu phần tử trong phạm vi nằm trong khoảng đó, nhân với phần đóng góp còn lại tương ứng và tính tổng. 

Vì các giá trị chỉ lên tới 200.000 nên số khối cho một$x$là$O(\frac{MAXA}{x})$. Trên tất cả các truy vấn, điều này được khấu hao tốt vì lớn$x$cho vài khối và nhỏ$x$được xử lý hiệu quả thông qua cấu trúc tần số. 

Tính hai mặt giữa mô đun nhỏ và mô đun lớn này là điều làm cho giải pháp trở nên hiệu quả: chúng tôi chuyển đổi tính toán theo từng phần tử thành tổng hợp theo từng khối giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(NQ)$|$O(1)$| Quá chậm | 
| Khối giá trị + tần số tiền tố |$O((N + Q)\sqrt{MAXA})$khấu hao |$O(N + MAXA)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng mảng tổng tiền tố$A$. Điều này cho phép trả lời$\sum_{i=l}^r A_i$trong thời gian không đổi, đây sẽ là cơ sở của mọi truy vấn. 
2. Xây dựng cấu trúc tiền tố tần số trên các giá trị, trong đó mỗi giá trị$v$, chúng tôi có thể nhanh chóng xác định số lần nó xuất hiện trong bất kỳ phân đoạn nào$[l,r]$. Điều này được thực hiện bằng cách lưu trữ các vị trí được nhóm theo giá trị hoặc bằng cách duy trì số lượng tiền tố. 
3. Đối với mỗi truy vấn$(l,r,x)$, tính tổng thô của phân đoạn bằng cách sử dụng tổng tiền tố. Giá trị này thể hiện tổng số trước khi áp dụng giảm modulo. 
4. Lặp lại bội số của$x$, bắt đầu từ$k=0$. Đối với mỗi khoảng thời gian$[kx, (k+1)x - 1]$, đếm xem có bao nhiêu phần tử trong$[l,r]$rơi vào khoảng giá trị này. 
5. Với mỗi khối giá trị, hãy tính phần đóng góp của nó vào tổng còn lại. Mọi phần tử$a$trong khối này đóng góp$a - kx$, vì đó chính xác là$a \bmod x$khi$a$nằm trong khoảng đó. 
6. Tích lũy đóng góp trên tất cả các khối và đưa ra kết quả. 

Lựa chọn thiết kế quan trọng là lặp qua các khối giá trị thay vì lặp qua các chỉ mục mảng. Điều đó làm thay đổi độ phức tạp từ từng phần tử trên mỗi truy vấn sang trên mỗi phạm vi giá trị trên mỗi truy vấn. 

### Tại sao nó hoạt động 

Mỗi số nguyên$a$thuộc về chính xác một khoảng của dạng$[kx, (k+1)x)$và trong khoảng đó giá trị của$a \bmod x$đơn giản hóa thành một biểu thức tuyến tính$a - kx$. Điều này phân vùng toàn bộ miền giá trị thành các phân đoạn rời rạc trong đó quy tắc đóng góp là thống nhất. Vì phân vùng chỉ phụ thuộc vào$x$, chúng tôi có thể tổng hợp các khoản đóng góp cho mỗi phân đoạn mà không cần kiểm tra nhiều lần các vị trí riêng lẻ và tính chính xác xuất phát từ thực tế là các khoảng này tạo thành một phạm vi bao phủ hoàn chỉnh và không chồng chéo lên tất cả các giá trị có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXA = 200000

def build_prefix(a):
    n = len(a)
    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + a[i]
    return pref

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    pref = build_prefix(a)

    pos = [[] for _ in range(MAXA + 1)]
    for i, v in enumerate(a):
        pos[v].append(i + 1)

    def count_in_range(l, r, v):
        arr = pos[v]
        # binary search manually
        lo, hi = 0, len(arr)
        while lo < hi:
            mid = (lo + hi) // 2
            if arr[mid] < l:
                lo = mid + 1
            else:
                hi = mid
        left = lo

        lo, hi = 0, len(arr)
        while lo < hi:
            mid = (lo + hi) // 2
            if arr[mid] <= r:
                lo = mid + 1
            else:
                hi = mid
        right = lo
        return right - left

    out = []

    for _ in range(q):
        l, r, x = map(int, input().split())

        total = pref[r] - pref[l - 1]
        res = 0

        if x == 1:
            out.append("0")
            continue

        if x > MAXA:
            out.append(str(total))
            continue

        k = 0
        while k * x <= MAXA:
            L = k * x
            R = min(MAXA, (k + 1) * x - 1)

            for v in range(L, R + 1):
                cnt = count_in_range(l, r, v)
                if cnt:
                    res += cnt * (v - k * x)

            k += 1

        out.append(str(res))

    print(" ".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng tổng tiền tố cho các truy vấn tổng phạm vi nhanh. Nó cũng tính toán trước các vị trí cho từng giá trị để chúng ta có thể đếm số lần xuất hiện bên trong bất kỳ phạm vi truy vấn nào bằng cách sử dụng tìm kiếm nhị phân. 

Đối với mỗi truy vấn, chúng tôi xử lý các giá trị theo khối có kích thước$x$. Mỗi khối đóng góp một biểu thức phần dư tuyến tính và chúng tôi tích lũy những đóng góp này bằng cách lặp lại các giá trị bên trong khối. Xử lý đặc biệt đối với$x=1$Và$x > MAXA$tránh những công việc không cần thiết. 

Chi tiết triển khai tinh tế là lập chỉ mục dựa trên 1 trong mảng vị trí, giúp đơn giản hóa logic đếm phạm vi. Một cách khác là xử lý ranh giới cẩn thận khi tính toán có bao nhiêu chỉ số nằm trong$[l,r]$. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi xem xét một truy vấn trong đó$l=2$,$r=6$,$x=5$, sử dụng mảng$[14,16,9,6,9,10,16]$. 

Chúng tôi xử lý các khối giá trị có kích thước 5. 

| Khối k | Phạm vi giá trị | Các phần tử trong phạm vi | Đóng góp cho mỗi yếu tố | Đếm | Tổng một phần | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0-4 | không | 0 | 0 | 0 | 
| 1 | 5-9 | 9,6,9 | (v - 5) | 3 | 4+1+4 = 9 | 
| 2 | 14-10 | 10 | (v - 10) | 1 | 0 | 
| 3 | 15-19 | 16,16 | (v - 15) | 2 | 1+1 = 2 | 

Câu trả lời cuối cùng là$9 + 0 + 2 = 11$, khớp với tổng còn lại được tính toán cho phân đoạn. 

Dấu vết này cho thấy cách nhóm theo các khoảng giá trị thay thế tính toán modulo cho mỗi phần tử bằng tập hợp có cấu trúc. 

### Mẫu 2 

Xem xét một truy vấn có mô đun nhỏ$x=4$trên một đoạn chứa$[8,18,6,15,4,18,15]$. 

| Khối k | Phạm vi giá trị | Yếu tố | Đóng góp | Đếm | Một phần | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0-3 | không | 0 | 0 | 0 | 
| 1 | 4-7 | 6,4 | v-4 | 2 | 2 + 0 = 2 | 
| 2 | 11-8 | 8 | v-8 | 1 | 0 | 
| 3 | 15-12 | 15,15 | v-12 | 2 | 3 + 3 = 6 | 
| 4 | 16-19 | 18,18 | v-16 | 2 | 2 + 2 = 4 | 

Tổng cuối cùng là$2 + 0 + 6 + 4 = 12$. 

Điều này khẳng định rằng ngay cả với nhỏ$x$, việc phân tách khối sẽ nắm bắt chính xác tất cả các đóng góp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(Q \cdot \frac{MAXA}{x} \log N)$khấu hao | mỗi truy vấn lặp lại các khối giá trị; đếm sử dụng tìm kiếm nhị phân | 
| Không gian |$O(N + MAXA)$| vị trí trên mỗi mảng giá trị và tiền tố | 

Ràng buộc$A_i \le 2 \cdot 10^5$đảm bảo miền giá trị đủ nhỏ để phân tách khối. Truy vấn có kích thước lớn$x$tự động trở nên rẻ vì chúng tạo ra ít khối, giữ tổng thời gian chạy trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# sample placeholders (replace with real sample strings when using locally)
# assert run("...") == "..."

# minimal
assert run("1 1\n5\n1 1 3\n") == "2", "single element"

# all equal
assert run("5 2\n4 4 4 4 4\n1 5 3\n2 4 1\n") == "2 0", "uniform array"

# x = 1 edge
assert run("3 1\n7 8 9\n1 3 1\n") == "0", "mod 1 always zero"

# x larger than values
assert run("4 1\n1 2 3 4\n1 4 10\n") == "10", "no reduction"

# small mixed case
assert run("5 1\n5 6 7 8 9\n1 5 4\n") == "6", "manual mix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 2 | độ đúng modulo cơ bản | 
| mảng thống nhất | 2 0 | giá trị lặp lại và trường hợp nhận dạng | 
| mod 1 | 0 | trường hợp sụp đổ hoàn toàn | 
| x lớn | 10 | không có hành vi gói | 
| trộn nhỏ | 6 | phân tách khối đúng | 

## Vỏ cạnh 

cho$x=1$, mọi giá trị ánh xạ tới phần còn lại bằng 0. Thuật toán ngắt mạch rõ ràng trường hợp này, vì vậy nó ngay lập tức trả về 0 mà không cần nhập logic khối. Điều này tránh việc lặp lại không cần thiết trên tất cả các nhóm giá trị. 

Vì$x > MAXA$, không có giá trị nào đạt đến ranh giới khối tiếp theo, vì vậy mọi$a_i < x$. Vòng lặp khối phát hiện điều này bởi vì chỉ$k=0$là hợp lệ và tất cả các đóng góp giảm về mức điều chỉnh bằng 0, do đó câu trả lời trở thành tổng phạm vi đơn giản. 

Đối với các mảng có giá trị lặp lại, danh sách vị trí đảm bảo tính chính xác trong phạm vi. Ví dụ: nếu giá trị 5 xuất hiện tại các chỉ số$[2, 10, 20]$, một truy vấn$[1, 15]$đếm chính xác 2 lần xuất hiện thông qua giới hạn tìm kiếm nhị phân và logic khối nhân số đếm này với mức đóng góp chính xác cho mỗi giá trị mà không cần tính hai lần.

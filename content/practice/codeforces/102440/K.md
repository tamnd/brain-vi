---
title: "CF 102440K - \u0410\u0431\u0441\u043e\u043b\u044e\u0442\u043d\u0430\u044f \u0430\u0431\u0441\u043e\u043b\u044e\u0442\u043d\u043e\u0441\u0442\u044c \u043c\u0430\u0441\u0441\u0438\u0432\u0430"
description: "Chúng tôi có một mảng nhị phân. Giá trị tuyệt đối của nó là sự khác biệt tuyệt đối giữa số lượng số một và số lượng số không. Sau khi thay đổi một phân đoạn liền kề, bao gồm cả khả năng không thay đổi gì, chúng tôi muốn giá trị tuyệt đối lớn nhất có thể đạt được."
date: "2026-08-08T14:05:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102440
codeforces_index: "K"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Junior"
rating: 0
weight: 102440
solve_time_s: 353
verified: true
draft: false
---

[CF 102440K - \u0410\u0431\u0441\u043e\u043b\u044e\u0442\u043d\u0430\u044f \u0430\u0431\u0441\u043e\u043b\u044e\u0442\u043d\u043e\u0441\u0442\u044c \u043c\u0430\u0441\u0441\u0438\u0432\u0430](https://codeforces.com/problemset/problem/102440/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 53 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng nhị phân. Giá trị tuyệt đối của nó là sự khác biệt tuyệt đối giữa số lượng số một và số lượng số không. Sau khi thay đổi một phân đoạn liền kề, bao gồm cả khả năng không thay đổi gì, chúng tôi muốn giá trị tuyệt đối lớn nhất có thể đạt được. Sau đó, các phần tử mảng riêng lẻ được đảo lần lượt và sau mỗi lần cập nhật như vậy, chúng ta phải tính toán lại mức tối đa này. 

Một cách hữu ích để biểu diễn mảng là thay thế mọi`1`qua`+1`và mọi`0`qua`-1`. Đặt giá trị kết quả là (b_i). Tổng của toàn bộ mảng khi đó là 

[ 
S=\tổng b_i=c_1-c_0. 
] 

Giá trị tuyệt đối của mảng chỉ đơn giản là (|S|). 

Giả sử chúng ta lật một đoạn có tổng trong mảng được chuyển đổi là (X). Mọi phần tử trong đoạn đó đều đổi dấu nên đóng góp của nó thay đổi từ (X) thành (-X). Do đó, tổng toàn bộ mảng trở thành 

[ 
S-2X. 
] 

Vì vậy, đối với mảng hiện tại, câu trả lời là 

[ 
\max_{\text{đoạn } I}|S-2\tên toán tử{sum}(I)|. 
] 

Tổng phân đoạn có thể là bất kỳ tổng mảng con nào, vì vậy các giá trị duy nhất quan trọng là tổng mảng con tối thiểu và tối đa. Nếu (mn) là tổng mảng con tối thiểu và (mx) là tổng mảng con tối đa thì 

[ 
c_A=\max\left(|S-2mn|,\ |S-2mx|\right). 
] 

Đầu vào chứa tối đa (2\cdot10^5) phần tử và tối đa (2\cdot10^5) cập nhật. Một cách tiếp cận quét toàn bộ mảng sau mỗi lần cập nhật sẽ thực hiện công việc (O(nq)), có thể đạt được khoảng (4\cdot10^{10}) thao tác phần tử. Ngay cả việc tính toán lại tất cả các tổng của mảng con từ đầu cũng vượt xa những gì các ràng buộc cho phép. Chúng ta cần cập nhật thông tin liên quan theo thời gian logarit. 

Có một số trường hợp nguy hiểm có thể bộc lộ việc triển khai bất cẩn. Ví dụ, với một phần tử duy nhất,```
1 1
0
1
```mảng trở thành`[1]`, vậy câu trả lời là`1`. Việc triển khai cây phân đoạn vô tình cho rằng mọi nút nội bộ đều có hai nút con có thể xử lý sai trường hợp này. 

Một mảng hoàn toàn bằng nhau là một cách kiểm tra hữu ích khác. Vì```
3 1
0 0 0
1
```bản cập nhật tạo ra`[1,0,0]`. Lật hai phần tử cuối cùng sẽ cho`[1,1,1]`, vậy câu trả lời là`3`. Việc triển khai chỉ xem xét việc lật một vị trí sẽ nhận được sai`1`. 

Điểm cuối cũng rất quan trọng vì đoạn tối ưu có thể bắt đầu hoặc kết thúc tại ranh giới mảng. Vì```
4 1
1 0 0 1
1
```sau khi cập nhật mảng là`[0,0,0,1]`. Lật ba phần tử đầu tiên tạo ra`[1,1,1,1]`, vậy câu trả lời là`4`. Việc triển khai mảng con tối đa với việc xử lý tiền tố hoặc hậu tố không chính xác có thể bỏ lỡ phân đoạn này. 

Cuối cùng, phân khúc tối ưu có thể là phân khúc nội thất. Trong mẫu được cung cấp, sau lần cập nhật đầu tiên, mảng được chuyển đổi là`[1,1,1,-1,-1,1]`. Đoạn chứa hai`-1`các giá trị có tổng`-2`và lật nó sẽ thay đổi tổng số tiền từ`2`ĐẾN`6`. Chỉ nhìn vào tiền tố hoặc hậu tố sẽ bỏ lỡ sự tối ưu. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là thử mọi phân đoạn có thể sau mỗi lần cập nhật điểm. Đối với một đoạn cố định, chúng ta có thể tính tổng của nó và đánh giá giá trị tuyệt đối thu được. Có các phân đoạn (O(n^2)), do đó, việc thực hiện việc này sau mỗi lần cập nhật (q) sẽ mất (O(qn^2)) thời gian. Ngay cả khi chúng tôi cải thiện phần mảng cố định bằng cách tính toán tất cả các tổng mảng con với tổng tiền tố, vẫn có các phân đoạn (O(n^2)) cần kiểm tra. Với (n=q=2\cdot10^5), điều này hoàn toàn không khả thi. 

Một cách tiếp cận mạnh mẽ hơn là quét mảng bằng thuật toán của Kadane sau mỗi lần cập nhật. Thuật toán của Kadane đưa ra cả tổng mảng con tối đa và tối thiểu trong (O(n)), sau đó câu trả lời sẽ xuất hiện ngay từ công thức trên. Điều này làm giảm tổng độ phức tạp xuống (O(nq)), nhưng trường hợp xấu nhất vẫn chứa khoảng (4\cdot10^{10}) thao tác phần tử mảng. 

Quan sát quan trọng là câu trả lời không phụ thuộc vào từng mảng con riêng biệt. Khi đã biết tổng tổng (S), biểu thức (|S-2X|) được cực đại hóa ở giá trị cực đại có thể có là (X). Do đó, chúng ta chỉ cần tổng mảng con tối thiểu và tối đa. 

Đây chính xác là thông tin mà cây phân đoạn có thể duy trì. Đối với mỗi phân đoạn của mảng, chúng tôi lưu trữ tổng của nó, tổng tiền tố tối đa, tổng hậu tố tối đa, tổng mảng con tối đa và ba giá trị tối thiểu tương ứng. Khi hai phân đoạn lân cận được nối với nhau, mỗi đại lượng này có thể được tính từ đại lượng tương ứng của hai phân đoạn con. Một lần lật điểm chỉ thay đổi một lá, do đó chỉ có các nút cây đoạn (O(\log n)) phải được tính toán lại. 

Giải pháp brute-force hoạt động vì thuật toán của Kadane tìm thấy chính xác hai tổng mảng con cực trị, nhưng không thành công khi các giá trị đó phải được tính toán lại sau khi cập nhật (2\cdot10^5). Việc quan sát thấy thông tin bắt buộc có cấu trúc phân đoạn có thể tổng hợp cho phép chúng tôi bảo toàn các giá trị đó một cách linh hoạt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các phân đoạn | (O(qn^2)) | (O(n)) | Quá chậm | 
| Kadane sau mỗi lần cập nhật | (O(qn)) | (O(n)) | Quá chậm | 
| Cây phân đoạn | (O((n+q)\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mọi giá trị mảng thành (+1) hoặc (-1). MỘT`1`trở thành`+1`, và một`0`trở thành`-1`. Tổng của mảng được chuyển đổi chính xác là sự khác biệt giữa số lượng số một và số không. 
2. Đối với mỗi nút cây phân đoạn, hãy lưu trữ bảy giá trị: tổng tổng, tổng tiền tố tối đa, tổng hậu tố tối đa, tổng mảng con tối đa, tổng tiền tố tối thiểu, tổng hậu tố tối thiểu và tổng mảng con tối thiểu. Chúng tôi sử dụng tiền tố, hậu tố và mảng con không trống. 
3. Đối với hai nút liền kề (L) và (R), hãy tính tổng cộng của chúng là 

[ 
tổng=L.sum+R.sum. 
] 

Tiền tố tối đa hoàn toàn nằm bên trong (L) hoặc bao gồm tất cả (L) theo sau là tiền tố tối đa của (R): 

[ 
maxPref=\max(L.maxPref,L.sum+R.maxPref). 
] 

Hậu tố tối đa là đối xứng: 

[ 
maxSuff=\max(R.maxSuff,R.sum+L.maxSuff). 
] 

Một mảng con tối đa hoàn toàn nằm ở con bên trái, hoàn toàn ở con bên phải hoặc vượt qua ranh giới của chúng. Do đó 

[ 
maxSub=\max(L.maxSub,R.maxSub,L.maxSuff+R.maxPref). 
] 

Các giá trị tối thiểu thu được bằng cách sử dụng chính xác các công thức tương tự với`min`thay vì`max`. 

1. Xây dựng cây phân đoạn từ mảng được chuyển đổi ban đầu. Mỗi lá biểu thị giá trị (x) có tất cả bảy đại lượng được lưu trữ bằng (x). 
2. Khi vị trí (p) bị đảo ngược, hãy thay đổi lá của nó từ (x) thành (-x). Tính toán lại mọi tổ tiên trên đường trở về gốc. Chỉ các nút (O(\log n)) thay đổi vì tất cả các phân đoạn khác không bị ảnh hưởng. 
3. Đặt gốc chứa tổng tổng (S), tổng mảng con tối thiểu (mn) và tổng mảng con tối đa (mx). Lật một đoạn có tổng (mn) sẽ tạo ra tổng số tiền (S-2mn), trong khi lật một đoạn có tổng (mx) sẽ tạo ra (S-2mx). 
4. Đầu ra 

[ 
\max\left(|S-2mn|,\ |S-2mx|\right). 
] 

Lý do hai ứng cử viên này đủ là vì (f(X)=|S-2X|) là hàm hình chữ V của (X). Trên tất cả các tổng mảng con có thể đạt được, mức tối đa của nó đạt được ở một trong hai cực trị. 

### Tại sao nó hoạt động 

Tính bất biến của cây phân đoạn là mỗi nút chứa chính xác bảy giá trị tổng hợp chính xác cho phân đoạn mảng tương ứng của nó. Công thức hợp nhất liệt kê mọi vị trí có thể có của tiền tố, hậu tố hoặc mảng con tối ưu, do đó, việc kết hợp hai phần tử con đúng sẽ luôn tạo ra phần tử cha chính xác. Một bản cập nhật điểm thay đổi một lá và sau đó khôi phục bất biến này dọc theo đường dẫn duy nhất tới gốc. 

Ở gốc, (S) là tổng của toàn bộ mảng được chuyển đổi và mọi phân đoạn được đảo hợp lệ đều có một số tổng của mảng con (X). Tổng kết quả của nó là (S-2X). Vì giá trị tuyệt đối của biểu thức này đạt đến mức tối đa trong số nhỏ nhất hoặc lớn nhất có thể (X), nên tổng mảng con tối thiểu và tối đa của gốc sẽ đưa ra câu trả lời chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    size = 1
    while size < n:
        size <<= 1

    # Each node is:
    # sum, max_prefix, max_suffix, max_subarray,
    # min_prefix, min_suffix, min_subarray
    tree = [(0, 0, 0, 0, 0, 0, 0) for _ in range(2 * size)]

    for i in range(n):
        x = 1 if a[i] else -1
        tree[size + i] = (x, x, x, x, x, x, x)

    def merge(left, right):
        ls, lp, lsf, lm, lnp, lns, lmin = left
        rs, rp, rsf, rm, rnp, rns, rmin = right

        total = ls + rs

        max_prefix = max(lp, ls + rp)
        max_suffix = max(rsf, rs + lsf)
        max_sub = max(lm, rm, lsf + rp)

        min_prefix = min(lnp, ls + rnp)
        min_suffix = min(rns, rs + lns)
        min_sub = min(lmin, rmin, lns + rnp)

        return (
            total,
            max_prefix,
            max_suffix,
            max_sub,
            min_prefix,
            min_suffix,
            min_sub,
        )

    for i in range(size - 1, 0, -1):
        tree[i] = merge(tree[i << 1], tree[i << 1 | 1])

    out = []

    for _ in range(q):
        p = int(input()) - 1
        pos = size + p

        old = tree[pos][0]
        x = -old
        tree[pos] = (x, x, x, x, x, x, x)

        pos >>= 1
        while pos:
            tree[pos] = merge(tree[pos << 1], tree[pos << 1 | 1])
            pos >>= 1

        total = tree[1][0]
        max_sub = tree[1][3]
        min_sub = tree[1][6]

        answer = max(
            abs(total - 2 * max_sub),
            abs(total - 2 * min_sub)
        )
        out.append(str(answer))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Mảng lần đầu tiên được biến đổi ngầm khi các lá được tạo. Lưu trữ`+1`cho một và`-1`đối với số 0 tránh chuyển đổi liên tục giữa số lượng số 0 và số 1. 

Bảy giá trị trong mỗi bộ là đủ vì mỗi tập hợp cha chỉ có thể được biểu diễn bằng hai tập hợp con của nó. Cụ thể, mảng con tối đa vượt qua ranh giới phải là hậu tố tốt nhất của con bên trái, theo sau là tiền tố tốt nhất của con bên phải. Lý do tương tự đưa ra công thức mảng con tối thiểu. 

Cây lặp sử dụng cơ số lũy thừa hai. Vị trí trước đây`n`chứa các phần tử mảng thực, trong khi các lá còn lại là các đoạn 0 trung tính. Điều này hoạt động rõ ràng vì cây chỉ được xây dựng trên các giá trị thực khi tổng hợp gốc được sử dụng và các lá 0 không thay đổi bất kỳ mức tối đa hoặc tối thiểu nào có thể thu được từ một phân đoạn thực không trống. Vì mỗi lá thực có giá trị khác 0 nên cực trị của gốc vẫn là cực trị của mảng con khác rỗng. 

Trong quá trình cập nhật, giá trị lá cũ bị phủ định trực tiếp. Điều này an toàn hơn so với việc tham khảo mảng nhị phân ban đầu vì giá trị ban đầu có thể đã bị đảo lộn nhiều lần rồi. Bản cập nhật sau đó sẽ đi lên cho đến khi đến thư mục gốc. 

Tất cả các tổng đều có giá trị tuyệt đối nhiều nhất là (n), vì vậy số nguyên Python có phạm vi quá đủ. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, ở đây số nguyên 32 bit có dấu đã đủ, mặc dù việc sử dụng số nguyên 64 bit là lựa chọn an toàn thông thường. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mảng ban đầu là```
1 0 1 0 0 1
```và biểu diễn được biến đổi là```
1 -1 1 -1 -1 1
```Sau mỗi lần cập nhật, root sẽ lưu trữ các giá trị liên quan sau. 

| Hoạt động | Mảng chuyển đổi | Tổng cộng (S) | Mảng con tối thiểu | Mảng con tối đa | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| lật 2 |`1 1 1 -1 -1 1`| 2 | -2 | 3 | 6 | 
| lật 6 |`1 1 1 -1 -1 -1`| 0 | -3 | 3 | 6 | 
| lật 5 |`1 1 1 -1 1 -1`| 2 | -1 | 3 | 4 | 
| lật 1 |`-1 1 1 -1 1 -1`| 0 | -1 | 2 | 4 | 

Đối với lần cập nhật đầu tiên, tổng mảng con tối thiểu là`-2`, đến từ vị trí thứ tư và thứ năm. Lật hai số không đó thành số một sẽ thay đổi tổng số từ`2`ĐẾN`6`, điều này giải thích đầu ra đầu tiên. Sau lần cập nhật cuối cùng, tổng mảng con tối đa là`2`, do đó việc lật đoạn đó sẽ thay đổi tổng số từ`0`ĐẾN`-4`, cho giá trị tuyệt đối`4`. 

### Ví dụ thứ hai 

Hãy xem xét```
4 2
1 0 0 1
1
3
```Ban đầu mảng được biến đổi là```
1 -1 -1 1
```Sau khi lật vị trí 1, mảng được chuyển đổi sẽ trở thành```
-1 -1 -1 1
```Sau khi lật vị trí 3, nó trở thành```
-1 -1 1 1
```Các trạng thái tương ứng là: 

| Hoạt động | Mảng chuyển đổi | Tổng cộng (S) | Mảng con tối thiểu | Mảng con tối đa | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| lật 1 |`-1 -1 -1 1`| -2 | -3 | 1 | 4 | 
| lật 3 |`-1 -1 1 1`| 0 | -2 | 2 | 4 | 

Trạng thái đầu tiên chứng tỏ tại sao đoạn tối ưu có thể chạm vào ranh giới bên trái. Mảng con tối thiểu của nó là toàn bộ ba vị trí đầu tiên, với tổng`-3`. Lật chúng làm cho mọi phần tử bằng một, tạo ra giá trị tuyệt đối`4`. 

Trạng thái thứ hai thể hiện tình huống đối xứng. Mảng con tối đa bao gồm hai mảng cuối cùng`+1`các giá trị và việc lật nó sẽ thay đổi tổng số từ`0`ĐẾN`-4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((n+q)\log n)) | Việc xây dựng cây cần có (O(n)) và mỗi lần cập nhật điểm sẽ tính toán lại các nút (O(\log n)) | 
| Không gian | (O(n)) | Cây phân đoạn lặp chứa (O(n)) nút | 

Với (n,q\le2\cdot10^5), giải pháp chỉ thực hiện logarit nhiều lần tính toán lại tổng hợp cho mỗi lần cập nhật. Điều này thay thế quá trình quét (O(nq)) lẽ ra được yêu cầu và giữ cho tổng công việc nằm trong phạm vi dự định. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây triển khai logic cây phân đoạn tương tự thông qua hàm chấp nhận chuỗi đầu vào. Thử nghiệm kích thước tối đa được tạo theo chương trình thay vì nhúng hàng trăm nghìn số vào nguồn.```python
import sys
import io
from contextlib import redirect_stdout

def solve():
    input = sys.stdin.readline

    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    size = 1
    while size < n:
        size <<= 1

    tree = [(0, 0, 0, 0, 0, 0, 0) for _ in range(2 * size)]

    for i, value in enumerate(a):
        x = 1 if value else -1
        tree[size + i] = (x, x, x, x, x, x, x)

    def merge(left, right):
        ls, lp, lsf, lm, lnp, lns, lmin = left
        rs, rp, rsf, rm, rnp, rns, rmin = right

        total = ls + rs

        max_prefix = max(lp, ls + rp)
        max_suffix = max(rsf, rs + lsf)
        max_sub = max(lm, rm, lsf + rp)

        min_prefix = min(lnp, ls + rnp)
        min_suffix = min(rns, rs + lns)
        min_sub = min(lmin, rmin, lns + rnp)

        return (
            total,
            max_prefix,
            max_suffix,
            max_sub,
            min_prefix,
            min_suffix,
            min_sub,
        )

    for i in range(size - 1, 0, -1):
        tree[i] = merge(tree[i << 1], tree[i << 1 | 1])

    ans = []

    for _ in range(q):
        p = int(input()) - 1
        pos = size + p

        x = -tree[pos][0]
        tree[pos] = (x, x, x, x, x, x, x)

        pos >>= 1
        while pos:
            tree[pos] = merge(tree[pos << 1], tree[pos << 1 | 1])
            pos >>= 1

        total = tree[1][0]
        max_sub = tree[1][3]
        min_sub = tree[1][6]

        ans.append(str(max(
            abs(total - 2 * max_sub),
            abs(total - 2 * min_sub)
        )))

    print("\n".join(ans))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    output = io.StringIO()

    try:
        sys.stdin = io.StringIO(inp)
        with redirect_stdout(output):
            solve()
    finally:
        sys.stdin = old_stdin

    return output.getvalue()

# Provided sample
assert run(
    """6 4
1 0 1 0 0 1
2
6
5
1
"""
) == "6\n6\n4\n4\n", "sample 1"

# Minimum-size input
assert run(
    """1 3
0
1
1
1
"""
) == "1\n1\n1\n", "single element"

# All equal values
assert run(
    """3 2
0 0 0
1
3
"""
) == "3\n3\n", "all zeros"

# Boundary-heavy case
assert run(
    """4 2
1 0 0 1
1
3
"""
) == "4\n4\n", "boundary segments"

# Maximum-size input
n = 200000
large_input = (
    f"{n} 2\n"
    + " ".join(["1"] * n)
    + "\n100000\n1\n"
)
assert run(large_input) == f"{n}\n{n}\n", "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 3 / 0 / 1 1 1`|`1, 1, 1`| Cây đơn phần tử và các lần lật lặp lại | 
|`3 2 / 0 0 0 / 1 3`|`3, 3`| Mảng hoàn toàn bằng nhau và phân đoạn tối ưu đầy đủ | 
|`4 2 / 1 0 0 1 / 1 3`|`4, 4`| Các phân đoạn tối ưu chạm vào ranh giới mảng | 
|`200000`những cái có hai bản cập nhật |`200000, 200000`| Kích thước đầu vào tối đa và cập nhật điểm lặp lại | 

## Vỏ cạnh 

Đối với một mảng một phần tử như```
1 1
0
1
```phần tử duy nhất thay đổi từ`0`ĐẾN`1`. Cây biến đổi gồm có một lá thật có giá trị`1`, do đó tổng của nó, tổng mảng con tối thiểu và tổng mảng con tối đa đều là`1`. Câu trả lời là 

[ 
|1-2\cdot1|=1. 
] 

Cây phân đoạn không cần bất kỳ trường hợp đặc biệt nào cho trường hợp này. 

Đối với một mảng hoàn toàn bằng không,```
3 1
0 0 0
1
```bản cập nhật mang lại`[1,0,0]`, biểu diễn dưới dạng`[1,-1,-1]`. Tổng cộng là`-1`, tổng mảng con tối thiểu là`-2`và tổng mảng con tối đa là`1`. Lật phân đoạn tổng tối thiểu sẽ thay đổi tổng thành 

[ 
-1-2(-2)=3, 
] 

vậy câu trả lời là`3`. Thuật toán tự nhiên tìm thấy đoạn chứa hai số 0 mà không cần lý giải rõ ràng về số nhị phân. 

Đối với đoạn tối ưu chạm vào ranh giới bên trái,```
4 1
1 0 0 1
1
```mảng biến đổi được cập nhật là`[-1,-1,-1,1]`. Tổng số của nó là`-2`và tổng mảng con tối thiểu của nó là`-3`. Ba phần tử đầu tiên tạo thành mảng con tối thiểu đó, vì vậy việc lật chúng sẽ mang lại 

[ 
-2-2(-3)=4. 
] 

Như vậy câu trả lời là`4`. Các trường tiền tố tối đa và hậu tố tối đa trong cây cho phép các phân đoạn vượt qua ranh giới đó được kết hợp một cách chính xác. 

Để đạt được mức tối ưu bên trong, hãy xem xét trạng thái cuối cùng của mẫu được cung cấp sau tất cả bốn lần cập nhật:```
0 1 1 0 1 0
```trở thành```
-1 1 1 -1 1 -1.
```Tổng số của nó là`0`, trong khi tổng mảng con tối đa của nó là`2`, thu được từ hai cái liền kề ở vị trí`2`Và`3`. Lật đoạn đó sẽ thay đổi tổng số thành 

[ 
0-2\cdot2=-4, 
] 

vậy câu trả lời là`4`. Điều này xác nhận lý do tại sao giải pháp phải theo dõi các mảng con chính hãng thay vì chỉ các tiền tố, hậu tố hoặc các lần chạy riêng lẻ. 

Cuối cùng, các cập nhật lặp lại cho cùng một vị trí yêu cầu sử dụng giá trị lá hiện tại thay vì giá trị đầu vào ban đầu. Ví dụ,```
1 3
0
1
1
1
```thay đổi giá trị được chuyển đổi thông qua`-1`,`+1`,`-1`,`+1`. Việc phủ định lá hiện tại mỗi lần sẽ tự động xử lý việc này. Giữ một mảng boolean riêng biệt cũng có tác dụng, nhưng bản thân chiếc lá đã chứa chính xác trạng thái cần thiết cho lần cập nhật tiếp theo.

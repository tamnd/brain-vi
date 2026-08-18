---
title: "CF 102253J - Hành Trình Cùng Ba Lô"
description: "Có (n) loại thực phẩm. Loại (i) chiếm (i) đơn vị thể tích và chúng ta có thể chọn giữa (0) và (ai) phần của nó. Chúng ta cũng phải chọn chính xác một loại thiết bị."
date: "2026-08-17T21:43:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102253
codeforces_index: "J"
codeforces_contest_name: "2017 Chinese Multi-University Training, BeihangU Contest"
rating: 0
weight: 102253
solve_time_s: 163
verified: true
draft: false
---

[CF 102253J - Hành trình cùng ba lô](https://codeforces.com/problemset/problem/102253/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Có (n) loại thực phẩm. Loại (i) chiếm (i) đơn vị thể tích và chúng ta có thể chọn giữa (0) và (a_i) phần của nó. Chúng ta cũng phải chọn chính xác một loại thiết bị. Thiết bị (j) chiếm (b_j) đơn vị thể tích, và hai thiết bị có thể tích bằng nhau vẫn là những lựa chọn khác nhau nếu chúng có chỉ số khác nhau. 

Ba lô có sức chứa (2n). Nếu thiết bị được chọn có thể tích (b) thì thực phẩm phải chiếm chính xác (2n-b). Gọi (f(s)) là số lượng lựa chọn thực phẩm hợp lệ với tổng khối lượng (s). Câu trả lời cuối cùng chỉ đơn giản là 

[ 
\sum_{j=1}^{m} f(2n-b_j). 
] 

Đầu vào có thể chứa khoảng 100 trường hợp thử nghiệm, với (n) đạt (5\cdot 10^4). Thuật toán bậc hai trên (2n) thể tích có thể đã quá lớn và quá trình chuyển đổi ba lô có giới hạn thông thường đối với tất cả (n) loại thực phẩm sẽ là (O(n^2)). Hạn chế mà tối đa năm thử nghiệm có (n\ge 1000) cho chúng ta biết rằng một giải pháp đại khái (O(n\sqrt n)) được dự định. Giới hạn bộ nhớ cũng thiên về một vài mảng một chiều hơn là một chương trình động hai chiều đầy đủ. 

Điều kiện đặc biệt là (a_1<a_2<\cdots<a_n). Vì (a_i) là các số nguyên không âm riêng biệt nên (a_i\ge i-1). Quan sát có vẻ nhỏ này là chìa khóa để làm cho hàm tạo có thể quản lý được. 

Có một số trường hợp ranh giới có thể âm thầm phá vỡ việc triển khai bất cẩn. Coi như```
1 1
0
1
```Không có thức ăn nào cả, và thiết bị duy nhất lấp đầy ba lô, vì vậy câu trả lời là`Case #1: 1`. Việc triển khai giả định mọi loại thực phẩm đều có thể đóng góp ít nhất một phần sẽ được tính là một lựa chọn không thể thực hiện được. 

Khối lượng thiết bị lặp đi lặp lại cũng phải là những lựa chọn lặp đi lặp lại. Vì```
2 3
0 1
4 4 4
```thức ăn phải có khối lượng bằng 0 nên chỉ có một lựa chọn thực phẩm. Có ba loại thiết bị khác nhau, do đó câu trả lời là`Case #1: 3`. Việc sao chép các giá trị (b_i) sẽ trả về một giá trị không chính xác. 

Giới hạn trên của thực phẩm phải được tôn trọng ngay cả khi một vách ngăn không hạn chế đưa ra một cách thể hiện khác. Vì```
2 1
0 1
2
```khối lượng thức ăn duy nhất có thể là (2), thu được bằng cách lấy một miếng loại 2. Cấm lấy hai miếng loại 1 vì (a_1=0). Câu trả lời là`Case #1: 1`. Coi mẫu số như một hàm phân vùng không hạn chế thông thường mà không khôi phục các thừa số của tử số sẽ tính hai biểu diễn. 

Vấn đề ranh giới tương tự xuất hiện ở khối lượng thiết bị lớn nhất. Nếu (b_j=2n), khối lượng thực phẩm cần thiết bằng 0 và tồn tại chính xác một lựa chọn thực phẩm, đó là không lấy gì cả. Hệ số (f(0)) phải duy trì bằng 1 trong mọi phép toán đa thức. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp có thể liệt kê mọi số lượng có thể có của từng loại thực phẩm, tính toán tổng khối lượng của nó, sau đó kiểm tra xem dung lượng còn lại có phù hợp với một trong các thiết bị hay không. Đối với loại thiết bị cố định, phần này sẽ kiểm tra 

[ 
\prod_{i=1}^{n}(a_i+1) 
] 

kết hợp thực phẩm. Bởi vì (a_i) khác biệt và nằm trong ([0,2n]), tích lớn nhất có thể thu được bằng cách chọn (a_i=n+i), cho 

\frac{(2n+1)!}{(n+1)!}. 
] 

Việc nhân lên tới (2n) loại trang bị khiến điều này hoàn toàn không khả thi. Ngay cả giới hạn dưới yếu hơn (\prod_i(a_i+1)\ge n!), theo sau (a_i\ge i-1), cũng đã lớn về mặt thiên văn đối với (n=5\cdot10^4). 

Cách tiếp cận lập trình động thông thường tốt hơn nhiều về mặt khái niệm. Chúng tôi có thể duy trì số cách để lấy từng khối lượng và chế biến từng loại thực phẩm bằng cách chuyển đổi ba lô có giới hạn. Thật không may, có (n) loại và (2n) khối lượng có thể, do đó việc triển khai đơn giản vẫn tốn kém (O(n^2)). 

Cách hữu ích để nén bài toán là viết hàm sinh thông thường của nó. Đối với loại thực phẩm (i), những đóng góp có thể là 

\frac{1-x^{(a_i+1)i}}{1-x^i}. 
] 

Như vậy 

\prod_{i=1}^{n} 
\frac{1-x^{(a_i+1)i}}{1-x^i}. 
] 

Chỉ các hệ số thông qua (x^{2n}) mới quan trọng, vì vậy tất cả các phép tính đa thức có thể được thực hiện theo modulo (x^{2n+1}). Đây là cách rút gọn chính được sử dụng trong bài xã luận chính thức. 

Bây giờ hãy xem xét tử số. Bởi vì (a_i\ge i-1), 

[ 
(a_i+1)i\ge i^2. 
] 

Nếu (i^2>2n) thì hệ số 

[ 
1-x^{(a_i+1)i} 
] 

chỉ là (1) modulo (x^{2n+1}). Do đó, chỉ các thừa số tử số (O(\sqrt n)) mới có thể ảnh hưởng đến câu trả lời. Chúng ta có thể nhân các thừa số đó thành một đa thức một cách rõ ràng trong thời gian (O(n\sqrt n)). 

Mẫu số giống với hàm tạo cho các phân vùng số nguyên: 

[ 
P(x)=\prod_{i\ge1}\frac1{1-x^i} 
=\sum_{k\ge0}p(k)x^k. 
] 

Phép truy hồi số ngũ giác của Euler tính từ (p(k)) đến (k=2n) trong thời gian (O(n\sqrt n)). Sự tái phát tiêu chuẩn là 

[ 
p(k)= 
\sum_{r\ge1}(-1)^{r+1} 
\left( 
p\left(k-\frac{r(3r-1)}2\right) 
+ 
p\left(k-\frac{r(3r+1)}2\right) 
\đúng). 
] 

Chúng ta cần mẫu số chỉ có thừa số (1,\ldots,n), không phải toàn bộ số nguyên dương. Lên đến bậc (2n), chúng ta có thể viết 

P(x)\prod_{i=n+1}^{2n}(1-x^i). 
] 

Bất kỳ tích hai lũy thừa nào (x^i x^j) với (i,j>n) đều có bậc lớn hơn (2n), do đó modulo (x^{2n+1}), 

1-\sum_{i=n+1}^{2n}x^i. 
] 

Do đó, các hệ số mẫu số được lấy từ các số phân vùng bằng cách sử dụng tổng tiền tố đơn giản. Đây là quan sát quan trọng thứ hai đằng sau nghiệm (O(n\sqrt n)). 

Các phương pháp tiếp cận bạo lực và tối ưu có thể được tóm tắt như sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O\left(m\prod_i(a_i+1)\right)) | (O(n)) | Quá chậm | 
| Ba Lô Ràng Buộc DP | (O(n^2)) | (O(n)) | Quá chậm | 
| Hàm sinh + Sự lặp lại hình ngũ giác | (O(n\sqrt n+m)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Cho (N=2n). Ta chỉ cần các hệ số (F(x)) từ bậc (0) đến (N), vì một thiết bị có thể tích dương và sức chứa ba lô chính xác là (N). 
2. Tính các số phân vùng không hạn chế (p(0),p(1),\ldots,p(N)) bằng phép truy hồi ngũ giác Euler. Ở đây (p(k)) đếm các phân vùng của (k) bằng cách sử dụng kích thước phần nguyên dương tùy ý. 
3. Chuyển đổi chuỗi phân vùng không giới hạn thành mẫu số cho loại thực phẩm (1,\ldots,n). Gọi (q(k)) là hệ số của 

[ 
\prod_{i=1}^{n}\frac1{1-x^i}. 
]

Với (k\le n), (q(k)=p(k)). Đối với (k>n), các phân vùng không hạn chế bổ sung chính xác là những phân vùng chứa một phần lớn hơn (n). Vì (k\le2n) nên có thể có nhiều nhất một phần như vậy. Tương tự, sử dụng nhận dạng đa thức ở trên, 

[ 
q(k)=p(k)-\sum_{r=0}^{k-n-1}p(r). 
] 

Tổng tiền tố đang chạy tính toán mọi hệ số như vậy theo thời gian tuyến tính. 

1. Sao chép các hệ số mẫu số này vào mảng làm việc (f). Ban đầu, (f(k)=q(k)), tương ứng với việc cho phép mọi loại thực phẩm xuất hiện mà không có giới hạn trên. 
2. Xử lý các thừa số tử số 

[ 
1-x^{(a_i+1)i}. 
] 

Đối với loại (i), xác định 

[ 
t_i=(a_i+1)i. 
] 

Nếu (t_i>N) thì hệ số đó không ảnh hưởng gì đến các hệ số mà chúng ta quan tâm. Vì (a_i) tăng nên các giá trị (t_i) cũng tăng nên chúng ta có thể dừng ở giá trị đầu tiên như (i). 

Với mỗi (t_i hữu ích), hãy nhân đa thức hiện tại với (1-x^{t_i}). Ở dạng hệ số, 

[ 
f[k]\leftarrow f[k]-f[k-t_i]. 
] 

Bản cập nhật phải chạy từ lớn (k) xuống (t_i). Thứ tự giảm dần làm cho mọi (f[k-t_i]) đều xuất phát từ đa thức trước khi hệ số này được áp dụng, giống hệt như quá trình chuyển đổi chiếc ba lô số 0. 

1. Sau khi áp dụng tất cả các hệ số tử số hữu ích, (f(s)) chính xác là số lượng lựa chọn thực phẩm hợp pháp có tổng khối lượng là (s). Đối với mỗi hạng mục thiết bị có khối lượng (b_j), thực phẩm phải có khối lượng (N-b_j). Thêm 

[ 
f[N-b_j] 
] 

đối với mỗi chỉ số thiết bị (j), lưu giữ các bản sao vì các loại thiết bị khác nhau thể hiện những cách khác nhau. 

### Tại sao nó hoạt động 

Hàm tạo cho một loại thực phẩm ghi lại mọi số lượng miếng được phép của loại đó chính xác một lần. Nhân các yếu tố này sẽ kết hợp các lựa chọn độc lập, do đó hệ số (x^s) tính chính xác các lựa chọn thực phẩm trong tổng khối lượng (s). 

Thay thế mỗi chuỗi hình học hữu hạn bằng 

[ 
\frac{1-x^{(a_i+1)i}}{1-x^i} 
] 

tách vấn đề thành một mẫu số không hạn chế và các hệ số hiệu chỉnh để loại bỏ các lựa chọn vượt quá mỗi giới hạn trên. Mẫu số được tính toán chính xác thông qua hàm tạo phân vùng và mọi hệ số tử số có liên quan sau đó sẽ được áp dụng chính xác một lần. Vì tất cả các thừa số tử số bị bỏ qua đều có bậc lớn hơn (2n), nên chúng không thể ảnh hưởng đến bất kỳ hệ số nào được câu trả lời sử dụng. 

Cuối cùng, việc chọn thiết bị (j) để lại khối lượng chính xác (2n-b_j) cho thực phẩm, do đó, việc tính tổng hệ số tương ứng một lần cho mỗi chỉ số thiết bị sẽ tính mỗi bao bì hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007

# Partition numbers p(k), initially p(0) = 1.
part = [1]

def ensure_partitions(N):
    """Extend part[] so that it contains p(0)..p(N)."""
    old = len(part)
    if old > N:
        return

    part.extend([0] * (N + 1 - old))

    pent = []
    r = 1
    while True:
        g1 = r * (3 * r - 1) // 2
        if g1 > N:
            break

        sign = 1 if (r & 1) else -1
        pent.append((g1, sign))

        g2 = r * (3 * r + 1) // 2
        if g2 <= N:
            pent.append((g2, sign))

        r += 1

    # The generalized pentagonal numbers are generated in increasing order.
    for k in range(old, N + 1):
        s = 0
        for g, sign in pent:
            if g > k:
                break
            if sign == 1:
                s += part[k - g]
            else:
                s -= part[k - g]
        part[k] = s % MOD

def solve_case(n, m, a, b):
    N = 2 * n
    ensure_partitions(N)

    # Start with P(x) = sum p(k)x^k.
    f = part[:N + 1]

    # Replace unrestricted partitions by partitions whose parts are <= n.
    # For k > n, subtract sum_{r=0}^{k-n-1} p(r).
    prefix = 0
    for k in range(n + 1, N + 1):
        prefix += part[k - n - 1]
        if prefix >= MOD:
            prefix -= MOD

        value = f[k] - prefix
        if value < 0:
            value += MOD
        f[k] = value

    # Apply the useful numerator factors:
    # product (1 - x^((a_i + 1)i)).
    #
    # Since a_i is increasing, t_i is increasing too.
    for i, ai in enumerate(a, 1):
        t = (ai + 1) * i
        if t > N:
            break

        for k in range(N, t - 1, -1):
            value = f[k] - f[k - t]
            if value < 0:
                value += MOD
            f[k] = value

    # Choose one equipment type. Equal b values are intentionally counted
    # repeatedly because the equipment types themselves are different.
    ans = 0
    for bj in b:
        ans += f[N - bj]
        if ans >= MOD:
            ans -= MOD

    return ans

def solve():
    case_no = 1
    out = []

    while True:
        line = input()
        if not line:
            break

        if not line.strip():
            continue

        n, m = map(int, line.split())

        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        ans = solve_case(n, m, a, b)
        out.append(f"Case #{case_no}: {ans}")
        case_no += 1

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Toàn cầu`part`mảng chỉ được mở rộng khi cần giá trị lớn hơn (2n). Điều này quan trọng vì đầu vào chứa nhiều trường hợp thử nghiệm và việc tính toán lại chuỗi phân vùng một cách độc lập cho mỗi thử nghiệm sẽ lặp lại phần tốn kém nhất của quá trình tính toán. Sự lặp lại của (p(k)) mới được thêm vào chỉ phụ thuộc vào các giá trị trước đó, do đó việc mở rộng mảng hiện tại là hợp lệ. 

Phép truy hồi phân vùng sử dụng các số ngũ giác tổng quát (r(3r-1)/2) và (r(3r+1)/2). Dấu dương cho số lẻ (r) và âm cho số chẵn (r). Vòng lặp bên trong dừng ngay khi số ngũ giác vượt quá mức hiện tại. 

Việc hiệu chỉnh mẫu số đáng được quan tâm đặc biệt. Đối với (k=n+1), tiền tố chỉ chứa (p(0)), do đó, chính xác các phân vùng có một phần (n+1) sẽ bị loại bỏ. Đối với (k=2n), tiền tố chứa (p(0),\ldots,p(n-1)), bao gồm mọi phân vùng có thể có phần duy nhất ở trên (n) là (n+1,\ldots,2n). 

Phép nhân tử số sử dụng các chỉ số giảm dần. Nếu vòng lặp chạy lên trên, hệ số được sửa đổi trước đó trong cùng hệ số có thể được sử dụng lại, áp dụng hiệu quả (1-x^t) nhiều lần. Thứ tự giảm dần ngăn chặn sự ô nhiễm đó. 

điều kiện`if t > N: break`là an toàn vì (t_i=(a_i+1)i) tăng theo (i). Đó cũng là lý do tử số chỉ có tác dụng (O(n\sqrt n)). 

Không có vấn đề tràn số nguyên trong Python. Các giá trị được giảm modulo (10^9+7) và ngay cả tổng tạm thời trong phép lặp phân vùng vẫn nằm trong kích thước số nguyên có thể quản lý được vì nó chỉ chứa các số hạng (O(\sqrt N)). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Trường hợp mẫu đầu tiên là```
1 1
1
1
```Ở đây (n=1), nên sức chứa của ba lô là (N=2). Có một loại thực phẩm có khối lượng (1), với tối đa một món và một hạng mục thiết bị có khối lượng (1). 

| Bước | Tiểu bang | Giá trị | 
| --- | --- | --- | 
| Công suất | (N=2n) | 2 | 
| Hệ số phân vùng | (p(0),p(1),p(2)) | (1,1,2) | 
| Mẫu số | bộ phận được phép lên tới 1 | (1,1,1) | 
| Số mũ tử số | ((a_1+1)\cdot1) | 2 | 
| Sau tử số | (f(0),f(1),f(2)) | (1,1,0) | 
| Mục tiêu thiết bị | (N-b_1) | 1 | 
| Đã thêm đóng góp | (f(1)) | 1 | 

Hệ số tử số (1-x^2) loại bỏ lựa chọn không hợp lệ của hai phần thực phẩm loại 1. Hệ số khối lượng một vẫn là một, tương ứng với việc lấy một miếng thức ăn. 

### Mẫu 2 

Trường hợp mẫu thứ hai là```
2 2
1 2
3 4
```Công suất là (N=4). Loại 1 cho phép một mảnh và loại 2 cho phép hai mảnh. 

| Bước | Tiểu bang | Giá trị | 
| --- | --- | --- | 
| Công suất | (N=2n) | 4 | 
| Hệ số phân vùng | (p(0)\ldots p(4)) | (1,1,2,3,5) | 
| Mẫu số | bộ phận được phép lên tới 2 | (1,1,2,2,3) | 
| Số mũ loại 1 | ((1+1)\cdot1) | 2 | 
| Cập nhật loại 1 | nhân với (1-x^2) | (1,1,1,1,1) | 
| Số mũ loại 2 | ((2+1)\cdot2) | 6 | 
| Cập nhật loại 2 | số mũ vượt quá 4 | không thay đổi | 
| Trang bị 1 mục tiêu | (4-3) | 1 | 
| Trang bị 2 mục tiêu | (4-4) | 0 | 
| Tổng cộng | (f(1)+f(0)) | 2 | 

Thừa số tử số thứ nhất loại bỏ cách biểu diễn không hạn chế chứa hai phần loại 1. Thừa số thứ hai không thể ảnh hưởng đến độ đến bốn vì số mũ của nó là sáu. Cả hai lựa chọn thiết bị đều có chính xác một lựa chọn thực phẩm tương thích, đưa ra tổng cộng hai cách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\sqrt n+m)) mỗi bài kiểm tra lớn | Chi phí tái phân vùng (O(n\sqrt n)), chi phí tử số hữu ích (O(n\sqrt n)), chi phí xử lý thiết bị (O(m)) | 
| Không gian | (O(n)) | Hai mảng hệ số có độ dài (2n+1), cộng với mảng (a) và (b) | 

Bậc đa thức thích hợp lớn nhất là (2n\le10^5). Chỉ các thừa số tử số (O(\sqrt n)) mới có thể quan trọng vì số mũ của chúng ít nhất là (i^2). Hạn chế mà tối đa năm trường hợp thử nghiệm có (n\ge10^3) ​​giúp kiểm soát tổng số lượng công việc tốn kém, trong khi các trường hợp nhỏ hơn yêu cầu ít thao tác hơn đáng kể. Việc triển khai cũng sử dụng lại trình tự phân vùng trong các trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 1_000_000_007

part = [1]

def ensure_partitions(N):
    old = len(part)
    if old > N:
        return

    part.extend([0] * (N + 1 - old))

    pent = []
    r = 1
    while True:
        g1 = r * (3 * r - 1) // 2
        if g1 > N:
            break

        sign = 1 if r & 1 else -1
        pent.append((g1, sign))

        g2 = r * (3 * r + 1) // 2
        if g2 <= N:
            pent.append((g2, sign))

        r += 1

    for k in range(old, N + 1):
        s = 0
        for g, sign in pent:
            if g > k:
                break
            s += sign * part[k - g]
        part[k] = s % MOD

def solve_case(n, m, a, b):
    N = 2 * n
    ensure_partitions(N)

    f = part[:N + 1]

    prefix = 0
    for k in range(n + 1, N + 1):
        prefix += part[k - n - 1]
        if prefix >= MOD:
            prefix -= MOD

        f[k] -= prefix
        if f[k] < 0:
            f[k] += MOD

    for i, ai in enumerate(a, 1):
        t = (ai + 1) * i
        if t > N:
            break

        for k in range(N, t - 1, -1):
            f[k] -= f[k - t]
            if f[k] < 0:
                f[k] += MOD

    ans = 0
    for bj in b:
        ans += f[N - bj]
        if ans >= MOD:
            ans -= MOD

    return ans

def solution():
    input = sys.stdin.readline
    case_no = 1
    out = []

    while True:
        line = input()
        if not line:
            break

        if not line.strip():
            continue

        n, m = map(int, line.split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        out.append(f"Case #{case_no}: {solve_case(n, m, a, b)}")
        case_no += 1

    return "\n".join(out)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        # Reuse the same global partition cache as the actual solution.
        exec_result = solution()
        sys.stdout.write(exec_result)
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples.
sample = """\
1 1
1
1
2 2
1 2
3 4
3 3
1 2 3
2 3 3
"""

assert run(sample) == (
    "Case #1: 1\n"
    "Case #2: 2\n"
    "Case #3: 6"
), "provided samples"

# Minimum-size input, with no food available.
assert run("""\
1 1
0
1
""") == "Case #1: 1", "minimum-size case"

# All equipment volumes are equal, so every equipment type must be counted.
assert run("""\
2 3
0 1
4 4 4
""") == "Case #1: 3", "duplicate equipment types"

# Boundary case for a food upper bound.
# With a = [0, 1], volume 2 can only be formed by one type-2 food.
assert run("""\
2 1
0 1
2
""") == "Case #1: 1", "food upper-bound boundary"

# Maximum n. Choosing equipment of volume 2n leaves zero volume for food,
# so the answer is exactly one regardless of the many food types.
n = 50000
a = " ".join(str(i) for i in range(n))
large_input = f"{n} 1\n{a}\n{2 * n}\n"
assert run(large_input) == "Case #1: 1", "maximum-size boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 0 / 1`|`Case #1: 1`| Kích thước tối thiểu và không có sẵn thực phẩm | 
|`2 3 / 0 1 / 4 4 4`|`Case #1: 3`| Khối lượng thiết bị bằng nhau phải có sự lựa chọn riêng biệt | 
|`2 1 / 0 1 / 2`|`Case #1: 1`| Giới hạn trên của thực phẩm và hiệu chỉnh tử số | 
| (n=50000,\ a_i=i-1,\ b_1=100000) |`Case #1: 1`| Kích thước tối đa và ranh giới (b=2n) | 

## Vỏ cạnh 

Khi giới hạn trên của thực phẩm bằng 0, toàn bộ hệ số hàm tạo của nó chỉ là (1). Vì```
1 1
0
1
```số mũ của hiệu chỉnh tử số của nó là ((0+1)\cdot1=1), do đó hàm tạo ra thực phẩm là 

[ 
\frac{1-x}{1-x}=1. 
] 

Khối lượng thực phẩm duy nhất bằng 0 và khối lượng thiết bị tiêu thụ đơn vị còn lại. Thuật toán tạo ra`Case #1: 1`. 

Khi nhiều loại trang bị có cùng dung lượng thì không được gộp chung. Vì```
2 3
0 1
4 4 4
```mọi hạng mục thiết bị đều để lại khối lượng thực phẩm bằng không. Hệ số (f(0)=1) và vòng lặp cuối cùng cộng hệ số đó ba lần. Kết quả là`Case #1: 3`. 

Đối với ví dụ hiệu chỉnh giới hạn trên```
2 1
0 1
2
```mẫu số không hạn chế cho phép phân chia hai phần bằng cách sử dụng phần một và hai, đưa ra hai khả năng: (1+1) và (2). Thừa số tử của loại 1 là (1-x), vì (a_1=0), trong khi thừa số của loại 2 là (1-x^4). Nhân với (1-x) sẽ loại bỏ cách biểu diễn (1+1), để lại (f(2)=1). Khối lượng thức ăn cần thiết là hai, vì vậy câu trả lời là`Case #1: 1`. 

Khi thiết bị lấp đầy toàn bộ ba lô, lượng thức ăn cần thiết sẽ bằng không. Đối với thử nghiệm kích thước tối đa với (n=50000), (a_i=i-1) và (b_1=100000=2n), mọi phép toán tử số và mẫu số đều giữ nguyên (f(0)=1). Tra cứu cuối cùng là (f(0)), vì vậy câu trả lời chính xác là một. Điều này cũng xác minh rằng các mảng hệ số bao gồm độ 0 và biểu thức`N - bj`không bao giờ cần chỉ số âm vì mỗi khối lượng thiết bị tối đa là (N). 

Ranh giới tinh tế nhất trong việc xây dựng đa thức xảy ra ở mức độ (2n). Tích của hai thừa số bất kỳ (x^i) và (x^j) với (i,j>n) có bậc lớn hơn (2n), do đó, các số hạng chéo đó có thể được loại bỏ một cách an toàn. Đây chính xác là lý do tại sao việc hiệu chỉnh mẫu số giảm xuống một phép trừ tổng tiền tố thay vì yêu cầu một phép nhân đa thức khác. 

Tử số có loại ranh giới ngược lại. Một thừa số có số mũ đúng (2n) vẫn thay đổi hệ số ở bậc (2n) nên điều kiện phải là`t > N`, không`t >= N`. Bản cập nhật giảm dần bao gồm`k=N`khi`t=N`, cần thiết để loại bỏ đóng góp đó một cách chính xác.

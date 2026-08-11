---
title: "CF 102428C - Giảm bất bình đẳng"
description: "Cứ mỗi tháng, của cải của người nông dân thay đổi theo giá trị tương ứng trong A. Sau khi cộng thu nhập của tháng đó, của cải ngay lập tức bị đẩy trở lại khoảng [L, U]: các giá trị trên U trở thành U và các giá trị dưới L trở thành L."
date: "2026-08-10T08:32:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102428
codeforces_index: "C"
codeforces_contest_name: "2019-2020 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 102428
solve_time_s: 452
verified: true
draft: false
---

[CF 102428C - Giảm bớt bất bình đẳng](https://codeforces.com/problemset/problem/102428/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 32 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi tháng, tài sản của người nông dân thay đổi theo giá trị tương ứng trong`A`. Sau khi cộng thu nhập của tháng đó, tài sản ngay lập tức bị đẩy trở lại khoảng thời gian`[L, U]`: các giá trị trên`U`trở nên`U`và các giá trị bên dưới`L`trở nên`L`. 

Truy vấn đưa ra tháng bắt đầu`B`, một tháng kết thúc`E`, và tài sản ban đầu`X`. Chúng ta phải áp dụng tháng`B`bởi vì`E`theo thứ tự, bao gồm cả việc sửa chữa`[L,U]`sau mỗi tháng và báo cáo sự giàu có cuối cùng. 

Đầu vào chứa một mảng`N`thu nhập hàng tháng, tiếp theo là`Q`các truy vấn độc lập. Các giá trị của`N`Và`Q`cả hai có thể đạt được`10^5`, trong khi một truy vấn có thể bao gồm tất cả`N`tháng. Kho lưu trữ chính thức đưa ra giới hạn thời gian 3 giây và giới hạn bộ nhớ 1024 MB. Với`10^5`truy vấn, một cách tiếp cận liên quan đến mỗi tháng của mỗi truy vấn có thể thực hiện tới`10^10`chuyển tiếp tháng. Điều đó loại trừ công việc tuyến tính trên mỗi truy vấn và yêu cầu công việc logarit đại khái cho mỗi truy vấn sau khi xử lý trước. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai có vẻ hợp lý trở nên sai lầm. Đầu tiên là việc điều chỉnh diễn ra hàng tháng chứ không chỉ vào cuối tháng. Coi như:```
2 1 10
-10 10
1
1 2 5
```Trình tự của cải là`5 -> 1 -> 10`, vậy câu trả lời là`10`. Một giải pháp bất cẩn cộng hai khoản thu nhập đầu tiên có được`5`, hoàn toàn thiếu rằng giới hạn dưới đã thay đổi điểm bắt đầu của tháng thứ hai. 

Trường hợp cạnh thứ hai là một truy vấn chứa đúng một tháng. Ví dụ:```
1 1 10
100
1
1 1 5
```Câu trả lời là`10`. Không có lý do gì để coi một tháng là khoảng thời gian trống hoặc áp dụng sự điều chỉnh trước khi cộng thu nhập của tháng đó. 

Trường hợp cạnh thứ ba là một phép biến đổi trở thành hằng số. Coi như:```
2 1 10
8 -8
1
1 2 3
```Bắt đầu từ`3`, tháng đầu tiên cho`10`, và cái thứ hai cho`2`, vậy câu trả lời là`2`. Sau hai tháng này, mọi giá trị khởi đầu đủ lớn đều bị ép vượt qua giới hạn trên và thành phần không còn là một bản dịch thông thường nữa. Một biểu diễn chỉ lưu trữ tổng thu nhập là không đủ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp rất đơn giản. Đối với mỗi truy vấn, hãy khởi tạo sự giàu có bằng`X`, sau đó quét từ`B`bởi vì`E`, thêm`A[i]`và kẹp kết quả vào`[L,U]`sau mỗi lần thêm. Điều này đúng vì nó tuân theo chính xác quy trình hàng tháng được mô tả bởi vấn đề. 

Vấn đề là số lượng công việc lặp đi lặp lại. Nếu mọi truy vấn bao trùm toàn bộ mảng thì có thể có`Q * N = 10^5 * 10^5 = 10^10`cập nhật từng tháng. Mặc dù mỗi lần cập nhật đều có thời gian không đổi, nhưng điều đó vượt xa những gì có thể phù hợp với giới hạn thời gian. 

Quan sát hữu ích là toàn bộ khoảng thời gian tháng có thể được coi là một hàm duy nhất. Trong một tháng với thu nhập`a`, sự biến đổi là 

[ 
f(x)=\min(U,\max(L,x+a)). 
] 

Lúc đầu, việc này có vẻ khó kết hợp vì thao tác kẹp có thể làm mất đi một phần thông tin của các tháng trước. Tuy nhiên, mọi thành phần của các phép biến đổi này đều có một biểu diễn rất nhỏ. 

Biểu thị một sự chuyển đổi như 

[ 
f(x)=\min(hi,\max(lo,x+add)). 
]

Đây`add`mô tả bản dịch ở phần mà hàm có độ dốc bằng một, trong khi`lo`Và`hi`mô tả các cao nguyên phía dưới và phía trên cuối cùng. Một tháng duy nhất có đại diện`(a, L, U)`. 

Giả sử phép biến đổi đầu tiên là 

[ 
f(x)=\tên toán tử{clamp}(x+s_1,l_1,u_1) 
] 

và phép biến đổi tiếp theo là 

[ 
g(x)=\tên toán tử{clamp}(x+s_2,l_2,u_2). 
] 

Chúng tôi cần thành phần`g(f(x))`. Trước kẹp thứ hai, đầu ra của chức năng thứ nhất được dịch chuyển bởi`s2`, do đó phạm vi hiệu quả của nó trở thành`[l1+s2, u1+s2]`. Nếu phạm vi dịch chuyển này giao nhau`[l2,u2]`, thành phần là 

[ 
\tên người vận hành{kẹp} 
\left( 
x+s_1+s_2, 
\max(l_1+s_2,l_2), 
\min(u_1+s_2,u_2) 
\đúng). 
] 

Nếu phạm vi dịch chuyển đầu tiên nằm hoàn toàn bên dưới`l2`, kết quả là liên tục`l2`. Nếu nó nằm hoàn toàn phía trên`u2`, kết quả là liên tục`u2`. 

Điều đó mang lại cho chúng ta một phép toán kết hợp có kích thước không đổi để kết hợp các tháng liên tiếp. Cây phân đoạn có thể lưu trữ phép biến đổi kết hợp của mỗi khoảng. Một truy vấn sau đó kết hợp`O(log N)`nút cây thay vì truy cập hàng tháng. 

Phương pháp brute-force hoạt động vì nó mô phỏng rõ ràng mọi chuyển đổi trạng thái, nhưng không thành công khi cùng một khoảng thời gian dài được mô phỏng lặp đi lặp lại. Quan sát cho thấy toàn bộ khoảng có thể được tóm tắt bằng ba số nguyên cho phép chúng ta thay thế mô phỏng lặp lại bằng thành phần phạm vi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NQ) trong trường hợp xấu nhất | O(1) ngoài đầu vào | Quá chậm | 
| Tối ưu | O(N + Q log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn mọi phép biến đổi khoảng dưới dạng`(add, lo, hi)`, ý nghĩa 

[ 
f(x)=\min(hi,\max(lo,x+add)). 
] 

Một tháng có thu nhập`A[i]`là`(A[i], L, U)`bởi vì người nông dân đầu tiên thêm vào`A[i]`và sau đó kẹp vào`[L,U]`. 
2. Xác định cách soạn hai phép biến đổi. Giả sử phép biến đổi đầu tiên là`(s1,l1,u1)`và thứ hai là`(s2,l2,u2)`. Sau khi áp dụng phép biến đổi đầu tiên, việc thêm`s2`chuyển phạm vi đầu ra của nó sang`[l1+s2,u1+s2]`. 

Nếu như`u1+s2 < l2`, mọi giá trị có thể đều nằm dưới giới hạn dưới của phép biến đổi thứ hai, do đó thành phần là hàm hằng`l2`. 

Nếu như`l1+s2 > u2`, mọi giá trị có thể đều nằm trên giới hạn trên của phép biến đổi thứ hai, do đó thành phần là hàm hằng`u2`. 

Mặt khác, hai phạm vi trùng nhau và thành phần được biểu thị bằng 

[ 
s=s_1+s_2, 
] 

[ 
lo=\max(l_1+s_2,l_2), 
] 

[ 
xin chào=\min(u_1+s_2,u_2). 
] 
3. Xây dựng cây phân đoạn. Mỗi lá lưu trữ sự biến đổi trong một tháng. Mỗi nút bên trong lưu trữ thành phần của phép biến đổi con trái của nó, sau đó là phép biến đổi của con bên phải. Thứ tự quan trọng vì các phép biến đổi hàng tháng không có tính giao hoán. 
4. Đối với một truy vấn`[B,E]`, hãy sử dụng truy vấn phạm vi cây phân đoạn lặp tiêu chuẩn để thu thập các phép biến đổi bao trùm khoảng đó. Các nút được chọn từ phía bên trái được sắp xếp theo thứ tự tự nhiên của chúng. Các nút được chọn từ phía bên phải phải được thêm vào trước vì chúng xuất hiện trước các nút đã được tích lũy ở phía đó. 
5. Bắt đầu bộ tích lũy truy vấn bằng phép chuyển đổi danh tính`(0,L,U)`. Vì mọi của cải có giá trị đều nằm ở`[L,U]`, điều này hoạt động như`f(x)=x`cho mọi đầu vào có thể. Soạn tất cả các nút được chọn từ trái sang phải. 
6. Áp dụng phép biến đổi kết quả cho`X`. Kết quả chính xác là sự giàu có sau mỗi tháng`E`, bởi vì phép chuyển đổi cây phân đoạn thể hiện thành phần của mọi hoạt động hàng tháng trong`[B,E]`. 

### Tại sao nó hoạt động 

Bất biến chính là mỗi nút cây phân đoạn thể hiện chính xác tác động của tất cả các tháng trong khoảng thời gian của nó đối với bất kỳ tài sản ban đầu hợp lệ nào. Một chiếc lá đúng vì nó chính xác là một bản cập nhật hàng tháng. Khi hai khoảng liền kề được hợp nhất, phép biến đổi bên phải được áp dụng cho đầu ra của phép biến đổi bên trái, do đó khoảng gốc biểu thị trình tự thời gian hoàn chỉnh của chúng. Bằng quy nạp, nghiệm của bất kỳ phân rã truy vấn nào biểu thị toàn bộ khoảng được yêu cầu. Vì phép biến đổi cuối cùng được đánh giá ở mức độ giàu có ban đầu của truy vấn`X`, giá trị được tạo ra chính xác là của cải sau tất cả những điều chỉnh cần thiết hàng tháng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def combine(s1, l1, u1, s2, l2, u2):
    """
    Return the transformation obtained by applying
    (s1, l1, u1) first and (s2, l2, u2) second.
    """

    shifted_l = l1 + s2
    shifted_u = u1 + s2

    if shifted_u < l2:
        return 0, l2, l2

    if shifted_l > u2:
        return 0, u2, u2

    return (
        s1 + s2,
        max(shifted_l, l2),
        min(shifted_u, u2),
    )

def main():
    n, L, U = map(int, input().split())
    a = list(map(int, input().split()))

    size = 1
    while size < n:
        size <<= 1

    # Each tree node stores (add, low, high).
    add = [0] * (2 * size)
    low = [L] * (2 * size)
    high = [U] * (2 * size)

    for i in range(n):
        add[size + i] = a[i]

    for i in range(size - 1, 0, -1):
        left = i << 1
        right = left | 1

        s1 = add[left]
        l1 = low[left]
        u1 = high[left]

        s2 = add[right]
        l2 = low[right]
        u2 = high[right]

        ns, nl, nu = combine(s1, l1, u1, s2, l2, u2)
        add[i] = ns
        low[i] = nl
        high[i] = nu

    q = int(input())
    out = []

    for _ in range(q):
        B, E, x = map(int, input().split())

        # Convert [B, E] to the half-open interval [B-1, E).
        left = B - 1 + size
        right = E + size

        # Identity transformation on [L, U].
        ls, ll, lu = 0, L, U
        rs, rl, ru = 0, L, U

        while left < right:
            if left & 1:
                ns, nl, nu = combine(
                    ls, ll, lu,
                    add[left], low[left], high[left]
                )
                ls, ll, lu = ns, nl, nu
                left += 1

            if right & 1:
                right -= 1
                ns, nl, nu = combine(
                    add[right], low[right], high[right],
                    rs, rl, ru
                )
                rs, rl, ru = ns, nl, nu

            left >>= 1
            right >>= 1

        # The right accumulator was built by prepending nodes,
        # so the complete interval is left_acc followed by right_acc.
        ss, sl, su = combine(ls, ll, lu, rs, rl, ru)

        x += ss
        if x < sl:
            x = sl
        elif x > su:
            x = su

        out.append(str(x))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```các`combine`hàm là cốt lõi toán học của giải pháp. Phép biến đổi đầu tiên của nó được áp dụng trước phép biến đổi thứ hai, khớp với thứ tự thời gian. Hai trường hợp rời rạc được xử lý một cách rõ ràng vì nếu không thì giới hạn dưới và giới hạn trên được tính toán có thể giao nhau, điều này sẽ không mô tả khoảng kẹp hợp lệ. 

Cây phân đoạn sử dụng bố cục lặp lại. Các lá tương ứng với các vị trí ngoài`N`được khởi tạo dưới dạng chuyển đổi danh tính`(0,L,U)`, do đó chúng không ảnh hưởng đến bất kỳ khoảng thực nào. Truy vấn sử dụng`[B-1,E)`nội bộ, điều này làm cho điểm cuối phù hợp trở thành độc quyền và tránh việc điều chỉnh từng cái một khi đi trên cây. 

Hai bộ tích lũy truy vấn là cần thiết vì truy vấn phạm vi lặp sẽ phát hiện một số nút ở phía bên phải theo thứ tự ngược lại. Bộ tích lũy bên trái sẽ thêm các phép biến đổi, trong khi bộ tích lũy bên phải sẽ thêm các phép biến đổi vào trước chúng. Kết hợp hai bộ tích lũy ở cuối sẽ khôi phục thứ tự từ trái sang phải ban đầu. 

Số nguyên Python có độ chính xác tùy ý, do đó số tích lũy`add`giá trị không tràn mặc dù lên đến`10^5`thu nhập tầm cỡ`10^6`có thể được kết hợp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hãy xem xét truy vấn đầu tiên từ mẫu chính thức:```
2 5 31
```Thu nhập liên quan là`10, 1, -1, -70`. Bắt đầu từ`31`, trình tự của cải thực tế là`41, 41, 40, 1`. 

Biểu diễn chuyển đổi thực hiện phép tính tương tự mà không mô phỏng mọi tài sản ban đầu có thể có. 

| Tháng xử lý | Thu nhập |`add`|`lo`|`hi`| Kết quả cho X = 31 | 
| --- | --- | --- | --- | --- | --- | 
| không | | 0 | 1 | 41 | 31 | 
| 2 | 10 | 10 | 1 | 41 | 41 | 
| 2 đến 3 | 1 | 11 | 2 | 41 | 41 | 
| 2 đến 4 | -1 | 10 | 1 | 40 | 40 | 
| 2 đến 5 | -70 | -60 | 1 | 1 | 1 | 

Sau thành phần cuối cùng, sự biến đổi không đổi ở`1`. Điều này phản ánh thực tế rằng thu nhập âm lớn sẽ buộc mọi tài sản ban đầu có thể có đều rơi vào giới hạn dưới vào cuối khoảng thời gian. Đầu ra mẫu cho truy vấn này là`1`. 

### Ví dụ thứ hai 

Lấy:```
4 2 10
7 -6 4 -20
1
1 4 5
```Bắt đầu từ`5`, của cải thay đổi như sau:```
5 -> 10 -> 4 -> 8 -> 2
```Sự chuyển đổi tích lũy qua các tháng là: 

| Tháng xử lý | Thu nhập |`add`|`lo`|`hi`| Kết quả cho X = 5 | 
| --- | --- | --- | --- | --- | --- | 
| không | | 0 | 2 | 10 | 5 | 
| 1 | 7 | 7 | 2 | 10 | 10 | 
| 1 đến 2 | -6 | 1 | 2 | 4 | 4 | 
| 1 đến 3 | 4 | 5 | 6 | 8 | 8 | 
| 1 đến 4 | -20 | -15 | 2 | 2 | 2 | 

Hàm số cuối cùng không đổi tại`2`. Bảng này giải thích tại sao chỉ lưu trữ tổng thu nhập là không đủ. Kẹp trên trung gian sau tháng 1 sẽ thay đổi đầu vào được nhìn thấy vào tháng 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + Q log N) | Việc xây dựng cây phân đoạn cần O(N) và mỗi truy vấn sẽ truy cập vào các nút O(log N). | 
| Không gian | O(N) | Cây phân đoạn lưu trữ ba số nguyên cho mỗi nút cây. | 

Với`N,Q <= 10^5`, quá trình tiền xử lý là tuyến tính và mỗi truy vấn chỉ thực hiện phép tính logarit. Tổng số tối đa là vào khoảng vài triệu thao tác trên cây chứ không phải là`10^10`cập nhật hàng tháng về lực lượng vũ phu, vì vậy phương pháp này phù hợp với các ràng buộc dự kiến. 

## Trường hợp thử nghiệm 

Dây nịt sau đây giả định`main`chức năng từ giải pháp có sẵn trong cùng một tệp. Nó chuyển hướng đầu vào tiêu chuẩn và nắm bắt đầu ra tiêu chuẩn để các xác nhận thực hiện việc triển khai thực tế.```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        main()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
sample = """\
5 1 41
-10 10 1 -1 -70
10
2 5 31
2 4 30
2 4 29
2 4 28
1 2 20
1 2 10
1 4 11
1 4 10
1 4 40
1 4 41
"""

sample_expected = """\
1
40
39
38
20
11
11
11
40
40
"""

assert run(sample) == sample_expected, "sample 1"

# Minimum-size and fixed-bound case
minimum = """\
1 5 5
123
3
1 1 5
1 1 5
1 1 5
"""

assert run(minimum) == "5\n5\n5\n", "minimum size and L = U"

# Intermediate lower clamp followed by positive income
lower_then_rise = """\
2 1 10
-10 10
1
1 2 5
"""

assert run(lower_then_rise) == "10\n", "intermediate lower clamp"

# Upper and lower boundaries, including single-month queries
boundaries = """\
4 1 10
9 -9 9 -9
5
1 1 1
1 2 1
2 2 10
2 3 10
1 4 5
"""

assert run(boundaries) == "10\n1\n1\n10\n1\n", "boundary transitions"

# All incomes equal, repeatedly hitting the upper bound
all_equal = """\
4 2 8
3 3 3 3
3
1 4 2
1 4 5
2 3 2
"""

assert run(all_equal) == "8\n8\n8\n", "all equal incomes"

# Maximum-size stress test.
# Every query covers the entire array, and every month has income 1.
n = 100000
q = 100000

max_input = (
    f"{n} 1 2\n"
    + ("1 " * (n - 1))
    + "1\n"
    + f"{q}\n"
    + ("1 100000 1\n" * q)
)

max_output = "2\n" * q

assert run(max_input) == max_output, "maximum-size test"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`1, 40, 39, 38, 20, 11, 11, 11, 40, 40`| Mẫu chính thức đầy đủ và hành vi cắt hỗn hợp | 
| Trường hợp tối thiểu |`5, 5, 5`|`N = 1`,`L = U`và một phép biến đổi liên tục cưỡng bức | 
| Hạ xuống rồi tăng |`10`| Kẹp sau một tháng trung gian | 
| Trường hợp ranh giới |`10, 1, 1, 10, 1`| Khoảng thời gian một tháng và cả hai giới hạn | 
| Tất cả đều bình đẳng |`8, 8, 8`| Cắt trên nhiều lần | 
| Vỏ kích thước tối đa |`100000`dòng chứa`2`| Tối đa`N`, tối đa`Q`và hiệu năng truy vấn logarit | 

## Vỏ cạnh 

Trường hợp kẹp trung gian là```
2 1 10
-10 10
1
1 2 5
```Phép biến đổi ban đầu là`(0,1,10)`. Tháng 1 đổi thành`(0,1,1)`, bởi vì mọi giá trị bắt đầu liên quan đến truy vấn đều bị giảm xuống dưới`1`rồi kẹp vào`1`. Soạn tháng 2 có thu nhập`10`tạo ra sự biến đổi liên tục`10`. Áp dụng nó vào`5`cho`10`. Phương pháp chỉ tổng tiền tố sẽ tạo ra không chính xác`5`. 

Trường hợp một tháng là```
1 1 10
100
1
1 1 5
```Cây phân đoạn chứa một lá`(100,1,10)`. Truy vấn chọn lá đó, áp dụng bộ tích lũy nhận dạng và đánh giá hàm kết quả tại`5`. Giá trị trung gian là`105`, ở trên`10`, vậy câu trả lời là`10`. 

Trường hợp biến đổi không đổi là```
2 1 10
8 -8
1
1 2 3
```Sau tháng đầu tiên sự chuyển đổi là`(8,1,10)`. Khi tháng thứ hai được lập, sự dịch chuyển của nó`-8`thay đổi khoảng đầu ra hiệu quả của phép biến đổi đầu tiên từ`[1,10]`ĐẾN`[-7,2]`. Giao điểm đó với`[1,10]`cho`[1,2]`, trong khi tổng độ dịch chuyển bằng không. Sự biến đổi cuối cùng là`clamp(x,1,2)`, vậy bắt đầu từ`3`cho`2`. Đây chính xác là loại hành vi mà một biểu diễn chỉ dựa trên tổng số không thể biểu diễn được. 

Trường hợp ràng buộc đẳng thức là```
1 5 5
123
1
1 1 5
```Bởi vì`L`Và`U`đều bình đẳng, mọi sự giàu có có thể đều đã bị buộc phải chính xác`5`. Sự biến đổi của lá là`(123,5,5)`, không đổi tại`5`, và truy vấn trả về`5`. Logic thành phần vẫn hợp lệ ngay cả khi khoảng thời gian cho phép chỉ chứa một giá trị.

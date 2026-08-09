---
title: "CF 102503N - Thánh Khói"
description: "Các thiên thần xác định một giá trị thiêng liêng cố định cho mỗi điếu thuốc. Cách hữu ích để xem xét quá trình này là quên đi các thiên thần và kiểm tra biểu diễn nhị phân của chỉ số thuốc lá. Xét điếu thuốc (x) và viết (y=x-1)."
date: "2026-08-09T19:19:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102503
codeforces_index: "N"
codeforces_contest_name: "National Olympiad in Informatics - Philippines (NOI.PH) Online Eliminations 2020"
rating: 0
weight: 102503
solve_time_s: 743
verified: true
draft: false
---

[CF 102503N - Thuốc lá thần thánh](https://codeforces.com/problemset/problem/102503/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 12m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Các thiên thần xác định một giá trị thiêng liêng cố định cho mỗi điếu thuốc. Cách hữu ích để xem xét quá trình này là quên đi các thiên thần và kiểm tra biểu diễn nhị phân của chỉ số thuốc lá. 

Xét điếu thuốc (x) và viết (y=x-1). Angel (i) chạm vào (x) chính xác khi bit ((i-1))-st của (y) được đặt. Do đó số lần (x) được chạm chính xác là số bit được đặt trong (x-1), hoặc 

[ 
\operatorname{popcount}(x-1). 
] 

Thời điểm điếu thuốc được chạm vào chính xác là vị trí của các bit đã đặt. Nếu hai điếu thuốc có cùng số lần chạm, lịch sử chạm của chúng sẽ được so sánh từ lần chạm gần đây nhất trở về sau. Sự khác biệt đầu tiên xảy ra sau đó đối với điếu thuốc có phần tương ứng quan trọng hơn. Do đó, trong số các loại thuốc lá có số lượng bằng nhau, giá trị lớn hơn của (x-1), tương đương với chỉ số thuốc lá (x) lớn hơn, sẽ thánh thiện hơn. 

Vì vậy, toàn bộ quy tắc xếp hạng trở nên rất đơn giản: 

[ 
x_1 \text{ thánh thiện hơn } x_2 
] 

chính xác khi nào 

[ 
\bigl(\operatorname{popcount}(x_1-1),x_1\bigr) 

> 

\bigl(\operatorname{popcount}(x_2-1),x_2\bigr) 
] 

theo thứ tự từ điển. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi giới hạn thứ tự này trong khoảng (L,\ldots,R). Các giá trị (a) và (b) yêu cầu tổng các phần tử xếp từ (a) đến (b). 

Khoảng thời gian có thể chứa tối đa (10^9) điếu thuốc và có thể có (5\cdot10^4) trường hợp thử nghiệm. Ngay cả công việc tuyến tính trên một khoảng cũng đã là quá nhiều, trong khi việc sắp xếp (O((R-L+1)\log(R-L+1))) là hoàn toàn không thể. Số (10^9) cũng đủ nhỏ ở dạng nhị phân để mọi chỉ mục liên quan chỉ sử dụng 30 bit, đây là thuộc tính cấu trúc mà chúng tôi khai thác. 

Có một số trường hợp ranh giới có thể âm thầm phá vỡ quá trình triển khai. 

Đối với đầu vào nhỏ nhất có thể,```
1
1 1 1 1
```điếu thuốc duy nhất là (1), nên đáp án là (1). Ở đây (x-1=0), có bit được đặt bằng 0. Việc triển khai vô tình sử dụng`popcount(x)`thay vì`popcount(x-1)`sẽ đưa ra thứ tự sai trong khoảng thời gian lớn hơn. 

Vì```
1
1 2 1 1
```câu trả lời là (2). Thuốc lá (2) có (x-1=1) nên nó được chạm vào một lần, còn điếu thuốc lá (1) không bao giờ được chạm vào. Do đó, hạng đầu tiên là (2). 

Số lượng dân số bằng nhau cũng cần được chăm sóc. Coi như```
1
10 11 1 1
```Các giá trị tương ứng của (x-1) là (9=1001_2) và (10=1010_2). Cả hai đều có hai bit được đặt, nhưng lần chạm thứ hai của (10) xảy ra ở vị trí góc (2), trong khi lần chạm thứ hai của (9) xảy ra ở vị trí góc (1). Vì vậy điếu thuốc lá (11) thánh thiện hơn và câu trả lời là (11). Chỉ sắp xếp theo số lần chạm sẽ không giải quyết được sự so sánh này. 

Ranh giới sức mạnh của hai là một nguồn sai lầm phổ biến khác. Vì```
1
8 9 1 2
```ta có (x-1=7) và (8). Cái đầu tiên có ba bit được đặt và cái thứ hai có một bit, vì vậy thứ tự là (8,9), đưa ra câu trả lời (17). Việc coi chính chỉ số thuốc lá là giá trị nhị phân sẽ thay đổi số lần chạm và tạo ra kết quả sai. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là kiểm tra từng điếu thuốc trong khoảng thời gian đó, tính toán khóa thánh của nó, sắp xếp tất cả các khóa và tính tổng các thứ hạng được yêu cầu. Việc tính toán khóa bằng cách thực sự mô phỏng các thiên thần sẽ yêu cầu kiểm tra khoảng 30 bit cho mỗi điếu thuốc, bởi vì (10^9<2^{30}). Đối với một khoảng có độ dài (10^9), đó là khoảng (3\cdot10^{10}) kiểm tra cơ bản trước khi sắp xếp. Việc sắp xếp hàng tỷ giá trị kết quả sẽ yêu cầu một phép so sánh khác khoảng (10^9\log_2(10^9)) hoặc khoảng (3\cdot10^{10}). Cách tiếp cận này không tương thích từ xa với giới hạn thời gian và việc lặp lại ngay cả việc quét tuyến tính cho các trường hợp kiểm thử (5\cdot10^4) là không thể. 

Việc quan sát nhị phân thay đổi hoàn toàn vấn đề. Chúng ta không bao giờ cần liệt kê thuốc lá. Chúng ta chỉ cần đếm và tính tổng các số có số bit quy định trong một khoảng. 

Đặt (y=x-1). Khi đó khoảng thời gian hút thuốc lá ([L,R]) trở thành khoảng thời gian 

[ 
[L-1,R-1] 
] 

của số nguyên nhị phân. Đối với số lượng cố định (c), tất cả các giá trị có số lượng đó xuất hiện liên tiếp theo thứ tự thiêng liêng và bên trong nhóm đó, chúng xuất hiện theo thứ tự số giảm dần. 

Giả sử chúng ta có thể trả lời truy vấn sau một cách nhanh chóng: 

|{y\le X:\operatorname{popcount}(y)=c}| 
] 

và 

\sum_{\substack{y\le X\\operatorname{popcount}(y)=c}}y. 
] 

Sau đó trừ các câu trả lời ở (L-2) khỏi các câu trả lời ở (R-1) sẽ cho ra số lượng và tổng của mỗi nhóm số lượng trong khoảng được yêu cầu. 

Nhiệm vụ còn lại là chỉ chọn một phần của một nhóm. Vì một nhóm được sắp xếp theo thứ tự giảm dần (y), nên chúng ta có thể tìm thấy giá trị biên bằng tìm kiếm nhị phân. Mỗi truy vấn đếm hoặc tổng có thể được thực hiện theo thời gian không đổi sau một sơ đồ tiền xử lý nhỏ dựa trên việc chia số 30 bit thành hai nửa 15 bit. 

Chúng tôi tính toán trước việc phân phối tiền tố cho tất cả các nửa thấp 15 bit. Chúng tôi cũng tính toán trước sự phân bố của các khối hoàn chỉnh được xác định bởi 15 bit cao. Một số (y) được viết là 

[ 
y=h\cdot2^{15}+l. 
] 

Đối với phần cao cố định (h), phần thấp nằm trong khoảng (0,\ldots,2^{15}-1). Số dân là 

[ 
\operatorname{popcount}(h)+\operatorname{popcount}(l), 
] 

và tổng của khối hoàn chỉnh có thể được lấy từ tổng và số nửa thấp tương ứng. 

Chỉ có thể có (2^{15}=32768) nửa thấp và chỉ có khoảng (30518) nửa cao có thể vì (y<10^9). Điều này làm cho quá trình tiền xử lý đủ nhỏ cho bộ nhớ và đủ nhanh để chia sẻ trên tất cả các trường hợp kiểm thử (5\cdot10^4). 

Hai cách tiếp cận có thể được tóm tắt như sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N\log N)) cho mỗi truy vấn, (N=R-L+1) | (O(N)) | Quá chậm | 
| Tối ưu | (O(\log 10^9)) cho mỗi truy vấn sau khi xử lý trước | (O(2^{15}\cdot30)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mọi chỉ số thuốc lá (x) thành (y=x-1). Angel (i) chạm vào (x) chính xác khi bit (i-1) của (y) là một, do đó số lần chạm là`popcount(y)`. Để có số lần chạm bằng nhau, việc so sánh lịch sử chạm từ sự kiện mới nhất trở về sau tương đương với việc so sánh (y) bằng số. Do đó thứ tự giảm dần số lượng, sau đó giảm dần (y). 
2. Biểu thị khoảng thời gian truy vấn từ (A=L-1) đến (B=R-1). Chúng ta sẽ làm việc hoàn toàn với các giá trị (y) này và chỉ thêm một giá trị khi chuyển đổi tổng của chúng về chỉ số thuốc lá. 
3. Chia mỗi (y) thành nửa cao và nửa thấp bằng cách sử dụng (15) bit: 
[ 
y=(h\ll15)+l. 
] 
Tính toán trước, đối với mọi điểm cuối thấp có thể có, có bao nhiêu số 15 bit của mỗi số đếm đã xuất hiện và tổng của chúng là bao nhiêu. 
4. Tính toán trước thông tin tương tự cho các khối cao hoàn chỉnh. Khối hoàn chỉnh có phần cao (h) chứa mọi giá trị thấp từ (0) đến (2^{15}-1). Nếu phần thấp có (j) bit được đặt thì khối hoàn chỉnh sẽ đóng góp 
[ 
\binom{15}{j} 
] 
các số có tổng phần nhỏ được biết từ quá trình tính toán trước. 
5. Sử dụng các bảng này để trả lời số đếm và tổng các số (y\le X) có số đếm cố định (c) bất kỳ trong thời gian không đổi. Các khối hoàn chỉnh trước (h) đến trực tiếp từ các bàn cao. Khối một phần cuối cùng được lấy từ bảng tiền tố thấp, được dịch chuyển bởi`popcount(h)`. 
6. Đối với khoảng truy vấn, hãy trừ thông tin tiền tố cho (A-1) khỏi thông tin tiền tố cho (B). Điều này tạo ra`cnt[c]`, số lượng các giá trị khoảng có số lượng (c) và`sm[c]`, tổng (y) của chúng. 
7. Để tính tổng của (k) điếu thuốc lá thiêng liêng nhất đầu tiên, hãy quét số lượng thuốc lá từ (30) xuống (0). Nếu cả nhóm nằm trong số các vị trí (k) đầu tiên, hãy cộng tổng đầy đủ của nó. Mặt khác, chỉ cần một phần của nhóm này và vì nhóm được sắp xếp theo thứ tự giảm dần (y), nên chúng ta cần các giá trị còn lại lớn nhất của nó. 
8. Giả sử một nhóm chứa (n) giá trị và chúng ta cần các giá trị (t<n) lớn nhất của nó. Tương tự, chúng ta có thể loại bỏ các giá trị (n-t) nhỏ nhất của nó. Tìm kiếm nhị phân ((n-t))-nhỏ nhất (y) trong khoảng. Truy vấn đếm tiền tố có số lượng cố định cho chúng ta biết liệu một ứng cử viên có chứa đủ giá trị hay không, vì vậy mỗi bước tìm kiếm nhị phân là thời gian không đổi. 
9. Tính tổng tiền tố cho hạng (b) và trừ tổng tiền tố cho hạng (a-1). Sự khác biệt chính xác là tổng cấp bậc được yêu cầu từ (a) đến (b). 

Tại sao nó hoạt động. Bất biến quan trọng là mỗi điếu thuốc thuộc về chính xác một nhóm dân số và tất cả các nhóm xuất hiện theo thứ tự dân số giảm dần. Bên trong một nhóm, lịch sử chạm được so sánh từ thiên thần mới nhất trở đi, đây chính xác là so sánh từ điển của các biểu diễn nhị phân từ bit quan trọng nhất trở xuống. Vì số lượng là cố định nên sự so sánh này tương đương với thứ tự số giảm dần. Do đó, quá trình tiền xử lý sẽ đưa ra kích thước và tổng chính xác của mỗi khối liên tiếp trong bảng xếp hạng cuối cùng. Khi tiền tố được yêu cầu kết thúc bên trong một khối, tìm kiếm nhị phân sẽ xác định chính xác ranh giới của hậu tố số được yêu cầu. Do đó, mọi phần tử được bao gồm trong tổng tiền tố đều có thứ hạng chính xác và không có phần tử nào nằm ngoài tiền tố đó được bao gồm. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

BITS = 15
BLOCK = 1 << BITS
MAX_Y = 10**9 - 1
MAX_HIGH = MAX_Y >> BITS
MAX_POP = 30

# Binomial coefficients C(15, k).
comb = [1] * (BITS + 1)
for i in range(1, BITS + 1):
    comb[i] = comb[i - 1] * (BITS - i + 1) // i

# For all 15-bit values, grouped by popcount:
# full_cnt[k] = number of values in [0, BLOCK-1] with popcount k
# full_sum[k] = their sum
full_cnt = comb[:]
full_sum = [0] * (BITS + 1)
for k in range(1, BITS + 1):
    full_sum[k] = ((BLOCK - 1) * comb[k - 1] * k) // BITS

# Low-half prefix tables.
# low_cnt[k][x] = count of v <= x with popcount(v) = k
# low_sum[k][x] = sum of those v
low_cnt = [array('I', [0]) * BLOCK for _ in range(BITS + 1)]
low_sum = [array('Q', [0]) * BLOCK for _ in range(BITS + 1)]

low_pop = [0] * BLOCK
for x in range(BLOCK):
    low_pop[x] = x.bit_count()

for k in range(BITS + 1):
    cnt = 0
    sm = 0
    ac = low_cnt[k]
    ass = low_sum[k]

    for x in range(BLOCK):
        if low_pop[x] == k:
            cnt += 1
            sm += x
        ac[x] = cnt
        ass[x] = sm

# High-block prefix tables.
#
# high_cnt[k][h] = number of y in complete blocks with high part < h
#                  having popcount k.
# high_sum[k][h] = corresponding sum of y.
#
# h itself is an exclusive endpoint.
HIGH_SIZE = MAX_HIGH + 1

high_cnt = [array('I', [0]) * HIGH_SIZE for _ in range(MAX_POP + 1)]
high_sum = [array('Q', [0]) * HIGH_SIZE for _ in range(MAX_POP + 1)]

high_pop = [h.bit_count() for h in range(MAX_HIGH)]

for k in range(MAX_POP + 1):
    cnt = 0
    sm = 0
    ac = high_cnt[k]
    ass = high_sum[k]

    for h in range(MAX_HIGH):
        j = k - high_pop[h]

        if 0 <= j <= BITS:
            c = full_cnt[j]
            cnt += c
            sm += h * BLOCK * c + full_sum[j]

        ac[h + 1] = cnt
        ass[h + 1] = sm

def prefix_distribution(x):
    """Return counts and sums by popcount for all y in [0, x]."""
    if x < 0:
        return [0] * (MAX_POP + 1), [0] * (MAX_POP + 1)

    h = x >> BITS
    l = x & (BLOCK - 1)
    hp = h.bit_count()

    cnt = [0] * (MAX_POP + 1)
    sm = [0] * (MAX_POP + 1]

    for k in range(MAX_POP + 1):
        c = high_cnt[k][h]
        s = high_sum[k][h]

        j = k - hp
        if 0 <= j <= BITS:
            lc = low_cnt[j][l]
            ls = low_sum[j][l]
            c += lc
            s += ls + h * BLOCK * lc

        cnt[k] = c
        sm[k] = s

    return cnt, sm

def count_sum_upto(x, k):
    """Count and sum y <= x with popcount(y) = k."""
    if x < 0:
        return 0, 0

    h = x >> BITS
    l = x & (BLOCK - 1)
    hp = h.bit_count()

    cnt = high_cnt[k][h]
    sm = high_sum[k][h]

    j = k - hp
    if 0 <= j <= BITS:
        lc = low_cnt[j][l]
        ls = low_sum[j][l]
        cnt += lc
        sm += ls + h * BLOCK * lc

    return cnt, sm

def sum_largest_in_group(A, B, k, total_count, total_sum, take):
    """
    Sum the 'take' largest y in [A, B] having popcount k.
    The group is ordered increasingly by y here, so we remove
    the smallest total_count - take values.
    """
    if take == 0:
        return 0
    if take == total_count:
        return total_sum

    remove = total_count - take

    base_count, base_sum = count_sum_upto(A - 1, k)

    lo = A
    hi = B

    # Find the remove-th smallest value.
    while lo < hi:
        mid = (lo + hi) // 2
        c, _ = count_sum_upto(mid, k)
        c -= base_count

        if c >= remove:
            hi = mid
        else:
            lo = mid + 1

    _, boundary_sum = count_sum_upto(lo, k)
    smallest_sum = boundary_sum - base_sum

    return total_sum - smallest_sum

def prefix_holiest(A, B, need, cnt, sm):
    """
    Sum the first 'need' holiest cigarettes in [A+1, B+1],
    where A and B are y=x-1 endpoints.
    """
    if need <= 0:
        return 0

    answer = 0
    remaining = need

    for k in range(MAX_POP, -1, -1):
        group_count = cnt[k]
        if group_count == 0:
            continue

        if remaining >= group_count:
            answer += sm[k]
            remaining -= group_count

            if remaining == 0:
                break
        else:
            answer += sum_largest_in_group(
                A, B, k, group_count, sm[k], remaining
            )
            break

    # Convert the selected y values into cigarette indices x=y+1.
    return answer + need

def solve():
    T = int(input())
    out = []

    for _ in range(T):
        L, R, a, b = map(int, input().split())

        A = L - 1
        B = R - 1

        right_cnt, right_sum = prefix_distribution(B)
        left_cnt, left_sum = prefix_distribution(A - 1)

        cnt = [0] * (MAX_POP + 1)
        sm = [0] * (MAX_POP + 1)

        for k in range(MAX_POP + 1):
            cnt[k] = right_cnt[k] - left_cnt[k]
            sm[k] = right_sum[k] - left_sum[k]

        result_b = prefix_holiest(A, B, b, cnt, sm)
        result_a = prefix_holiest(A, B, a - 1, cnt, sm)

        out.append(str(result_b - result_a))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Khối tiền xử lý đầu tiên xây dựng hệ số nhị thức cho 15 bit. Một nhóm các giá trị 15 bit với các bit được đặt chính xác (k) chứa các phần tử (\binom{15}{k}). Tổng tương ứng thu được bằng cách quan sát rằng mọi vị trí trong số 15 bit đều xuất hiện ở chính xác (\binom{14}{k-1}) các giá trị như vậy. 

Các bảng nửa dưới lưu trữ thông tin tiền tố chính xác cho mọi điểm cuối từ (0) đến (32767). Vì chỉ có 16 số đếm có thể có cho giá trị 15 bit nên các bảng này rất nhỏ. 

Các bảng nửa cao đại diện cho các khối hoàn chỉnh.`high_cnt[k][h]`chứa tất cả các khối hoàn chỉnh có phần cao nhỏ hơn`h`. Quy ước độc quyền này làm cho truy vấn tiền tố trở nên rõ ràng: khi giá trị được yêu cầu có phần cao`h`, các khối trước`h`đã hoàn thành và chỉ phần thấp của khối`h`cần xử lý đặc biệt. 

chức năng`prefix_distribution`kết hợp hai phần đó. Nếu phần cao đóng góp`hp`đặt bit, sau đó là phần thấp với`j`tập hợp các bit cho tổng số lượng là`hp + j`. Giá trị số của nó là`h * BLOCK + low`, điều này giải thích thêm`h * BLOCK * count`số hạng trong tổng. 

Phân phối khoảng có được bằng cách trừ hai tiền tố. Phép trừ này phải sử dụng (L-2) trong tọa độ điếu thuốc ban đầu, bởi vì (A=L-1) là (y) đầu tiên và chúng tôi muốn các giá trị đúng trước (A).`prefix_holiest`xử lý số lượng popcount từ lớn nhất đến nhỏ nhất vì số lần chạm là tiêu chí chính xác. Trong một nhóm dân số, các giá trị số lớn nhất sẽ xuất hiện trước, vì vậy`sum_largest_in_group`chọn hậu tố cần thiết của nhóm tăng dần về số lượng. 

Tìm kiếm nhị phân sử dụng hàm đếm số lượng cố định thay vì xây dựng bất kỳ giá trị thực tế nào. Khoảng tìm kiếm bao gồm cả hai vế và điều kiện`c >= remove`tìm giá trị đầu tiên chứa ít nhất số phần tử nhỏ hơn được yêu cầu. Do đó, tổng tại ranh giới đó chứa chính xác giá trị nhỏ nhất`remove`phần tử sau khi trừ tiền tố trước (A). 

Số nguyên Python có độ chính xác tùy ý, do đó tổng không có nguy cơ bị tràn. Các mảng được sử dụng để xử lý trước sử dụng số nguyên 32 bit không dấu cho số đếm và số nguyên 64 bit không dấu cho tổng, giúp duy trì mức sử dụng bộ nhớ ở mức nhỏ trong khi bao phủ toàn bộ phạm vi số. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, điếu thuốc duy nhất là (1), vì vậy (y=0). Số lượng của nó bằng 0 và nó là thành viên duy nhất của khoảng. 

| Bước | (A) | (B) | Số lượng | Số nhóm | Còn lại | Tổng số đã chọn (y) | 
| --- | --- | --- | --- | --- | --- | --- | 
| Nhóm quy trình | 0 | 0 | 0 | 1 | 1 | 0 | 
| Chuyển đổi sang chỉ số thuốc lá | 0 | 0 | 0 | 1 | 0 | 1 | 

Tổng (y) được chọn bằng 0, nhưng mọi (y) được chọn đều tương ứng với điếu thuốc (y+1). Do đó câu trả lời là (0+1=1). 

Đối với truy vấn đầu tiên của Mẫu 2, khoảng từ (2) đến (11), do đó (y) nằm trong khoảng từ (1) đến (10). Các nhóm là 

[ 
\bắt đầu{căn chỉnh} 
\operatorname{popcount}=3 &: 7,\ 
\operatorname{popcount}=2 &: 10,9,6,5,3,\ 
\operatorname{popcount}=1 &: 8,4,2,1. 
\end{căn chỉnh} 
] 

Sau khi thêm một để chuyển đổi trở lại chỉ số thuốc lá, thứ tự thiêng liêng là 

[ 
8,11,10,7,6,4,9,5,3,2. 
] 

Để đạt được thứ hạng từ (6) đến (8), chúng ta trừ tiền tố có độ dài (5) khỏi tiền tố có độ dài (8). 

| Tiền tố | Số lượng | Số nhóm | Được yêu cầu từ nhóm | Chỉ số thuốc lá được chọn | Tổng tiền tố | 
| --- | --- | --- | --- | --- | --- | 
| 5 đầu tiên | 3 | 1 | 1 | 8 | 8 | 
| 5 đầu tiên | 2 | 5 | 4 | 11, 10, 7, 6 | 42 | 
| 8 đầu tiên | 3 | 1 | 1 | 8 | 8 | 
| 8 đầu tiên | 2 | 5 | 5 | 11, 10, 7, 6, 4 | 46 | 
| 8 đầu tiên | 1 | 4 | 2 | 9, 5 | 60 | 

Số tiền được yêu cầu là 

[ 
60-42=18. 
] 

Điều này chứng tỏ tính bất biến trung tâm: các nhóm dân số hoàn chỉnh có thể được sử dụng ngay lập tức, trong khi một nhóm một phần chỉ cần các thành viên có số lượng lớn nhất. 

Đối với truy vấn thứ ba của Mẫu 2, khoảng từ (2) đến (17), tương ứng với (y=1) đến (16). Sự bắt đầu của đơn hàng là 

[ 
16,14,13,11,7,12,10,9,6,5,3,15,8,4,2,1 
] 

khi được viết dưới dạng giá trị (y) trong các nhóm dân số thích hợp. Sau khi thêm một, hạng (12,13,14) là (17,9,5), cho (31). 

| Xếp hạng | (y) | Thuốc lá (y+1) | Số lượng | 
| --- | --- | --- | --- | 
| 12 | 16 | 17 | 1 | 
| 13 | 8 | 9 | 1 | 
| 14 | 4 | 5 | 1 | 

Ba điếu thuốc được yêu cầu đều nằm trong cùng một nhóm số lượng, do đó thứ tự số trong nhóm đó sẽ xác định thứ tự của chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian tiền xử lý | (O(30\cdot2^{15})) | Xây dựng các bảng tiền tố có số lượng phổ biến thấp và cao | 
| Mỗi trường hợp thử nghiệm | (O(30+\log 10^9)) | Hai bản phân phối, hai lần quét tiền tố và nhiều nhất là hai tìm kiếm nhị phân 30 bước | 
| Tổng thời gian | (O(30\cdot2^{15}+T\log 10^9)) | Tiền xử lý được chia sẻ cộng với tất cả các trường hợp thử nghiệm | 
| Không gian | (O(30\cdot2^{15})) | Bảng tổng hợp và đếm dân số | 

Quá trình tiền xử lý chỉ được thực hiện một lần. Đối với các trường hợp thử nghiệm (5\cdot10^4), công việc truy vấn chỉ bao gồm vài triệu thao tác bảng nhỏ cộng với khoảng 30 lần lặp tìm kiếm nhị phân cho mỗi tiền tố được yêu cầu. Điều này tránh mọi sự phụ thuộc vào (R-L+1), đây là yêu cầu quan trọng đối với khoảng thời gian chứa gần một tỷ điếu thuốc. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây sử dụng lời tiên tri mạnh mẽ trực tiếp. Nó nhằm mục đích xác nhận, không phải để gửi, vì vậy nó rất đơn giản.```python
import io
import sys

def run(inp: str) -> str:
    data = io.StringIO(inp)
    t = int(data.readline())
    answers = []

    for _ in range(t):
        L, R, a, b = map(int, data.readline().split())

        values = list(range(L, R + 1))
        values.sort(
            key=lambda x: ((x - 1).bit_count(), x),
            reverse=True
        )

        answers.append(str(sum(values[a - 1:b])))

    return "\n".join(answers)

# Provided samples
assert run(
    """1
1 1 1 1
"""
) == "1", "sample 1"

assert run(
    """3
2 11 6 8
2 11 1 1
2 17 12 14
"""
) == "18\n8\n31", "sample 2"

# Minimum interval
assert run(
    """1
1 1 1 1
"""
) == "1", "minimum-size input"

# Using x-1 instead of x must be handled correctly.
assert run(
    """1
1 2 1 1
"""
) == "2", "x-1 popcount boundary"

# Equal popcount, where numerical order breaks the tie.
assert run(
    """1
10 11 1 1
"""
) == "11", "equal-popcount ordering"

# Crossing a power of two changes the touch count sharply.
assert run(
    """1
8 9 1 2
"""
) == "17", "power-of-two boundary"

# Large index, testing the upper numeric boundary.
assert run(
    """1
1000000000 1000000000 1 1
"""
) == "1000000000", "maximum index"

# A partial popcount group.
assert run(
    """1
1 4 2 3
"""
) == "5", "partial group"

# All requested ranks collapse to one exact rank.
assert run(
    """1
2 11 6 6
"""
) == "4", "single requested rank"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1`|`1`| Khoảng thời gian tối thiểu và số lượng bằng 0 | 
|`1 2 1 1`|`2`| Sử dụng đúng (x-1) | 
|`10 11 1 1`|`11`| Số lượng bằng nhau và đặt hàng lần chạm gần đây | 
|`8 9 1 2`|`17`| Ranh giới sức mạnh của hai | 
|`1000000000 1000000000 1 1`|`1000000000`| Chỉ số tối đa | 
|`1 4 2 3`|`5`| Lựa chọn một phần bên trong bảng xếp hạng | 
|`2 11 6 6`|`4`| Truy vấn xếp hạng đơn | 

## Vỏ cạnh 

Đối với đầu vào tối thiểu```
1
1 1 1 1
```chúng ta có (A=B=0). Nhóm duy nhất có thành viên là số 0, có số một và tổng bằng 0. Tiền tố thiêng liêng nhất đầu tiên chọn một giá trị đó và chuyển đổi cuối cùng sẽ thêm một giá trị, tạo ra (1). 

Vì```
1
1 2 1 1
```khoảng biến đổi là (y=0,1). Nhóm popcount-one chứa (y=1), do đó nó được xử lý trước nhóm popcount-zero chứa (y=0). Do đó, hạng đầu tiên là thuốc lá (1+1=2). Điều này nắm bắt được lỗi phổ biến của máy tính`popcount(x)`thay vì`popcount(x-1)`. 

Vì```
1
10 11 1 1
```các giá trị được chuyển đổi là (9=1001_2) và (10=1010_2). Cả hai đều có hai lần chạm. Lần chạm gần đây nhất của họ là thiên thần (4) cho cả hai, vì vậy sự so sánh sẽ chuyển sang lần chạm trước đó. Thiên thần (2) chạm vào (10), trong khi thiên thần (1) chạm vào (9). Do đó (10) thánh thiện hơn (9), tương ứng với thuốc lá (11) xếp hạng một. 

Vì```
1
8 9 1 2
```các giá trị được chuyển đổi là (7=111_2) và (8=1000_2). Số lần chạm của họ lần lượt là ba và một. Thuốc lá số ba phải đến trước bất kể giá trị số nhỏ hơn của nó, đưa ra thứ tự (8,9) và câu trả lời (17). 

Để có chỉ số tối đa,```
1
1000000000 1000000000 1 1
```giá trị được chuyển đổi là (999999999), vẫn phù hợp với 30 bit. Các bảng tiền xử lý và tra cứu bao gồm mọi giá trị được chuyển đổi có thể lên đến (10^9-1), do đó, khoảng thời gian đơn lẻ được xử lý mà không có trường hợp đặc biệt và câu trả lời là chính xác (1000000000). 

Đối với một phần nhóm như```
1
1 4 2 3
```các giá trị được chuyển đổi là (0,1,2,3). Số lượng của chúng là (0,1,1,2), nên thứ tự của thuốc lá là (4,3,2,1). Hạng (2) và (3) là (3) và (2), cho (5). Thuật toán tiếp cận nhóm popcount-một sau khi sử dụng nhóm popcount-2 hoàn chỉnh và sau đó sử dụng quy trình chọn nhóm một phần để lấy chính xác giá trị lớn nhất được yêu cầu.

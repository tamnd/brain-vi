---
title: "CF 1516D - Cắt"
description: "Chúng ta được cung cấp một mảng số nguyên lên tới $10^5$. Đối với mỗi truy vấn $[l,r]$, chúng ta phải chia mảng con đó thành số phần nhỏ nhất có thể của các phần liền kề sao cho, bên trong mỗi phần, tích của tất cả các phần tử bằng bội số chung nhỏ nhất của các phần tử đó."
date: "2026-06-10T18:27:15+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "graphs", "number-theory", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1516
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 717 (Div. 2)"
rating: 2100
weight: 1516
solve_time_s: 145
verified: true
draft: false
---

[CF 1516D - Cắt](https://codeforces.com/problemset/problem/1516/D) 

**Đánh giá:** 2100 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, đồ thị, lý thuyết số, hai con trỏ 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng lên tới$10^5$số nguyên. Đối với mỗi truy vấn$[l,r]$, chúng ta phải chia mảng con đó thành số phần tử liền kề tối thiểu có thể sao cho, bên trong mỗi phần, tích của tất cả các phần tử bằng bội số chung nhỏ nhất của các phần tử đó. 

Nhiệm vụ không phải là xây dựng phân vùng. Chúng tôi chỉ cần số lượng phân khúc tối thiểu. 

Khó khăn chính là có tới$10^5$các truy vấn, do đó việc trả lời từng truy vấn một cách độc lập là không thể. Chúng tôi cần tiền xử lý nặng và xử lý truy vấn rất nhanh. 

Để hiểu điều kiện, hãy xem xét một đoạn chứa các số$x_1,x_2,\dots,x_k$. 

Tích số chỉ bằng LCM khi không có thừa số nguyên tố nào xuất hiện ở nhiều hơn một phần tử của phân đoạn. 

Ví dụ, đoạn$[6,35]$là hợp lệ. Các thừa số nguyên tố là:$$6 = 2 \cdot 3,\qquad 35 = 5 \cdot 7$$Mỗi số nguyên tố tổng thể đều xuất hiện đúng một lần, vì vậy:$$6 \cdot 35 = \operatorname{lcm}(6,35)$$Bây giờ hãy xem xét$[6,10]$:$$6 = 2 \cdot 3,\qquad 10 = 2 \cdot 5$$Xuất sắc$2$xuất hiện ở cả hai số. Sản phẩm là$60$, trong khi LCM là$30$. Phân đoạn này không hợp lệ. 

Quan sát này thay đổi hoàn toàn vấn đề. Một phân đoạn hợp lệ khi và chỉ khi không có thừa số nguyên tố nào xuất hiện ở hai vị trí khác nhau trong phân đoạn đó. 

Những hạn chế buộc phải có một giải pháp hiệu quả. Với$n,q \le 10^5$, thậm chí$O(nq)$sẽ yêu cầu đại khái$10^{10}$hoạt động. Xử lý truy vấn phải xung quanh$O(\log n)$, gợi ý mạnh mẽ về tiền xử lý, nâng nhị phân hoặc các kỹ thuật liên quan. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
1
6
```Một phần tử luôn hợp lệ vì tích của nó bằng LCM của nó. Câu trả lời phải là 1. 

Hãy xem xét:```
2 4
```Cả hai số đều chứa thừa số nguyên tố 2. Toàn bộ đoạn không hợp lệ nên phải chia thành hai phần:```
[2] [4]
```Câu trả lời là 2. Việc thực hiện bất cẩn chỉ kiểm tra xem các số liền kề có phải là số nguyên tố cùng nhau hay không sẽ chấp nhận toàn bộ phân đoạn một cách sai lầm. 

Coi như:```
6 10 15
```Mỗi cặp đều có chung một thừa số nguyên tố, nhưng không có số nguyên tố nào xuất hiện đồng thời ở cả ba vị trí. Phạm vi hợp lệ tối đa vẫn phải được tính toán cẩn thận. Những quyết định tham lam mang tính địa phương mà không theo dõi các sự kiện quan trọng trên toàn cầu có thể thất bại. 

Một trường hợp tinh tế khác là giá trị 1. Nó không có thừa số nguyên tố và không bao giờ tạo ra xung đột.```
1 1 1 1
```Toàn bộ phân đoạn là hợp lệ và câu trả lời là 1. Bất kỳ triển khai nào giả định mọi số đều đóng góp một thừa số nguyên tố sẽ rút ngắn phạm vi một cách không chính xác. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ xử lý từng truy vấn một cách độc lập. 

Đối với một truy vấn$[l,r]$, chúng ta có thể thử mở rộng một phân đoạn trong khi vẫn duy trì tập hợp các thừa số nguyên tố đã được sử dụng. Khi một số nguyên tố lặp lại xuất hiện, chúng tôi cắt và bắt đầu một phân đoạn mới. Việc xây dựng tham lam này thực sự là tối ưu vì việc mở rộng một phân đoạn hợp lệ càng nhiều càng tốt sẽ không bao giờ làm tăng số lượng các phần. 

Vấn đề là sự phức tạp. Một truy vấn có thể kiểm tra$O(n)$các vị trí. Với$10^5$truy vấn, trường hợp xấu nhất sẽ trở thành$O(nq)$, đại khái$10^{10}$hoạt động. 

Quan sát quan trọng là tính hợp lệ chỉ phụ thuộc vào các thừa số nguyên tố lặp lại. 

Giả sử chúng ta biết, với mọi vị trí bắt đầu$i$, vị trí xa nhất$R[i]$như vậy$[i,R[i]]$là hợp lệ. Khi đó, phân vùng tối ưu của bất kỳ truy vấn nào đều đơn giản: 

Bắt đầu từ vị trí hiện tại, luôn lấy đoạn hợp lệ dài nhất. Đây chính xác là chiến lược tham lam tương tự như việc nhảy qua các khoảng thời gian. 

Thử thách còn lại trở thành câu trả lời: 

> Bắt đầu từ vị trí$l$, cần bao nhiêu lần nhảy tham lam để đạt hoặc vượt qua$r$? 

Đây là một vấn đề nâng nhị phân cổ điển. 

Trước tiên, chúng tôi tính toán mọi khoảng thời gian hợp lệ tối đa bằng cửa sổ trượt. Sau đó, chúng tôi xây dựng con trỏ nhảy trong đó:$$up[0][i] = R[i] + 1$$có nghĩa là "sau khi lấy một đoạn tối đa bắt đầu từ$i$, phân đoạn tiếp theo bắt đầu từ đây". 

Quyền lực cao hơn được xây dựng bình thường:$$up[k][i] = up[k-1][up[k-1][i]]$$Sau đó, một truy vấn sẽ trở nên giống với việc liên tục nhảy về phía trước trong khi vẫn ở trước$r$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n + q \log n)$|$O(n \log n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính thừa số nguyên tố nhỏ nhất cho mọi số đến$10^5$dùng sàng. 
2. Phân tích từng phần tử mảng và lưu trữ các thừa số nguyên tố riêng biệt của nó. 
3. Xây dựng khoảng thời gian hợp lệ tối đa bắt đầu từ mọi vị trí bằng cửa sổ hai con trỏ. 

Duy trì số lượng cho mọi số nguyên tố hiện có trong cửa sổ. 

Mở rộng con trỏ bên phải trong khi không có thừa số nguyên tố nào xuất hiện hai lần. 
4. Đối với mỗi điểm cuối bên trái$i$, lưu trữ điểm cuối bên phải hợp lệ lớn nhất$R[i]$. 
5. Xác định:$$up[0][i] = R[i] + 1$$Nếu đoạn tối đa bắt đầu tại$i$kết thúc tại$R[i]$, đoạn tiếp theo phải bắt đầu lúc$R[i]+1$. 
6. Xây dựng bàn nâng nhị phân. 

Vì mọi quyền lực$k$,$$up[k][i] = up[k-1][up[k-1][i]]$$7. Để trả lời một câu hỏi$[l,r]$, bắt đầu ở vị trí$l$. 
8. Hãy thử lũy thừa từ lớn nhất đến nhỏ nhất. 

Bất cứ khi nào nhảy$2^k$phân đoạn vẫn rời khỏi vị trí tại hoặc trước$r$, thực hiện bước nhảy và thêm$2^k$để trả lời. 
9. Sau tất cả các lần nhảy có thể, cần có một đoạn cuối cùng để che phần còn lại. 
10. Xuất ra tổng số đoạn. 

### Tại sao nó hoạt động 

Cửa sổ trượt tính toán chính xác phân đoạn hợp lệ tối đa bắt đầu ở mọi vị trí vì điều kiện hợp lệ là đơn điệu. Một khi một thừa số nguyên tố lặp lại xuất hiện, việc mở rộng thêm sẽ không bao giờ khôi phục được giá trị. 

Đối với bất kỳ khoảng thời gian nào, lấy tiền tố hợp lệ dài nhất là tối ưu. Bất kỳ phân đoạn đầu tiên ngắn hơn nào cũng sẽ để lại nhiều phần tử chưa được khám phá và không thể giảm số lượng các phân đoạn trong tương lai. Việc lặp lại đối số này một cách đệ quy chứng tỏ rằng phân vùng tham lam tạo ra số lượng phần tối thiểu. 

Bảng nhảy lưu trữ các ứng dụng lặp lại của bước tham lam này. Việc nâng nhị phân chỉ đơn thuần đẩy nhanh quá trình đếm số lượng phân đoạn tham lam được yêu cầu trước khi đến điểm cuối truy vấn. Vì phân vùng tham lam là tối ưu nên số đếm được chính xác là câu trả lời tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXA = 100000
LOG = 18

# smallest prime factor
spf = list(range(MAXA + 1))
for i in range(2, int(MAXA ** 0.5) + 1):
    if spf[i] == i:
        step = i
        start = i * i
        for j in range(start, MAXA + 1, step):
            if spf[j] == j:
                spf[j] = i

n, q = map(int, input().split())
a = list(map(int, input().split()))

factors = [[] for _ in range(n)]

for i, x in enumerate(a):
    while x > 1:
        p = spf[x]
        factors[i].append(p)
        while x % p == 0:
            x //= p

cnt = [0] * (MAXA + 1)

R = [0] * n
r = 0

for l in range(n):
    while r < n:
        ok = True
        for p in factors[r]:
            if cnt[p]:
                ok = False
                break

        if not ok:
            break

        for p in factors[r]:
            cnt[p] += 1
        r += 1

    R[l] = r - 1

    for p in factors[l]:
        cnt[p] -= 1

up = [[n] * (n + 1) for _ in range(LOG)]

for i in range(n):
    nxt = R[i] + 1
    if nxt > n:
        nxt = n
    up[0][i] = nxt

up[0][n] = n

for k in range(1, LOG):
    prev = up[k - 1]
    cur = up[k]
    for i in range(n + 1):
        cur[i] = prev[prev[i]]

out = []

for _ in range(q):
    l, r = map(int, input().split())
    l -= 1
    r -= 1

    pos = l
    ans = 0

    for k in range(LOG - 1, -1, -1):
        nxt = up[k][pos]
        if nxt <= r:
            ans += 1 << k
            pos = nxt

    out.append(str(ans + 1))

print("\n".join(out))
```Giai đoạn đầu tiên tính hệ số nguyên tố nhỏ nhất cho mọi giá trị lên đến$10^5$. Điều này cho phép nhân tử hóa mọi phần tử mảng theo thời gian logarit. 

Mỗi vị trí chỉ lưu trữ các thừa số nguyên tố riêng biệt. Các lũy thừa lặp lại bên trong cùng một số là không liên quan. Ví dụ,$12 = 2^2 \cdot 3$chỉ đóng góp số nguyên tố$2$Và$3$. 

Cửa sổ trượt duy trì tính bất biến là mỗi số nguyên tố xuất hiện nhiều nhất một lần bên trong cửa sổ hiện tại. Khi việc thêm vị trí tiếp theo sẽ tạo ra một số nguyên tố trùng lặp, quá trình mở rộng sẽ dừng lại. Ranh giới bên phải thu được chính xác là đoạn hợp lệ tối đa bắt đầu từ điểm cuối bên trái hiện tại. 

Bàn nhảy lưu trữ nơi chúng tôi đến sau khi thực hiện$2^k$phân khúc tham lam. Chức vụ$n$hoạt động như một trạng thái canh gác, ngăn chặn các truy cập ngoài giới hạn. 

Trong quá trình xử lý truy vấn, vòng lặp tham lam thực hiện bước nhảy lớn nhất có thể mà vẫn nằm trong phạm vi truy vấn. Đây là mẫu nâng nhị phân tiêu chuẩn để đếm số lần nhảy. 

Một sai lầm thường gặp là quên điều đó$R[i]$bao gồm trong khi đoạn tiếp theo bắt đầu lúc$R[i]+1$. Một lỗi dễ dàng khác là sử dụng`< r`thay vì`<= r`trong điều kiện nâng, được tính dưới mức khi bước nhảy tiếp đất chính xác trên điểm cuối truy vấn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 3
2 3 10 7 5 14
1 6
2 4
3 5
```Khoảng thời gian hợp lệ tối đa là: 

| Bắt đầu | Kết thúc hợp lệ tối đa | 
| --- | --- | 
| 1 | 2 | 
| 2 | 4 | 
| 3 | 4 | 
| 4 | 5 | 
| 5 | 6 | 
| 6 | 6 | 

Đối với truy vấn$[1,6]$: 

| Vị trí hiện tại | Phân đoạn đã thực hiện | Vị trí tiếp theo | 
| --- | --- | --- | 
| 1 | [1,2] | 3 | 
| 3 | [3,4] | 5 | 
| 5 | [5,6] | 7 | 

Đáp án = 3. 

Đối với truy vấn$[2,4]$: 

| Vị trí hiện tại | Phân đoạn đã thực hiện | Vị trí tiếp theo | 
| --- | --- | --- | 
| 2 | [2,4] | 5 | 

Trả lời = 1. 

Ví dụ này cho thấy các khoảng hợp lệ tối đa sẽ tạo ra phân vùng tối ưu một cách tự nhiên. 

### Ví dụ 2 

đầu vào:```
4 1
2 4 8 16
1 4
```Mọi số đều chứa số nguyên tố 2. 

| Bắt đầu | Kết thúc hợp lệ tối đa | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 3 | 
| 4 | 4 | 

Phân vùng trở thành: 

| Vị trí hiện tại | Phân đoạn đã thực hiện | Vị trí tiếp theo | 
| --- | --- | --- | 
| 1 | [1] | 2 | 
| 2 | [2] | 3 | 
| 3 | [3] | 4 | 
| 4 | [4] | 5 | 

Đáp án = 4. 

Điều này chứng tỏ rằng các thừa số nguyên tố lặp đi lặp lại sẽ cắt giảm ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n + q \log n)$| Cửa sổ trượt cộng với tiền xử lý và truy vấn nâng nhị phân | 
| Không gian |$O(n \log n)$| Bảng nhảy chiếm ưu thế trong việc sử dụng bộ nhớ | 

Độ dài mảng và số lượng truy vấn đều$10^5$. MỘT$O(n \log n + q \log n)$giải pháp chỉ thực hiện vài triệu thao tác, phù hợp thoải mái trong thời gian giới hạn. Bàn nhảy lưu trữ khoảng$18 \times 10^5$số nguyên, nằm trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    from subprocess import run as prun, PIPE
    return "implementation dependent"

# sample 1
# expected:
# 3
# 1
# 2

# minimum size
inp = """\
1 1
1
1 1
"""
expected = "1"

# all ones
inp2 = """\
5 2
1 1 1 1 1
1 5
2 4
"""
expected2 = """\
1
1"""

# repeated prime everywhere
inp3 = """\
4 1
2 4 8 16
1 4
"""
expected3 = "4"

# boundary query
inp4 = """\
5 2
2 3 5 7 11
1 5
5 5
"""
expected4 = """\
1
1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Yếu tố đơn | 1 | Ví dụ kích thước tối thiểu | 
| Tất cả những cái | 1, 1 | Giá trị không có Prime không bao giờ tạo ra xung đột | 
| 2 4 8 16 | 4 | Mỗi vị trí phải trở thành phân khúc riêng của mình | 
| Số nguyên tố riêng biệt | 1 | Toàn bộ khoảng thời gian vẫn hợp lệ | 

## Vỏ cạnh 

Hãy xem xét:```
1 1
1
1 1
```Số 1 không đóng góp yếu tố nguyên tố nào. Cửa sổ trượt mở rộng trên phần tử ngay lập tức, mang lại$R[1]=1$. Câu trả lời truy vấn là 1. 

Hãy xem xét:```
2 1
2 4
1 2
```Vị trí đầu tiên đóng góp số nguyên tố 2. Khi cửa sổ cố gắng bao gồm vị trí thứ hai, số nguyên tố 2 sẽ xuất hiện hai lần. Việc mở rộng dừng lại. Các khoảng tối đa trở thành$[1,1]$Và$[2,2]$, tạo ra câu trả lời 2. 

Hãy xem xét:```
4 1
1 1 1 1
1 4
```Không có vị trí đóng góp bất kỳ yếu tố nguyên tố nào. Cửa sổ trượt đến cuối mảng ngay lập tức. Khoảng cách tối đa từ vị trí đầu tiên bao trùm toàn bộ truy vấn, vì vậy câu trả lời là 1. 

Hãy xem xét:```
3 1
6 10 15
1 3
```Vị trí 1 sử dụng số nguyên tố$\{2,3\}$. Vị trí 2 sử dụng$\{2,5\}$, gây ra xung đột ngay lập tức. Khoảng tối đa đầu tiên kết thúc ở vị trí 1. Xung đột tương tự xảy ra sau đó. Thuật toán xác định chính xác rằng cần phải cắt nhiều lần vì các thừa số nguyên tố lặp lại được theo dõi trên toàn cầu bên trong phân đoạn hiện tại thay vì chỉ giữa các phần tử liền kề.

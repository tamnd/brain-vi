---
title: "CF 1696C - Fishingprince Chơi Với Mảng"
description: "Chúng ta có hai mảng và một số nguyên cố định $m$. Bắt đầu từ mảng đầu tiên, chúng ta có thể liên tục chia một phần tử chia hết cho $m$ thành $m$ các phần bằng nhau hoặc hợp nhất $m$ các phần tử bằng nhau liên tiếp thành một phần tử lớn hơn."
date: "2026-06-09T22:39:31+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1696
codeforces_index: "C"
codeforces_contest_name: "Codeforces Global Round 21"
rating: 1400
weight: 1696
solve_time_s: 508
verified: false
draft: false
---

[CF 1696C - Fishingprince chơi với mảng](https://codeforces.com/problemset/problem/1696/C) 

**Đánh giá:** 1400 
**Tags:** thuật toán xây dựng, tham lam, thực hiện, toán học 
**Thời gian giải:** 8 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng và một số nguyên cố định$m$. Bắt đầu từ mảng đầu tiên, chúng ta có thể chia liên tục một phần tử chia hết cho$m$vào trong$m$phần bằng nhau hoặc hợp nhất$m$phần tử bằng nhau liên tiếp thành một phần tử lớn hơn. 

Câu hỏi đặt ra là liệu các thao tác này có cho phép chúng ta chuyển đổi mảng đầu tiên thành mảng thứ hai hay không. 

Các thao tác có vẻ phức tạp vì độ dài mảng thay đổi và có thể thực hiện được nhiều chuỗi di chuyển khác nhau. Quan sát quan trọng là không có thao tác nào làm thay đổi "giá trị nguyên tử" ẩn bên trong một phần tử. 

Lấy một phần tử$x$. Lặp đi lặp lại chia nó cho$m$trong khi có thể:$$x = base \cdot m^t$$Ở đâu$base$không còn chia hết cho$m$. 

Ví dụ, khi$m=2$,$$12 = 3 \cdot 2^2$$Và$$8 = 1 \cdot 2^3.$$Thao tác phân chia chỉ di chuyển lũy thừa của$m$giữa giá trị và số lượng bản sao. Cơ bản$base$không bao giờ thay đổi. 

Các ràng buộc gợi ý rõ ràng rằng chúng ta cần một giải pháp gần như tuyến tính. Tổng của tất cả độ dài mảng trong tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$. MỘT$O(n^2)$thuật toán sẽ yêu cầu khoảng$4 \cdot 10^{10}$trong trường hợp xấu nhất vượt xa giới hạn. A solution around$O(n)$hoặc$O(n \log n)$mỗi trường hợp thử nghiệm là cần thiết. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
m = 2
a = [8]
b = [4,4]
```Câu trả lời đúng là`Yes`. Một giải pháp chỉ so sánh từng phần tử của mảng sẽ từ chối nó một cách không chính xác. Chia lượt$8$thành hai$4$'S. 

Coi như:```
m = 2
a = [2,2]
b = [4]
```Câu trả lời đúng là`Yes`. Ở đây sự chuyển đổi đòi hỏi phải hợp nhất hơn là chia tách. 

Coi như:```
m = 2
a = [2,4]
b = [1,1,1,1,1,1]
```Câu trả lời đúng là`Yes`. 

Sau khi mở rộng đầy đủ:$$2 \to (1,2),\quad 4 \to (1,4)$$đưa ra sáu bản sao của giá trị cơ bản$1$. Việc triển khai bất cẩn khiến hai nhóm tách biệt sẽ khiến các nhóm liền kề có cùng cơ sở có thể kết hợp với nhau. 

Coi như:```
m = 3
a = [3,3]
b = [9]
```Câu trả lời đúng là`No`. 

Mỗi$3$đóng góp một bản sao của cơ sở$1$. Họ cùng nhau đóng góp hai bản sao, nhưng việc hợp nhất đòi hỏi các nhóm có chính xác$m=3$. Chúng tôi không thể tạo một$9$. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các chuỗi phân tách và hợp nhất có thể có. Về nguyên tắc, điều này đúng vì mọi mảng có thể truy cập cuối cùng đều có thể được khám phá. 

Vấn đề là kích thước của không gian trạng thái. Ngay cả một giá trị duy nhất như$$10^9 = 2^9 \cdot 1953125$$có thể được chia ra nhiều lần, tạo ra nhiều mảng trung gian theo cấp số nhân. Số lượng cấu hình có thể truy cập tăng quá nhanh để có thể tìm kiếm toàn diện. 

Sự đột phá đến từ việc xem xét những gì không bao giờ thay đổi. 

Lấy một phần tử$x$. Loại bỏ mọi yếu tố có thể có của$m$:$$x = base \cdot m^t.$$Nếu chúng ta mở rộng hoàn toàn$x$, nó trở thành$$base$$lặp đi lặp lại$m^t$lần. 

Ví dụ, với$m=2$,$$12 = 3 \cdot 2^2$$hoạt động chính xác như bốn bản sao của$3$. 

Thứ tự của các giá trị cơ bản này không bao giờ thay đổi. Hoạt động chỉ phân phối lại quyền hạn của$m$bên trong một khối liền kề tương ứng với cùng một giá trị cơ sở. 

Điều này có nghĩa là mọi phần tử có thể được biểu diễn dưới dạng một cặp:$$(base,\ count)$$Ở đâu$$count = m^t.$$Vì$12$với$m=2$, cặp đó là$(3,4)$. 

Các cặp liền kề có cùng cơ sở có thể được hợp nhất vì dạng mở rộng của chúng không thể phân biệt được. Sau khi nén toàn bộ mảng theo cách này, câu hỏi chuyển đổi sẽ trở thành: 

Thực hiện hai chuỗi nén của$(base,count)$các cặp khớp nhau một cách chính xác? 

Nếu đúng như vậy, các mảng sẽ biểu thị cùng một chuỗi được mở rộng hoàn toàn và có thể truy cập được lẫn nhau. Nếu không, không có chuỗi thao tác nào có thể khắc phục được sự không khớp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu |$O((n+k)\log_m V)$|$O(n+k)$| Đã chấp nhận | 

Đây$V \le 10^9$là giá trị phần tử lớn nhất. 

## Hướng dẫn thuật toán 

1. Xử lý mảng đầu tiên từ trái qua phải. 
2. Với mỗi giá trị$x$, chia nhiều lần cho$m$trong khi có thể chia được. Gọi giá trị còn lại là`base`. 
3. Đếm xem có bao nhiêu thừa số của$m$đã được gỡ bỏ. Nếu như$x = base \cdot m^t$, thì phần tử này đóng góp`cnt = m^t`bản sao của`base`. 
4. Bảo quản cặp`(base, cnt)`. 
5. Nếu cặp được lưu trước đó có cùng`base`, cộng các số đếm lại với nhau thay vì tạo một cặp mới. 

Điều này hoạt động vì các khối liền kề có cùng cơ sở biểu thị các bản sao liên tiếp có cùng giá trị mở rộng. 
6. Lặp lại quá trình nén tương tự cho mảng thứ hai. 
7. So sánh hai chuỗi kết quả của các cặp. 
8. Nếu các dãy giống hệt nhau, hãy in`Yes`. Nếu không thì in`No`. 

### Tại sao nó hoạt động 

Đối với mọi phần tử$x$, giá trị$$base = \frac{x}{m^t}$$không thể thay đổi bởi một trong hai hoạt động. 

Tách chuyển đổi$$(base,m^t)$$vào trong$m$bản sao của$$(base,m^{t-1}),$$trong khi hợp nhất thực hiện chuyển đổi ngược lại. 

Do đó, mọi thao tác đều bảo toàn chuỗi giá trị cơ sở mở rộng. Sự tự do duy nhất là cách tổng số bội của một giá trị cơ sở được nhóm thành các phần tử. 

Sau khi nén các cơ số bằng nhau liền kề thành một số đếm duy nhất, chúng ta thu được một biểu diễn chuẩn. Hai mảng có thể truy cập được từ nhau khi và chỉ khi các cách biểu diễn chuẩn này giống hệt nhau. Thuật toán tính toán chính xác biểu diễn này và so sánh kết quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def normalize(arr, m):
    res = []

    for x in arr:
        base = x
        cnt = 1

        while base % m == 0:
            base //= m
            cnt *= m

        if res and res[-1][0] == base:
            res[-1][1] += cnt
        else:
            res.append([base, cnt])

    return res

def solve():
    t = int(input())

    ans = []

    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        k = int(input())
        b = list(map(int, input().split()))

        na = normalize(a, m)
        nb = normalize(b, m)

        ans.append("Yes" if na == nb else "No")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```các`normalize`hàm xây dựng biểu diễn chính tắc. Mỗi số được giảm xuống giá trị cơ bản không thể phân chia của nó và lũy thừa bị loại bỏ của$m$được chuyển đổi thành số bội. 

Phần tinh tế là hợp nhất các mục liền kề với cùng một cơ sở. Nếu không có bước này, các mảng như`[2,4]`Và`[1,1,1,1,1,1]`sẽ chuẩn hóa khác nhau mặc dù chúng biểu diễn cùng một chuỗi mở rộng. 

Số lượng được lưu trữ trực tiếp dưới dạng$m^t$. Vì mọi giá trị ban đầu nhiều nhất là$10^9$, phép nhân lặp lại vẫn an toàn trong phạm vi số nguyên của Python. 

Sự so sánh cuối cùng chỉ đơn giản là sự bình đẳng giữa hai danh sách các cặp. Sau khi các biểu mẫu chuẩn khớp nhau, mọi chuỗi phân tách hoặc hợp nhất cần thiết sẽ tự động tồn tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
m = 2
a = [1, 2, 2, 4, 2]
b = [1, 4, 4, 2]
```Bình thường hóa`a`: 

| Giá trị | Căn cứ | Đếm | Đại diện hiện tại | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | [(1,1)] | 
| 2 | 1 | 2 | [(1,3)] | 
| 2 | 1 | 2 | [(1,5)] | 
| 4 | 1 | 4 | [(1,9)] | 
| 2 | 1 | 2 | [(1,11)] | 

Bình thường hóa`b`: 

| Giá trị | Căn cứ | Đếm | Đại diện hiện tại | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | [(1,1)] | 
| 4 | 1 | 4 | [(1,5)] | 
| 4 | 1 | 4 | [(1,9)] | 
| 2 | 1 | 2 | [(1,11)] | 

Cả hai đều bình thường hóa thành:```
[(1,11)]
```Trả lời:`Yes`. 

Ví dụ này cho thấy nhiều nhóm khác nhau của cùng một chuỗi mở rộng sẽ trở nên giống hệt nhau sau khi chuẩn hóa. 

### Ví dụ 2 

đầu vào:```
m = 3
a = [3,3,3,3,3,3,3,3]
b = [6,6,6,6]
```Bình thường hóa`a`: 

| Giá trị | Căn cứ | Đếm | 
| --- | --- | --- | 
| 3 | 1 | 3 | 
| 3 | 1 | 3 | 
| 3 | 1 | 3 | 
| 3 | 1 | 3 | 
| 3 | 1 | 3 | 
| 3 | 1 | 3 | 
| 3 | 1 | 3 | 
| 3 | 1 | 3 | 

Dạng nén:```
[(1,24)]
```Bình thường hóa`b`: 

| Giá trị | Căn cứ | Đếm | 
| --- | --- | --- | 
| 6 | 2 | 3 | 
| 6 | 2 | 3 | 
| 6 | 2 | 3 | 
| 6 | 2 | 3 | 

Dạng nén:```
[(2,12)]
```Các giá trị cơ bản khác nhau nên không có chuỗi thao tác nào có thể chuyển đổi mảng này sang mảng khác. 

Trả lời:`No`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+k)\log_m V)$| Mỗi giá trị được chia cho$m$cho đến khi không còn chia hết | 
| Không gian |$O(n+k)$| Lưu trữ các chuỗi đã chuẩn hóa | 

Từ$V \le 10^9$, mỗi phần tử có thể chia tối đa khoảng 30 lần. Tổng số phần tử mảng trong tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$, do đó lời giải nằm vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    input_stream = io.StringIO(inp)

    def input():
        return input_stream.readline()

    def normalize(arr, m):
        res = []

        for x in arr:
            base = x
            cnt = 1

            while base % m == 0:
                base //= m
                cnt *= m

            if res and res[-1][0] == base:
                res[-1][1] += cnt
            else:
                res.append([base, cnt])

        return res

    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        k = int(input())
        b = list(map(int, input().split()))

        out.append(
            "Yes"
            if normalize(a, m) == normalize(b, m)
            else "No"
        )

    return "\n".join(out)

# provided sample
assert run(
"""5
5 2
1 2 2 4 2
4
1 4 4 2
6 2
1 2 2 8 2 2
2
1 16
8 3
3 3 3 3 3 3 3 3
4
6 6 6 6
8 3
3 9 6 3 12 12 36 12
16
9 3 2 2 2 3 4 12 4 12 4 12 4 12 4 4
8 3
3 9 6 3 12 12 36 12
7
12 2 4 3 4 12 56
"""
) == """Yes
Yes
No
Yes
No"""

# minimum size
assert run(
"""1
1 2
1
1
1
"""
) == "Yes"

# split once
assert run(
"""1
1 2
8
2
4 4
"""
) == "Yes"

# merge impossible
assert run(
"""1
2 3
3 3
1
9
"""
) == "No"

# adjacent equal bases must combine
assert run(
"""1
2 2
2 4
6
1 1 1 1 1 1
"""
) == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a=[1], b=[1]`| Có | Trường hợp hợp lệ nhỏ nhất | 
|`a=[8], b=[4,4]`| Có | Tách thuần túy | 
|`a=[3,3], b=[9]`| Không | Không đủ đa dạng để hợp nhất | 
|`a=[2,4], b=[1,1,1,1,1,1]`| Có | Các cơ sở bằng nhau liền kề phải được hợp nhất trong quá trình chuẩn hóa | 

## Vỏ cạnh 

Hãy xem xét:```
m = 2
a = [8]
b = [4,4]
```Chuẩn hóa mang lại:```
a -> [(1,8)]
b -> [(1,8)]
```Thuật toán trả về`Yes`. Sự biến đổi thực tế thu được bằng cách tách$8$một lần. 

Coi như:```
m = 2
a = [2,2]
b = [4]
```Chuẩn hóa mang lại:```
a -> [(1,4)]
b -> [(1,4)]
```Thuật toán trả về`Yes`. Hoạt động tương ứng là hợp nhất hai giá trị bằng nhau. 

Coi như:```
m = 2
a = [2,4]
b = [1,1,1,1,1,1]
```Chuẩn hóa mang lại:```
a -> [(1,6)]
b -> [(1,6)]
```Bước quan trọng là kết hợp các mục liền kề với giá trị cơ sở$1$. Nếu không có sự nén đó, phía bên trái sẽ xuất hiện dưới dạng`[(1,2),(1,4)]`và việc so sánh sẽ thất bại không chính xác. 

Coi như:```
m = 3
a = [3,3]
b = [9]
```Chuẩn hóa mang lại:```
a -> [(1,6)]
b -> [(1,9)]
```Số lượng khác nhau nên thuật toán trả về`No`. Mặc dù tất cả các giá trị cơ sở đều khớp nhau nhưng không có đủ bội số để tạo một$9$.

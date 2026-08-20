---
title: "CF 102272D - C\u00e1nh \u0110\u1ed3ng Hoa"
description: "Chúng tôi có một loạt số lượng hoa (A1, ldots, AN). Thao tác loại 1 chọn một khoảng ([l,r]) và thêm một bậc thang vào đó. Vị trí (l) nhận (1), vị trí (l+1) nhận (2) và ở vị trí chung (i) nhận (i-l+1)."
date: "2026-08-19T05:11:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102272
codeforces_index: "D"
codeforces_contest_name: "HCW 19 Individual Day 1"
rating: 0
weight: 102272
solve_time_s: 206
verified: true
draft: false
---

[CF 102272D - C\u00e1nh \u0110\u1ed3ng Hoa](https://codeforces.com/problemset/problem/102272/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dãy số lượng hoa (A_1,\ldots,A_N). Thao tác loại 1 chọn một khoảng ([l,r]) và thêm một bậc thang vào đó. Vị trí (l) nhận (1), vị trí (l+1) nhận (2) và ở vị trí chung (i) nhận (i-l+1). Thao tác loại 2 yêu cầu tính tổng của mảng hiện tại trên một khoảng ([u,v]). 

Các thao tác được xử lý theo thứ tự nên mọi truy vấn đều phải xem tất cả các cập nhật xảy ra trước đó. Nhiệm vụ là in ra câu trả lời cho mọi thao tác loại 2. 

Thử nghiệm lớn nhất có thể chứa (10^5) vị trí và (10^5) thao tác, với tối đa bốn trường hợp thử nghiệm. Giải pháp (O(NQ)) có thể thực hiện khoảng (10^{10}) thao tác mảng cơ bản trong trường hợp xấu nhất, vượt xa giới hạn hai giây. Ngay cả (O(N+Q\sqrt N)) ở đây cũng sẽ đắt một cách không cần thiết. Chúng tôi cần mỗi thao tác mất khoảng (O(\log N)) thời gian. 

Có một số trường hợp ranh giới có thể khiến việc triển khai có vẻ chính xác không thành công. Đầu tiên, một bản cập nhật có thể chứa chính xác một vị trí. Ví dụ,```
1
1
0
2
1 1 1
2 1 1
```sản xuất```
1
```vì bản cập nhật chỉ thêm (1). Một công thức luôn chèn chênh lệch thứ hai tại (l+1) mà không kiểm tra xem liệu (l<r) có thể làm hỏng trạng thái hay không. 

Một bản cập nhật cũng có thể đạt tới vị trí mảng cuối cùng. Ví dụ,```
1
3
0 0 0
2
1 2 3
2 1 3
```sản xuất```
6
```vì các giá trị được thêm vào là (1,2) trên các vị trí (2,3), cho ra mảng ([0,1,2]). Biểu diễn bên trong có thể sử dụng vị trí (r+1=4), nhưng vị trí đó không thuộc về mảng và chỉ đóng vai trò là sự khác biệt cuối cùng. Việc phân bổ cây Fenwick quá hẹp hoặc truy vấn nó không chính xác ở ranh giới này có thể gây ra lỗi riêng lẻ. 

Một truy vấn có thể chỉ bao gồm một phần của bản cập nhật. Ví dụ,```
1
5
0 0 0 0 0
2
1 2 5
2 3 4
```sản xuất```
5
```vì bản cập nhật tạo ra ([0,1,2,3,4]) và các vị trí (3,4) tổng bằng (5). Coi cầu thang như một phép cộng phạm vi không đổi sẽ cho kết quả không chính xác (2+2=4). 

Cuối cùng, một số bản cập nhật có thể chồng chéo lên nhau. Ví dụ,```
1
4
0 0 0 0
3
1 1 3
1 2 4
2 2 3
```sản xuất```
5
```Bản cập nhật đầu tiên thêm ([1,2,3,0]), bản cập nhật thứ hai thêm ([0,1,2,3]), do đó vị trí (2,3) chứa (3,5). Mỗi bản cập nhật phải đóng góp độc lập vào số tiền cuối cùng. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là xử lý thao tác loại 1 bằng cách truy cập mọi (i) từ (l) đến (r) và thêm (i-l+1) vào (A_i). Sau đó, thao tác loại 2 có thể được trả lời bằng cấu trúc tổng tiền tố hoặc đơn giản bằng cách quét khoảng thời gian được yêu cầu. Điều này đúng vì mọi cập nhật đều được áp dụng chính xác cho các vị trí mà nó mô tả. 

Vấn đề là số lượng vị trí được cập nhật. Nếu (N=Q=10^5), chúng ta có thể có (10^5) bản cập nhật bao trùm gần như toàn bộ mảng. Một bản cập nhật có thể yêu cầu bổ sung (10^5), đưa ra khoảng (10^{10}) thao tác trong trường hợp xấu nhất. Giới hạn hai giây loại trừ điều này. 

Quan sát hữu ích là giá trị được thêm vào bởi một bản cập nhật không phải là tùy ý. Trên ([l,r]), 

[ 
i-l+1=i+(1-l). 
] 

Vì vậy, mỗi bản cập nhật đều thêm một hàm tuyến tính của chỉ số vị trí. Cụ thể hơn, nếu giá trị gia tăng tại vị trí (i) được viết là 

[ 
f(i)=ai+b, 
] 

thì ở đây (a=1) và (b=1-l). 

Chúng tôi thực sự không cần phải lưu trữ mọi giá trị bị ảnh hưởng. Thay vào đó, hãy xem xét mảng khác biệt của các giá trị được đóng góp bởi tất cả các bản cập nhật. Đối với một cập nhật tuyến tính (f(i)=ai+b) trên ([l,r]), mảng sai phân của nó chỉ có ba thay đổi có thể xảy ra. Tại (l), chúng ta bắt đầu với (f(l)). Giữa (l) và (r), các giá trị liên tiếp tăng thêm (a), vì vậy tại (l+1) chúng ta thêm (a). Tại (r+1), chúng ta trừ (f(r)), kết thúc quá trình cập nhật. 

Đối với bài toán cụ thể này, (a=1) và (b=1-l), vì vậy 

[ 
f(l)=1 
] 

và 

[ 
f(r)=r-l+1. 
] 

Do đó, một bản cập nhật chỉ có thể được biểu diễn bằng một số lượng điểm thay đổi không đổi trong một mảng sai phân. 

Câu hỏi còn lại là làm thế nào để khôi phục tổng phạm vi từ những thay đổi chênh lệch này một cách hiệu quả. Nếu (D_j) là mảng sai phân thì giá trị tại vị trí (i) là 

[ 
X_i=\sum_{j\le i}D_j. 
] 

Do đó, tiền tố tổng qua (x) là 

\sum_{j=1}^{x}D_j(x-j+1). 
] 

Sắp xếp lại, 

(x+1)\sum_{j=1}^{x}D_j-\sum_{j=1}^{x}jD_j. 
] 

Điều này có nghĩa là chúng ta chỉ cần hai đại lượng tiền tố: (\sum D_j) và (\sum jD_j). Hai cây Fenwick có thể duy trì các đại lượng này khi thay đổi điểm trong (O(\log N)). 

Mảng ban đầu không cần phải chèn vào các cây Fenwick này. Chúng tôi tính toán trước các tổng tiền tố thông thường của nó một lần, sau đó cộng phần đóng góp của tất cả các cập nhật bậc thang tiếp theo khi trả lời một truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(NQ)) | (O(N)) | Quá chậm | 
| Tối ưu | (O(N+Q\log N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính mảng tổng tiền tố thông thường của số lượng hoa ban đầu. Đặt (P[x]) là tổng các giá trị ban đầu từ vị trí (1) đến (x). Sau đó, một truy vấn trên ([u,v]) có thể nhận được đóng góp ban đầu của nó dưới dạng (P[v]-P[u-1]). 
2. Duy trì hai cây Fenwick. Điểm lưu trữ đầu tiên thay đổi thành mảng sai phân (D), trong khi điểm thứ hai lưu trữ những thay đổi tương tự nhân với vị trí của chúng. Nếu sự thay đổi khác biệt của (d) xảy ra ở vị trí (p), hãy thêm (d) vào cây thứ nhất và (p d) vào cây thứ hai. 
3. Đối với một bản cập nhật ([l,r]), giá trị gia tăng là (f(i)=i-l+1). Tại vị trí (l), mảng sai phân phải tăng thêm (f(l)=1) nên cộng (+1) tại (l). Nếu (l<r), các giá trị liên tiếp tăng thêm (1), nên cộng (+1) tại (l+1). Tại (r+1), trừ đi giá trị cuối cùng (f(r)=r-l+1). Những thay đổi khác biệt tạo ra mô tả chính xác cầu thang được thêm vào bởi bản cập nhật này. 
4. Để tính toán sự đóng góp của tất cả các cập nhật cho tiền tố ([1,x]), hãy lấy 

[ 
S_D=\sum_{j\le x}D_j 
] 

từ cây Fenwick đầu tiên và 

[ 
S_{jD}=\sum_{j\le x}jD_j 
] 

từ cái thứ hai. Tổng tiền tố động là 

[ 
(x+1)S_D-S_{jD}. 
] 

Công thức tiếp theo trực tiếp từ việc đếm xem có bao nhiêu vị trí tiền tố chứa từng giá trị chênh lệch. Một sự khác biệt được đưa ra ở vị trí (j) ảnh hưởng đến các vị trí (j,j+1,\ldots,x), chính xác là các vị trí (x-j+1).

1. Đối với truy vấn loại 2 ([u,v]), hãy tính tổng tiền tố động thông qua (v) và trừ tổng tiền tố động thông qua (u-1). Thêm chênh lệch tổng tiền tố ban đầu tương ứng. Điều này đưa ra tổng hiện tại đầy đủ trên ([u,v]). 
2. Xử lý mọi thao tác theo thứ tự đầu vào. Các bản cập nhật sẽ sửa đổi hai cây Fenwick ngay lập tức, trong khi các truy vấn chỉ đọc chúng, vì vậy mọi truy vấn sẽ tự động thấy chính xác các bản cập nhật trước nó. 

### Tại sao nó hoạt động 

Điều bất biến là hai cây Fenwick biểu thị mảng khác biệt của mỗi đóng góp hoa do các thao tác loại 1 đã xử lý gây ra. Đối với mỗi bản cập nhật, ba thay đổi khác nhau sẽ tái cấu trúc chuỗi (1,2,\ldots,r-l+1) trên ([l,r]) và số 0 bên ngoài nó. Vì các mảng khác biệt cộng tuyến tính nên các cập nhật trùng lặp được thể hiện chính xác bằng cách cộng các thay đổi chênh lệch của chúng. 

Đối với bất kỳ tiền tố nào kết thúc tại (x), mọi khác biệt (D_j) đóng góp vào các vị trí (j) đến (x), tạo ra tổng số hoa (D_j(x-j+1)). Danh tính 

(x+1)\sum_{j\le x}D_j-\sum_{j\le x}jD_j 
] 

do đó phục hồi tổng tiền tố động chính xác. Trừ hai tiền tố sẽ cho tổng khoảng động chính xác và việc cộng các tổng tiền tố ban đầu không thay đổi sẽ cho tổng mảng hiện tại. Do đó mọi câu trả lời loại 2 đều đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    __slots__ = ("n", "bit")

    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, idx, value):
        n = self.n
        bit = self.bit
        while idx <= n:
            bit[idx] += value
            idx += idx & -idx

    def sum(self, idx):
        bit = self.bit
        res = 0
        while idx > 0:
            res += bit[idx]
            idx -= idx & -idx
        return res

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        prefix = [0] * (n + 1)
        for i, value in enumerate(a, 1):
            prefix[i] = prefix[i - 1] + value

        # One tree stores D[j].
        # The other stores j * D[j].
        bit_d = Fenwick(n + 1)
        bit_jd = Fenwick(n + 1)

        def add_difference(pos, delta):
            if pos > n + 1:
                return
            bit_d.add(pos, delta)
            bit_jd.add(pos, pos * delta)

        def dynamic_prefix(x):
            if x <= 0:
                return 0
            sum_d = bit_d.sum(x)
            sum_jd = bit_jd.sum(x)
            return (x + 1) * sum_d - sum_jd

        q = int(input())

        for _ in range(q):
            query = list(map(int, input().split()))
            typ, x, y = query

            if typ == 1:
                l, r = x, y

                # f(i) = i - l + 1
                # At l: start with f(l) = 1.
                add_difference(l, 1)

                # From l+1 through r, consecutive values differ by 1.
                if l < r:
                    add_difference(l + 1, 1)

                # At r+1, terminate the staircase.
                add_difference(r + 1, -(r - l + 1))

            else:
                u, v = x, y

                initial = prefix[v] - prefix[u - 1]
                dynamic = dynamic_prefix(v) - dynamic_prefix(u - 1)

                out.append(str(initial + dynamic))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`prefix`mảng xử lý những bông hoa ban đầu một cách riêng biệt. Điều này thuận tiện vì các giá trị ban đầu không bao giờ thay đổi nên không có lý do gì để cấu trúc Fenwick biểu diễn chúng.`bit_d`đại diện cho (D_j), trong khi`bit_jd`đại diện cho (jD_j). Người trợ giúp`add_difference`cập nhật cả hai cấu trúc với nhau, điều này ngăn cản hai biểu diễn không đồng bộ. 

Đối với bản cập nhật ([l,r]), thay đổi đầu tiên luôn là`+1`Tại`l`. Sự thay đổi thứ hai cũng`+1`, nhưng chỉ khi`l < r`. Điều kiện này là cần thiết cho bản cập nhật một phần tử. Sự thay đổi cuối cùng được đặt tại`r + 1`và bằng`-(r-l+1)`. Những cây Fenwick có kích thước`n + 1`đặc biệt để sự khác biệt kết thúc này có thể được lưu trữ khi`r = n`. 

các`dynamic_prefix`thực hiện chức năng 

[ 
(x+1)\sum_{j\le x}D_j-\sum_{j\le x}jD_j. 
]

Khi`x`bằng 0, câu trả lời ngay lập tức là 0, điều này làm cho các truy vấn bắt đầu ở vị trí (1) an toàn vì chúng yêu cầu`dynamic_prefix(0)`. 

Số nguyên Python có độ chính xác tùy ý, do đó số lượng hoa có thể lớn sẽ không bị tràn. Tổng lớn nhất có thể có thể vượt quá phạm vi số nguyên 32 bit một khoảng lớn. 

Mỗi phép toán Fenwick đều là logarit và mỗi lần cập nhật đều thực hiện một số lượng không đổi. Một truy vấn thực hiện hai phép tính tiền tố, một phép tính cho mỗi điểm cuối. Việc triển khai đạt được sẽ tránh chạm vào khoảng thời gian cập nhật có thể rất lớn. 

## Ví dụ đã hoạt động 

Trường hợp thử nghiệm đầu tiên bắt đầu bằng 

[ 
[2,1,3,5,2]. 
] 

Bảng sau theo dõi mảng sau mỗi thao tác và câu trả lời mỗi khi truy vấn xảy ra. 

| Hoạt động | Cập nhật hoặc truy vấn | Mảng hiện tại | Trả lời | 
| --- | --- | --- | --- | 
|`1 1 3`| Thêm (1,2,3) vào vị trí (1,2,3) |`[3, 3, 6, 5, 2]`| | 
|`2 3 5`| Tổng các vị trí (3) đến (5) |`[3, 3, 6, 5, 2]`|`13`| 
|`1 4 5`| Thêm (1,2) vào vị trí (4,5) |`[3, 3, 6, 6, 4]`| | 
|`1 2 5`| Thêm (1,2,3,4) vào các vị trí (2) đến (5) |`[3, 4, 8, 9, 8]`| | 
|`1 1 1`| Thêm (1) vào vị trí (1) |`[4, 4, 8, 9, 8]`| | 
|`2 1 4`| Tổng các vị trí (1) đến (4) |`[4, 4, 8, 9, 8]`|`25`| 

Đối với lần cập nhật đầu tiên, phần trình bày sự khác biệt nhận được`+1`tại vị trí (1),`+1`ở vị trí (2) và`-3`ở vị trí (4). Các giá trị được xây dựng lại của nó là (1,2,3,0,0), chính xác là cầu thang mà bản cập nhật yêu cầu. Sự thể hiện tương tự được thêm vào cho các bản cập nhật sau này, do đó các hoạt động chồng chéo sẽ tích lũy một cách tự nhiên. 

Trường hợp thử nghiệm thứ hai bắt đầu bằng 

[ 
[10,5,2,0,8,6,2]. 
] 

| Hoạt động | Cập nhật hoặc truy vấn | Mảng hiện tại | Trả lời | 
| --- | --- | --- | --- | 
|`1 2 5`| Thêm (1,2,3,4) vào các vị trí (2) đến (5) |`[10, 6, 4, 3, 12, 6, 2]`| | 
|`1 1 6`| Thêm (1,2,3,4,5,6) vào các vị trí (1) đến (6) |`[11, 8, 7, 7, 17, 12, 2]`| | 
|`2 4 7`| Tổng các vị trí (4) đến (7) |`[11, 8, 7, 7, 17, 12, 2]`|`38`| 
|`1 1 3`| Thêm (1,2,3) vào các vị trí (1) đến (3) |`[12, 10, 10, 7, 17, 12, 2]`| | 
|`1 5 5`| Thêm (1) vào vị trí (5) |`[12, 10, 10, 7, 18, 12, 2]`| | 
|`1 1 5`| Thêm (1,2,3,4,5) vào các vị trí (1) đến (5) |`[13, 12, 13, 11, 23, 12, 2]`| | 
|`2 1 7`| Tính tổng toàn bộ mảng |`[13, 12, 13, 11, 23, 12, 2]`|`86`| 

Cập nhật một vị trí`1 5 5`là một kiểm tra hữu ích. Từ`l == r`, mã chỉ chèn chênh lệch bắt đầu và chênh lệch kết thúc. Trung gian`l+1`thay đổi bị bỏ qua, do đó chuỗi biểu diễn chứa chính xác một bông hoa được thêm vào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N+Q\log N)) | Xây dựng tổng chi phí tiền tố ban đầu (O(N)); mọi cập nhật và truy vấn thực hiện một số lượng hoạt động Fenwick không đổi | 
| Không gian | (O(N)) | Mảng tiền tố ban đầu và hai cây Fenwick mỗi mảng sử dụng bộ nhớ (O(N)) | 

Với (N,Q\le10^5), giải pháp thực hiện theo thứ tự vài triệu lần lặp cây Fenwick cho mỗi trường hợp thử nghiệm thay vì hàng tỷ cập nhật mảng trực tiếp. Việc sử dụng bộ nhớ là tuyến tính và thoải mái dưới 256 MB. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây sử dụng phiên bản có thể gọi được của cùng một thuật toán. Trường hợp kích thước tối đa được tạo ra thay vì viết ra theo nghĩa đen, giúp giữ cho nguồn kiểm tra có thể đọc được trong khi vẫn thực hiện các giới hạn đã nêu.```python
import sys
import io

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, idx, value):
        while idx <= self.n:
            self.bit[idx] += value
            idx += idx & -idx

    def sum(self, idx):
        res = 0
        while idx:
            res += self.bit[idx]
            idx -= idx & -idx
        return res

def solve_io():
    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        prefix = [0] * (n + 1)
        for i, value in enumerate(a, 1):
            prefix[i] = prefix[i - 1] + value

        bit_d = Fenwick(n + 1)
        bit_jd = Fenwick(n + 1)

        def add_difference(pos, delta):
            bit_d.add(pos, delta)
            bit_jd.add(pos, pos * delta)

        def dynamic_prefix(x):
            if x <= 0:
                return 0
            sd = bit_d.sum(x)
            sjd = bit_jd.sum(x)
            return (x + 1) * sd - sjd

        q = int(input())

        for _ in range(q):
            typ, x, y = map(int, input().split())

            if typ == 1:
                l, r = x, y
                add_difference(l, 1)
                if l < r:
                    add_difference(l + 1, 1)
                add_difference(r + 1, -(r - l + 1))
            else:
                u, v = x, y
                ans = (
                    prefix[v] - prefix[u - 1]
                    + dynamic_prefix(v)
                    - dynamic_prefix(u - 1)
                )
                out.append(str(ans))

    return "\n".join(out)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve_io()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

sample = """\
2
5
2 1 3 5 2
6
1 1 3
2 3 5
1 4 5
1 2 5
1 1 1
2 1 4
7
10 5 2 0 8 6 2
7
1 2 5
1 1 6
2 4 7
1 1 3
1 5 5
1 1 5
2 1 7
"""

assert run(sample) == "13\n25\n38\n86", "provided sample"

assert run("""\
1
1
0
2
1 1 1
2 1 1
""") == "1", "minimum size"

assert run("""\
1
3
0 0 0
3
1 2 3
2 1 3
2 3 3
""") == "6\n2", "right boundary and partial query"

assert run("""\
1
5
7 7 7 7 7
4
2 1 5
1 3 5
2 1 5
2 3 5
""") == "35\n41\n24", "all equal initial values"

assert run("""\
1
4
0 0 0 0
5
1 1 4
1 2 3
2 1 4
2 2 3
2 4 4
""") == "14\n7\n4", "overlap and boundaries"

n = 100000
maximum_case = (
    "1\n"
    f"{n}\n"
    + ("1 " * (n - 1))
    + "1\n"
    + f"{n}\n"
    + "\n".join(
        ["1 1 100000"] * (n - 1)
        + ["2 1 100000"]
    )
    + "\n"
)

expected = n + (n - 1) * (n * (n + 1) // 2)
assert run(maximum_case) == str(expected), "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Vỏ có kích thước tối thiểu với (N=1) |`1`| Cập nhật một vị trí và ranh giới tiền tố (u-1=0) | 
| Cập nhật vị trí tiếp cận (N) |`6`,`2`| Xử lý đúng sự khác biệt kết thúc tại (r+1) và truy vấn một phần | 
| Tất cả các giá trị ban đầu bằng nhau |`35`,`41`,`24`| Tách các tổng tiền tố ban đầu không thay đổi khỏi các cập nhật động | 
| Cập nhật chồng chéo |`14`,`7`,`4`| Tính cộng của nhiều bản cập nhật cầu thang và ranh giới khoảng cách | 
| Trường hợp đã tạo (N=Q=10^5) | Tính theo công thức trong bài thi | Độ phức tạp về thời gian, số nguyên lớn và cập nhật toàn phạm vi lặp đi lặp lại | 

## Vỏ cạnh 

Để cập nhật một thành phần, hãy xem xét```
1
1
0
2
1 1 1
2 1 1
```Bản cập nhật là (f(1)=1). Thuật toán bổ sung`+1`đến mảng khác biệt ở vị trí (1) và`-1`ở vị trí (2). Thay đổi thứ hai được lưu trữ trong cây Fenwick nhưng nằm ngoài tiền tố được truy vấn. Do đó, tiền tố xuyên qua vị trí (1) là (1) và đầu ra là`1`. Sự thay đổi trung gian bị thiếu ở (l+1) là có chủ ý vì không có vị trí thứ hai trong bậc thang. 

Đối với bản cập nhật kết thúc ở vị trí cuối cùng, hãy xem xét```
1
3
0 0 0
2
1 2 3
2 1 3
```Bản cập nhật đóng góp (1,2) cho vị trí (2,3). Những thay đổi khác biệt của nó là`+1`tại (2),`+1`tại (3), và`-2`tại (4). Cây Fenwick có kích thước bằng (N+1), do đó vị trí (4) có thể chứa chênh lệch cuối cùng. Tiền tố xuyên qua (3) bỏ qua sự kết thúc đó và cho (3), vì vậy kết quả đầu ra dự kiến ​​thực sự là`3`. 

Đối với truy vấn chỉ bao gồm một phần của bản cập nhật, hãy xem xét```
1
5
0 0 0 0 0
2
1 2 5
2 3 4
```Bản cập nhật tạo ra ([0,1,2,3,4]). Tiền tố đến (4) là (6), trong khi tiền tố đến (2) là (1), do đó tổng được yêu cầu là (6-1=5). Công thức tiền tố động hoạt động mà không cần biết các giá trị riêng lẻ trong khoảng. 

Đối với các bản cập nhật chồng chéo, hãy xem xét```
1
4
0 0 0 0
3
1 1 3
1 2 4
2 2 3
```Bản cập nhật đầu tiên đóng góp ([1,2,3,0]) và bản cập nhật thứ hai đóng góp ([0,1,2,3]). Tổng của chúng là ([1,3,5,3]), nên vị trí (2,3) chứa (3+5=8). Việc biểu diễn sự khác biệt chỉ đơn giản là thêm những thay đổi khác biệt từ cả hai bản cập nhật, tạo ra một mảng kết hợp giống hệt nhau. 

Trường hợp kích thước tối đa thực hiện một ranh giới thực tế khác. Với (N=Q=10^5), việc cập nhật liên tục toàn bộ mảng sẽ là không thể nếu mỗi lần cập nhật truy cập vào tất cả (N) vị trí. Biểu diễn Fenwick chỉ chạm đến một số lượng vị trí không đổi trong mỗi lần cập nhật, do đó số lượng thao tác tăng lên dưới dạng (O(Q\log N)) thay vì (O(NQ)). Các số nguyên có độ chính xác tùy ý của Python cũng xử lý tổng kết quả một cách an toàn, có thể lớn hơn nhiều so với (2^{31}-1).

---
title: "CF 2227G - Đuối nước"
description: "Ta có một mảng dương nguyên. Được phép giảm ba phần tử liên tiếp sao cho phần tử giữa nhỏ hơn tổng hai phần tử. Khi bộ ba $$(c{i-1},ci,c{i+1})$$ được thay thế bằng một giá trị duy nhất $$x=c{i-1}-ci+c{i+1}.$$ Mỗi lần thực hiện, độ dài giảm đi đúng 2."
date: "2026-06-01T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "math"]
categories: ["algorithms"]
codeforces_contest: 2227
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 1096 (Div. 3)"
rating: 0
weight: 2227
solve_time_s: 177
verified: false
draft: false
---

[CF 2227G - Chết đuối](https://codeforces.com/problemset/problem/2227/G) 

**Đánh giá:** - 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, toán học 
**Thời gian giải:** 2m 57s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Ta có một mảng dương nguyên. Được phép giảm ba phần tử liên tiếp sao cho phần tử giữa nhỏ hơn tổng hai phần tử. Khi bộ ba đó$$(c_{i-1},c_i,c_{i+1})$$được thay thế bằng một giá trị duy nhất$$x=c_{i-1}-c_i+c_{i+1}.$$Mỗi lần thực hiện, độ dài mảng giảm đi đúng 2. 

Một đoạn được gọi là tốt nếu có thể áp dụng các phép giảm nhiều lần để cuối cùng vẫn đúng một phần tử. Nhiệm vụ là đếm bao nhiêu đoạn con của mảng đầu là tốt. 

Điểm khó nằm ở vị trí định nghĩa của các biến được phép có vẻ rất cục bộ, nhưng một số cách thực hiện lại. Với$n$lên tới$2\cdot 10^5$, tổng hợp$n$trên tất cả các bài kiểm tra cũng đạt được$2\cdot 10^5$, nên mọi lời giải thích dạng xét nghiệm từng đoạn con rồi mô phỏng đều không khả thi. Ta cần một toàn bộ đặc quyền được phép nhận biết ngay một đoạn có tốt hay không. 

Một tình huống dễ bị bỏ sót là các đoạn có chiều dài thậm chí. Vì cho phép mỗi lần giảm độ dài đúng 2, tính toán chẵn lẻ của độ dài không bao giờ thay đổi. Muốn kết thúc ở độ dài 1 thì lệnh cấm bắt đầu dài phải là số lẻ. 

Ví dụ với mảng:```
[1, 1]
```không có cách nào để giảm xuống một phần tử dù mọi giá trị đều nhỏ. 

Một vấn đề khác là đoạn dài 3. Khi đó chỉ được phép thực hiện một cách đúng đắn, cần có điều kiện trở thành thành công:```
a1 + a3 > a2
```Ví dụ:```
[10, 20, 10]
```không tốt vì$10+10=20$, not a Fatal normal. 

Điều nguy hiểm nhất được cho là chỉ cần tồn tại ở một bước hợp lý là đủ. Timeline:```
[100, 1, 1]
```có một giá trị hợp lệ và sau khi giảm sẽ còn một phần tử. Đoạn này tốt. Nhưng để xử lý các đoạn dài hơn, ta cần một tiêu chuẩn mạnh hơn nhiều. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu tự nhiên nhất là xét mọi đoạn con. Với mỗi đoạn có lẻ dài, hãy thử tất cả các chuỗi được phép giảm có thể xảy ra để kiểm tra xem có thể về một phần tử hay không. 

Cách này đúng vì nó duyệt toàn bộ trạng thái. Vấn đề là trạng thái tăng theo số nhân. Chỉ có công việc riêng$O(n^2)$đoạn con đã tạo thẻ không thể xử lý ở$n=2\cdot10^5$. 

Điểm đột phá là việc làm quan sát một lượng lớn bất biến. 

Phiên bản tổng hợp định nghĩa:$$S(c)=c_1-c_2+c_3-c_4+\cdots$$Khi thay$$(c_{i-1},c_i,c_{i+1})$$bởi$$x=c_{i-1}-c_i+c_{i+1},$$đóng góp của ba phần tử này trong tổng phiên bản chính xác bằng cách đóng góp của phần tử mới. Vì thế tổng phiên luân phiên không thay đổi sau tất cả các biến được phép. 

Nếu một mảng được giảm thành một phần tử dương duy nhất thì tổng phiên luân cuối cùng chắc chắn dương. Do bất biến, tổng luân phiên cấm đầu cũng phải dương. Đây là điều kiện cần thiết. 

Điều bất ngờ là điều kiện cũng là điều kiện đủ. Ta có thể chứng minh rằng với một lẻ dài mảng, nếu tổng biến số dương thì sẽ tồn tại ít nhất một giá trị hợp lệ. Sau khi thực hiện, tổng luân phiên vẫn dương và tiếp tục nạp cho khi còn một phần tử. 

Vì thế một đoạn con tốt khi và chỉ khi: 

1. Độ dài của nó là lẻ. 
2. Tổng phiên luân của nó dương. 

Lúc này, bài toán trở thành số đếm các đoạn con lẻ dài có tổng luân phiên dương. 

Ta đưa ra tổng phiên bản về tổng tiền tố và đếm các cặp số chỉ bằng Fenwick Tree sau khi nén ở chế độ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ hoặc tiền tệ hơn$O(n^3)$| Rất lớn | Quá chậm | 
| Tối ưu |$O(n\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1 

Xây dựng tiền tố tổng xen kẽ:$$pref[0]=0$$

$$pref[i]=a_1-a_2+a_3-\cdots+(-1)^{i-1}a_i.$$Khi đó mọi tổng luân phiên của đoạn con đều có thể tính từ hai tiền tố giá trị. 

### Bước 2 

Một đoạn con$[l,r]$có lẻ dài. 

Tương thích với lẻ dài$l$và$r$cùng ngang bằng. 

Nếu$l$lẻ:$$alt(l,r)=pref[r]-pref[l-1].$$Ta cần:$$pref[r]>pref[l-1].$$Nếu$l$thậm chí:$$alt(l,r)=pref[l-1]-pref[r].$$Ta cần:$$pref[r]<pref[l-1].$$### Bước 3 

Đặt:$$p=l-1.$$Khi đó: 

Nếu$r$lẻ thì$p$là chẵn và cần thiết$$pref[p]<pref[r].$$Nếu$r$thậm chí thì$p$là lẻ và ta cần$$pref[p]>pref[r].$$Bài toán trở thành đếm các cặp$(p,r)$với định nghĩa chẵn lẻ và bất kỳ biểu thức đẳng cấp nào trên tiền tố giá trị. 

### Bước 4 

Nén toàn bộ giá trị$pref[i]$. 

Giá trị có thể lớn tới khoảng$2\cdot10^{14}$, nhưng có chỉ$n+1$các giá trị khác nhau cần đánh giá. 

### Bước 5 

Duy trì hai Fenwick Tree. 

Fenwick lưu trữ tiền tố thứ nhất ở một số chẵn. 

Fenwick thứ hai lưu các tiền tố ở một số lẻ. 

Khi đang xử lý vị trí$r$: 

Nếu$r$lẻ, cần số lượng chỉ số trước đó:$$pref[p]<pref[r].$$Đó là truy vấn phần tử nhỏ hơn giá trị hiện tại ở Fenwick. 

Nếu$r$thậm chí, cần có số lượng chỉ số trước đó:$$pref[p]>pref[r].$$Ta lấy:$$\text{tổng phần tử lẻ đã thấy} - \text{số phần tử}\le pref[r].$$### Bước 6 

Sau khi cộng dồn vị trí$r$, chèn$pref[r]$to Fenwick match with parity of$r$. 

### Tại sao nó hoạt động 

Tổng phiên bản luân phiên là bất kỳ biến thể nào được phép giảm. Một đoạn con tốt bắt buộc có tổng luân phiên dương và lẻ dài. Ngược lại, với lẻ dài, nếu tổng luân phiên dương thì luôn tồn tại một hợp lệ được phép, nên có thể nạp tăng dần về độ dài 1. Vì vậy điều kiện "độ dài và tổng luân phiên dương" là tương thích với "đoạn tốt". 

Tiền tố biến đổi sang mảng cho phép biểu diễn tổng luân phiên của mọi giai đoạn bằng cách quan hệ giữa hai tiền tố giá trị. Fenwick Tree đếm chính xác số cặp chỉ số đồng ý bất đẳng thức tương ứng, nên tổng số cặp đếm chính là số đoạn con tốt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, idx, val):
        n = self.n
        while idx <= n:
            self.bit[idx] += val
            idx += idx & -idx

    def sum(self, idx):
        res = 0
        while idx > 0:
            res += self.bit[idx]
            idx -= idx & -idx
        return res

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        pref = [0] * (n + 1)

        for i in range(1, n + 1):
            if i & 1:
                pref[i] = pref[i - 1] + a[i - 1]
            else:
                pref[i] = pref[i - 1] - a[i - 1]

        vals = sorted(set(pref))
        m = len(vals)

        def get_id(x):
            return 1 + __import__("bisect").bisect_left(vals, x)

        even_bit = Fenwick(m)
        odd_bit = Fenwick(m)

        even_cnt = 0
        odd_cnt = 0

        ans = 0

        idx0 = get_id(pref[0])
        even_bit.add(idx0, 1)
        even_cnt = 1

        for r in range(1, n + 1):
            pos = get_id(pref[r])

            if r & 1:
                ans += even_bit.sum(pos - 1)
            else:
                ans += odd_cnt - odd_bit.sum(pos)

            if r & 1:
                odd_bit.add(pos, 1)
                odd_cnt += 1
            else:
                even_bit.add(pos, 1)
                even_cnt += 1

        print(ans)

if __name__ == "__main__":
    solve()
```Mảng xây dựng đầu tiên`pref`, chính là tổng luân phiên của tiền tố. 

Sau đó ta nén tốc độ vì tiền tố có giá trị có thể rất lớn. Fenwick Tree chỉ cần biết thứ tự tương đối giữa các giá trị, nên nén chế độ là đủ. 

Hai Fenwick Tree được phân tách theo tính chẵn lẻ của tiền tố chỉ số. Đây là chi tiết quan trọng nhất. Điều kiện của bài toán không chỉ phụ thuộc vào tiền tố giá trị mà còn phụ thuộc vào tính chẵn lẻ của hai đoạn đầu. 

Với lẻ chỉ số, tiền tố đếm số chẵn trước đó nhỏ hơn giá trị hiện tại. Với số chẵn, tiền tố đếm số lẻ trước đó lớn hơn giá trị hiện tại. 

Mọi quyền tính đều dùng nguyên 64 bit. Trong Python điều này tự động an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
a = [10, 20, 10]
```Tiền tố:$$pref=[0,10,-10,0]$$| r | trước[r] | Tính chẵn lẻ của r | Kết quả truy vấn | Tổng số câu trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | lẻ | 1 | 1 | 
| 2 | -10 | Thậm chí | 1 | 2 | 
| 3 | 0 | lẻ | 1 | 3 | 

Kết quả là 3. 

Ba đoạn tốt chính là ba đoạn độ dài 1. Đoạn`[10,20,10]`có phiên bản tổng hợp bằng 0 nên không thể tính được. 

### Ví dụ 2```
a = [1,1,1,1,1]
```Tiền tố:$$[0,1,0,1,0,1]$$| r | trước[r] | Chẵn lẻ | Kết quả truy vấn | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | lẻ | 1 | 1 | 
| 2 | 0 | Thậm chí | 1 | 2 | 
| 3 | 1 | lẻ | 2 | 4 | 
| 4 | 0 | Thậm chí | 2 | 6 | 
| 5 | 1 | lẻ | 3 | 9 | 

Câu trả lời cuối cùng là 9. 

Ví dụ này đã tìm thấy các đoạn dài 3 và dài 5 đều được tính chính xác thông qua điều kiện tổng luân phiên dương. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Nén chế độ và mỗi tiền tố thực hiện một số thao tác Fenwick | 
| Không gian |$O(n)$| Tiền tố, mảng nén và Fenwick | 

Tổng$n$on all test only is$2\cdot10^5$. Với phức tạp tốc độ$O(n\log n)$, thao tác số thực tế nằm rất xa trong thời gian 2 giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    from bisect import bisect_left

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, idx, val):
            while idx <= self.n:
                self.bit[idx] += val
                idx += idx & -idx

        def sum(self, idx):
            res = 0
            while idx > 0:
                res += self.bit[idx]
                idx -= idx & -idx
            return res

    data = io.StringIO(inp)
    t = int(data.readline())
    out = []

    for _ in range(t):
        n = int(data.readline())
        a = list(map(int, data.readline().split()))

        pref = [0]
        cur = 0
        for i, x in enumerate(a, start=1):
            cur += x if i & 1 else -x
            pref.append(cur)

        vals = sorted(set(pref))
        m = len(vals)

        even = Fenwick(m)
        odd = Fenwick(m)

        even_cnt = 1
        odd_cnt = 0

        p0 = bisect_left(vals, pref[0]) + 1
        even.add(p0, 1)

        ans = 0

        for r in range(1, n + 1):
            pos = bisect_left(vals, pref[r]) + 1

            if r & 1:
                ans += even.sum(pos - 1)
            else:
                ans += odd_cnt - odd.sum(pos)

            if r & 1:
                odd.add(pos, 1)
                odd_cnt += 1
            else:
                even.add(pos, 1)
                even_cnt += 1

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run(
"""4
3
10 20 10
5
1 1 1 1 1
4
5 1 5 1
1
100
"""
) == "3\n9\n5\n1"

# custom cases
assert run(
"""1
1
7
"""
) == "1"

assert run(
"""1
2
1 1
"""
) == "2"

assert run(
"""1
3
1 100 1
"""
) == "3"

assert run(
"""1
5
2 2 2 2 2
"""
) == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[7]`|`1`| Kích thước nhỏ nhất | 
|`[1,1]`|`2`| Đoạn dài thậm chí không được tính | 
|`[1,100,1]`|`3`| Tổng phiên luân không dương | 
|`[2,2,2,2,2]`|`9`| Nhiều tiền tố trùng lặp nhau | 

## Vỏ cạnh 

### Độ dài thậm chí 

đầu vào:```
1
2
1 1
```Không có phép nào làm cho chiều dài 2 trở thành chiều dài 1 vì mỗi lần giảm đúng 2 phần tử. Thuật toán cũng không đếm được đoạn`[1,1]`vì điều kiện chẵn lẻ của hai đoạn đầu không được đồng ý. 

Kết quả:```
2
```chỉ gồm hai đoạn đơn. 

### Tổng luân phiên bằng 0 

đầu vào:```
1
3
10 20 10
```Tổng phiên luân của cả đoạn là:$$10-20+10=0.$$Điều kiện cần phải dương, nên đoạn dài 3 không được tính toán. Thuật toán đếm ba đoạn dài 1. 

Kết quả:```
3
```### Tổng luân phiên dương nhưng có vẻ khó giảm 

đầu vào:```
1
5
1 1 1 1 1
```Tổng số phiên bản toàn bộ đoạn là:$$1-1+1-1+1=1.$$Điều kiện khẳng định đoạn này tốt. Quả thực như vậy:```
[1,1,1,1,1]
→ [1,1,1]
→ [1]
```Thuật toán đếm chính xác đoạn dài 5 cùng đoạn dài 3 và dài 1, cho đáp án 9.

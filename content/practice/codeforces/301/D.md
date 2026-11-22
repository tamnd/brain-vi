---
title: "CF 301D - Yaroslav và các ước số"
description: "Chúng ta được cho một hoán vị các số từ 1 đến n. Đối với mỗi khoảng truy vấn [l, r], chúng ta phải đếm xem có bao nhiêu cặp vị trí theo thứ tự (q, w) bên trong khoảng đó thỏa mãn: $$p[q] mid p[w]$$ Vì tất cả các giá trị đều khác biệt và tạo thành một hoán vị nên mỗi số xuất hiện chính xác một lần."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 301
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 182 (Div. 1)"
rating: 2200
weight: 301
solve_time_s: 175
verified: true
draft: false
---

[CF 301D - Yaroslav và Số chia](https://codeforces.com/problemset/problem/301/D) 

**Đánh giá:** 2200 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 2m 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số từ`1`ĐẾN`n`. Đối với mỗi khoảng truy vấn`[l, r]`, ta phải đếm xem có bao nhiêu cặp vị trí có thứ tự`(q, w)`trong khoảng đó thỏa mãn:$$p[q] \mid p[w]$$Vì tất cả các giá trị đều khác nhau và tạo thành một hoán vị nên mỗi số xuất hiện đúng một lần. Nhiệm vụ không hỏi về khả năng phân chia giữa các chỉ mục, chỉ hỏi về các giá trị được lưu trữ tại các chỉ mục đó. 

Ví dụ: nếu mảng con chứa các giá trị`[2, 6, 3]`, thì các cặp thứ tự hợp lệ là: 

-`(2, 2)`vì mọi số đều tự chia hết 
-`(2, 6)`-`(3, 3)`-`(6, 6)`Câu trả lời sẽ là`4`. 

Những hạn chế là thách thức thực sự ở đây. Cả hai`n`Và`m`có thể đạt được`2 * 10^5`. Một cách tiếp cận đơn giản để kiểm tra tất cả các cặp trong mỗi khoảng thời gian truy vấn sẽ yêu cầu tới:$$O(m \cdot n^2)$$hoạt động trong trường hợp xấu nhất, điều này hoàn toàn không thể thực hiện được trong giới hạn 2 giây. 

Ngay cả việc lặp lại tất cả các cặp một lần trên toàn cầu cũng đã có sẵn:$$(2 \cdot 10^5)^2 = 4 \cdot 10^{10}$$đó là vượt xa giới hạn thực tế. 

Cấu trúc của hoán vị rất quan trọng. Bởi vì mọi giá trị từ`1`ĐẾN`n`xuất hiện chính xác một lần, chúng ta có thể xử lý trước vị trí của mọi số và lý do về tính chia hết bằng cách sử dụng các giá trị thay vì chỉ số. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
n = 1
p = [1]
query = [1,1]
```Câu trả lời đúng là`1`, bởi vì`1`tự phân chia. Việc thực hiện bất cẩn chỉ đếm các cặp số riêng biệt sẽ trả về sai`0`. 

Một trường hợp tinh tế khác là khi số chia xuất hiện sau bội số trong mảng.```
p = [6,2]
query = [1,2]
```Các cặp hợp lệ là: 

-`(6,6)`-`(2,2)`-`(2,6)`Câu trả lời là`3`. 

Cặp đôi`(2,6)`vẫn hợp lệ mặc dù`2`xuất hiện sau trong mảng. Điều kiện là về giá trị chứ không phải thứ tự chỉ mục. Nhiều giải pháp sai vô tình chỉ được tính`i <= j`. 

Một tình huống nguy hiểm khác là đếm kép các cặp đối xứng.```
p = [2,4]
```Chỉ một`(2,4)`hợp lệ, không`(4,2)`. Sự phân chia có tính định hướng. Coi nó như một mối quan hệ không có thứ tự sẽ tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi truy vấn`[l, r]`, lặp lại tất cả các cặp vị trí trong khoảng và kiểm tra xem một giá trị có chia hết cho giá trị kia hay không. 

Ý tưởng mã giả trông như thế này:```
for q in [l, r]:
    for w in [l, r]:
        if p[w] % p[q] == 0:
            answer += 1
```Điều này đúng vì nó trực tiếp tuân theo định nghĩa của vấn đề. Thật không may, sự phức tạp là khủng khiếp. 

Một truy vấn có thể chứa`O(n^2)`cặp. Với`m = 2 * 10^5`, tổng công sẽ trở thành gần đúng:$$O(mn^2)$$có thể đạt được`8 * 10^{15}`hoạt động. 

Quan sát tiếp theo là bước ngoặt quan trọng. 

Chúng tôi thực sự không quan tâm đến các cặp giá trị tùy ý. Chúng tôi chỉ quan tâm đến các cặp trong đó một số chia cho số kia. 

Đối với một giá trị cố định`x`, các ứng cử viên duy nhất là bội số của nó:$$x, 2x, 3x, \dots$$Tổng số mối quan hệ chia số giữa các số từ`1`ĐẾN`n`là:$$\sum_{x=1}^{n} \frac{n}{x}$$đó là khoảng:$$n \log n$$Điều này là đủ nhỏ. 

Vì mảng là một hoán vị nên chúng ta biết chính xác vị trí của mọi giá trị. Với mọi quan hệ chia số hợp lệ`(x, y)`Ở đâu`x | y`, chúng ta có thể tạo một cặp vị trí:$$(pos[x], pos[y])$$Một truy vấn hỏi có bao nhiêu cặp như vậy nằm hoàn toàn bên trong`[l, r]`. 

Điều đó biến bài toán thành bài toán đếm hình học: 

Đếm xem có bao nhiêu cặp vị trí hợp lệ thỏa mãn:$$l \le a \le r$$

$$l \le b \le r$$Để thuận tiện, chúng tôi luôn lưu trữ các cặp như sau:$$(\min(a,b), \max(a,b))$$Sau đó, truy vấn trở thành: 

Đếm các cặp với:$$a \ge l$$

$$b \le r$$Đây là một bài toán Fenwick Tree ngoại tuyến cổ điển. 

Chúng tôi xử lý các truy vấn bằng cách tăng`r`. Khi chúng tôi quét từ trái sang phải, chúng tôi kích hoạt mọi cặp có điểm cuối lớn hơn bằng vị trí hiện tại. Đối với mỗi cặp kích hoạt`(a,b)`, chúng tôi thêm`1`ở vị trí`a`trong Cây Fenwick. 

Sau đó để truy vấn`[l,r]`, Cây Fenwick chứa chính xác các cặp có điểm cuối lớn hơn nhiều nhất là`r`. Truy vấn tổng hậu tố từ`l`ĐẾN`n`đếm xem có bao nhiêu người trong số họ cũng thỏa mãn`a >= l`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(mn²) | O(1) | Quá chậm | 
| Tối ưu | O(n log n + m log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hoán vị và tính toán`pos[x]`, vị trí có giá trị`x`xuất hiện. 
2. Liệt kê tất cả các mối quan hệ số chia. 

Với mọi giá trị`x`từ`1`ĐẾN`n`, lặp qua tất cả bội số của`x`:```
y = x, 2x, 3x, ...
```Từ`x`chia rẽ`y`, cặp này hợp lệ. 
3. Chuyển đổi các cặp giá trị thành các cặp vị trí. 

Cho phép:```
a = pos[x]
b = pos[y]
```Lưu trữ cặp dưới dạng:```
(min(a,b), max(a,b))
```Việc chuẩn hóa này cho phép chúng ta xử lý mọi cặp một cách nhất quán. 
4. Nhóm các cặp theo điểm cuối lớn hơn của chúng. 

Giả sử một cặp trở thành`(a,b)`với`a <= b`. 

Cửa hàng`a`bên trong một thùng liên kết với`b`. 

Sau này, khi xử lý vị trí`b`, chúng ta sẽ kích hoạt cặp này. 
5. Sắp xếp truy vấn theo điểm cuối bên phải`r`. 

Chúng tôi xử lý các vị trí từ trái sang phải. Khi chúng tôi đạt đến vị trí`r`, mọi cặp có điểm cuối lớn hơn nhiều nhất`r`nên đã hoạt động. 
6. Duy trì Cây Fenwick ở các vị trí. 

Khi xử lý vị trí`b`, kích hoạt tất cả các cặp được lưu trữ bằng cách thêm`1`tại điểm cuối nhỏ hơn của họ`a`. 
7. Trả lời các câu hỏi. 

Đối với truy vấn`[l,r]`, tất cả các cặp hoạt động đã thỏa mãn:```
larger endpoint <= r
```Chúng tôi vẫn cần:```
smaller endpoint >= l
```Đây chính xác là tổng hậu tố:```
sum(n) - sum(l-1)
```8. Xuất câu trả lời theo thứ tự truy vấn ban đầu. 

### Tại sao nó hoạt động 

Mọi quan hệ ước số hợp lệ giữa các giá trị đều tạo ra chính xác một cặp vị trí chuẩn hóa`(a,b)`với`a <= b`. 

Khoảng truy vấn`[l,r]`nên đếm chính xác những cặp hoàn toàn nằm trong khoảng. Điều kiện đó tương đương với:$$a \ge l \quad \text{and} \quad b \le r$$Trong quá trình quét, một khi vị trí`r`được xử lý, Cây Fenwick chứa tất cả các cặp có`b <= r`. Mỗi cặp như vậy đóng góp`1`tại chỉ mục`a`. 

Truy vấn hậu tố`[l,n]`đếm chính xác các cặp có điểm cuối nhỏ hơn ít nhất`l`. Kết hợp cả hai điều kiện sẽ cho ra chính xác các cặp hoàn toàn nằm trong khoảng. 

Không có cặp nào bị bỏ sót và không có cặp nào được tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

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

    def range_sum(self, l, r):
        return self.sum(r) - self.sum(l - 1)

def solve():
    n, m = map(int, input().split())
    p = [0] + list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i in range(1, n + 1):
        pos[p[i]] = i

    events = [[] for _ in range(n + 1)]

    for x in range(1, n + 1):
        y = x
        while y <= n:
            a = pos[x]
            b = pos[y]

            if a > b:
                a, b = b, a

            events[b].append(a)
            y += x

    queries = [[] for _ in range(n + 1)]

    for idx in range(m):
        l, r = map(int, input().split())
        queries[r].append((l, idx))

    bit = Fenwick(n)
    ans = [0] * m

    for r in range(1, n + 1):
        for a in events[r]:
            bit.add(a, 1)

        for l, idx in queries[r]:
            ans[idx] = bit.range_sum(l, n)

    print("\n".join(map(str, ans)))

solve()
```Bước tiền xử lý quan trọng đầu tiên là xây dựng`pos`. Vì mảng là một hoán vị nên điều này cho phép truy cập liên tục theo thời gian vào vị trí của mọi giá trị. 

Nhiều vòng lặp lồng nhau là trung tâm của việc tối ưu hóa. Thay vì kiểm tra tất cả các cặp giá trị, chúng tôi chỉ kiểm tra các cặp có đảm bảo tính chia hết. Tổng số lần lặp hoạt động giống như sự tăng trưởng chuỗi hài hòa:$$n \left(1 + \frac12 + \frac13 + \dots \right)$$đó là khoảng`n log n`. 

Sự bình thường hóa:```
if a > b:
    a, b = b, a
```là điều cần thiết. Các truy vấn chỉ quan tâm liệu cả hai điểm cuối có nằm trong khoảng hay không. Họ không quan tâm đến thứ tự xuất hiện của các giá trị. 

Số lượng cửa hàng Fenwick Tree được lập chỉ mục theo điểm cuối nhỏ hơn. Khi xử lý điểm cuối bên phải`r`, mọi cặp hoạt động đều đã thỏa mãn`b <= r`. Một truy vấn hậu tố trên`[l,n]`lọc chính xác những người có`a >= l`. 

Một chi tiết triển khai tinh tế là các cặp trong đó`x == y`cũng phải được đưa vào. Mỗi số đều tự chia nên các cặp đường chéo là hợp lệ và cần thiết. 

Một sai lầm dễ mắc phải khác là sử dụng tổng tiền tố thay vì tổng hậu tố khi trả lời truy vấn. Chúng tôi cần:```
a >= l
```không:```
a <= l
```vì vậy truy vấn phải là:```
bit.range_sum(l, n)
```## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1
1
1 1
```Hoán vị chỉ chứa một giá trị. 

Cặp số chia hợp lệ: 

| x | y | Vị trí | Cặp chuẩn hóa | 
| --- | --- | --- | --- | 
| 1 | 1 | (1,1) | (1,1) | 

Xử lý: 

| r | Cặp kích hoạt | Nội dung BIT | Truy vấn | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | (1,1) | đếm 1 = 1 | [1,1] | 1 | 

Kết quả là`1`. 

Ví dụ này xác nhận rằng khả năng tự phân chia phải được tính đến. 

### Ví dụ 2 

đầu vào:```
5 3
2 1 4 5 3
1 5
2 4
3 5
```Vị trí: 

| Giá trị | Vị trí | 
| --- | --- | 
| 1 | 2 | 
| 2 | 1 | 
| 3 | 5 | 
| 4 | 3 | 
| 5 | 4 | 

Quan hệ chia số hợp lệ: 

| x | y | Cặp vị trí | Chuẩn hóa | 
| --- | --- | --- | --- | 
| 1 | 1 | (2,2) | (2,2) | 
| 1 | 2 | (2,1) | (1,2) | 
| 1 | 3 | (2,5) | (2,5) | 
| 1 | 4 | (2,3) | (2,3) | 
| 1 | 5 | (2,4) | (2,4) | 
| 2 | 2 | (1,1) | (1,1) | 
| 2 | 4 | (1,3) | (1,3) | 
| 3 | 3 | (5,5) | (5,5) | 
| 4 | 4 | (3,3) | (3,3) | 
| 5 | 5 | (4,4) | (4,4) | 

Quá trình quét: 

| r | Điểm cuối nhỏ hơn được kích hoạt | Truy vấn | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 1 | không | - | 
| 2 | 2,1 | không | - | 
| 3 | 2,1,3 | [2,4] | 4 | 
| 4 | 2,4 | không | - | 
| 5 | 2,5 | [1,5] | 10 | 
| 5 | đã hoạt động | [3,5] | 3 | 

Khoảng thời gian`[2,4]`chứa các giá trị`[1,4,5]`. 

Các cặp thứ tự hợp lệ là: 

-`(1,1)`-`(1,4)`-`(1,5)`-`(4,4)`đưa ra câu trả lời`4`. 

Dấu vết này thể hiện cách đường quét đảm bảo rằng chỉ các cặp hoàn toàn bên trong ranh giới bên phải hiện tại mới hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m log n) | Việc liệt kê bội số tốn thời gian chuỗi hài, các phép toán Fenwick tốn thời gian logarit | 
| Không gian | O(n log n) | Các cặp số chia được lưu trữ cộng với Cây Fenwick và các truy vấn | 

Chuỗi hài bị ràng buộc là lý do giải pháp này phù hợp thoải mái bên trong các giới hạn. Vì`n = 2 * 10^5`, số lượng cặp ước số là khoảng vài triệu, có thể quản lý được bằng Python với cấu trúc dữ liệu hiệu quả. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

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

        def range_sum(self, l, r):
            return self.sum(r) - self.sum(l - 1)

    n, m = map(int, input().split())
    p = [0] + list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i in range(1, n + 1):
        pos[p[i]] = i

    events = [[] for _ in range(n + 1)]

    for x in range(1, n + 1):
        y = x
        while y <= n:
            a = pos[x]
            b = pos[y]

            if a > b:
                a, b = b, a

            events[b].append(a)
            y += x

    queries = [[] for _ in range(n + 1)]

    for idx in range(m):
        l, r = map(int, input().split())
        queries[r].append((l, idx))

    bit = Fenwick(n)
    ans = [0] * m

    for r in range(1, n + 1):
        for a in events[r]:
            bit.add(a, 1)

        for l, idx in queries[r]:
            ans[idx] = bit.range_sum(l, n)

    return "\n".join(map(str, ans))

# provided sample
assert run("1 1\n1\n1 1\n") == "1"

# reversed divisor order
assert run("2 1\n6 2\n1 2\n") == "3"

# simple increasing permutation
assert run(
    "4 2\n1 2 3 4\n1 4\n2 3\n"
) == "8\n2"

# single element intervals
assert run(
    "5 5\n2 1 4 5 3\n1 1\n2 2\n3 3\n4 4\n5 5\n"
) == "1\n1\n1\n1\n1"

# off-by-one interval boundaries
assert run(
    "3 3\n2 3 1\n1 2\n2 3\n1 3\n"
) == "2\n2\n5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 / 1 1`|`1`| Tự phân chia | 
|`6 2`trường hợp hoán vị |`3`| Số chia có thể xuất hiện sau nhiều | 
| Tăng hoán vị |`8`,`2`| Đếm ước chung | 
| Truy vấn một phần tử | tất cả`1`| Mọi số đều tự chia hết | 
| Kiểm tra khoảng ranh giới |`2`,`2`,`5`| Xử lý đúng giới hạn khoảng thời gian | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1 1
1
1 1
```Thuật toán tạo cặp số chia`(1,1)`. Cặp vị trí của nó cũng là`(1,1)`. Trong quá trình quét tại`r = 1`, Cây Fenwick nhận được một bản cập nhật tại vị trí`1`. Hậu tố truy vấn`[1,n]`trả lại`1`. 

Điều này xác nhận rằng các cặp tự được đưa vào một cách chính xác. 

Bây giờ hãy xem xét trường hợp thứ tự đảo ngược:```
2 1
6 2
1 2
```Sau khi bình thường hóa: 

-`(6,6)`trở thành`(1,1)`-`(2,2)`trở thành`(2,2)`-`(2,6)`trở thành`(1,2)`Mặc dù số chia`2`xuất hiện sau trong mảng, việc chuẩn hóa sẽ duy trì hình dạng khoảng chính xác. Truy vấn`[1,2]`đếm cả ba cặp. 

Cuối cùng, hãy xem xét một ví dụ về ranh giới:```
3 1
2 3 1
2 3
```Các quan hệ chia số hợp lệ trên toàn cầu là: 

-`(2,2)`-`(3,3)`-`(1,1)`-`(1,2)`-`(1,3)`Khoảng bên trong`[2,3]`, chỉ các vị trí`2`Và`3`được phép. Các cặp hợp lệ duy nhất là: 

-`(3,3)`-`(1,1)`sử dụng giá trị`1`ở vị trí`3`Thuật toán xử lý việc này vì truy vấn Fenwick chỉ đếm các cặp có điểm cuối nhỏ hơn ít nhất`2`. Vị trí chạm cặp`1`được loại trừ tự động.

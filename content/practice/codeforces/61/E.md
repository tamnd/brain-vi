---
title: "CF 61E - Địch yếu"
description: "Chúng ta được cho một dãy các số nguyên riêng biệt biểu thị sức mạnh của những người lính đứng trong hàng. Ta cần đếm xem có bao nhiêu bộ ba chỉ số (i, j, k) cùng lúc thỏa mãn hai điều kiện: - Các vị trí được sắp xếp theo thứ tự i < j < k - Các giá trị giảm nghiêm ngặt là a[i] a[j]…"
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "trees"]
categories: ["algorithms"]
codeforces_contest: 61
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 57 (Div. 2)"
rating: 1900
weight: 61
solve_time_s: 114
verified: true
draft: false
---

[CF 61E - Kẻ thù yếu](https://codeforces.com/problemset/problem/61/E) 

**Xếp hạng:** 1900 
**Thẻ:** cấu trúc dữ liệu, cây 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các số nguyên riêng biệt biểu thị sức mạnh của những người lính đứng trong hàng. Chúng ta cần đếm xem có bao nhiêu chỉ số tăng gấp ba`(i, j, k)`thỏa mãn đồng thời hai điều kiện: 

- Các vị trí được sắp xếp như`i < j < k`- Các giá trị giảm dần theo`a[i] > a[j] > a[k]`Một cách khác để nghĩ về nó là thế này: với mọi vị trí ở giữa`j`, chúng ta muốn biết có bao nhiêu phần tử lớn hơn tồn tại trước nó và bao nhiêu phần tử nhỏ hơn tồn tại sau nó. Mọi sự kết hợp của những lựa chọn đó tạo thành một bộ ba giảm hợp lệ. 

Các ràng buộc xác định hoàn toàn loại thuật toán chúng ta cần. Kích thước mảng có thể đạt tới`10^6`, loại trừ mọi thứ bậc hai, chứ đừng nói đến bậc ba. Thậm chí một`O(n^2)`giải pháp sẽ yêu cầu khoảng`10^12`hoạt động trong trường hợp xấu nhất, vượt xa những gì phù hợp trong 5 giây. Độ phức tạp của mục tiêu phải gần tuyến tính hoặc`O(n log n)`. 

Bản thân các giá trị có thể lớn bằng`10^9`, vì vậy chúng tôi không thể trực tiếp xây dựng các mảng được lập chỉ mục theo giá trị. Bất kỳ cấu trúc dữ liệu nào phụ thuộc vào phạm vi giá trị đều cần nén tọa độ trước tiên. 

Một vấn đề tế nhị là tràn số nguyên. Số lượng bộ ba giảm có thể cực kỳ lớn. Ví dụ: nếu mảng giảm nghiêm ngặt và`n = 10^6`, câu trả lời là:$$\binom{10^6}{3}$$Đó là xung quanh`1.67 × 10^17`, không phù hợp với số nguyên 32 bit. Python xử lý việc này một cách tự động, nhưng trong C++ câu trả lời phải được lưu trữ trong`long long`. 

Một sai lầm dễ mắc phải khác là xử lý sai điều kiện “hoàn toàn lớn hơn”. Giả sử mảng là:```
4
5 5 4 3
```Vấn đề đảm bảo các giá trị riêng biệt, do đó đầu vào này không hợp lệ, nhưng nhiều cách triển khai chung vô tình sử dụng`>=`hoặc`<=`logic. Nếu có sự trùng lặp, việc xử lý bất đẳng thức không chính xác sẽ âm thầm đếm quá mức. Điều kiện đúng là giảm nghiêm ngặt ở mỗi bước. 

Trường hợp cạnh thực tế hơn là một mảng tăng nghiêm ngặt:```
5
1 2 3 4 5
```Câu trả lời đúng là`0`vì không tồn tại bộ ba giảm. Việc triển khai bất cẩn làm trộn lẫn số lượng bên trái và bên phải có thể vô tình đếm các mẫu tăng dần. 

Một tình huống quan trọng khác là khi tất cả các bộ ba hợp lệ đều có chung phần tử ở giữa. Ví dụ:```
5
5 4 3 2 1
```Mọi bộ ba đều hợp lệ ở đây, cho:$$\binom{5}{3} = 10$$Trường hợp này nhấn mạnh liệu thuật toán có kết hợp chính xác “lớn hơn ở bên trái” với “nhỏ hơn ở bên phải” hay không. 

## Phương pháp tiếp cận 

Giải pháp vũ phu trực tiếp tuân theo định nghĩa. Chúng tôi cố gắng từng ba lần`(i, j, k)`với`i < j < k`và kiểm tra xem các giá trị có tạo thành một dãy giảm dần hay không.```
for i:
    for j > i:
        for k > j:
            check a[i] > a[j] > a[k]
```Điều này đúng vì mỗi bộ ba có thể được kiểm tra chính xác một lần. Vấn đề là thời gian chạy. Số bộ ba là:$$\binom{n}{3}$$Vì`n = 10^6`, đại khái là vậy`1.67 × 10^17`lặp đi lặp lại, điều này là hoàn toàn không thể. 

Quan sát quan trọng là chỉ số trung bình`j`quyết định mọi thứ. Nếu chúng ta đã biết: 

- có bao nhiêu phần tử trước đó`j`lớn hơn`a[j]`- có bao nhiêu phần tử sau`j`nhỏ hơn`a[j]`sau đó số lượng bộ ba hợp lệ bằng cách sử dụng`j`vì phần tử ở giữa chỉ đơn giản là:$$\text{leftGreater}[j] \times \text{rightSmaller}[j]$$Điều này biến vấn đề từ tìm kiếm theo bộ ba thành trả lời các truy vấn tần suất. 

Bây giờ nhiệm vụ trở thành: 

- Khi quét từ trái sang phải, đếm xem có bao nhiêu giá trị trước đó lớn hơn giá trị hiện tại. 
- Khi quét từ phải sang trái, đếm xem có bao nhiêu giá trị sau nhỏ hơn giá trị hiện tại. 

Đó là những truy vấn tần số tiền tố cổ điển, có thể được xử lý hiệu quả bằng Cây Fenwick. 

Bởi vì giá trị mảng lên đến`10^9`, trước tiên chúng tôi nén chúng thành các thứ hạng từ`1`ĐẾN`n`. Vì tất cả các giá trị đều khác nhau nên việc nén sẽ duy trì thứ tự chính xác. 

Mỗi hoạt động của Fenwick Tree mất`O(log n)`và chúng tôi thực hiện một số thao tác không đổi cho mỗi phần tử. Điều đó mang lại sự phức tạp hoàn toàn`O(n log n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n^3)`|`O(1)`| Quá chậm | 
| Tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và nén các giá trị của nó. 

Chúng tôi sắp xếp các giá trị riêng biệt và gán thứ hạng từ`1`ĐẾN`n`. Cây Fenwick hoạt động trên các chỉ số số nguyên nhỏ gọn, do đó, việc nén cho phép chúng ta duy trì các so sánh trong khi tránh các mảng lớn. 
2. Tạo một mảng`left_greater`.`left_greater[i]`sẽ lưu trữ bao nhiêu phần tử trước đó lớn hơn`a[i]`. 
3. Quét mảng từ trái sang phải bằng Fenwick Tree. 

Tại mỗi vị trí: 

- Truy vấn có bao nhiêu phần tử được xử lý nhỏ hơn hoặc bằng giá trị hiện tại. 
- Trừ đi số phần tử đã xử lý để có số phần tử lớn hơn. 
- Lưu trữ số lượng trong`left_greater[i]`. 
- Chèn giá trị hiện tại vào Fenwick Tree. 

Lý do phép trừ có tác dụng là vì mọi phần tử trước đều lớn hơn hoặc không lớn hơn và tất cả các giá trị đều khác biệt. 
4. Đặt lại cây Fenwick. 

Bây giờ chúng tôi sử dụng lại nó cho thông tin hậu tố. 
5. Create an array`right_smaller`.`right_smaller[i]`sẽ lưu trữ bao nhiêu phần tử sau này nhỏ hơn`a[i]`. 
6. Quét mảng từ phải sang trái. 

Tại mỗi vị trí: 

- Truy vấn có bao nhiêu giá trị đã thấy nhỏ hơn giá trị hiện tại. 
- Lưu kết quả đó vào`right_smaller[i]`. 
- Chèn giá trị hiện tại vào Fenwick Tree. 

Vì chúng ta di chuyển từ phải sang trái nên cây luôn chứa chính xác các phần tử sau chỉ mục hiện tại. 
7. Tính đáp án cuối cùng. 

Đối với mọi vị trí`i`, thêm vào:$$\text{left\_greater}[i] \times \text{right\_smaller}[i]$$Mỗi bộ ba hợp lệ có chính xác một chỉ số ở giữa, do đó, điều này sẽ tính mỗi bộ ba giảm đi một lần. 

### Tại sao nó hoạt động 

Đối với mọi chỉ số ở giữa cố định`j`, một bộ ba hợp lệ hoàn toàn được xác định bằng cách chọn: 

- một chỉ số`i < j`với`a[i] > a[j]`- một chỉ số`k > j`với`a[k] < a[j]`Các lựa chọn là độc lập nên số bộ ba tập trung tại`j`là tích của hai số đó. 

Đường chuyền Fenwick từ trái sang phải theo dõi chính xác tất cả các yếu tố trước đó`j`và đường chuyền từ phải sang trái sẽ theo dõi chính xác tất cả các phần tử sau`j`. Vì mỗi bộ ba giảm đều có một vị trí ở giữa duy nhất, tổng của tất cả`j`đếm mọi bộ ba hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def update(self, idx, delta):
        while idx <= self.n:
            self.bit[idx] += delta
            idx += idx & -idx

    def query(self, idx):
        res = 0
        while idx > 0:
            res += self.bit[idx]
            idx -= idx & -idx
        return res

def solve():
    n = int(input())
    arr = list(map(int, input().split()))

    # coordinate compression
    vals = sorted(arr)
    rank = {v: i + 1 for i, v in enumerate(vals)}
    comp = [rank[x] for x in arr]

    left_greater = [0] * n
    right_smaller = [0] * n

    bit = Fenwick(n)

    # left greater counts
    for i in range(n):
        x = comp[i]

        less_or_equal = bit.query(x)
        left_greater[i] = i - less_or_equal

        bit.update(x, 1)

    bit = Fenwick(n)

    # right smaller counts
    for i in range(n - 1, -1, -1):
        x = comp[i]

        right_smaller[i] = bit.query(x - 1)

        bit.update(x, 1)

    ans = 0

    for i in range(n):
        ans += left_greater[i] * right_smaller[i]

    print(ans)

solve()
```Cây Fenwick lưu trữ số lượng tần số của các giá trị nén đã được xử lý. các`update`thao tác chèn một giá trị và`query(idx)`trả về số lượng giá trị được chèn có thứ hạng nén nhiều nhất`idx`. 

Việc nén tọa độ là cần thiết vì các giá trị có thể lớn bằng`10^9`. Cây Fenwick yêu cầu các chỉ số nguyên dày đặc, vì vậy chúng tôi ánh xạ lại các giá trị vào phạm vi`1...n`trong khi vẫn bảo quản trật tự. 

Trong quá trình chuyển từ trái sang phải, cây chứa chính xác các phần tử trước chỉ mục hiện tại.`bit.query(x)`cho chúng ta biết có bao nhiêu giá trị trước đó nhỏ hơn hoặc bằng giá trị hiện tại. Vì tất cả các giá trị đều khác biệt nên điều này tương đương với việc đếm các giá trị nhỏ hơn. Trừ từ`i`, số phần tử trước đó, cho ra số lượng giá trị lớn hơn. 

Đường chuyền từ phải sang trái hơi khác một chút. Ở đây chúng tôi truy vấn trực tiếp`x - 1`, tính các phần tử nhỏ hơn trong số hậu tố đã được chèn vào cây. 

Một sai lầm dễ mắc phải là quên rằng Cây Fenwick thường có chỉ mục 1. Quá trình nén bắt đầu từ`1`, không`0`, đặc biệt là để tránh các vòng lặp vô hạn trong`update`. 

Một chi tiết tinh tế khác là sử dụng hai lượt riêng biệt thay vì cố gắng tính toán cả hai hướng cùng một lúc. Ý nghĩa của cây phụ thuộc hoàn toàn vào hướng đi ngang. Việc sử dụng lại cùng một cây mà không đặt lại nó sẽ trộn lẫn thông tin tiền tố và hậu tố không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 2 1
```Giá trị nén vẫn còn`[3, 2, 1]`. 

Đường chuyền từ trái sang phải: 

| tôi | giá trị | less_or_equal | left_Greater | 
| --- | --- | --- | --- | 
| 0 | 3 | 0 | 0 | 
| 1 | 2 | 0 | 1 | 
| 2 | 1 | 0 | 2 | 

Đường chuyền từ phải sang trái: 

| tôi | giá trị | nhỏ_right | đúng_smaller | 
| --- | --- | --- | --- | 
| 2 | 1 | 0 | 0 | 
| 1 | 2 | 1 | 1 | 
| 0 | 3 | 2 | 2 | 

Tính toán cuối cùng: 

| tôi | left_Greater | đúng_smaller | đóng góp | 
| --- | --- | --- | --- | 
| 0 | 0 | 2 | 0 | 
| 1 | 1 | 1 | 1 | 
| 2 | 2 | 0 | 0 | 

Tổng số câu trả lời =`1`. 

Dấu vết này cho thấy ý tưởng trung tâm một cách rõ ràng. Chỉ mục`1`có thể đóng vai trò là phần tử ở giữa của bộ ba giảm. 

### Ví dụ 2 

đầu vào:```
5
5 1 4 2 3
```Các giá trị nén là`[5, 1, 4, 2, 3]`. 

Đường chuyền từ trái sang phải: 

| tôi | giá trị | less_or_equal | left_Greater | 
| --- | --- | --- | --- | 
| 0 | 5 | 0 | 0 | 
| 1 | 1 | 0 | 1 | 
| 2 | 4 | 1 | 1 | 
| 3 | 2 | 1 | 2 | 
| 4 | 3 | 2 | 2 | 

Đường chuyền từ phải sang trái: 

| tôi | giá trị | nhỏ_right | đúng_smaller | 
| --- | --- | --- | --- | 
| 4 | 3 | 0 | 0 | 
| 3 | 2 | 0 | 0 | 
| 2 | 4 | 2 | 2 | 
| 1 | 1 | 0 | 0 | 
| 0 | 5 | 4 | 4 | 

Tính toán cuối cùng: 

| tôi | left_Greater | đúng_smaller | đóng góp | 
| --- | --- | --- | --- | 
| 0 | 0 | 4 | 0 | 
| 1 | 1 | 0 | 0 | 
| 2 | 1 | 2 | 2 | 
| 3 | 2 | 0 | 0 | 
| 4 | 2 | 0 | 0 | 

Tổng số câu trả lời =`2`. 

Bộ ba hợp lệ là`(5,4,2)`Và`(5,4,3)`. Ví dụ này chứng tỏ tại sao quan điểm chỉ số ở giữa lại có tác dụng mạnh mẽ. chỉ mục`2`đóng góp chính xác hai bộ ba vì nó có một phần tử lớn hơn ở trước và hai phần tử nhỏ hơn ở sau nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Mỗi lần cập nhật và truy vấn Fenwick mất`O(log n)`và chúng tôi thực hiện một số không đổi cho mỗi phần tử | 
| Không gian |`O(n)`| Mảng nén, Cây Fenwick và mảng trợ giúp đều có tỷ lệ tuyến tính | 

Với`n = 10^6`,`O(n log n)`đủ nhanh trong Python khi được triển khai cẩn thận với Fenwick Trees và I/O nhanh. Việc sử dụng bộ nhớ cũng vừa vặn thoải mái trong giới hạn 256 MB. 

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

        def update(self, idx, delta):
            while idx <= self.n:
                self.bit[idx] += delta
                idx += idx & -idx

        def query(self, idx):
            res = 0
            while idx > 0:
                res += self.bit[idx]
                idx -= idx & -idx
            return res

    n = int(input())
    arr = list(map(int, input().split()))

    vals = sorted(arr)
    rank = {v: i + 1 for i, v in enumerate(vals)}
    comp = [rank[x] for x in arr]

    left_greater = [0] * n
    right_smaller = [0] * n

    bit = Fenwick(n)

    for i in range(n):
        x = comp[i]
        left_greater[i] = i - bit.query(x)
        bit.update(x, 1)

    bit = Fenwick(n)

    for i in range(n - 1, -1, -1):
        x = comp[i]
        right_smaller[i] = bit.query(x - 1)
        bit.update(x, 1)

    ans = 0

    for i in range(n):
        ans += left_greater[i] * right_smaller[i]

    return str(ans)

# provided sample
assert run("3\n3 2 1\n") == "1", "sample 1"

# minimum size, no valid triplet
assert run("3\n1 2 3\n") == "0", "strictly increasing"

# minimum size, exactly one triplet
assert run("3\n3 2 1\n") == "1", "strictly decreasing"

# larger decreasing case
assert run("5\n5 4 3 2 1\n") == "10", "all triples valid"

# mixed ordering
assert run("5\n5 1 4 2 3\n") == "2", "mixed arrangement"

# off-by-one style check
assert run("4\n4 1 3 2\n") == "1", "single valid triplet"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 1 2 3`|`0`| Không tồn tại bộ ba giảm | 
|`3 / 3 2 1`|`1`| Trường hợp hợp lệ không tầm thường nhỏ nhất | 
|`5 / 5 4 3 2 1`|`10`| Mỗi bộ ba đều hợp lệ | 
|`5 / 5 1 4 2 3`|`2`| Đặt hàng hỗn hợp với sự đóng góp có chọn lọc | 
|`4 / 4 1 3 2`|`1`| Phát hiện từng lỗi một trong truy vấn Fenwick | 

## Vỏ cạnh 

Hãy xem xét mảng tăng nghiêm ngặt:```
5
1 2 3 4 5
```Trong quá trình chuyển từ trái sang phải, mọi giá trị trước đó đều nhỏ hơn, do đó`left_greater`trở thành:```
[0, 0, 0, 0, 0]
```Chuyển từ phải sang trái vẫn tìm thấy các giá trị hậu tố nhỏ hơn, nhưng mọi đóng góp cuối cùng đều chứa hệ số 0:```
left_greater[i] * right_smaller[i] = 0
```Câu trả lời cuối cùng là chính xác`0`. 

Bây giờ hãy xem xét mảng giảm dần:```
5
5 4 3 2 1
```Thuật toán tính toán:```
left_greater  = [0, 1, 2, 3, 4]
right_smaller = [4, 3, 2, 1, 0]
```Những đóng góp trở thành:```
0*4 + 1*3 + 2*2 + 3*1 + 4*0
= 0 + 3 + 4 + 3 + 0
= 10
```Điều đó phù hợp với giá trị mong đợi:$$\binom{5}{3} = 10$$Điều này xác nhận rằng logic nhân đếm chính xác mọi cặp phần tử lớn hơn bên trái và nhỏ hơn bên phải có thể có. 

Cuối cùng, hãy xem xét trường hợp chỉ có một chỉ số ở giữa đóng góp:```
5
5 1 4 2 3
```Tại chỉ mục`2`có giá trị`4`: 

- Một phần tử lớn hơn tồn tại trước nó:`5`- Hai phần tử nhỏ hơn tồn tại sau nó:`2`Và`3`Thuật toán nhân các số đếm này:```
1 * 2 = 2
```Không có chỉ số nào khác đóng góp. Điều này chứng tỏ tại sao việc tách vấn đề bằng phần tử ở giữa sẽ tránh được cả việc đếm thiếu và đếm kép.

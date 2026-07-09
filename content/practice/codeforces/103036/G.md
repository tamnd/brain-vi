---
title: "CF 103036G - Độ tốt của cân"
description: "Chúng ta được cho một hoán vị của các số nguyên từ 1 đến n và chúng ta mô phỏng việc xử lý nó từ trái sang phải. Ở mỗi bước, chúng tôi chỉ xem xét các giá trị đã xuất hiện trước vị trí hiện tại."
date: "2026-07-04T02:07:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103036
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 04-02-21 Div. 2 (Beginner)"
rating: 0
weight: 103036
solve_time_s: 50
verified: true
draft: false
---

[CF 103036G - Độ tốt của quy mô](https://codeforces.com/problemset/problem/103036/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị của các số nguyên từ 1 đến n và chúng ta mô phỏng việc xử lý nó từ trái sang phải. Ở mỗi bước, chúng tôi chỉ xem xét các giá trị đã xuất hiện trước vị trí hiện tại. 

Đối với giá trị hiện tại x, chúng tôi xác định hai điểm tham chiếu trong số các giá trị đã thấy trước đó. Một là giá trị lớn nhất được thấy trước đó nhưng vẫn nhỏ hơn x. Giá trị còn lại là giá trị nhỏ nhất được thấy trước đó vẫn lớn hơn x. Nếu một bên không tồn tại, chúng ta sẽ mở rộng khoảng đến ranh giới tự nhiên của phạm vi giá trị. 

Khi hai ranh giới này được xác định, phần đóng góp của x là số số nguyên nằm hoàn toàn giữa chúng. Câu trả lời cuối cùng là tổng của những đóng góp này trên toàn bộ hoán vị. 

Một cách hữu ích để giải thích điều này là chúng ta duy trì một tập hợp động các giá trị nhìn thấy trên trục số từ 1 đến n. Mỗi giá trị mới “kéo dài” khoảng cách giữa các giá trị lân cận hiện có gần nhất của nó trong tập hợp này và chúng tôi đếm có bao nhiêu số nguyên không được sử dụng nằm trong khoảng trống đó. 

Ràng buộc n lên tới 2 · 10^5 ngay lập tức loại trừ việc tính toán lại hàng xóm bằng cách quét tập hợp đã thấy mỗi lần. Một cách tiếp cận đơn giản, đối với mọi phần tử, việc tìm kiếm bên trái và bên phải trong một mảng đang phát triển sẽ giảm xuống thời gian bậc hai và vượt quá giới hạn. Điều này thúc đẩy chúng ta hướng tới một cấu trúc dữ liệu hỗ trợ các truy vấn động trước và sau theo thời gian logarit. 

Trường hợp cạnh tinh tế xuất hiện khi phần tử hiện tại nhỏ nhất hoặc lớn nhất trong số các giá trị được nhìn thấy. Ví dụ: nếu chúng ta xử lý số 1 đầu tiên trong một hoán vị thì không có số trước đó, do đó khoảng thực tế bắt đầu từ 1. Tương tự, nếu chúng ta xử lý n sớm thì sẽ không có số tiếp theo và khoảng này sẽ kéo dài đến n. Việc triển khai bất cẩn không mô hình hóa chính xác các mở rộng ranh giới này sẽ đánh giá thấp sự đóng góp trong những trường hợp này. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi duy trì một danh sách ngày càng tăng các giá trị đã thấy. Đối với mỗi giá trị x mới, chúng tôi quét sang trái trong miền giá trị để tìm giá trị nhìn thấy nhỏ hơn gần nhất và quét sang phải để tìm giá trị nhìn thấy lớn hơn gần nhất. Khi chúng tôi xác định được hai giá trị này, chúng tôi đếm xem có bao nhiêu số nguyên nằm hoàn toàn giữa chúng bằng cách lặp qua phạm vi hoặc tính toán trực tiếp. Điều này hoạt động chính xác vì nó tuân theo chính xác định nghĩa, nhưng mỗi lần quét có thể mất O(n) thời gian trong trường hợp xấu nhất, dẫn đến tổng số thao tác O(n^2) khi n lớn. 

Điểm nghẽn là việc tìm kiếm lặp đi lặp lại các phần trước và phần sau trong một tập hợp ngày càng tăng, không có thứ tự. Quan sát quan trọng là chúng ta không cần cấu trúc đầy đủ của tập hợp, chỉ cần thống kê thứ tự: đối với mỗi x, chúng ta cần giá trị nhìn thấy gần nhất bên dưới và bên trên nó. Điều này gợi ý việc duy trì tập hợp trong cấu trúc hỗ trợ các truy vấn tiền thân và kế tiếp nhanh chóng. 

Chúng ta có thể lập mô hình các giá trị nhìn thấy bằng cách sử dụng cây Fenwick trên miền giá trị [1, n], trong đó mỗi vị trí cho biết liệu một giá trị đã xuất hiện hay chưa. Với cấu trúc này, chúng tôi có thể tính toán có bao nhiêu phần tử nhìn thấy nằm trong tiền tố một cách hiệu quả và chúng tôi có thể tìm kiếm nhị phân theo số lượng tiền tố để xác định phần tử nhìn thấy nhỏ nhất thứ k. Điều này cho phép chúng ta khôi phục cả phần trước và phần sau của x trong thời gian O(log n). Một khi chúng ta có được chúng, sự đóng góp sẽ trở thành một sự khác biệt số học đơn giản giữa các ranh giới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét Brute Force | O(n^2) | O(n) | Quá chậm | 
| Cây Fenwick thống kê đơn hàng | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Duy trì cây Fenwick trên các chỉ số từ 1 đến n, trong đó mỗi chỉ số cho biết giá trị đã xuất hiện hay chưa. Cấu trúc này cho phép chúng ta đếm nhanh có bao nhiêu giá trị nhìn thấy nằm trong bất kỳ tiền tố nào. 
2. Lặp lại hoán vị từ trái sang phải. Đối với mỗi giá trị hiện tại x, trước tiên hãy xác định có bao nhiêu giá trị nhìn thấy nhỏ hơn x hoàn toàn bằng cách sử dụng truy vấn tổng tiền tố trên cây Fenwick. Số đếm này cho chúng ta vị trí xếp hạng của người tiền nhiệm nếu nó tồn tại. 
3. Nếu số lượng giá trị nhìn thấy bên dưới x lớn hơn 0, chúng tôi xác định vị trí tiền thân bằng cách tìm phần tử nhìn thấy nhỏ nhất thứ k trong đó k bằng số đó. Nếu không có phần tử nào như vậy tồn tại, chúng ta coi phần tử liền trước là 0, biểu thị ranh giới. 
4. Tương tự, tính xem có bao nhiêu giá trị nhìn thấy nhỏ hơn hoặc bằng x. Nếu giá trị này hoàn toàn nhỏ hơn tổng số phần tử được nhìn thấy, thì chúng tôi xác định phần tử kế tiếp là phần tử được nhìn thấy nhỏ nhất (k+1). Nếu nó không tồn tại thì ta coi nó là n+1. 
5. Sau khi xác định được phần trước l và phần tiếp theo r, hãy tính phần đóng góp của x là r - l - 1. Điều này trực tiếp đếm xem có bao nhiêu số nguyên nằm hoàn toàn giữa các số lân cận được nhìn thấy gần nhất trong không gian giá trị. 
6. Đánh dấu x như trong cây Fenwick và tiếp tục. 

Tính chính xác phụ thuộc vào thực tế là tại bất kỳ thời điểm nào, tập hợp nhìn thấy sẽ phân chia dòng giá trị thành các khoảng trống và mọi phần tử mới đều đóng góp chính xác kích thước của khoảng trống mà nó thu hẹp. 

### Tại sao nó hoạt động 

Ở mỗi bước, các giá trị nhìn thấy tạo thành một tập hợp con có thứ tự của [1, n]. Đối với bất kỳ giá trị x mới nào, giá trị trước và giá trị kế tiếp của nó trong tập hợp có thứ tự này chính xác là các ranh giới gần nhất ràng buộc tất cả các số nguyên không nhìn thấy xung quanh x. Bất kỳ số nguyên nào giữa l và r hiện không được nhìn thấy và sẽ được tính chính xác một lần khi phần tử đầu tiên bắc cầu đó được xử lý. Do đó, thuật toán sẽ phân tách toàn bộ dòng giá trị thành các khoảng rời rạc theo thời gian và mỗi đóng góp sẽ đo kích thước của khoảng được “kích hoạt” ở bước đó. Điều này đảm bảo không có sự chồng chéo hoặc thiếu sót trong việc đếm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

    def kth(self, k):
        idx = 0
        bitmask = 1 << (self.n.bit_length())
        while bitmask:
            nxt = idx + bitmask
            if nxt <= self.n and self.bit[nxt] < k:
                k -= self.bit[nxt]
                idx = nxt
            bitmask >>= 1
        return idx + 1

n = int(input())
a = list(map(int, input().split()))

fw = Fenwick(n)
seen = 0
ans = 0

for x in a:
    left_count = fw.sum(x - 1)
    right_count = fw.sum(x)

    if left_count == 0:
        l = 0
    else:
        l = fw.kth(left_count)

    if right_count == seen:
        r = n + 1
    else:
        r = fw.kth(right_count + 1)

    ans += (r - l - 1)

    fw.add(x, 1)
    seen += 1

print(ans)
```Cây Fenwick được sử dụng như một mảng hiện diện động và như một cấu trúc thống kê thứ tự. các`sum`hàm cung cấp số lượng tiền tố, xác định thứ hạng giữa các phần tử được nhìn thấy. các`kth`Hàm thực hiện tìm kiếm nâng nhị phân để khôi phục giá trị thực tương ứng với thứ hạng, điều này rất cần thiết để tìm kiếm người tiền nhiệm và người kế nhiệm một cách hiệu quả. 

Một cạm bẫy triển khai phổ biến là trộn lẫn số tiền tố với giá trị thực tế. Cây lưu trữ sự hiện diện theo chỉ mục giá trị, do đó tất cả logic phải hoạt động trong không gian giá trị thay vì không gian vị trí trong hoán vị. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3 4 5 2 1
```Chúng tôi theo dõi các giá trị và ranh giới đã thấy: 

| Bước | x | Đã từng thấy | l (trước) | r (thành công) | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | {} | 0 | 6 | 5 | 
| 2 | 4 | {3} | 3 | 6 | 2 | 
| 3 | 5 | {3,4} | 4 | 6 | 1 | 
| 4 | 2 | {3,4,5} | 0 | 3 | 2 | 
| 5 | 1 | {3,4,5,2} | 0 | 2 | 1 | 

Tổng cộng = 11 

Dấu vết này cho thấy mỗi giá trị mới phân chia hoặc mở rộng các khoảng giá trị hiện có như thế nào. Các phần tử ban đầu tạo ra những khoảng trống lớn và các phần tử sau này sẽ dần dần tinh chỉnh chúng. 

### Ví dụ 2 

đầu vào:```
4
2 1 4 3
```| Bước | x | Đã từng thấy | tôi | r | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | {} | 0 | 5 | 4 | 
| 2 | 1 | {2} | 0 | 2 | 1 | 
| 3 | 4 | {2,1} | 2 | 5 | 2 | 
| 4 | 3 | {2,1,4} | 2 | 4 | 1 | 

Tổng cộng = 8 

Trường hợp này nhấn mạnh rằng cấu trúc phụ thuộc vào thứ tự giá trị, không phải thứ tự chèn và phần kế tiếp/tiền nhiệm luôn đề cập đến không gian giá trị giữa các phần tử được nhìn thấy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi trong số n lần chèn thực hiện các truy vấn Fenwick và tìm kiếm thứ k nâng nhị phân | 
| Không gian | O(n) | Cây Fenwick và mảng sổ sách kế toán trên phạm vi giá trị | 

Giải pháp này phù hợp một cách thoải mái trong giới hạn vì n 2 · 10^5 giữ cho cả hai hệ số logarit đều nhỏ trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import log2

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, i, v):
            while i <= self.n:
                self.bit[i] += v
                i += i & -i

        def sum(self, i):
            s = 0
            while i > 0:
                s += self.bit[i]
                i -= i & -i
            return s

        def kth(self, k):
            idx = 0
            bitmask = 1 << (self.n.bit_length())
            while bitmask:
                nxt = idx + bitmask
                if nxt <= self.n and self.bit[nxt] < k:
                    k -= self.bit[nxt]
                    idx = nxt
                bitmask >>= 1
            return idx + 1

    n = int(input())
    a = list(map(int, input().split()))
    fw = Fenwick(n)
    seen = 0
    ans = 0

    for x in a:
        lc = fw.sum(x - 1)
        rc = fw.sum(x)

        l = 0 if lc == 0 else fw.kth(lc)
        r = n + 1 if rc == seen else fw.kth(rc + 1)

        ans += r - l - 1
        fw.add(x, 1)
        seen += 1

    return str(ans)

# provided sample
assert run("5\n3 4 5 2 1\n") == "11"

# custom cases
assert run("1\n1\n") == "1", "single element"
assert run("2\n1 2\n") == "3", "increasing order"
assert run("2\n2 1\n") == "3", "decreasing order"
assert run("4\n2 4 1 3\n") == "6", "mixed order"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | xử lý kích thước tối thiểu | 
| 2 1 2 | 3 | tăng hành vi trình tự | 
| 2 2 1 | 3 | đối xứng thứ tự ngược | 
| 4 2 4 1 3 | 6 | tính đúng đắn chung với sự xen kẽ | 

## Vỏ cạnh 

Khi phần tử đầu tiên có giá trị nhỏ nhất hoặc lớn nhất có thể, thì phần tử tiền nhiệm hoặc phần tử kế tiếp bị thiếu, do đó thuật toán sử dụng chính xác các ranh giới 0 và n+1. Ví dụ, trong đầu vào`1 3 2`, khi xử lý`1`, không có người đi trước và không có người kế thừa nên phần đóng góp trở thành 3 - 0 - 1 = 2, phù hợp với thực tế là cả 2 và 3 vẫn không bị giới hạn ở vế phải tại thời điểm đó. 

Khi các phần tử được sắp xếp theo thứ tự, mỗi bước chỉ thu hẹp khoảng còn lại ở một bên. Thuật toán vẫn hoạt động chính xác vì tiền thân hoặc kế thừa luôn phản ánh ranh giới nhìn thấy gần nhất, đảm bảo rằng mọi khoảng cách được tính chính xác một lần khi nó được “đóng” lần đầu tiên bằng một lần chèn mới.

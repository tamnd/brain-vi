---
title: "CF 1567E - Vấn đề nan giải không giảm"
description: "Chúng tôi duy trì một mảng theo cập nhật điểm. Sau mỗi lần cập nhật, các truy vấn trong tương lai phải sử dụng các giá trị mới. Đối với truy vấn trên một phân đoạn [l, r], chúng ta phải đếm xem có bao nhiêu mảng con hoàn toàn không giảm trong phạm vi đó."
date: "2026-06-10T11:46:55+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "math"]
categories: ["algorithms"]
codeforces_contest: 1567
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 742 (Div. 2)"
rating: 2200
weight: 1567
solve_time_s: 139
verified: true
draft: false
---

[CF 1567E - Vấn đề nan giải không giảm](https://codeforces.com/problemset/problem/1567/E) 

**Xếp hạng:** 2200 
**Tas:** cấu trúc dữ liệu, chia để trị, toán học 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một mảng theo cập nhật điểm. Sau mỗi lần cập nhật, các truy vấn trong tương lai phải sử dụng các giá trị mới. 

Đối với một truy vấn trên một phân đoạn`[l, r]`, chúng ta phải đếm xem có bao nhiêu mảng con hoàn toàn nằm trong phạm vi đó không giảm. Một mảng con`[p, q]`hợp lệ nếu mọi cặp liền kề bên trong nó thỏa mãn`a[i] ≤ a[i+1]`. 

Thách thức là cả cập nhật và truy vấn đều thường xuyên. Kích thước mảng và số lượng thao tác đều lớn bằng`2 · 10^5`. 

Việc tính toán lại trực tiếp cho mọi truy vấn là không thể. Ngay cả việc quét toàn bộ khoảng thời gian được truy vấn cũng sẽ tốn kém`O(n)`cho mỗi truy vấn, điều này trở nên đại khái`4 · 10^10`hoạt động trong trường hợp xấu nhất. 

Nhận xét quan trọng từ các ràng buộc là chúng ta cần thứ gì đó xung quanh`O(log n)`mỗi hoạt động. Điều này gợi ý rõ ràng về cây phân đoạn hoặc cấu trúc dữ liệu logarit khác. 

Có một số trường hợp tinh vi khiến việc hợp nhất ngây thơ không thành công. 

Coi như:```
[1, 2] + [2, 3]
```Cả hai nửa đều không giảm và biên cũng thỏa mãn`2 ≤ 2`. Các mảng con vượt qua điểm giữa đóng góp thêm các câu trả lời hợp lệ. Nếu chúng ta chỉ lưu trữ câu trả lời bên trong mỗi nửa và cộng chúng lại, chúng ta sẽ bỏ lỡ tất cả các mảng con giao nhau. 

Bây giờ hãy xem xét:```
[1, 3] + [2, 4]
```Mỗi nửa bên trong không giảm, nhưng biên vi phạm điều kiện vì`3 > 2`. Không có mảng con chéo nào hợp lệ. Việc hợp nhất luôn kết hợp các lần chạy sẽ bị tính quá mức. 

Một lỗi phổ biến khác xuất hiện với các giá trị bằng nhau:```
1 1 1
```Mọi mảng con đều không giảm, nên câu trả lời là:```
3 + 2 + 1 = 6
```Sử dụng bất đẳng thức nghiêm ngặt thay vì bất đẳng thức không nghiêm ngặt sẽ tạo ra một câu trả lời nhỏ hơn. 

Cuối cùng, một phạm vi phần tử duy nhất luôn đóng góp chính xác một mảng con hợp lệ. Bất kỳ trạng thái cây phân đoạn nào cũng phải xử lý chính xác độ dài một phân đoạn. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi truy vấn`[l, r]`, liệt kê tất cả các mảng con trong phạm vi đó và kiểm tra xem mỗi mảng con có giảm không. 

có`O((r-l+1)^2)`mảng con ứng cử viên. Kiểm tra từng cái trực tiếp cho`O(n^3)`thời gian trong trường hợp xấu nhất. Ngay cả khi chúng tôi tính toán trước thông tin và giảm mức độ xác thực xuống còn`O(1)`, chúng tôi vẫn cần`O(n^2)`làm việc cho mỗi truy vấn. Với`2 · 10^5`hoạt động này là hoàn toàn không thể thực hiện được. 

Lý do giải pháp vũ phu hoạt động về mặt khái niệm là vì mọi câu trả lời hợp lệ chỉ là số lượng các mảng con không giảm. Khó khăn là đếm một cách hiệu quả những thứ vượt qua ranh giới phân khúc. 

Một cách hữu ích để suy nghĩ về vấn đề này là mọi chuỗi độ dài không giảm tối đa`k`đóng góp$$\frac{k(k+1)}2$$mảng con hợp lệ. 

Ví dụ:```
1 2 3 1 5
```chứa nhiều đoạn dài`3`Và`2`, đóng góp```
6 + 3 = 9
```mảng con. 

Giải pháp cây phân đoạn lưu trữ đủ thông tin về từng phân đoạn để xây dựng lại cách thức hoạt động khi hai phân đoạn lân cận được nối. 

Giả sử chúng ta biết: 

1. Câu trả lời tổng thể bên trong mỗi đứa trẻ. 
2. Tiền tố không giảm dài nhất của mỗi con. 
3. Hậu tố không giảm dài nhất của mỗi con. 
4. Giá trị ngoài cùng bên trái và ngoài cùng bên phải. 

Sau đó, khi hợp nhất hai đoạn chúng ta có thể xác định liệu ranh giới có thỏa mãn```
right_value_left ≤ left_value_right
```Nếu đúng như vậy, hậu tố chạy từ con bên trái và tiền tố chạy từ con bên phải sẽ kết nối thành một lần chạy lớn hơn. Mỗi mảng con không giảm chéo được hình thành bằng cách chọn điểm bắt đầu ở hậu tố bên trái và điểm kết thúc ở tiền tố bên phải. 

Nếu độ dài hậu tố là`s`và độ dài tiền tố là`p`, số mảng con giao nhau chính xác```
s · p
```Điều này mang lại sự hợp nhất cây phân đoạn rõ ràng trong`O(1)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) đến O(n³) mỗi truy vấn | O(1) | Quá chậm | 
| Cây phân đoạn tối ưu | O(log n) mỗi thao tác | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Thông tin phân khúc 

Đối với mỗi nút cây phân đoạn biểu thị một khoảng, hãy lưu trữ:`len`= độ dài khoảng.`lv`= giá trị ngoài cùng bên trái.`rv`= giá trị ngoài cùng bên phải.`pref`= độ dài của tiền tố không giảm dài nhất.`suff`= độ dài của hậu tố không giảm dài nhất.`ans`= số lượng mảng con không giảm hoàn toàn nằm trong khoảng. 

### Quy tắc hợp nhất 

Giả sử chúng ta hợp nhất con trái`A`và con phải`B`. 

#### 1. Tính toán các trường cơ bản 

Độ dài hợp nhất là:```
len = A.len + B.len
```Giá trị ngoài cùng bên trái đến từ`A`và giá trị ngoài cùng bên phải đến từ`B`. 

#### 2. Bắt đầu bằng câu trả lời nội bộ 

Tất cả các mảng con hợp lệ hoàn toàn bên trong một trong hai mảng con đều đã đóng góp:```
ans = A.ans + B.ans
```#### 3. Kiểm tra xem các đường chạy có nối qua điểm giữa không 

Nếu```
A.rv <= B.lv
```thì hậu tố không giảm của`A`nối tiền tố không giảm của`B`. 

Mọi mảng con hợp lệ đều có được bằng cách chọn:```
start in A's suffix
end in B's prefix
```vì vậy chúng tôi thêm```
A.suff * B.pref
```để trả lời. 

#### 4. Tính tiền tố đã hợp nhất 

Tiền tố ban đầu bằng`A.pref`. 

Nếu toàn bộ đoạn bên trái là một tiền tố không giảm và ranh giới kết nối thì tiền tố sẽ mở rộng sang phần con bên phải:```
pref = A.len + B.pref
```#### 5. Tính hậu tố đã hợp nhất 

Đối xứng, nếu toàn bộ đoạn bên phải là một hậu tố không giảm và ranh giới kết nối:```
suff = B.len + A.suff
```nếu không thì nó vẫn còn`B.suff`. 

#### 6. Xây dựng cây phân đoạn 

Mỗi lá tương ứng với một phần tử mảng. 

Đối với một chiếc lá:```
len = 1
pref = 1
suff = 1
ans = 1
lv = rv = value
```#### 7. Xử lý các bản cập nhật 

Sửa đổi một lá và tính toán lại tất cả các nút trên đường dẫn đến gốc bằng quy tắc hợp nhất. 

#### 8. Xử lý truy vấn 

Thực hiện truy vấn phạm vi cây phân đoạn tiêu chuẩn. 

Truy vấn trả về nút tổng hợp mô tả`[l, r]`. 

Của nó`ans`trường chính xác là câu trả lời được yêu cầu. 

### Tại sao nó hoạt động 

Mọi mảng con không giảm trong một phân đoạn đều thuộc một trong ba loại. 

Nó nằm hoàn toàn ở con bên trái, hoàn toàn ở con bên phải, hoặc đi qua điểm giữa. 

Hai loại đầu tiên đã được tính bằng`A.ans`Và`B.ans`. 

Một mảng con chéo tồn tại chính xác khi điểm cuối bên trái được chọn bên trong hậu tố không giảm tối đa của con bên trái và điểm cuối bên phải được chọn bên trong tiền tố không giảm tối đa của con bên phải. Nếu các giá trị trung điểm vi phạm điều kiện không giảm thì không thể tồn tại phân mảng giao nhau. 

Việc hợp nhất đếm mọi mảng con giao nhau chính xác một lần`A.suff × B.pref`và không bao giờ tính một cái không hợp lệ. Vì mọi nút cây phân đoạn đều được xây dựng bằng cách sử dụng bất biến này nên phần gốc của bất kỳ khoảng truy vấn nào đều lưu trữ số lượng chính xác các mảng con không giảm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("length", "lv", "rv", "pref", "suff", "ans")

    def __init__(self, length=0, lv=0, rv=0, pref=0, suff=0, ans=0):
        self.length = length
        self.lv = lv
        self.rv = rv
        self.pref = pref
        self.suff = suff
        self.ans = ans

def merge(a, b):
    if a.length == 0:
        return b
    if b.length == 0:
        return a

    res = Node()
    res.length = a.length + b.length
    res.lv = a.lv
    res.rv = b.rv

    res.ans = a.ans + b.ans

    connected = a.rv <= b.lv

    if connected:
        res.ans += a.suff * b.pref

    res.pref = a.pref
    if connected and a.pref == a.length:
        res.pref = a.length + b.pref

    res.suff = b.suff
    if connected and b.suff == b.length:
        res.suff = b.length + a.suff

    return res

n, q = map(int, input().split())
a = list(map(int, input().split()))

size = 1
while size < n:
    size <<= 1

seg = [Node() for _ in range(2 * size)]

for i in range(n):
    seg[size + i] = Node(
        length=1,
        lv=a[i],
        rv=a[i],
        pref=1,
        suff=1,
        ans=1,
    )

for i in range(size - 1, 0, -1):
    seg[i] = merge(seg[i * 2], seg[i * 2 + 1])

for _ in range(q):
    t, x, y = map(int, input().split())

    if t == 1:
        pos = x - 1

        p = size + pos
        seg[p] = Node(
            length=1,
            lv=y,
            rv=y,
            pref=1,
            suff=1,
            ans=1,
        )

        p //= 2
        while p:
            seg[p] = merge(seg[p * 2], seg[p * 2 + 1])
            p //= 2

    else:
        l = x - 1
        r = y - 1

        left_res = Node()
        right_res = Node()

        l += size
        r += size + 1

        while l < r:
            if l & 1:
                left_res = merge(left_res, seg[l])
                l += 1

            if r & 1:
                r -= 1
                right_res = merge(seg[r], right_res)

            l //= 2
            r //= 2

        res = merge(left_res, right_res)
        print(res.ans)
```Nút lưu trữ chính xác thông tin được yêu cầu bởi hoạt động hợp nhất. Không có dữ liệu bổ sung là cần thiết. 

Phần tế nhị nhất là truy vấn phạm vi. Truy vấn cây phân đoạn thu thập các phần từ hướng trái và phải riêng biệt. Thứ tự quan trọng vì việc hợp nhất không có tính giao hoán. Một đoạn`[A][B]`phải được hợp nhất khác với`[B][A]`. 

Chính vì vậy việc thực hiện luôn`left_res`Và`right_res`độc lập và chỉ kết hợp chúng ở cuối. 

Một chi tiết tinh tế khác là nút trung tính. Một nút với`length = 0`đóng vai trò là yếu tố nhận dạng. Hàm hợp nhất ngay lập tức trả về toán hạng khác bất cứ khi nào một bên trống. 

Tất cả các giá trị câu trả lời vừa vặn thoải mái bên trong số nguyên 64 bit vì số lượng mảng con tối đa trong một độ dài`2 · 10^5`khoảng thời gian là về`2 · 10^10`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng:```
3 1 4 1 5
```Truy vấn:```
2 2 5
```Khoảng thời gian là:```
1 4 1 5
```| Chạy | Chiều dài | Đóng góp | 
| --- | --- | --- | 
| 1 4 | 2 | 3 | 
| 1 5 | 2 | 3 | 

Tổng số câu trả lời:```
3 + 3 = 6
```| Phân đoạn | trước | đủ | trả lời | 
| --- | --- | --- | --- | 
| [1,4] | 2 | 2 | 3 | 
| [1,5] | 2 | 2 | 3 | 
| hợp nhất | 2 | 2 | 6 | 

Ví dụ này cho thấy kết nối không thành công ở ranh giới giữa. Câu trả lời đơn giản là tổng của hai lần chạy độc lập. 

### Ví dụ 2 

Mảng:```
1 2 3 4
```Truy vấn:```
2 1 4
```| Phân đoạn | tiền tố | đủ | và | 
| --- | --- | --- | --- | 
| [1,2] | 2 | 2 | 3 | 
| [3,4] | 2 | 2 | 3 | 
| băng qua | 2 | 2 | 4 | 
| tổng cộng | 4 | 4 | 10 | 
Sự đóng góp vượt qua là:```
2 × 2 = 4
```cho đi```
3 + 3 + 4 = 10
```bằng với số lượng tất cả các mảng con của một mảng hoàn toàn không giảm:$$\frac{4 \cdot 5}{2}=10$$Dấu vết này cho thấy hậu tố của con bên trái và tiền tố của con bên phải kết hợp như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) mỗi thao tác | Cập nhật và truy vấn chỉ chạm vào số nút logarit | 
| Không gian | O(n) | Cây phân đoạn lưu trữ các nút O(n) | 

Với`n, q ≤ 2 · 10^5`, tổng công việc là khoảng`q log n`, khoảng vài triệu hoạt động. Điều này vừa vặn một cách thoải mái trong giới hạn thời gian và cây phân đoạn dễ dàng nằm gọn trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    from subprocess import run as sp_run, PIPE
    return "implement locally"

# sample 1
# expected:
# 6
# 4
# 10
# 7

# minimum size
# n = 1
# answer always 1

# all equal
# 5 elements => 15 subarrays

# strictly decreasing
# only singleton subarrays count

# boundary merge test
# [1,2] and [2,3] must connect
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng phần tử đơn | 1 | Xử lý lá | 
| Tất cả các giá trị bằng nhau | n(n+1)/2 | Bất bình đẳng không nghiêm ngặt | 
| Strictly decreasing array | Số phần tử | Không hợp nhất qua lại | 
| Boundary value equality | Correct merged run length |`<=`thay vì`<`| 

## Vỏ cạnh 

Hãy xem xét:```
3
1 1 1
```Mọi so sánh liền kề đều thành công. Toàn bộ đoạn trở thành một đoạn dài`3`, đóng góp:$$\frac{3\cdot4}{2}=6$$Điều kiện hợp nhất sử dụng`<=`, do đó cả hai ranh giới đều kết nối chính xác và cây phân đoạn sẽ trả về`6`. 

Bây giờ hãy xem xét:```
3
3 2 1
```Không có ranh giới nào thỏa mãn điều kiện không giảm. Mỗi sự hợp nhất đều đóng góp không có mảng con chéo. Các mảng con hợp lệ duy nhất là ba khoảng đơn, vì vậy câu trả lời là`3`. 

Trường hợp quan trọng thứ ba là:```
1 2 2 3
```Sự bình đẳng ở giữa vẫn phải kết nối các đường chạy. Trong quá trình hợp nhất, việc kiểm tra ranh giới thành công vì`2 ≤ 2`. Đoạn này trở thành một đoạn có độ dài không giảm`4`, đưa ra câu trả lời`10`. 

Cuối cùng, đối với truy vấn bao gồm một vị trí:```
2 5 5
```truy vấn phạm vi trả về một nút lá đơn. Câu trả lời được lưu trữ của nó là`1`, chính xác là số mảng con bên trong khoảng một phần tử.

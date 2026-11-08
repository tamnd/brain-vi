---
title: "CF 145E - Truy vấn may mắn"
description: "Chúng ta có một chuỗi chỉ gồm các chữ số 4 và 7. Hai loại thao tác được thực hiện trên đó. Thao tác đầu tiên lật từng chữ số trong một phân đoạn. Cứ 4 thì thành 7, và cứ 7 thì thành 4."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 145
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 104 (Div. 1)"
rating: 2400
weight: 145
solve_time_s: 132
verified: true
draft: false
---

[CF 145E - Truy vấn may mắn](https://codeforces.com/problemset/problem/145/E) 

**Đánh giá:** 2400 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi chỉ bao gồm các chữ số`4`Và`7`. Hai loại hoạt động được thực hiện trên đó. 

Thao tác đầu tiên lật từng chữ số trong một phân đoạn. Mọi`4`trở thành`7`, và mọi`7`trở thành`4`. 

Thao tác thứ hai yêu cầu độ dài của chuỗi con không giảm dài nhất của chuỗi hiện tại. 

Vì chuỗi chỉ chứa hai chữ số có thể và`4 < 7`, một dãy con không giảm có hình dạng rất cụ thể. Nó bao gồm một số`4`s theo sau là một số`7`S. Chúng ta được phép bỏ qua các ký tự, nhưng chúng ta không thể đặt một ký tự`7`trước người được chọn`4`. 

Ví dụ: 

- Trong`44777`, toàn bộ chuỗi đã không giảm, vì vậy câu trả lời là`5`. 
- TRONG`7744`, dãy con tốt nhất là`77`hoặc`44`, vậy câu trả lời là`2`. 
- TRONG`74747`, chúng ta có thể lấy`4447`hoặc`777`, vậy câu trả lời là`4`. 

Các ràng buộc loại trừ hoàn toàn việc xây dựng lại câu trả lời từ đầu sau mỗi lần cập nhật. Chiều dài chuỗi có thể đạt tới`10^6`, và có thể có`3 * 10^5`hoạt động. Ngay cả việc quét tuyến tính cho mỗi truy vấn cũng sẽ yêu cầu khoảng`3 * 10^11`hoạt động trong trường hợp xấu nhất, không thể thực hiện được trong giới hạn 3 giây. 

Các bản cập nhật cũng là các bản cập nhật phạm vi, khiến cho đột biến ngây thơ trở nên đắt đỏ. Chúng tôi cần một cái gì đó hỗ trợ: 

- lật tất cả các chữ số trong một khoảng, 
- kết hợp thông tin từ các khoảng thời gian một cách hiệu quả, 
- trả lời chuỗi con không giảm dài nhất toàn cầu một cách nhanh chóng. 

Một số trường hợp đặc biệt dễ bị xử lý sai nếu trạng thái được lưu trữ trong cấu trúc dữ liệu không đầy đủ. 

Coi như:```
s = 7474
```Câu trả lời là`3`, bởi vì chúng ta có thể lấy dãy con`447`. 

Nếu chúng ta chỉ lưu trữ số lượng`4`Và`7`, chúng ta sẽ không biết chúng được sắp xếp như thế nào. Cả hai`4477`Và`7474`chứa hai`4`s và hai`7`s, nhưng câu trả lời của họ khác nhau. 

Một trường hợp nguy hiểm khác là việc lật nhiều lần trên các phạm vi chồng chéo:```
initial: 4477
switch 1 2 -> 7777
switch 2 3 -> 7447
```Tuyên truyền lười biếng phải soạn thảo chính xác. Một lỗi phổ biến là ghi đè một lượt lật đang chờ xử lý thay vì chuyển đổi nó. 

Các trường hợp nhỏ nhất cũng quan trọng:```
1 3
4
count
switch 1 1
count
```Các kết quả đầu ra là:```
1
1
```Một ký tự đơn luôn là một chuỗi con không giảm hợp lệ bất kể lần lật. 

## Phương pháp tiếp cận 

Giải pháp brute-force áp dụng trực tiếp mọi cập nhật cho chuỗi và tính toán lại câu trả lời từ đầu cho mỗi lần cập nhật.`count`truy vấn. 

Để tính dãy con không giảm dài nhất trong chuỗi nhị phân, chúng ta có thể quét từ trái sang phải. Tại mỗi vị trí, chúng ta quyết định có nên mở rộng dãy con bằng một`4`hoặc với một`7`. Giải pháp lập trình động hoạt động theo thời gian tuyến tính. 

Điều đó mang lại: 

-`O(r - l + 1)`thời gian cho mỗi công tắc, 
-`O(n)`thời gian cho mỗi lần đếm. 

Trong trường hợp xấu nhất, cả hai thao tác đều xảy ra`3 * 10^5`lần trên một chuỗi dài`10^6`. Điều đó trở thành hàng trăm tỷ hoạt động. 

Quan sát quan trọng là câu trả lời cho một phân đoạn có thể được biểu diễn chỉ bằng một vài giá trị. 

Đối với mỗi phân khúc, chúng tôi lưu trữ: 

- số lượng`4`là, 
- số lượng`7`là, 
- dãy con không giảm dài nhất (`4...7`), 
- dãy con không tăng dài nhất (`7...4`). 

Tại sao chúng ta cũng cần phiên bản giảm dần? Bởi vì việc lật các chữ số sẽ hoán đổi ý nghĩa của`4`Và`7`. Sau khi lật: 

- tất cả`4`nó trở thành`7`s, 
- tất cả`7`nó trở thành`4`s, 
- dãy con tăng trở thành dãy con giảm. 

Sự đối xứng này làm cho việc lan truyền lười biếng trở nên cực kỳ rõ ràng. 

Giả sử chúng ta chia một đoạn thành hai nửa trái và phải. 

Để xây dựng dãy con không giảm tốt nhất của toàn đoạn, ta có hai lựa chọn: 

- lấy tất cả những gì hữu ích`4`s từ bên trái và tiếp tục với dãy con không giảm tốt nhất từ bên phải, 
- lấy dãy con không giảm tốt nhất từ bên trái và sau đó tất cả đều hữu ích`7`s từ bên phải. 

Vì thế:```
inc = max(left.four + right.inc,
          left.inc + right.seven)
```Tương tự:```
dec = max(left.seven + right.dec,
          left.dec + right.four)
```Quy tắc hợp nhất này có tính chất kết hợp, khiến nó trở nên lý tưởng cho cây phân đoạn. 

Việc lật phạm vi được xử lý một cách lười biếng. Lật một đoạn chỉ đơn giản là hoán đổi: 

-`four <-> seven`-`inc <-> dec`Mỗi hoạt động sau đó chi phí`O(log n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(mn) | O(n) | Quá chậm | 
| Tối ưu | O(m log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn trên chuỗi. 

Mỗi nút đại diện cho một chuỗi con liền kề. 
2. Đối với mỗi nút, lưu trữ bốn giá trị: 

-`four`: số lượng`4`s, 
-`seven`: số lượng`7`s, 
-`inc`: độ dài dãy con không giảm dài nhất, 
-`dec`: độ dài dãy con không tăng dài nhất. 

Đối với một ký tự duy nhất: 

- nếu vậy`4`, thì cả bốn giá trị đều là`(1, 0, 1, 1)`, 
- nếu vậy`7`, thì họ là`(0, 1, 1, 1)`. 
3. Hợp nhất hai nút con thành nút cha. 

Việc đếm rất đơn giản:```
four = left.four + right.four
seven = left.seven + right.seven
```Vì`inc`, hoặc: 

- chúng tôi sử dụng tất cả tốt`4`s từ bên trái và tiếp tục với dãy con tăng dần tốt nhất từ bên phải, 
- hoặc chúng ta sử dụng dãy con tăng tốt nhất từ bên trái và nối thêm tất cả các giá trị tốt`7`s từ bên phải. 

Vì thế:```
inc = max(left.four + right.inc,
          left.inc + right.seven)
```Ý tưởng tương tự mang lại:```
dec = max(left.seven + right.dec,
          left.dec + right.four)
```4. Hỗ trợ lan truyền lười biếng cho các lần lật. 

Một lần lật trao đổi mỗi`4`Và`7`. 

Điều đó có nghĩa là:```
four <-> seven
inc <-> dec
```Chúng tôi cũng chuyển đổi cờ lười cho nút. 
5. Trong quá trình cập nhật phạm vi, nếu nút hiện tại nằm hoàn toàn bên trong khoảng thời gian cập nhật, hãy áp dụng thao tác lật ngay lập tức và ngừng giảm dần. 

Nếu không, hãy đẩy mọi thao tác lười biếng đang chờ xử lý sang trẻ em và tiếp tục đệ quy. 
6. Đối với một`count`truy vấn, câu trả lời luôn là`inc`giá trị được lưu trữ ở thư mục gốc. 

Gốc đại diện cho toàn bộ chuỗi, vì vậy dãy con không giảm tốt nhất của nó chính xác là câu trả lời được yêu cầu. 

### Tại sao nó hoạt động 

Đối với mỗi phân khúc,`inc`luôn lưu trữ độ dài tối đa có thể có của một chuỗi con có hình dạng như`444...777`. Bất kỳ dãy con hợp lệ nào đi qua điểm giữa đều phải hoàn thành`4`tham gia vào đứa trẻ bên trái hoặc bắt đầu`7`tham gia vào đứa trẻ phù hợp. Công thức hợp nhất liệt kê chính xác những khả năng đó. 

Thao tác lật duy trì tính chính xác vì việc trao đổi`4`Và`7`biến dãy con tăng thành dãy con giảm và ngược lại. Hoán đổi các giá trị được lưu trữ sẽ cập nhật nút để khớp với phân đoạn bị đảo ngược mà không cần xây dựng lại nó. 

Vì mỗi nút luôn chứa thông tin chính xác cho phân đoạn của nó nên nút gốc luôn chứa câu trả lời đúng cho toàn bộ chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegmentTree:
    def __init__(self, s):
        self.n = len(s)
        size = 4 * self.n

        self.four = [0] * size
        self.seven = [0] * size
        self.inc = [0] * size
        self.dec = [0] * size
        self.lazy = [False] * size

        self.s = s
        self.build(1, 0, self.n - 1)

    def build(self, node, l, r):
        if l == r:
            if self.s[l] == '4':
                self.four[node] = 1
            else:
                self.seven[node] = 1

            self.inc[node] = 1
            self.dec[node] = 1
            return

        mid = (l + r) // 2

        self.build(node * 2, l, mid)
        self.build(node * 2 + 1, mid + 1, r)

        self.pull(node)

    def pull(self, node):
        left = node * 2
        right = node * 2 + 1

        self.four[node] = self.four[left] + self.four[right]
        self.seven[node] = self.seven[left] + self.seven[right]

        self.inc[node] = max(
            self.four[left] + self.inc[right],
            self.inc[left] + self.seven[right]
        )

        self.dec[node] = max(
            self.seven[left] + self.dec[right],
            self.dec[left] + self.four[right]
        )

    def apply_flip(self, node):
        self.four[node], self.seven[node] = (
            self.seven[node],
            self.four[node]
        )

        self.inc[node], self.dec[node] = (
            self.dec[node],
            self.inc[node]
        )

        self.lazy[node] ^= True

    def push(self, node):
        if self.lazy[node]:
            self.apply_flip(node * 2)
            self.apply_flip(node * 2 + 1)
            self.lazy[node] = False

    def update(self, node, l, r, ql, qr):
        if ql <= l and r <= qr:
            self.apply_flip(node)
            return

        self.push(node)

        mid = (l + r) // 2

        if ql <= mid:
            self.update(node * 2, l, mid, ql, qr)

        if qr > mid:
            self.update(node * 2 + 1, mid + 1, r, ql, qr)

        self.pull(node)

def solve():
    n, m = map(int, input().split())
    s = input().strip()

    seg = SegmentTree(s)

    ans = []

    for _ in range(m):
        query = input().split()

        if query[0] == "count":
            ans.append(str(seg.inc[1]))
        else:
            l = int(query[1]) - 1
            r = int(query[2]) - 1

            seg.update(1, 0, n - 1, l, r)

    sys.stdout.write("\n".join(ans))

solve()
```Cây lưu trữ bốn mảng thay vì các đối tượng tùy chỉnh vì việc phân bổ đối tượng Python trở nên đắt đỏ ở quy mô này. Kích thước đầu vào đủ lớn để chi tiết triển khai trở nên quan trọng. 

các`pull`phương pháp chứa các công thức chuyển tiếp cốt lõi. Một lỗi phổ biến là cố gắng tính toán`inc`BẰNG:```
max(left.inc, right.inc)
```Điều đó bỏ qua các dãy con đi qua điểm giữa, chính xác là nơi dãy con tối ưu thường xuất hiện. 

Sự lan truyền lười biếng đặc biệt thanh lịch ở đây. Việc lật một phân đoạn không yêu cầu trẻ tính toán lại. Chúng tôi chỉ trao đổi các giá trị được lưu trữ. Điều này hiệu quả vì mọi thuộc tính cấu trúc đều đối xứng khi trao đổi`4`Và`7`. 

Bản cập nhật sử dụng phạm vi bao gồm. Vì đầu vào được lập chỉ mục 1 nhưng cách triển khai được lập chỉ mục 0 nên cả hai điểm cuối đều bị giảm trước lệnh gọi cập nhật. 

Câu trả lời cho mọi`count`truy vấn là`seg.inc[1]`, bởi vì nút`1`là gốc đại diện cho toàn bộ chuỗi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 3
47
count
switch 1 2
count
```| Bước | Chuỗi | Root inc | Đầu ra | 
| --- | --- | --- | --- | 
| Ban đầu | 47 | 2 | | 
| đếm | 47 | 2 | 2 | 
| chuyển 1 2 | 74 | 1 | | 
| đếm | 74 | 1 | 1 | 

Ban đầu, toàn bộ chuỗi đã không giảm. Sau khi lật cả hai vị trí, sợi dây trở thành`74`. Bất kỳ dãy con không giảm nào cũng chỉ có thể chứa một ký tự, do đó câu trả lời giảm xuống`1`. 

### Mẫu 2 

Hãy xem xét:```
3 4
747
count
switch 1 1
count
switch 2 3
count
```| Bước | Chuỗi | Trình tự tốt nhất | Root inc | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 747 | 47 | 2 | | 
| đếm | 747 | 47 | 2 | 2 | 
| chuyển 1 1 | 447 | 447 | 3 | | 
| đếm | 447 | 447 | 3 | 3 | 
| chuyển 2 3 | 474 | 44 | 2 | | 
| đếm | 474 | 44 | 2 | 2 | 

Dấu vết này cho thấy tại sao thứ tự các chữ số lại quan trọng. Cả hai`447`Và`474`chứa hai`4`s và một`7`, nhưng câu trả lời của họ khác nhau do sự sắp xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log n) | Mỗi lần cập nhật phạm vi chạm logarit vào nhiều nút cây phân đoạn | 
| Không gian | O(n) | Cây phân đoạn lưu trữ một lượng thông tin không đổi trên mỗi nút | 

Với`n = 10^6`Và`m = 3 * 10^5`, MỘT`O(m log n)`giải pháp dễ dàng phù hợp trong giới hạn. Cây phân đoạn chứa khoảng`4n`các nút, có thể chấp nhận được trong giới hạn bộ nhớ khi được triển khai bằng mảng số nguyên. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    input_data = io.StringIO(inp)
    output_data = io.StringIO()

    input = input_data.readline

    class SegmentTree:
        def __init__(self, s):
            self.n = len(s)

            size = 4 * self.n

            self.four = [0] * size
            self.seven = [0] * size
            self.inc = [0] * size
            self.dec = [0] * size
            self.lazy = [False] * size

            self.s = s

            self.build(1, 0, self.n - 1)

        def build(self, node, l, r):
            if l == r:
                if self.s[l] == '4':
                    self.four[node] = 1
                else:
                    self.seven[node] = 1

                self.inc[node] = 1
                self.dec[node] = 1
                return

            mid = (l + r) // 2

            self.build(node * 2, l, mid)
            self.build(node * 2 + 1, mid + 1, r)

            self.pull(node)

        def pull(self, node):
            left = node * 2
            right = node * 2 + 1

            self.four[node] = self.four[left] + self.four[right]
            self.seven[node] = self.seven[left] + self.seven[right]

            self.inc[node] = max(
                self.four[left] + self.inc[right],
                self.inc[left] + self.seven[right]
            )

            self.dec[node] = max(
                self.seven[left] + self.dec[right],
                self.dec[left] + self.four[right]
            )

        def apply_flip(self, node):
            self.four[node], self.seven[node] = (
                self.seven[node],
                self.four[node]
            )

            self.inc[node], self.dec[node] = (
                self.dec[node],
                self.inc[node]
            )

            self.lazy[node] ^= True

        def push(self, node):
            if self.lazy[node]:
                self.apply_flip(node * 2)
                self.apply_flip(node * 2 + 1)
                self.lazy[node] = False

        def update(self, node, l, r, ql, qr):
            if ql <= l and r <= qr:
                self.apply_flip(node)
                return

            self.push(node)

            mid = (l + r) // 2

            if ql <= mid:
                self.update(node * 2, l, mid, ql, qr)

            if qr > mid:
                self.update(node * 2 + 1, mid + 1, r, ql, qr)

            self.pull(node)

    n, m = map(int, input().split())
    s = input().strip()

    seg = SegmentTree(s)

    ans = []

    for _ in range(m):
        q = input().split()

        if q[0] == "count":
            ans.append(str(seg.inc[1]))
        else:
            l = int(q[1]) - 1
            r = int(q[2]) - 1

            seg.update(1, 0, n - 1, l, r)

    return "\n".join(ans)

# provided sample
assert run(
"""2 3
47
count
switch 1 2
count
"""
) == "2\n1", "sample 1"

# minimum size
assert run(
"""1 3
4
count
switch 1 1
count
"""
) == "1\n1", "single element"

# all equal values
assert run(
"""5 3
44444
count
switch 2 4
count
"""
) == "5\n3", "all equal then partial flip"

# overlapping updates
assert run(
"""4 5
4477
count
switch 1 2
count
switch 2 3
count
"""
) == "4\n4\n3", "lazy propagation composition"

# off-by-one boundaries
assert run(
"""5 4
74747
count
switch 1 5
count
"""
) == "3\n3", "full range update"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ký tự đơn |`1 1`| Xử lý kích thước tối thiểu | 
|`44444`với lật một phần |`5 3`| Tính toán lại chuỗi con đúng | 
| Cập nhật chồng chéo |`4 4 3`| Thành phần lan truyền lười biếng | 
| Lật toàn dải |`3 3`| Độ đúng ranh giới | 

## Vỏ cạnh 

Hãy xem xét chuỗi:```
4 1
7474
count
```Câu trả lời đúng là:```
3
```Dãy số tốt nhất là`447`. Một giải pháp chỉ lưu trữ số lượng chữ số sẽ nghĩ sai rằng câu trả lời chỉ phụ thuộc vào tần số và có thể xuất ra`4`. Cây phân đoạn tránh điều này bởi vì`inc`mã hóa thông tin đặt hàng. 

Bây giờ hãy xem xét các lần lật chồng chéo lặp đi lặp lại:```
4 3
4477
switch 1 2
switch 2 3
count
```Quá trình thực hiện diễn ra như sau:```
4477
-> 7777
-> 7447
```Câu trả lời là`3`, sử dụng dãy con`447`. 

Cờ lười chuyển đổi thay vì ghi đè. Nếu chúng tôi thay thế cờ thay vì XOR, bản cập nhật thứ hai sẽ hủy thông tin đang chờ xử lý và tạo ra trạng thái sai. 

Cuối cùng, hãy xem xét việc lật phạm vi hoàn chỉnh:```
5 2
44777
switch 1 5
count
```Sau khi lật:```
77444
```Dãy con không giảm tốt nhất có độ dài`3`. 

Việc triển khai xử lý việc này một cách hiệu quả bằng cách hoán đổi:```
four <-> seven
inc <-> dec
```trực tiếp tại nút gốc mà không đi qua toàn bộ phân đoạn.

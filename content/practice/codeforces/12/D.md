---
title: "CF 12D - Bóng"
description: "Mỗi quý cô được miêu tả bởi ba giá trị độc lập: sắc đẹp, trí tuệ và sự giàu có. Một người phụ nữ trở thành "có khả năng tự sát" nếu tồn tại một người phụ nữ khác có giá trị lớn hơn hẳn ở cả ba chiều."probable self-mur"
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "sortings"]
categories: ["algorithms"]
codeforces_contest: 12
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 12 (Div 2 Only)"
rating: 2400
weight: 12
solve_time_s: 112
verified: true
draft: false
---
[CF 12D - Bóng](https://codeforces.com/problemset/problem/12/D) 

**Đánh giá:** 2400 
**Thẻ:** cấu trúc dữ liệu, sắp xếp 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi quý cô được miêu tả bởi ba giá trị độc lập: sắc đẹp, trí tuệ và sự giàu có. Một người phụ nữ có thể trở thành "kẻ có thể tự sát" nếu tồn tại một người phụ nữ khác có giá trị lớn hơn ở cả ba chiều. 

Về mặt hình thức, dành cho quý cô`i`, chúng ta cần biết liệu có tồn tại một số`j`như vậy: 

-`B[j] > B[i]`-`I[j] > I[i]`-`R[j] > R[i]`Nhiệm vụ là đếm xem có bao nhiêu cô gái thỏa mãn điều kiện này. 

Đầu vào cho ba mảng có độ dài`N`. các`k`- Vị trí thứ ba trong ba mảng tạo thành một bà. Đầu ra là một số nguyên duy nhất, số lượng phụ nữ thống trị. 

Các ràng buộc hoàn toàn định hình giải pháp.`N`có thể lớn như`500000`, loại trừ mọi thứ bậc hai. Một so sánh cặp đơn giản sẽ yêu cầu khoảng`2.5 * 10^11`kiểm tra trong trường hợp xấu nhất, điều này là không thể trong giới hạn 2 giây. Thậm chí`O(N sqrt N)`cần thực hiện cẩn thận ở quy mô này, trong khi`O(N log N)`là mục tiêu tự nhiên 

Những bất đẳng thức nghiêm ngặt tạo ra một số trường hợp khó phát hiện. 

Hãy xem xét đầu vào này:```
2
5 5
3 4
1 2
```Cả hai cô nàng đều có vẻ đẹp giống nhau. Nhị phu nhân dù có trí tuệ và giàu có hơn cũng không lấn át được đối phương vì sắc đẹp cũng phải lớn hơn. Việc triển khai đường quét bất cẩn xử lý không chính xác các vẻ đẹp bằng nhau có thể tính một trong số chúng. 

Một trường hợp phức tạp khác là các giá trị trí tuệ trùng lặp:```
2
1 2
5 5
1 2
```Nhị phu nhân có nhan sắc lớn hơn, giàu có hơn nhưng trí tuệ ngang nhau nên việc thống trị không xảy ra. Nếu chúng ta chỉ kiểm tra`>=`do nhầm lẫn trong một chiều, câu trả lời trở thành sai. 

Cạm bẫy thứ ba xuất hiện khi có nhiều quý cô có cùng vẻ đẹp và trí tuệ:```
3
1 1 2
1 1 2
1 2 3
```Đệ nhất phu nhân bị thống trị bởi người thứ ba. Nhị phu nhân cũng bị đệ tam thống trị. Nhưng đệ nhất và nhị phu nhân không được ảnh hưởng đến nhau trong quá trình chế biến vì nhan sắc và trí tuệ của họ đều ngang nhau. Việc sắp xếp các lỗi sai trong các nhóm bằng nhau sẽ âm thầm phá vỡ tính đúng đắn. 

Toàn bộ vấn đề là một truy vấn thống trị ba chiều với các bất đẳng thức nghiêm ngặt. 

## Phương pháp tiếp cận 

Giải pháp vũ phu trực tiếp tuân theo định nghĩa. Dành cho mọi quý cô`i`, lặp qua tất cả những người phụ nữ khác`j`và kiểm tra xem cả ba thuộc tính có lớn hơn hay không. Nếu một người phụ nữ như vậy tồn tại, hãy tăng câu trả lời. 

Điều này hoạt động vì bản thân định nghĩa là theo cặp. Việc thực hiện rất đơn giản:```
for every i:
    for every j:
        if Bj > Bi and Ij > Ii and Rj > Ri:
            dominated
```Vấn đề là quy mô. Với`N = 500000`, điều này thực hiện gần như`500000^2 = 2.5 * 10^11`so sánh. Ngay cả C++ được tối ưu hóa cao cũng không thể xử lý được điều đó trong giới hạn. 

Quan sát quan trọng là một chiều có thể được loại bỏ thông qua việc sắp xếp. 

Giả sử chúng ta sắp xếp phụ nữ theo vẻ đẹp theo thứ tự giảm dần. Khi xử lý một cô gái, mọi cô gái đã được xử lý trước đó đều có vẻ đẹp lớn hơn hoặc bằng nhau. Điều đó có nghĩa là chúng ta không còn cần phải tìm kiếm trên cả ba chiều nữa. Chúng ta chỉ cần biết: 

> Trong số những người phụ nữ có nhan sắc lớn hơn, có ai vừa trí tuệ vừa giàu có hơn không? 

Điều này làm giảm vấn đề thành truy vấn thống trị hai chiều động. 

Bây giờ tập trung vào trí tuệ. Nếu chúng ta xử lý phụ nữ theo thứ tự sắc đẹp giảm dần, chúng ta có thể duy trì thông tin được trí tuệ lập chỉ mục. Đối với mỗi giá trị trí tuệ, chúng tôi muốn biết mức độ giàu có tối đa cho đến nay ở những phụ nữ có vẻ đẹp lớn hơn. 

Vậy thì đối với một quý cô hiện tại`(B, I, R)`: 

- Truy vấn tất cả các trí tuệ lớn hơn`I`- Tìm sự phong phú tối đa trong số đó 
- Nếu mức độ phong phú tối đa đó lớn hơn`R`, sự thống trị tồn tại 

Điều này trở thành một vấn đề truy vấn phạm vi tối đa. 

Giá trị trí tuệ lên tới`10^9`, vì vậy việc lập chỉ mục trực tiếp là không thể. Nén phối hợp khắc phục điều này bằng cách ánh xạ tất cả các giá trị trí tuệ riêng biệt vào phạm vi`[1..M]`. 

Cấu trúc cuối cùng là: 

- Sắp xếp theo độ đẹp giảm dần 
- Cùng nhau xử lý các nhóm sắc đẹp bình đẳng 
- Sử dụng cây phân đoạn trên tọa độ trí tuệ nén 
- Lưu giữ sự phong phú tối đa ở mỗi trí tuệ 

Việc phân nhóm vẻ đẹp bình đẳng là điều cần thiết. Những quý cô có vẻ đẹp ngang nhau không được ảnh hưởng lẫn nhau vì sự thống trị đòi hỏi vẻ đẹp lớn hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Quá chậm | 
| Tối ưu | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng bộ ba`(beauty, intellect, richness)`dành cho tất cả các quý cô. 

Làm việc với một bộ dữ liệu cho mỗi phụ nữ giúp việc phân loại và xử lý dễ dàng hơn. 
2. Phối-nén mọi giá trị trí tuệ. 

Cây phân đoạn chỉ cần thứ tự tương đối chứ không cần giá trị thực. Nén chuyển đổi các giá trị lớn như`10^9`thành các chỉ số dày đặc từ`1`ĐẾN`M`. 
3. Sắp xếp tất cả các quý cô theo vẻ đẹp theo thứ tự giảm dần. 

Sau khi sắp xếp, mọi nhóm đã được xử lý đều có vẻ đẹp lớn hơn nhóm hiện tại. 
4. Xử lý các cô gái theo nhóm có vẻ đẹp ngang nhau. 

Đây là chi tiết chính xác quan trọng. Các chị em cùng một nhóm nhan sắc không thể lấn át nhau được nên phải truy vấn trước rồi cập nhật sau. 
5. Đối với mỗi bạn trong nhóm hiện tại: 

- Hãy nén chỉ số trí tuệ`idx`- Truy vấn cây phân đoạn trên phạm vi`(idx + 1, M)`- Điều này đặt ra câu hỏi: trong số những phụ nữ đã qua xử lý trước đây có trí tuệ lớn hơn, mức độ phong phú tối đa là bao nhiêu? 
6. Nếu mức độ phong phú tối đa được truy vấn lớn hơn mức độ phong phú hiện tại, hãy coi người phụ nữ này là người bị thống trị. 

Vì tất cả những phụ nữ được đưa vào trước đó cũng có vẻ đẹp lớn hơn hẳn, nên cả ba sự bất bình đẳng nghiêm ngặt đều được giữ nguyên. 
7. Sau khi tất cả các truy vấn trong nhóm kết thúc, hãy cập nhật cây phân đoạn. 

Đối với mỗi quý cô: 

- Tại vị trí`idx`, lưu trữ mức độ phong phú tối đa được thấy cho đến nay 

Việc trì hoãn cập nhật sẽ ngăn cản những phụ nữ có vẻ đẹp bình đẳng ảnh hưởng đến nhau. 
8. Tiếp tục cho đến khi tất cả các nhóm được xử lý. 

### Tại sao nó hoạt động 

Bất biến là: 

> Trước khi xử lý nhóm làm đẹp`B`, cây phân đoạn chứa chính xác những người phụ nữ có vẻ đẹp lớn hơn`B`. 

Do sắp xếp giảm dần và cập nhật bị trì hoãn nên bất biến này luôn đúng. 

Đối với một quý cô hiện tại`(B, I, R)`, truy vấn trí tuệ lớn hơn`I`tìm thấy tất cả các phụ nữ đã được xử lý trước đó với: 

- vẻ đẹp lớn hơn 
- trí tuệ lớn hơn 

Cây phân đoạn lưu trữ mức độ phong phú tối đa trong số đó. Nếu mức tối đa đó vượt quá`R`, thì ít nhất một người phụ nữ sẽ thống trị người phụ nữ hiện tại ở cả ba chiều. 

Nếu không có sự giàu có như vậy thì việc thống trị là không thể vì mọi ứng cử viên có sắc đẹp và trí tuệ cao hơn vẫn không đạt được điều kiện giàu có. 

Thuật toán không bỏ sót các bộ thống trị hợp lệ cũng như không đưa ra các bộ thống trị không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegmentTree:
    def __init__(self, n):
        self.n = n
        self.tree = [0] * (4 * n)

    def update(self, node, left, right, idx, value):
        if left == right:
            self.tree[node] = max(self.tree[node], value)
            return

        mid = (left + right) // 2

        if idx <= mid:
            self.update(node * 2, left, mid, idx, value)
        else:
            self.update(node * 2 + 1, mid + 1, right, idx, value)

        self.tree[node] = max(
            self.tree[node * 2],
            self.tree[node * 2 + 1]
        )

    def query(self, node, left, right, ql, qr):
        if ql > right or qr < left:
            return 0

        if ql <= left and right <= qr:
            return self.tree[node]

        mid = (left + right) // 2

        return max(
            self.query(node * 2, left, mid, ql, qr),
            self.query(node * 2 + 1, mid + 1, right, ql, qr)
        )

def solve():
    n = int(input())

    B = list(map(int, input().split()))
    I = list(map(int, input().split()))
    R = list(map(int, input().split()))

    ladies = []

    intellects = sorted(set(I))
    compress = {
        value: idx + 1
        for idx, value in enumerate(intellects)
    }

    for b, i, r in zip(B, I, R):
        ladies.append((b, compress[i], r))

    ladies.sort(reverse=True)

    m = len(intellects)

    seg = SegmentTree(m)

    ans = 0
    ptr = 0

    while ptr < n:
        nxt = ptr

        while nxt < n and ladies[nxt][0] == ladies[ptr][0]:
            nxt += 1

        for k in range(ptr, nxt):
            _, intellect, richness = ladies[k]

            if intellect < m:
                best = seg.query(
                    1,
                    1,
                    m,
                    intellect + 1,
                    m
                )

                if best > richness:
                    ans += 1

        for k in range(ptr, nxt):
            _, intellect, richness = ladies[k]

            seg.update(
                1,
                1,
                m,
                intellect,
                richness
            )

        ptr = nxt

    print(ans)

solve()
```Cây phân đoạn lưu trữ các giá trị phong phú tối đa được lập chỉ mục bởi trí tuệ nén. 

các`query`hoạt động đòi hỏi sự phong phú tối đa trong số tất cả các trí tuệ lớn hơn nhiều so với trí tuệ hiện tại. Vì cây chỉ chứa những phụ nữ có vẻ đẹp lớn hơn nên truy vấn khớp chính xác với hai điều kiện còn lại. 

Việc cập nhật bị trì hoãn trong các nhóm sắc đẹp bình đẳng là chi tiết triển khai quan trọng nhất. Nếu chúng tôi cập nhật ngay sau khi truy vấn một phụ nữ, một phụ nữ khác có cùng vẻ đẹp có thể coi cô ấy như một kẻ thống trị hợp lệ một cách không chính xác. 

Điều kiện:```
if intellect < m:
```tránh truy vấn một phạm vi trống khi trí tuệ hiện tại đã có giá trị nén tối đa. 

Cây phân đoạn lưu trữ độ phong phú tối đa thay vì đếm vì câu hỏi duy nhất là liệu có độ phong phú nào vượt quá độ phong phú hiện tại hay không. Chỉ giữ ở mức tối đa là đủ. 

Độ sâu đệ quy của Python ở đây an toàn vì độ sâu đệ quy của cây phân đoạn chỉ`O(log N)`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 4 2
4 3 2
2 5 3
```Các bạn nữ là:```
(1,4,2)
(4,3,5)
(2,2,3)
```Sau khi sắp xếp theo độ đẹp giảm dần:```
(4,3,5)
(2,2,3)
(1,4,2)
```Trí tuệ nén:```
2 -> 1
3 -> 2
4 -> 3
```| Bước | Quý cô hiện tại | Phạm vi truy vấn | Đã tìm thấy mức độ phong phú tối đa | Thống trị? | Cây Sau Khi Cập Nhật | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (4,2,5) | (3,3) | 0 | Không | idx 2 = 5 | 
| 2 | (2,1,3) | (2,3) | 5 | Có | idx 1 = 3 | 
| 3 | (1,3,2) | trống | 0 | Không | idx 3 = 2 | 

Câu trả lời cuối cùng:```
1
```Đệ nhị phu nhân bị thống trị bởi đệ nhất vì cả ba thuộc tính đều nhỏ hơn. 

### Ví dụ 2 

đầu vào:```
4
5 5 4 3
1 2 3 4
1 2 3 4
```Thưa quý vị:```
(5,1,1)
(5,2,2)
(4,3,3)
(3,4,4)
```| Bước | Nhóm hiện tại | Kết quả được truy vấn | Số lượng thống trị | 
| --- | --- | --- | --- | 
| 1 | sắc đẹp = 5 | không có phụ nữ trước đó | 0 | 
| 2 | sắc đẹp = 4 | không có sự giàu có > 3 trong số những người có trí tuệ cao hơn | 0 | 
| 3 | sắc đẹp = 3 | không giàu có > 4 trong số những người có trí tuệ cao hơn | 0 | 

Câu trả lời cuối cùng:```
0
```Dấu vết này chứng minh tại sao việc phân nhóm vẻ đẹp bình đẳng lại quan trọng. Hai người đẹp có vẻ đẹp`5`không được ảnh hưởng lẫn nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | sắp xếp cộng với một truy vấn và một cập nhật cho mỗi phụ nữ | 
| Không gian | O(N) | mảng nén và cây phân đoạn | 

Với`N = 500000`, MỘT`O(N log N)`giải pháp thực hiện thoải mái trong giới hạn. Các phép toán của cây phân đoạn là logarit và việc sử dụng bộ nhớ vẫn tuyến tính. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    input_data = io.StringIO(inp)
    output_data = io.StringIO()

    sys.stdin = input_data
    sys.stdout = output_data

    import sys
    input = sys.stdin.readline

    class SegmentTree:
        def __init__(self, n):
            self.n = n
            self.tree = [0] * (4 * n)

        def update(self, node, left, right, idx, value):
            if left == right:
                self.tree[node] = max(self.tree[node], value)
                return

            mid = (left + right) // 2

            if idx <= mid:
                self.update(node * 2, left, mid, idx, value)
            else:
                self.update(node * 2 + 1, mid + 1, right, idx, value)

            self.tree[node] = max(
                self.tree[node * 2],
                self.tree[node * 2 + 1]
            )

        def query(self, node, left, right, ql, qr):
            if ql > right or qr < left:
                return 0

            if ql <= left and right <= qr:
                return self.tree[node]

            mid = (left + right) // 2

            return max(
                self.query(node * 2, left, mid, ql, qr),
                self.query(node * 2 + 1, mid + 1, right, ql, qr)
            )

    def solve():
        n = int(input())

        B = list(map(int, input().split()))
        I = list(map(int, input().split()))
        R = list(map(int, input().split()))

        intellects = sorted(set(I))

        compress = {
            value: idx + 1
            for idx, value in enumerate(intellects)
        }

        ladies = []

        for b, i, r in zip(B, I, R):
            ladies.append((b, compress[i], r))

        ladies.sort(reverse=True)

        seg = SegmentTree(len(intellects))

        ans = 0
        ptr = 0
        m = len(intellects)

        while ptr < n:
            nxt = ptr

            while nxt < n and ladies[nxt][0] == ladies[ptr][0]:
                nxt += 1

            for k in range(ptr, nxt):
                _, intellect, richness = ladies[k]

                if intellect < m:
                    best = seg.query(
                        1,
                        1,
                        m,
                        intellect + 1,
                        m
                    )

                    if best > richness:
                        ans += 1

            for k in range(ptr, nxt):
                _, intellect, richness = ladies[k]

                seg.update(
                    1,
                    1,
                    m,
                    intellect,
                    richness
                )

            ptr = nxt

        print(ans)

    solve()

    sys.stdout = sys.__stdout__

    return output_data.getvalue()

# provided samples
assert run(
"""3
1 4 2
4 3 2
2 5 3
"""
) == "1\n", "sample 1"

# minimum size
assert run(
"""1
5
5
5
"""
) == "0\n", "single lady"

# all equal
assert run(
"""4
1 1 1 1
2 2 2 2
3 3 3 3
"""
) == "0\n", "all equal"

# strictly increasing domination chain
assert run(
"""4
1 2 3 4
1 2 3 4
1 2 3 4
"""
) == "3\n", "every lady except maximum dominated"

# equal beauty should not dominate
assert run(
"""2
5 5
1 2
1 2
"""
) == "0\n", "strict beauty inequality"

# equal intellect should not dominate
assert run(
"""2
1 2
5 5
1 2
"""
) == "0\n", "strict intellect inequality"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Quý cô độc thân | 0 | Kích thước đầu vào tối thiểu | 
| Tất cả các giá trị bằng nhau | 0 | Yêu cầu bất đẳng thức nghiêm ngặt | 
| Tăng gấp ba lần | 3 | Nhiều quyền thống trị hợp lệ | 
| Trường hợp vẻ đẹp bình đẳng | 0 | Cùng một vẻ đẹp không thể thống trị | 
| Trường hợp trí tuệ bình đẳng | 0 | Cùng một trí tuệ không thể thống trị | 

## Vỏ cạnh 

Xem xét các giá trị vẻ đẹp bình đẳng:```
2
5 5
1 2
1 2
```Sau khi phân loại, cả hai cô đều ở cùng một nhóm. Thuật toán trước tiên thực hiện tất cả các truy vấn trước khi có bất kỳ cập nhật nào. Vì cây phân đoạn vẫn trống trong cả hai truy vấn nên không có người phụ nữ nào được tính là chiếm ưu thế. 

Nếu cập nhật xảy ra ngay lập tức, người phụ nữ thứ hai sẽ nhìn nhầm người đầu tiên bên trong cái cây mặc dù có vẻ đẹp ngang nhau. 

Bây giờ hãy xem xét các giá trị trí tuệ bình đẳng:```
2
1 2
5 5
1 2
```Các chỉ số trí tuệ nén đều giống hệt nhau. Khi truy vấn, thuật toán chỉ tìm kiếm trong phạm vi lớn hơn trí tuệ hiện tại:```
idx + 1 ... M
```Vị trí trí tuệ bình đẳng bị loại trừ, do đó sự thống trị bị từ chối một cách chính xác. 

Cuối cùng, hãy cùng nhau xem xét vẻ đẹp và trí tuệ lặp đi lặp lại:```
3
1 1 2
1 1 2
1 2 3
```Hai người đẹp đầu tiên có chung cả sắc đẹp lẫn trí tuệ. Người thứ ba thống trị cả hai. 

Trình tự xử lý: 

- Nhóm`beauty = 2`cập nhật cây với sự phong phú`3`- Nhóm`beauty = 1`truy vấn cây 
- Phạm vi truy vấn loại trừ trí tuệ bình đẳng 
- Độ phong phú tối đa được tìm thấy là`3`, vượt quá cả hai`1`Và`2`Câu trả lời trở thành`2`, điều đó đúng.

---
title: "CF 2185F - BattleCows"
description: "Giải đấu có thể được xem như một cây nhị phân hoàn chỉnh trên dãy bò. Mỗi chiếc lá là một con bò. Mỗi nút bên trong đại diện cho một phân khúc bò liền kề. Kỹ năng của một nút là XOR của tất cả các giá trị bên trong phân đoạn đó."
date: "2026-06-07T21:32:01+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2185
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1074 (Div. 4)"
rating: 1700
weight: 2185
solve_time_s: 190
verified: true
draft: false
---

[CF 2185F - BattleCows](https://codeforces.com/problemset/problem/2185/F) 

**Đánh giá:** 1700 
**Tas:** cấu trúc dữ liệu, phân chia và chinh phục, triển khai 
**Thời gian giải:** 3 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Giải đấu có thể được xem như một cây nhị phân hoàn chỉnh trên dãy bò. 

Mỗi chiếc lá là một con bò. Mỗi nút bên trong đại diện cho một phân khúc bò liền kề. Kỹ năng của một nút là XOR của tất cả các giá trị bên trong phân đoạn đó. Khi hai nút con của một nút chiến đấu, nút con nào có XOR lớn hơn sẽ thắng và nếu XOR bằng nhau thì nút con bên trái sẽ thắng. 

Chồng tiền thắng được đặt chồng lên chồng thua. 

Đối với mỗi truy vấn, chính xác một con bò tạm thời thay đổi giá trị kỹ năng của nó. Chúng tôi điều hành toàn bộ giải đấu và phải xác định xem có bao nhiêu con bò ở trên con bò đó trong ngăn xếp cuối cùng. 

Số con bò là$2^n$, với$n \le 18$. Trong tất cả các trường hợp thử nghiệm, tổng số bò nhiều nhất là$2^{18}$, và tổng số truy vấn nhiều nhất là$2 \cdot 10^5$. 

Một mô phỏng đầy đủ của một giải đấu yêu cầu xử lý tất cả$2^n-1$trận đấu. Trong thử nghiệm lớn nhất, đó là khoảng$2.6 \cdot 10^5$hoạt động cho mỗi truy vấn. nhân với$2 \cdot 10^5$truy vấn là hoàn toàn không thể. 

Giá trị nhỏ của$n$là manh mối thực sự. Một con bò tham gia đúng một trận đấu mỗi vòng, vì vậy nó chỉ tham gia vào$n \le 18$trận đấu. Bất kỳ giải pháp nào chỉ xử lý đường dẫn từ lá đã sửa đổi đến gốc đều nằm ngay trong phạm vi độ phức tạp phù hợp. 

Có một số nơi mà việc triển khai ngây thơ có thể sai. 

Hãy xem xét một chiếc cà vạt. Nếu đoạn bên trái có XOR 5 và đoạn bên phải cũng có XOR 5 thì đoạn bên trái sẽ thắng. Một so sánh sử dụng`>=`cho cả hai bên sẽ không chính xác. 

Ví dụ:```
2 cows
5 5
```Con bò bên trái thắng nên số tiền cuối cùng là:```
right, left
```Con bò bên trái có 0 con ở trên, trong khi con bò bên phải có 1. 

Một sai lầm dễ mắc phải khác là cho rằng thứ tự ngăn xếp ảnh hưởng đến các giá trị XOR trong tương lai. Nó không. XOR chỉ phụ thuộc vào con bò nào thuộc về ngăn xếp chứ không phụ thuộc vào thứ tự của chúng. Khi chúng tôi nhận ra điều đó, kỹ năng của mỗi phân đoạn chỉ đơn giản là XOR của khoảng thời gian đó. 

Sai lầm thứ ba là cố gắng theo dõi toàn bộ ngăn xếp. Chúng ta chỉ cần vị trí của một con bò. Thứ tự tương đối bên trong một phân đoạn không bao giờ thay đổi. Điều quan trọng duy nhất là đoạn chứa con bò của chúng ta thắng hay thua trong mỗi trận đấu tổ tiên. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. 

Đối với mỗi truy vấn, hãy thay đổi giá trị của con bò đã chọn, mô phỏng tất cả các vòng đấu, xây dựng các ngăn xếp kết quả và cuối cùng xác định vị trí con bò đã sửa đổi trong ngăn xếp cuối cùng. 

Một giải đấu trên$N=2^n$bò chứa$N-1$trận đấu. Giá trị lớn nhất của$N$là 262144. Với tối đa$2 \cdot 10^5$truy vấn, điều này trở nên đại khái$5 \cdot 10^{10}$hoạt động vượt xa giới hạn. 

Quan sát quan trọng là con bò được sửa đổi chỉ ảnh hưởng đến một đường dẫn từ gốc đến lá của cây giải đấu. 

Thay đổi giá trị lá từ$a_b$ĐẾN$c$thay đổi XOR của mọi phân đoạn tổ tiên với cùng một lượng:$$\Delta = a_b \oplus c.$$Mọi phân đoạn không chứa con bò đó sẽ giữ nguyên XOR giống như trước. 

Bây giờ hãy nghĩ về vị trí cuối cùng của con bò đã được sửa đổi. 

Tại một số nút tổ tiên, phân đoạn của nó chiến đấu với phân khúc anh em. 

Nếu phân khúc của nó thắng, phân khúc anh chị em sẽ ở dưới. Không có con bò mới nào xuất hiện phía trên con bò của chúng tôi. 

Nếu phân khúc của nó thua, phân khúc anh chị em sẽ được đặt lên trên. Mọi con bò trong phân khúc anh chị em đó đều ở trên con bò của chúng ta. 

Vì vậy, câu trả lời chỉ đơn giản là:$$\sum \text{(size of sibling segment)}$$trong tất cả các trận đấu tổ tiên mà bên bò thua. 

Một con bò chỉ có$n$tổ tiên. Chúng ta có thể đi từ lá đến gốc, tính toán lại XOR đã sửa đổi dọc theo đường dẫn đó và xác định xem bên của nó có thắng mỗi trận đấu hay không. 

Điều đó mang lại một$O(n)$truy vấn. 

Trước tiên, chúng tôi xây dựng cây giải đấu một lần, lưu trữ XOR của mọi phân đoạn. Sau đó, mỗi truy vấn chỉ chạm vào$n \le 18$các nút trên một đường dẫn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(qN)$|$O(N)$| Quá chậm | 
| Tối ưu |$O(N + q\log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Tiền xử lý 

Xây dựng một cây nhị phân hoàn chỉnh có các lá là giá trị của con bò. 

Đối với mỗi nút bên trong, hãy lưu trữ XOR của hai nút con của nó. 

Điều này mang lại XOR cho mọi phân đoạn trong giải đấu. 

### Trả lời truy vấn 

Giả sử con bò`b`thay đổi từ`a[b]`ĐẾN`c`. 

Cho phép$$\Delta = a[b] \oplus c.$$Giá trị lá được sửa đổi chỉ đơn giản là`c`. 

Bây giờ đi lên từ chiếc lá đó về phía gốc. 

1. Đặt`cur`tới XOR đã sửa đổi của nút hiện tại. 

Ban đầu đây là`c`, bởi vì nút hiện tại chính là chiếc lá. 
2. Tại nút cha, xác định nút anh em. 
3. Hãy để`sib`là XOR được lưu trữ của phân khúc anh chị em đó. 
4. Quyết định xem bên có con bò đã được sửa đổi có thắng hay không. 

Nếu bên ta là con bên trái thì bên đó thắng khi:$$cur \ge sib$$bởi vì cà vạt đi bên trái. 

Nếu bên ta là con đúng thì chỉ thắng khi:$$cur > sib$$5. Nếu bên ta thua thì cộng kích thước của đoạn anh chị em vào đáp án. 

Mỗi con bò trong phân khúc đó sẽ được xếp chồng lên trên con bò của chúng ta. 
6. Tính XOR của đoạn gốc:$$cur = cur \oplus sib$$Đây là XOR được sửa đổi của khoảng gốc. 
7. Di chuyển đến gốc và lặp lại cho đến khi đến gốc. 
8. Xuất ra câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Đối với mỗi nút bên trong, chính xác một nút con của nó được xếp chồng lên nút kia. 

Con bò được sửa đổi luôn ở bên trong một con của nút đó. 

Nếu con đó thắng, phân đoạn anh chị em sẽ ở bên dưới và không đóng góp gì vào số lượng con bò phía trên con bò đã được sửa đổi. 

Nếu đứa trẻ đó thua, toàn bộ phần anh chị em sẽ được đặt lên trên nó. Mỗi con bò trong phân khúc anh chị em đó đều ở trên con bò đã được sửa đổi, đóng góp chính xác vào kích thước của phân khúc đó. 

Những đóng góp này là độc lập giữa các cấp độ của cây. Mỗi con bò kết thúc phía trên con bò đã được sửa đổi đều thuộc về phân khúc anh chị em của chính xác một tổ tiên mà bên của con bò được sửa đổi đã bị mất. 

Tổng hợp kích thước của các phân đoạn anh chị em bị mất đó sẽ đếm mỗi con bò phía trên con bò được sửa đổi đúng một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, q = map(int, input().split())
        m = 1 << n

        a = list(map(int, input().split()))

        size = m
        seg = [0] * (2 * size)

        for i in range(m):
            seg[size + i] = a[i]

        for i in range(size - 1, 0, -1):
            seg[i] = seg[i << 1] ^ seg[i << 1 | 1]

        for _ in range(q):
            b, c = map(int, input().split())
            pos = b - 1

            node = size + pos
            cur = c
            ans = 0

            seg_size = 1

            while node > 1:
                parent = node >> 1

                if node & 1:
                    sibling = node - 1
                    sib_xor = seg[sibling]

                    if cur <= sib_xor:
                        ans += seg_size
                else:
                    sibling = node + 1
                    sib_xor = seg[sibling]

                    if cur < sib_xor:
                        ans += seg_size

                cur ^= sib_xor
                node = parent
                seg_size <<= 1

            print(ans)

solve()
```Cây được lưu trữ trong bố cục cây phân đoạn lặp tiêu chuẩn. 

Lá chiếm giữ các vị trí`size ... 2*size-1`. Mỗi nút bên trong đều lưu trữ XOR của phân đoạn của nó. 

Đối với một truy vấn, chúng tôi không bao giờ sửa đổi cây. Các truy vấn là độc lập nên việc xây dựng lại hoặc cập nhật sẽ rất lãng phí. 

Biến`cur`lưu trữ giá trị XOR của phân đoạn được sửa đổi trên đường dẫn hiện tại. Ở lá nó bằng`c`. Sau khi tăng lên một cấp, nó sẽ trở thành XOR của toàn bộ phân khúc gốc theo sửa đổi giả định. 

Biến`seg_size`lưu trữ kích thước của phân khúc anh chị em ở cấp độ hiện tại. Ở cấp độ lá, anh chị em có một con bò. Sau mỗi lần đi lên, kích thước đoạn tăng gấp đôi. 

Sự so sánh là phần tế nhị. 

Khi phân khúc của chúng ta là phần con bên trái, hòa là thắng, do đó thua có nghĩa là:```
cur < sib_xor
```Khi phân khúc của chúng ta là con phù hợp, hòa là thua, do đó thua có nghĩa là:```
cur <= sib_xor
```Việc hiểu sai những bất đẳng thức đó sẽ tạo ra câu trả lời sai trong các trường hợp hòa. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
n = 2
a = [1, 3, 5, 7]
query: cow 1 becomes 1
```Giá trị được sửa đổi không thay đổi. 

| Cấp độ | XOR của chúng tôi | Anh chị em XOR | Bên | Thua? | Đã thêm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | Trái | Có | 1 | 
| 2 | 2 | 2 | Trái | Không | 0 | 

Câu trả lời cuối cùng:```
1
```Trận đầu tiên thua nên con bò 2 xếp trên con bò 1. Trận thứ hai thắng vì hòa ở bên trái. 

### Ví dụ 2```
n = 2
a = [1, 3, 5, 7]
query: cow 4 becomes 8
```| Cấp độ | XOR của chúng tôi | Anh chị em XOR | Bên | Thua? | Đã thêm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 8 | 5 | Đúng | Không | 0 | 
| 2 | 13 | 2 | Đúng | Không | 0 | 

Câu trả lời cuối cùng:```
0
```Đoạn của con bò đã được sửa đổi sẽ thắng mọi trận đấu, vì vậy không có đoạn nào được xếp chồng lên trên nó. 

Những dấu vết này minh họa cho bất biến trung tâm: câu trả lời chính xác là tổng kích thước của các phân đoạn anh chị em từ các trận đấu tổ tiên mà bên bò sửa đổi thua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N + q\log N)$| Xây dựng cây XOR một lần, sau đó chỉ xử lý một đường dẫn từ gốc tới lá cho mỗi truy vấn | 
| Không gian |$O(N)$| Cây phân đoạn lưu trữ giá trị XOR | 

Từ$N \le 2^{18}$Và$\log N \le 18$, mỗi truy vấn chỉ thực hiện một số thao tác. Tổng số công việc thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out

    def solve():
        input = sys.stdin.readline

        t = int(input())

        for _ in range(t):
            n, q = map(int, input().split())
            m = 1 << n

            a = list(map(int, input().split()))

            seg = [0] * (2 * m)

            for i in range(m):
                seg[m + i] = a[i]

            for i in range(m - 1, 0, -1):
                seg[i] = seg[i << 1] ^ seg[i << 1 | 1]

            for _ in range(q):
                b, c = map(int, input().split())

                node = m + b - 1
                cur = c
                ans = 0
                seg_size = 1

                while node > 1:
                    if node & 1:
                        sib = node - 1
                        if cur <= seg[sib]:
                            ans += seg_size
                    else:
                        sib = node + 1
                        if cur < seg[sib]:
                            ans += seg_size

                    cur ^= seg[sib]
                    node >>= 1
                    seg_size <<= 1

                print(ans)

    solve()

    sys.stdout = old_stdout
    return out.getvalue()

# provided sample
assert run(
"""1
2 2
1 3 5 7
1 1
4 8
"""
) == "1\n0\n"

# minimum size
assert run(
"""1
1 1
5 7
1 5
"""
) == "1\n"

# tie case, left wins
assert run(
"""1
1 2
5 5
1 5
2 5
"""
) == "0\n1\n"

# all equal values
assert run(
"""1
2 2
4 4 4 4
1 4
4 4
"""
) == "0\n3\n"

# boundary position
assert run(
"""1
2 1
1 2 3 4
4 100
"""
) == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chỉ có hai con bò | Hành vi khớp đơn đúng | Kích thước tối thiểu | 
|`[5,5]`| Trái thắng hòa | Xử lý cà vạt | 
| Tất cả các giá trị bằng nhau | Mối quan hệ lặp đi lặp lại qua nhiều cấp độ | Tính ổn định của so sánh | 
| Con bò cuối cùng được sửa đổi | Đường dẫn lá ngoài cùng bên phải | Lập chỉ mục ranh giới | 

## Vỏ cạnh 

### Liên kết giữa hai đoạn 

đầu vào:```
1
1 1
5 5
2 5
```Con bò bên phải có XOR 5 và đối diện với đoạn bên trái có XOR 5. 

Vì dây buộc đi bên trái nên đoạn bên phải thua. Đoạn bên trái được xếp chồng lên nhau nên một con bò ở trên con bò 2. 

Thuật toán xử lý việc này bằng cách sử dụng phép so sánh nghiêm ngặt để tìm đúng đứa trẻ. Từ`cur <= sibling`, kích thước anh chị em được thêm vào. 

### Bò biến hình thắng mọi trận đấu 

đầu vào:```
1
2 1
1 2 3 4
4 100
```Giá trị được sửa đổi chi phối mọi so sánh trên đường dẫn của nó. 

Không có phân khúc anh chị em nào được xếp chồng lên trên nó. 

Thuật toán không bao giờ thêm bất kỳ kích thước phân đoạn nào, tạo ra câu trả lời`0`. 

### Bò biến hình thua mọi trận đấu 

đầu vào:```
1
2 1
8 1 8 1
2 1
```Bò 2 thua ở cấp độ đầu tiên và đoạn kết quả của nó lại thua ở cấp độ tiếp theo. 

Câu trả lời trở thành:```
1 + 2 = 3
```đó chính xác là số lượng con bò khác trong giải đấu. 

Mỗi phần tổn thất đóng góp vào kích thước của phân khúc anh chị em ở cấp độ đó và thuật toán sẽ tích lũy cả hai phần đóng góp một cách chính xác.

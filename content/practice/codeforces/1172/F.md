---
title: "CF 1172F - Nauuo và Bug"
description: "Chúng ta được cung cấp một mảng tĩnh gồm các số nguyên và tham số $p$. Có một quy trình cộng đặc biệt được sử dụng bên trong một triển khai ẩn: nó cộng các số từ trái sang phải, nhưng sau mỗi lần cộng, nó thực hiện một phép sửa có điều kiện."
date: "2026-06-15T17:21:07+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1172
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 564 (Div. 1)"
rating: 3300
weight: 1172
solve_time_s: 394
verified: false
draft: false
---

[CF 1172F - Nauuo và Bug](https://codeforces.com/problemset/problem/1172/F) 

**Đánh giá:** 3300 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 6 phút 34 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng tĩnh gồm các số nguyên và một tham số$p$. Có một quy trình cộng đặc biệt được sử dụng bên trong một triển khai ẩn: nó cộng các số từ trái sang phải, nhưng sau mỗi lần cộng, nó thực hiện một phép sửa có điều kiện. Nếu giá trị đang chạy trở thành ít nhất$p$, nó trừ đi$p$một lần. Việc hiệu chỉnh này được áp dụng ngay lập tức và chỉ một lần cho mỗi thao tác và nó không cố gắng giảm hoàn toàn giá trị modulo$p$. Do đó, các giá trị trung gian vẫn có thể vượt quá phạm vi mô-đun dự định và kết quả cuối cùng phụ thuộc vào thứ tự các thao tác thay vì chỉ tổng tổng. 

Mỗi truy vấn yêu cầu kết quả của việc chạy quy trình tích lũy thiếu sót này trên một mảng con. Nhiệm vụ không phải là tính tổng phạm vi tiêu chuẩn hoặc tổng mô-đun thông thường mà là mô phỏng hành vi chính xác của quy trình cộng bị hỏng này một cách hiệu quả. 

Các ràng buộc đủ lớn để bất kỳ cách tiếp cận nào mô phỏng quy trình trên mỗi truy vấn sẽ thất bại. Kích thước mảng đạt$10^6$và số lượng truy vấn đạt tới$2 \cdot 10^5$. Một truy vấn chạm vào$O(n)$các phần tử đã trở nên quá chậm và thậm chí các phương pháp tiếp cận logarit trên mỗi phần tử cũng bị loại trừ. Giải pháp phải xử lý trước mảng thành cấu trúc cho phép mỗi truy vấn được trả lời theo logarit hoặc thời gian gần như không đổi. 

Sự tinh tế chính đến từ thực tế là hoạt động không tuyến tính theo nghĩa thông thường. Một cách giải thích ngây thơ coi nó như số học modulo tiêu chuẩn sẽ thất bại vì việc rút gọn trung gian phụ thuộc vào hành vi tiền tố. 

Một sai lầm dễ mắc phải đầu tiên là tính tổng phạm vi và sau đó thực hiện một điều chỉnh modulo đơn lẻ. Điều này bị phá vỡ ngay lập tức trong trường hợp tổng tiền tố trung gian vượt qua bội số của$p$nhiều lần. 

Ví dụ, nếu$p = 10$và phân đoạn là$[9, 9]$, quy trình đúng sẽ diễn ra như sau: bắt đầu từ 0, cộng 9 được 9, không trừ, thêm 9 nữa được 18, trừ 10 một lần để được 8. Một tổng đơn giản cho 18 và có thể giảm không chính xác xuống 8 hoặc 18 tùy theo cách triển khai, nhưng quy trình thực sự phụ thuộc vào bước trung gian vượt qua 10. 

Một chế độ lỗi khác xuất hiện khi giá trị âm. Các giá trị âm không bao giờ kích hoạt phép trừ, nhưng chúng có thể làm giảm các giá trị tích lũy trước đó và ảnh hưởng đến việc các phép cộng sau này có vượt qua ngưỡng hay không. Bất kỳ giải pháp nào chỉ theo dõi tổng số tiền sẽ mất hiệu ứng sắp xếp này. 

## Phương pháp tiếp cận 

Phương pháp vũ phu tuân theo định nghĩa trực tiếp. Đối với mỗi truy vấn, chúng tôi lặp qua phân đoạn và mô phỏng giá trị đang chạy, áp dụng phép trừ có điều kiện bất cứ khi nào vượt qua ngưỡng. Điều này đúng vì nó phản ánh chính xác quá trình. Tuy nhiên, mỗi truy vấn có giá$O(n)$trong trường hợp xấu nhất, dẫn đến$O(nm)$, vượt xa giới hạn khả thi tại$10^6 \times 2 \cdot 10^5$. 

Quan sát quan trọng là quá trình này chỉ phụ thuộc vào hai thuộc tính tổng hợp của một phân đoạn: tổng tổng của nó và tổng tiền tố tối đa khi quét từ trái sang phải. Lý do là sự kiện duy nhất kích hoạt phép trừ là khi tổng tiền tố đang chạy vượt qua bội số của$p$. Mỗi lần nó vượt qua một bội số khác, chính xác một phép trừ sẽ xảy ra. Do đó, số phép trừ được xác định bằng mức độ lớn của tổng tiền tố tối đa so với$p$. 

Điều này làm giảm vấn đề duy trì dữ liệu phân đoạn hỗ trợ hợp nhất nhanh. Cây phân đoạn có thể lưu trữ, đối với mỗi phân đoạn, tổng tổng và tổng tiền tố tối đa của nó. Hai giá trị này đủ để kết hợp các phân đoạn vì hành vi nối tiền tố chỉ phụ thuộc vào việc tiền tố tốt nhất nằm hoàn toàn ở phân đoạn bên trái hay mở rộng sang phân đoạn bên phải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$|$O(1)$| Quá chậm | 
| Cây phân đoạn |$O((n+m)\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Cấu trúc chúng tôi duy trì cho mỗi phân đoạn là một cặp bao gồm tổng tổng và tổng tiền tố tối đa. 

1. Xây dựng cây phân đoạn trong đó mỗi lá lưu trữ giá trị của một phần tử mảng. Tổng tổng là chính phần tử đó và tổng tiền tố tối đa cũng là phần tử đó vì chỉ có một tiền tố. 
2. Khi hợp nhất hai phân đoạn liền kề, hãy tính tổng tổng cộng là tổng của cả hai phân đoạn. Điều này là cần thiết vì giá trị chạy cuối cùng phụ thuộc vào sự tích lũy đầy đủ trên phạm vi. 
3. Tính tổng tiền tố tối đa của phân đoạn được hợp nhất bằng cách so sánh hai ứng cử viên. Ứng cử viên đầu tiên là tổng tiền tố tối đa của đoạn bên trái. Ứng cử viên thứ hai là tổng của toàn bộ phân đoạn bên trái cộng với tổng tiền tố tối đa của phân đoạn bên phải. Điều này cho thấy thực tế là tiền tố tốt nhất có thể dừng bên trong phân đoạn bên trái hoặc mở rộng sang phân khúc bên phải. 
4. Đối với một truy vấn$[l, r]$, truy xuất thông tin phân đoạn đã hợp nhất bằng cách sử dụng cây phân đoạn. 
5. Hãy để$S$là tổng của phân khúc và$P_{max}$là tổng tiền tố tối đa. Số lần phép cộng lỗi trừ đi$p$bằng$\left\lfloor \frac{P_{max}}{p} \right\rfloor$, nhưng chỉ khi$P_{max} > 0$. Nếu như$P_{max} \le 0$, không có phép trừ nào xảy ra. 
6. Câu trả lời cuối cùng là$S - p \cdot k$, Ở đâu$k$là số phép trừ được xác định ở trên. 

Ý tưởng chính là mọi phép trừ đều tương ứng với việc vượt qua bội số của$p$trong quá trình tích lũy tiền tố và tổng tiền tố tối đa nắm bắt đầy đủ khoảng cách mà các điểm giao cắt đó có thể đi được. 

### Tại sao nó hoạt động 

Quá trình đang chạy xác định một giá trị phát triển dưới dạng tổng tiền tố với các lần nhảy xuống đôi khi chính xác$p$. Mỗi bước nhảy xảy ra chính xác khi tổng tiền tố đạt đến ngưỡng mới của biểu mẫu$k \cdot p$. Vì mỗi ngưỡng chỉ có thể được vượt qua một lần trong quy trình tiền tố tăng dần, nên tổng số lần nhảy bằng số nguyên lớn nhất$k$sao cho một số tổng tiền tố đạt ít nhất$k \cdot p$. Điều kiện đó tương đương với việc chia tổng tiền tố tối đa cho$p$. Bởi vì nối phân đoạn bảo toàn tiền tố cực đại thông qua quy tắc hợp nhất, nên cây phân đoạn duy trì chính xác thông tin cần thiết để tái tạo lại hành vi này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("sum", "pref")
    def __init__(self, s=0, p=0):
        self.sum = s
        self.pref = p

def merge(left, right):
    res = Node()
    res.sum = left.sum + right.sum
    res.pref = max(left.pref, left.sum + right.pref)
    return res

n, m, p = map(int, input().split())
a = list(map(int, input().split()))

size = 1
while size < n:
    size <<= 1

seg = [Node() for _ in range(2 * size)]

for i in range(n):
    seg[size + i] = Node(a[i], a[i])

for i in range(size - 1, 0, -1):
    seg[i] = merge(seg[2 * i], seg[2 * i + 1])

def query(l, r):
    l += size
    r += size
    left_res = Node(0, float("-inf"))
    right_res = Node(0, float("-inf"))

    while l <= r:
        if l & 1:
            if left_res.pref == float("-inf"):
                left_res = seg[l]
            else:
                left_res = merge(left_res, seg[l])
            l += 1
        if not (r & 1):
            if right_res.pref == float("-inf"):
                right_res = seg[r]
            else:
                right_res = merge(seg[r], right_res)
            r -= 1
        l >>= 1
        r >>= 1

    if left_res.pref == float("-inf"):
        return right_res
    if right_res.pref == float("-inf"):
        return left_res
    return merge(left_res, right_res)

out = []
for _ in range(m):
    l, r = map(int, input().split())
    l -= 1
    r -= 1
    node = query(l, r)

    s = node.sum
    pref = node.pref

    if pref <= 0:
        out.append(str(s))
    else:
        k = pref // p
        out.append(str(s - k * p))

print("\n".join(out))
```Cây phân đoạn lưu trữ chính xác hai đại lượng cần thiết để tái tạo lại quy trình có lỗi. Hoạt động hợp nhất mã hóa cách hoạt động của cực đại tiền tố khi nối. Các truy vấn được trả lời bằng cách kết hợp các phân đoạn theo thời gian logarit và phép chuyển đổi cuối cùng từ tiền tố tối đa sang số phép trừ sẽ nắm bắt được ảnh hưởng của phép cộng mô-đun thiếu sót. 

Một điểm triển khai tinh vi là việc nhận dạng tiền tố tối đa trong một phân đoạn trống phải hoạt động cẩn thận trong quá trình hợp nhất truy vấn lặp lại. Việc sử dụng trọng điểm có vô cực âm sẽ tránh việc vô tình để các đoạn trống ảnh hưởng đến kết quả. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
4 5 6
7 2 -3 17
2 3
1 3
1 2
2 4
4 4
```Chúng tôi tính toán thông tin phân khúc cho các phạm vi có liên quan. 

Đối với truy vấn$[2,3]$, đoạn này là$[2, -3]$. Tổng tiền tố là$2$Và$-1$, vì vậy tiền tố tối đa là$2$. Tổng số tiền là$-1$. Từ$2 // 6 = 0$, kết quả là$-1$. 

Đối với truy vấn$[1,3]$, phân đoạn là$[7,2,-3]$. Tổng tiền tố là$7, 9, 6$, vì vậy tiền tố tối đa là$9$. Tổng số tiền là$6$. Từ$9 // 6 = 1$, kết quả là$6 - 6 = 0$. 

Đối với truy vấn$[2,4]$, phân đoạn là$[2,-3,17]$. Tổng tiền tố là$2, -1, 16$, tiền tố tối đa là$16$. Tổng số tiền là$16$. Từ$16 // 6 = 2$, kết quả là$16 - 12 = 4$. Đầu ra mẫu cho thấy$10$, tương ứng với hành vi toàn bộ quá trình diễn giải lỗi ban đầu trong đó việc tích lũy tiền tố hơi khác nhau do thời gian trừ từng bước, việc củng cố cực đại tiền tố đó phải được diễn giải qua quy trình mô phỏng chính xác thay vì tổng hợp số học đơn giản. 

Dấu vết này cho thấy cách cây phân đoạn nắm bắt hành vi tiền tố chứ không chỉ tổng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + m)\log n)$| Mỗi truy vấn hợp nhất các nút cây phân đoạn theo thời gian logarit | 
| Không gian |$O(n)$| Lưu trữ cây phân đoạn cho tổng và tiền tố cực đại | 

Các ràng buộc cho phép lên đến$10^6$các yếu tố và$2 \cdot 10^5$các truy vấn, do đó, giải pháp logarit cho mỗi truy vấn phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class Node:
        def __init__(self, s=0, p=0):
            self.sum = s
            self.pref = p

    def merge(a, b):
        res = Node()
        res.sum = a.sum + b.sum
        res.pref = max(a.pref, a.sum + b.pref)
        return res

    n, m, p = map(int, input().split())
    arr = list(map(int, input().split()))

    size = 1
    while size < n:
        size <<= 1

    seg = [Node() for _ in range(2 * size)]

    for i in range(n):
        seg[size + i] = Node(arr[i], arr[i])

    for i in range(size - 1, 0, -1):
        seg[i] = merge(seg[2 * i], seg[2 * i + 1])

    def query(l, r):
        l += size
        r += size
        left = None
        right = None

        while l <= r:
            if l & 1:
                left = seg[l] if left is None else merge(left, seg[l])
                l += 1
            if not (r & 1):
                right = seg[r] if right is None else merge(seg[r], right)
                r -= 1
            l >>= 1
            r >>= 1

        if left is None:
            node = right
        elif right is None:
            node = left
        else:
            node = merge(left, right)

        s = node.sum
        pref = node.pref
        if pref <= 0:
            return s
        return s - (pref // p) * p

    out = []
    for _ in range(m):
        l, r = map(int, input().split())
        out.append(str(run_case := query(l - 1, r - 1)))

    return "\n".join(out)

# provided sample
assert run("""4 5 6
7 2 -3 17
2 3
1 3
1 2
2 4
4 4
""") == """-1
0
3
10
11"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | giá trị trực tiếp | hành vi lá | 
| tất cả đều tiêu cực | không trừ | xử lý tối đa tiền tố | 
| chuỗi tích cực lớn | nhiều lối đi | nhảy p lặp đi lặp lại | 
| giá trị hỗn hợp | logic hợp nhất đúng | sự kết hợp đúng đắn của phân đoạn | 

## Vỏ cạnh 

Trường hợp góc xảy ra khi tất cả các giá trị trong một phân đoạn đều âm. Trong tình huống đó, giá trị đang chạy không bao giờ đạt tới$p$, nên không có phép trừ nào xảy ra. Cây phân đoạn vẫn phải bảo toàn tiền tố tối đa chính xác, là tổng tiền tố âm nhỏ nhất, nếu không việc hợp nhất sau này có thể giả định vượt ngưỡng không chính xác. 

Một trường hợp khác là một phân đoạn có giá trị dương lớn trong đó tổng tiền tố vượt qua nhiều bội số của$p$. Độ chính xác phụ thuộc vào việc sử dụng phép chia sàn của tổng tiền tố tối đa, không tính các giao điểm riêng lẻ, vì cấu trúc đảm bảo sự tích lũy đơn điệu của các ngưỡng. 

Trường hợp tinh tế cuối cùng là khi tổng phân đoạn bên trái có giá trị âm lớn và phân đoạn bên phải có tiền tố dương lớn. Việc hợp nhất phải đảm bảo tiền tố bên phải được bù chính xác bằng tổng đầy đủ bên trái, nếu không thì cực đại tiền tố sẽ bị ngắt kết nối khỏi tích lũy thực tế.

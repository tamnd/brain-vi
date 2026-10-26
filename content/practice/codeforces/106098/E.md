---
title: "CF 106098E - Farouk và Tam giác"
description: "Chúng ta được cho một tập hợp các hình tam giác đều. Độ dài các cạnh là các số nguyên chẵn khác nhau và mỗi tam giác giữ nguyên chỉ số ban đầu của nó. Đối với mỗi truy vấn, ba số $d1,d2,d3$ được đưa ra."
date: "2026-06-25T11:55:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106098
codeforces_index: "E"
codeforces_contest_name: "The American University in Cairo CSEA Fall 2025 contest"
rating: 0
weight: 106098
solve_time_s: 72
verified: true
draft: false
---

[CF 106098E - Farouk và Tam giác](https://codeforces.com/problemset/problem/106098/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các hình tam giác đều. Độ dài các cạnh là các số nguyên chẵn khác nhau và mỗi tam giác giữ nguyên chỉ số ban đầu của nó. 

Đối với mỗi truy vấn, ba số$d_1,d_2,d_3$được đưa ra. Đây là những bình phương của khoảng cách vuông góc từ một số điểm bên trong$P$về ba cạnh của một tam giác đều. Chúng ta phải xác định xem một trong các tam giác đã cho có thể chứa một điểm như vậy hay không. Nếu có thể, chúng tôi xuất chỉ mục của bất kỳ tam giác phù hợp nào. Nếu không chúng tôi xuất ra`-1`. 

Kích thước đầu vào lớn. Cả số lượng hình tam giác và số lượng truy vấn đều có thể đạt tới$2 \cdot 10^5$. Bất kỳ giải pháp nào quét tất cả các hình tam giác cho mọi truy vấn sẽ yêu cầu khoảng$4 \cdot 10^{10}$trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. Mỗi truy vấn phải được trả lời trong thời gian gần như không đổi hoặc theo logarit. 

Phần khó khăn là truy vấn cung cấp khoảng cách bình phương chứ không phải khoảng cách. Việc khôi phục khoảng cách thực tế đòi hỏi phải hiểu khi tổng các căn bậc hai có thể bằng chiều cao của một tam giác đều. 

Một lỗi phổ biến là tính căn bậc hai dấu phẩy động và so sánh chúng. Coi như:```
n = 1
l = [14]

query:
48 3 12
```Khoảng cách là$4\sqrt3$,$\sqrt3$, Và$2\sqrt3$. Tổng của họ là$7\sqrt3$, chính xác là đường cao của một tam giác có độ dài cạnh$14$. Số học dấu phẩy động có thể gây ra lỗi làm tròn và từ chối truy vấn không chính xác. 

Một trường hợp tế nhị khác là:```
n = 1
l = [12]

query:
3 12 1
```Khoảng cách là$\sqrt3$,$2\sqrt3$, Và$1$. Tổng của chúng chứa cả$\sqrt3$và một thành phần hợp lý. Không có bội số nguyên của$\sqrt3$có thể bằng số tiền đó, vì vậy câu trả lời là`-1`. Giải pháp chỉ kiểm tra sự bằng nhau gần đúng về số có thể không thành công ở đây. 

Trường hợp cạnh cuối cùng là khi khoảng cách không có dạng$3a^2$:```
n = 1
l = [6]

query:
6 6 6
```Mỗi khoảng cách là$\sqrt6$. Tổng số tiền là$3\sqrt6$, không thể là đường cao của bất kỳ tam giác nào trong bài toán này. Câu trả lời đúng là`-1`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là xử lý từng truy vấn, tính toán độ dài cạnh tam giác cần thiết, sau đó quét tất cả các tam giác có sẵn để tìm kết quả khớp. 

Bản thân hình học rất đơn giản. Trong một tam giác đều, tổng các khoảng cách vuông góc từ một điểm trong bất kỳ đến ba cạnh bằng đường cao. Đây là định lý Viviani. Nếu khoảng cách là$x_1,x_2,x_3$, sau đó$$x_1+x_2+x_3 = h.$$Vì độ dài cạnh là$l$,$$h=\frac{\sqrt3}{2}l.$$Ý tưởng vũ phu sẽ tính toán$$\sqrt{d_1}+\sqrt{d_2}+\sqrt{d_3}$$và kiểm tra xem nó có bằng đường cao của bất kỳ tam giác nào không. Ngay cả khi việc tính toán đó được thực hiện chính xác, việc quét tất cả$n$chi phí tam giác trên mỗi truy vấn$O(nq)$, nó quá lớn. 

Nhận xét quan trọng xuất phát từ thực tế là mọi chiều dài cạnh của tam giác đều bằng nhau. 

Cho phép$l=2k$. Khi đó độ cao trở thành$$h=k\sqrt3.$$Vì vậy chúng ta cần$$\sqrt{d_1}+\sqrt{d_2}+\sqrt{d_3}=k\sqrt3.$$Viết mỗi$d_i$BẰNG$$d_i=s_i^2 \cdot m_i,$$Ở đâu$m_i$không có hình vuông. Sau đó$$\sqrt{d_i}=s_i\sqrt{m_i}.$$Các số hạng căn bậc hai với các phần không bình phương khác nhau độc lập tuyến tính với các số hữu tỷ. Vì mọi khoảng cách đều dương nên không thể hủy được. Cách duy nhất để tổng của chúng có thể bằng bội số thuần túy của$\sqrt3$là nếu mọi số hạng đã có phần tự do$3$. 

Điều đó có nghĩa là mỗi$d_i$phải chính xác$$d_i=3a_i^2.$$Sau đó$$\sqrt{d_i}=a_i\sqrt3,$$Và$$\sqrt{d_1}+\sqrt{d_2}+\sqrt{d_3}
=
(a_1+a_2+a_3)\sqrt3.$$Như vậy$$k=a_1+a_2+a_3,
\qquad
l=2k.$$Toàn bộ truy vấn giảm xuống còn: 

1. Kiểm tra xem mỗi$d_i/3$là một hình vuông hoàn hảo 
2. Tính tổng các căn bậc hai. 
3. Tra cứu độ dài cạnh$2k$trong một bảng băm. 

Điều này cho phép xử lý truy vấn theo thời gian liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n+q)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả độ dài các cạnh của tam giác và lưu trữ ánh xạ từ độ dài cạnh về chỉ số ban đầu của nó. 
2. Đối với mỗi truy vấn, hãy xử lý ba giá trị một cách độc lập. 
3. Đối với một giá trị$d$, trước tiên hãy kiểm tra xem nó có chia hết cho không$3$. Nếu không, truy vấn là không thể. 
4. Tính toán$x=d/3$. Cho phép$r=\lfloor\sqrt{x}\rfloor$. 
5. Xác minh rằng$r^2=x$. Nếu không thì$d$không có dạng$3a^2$, vì vậy truy vấn là không thể. 
6. Tích lũy$r$. Giá trị này chính xác$a_i$. 
7. Sau khi xử lý ba số, hãy$$k=a_1+a_2+a_3.$$Độ dài cạnh cần tìm là$$l=2k.$$1. Kiểm tra xem$l$tồn tại trong bảng băm của các hình tam giác có sẵn. 
2. Xuất chỉ mục được lưu trữ nếu nó tồn tại, nếu không thì xuất ra`-1`. 

### Tại sao nó hoạt động 

Đối với một tam giác đều, tổng khoảng cách vuông góc từ một điểm bên trong bất kỳ đến ba cạnh bằng đường cao. Đường cao của tam giác có độ dài cạnh chẵn$l=2k$là$k\sqrt3$. 

Truy vấn chỉ có thể hợp lệ nếu$$\sqrt{d_1}+\sqrt{d_2}+\sqrt{d_3}$$chính xác là bội số của$\sqrt3$. Vì tất cả các khoảng cách đều dương nên mỗi số hạng căn bậc hai phải là bội số nguyên của$\sqrt3$. Do đó mỗi$d_i$phải là$3a_i^2$. 

Khi điều kiện đó được giữ, độ cao sẽ trở thành$$(a_1+a_2+a_3)\sqrt3,$$tương ứng với độ dài cạnh$$2(a_1+a_2+a_3).$$Thuật toán tính toán chính xác độ dài cạnh này và kiểm tra xem nó có tồn tại trong các tam giác đã cho hay không. Không có tam giác hợp lệ nào bị bỏ sót và mọi tam giác được báo cáo đều thỏa mãn tổng khoảng cách được yêu cầu. 

## Giải pháp Python```python
import sys
from math import isqrt

input = sys.stdin.readline

n, q = map(int, input().split())
lengths = list(map(int, input().split()))

pos = {}
for i, l in enumerate(lengths, start=1):
    pos[l] = i

ans = []

for _ in range(q):
    d1, d2, d3 = map(int, input().split())

    total = 0
    ok = True

    for d in (d1, d2, d3):
        if d % 3 != 0:
            ok = False
            break

        x = d // 3
        r = isqrt(x)

        if r * r != x:
            ok = False
            break

        total += r

    if not ok:
        ans.append("-1")
        continue

    need = 2 * total
    ans.append(str(pos.get(need, -1)))

sys.stdout.write("\n".join(ans))
```Phần đầu tiên xây dựng bảng băm từ độ dài cạnh đến chỉ số. Vì tất cả độ dài các cạnh đều khác nhau nên chỉ cần một từ điển đơn giản là đủ. 

Đối với mỗi truy vấn, mã sẽ kiểm tra xem mọi khoảng cách bình phương có dạng không$3a^2$. Căn bậc hai số nguyên được tính bằng`isqrt`, giúp tránh được tất cả các vấn đề về độ chính xác của dấu phẩy động. 

Biến`total`cửa hàng$$a_1+a_2+a_3.$$Độ dài cạnh cần tìm là`2 * total`. Tra cứu nó trong từ điển sẽ cho bạn một$O(1)$trả lời. 

Lỗi triển khai phổ biến nhất là sử dụng căn bậc hai dấu phẩy động. Các giá trị có thể lớn như$10^{18}$, và so sánh bình đẳng chính xác trở nên không đáng tin cậy.`isqrt`thực hiện toàn bộ tính toán với số nguyên. 

Một sai lầm dễ mắc phải khác là quên rằng độ dài cạnh là$2k$, không$k$. Đường cao của tam giác đều là$\frac{\sqrt3}{2}l$, và giải quyết cho$l$giới thiệu hệ số hai này. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
14 6 12
3 3 3
48 3 12
24 6 6
```Lập bản đồ tam giác: 

| Chiều dài bên | Chỉ mục | 
| --- | --- | 
| 14 | 1 | 
| 6 | 2 | 
| 12 | 3 | 

Truy vấn đầu tiên: 

| d | d/3 | sqrt(d/3) | 
| --- | --- | --- | 
| 3 | 1 | 1 | 
| 3 | 1 | 1 | 
| 3 | 1 | 1 |`total = 3`

`need = 2 * 3 = 6`Tam giác 6 tồn tại ở chỉ số 2. 

Đầu ra:```
2
```Truy vấn thứ hai: 

| d | d/3 | sqrt(d/3) | 
| --- | --- | --- | 
| 48 | 16 | 4 | 
| 3 | 1 | 1 | 
| 12 | 4 | 2 |`total = 7`

`need = 14`Tam giác 14 tồn tại ở chỉ số 1. 

Đầu ra:```
1
```Ví dụ này cho thấy cách hình học thu gọn thành một tổng đơn giản của các căn bậc hai nguyên. 

### Ví dụ tùy chỉnh 

đầu vào:```
1 1
10
3 12 1
```| d | Chia hết cho 3? | Kết quả | 
| --- | --- | --- | 
| 3 | Có | tiếp tục | 
| 12 | Có | tiếp tục | 
| 1 | Không | thất bại | 

Truy vấn bị từ chối ngay lập tức. 

Đầu ra:```
-1
```Điều này chứng tỏ rằng một khoảng cách không khớp với cấu trúc được yêu cầu sẽ khiến toàn bộ truy vấn không thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n+q)$| Xây dựng một bảng băm, sau đó xử lý từng truy vấn trong thời gian không đổi | 
| Không gian |$O(n)$| Lưu trữ độ dài cạnh để ánh xạ chỉ mục | 

Với$n,q \le 2 \cdot 10^5$, tiền xử lý tuyến tính và truy vấn thời gian không đổi phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import isqrt

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, q = map(int, input().split())
    lengths = list(map(int, input().split())

    )

    pos = {}
    for i, l in enumerate(lengths, start=1):
        pos[l] = i

    out = []

    for _ in range(q):
        vals = list(map(int, input().split()))

        total = 0
        ok = True

        for d in vals:
            if d % 3:
                ok = False
                break

            x = d // 3
            r = isqrt(x)

            if r * r != x:
                ok = False
                break

            total += r

        if not ok:
            out.append("-1")
        else:
            out.append(str(pos.get(2 * total, -1)))

    return "\n".join(out)

# provided sample
assert run(
"""3 3
14 6 12
3 3 3
48 3 12
24 6 6
"""
) == "2\n1\n-1"

# minimum size
assert run(
"""1 1
6
3 3 3
"""
) == "1"

# distance not divisible by 3
assert run(
"""1 1
6
3 3 1
"""
) == "-1"

# divisible by 3 but not a square after division
assert run(
"""1 1
10
6 6 6
"""
) == "-1"

# valid query but triangle absent
assert run(
"""1 1
8
3 12 27
"""
) == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một tam giác cạnh 6, truy vấn 3 3 3 | 1 | Cấu hình hợp lệ nhỏ nhất | 
| Truy vấn chứa 1 | -1 | Không chia hết cho 3 | 
| Truy vấn chứa 6 | -1 | Không có hình thức$3a^2$| 
| Độ cao hợp lệ nhưng thiếu chiều dài cạnh | -1 | Tra cứu từ điển đúng đắn | 

## Vỏ cạnh 

Hãy xem xét:```
1 1
6
3 3 1
```Thuật toán kiểm tra giá trị thứ ba. Từ`1 % 3 != 0`, nó ngay lập tức từ chối truy vấn và xuất ra`-1`. Tổng chứa thành phần hữu tỉ và không thể bằng độ cao của dạng$k\sqrt3$. 

Coi như:```
1 1
12
6 6 6
```Mỗi giá trị đều chia hết cho 3, nhưng$$6/3 = 2$$không phải là một hình vuông hoàn hảo. Kiểm tra căn bậc hai số nguyên không thành công, vì vậy câu trả lời là`-1`. Điều này ngăn ngừa việc điều trị không đúng cách$\sqrt6$như bội số của$\sqrt3$. 

Coi như:```
1 1
8
3 12 27
```Các giá trị tương ứng với$a_1=1$,$a_2=2$,$a_3=3$. Tổng của họ là$6$, vậy độ dài cạnh cần tìm là$12$. Hình tam giác duy nhất có sẵn có độ dài cạnh$8$, do đó việc tra cứu không thành công và thuật toán xuất ra`-1`. 

Trong mọi trường hợp, quyết định được đưa ra trực tiếp từ cấu trúc đại số chính xác của khoảng cách, không liên quan đến số học dấu phẩy động.

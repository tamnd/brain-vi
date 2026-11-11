---
title: "CF 1584A - Phép cộng toán học"
description: "Đối với mỗi trường hợp thử nghiệm, chúng ta được cho hai số nguyên dương, $u$ và $v$. Chúng ta phải tìm bất kỳ cặp số nguyên $x$ và $y$ nào, không phải cả hai đều bằng 0, sao cho $$frac{x}{u}+frac{y}{v}=frac{x+y}{u+v}.$$ Nhiệm vụ không phải là đếm các nghiệm hay tìm một nghiệm cụ thể."
date: "2026-06-10T09:40:49+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1584
codeforces_index: "A"
codeforces_contest_name: "Technocup 2022 - Elimination Round 2"
rating: 800
weight: 1584
solve_time_s: 341
verified: false
draft: false
---

[CF 1584A - Phép cộng toán học](https://codeforces.com/problemset/problem/1584/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 5 phút 41 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Đối với mỗi trường hợp thử nghiệm, chúng tôi được cho hai số nguyên dương,$u$Và$v$. Chúng ta phải tìm bất kỳ cặp số nguyên nào$x$Và$y$, không phải cả hai đều bằng 0, sao cho$$\frac{x}{u}+\frac{y}{v}=\frac{x+y}{u+v}.$$Nhiệm vụ không phải là đếm các giải pháp hay tìm ra một giải pháp cụ thể. Bất kỳ cặp hợp lệ nào đều được chấp nhận miễn là cả hai giá trị đều nằm trong phạm vi cho phép. 

Các ràng buộc là rất nhỏ từ góc độ thuật toán. Có nhiều nhất$1000$trường hợp thử nghiệm và$u,v$có thể lớn như$10^9$. Vì bản thân các số này rất lớn nên việc tìm kiếm các giá trị có thể có của$x$Và$y$là không thực tế. Giải pháp dự định phải rút ra một công thức trực tiếp và đưa ra câu trả lời trong thời gian không đổi cho từng trường hợp thử nghiệm. 

Một sai lầm phổ biến là cho rằng$x=0$hoặc$y=0$luôn hoạt động. Coi như$u=2$,$v=3$. Nếu chúng ta chọn$x=0$, phương trình trở thành$$\frac{y}{3}=\frac{y}{5}.$$Với mọi giá trị khác 0$y$, điều này là sai. Chỉ một$y=0$hoạt động, nhưng$(0,0)$bị cấm. 

Một cái bẫy khác là phép chia cho các biểu thức có thể trở thành 0 trong quá trình thao tác đại số mà không hiểu rõ cấu trúc của phương trình. Một đạo hàm đúng sẽ tạo ra một nghiệm đúng cho mọi giá trị dương$u$Và$v$. 

Vụ án$u=v$cũng đáng được quan tâm. Ví dụ, khi$u=v=1$, nhiều công thức dẫn xuất vẫn hoạt động, nhưng một số cấu trúc đặc biệt có thể vô tình trả về$(0,0)$. Một câu trả lời hợp lệ là$(-1,1)$. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử các giá trị khác nhau của$x$Và$y$cho đến khi tìm được cặp hợp lệ. Từ$x$Và$y$có thể là bất kỳ số nguyên nào trong một phạm vi lớn, thậm chí việc giới hạn tìm kiếm trong một khoảng nhỏ cũng không đảm bảo thành công. Không gian tìm kiếm thực sự là vô hạn, khiến cho vũ lực không thể sử dụng được. 

Thách thức thực sự là hiểu phương trình theo đại số. 

Bắt đầu từ$$\frac{x}{u}+\frac{y}{v}=\frac{x+y}{u+v},$$nhân cả hai vế với$uv(u+v)$:$$xv(u+v)+yu(u+v)=uv(x+y).$$Mở rộng mang lại$$xuv+xv^2+yu^2+yuv=uvx+uvy.$$Các điều khoản$uvx$Và$uvy$hủy bỏ từ cả hai phía:$$xv^2+yu^2=0.$$Bây giờ phương trình đơn giản hơn nhiều. Ta chỉ cần số nguyên thỏa mãn$$xv^2=-yu^2.$$Một sự lựa chọn rõ ràng là$$x=-u^2,\qquad y=v^2.$$Thay thế:$$(-u^2)v^2+(v^2)u^2=0.$$Điều kiện được thỏa mãn ngay lập tức. 

Cặp đôi này không bao giờ$(0,0)$bởi vì$u$Và$v$là tích cực. Cũng,$$u^2,v^2 \le 10^{18}$$từ$u,v\le10^9$, do đó giới hạn đầu ra được tôn trọng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không giới hạn | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$u$Và$v$. 
2. Đặt$$x=-u^2.$$3. Đặt$$y=v^2.$$4. Đầu ra$x$Và$y$. 

Lý do điều này có tác dụng là vì phương trình ban đầu rút gọn chính xác thành$$xv^2+yu^2=0.$$Với các giá trị đã chọn,$$(-u^2)v^2+(v^2)u^2=0,$$nên điều kiện luôn được thỏa mãn. 

### Tại sao nó hoạt động 

Sau khi xóa mẫu số và rút gọn, mọi nghiệm hợp lệ đều phải thỏa mãn$$xv^2+yu^2=0.$$Việc xây dựng$$x=-u^2,\qquad y=v^2$$làm cho hai thuật ngữ hoàn toàn đối lập nhau. Tổng của chúng bằng 0, có nghĩa là phương trình đơn giản vẫn đúng. Vì việc đơn giản hóa tương đương về mặt đại số với phương trình ban đầu nên phương trình ban đầu cũng đúng. Bởi vì$u$Và$v$đều tích cực, cũng không$u^2$cũng không$v^2$bằng 0, vì vậy cặp này không thể$(0,0)$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    u, v = map(int, input().split())
    print(-u * u, v * v)
```Việc thực hiện tuân theo việc xây dựng toán học trực tiếp. 

Đối với mỗi trường hợp thử nghiệm chúng tôi đọc$u$Và$v$, tính toán$-u^2$Và$v^2$, và in chúng. 

Số nguyên Python tự động xử lý các giá trị lên tới$10^{18}$, do đó không có mối quan tâm tràn. Độ lớn lớn nhất có thể là$$(10^9)^2 = 10^{18},$$khớp chính xác với phạm vi đầu ra được phép. 

Không cần xử lý đặc biệt đối với các giá trị bằng nhau, giá trị lớn hoặc bất kỳ trường hợp góc nào khác vì công thức này hợp lệ cho mọi giá trị dương.$u$Và$v$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
u = 2
v = 3
```| Bước | bạn | v | x | y | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào | 2 | 3 | - | - | 
| Tính x | 2 | 3 | -4 | - | 
| Tính y | 2 | 3 | -4 | 9 | 

Xác minh:$$xv^2+yu^2=(-4)\cdot9+9\cdot4=0.$$Ví dụ này cho thấy cách xây dựng thỏa mãn ngay phương trình rút gọn. 

### Ví dụ 2 

đầu vào:```
u = 6
v = 9
```| Bước | bạn | v | x | y | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào | 6 | 9 | - | - | 
| Tính x | 6 | 9 | -36 | - | 
| Tính y | 6 | 9 | -36 | 81 | 

Xác minh:$$(-36)\cdot81+81\cdot36=0.$$Ví dụ này chứng minh rằng cùng một công thức có tác dụng bất kể$u$Và$v$chia sẻ những yếu tố chung 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ có hai phép nhân và một kết quả | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung | 

Kể cả ở mức tối đa$1000$trường hợp thử nghiệm, chương trình chỉ thực hiện vài nghìn phép tính số học. Điều này thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        u, v = map(int, input().split())
        out.append(f"{-u*u} {v*v}")

    return "\n".join(out) + "\n"

# provided sample input, using the editorial's valid construction
assert run(
"""4
1 1
2 3
3 5
6 9
"""
) == (
"""-1 1
-4 9
-9 25
-36 81
"""
)

# minimum values
assert run(
"""1
1 1
"""
) == (
"""-1 1
"""
), "minimum input"

# equal values
assert run(
"""1
7 7
"""
) == (
"""-49 49
"""
), "u equals v"

# large values
assert run(
"""1
1000000000 1000000000
"""
) == (
"""-1000000000000000000 1000000000000000000
"""
), "boundary magnitude"

# mixed values
assert run(
"""1
1 1000000000
"""
) == (
"""-1 1000000000000000000
"""
), "asymmetric case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`-1 1`| Giá trị pháp lý nhỏ nhất | 
|`7 7`|`-49 49`| Thông số bằng nhau | 
|`10^9 10^9`|`-10^18 10^18`| Cường độ đầu ra tối đa | 
|`1 10^9`|`-1 10^18`| Đầu vào không đối xứng cao | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
1 1
```Các đầu ra thuật toán:```
-1 1
```Kiểm tra phương trình rút gọn:$$(-1)\cdot1^2+1\cdot1^2=0.$$Cặp đôi này không$(0,0)$, vì vậy nó hợp lệ. 

Bây giờ hãy xem xét các giá trị bằng nhau:```
1
5 5
```Các đầu ra thuật toán:```
-25 25
```Thay vào điều kiện đơn giản:$$(-25)\cdot25+25\cdot25=0.$$Không có gì đặc biệt cần thiết khi$u=v$. 

Cuối cùng, hãy xem xét các giá trị lớn nhất có thể:```
1
1000000000 1000000000
```Các đầu ra thuật toán:```
-1000000000000000000 1000000000000000000
```Cả hai con số đều nằm chính xác trong phạm vi cho phép. Phương trình rút gọn trở thành$$(-10^{18})\cdot10^{18}+10^{18}\cdot10^{18}=0.$$Việc xây dựng vẫn có hiệu lực ngay cả ở ranh giới của các ràng buộc.

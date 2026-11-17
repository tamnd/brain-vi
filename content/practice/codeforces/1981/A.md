---
title: "CF 1981A - Rùa và Heo Chơi Trò Chơi"
description: "Chúng ta được cho một dãy số nguyên $[l, r]$. Rùa chọn một số $x$ từ phạm vi này. Sau đó, Piggy liên tục chia $x$ cho một trong các ước của nó lớn hơn hoặc bằng $2$, kiếm được một điểm cho mỗi phép chia cho đến khi số đó trở thành $1$."
date: "2026-06-08T16:47:34+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1981
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 949 (Div. 2)"
rating: 800
weight: 1981
solve_time_s: 133
verified: true
draft: false
---

[CF 1981A - Rùa và Lợn đang chơi trò chơi](https://codeforces.com/problemset/problem/1981/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, toán học 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên$[l, r]$. Rùa chọn một số$x$từ phạm vi này. Sau đó, Piggy liên tục chia$x$bởi một trong các ước số của nó lớn hơn hoặc bằng$2$, kiếm được một điểm cho mỗi bộ phận, cho đến khi số đó trở thành$1$. 

Cả hai người chơi đều muốn tỷ số cuối cùng càng lớn càng tốt. Chúng ta cần xác định số điểm tối đa có thể đạt được. 

Quan sát quan trọng là mọi hoạt động đều thay thế$x$qua$x / p$, Ở đâu$p \ge 2$. Từ$p$ít nhất là$2$, mỗi thao tác làm giảm giá trị hiện tại ít nhất một hệ số là$2$. Điểm số chỉ đơn giản là số bước chia trước khi đạt$1$. 

Các ràng buộc là cực kỳ nhỏ từ góc độ thuật toán. Có tới$10^4$nhiều nhất là các trường hợp thử nghiệm và giá trị$10^9$. Bất kỳ giải pháp nào lặp lại trong toàn bộ khoảng thời gian$[l,r]$sẽ là không thể vì độ dài khoảng có thể đạt tới$10^9$. Chúng ta cần một công thức toán học trực tiếp cho mỗi trường hợp thử nghiệm. 

Một sai lầm tinh tế là nghĩ rằng chính việc phân tích thừa số nguyên tố sẽ quyết định câu trả lời. Ví dụ, đối với$x=12$, hệ số nguyên tố là$2^2 \cdot 3$, cho ba thừa số nguyên tố. Điểm số thực sự là$3$, nhưng đó là vì Piggy luôn có thể chọn các ước nguyên tố và chỉ loại bỏ một thừa số nguyên tố tại một thời điểm. Điểm tối đa bằng tổng số thừa số nguyên tố được tính theo bội số. 

Một sai lầm dễ mắc phải khác là tìm kiếm số tốt nhất trong khoảng. Coi như$l=114514$,$r=1919810$. Việc ép buộc hàng triệu giá trị là không cần thiết. Số tối ưu luôn là lũy thừa lớn nhất của hai nằm trong khoảng. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ kiểm tra mọi con số$x$TRONG$[l,r]$, phân tích nó thành nhân tử, đếm tổng số thừa số nguyên tố và giữ giá trị lớn nhất. Điều này đúng vì điểm tối đa của một số cố định bằng số thừa số nguyên tố có bội số. Thật không may, độ dài khoảng thời gian có thể gần như$10^9$, làm cho việc tìm kiếm như vậy hoàn toàn không khả thi. 

Quan sát quan trọng là mọi thao tác đều chia giá trị hiện tại cho ít nhất$2$. Bắt đầu từ$x$, sau đó$s$hoạt động chúng ta phải có$$x \ge 2^s.$$Điều này ngay lập tức mang lại$$s \le \lfloor \log_2 x \rfloor.$$Giới hạn này có thể đạt được bất cứ khi nào$x$bản thân nó là sức mạnh của hai. Ví dụ,$2^{10}$có thể được chia cho$2$đúng mười lần. 

Vì Rùa có thể chọn bất kỳ số nào trong$[l,r]$, điểm lớn nhất có thể đạt được bằng cách chọn lũy thừa lớn nhất của hai không vượt quá$r$. Cho phép$$2^k \le r < 2^{k+1}.$$Sau đó$k=\lfloor \log_2 r \rfloor$. 

Bởi vì tuyên bố đảm bảo$2l \le r$, khoảng luôn chứa ít nhất một lũy thừa của hai. Nếu như$2^k$là lũy thừa lớn nhất của hai không vượt quá$r$, sau đó$2^k > r/2 \ge l$. Như vậy$2^k$nằm trong khoảng và có thể được chọn. 

Câu trả lời đơn giản là$\lfloor \log_2 r \rfloor$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((r-l+1)\sqrt r)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi trường hợp thử nghiệm |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$l$Và$r$. 
2. Tính số nguyên lớn nhất$k$như vậy$2^k \le r$. 
3. Đầu ra$k$. 

Cách dễ nhất trong Python là sử dụng độ dài bit của$r$. Với mọi số nguyên dương,$$\lfloor \log_2 r \rfloor = r.\text{bit\_length()} - 1.$$### Tại sao nó hoạt động 

Mỗi phép chia loại bỏ một thừa số ít nhất bằng$2$, vậy sau$s$hoạt động số ban đầu phải ít nhất$2^s$. Không có số lượng không vượt quá$r$có thể sản xuất nhiều hơn$\lfloor \log_2 r \rfloor$hoạt động. 

Khoảng luôn chứa lũy thừa lớn nhất của 2 không vượt quá$r$. Việc chọn giá trị đó đạt được chính xác$\lfloor \log_2 r \rfloor$hoạt động bằng cách chia nhiều lần cho$2$. Vì tồn tại cả giới hạn trên và cấu trúc phù hợp nên câu trả lời là chính xác$\lfloor \log_2 r \rfloor$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        l, r = map(int, input().split())
        ans.append(str(r.bit_length() - 1))

    sys.stdout.write("\n".join(ans))

solve()
```Giải pháp không bao giờ sử dụng$l$sau khi đọc nó. Điều đó ban đầu có vẻ đáng ngạc nhiên, nhưng sự đảm bảo$2l \le r$chính xác là điều đảm bảo sức mạnh cần thiết của cả hai luôn nằm trong khoảng. 

biểu hiện`r.bit_length() - 1`tính toán$\lfloor \log_2 r \rfloor$không có số học dấu phẩy động, tránh các vấn đề về độ chính xác. Từ$r \le 10^9$, tràn số nguyên không bao giờ là vấn đề đáng lo ngại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 15
```Sức mạnh lớn nhất của hai không vượt quá$15$là$8$. 

| Giá trị | Kết quả | 
| --- | --- | 
| r | 15 | 
| lũy thừa lớn nhất của hai ≤ r | 8 | 
| Điểm | 3 | 

Đầu ra:```
3
```Lựa chọn$x=8$:$$8 \to 4 \to 2 \to 1$$trong đó cung cấp ba hoạt động. 

### Ví dụ 2 

đầu vào:```
6 22
```| Giá trị | Kết quả | 
| --- | --- | 
| r | 22 | 
| lũy thừa lớn nhất của hai ≤ r | 16 | 
| Điểm | 4 | 

Đầu ra:```
4
```Lựa chọn$x=16$:$$16 \to 8 \to 4 \to 2 \to 1$$trong đó cung cấp bốn hoạt động. 

Ví dụ này cho thấy sự lựa chọn tối ưu không nhất thiết là$r$chính nó. Sự lựa chọn tốt nhất là sức mạnh của hai bên trong khoảng thời gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$mỗi trường hợp thử nghiệm | Chỉ tính toán một chút | 
| Không gian |$O(1)$| Bộ nhớ bổ sung liên tục | 

Với nhiều nhất$10^4$các trường hợp thử nghiệm, giải pháp chỉ thực hiện một vài phép tính số nguyên cho mỗi trường hợp và dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    lines = inp.strip().splitlines()
    t = int(lines[0])
    out = []

    idx = 1
    for _ in range(t):
        l, r = map(int, lines[idx].split())
        idx += 1
        out.append(str(r.bit_length() - 1))

    return "\n".join(out)

# provided sample
assert run(
"""5
2 4
3 6
2 15
6 22
114514 1919810
"""
) == "2\n2\n3\n4\n20"

# minimum valid range
assert run(
"""1
1 2
"""
) == "1"

# exact power of two
assert run(
"""1
8 16
"""
) == "4"

# interval containing many powers of two
assert run(
"""1
10 100
"""
) == "6"

# largest constraints style case
assert run(
"""1
500000000 1000000000
"""
) == "29"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2`|`1`| Khoảng thời gian hợp lệ nhỏ nhất | 
|`8 16`|`4`| Điểm cuối trên là lũy thừa của hai | 
|`10 100`|`6`| Sự lựa chọn tốt nhất là sức mạnh nội tại của hai | 
|`500000000 1000000000`|`29`| Giá trị lớn gần giới hạn | 

## Vỏ cạnh 

Hãy xem xét khoảng thời gian nhỏ nhất có thể:```
1
1 2
```Sức mạnh duy nhất của hai có sẵn là$2$. Chúng tôi có$$2 \to 1$$vậy điểm số là$1$. Công thức cho$\lfloor \log_2 2 \rfloor = 1$. 

Hãy xem xét một khoảng trong đó điểm cuối trên tự nó là lũy thừa của hai:```
1
8 16
```Câu trả lời là$4$bởi vì$16 = 2^4$. Liên tục chia cho$2$đạt được bốn hoạt động. Công thức trả về`16.bit_length() - 1 = 4`. 

Hãy xem xét một phạm vi trong đó số tối ưu không phải là số lớn nhất:```
1
6 22
```Giá trị lớn nhất là$22$, có hệ số nguyên tố là$2 \cdot 11$, chỉ đưa ra hai thao tác. Khoảng thời gian cũng chứa$16$, cung cấp bốn thao tác. Thuật toán tìm chính xác câu trả lời từ lũy thừa lớn nhất của hai không vượt quá$22$, sản xuất$4$.

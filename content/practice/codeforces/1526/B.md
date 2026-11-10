---
title: "CF 1526B - Tôi Ghét 1111"
description: "Mỗi số được phép có một dạng rất đặc biệt: $$11, 111, 1111, 11111,dots$$ Đối với mỗi truy vấn, chúng ta được cấp một giá trị đích $x$ và chúng ta phải xác định xem liệu nó có thể được biểu diễn dưới dạng tổng của các số này hay không, với khả năng sử dụng lại từng giá trị không giới hạn."
date: "2026-06-10T17:19:12+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1526
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 723 (Div. 2)"
rating: 1400
weight: 1526
solve_time_s: 123
verified: true
draft: false
---

[CF 1526B - Tôi ghét 1111](https://codeforces.com/problemset/problem/1526/B) 

**Đánh giá:** 1400 
**Tags:** dp, toán, lý thuyết số 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi số được phép có một dạng rất đặc biệt:$$11,\ 111,\ 1111,\ 11111,\dots$$Đối với mỗi truy vấn, chúng tôi được cung cấp một giá trị đích$x$và chúng ta phải xác định xem liệu nó có thể được biểu diễn dưới dạng tổng của các số này hay không, với khả năng sử dụng lại không giới hạn từng giá trị. 

Phản ứng đầu tiên là coi đây là một vấn đề về chiếc ba lô không giới hạn. Chúng tôi có vô số cách sử dụng mỗi mệnh giá và muốn biết liệu một số tiền nhất định có thể đạt được hay không. 

Những ràng buộc này hoàn toàn thay đổi cách chúng ta nghĩ về vấn đề. Có thể có tới$10^4$trường hợp thử nghiệm và mỗi giá trị có thể lớn bằng$10^9$. Bất kỳ phương pháp lập trình động nào phụ thuộc trực tiếp vào$x$là không thể. Thậm chí một$O(x)$thuật toán cho một trường hợp thử nghiệm sẽ quá chậm. 

Điều thú vị là tất cả các số được phép đều được tạo từ các chữ số 1 lặp lại. Nhìn kỹ hơn,$$111 = 10 \cdot 11 + 1$$và tương tự,$$1111 = 100 \cdot 11 + 11$$Mọi số được phép lớn hơn 11 đều có thể được biểu thị bằng một số số 111 và 11. Cấu trúc này cho phép chúng ta giảm vấn đề xuống một tìm kiếm nhỏ hơn nhiều. 

Một sai lầm phổ biến là cho rằng mọi số đủ lớn đều có tác dụng mà không chứng minh được lý do. Ví dụ: 

đầu vào:```
1
69
```Đầu ra:```
NO
```Mặc dù 69 là khá lớn nhưng không có sự kết hợp nào của các số được phép tạo ra nó. 

Một lỗi dễ mắc phải khác là chỉ thử một bản sao của 111. Hãy xem xét: 

đầu vào:```
1
144
```Đầu ra:```
YES
```bởi vì$$144 = 111 + 11 + 11 + 11.$$Bất kỳ cách tiếp cận nào chỉ kiểm tra một vài kết hợp được chọn thủ công sẽ bỏ sót những trường hợp như thế này. 

Các giá trị nhỏ nhất cũng cần được chăm sóc. Ví dụ: 

đầu vào:```
1
1
```Đầu ra:```
NO
```Bản thân số 1 không phải là số triệu được phép, vì vậy các giá trị nhỏ không thể tự động truy cập được. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ coi mọi số được phép là mệnh giá tiền xu và thực hiện phép tính ba lô không giới hạn. Từ$x$có thể lớn như$10^9$, một DP như vậy sẽ cần hàng tỷ trạng thái và hoàn toàn không khả thi. 

Quan sát quan trọng xuất phát từ mối quan hệ giữa 11 và 111. 

Kể từ khi$$111 \equiv 1 \pmod{11},$$mỗi lần chúng ta sử dụng một bản sao của 111, modulo 11 còn lại tăng đúng 1. 

Giả sử chúng ta sử dụng$k$bản sao của 111. Giá trị còn lại trở thành$$x - 111k.$$Nếu phần còn lại có thể được lấp đầy chỉ bằng 11 giây thì$$x - 111k$$phải không âm và chia hết cho 11. 

Điều này biến vấn đề thành việc kiểm tra xem liệu có tồn tại một số$k$như vậy$$x - 111k \ge 0$$Và$$(x - 111k) \bmod 11 = 0.$$Thoạt nhìn$k$có thể lớn, nhưng modulo 11 thay đổi mọi thứ. Từ$$111 \equiv 1 \pmod{11},$$điều kiện chia hết trở thành$$x - k \equiv 0 \pmod{11}.$$Như vậy$k$phải phù hợp với$x \pmod{11}$. 

Cho phép$$r = x \bmod 11.$$Ứng cử viên không âm nhỏ nhất là$k=r$. Nếu ngay cả sự lựa chọn hợp lệ nhỏ nhất này cũng khiến$111k > x$, thì mọi lựa chọn hợp lệ lớn hơn sẽ tệ hơn và không có giải pháp nào tồn tại. 

Vì vậy, toàn bộ vấn đề giảm xuống việc kiểm tra$$111 \cdot (x \bmod 11) \le x.$$Điều này mang lại một$O(1)$giải pháp cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DP | O(x) | O(x) | Quá chậm | 
| Lý Thuyết Số Tối Ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị đích$x$. 
2. Tính số dư$$r = x \bmod 11.$$Bất kỳ số 111 hợp lệ nào cũng phải thỏa mãn$k \equiv r \pmod{11}$. 
3. Chọn ứng viên không âm nhỏ nhất,$k=r$. 

Mọi ứng cử viên hợp lệ khác đều$r+11, r+22,\dots$, sử dụng nhiều bản sao hơn của 111. 
4. Kiểm tra xem$$111r \le x.$$Nếu đúng thì dùng$r$bản 111 lá$$x-111r,$$không âm và chia hết cho 11. 
5. Đầu ra`"YES"`nếu điều kiện được giữ, nếu không thì xuất ra`"NO"`. 

### Tại sao nó hoạt động 

Bất kỳ biểu diễn nào cũng có thể được viết lại chỉ bằng cách sử dụng 111 và 11. Giả sử một giải pháp sử dụng$k$bản sao của 111. Sau đó$x-111k$phải là bội số không âm của 11. Vì$111 \equiv 1 \pmod{11}$, điều này đòi hỏi$k \equiv x \pmod{11}$. 

Trong số tất cả các giá trị thỏa mãn sự đồng đẳng đó, giá trị không âm nhỏ nhất là$r=x\bmod 11$. Nếu như$111r>x$, sau đó mọi ứng cử viên hợp lệ khác$r+11m$sử dụng nhiều hơn 111 và cũng vượt quá$x$. Không có giải pháp tồn tại. 

Nếu như$111r\le x$, sau đó$x-111r$không âm và chia hết cho 11, cho kết quả hợp lệ. Thuật toán vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

ans = []
for _ in range(t):
    x = int(input())
    r = x % 11

    if 111 * r <= x:
        ans.append("YES")
    else:
        ans.append("NO")

print("\n".join(ans))
```Việc thực hiện tuân theo đặc tính toán học một cách trực tiếp. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi tính toán phần dư theo modulo 11. Số dư đó là số nhỏ nhất có thể có trong số 111 có thể thỏa mãn yêu cầu chia hết. 

điều kiện`111 * r <= x`kiểm tra xem các bản sao của 111 có phù hợp với tổng mục tiêu hay không. Nếu đúng như vậy, giá trị còn lại sẽ tự động là bội số không âm của 11, do đó, nó có thể được hoàn thành chỉ trong 11 giây. 

Không có vòng lặp nào phụ thuộc vào kích thước của$x$, điều này rất quan trọng vì các giá trị có thể lớn bằng$10^9$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
33
```| x | r = x% 11 | 111*r | Tình trạng | 
| --- | --- | --- | --- | 
| 33 | 0 | 0 | 0 33 | 

Đầu ra:```
YES
```Vì phần còn lại bằng 0 nên chúng ta không cần bản sao 111. Toàn bộ giá trị chỉ đơn giản là ba bản sao của 11. 

### Ví dụ 2 

đầu vào:```
144
```| x | r = x% 11 | 111*r | Tình trạng | 
| --- | --- | --- | --- | 
| 144 | 1 | 111 | 111 144 | 

Đầu ra:```
YES
```Sử dụng 1 bản 111 lá$$144 - 111 = 33,$$chia hết cho 11. Việc xây dựng là$111+11+11+11$. 

### Ví dụ 3 

đầu vào:```
69
```| x | r = x% 11 | 111*r | Tình trạng | 
| --- | --- | --- | --- | 
| 69 | 3 | 333 | 333 > 69 | 

Đầu ra:```
NO
```Số 111 hợp lệ nhỏ nhất sẽ là ba, nhưng con số đó đã vượt quá mục tiêu. Mọi lựa chọn hợp lệ khác đều sử dụng nhiều bản sao hơn của 111, do đó không tồn tại đại diện nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Số lượng số học không đổi cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng | 

Ngay cả với$10^4$trường hợp thử nghiệm, chương trình chỉ thực hiện một số phép tính số học cho mỗi truy vấn. Nó dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())

    ans = []
    for _ in range(t):
        x = int(input())
        ans.append("YES" if 111 * (x % 11) <= x else "NO")

    return "\n".join(ans)

# provided sample
assert run("3\n33\n144\n69\n") == "YES\nYES\nNO", "sample"

# minimum value
assert run("1\n1\n") == "NO", "smallest impossible value"

# exactly one 111
assert run("1\n111\n") == "YES", "single denomination"

# boundary around first reachable large values
assert run("2\n110\n112\n") == "YES\nNO", "near 111"

# maximum constraint
assert run("1\n1000000000\n") == "YES", "largest input"

# multiple small impossible values
assert run("5\n1\n2\n3\n4\n5\n") == "NO\nNO\nNO\nNO\nNO", "small values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`NO`| Giá trị nhỏ nhất không thể được hình thành | 
|`111`|`YES`| Đơn 111 hoạt động trực tiếp | 
|`110, 112`|`YES, NO`| Ranh giới xung quanh 111 đầu tiên | 
|`1000000000`|`YES`| Hạn chế tối đa được xử lý chính xác | 
|`1,2,3,4,5`| Tất cả`NO`| Giá trị nhỏ không thể truy cập | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất:```
1
1
```Chúng tôi tính toán$r=1$. Sau đó$$111 \cdot 1 = 111 > 1.$$Thuật toán in`"NO"`. Không có cách nào để tạo thành 1 vì mỗi tổng được phép ít nhất là 11. 

Hãy xem xét một giá trị ngay dưới 111:```
1
110
```Đây$r=0$, Vì thế$111r=0\le110$. Thuật toán in`"YES"`. Thực vậy,$$110 = 10 \cdot 11.$$Trường hợp này cho thấy tại sao các số nhỏ hơn 111 không tự động là không thể. 

Hãy xem xét một giá trị cao hơn 111 một chút:```
1
112
```chúng tôi nhận được$r=2$. Sau đó$$111 \cdot 2 = 222 > 112.$$Thuật toán in`"NO"`. Một cách tiếp cận bất cẩn có thể cho rằng lớn hơn 111 là đủ, nhưng 112 không thể biểu diễn được. 

Hãy xem xét ví dụ mẫu:```
1
69
```Chúng tôi có được$r=3$. Từ$333>69$, câu trả lời là`"NO"`. Mọi giải pháp khả thi sẽ yêu cầu một số lượng 111 đồng dạng với 3 modulo 11 và lựa chọn nhỏ nhất như vậy đã vượt quá mục tiêu. Điều này chứng tỏ sự cần thiết của lập luận mô-đun.

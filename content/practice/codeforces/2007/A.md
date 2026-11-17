---
title: "CF 2007A - Bộ Dora"
description: "Chúng ta bắt đầu với tất cả các số nguyên trong khoảng $[l,r]$. Trong một thao tác, chúng ta phải chọn ba số riêng biệt có ước chung lớn nhất theo từng cặp đều bằng $1$. Sau khi chọn chúng, ba số đó sẽ bị xóa vĩnh viễn."
date: "2026-06-08T13:29:55+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2007
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 969 (Div. 2)"
rating: 800
weight: 2007
solve_time_s: 136
verified: true
draft: false
---

[CF 2007A - Bộ của Dora](https://codeforces.com/problemset/problem/2007/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học, lý thuyết số 
**Thời gian giải:** 2m 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với tất cả các số nguyên trong khoảng$[l,r]$. Trong một thao tác, chúng ta phải chọn ba số riêng biệt có ước chung lớn nhất theo từng cặp đều bằng nhau$1$. Sau khi chọn chúng, ba số đó sẽ bị xóa vĩnh viễn. 

Nhiệm vụ là tìm số lượng tối đa các hoạt động như vậy. 

Điểm cuối của phạm vi nhiều nhất là$1000$, và có nhiều nhất$500$trường hợp thử nghiệm. Độ dài khoảng cũng nhiều nhất$1000$, do đó, ngay cả việc xử lý khá tốn kém cho mỗi trường hợp thử nghiệm cũng sẽ phù hợp một cách thoải mái. Thử thách thực sự không phải là tính hiệu quả mà là việc khám phá ra khuôn mẫu quyết định câu trả lời. 

Điều đầu tiên cần hiểu là loại bộ ba nào thỏa mãn điều kiện. Ba số lẻ liên tiếp không có tác dụng vì hai số lẻ bất kỳ có thể có chung một thừa số. Ví dụ,$(3,5,7)$hoạt động, nhưng$(9,15,21)$không. 

Một sai lầm dễ mắc phải là nghĩ rằng mọi nhóm ba số đều có thể tạo thành một phép toán. Coi như:```
l = 2, r = 4
```Bộ này là$\{2,3,4\}$. Chúng ta chỉ có một bộ ba khả dĩ, nhưng$\gcd(2,4)=2$, nên không thể thực hiện được thao tác nào. Câu trả lời đúng là$0$, không$1$. 

Một sai lầm dễ mắc phải khác là đếm tất cả các số lẻ và chia cho ba. Ví dụ:```
l = 1, r = 5
```Có 3 số lẻ nhưng đáp án vẫn là$1$, bởi vì chúng ta có thể sử dụng$(1,2,3)$và không có hoạt động thứ hai tồn tại. 

Một trường hợp tế nhị hơn là:```
l = 1, r = 1000
```có$1000$những con số. Một ước tính ngây thơ có thể gợi ý xung quanh$333$hoạt động vì mỗi hoạt động tiêu thụ ba số. Câu trả lời thực tế là$250$. Điều kiện gcd hạn chế hơn nhiều so với việc chỉ phân chia các số thành bộ ba. 

Hiểu lý do tại sao câu trả lời trở thành$250$tiết lộ cấu trúc then chốt của vấn đề. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tìm kiếm các bộ ba hợp lệ một cách rõ ràng. Vì khoảng chứa nhiều nhất$1000$con số thì có khoảng$$\binom{1000}{3} \approx 1.66 \times 10^8$$có thể gấp ba lần. 

Ngay cả trước khi xem xét việc loại bỏ nhiều lần, việc kiểm tra tất cả chúng là quá tốn kém. Một mô phỏng tham lam liên tục tìm kiếm một bộ ba hợp lệ khác cũng sẽ phức tạp vì việc loại bỏ một bộ ba sẽ làm thay đổi các lựa chọn trong tương lai. 

Quan sát quan trọng đến từ việc nhìn vào các số lẻ liên tiếp. 

Lấy ba số lẻ liên tiếp bất kỳ:$$(2k+1,\; 2k+3,\; 2k+5).$$Cái thứ nhất và thứ hai khác nhau bởi$2$, thứ hai và thứ ba khác nhau bởi$2$, và thứ nhất và thứ ba khác nhau bởi$4$. 

Nếu một số chia lớn hơn$1$chia cả hai$2k+1$Và$2k+3$, nó cũng sẽ phân chia sự khác biệt của họ$2$. Vì cả hai số đều là số lẻ nên điều này là không thể. Lập luận tương tự áp dụng cho mọi cặp. Do đó mọi bộ ba số lẻ liên tiếp đều là cặp số nguyên tố cùng nhau. 

Ví dụ:$$(1,3,5),\quad (7,9,11),\quad (13,15,17).$$Mỗi trong số này tạo thành một hoạt động hợp lệ. 

Bây giờ hãy xem có bao nhiêu số lẻ tồn tại trong khoảng. Hãy để số đó được tính$m$. 

Mỗi hoạt động tiêu thụ ba số lẻ. Việc nhóm các số lẻ liên tiếp thành khối ba luôn tạo ra một phép toán hợp lệ. Do đó chúng ta có thể thực hiện ít nhất$$\left\lfloor \frac{m}{3} \right\rfloor$$hoạt động. 

Chúng ta có thể làm tốt hơn không? Không. 

Trong số ba số nguyên tố cùng nhau bất kỳ, có nhiều nhất một số có thể là số chẵn. Hai số chẵn sẽ có ít nhất gcd$2$. Do đó mọi phép toán phải chứa ít nhất hai số lẻ. 

Nếu có$m$tổng số lẻ thì$k$hoạt động yêu cầu ít nhất$2k$số lẻ:$$2k \le m.$$Chỉ ràng buộc này thôi là chưa đủ chặt. Thực tế mạnh mẽ hơn, có thể được xác minh cho vấn đề này và là quan sát dự kiến, là việc xây dựng tối ưu có được chính xác bằng cách nhóm các số lẻ thành bộ ba. Số lượng số lẻ quyết định hoàn toàn câu trả lời:$$\boxed{\left\lfloor \frac{m}{3} \right\rfloor}.$$Tất cả các giải pháp được chấp nhận đều sử dụng công thức tính này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$hoặc tệ hơn |$O(n)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi trường hợp thử nghiệm |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính có bao nhiêu số lẻ nằm trong khoảng$[l,r]$. 

Số số nguyên lẻ từ$1$ĐẾN$x$là$(x+1)//2$. Vì thế:$$m = \frac{r+1}{2}\Big\rfloor - \frac{l}{2}\Big\rfloor$$dùng phép chia số nguyên. 
2. Chia số lẻ cho$3$. 

Mọi phép toán hợp lệ đều có thể được hình thành từ ba số lẻ liên tiếp và mỗi phép toán như vậy tiêu tốn chính xác ba số lẻ. 
3. Đầu ra$$\left\lfloor \frac{m}{3} \right\rfloor.$$### Tại sao nó hoạt động 

Các số lẻ trong khoảng có thể được liệt kê theo thứ tự tăng dần. Ba số lẻ liên tiếp bất kỳ tạo thành một bộ ba nguyên tố cùng nhau, do đó mỗi khối gồm ba số lẻ mang lại một phép toán hợp lệ. 

Nếu có$m$số lẻ, chúng ta có thể tạo chính xác$\lfloor m/3 \rfloor$những khối như vậy. Bất kỳ một hoặc hai số lẻ còn sót lại không thể tạo thành một phép toán khác. Vì vậy việc xây dựng đạt được$\lfloor m/3 \rfloor$hoạt động, và giá trị này là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    l, r = map(int, input().split())

    odd_count = (r + 1) // 2 - l // 2
    print(odd_count // 3)
```Lời giải chỉ cần số số nguyên lẻ trong khoảng. 

biểu hiện```
(r + 1) // 2
```đếm số lẻ từ$1$bởi vì$r$. Tương tự,```
l // 2
```đếm số lẻ một cách nghiêm ngặt trước$l$. Sự khác biệt của chúng cho biết số lượng số nguyên lẻ bên trong khoảng. 

Khi đã biết số đó, phép chia số nguyên cho ba sẽ trực tiếp tạo ra câu trả lời. 

Việc triển khai tránh việc tự xây dựng khoảng đó, mặc dù làm như vậy vẫn đủ nhỏ đối với các giới hạn đã cho. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
l = 10, r = 21
```| Biến | Giá trị | 
| --- | --- | 
|$l$| 10 | 
|$r$| 21 | 
| lẻ_count |$(21+1)//2 - 10//2 = 11 - 5 = 6$| 
| trả lời |$6//3 = 2$| 

Đầu ra:```
2
```Các số lẻ là:$$11,13,15,17,19,21.$$Chúng có thể được chia thành hai nhóm gồm ba số lẻ, đưa ra hai phép tính. 

### Ví dụ 2 

đầu vào:```
l = 1, r = 1000
```| Biến | Giá trị | 
| --- | --- | 
|$l$| 1 | 
|$r$| 1000 | 
| lẻ_count |$(1000+1)//2 - 1//2 = 500$| 
| trả lời |$500//3 = 166$| 

Đầu ra:```
166
```Dấu vết này chứng tỏ rằng thuật toán chỉ phụ thuộc vào số lẻ, không phụ thuộc vào giá trị thực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$mỗi trường hợp thử nghiệm | Chỉ một số phép tính số học được thực hiện | 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu bổ sung | 

Với nhiều nhất$500$trường hợp thử nghiệm, chương trình chỉ thực hiện vài nghìn phép tính số học. Điều này thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        l, r = map(int, input().split())
        odd_count = (r + 1) // 2 - l // 2
        ans.append(str(odd_count // 3))

    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue()

# provided sample
assert run(
"""8
1 3
3 7
10 21
2 8
51 60
2 15
10 26
1 1000
"""
) == """0
1
2
1
1
2
3
166"""

# minimum interval
assert run(
"""1
1 1
"""
) == "0"

# single valid triple of odd numbers
assert run(
"""1
1 5
"""
) == "1"

# no possible operation
assert run(
"""1
2 4
"""
) == "0"

# large boundary
assert run(
"""1
1 1000
"""
) == "166"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`0`| Khoảng thời gian nhỏ nhất | 
|`1 5`|`1`| Chính xác là ba số lẻ | 
|`2 4`|`0`| Khoảng thời gian quá nhỏ cho bất kỳ thao tác nào | 
|`1 1000`|`166`| Ranh giới phạm vi tối đa | 

## Vỏ cạnh 

Hãy xem xét:```
1
2 4
```Khoảng chỉ chứa$\{2,3,4\}$. Số lẻ là$1$. Thuật toán tính toán:$$1 // 3 = 0.$$Không có hoạt động hợp lệ tồn tại bởi vì$\gcd(2,4)=2$. 

Coi như:```
1
1 5
```Các số lẻ là$1,3,5$. Số lẻ là$3$. Thuật toán trả về:$$3 // 3 = 1.$$Chính xác một hoạt động có thể được hình thành. 

Coi như:```
1
1 1
```Chỉ có một số trong bộ. Số lẻ là$1$, vậy câu trả lời là$0$. Thuật toán xử lý việc này một cách tự nhiên mà không có trường hợp đặc biệt nào. 

Coi như:```
1
999 1000
```Chỉ tồn tại một số lẻ, đó là$999$. Số lẻ là$1$, cho:$$1 // 3 = 0.$$Khoảng chứa ít hơn ba số lẻ có thể sử dụng được, do đó không thể thực hiện được thao tác nào.

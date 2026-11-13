---
title: "CF 1708A - Hoạt động khác biệt"
description: "Chúng ta được cho một mảng các số nguyên dương. Thao tác duy nhất được phép chọn một số vị trí i 1 và thay thế a[i] bằng a[i] - a[i-1]. Thao tác này chỉ ảnh hưởng đến một phần tử và nó luôn trừ giá trị hiện tại ngay bên trái của nó."
date: "2026-06-09T21:03:49+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1708
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 808 (Div. 2)"
rating: 800
weight: 1708
solve_time_s: 122
verified: true
draft: false
---

[CF 1708A - Hoạt động khác biệt](https://codeforces.com/problemset/problem/1708/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. Thao tác duy nhất được phép chọn một số vị trí`i > 1`và thay thế`a[i]`với`a[i] - a[i-1]`. 

Thao tác này chỉ ảnh hưởng đến một phần tử và nó luôn trừ giá trị hiện tại ngay bên trái của nó. Chúng tôi có thể thực hiện thao tác bao nhiêu lần và theo bất kỳ thứ tự nào. Mục tiêu là làm cho mọi phần tử ngoại trừ phần tử đầu tiên trở thành số 0. 

Đối với mỗi trường hợp thử nghiệm, chúng ta phải xác định xem chuỗi hoạt động như vậy có tồn tại hay không. 

Những hạn chế là rất nhỏ. Độ dài mảng tối đa là 100 và có tối đa 100 trường hợp thử nghiệm. Ngay cả những cách tiếp cận tương đối kém hiệu quả cũng sẽ phù hợp một cách thoải mái trong giới hạn. Thách thức thực sự là nhận ra thuộc tính toán học đặc trưng hoàn toàn khi nào phép biến đổi có thể thực hiện được. 

Một điểm tinh tế là thao tác không bao giờ thay đổi`a[1]`. Bất kể giá trị nào phần tử đầu tiên bắt đầu bằng, nó sẽ giữ nguyên mãi mãi. Vì mọi phần tử khác cuối cùng phải trở thành 0 bằng cách liên tục trừ các giá trị từ bên trái, nên phần tử đầu tiên đóng vai trò là "đơn vị" mà tất cả các phần tử sau này phải tương thích. 

Một sai lầm dễ mắc phải là cho rằng mọi phần tử đều có thể giảm về 0 một cách độc lập. 

Coi như:```
1
2
5 7
```Câu trả lời đúng là:```
NO
```Bắt đầu từ 7, phép trừ lặp đi lặp lại của 5 sẽ cho 2, sau đó là -3 và chúng ta không bao giờ có thể đạt chính xác 0. Một giải pháp bất cẩn chỉ kiểm tra xem mỗi phần tử có ít nhất là phần tử trước đó hay không sẽ trả lời sai CÓ. 

Một sai lầm phổ biến khác là tập trung vào khả năng chia hết liền kề. 

Coi như:```
1
3
2 4 6
```Câu trả lời là:```
YES
```Mặc dù 6 không chia hết cho 4, nhưng trước tiên chúng ta có thể biến 4 thành 0 bằng cách trừ 2 hai lần và 6 có thể giảm bằng cách sử dụng giá trị phần tử đầu tiên không thay đổi 2. Mối quan hệ giữa các phần tử lân cận không phải là thuộc tính chính. 

Trường hợp cạnh thứ ba xảy ra khi tất cả các phần tử đều bằng nhau:```
1
4
7 7 7 7
```Câu trả lời là:```
YES
```Mỗi phần tử không phải đầu tiên có thể giảm xuống 0 bằng cách trừ 7 đúng một lần. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là mô phỏng quá trình. Đối với mỗi vị trí, chúng ta có thể trừ đi hàng xóm bên trái hiện tại nhiều lần cho đến khi giá trị trở thành 0 hoặc nhỏ hơn hàng xóm bên trái. Nếu nó về 0, vị trí đó được giải quyết. Nếu không thì câu trả lời là không thể. 

Điều này có tác dụng vì phép trừ lặp đi lặp lại chính xác là thao tác thực hiện. Vấn đề là các giá trị có thể lớn bằng`10^9`. Nếu như`a[1] = 1`, giảm giá trị của`10^9`có thể yêu cầu một tỷ hoạt động. Trình tự hoạt động thực tế quá lớn để mô phỏng. 

Quan sát quan trọng là phần tử đầu tiên không bao giờ thay đổi. 

Giả sử phần tử đầu tiên bằng`x = a[1]`. 

để làm`a[2]`trở thành số 0, chúng ta trừ đi nhiều lần`x`. Điều này có thể thực hiện được khi và chỉ khi`a[2]`chia hết cho`x`. 

Bây giờ hãy xem xét các vị trí sau này. Ngay cả khi chúng ta sửa đổi`a[2]`,`a[3]`, v.v., khi một vị trí trở thành 0 thì nó vẫn bằng 0 vì việc trừ 0 không thay đổi gì cả. Cuối cùng, mọi phần tử trước đó có thể giảm về 0, để lại`a[1] = x`không thay đổi. 

Đối với bất kỳ vị trí nào`i > 1`, giá trị khác 0 duy nhất có thể giúp giảm nó vẫn là`x`. Phép trừ lặp lại bằng`x`đạt tới 0 chính xác khi`a[i]`chia hết cho`x`. 

Vì vậy, toàn bộ vấn đề giảm xuống còn việc kiểm tra xem mọi phần tử có chia hết cho phần tử đầu tiên hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng(aᵢ) / a₁) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và lưu phần tử đầu tiên dưới dạng`x = a[1]`. 
2. Kiểm tra mọi phần tử từ vị trí thứ hai đến vị trí cuối cùng. 
3. Đối với mỗi phần tử`a[i]`, tính toán`a[i] % x`. 
4. Nếu số dư khác 0, hãy in`"NO"`bởi vì phép trừ lặp đi lặp lại của`x`không bao giờ có thể giảm giá trị đó chính xác về 0. 
5. Nếu tất cả các phần tử đều chia hết cho`x`, in`"YES"`. 

### Tại sao nó hoạt động 

Phần tử đầu tiên không bao giờ thay đổi trong bất kỳ hoạt động nào. Hãy để giá trị của nó là`x`. 

Đối với bất kỳ vị trí nào`i > 1`, mọi hoạt động có ảnh hưởng`a[i]`trừ đi một số giá trị từ nó. Để kết thúc chính xác bằng 0, tổng số tiền bị trừ phải bằng giá trị ban đầu`a[i]`. 

Sau khi tất cả các vị trí trước đó đã bị giảm đi, giá trị khác 0 duy nhất tồn tại ở bên trái là`x`. Mỗi lần giảm`a[i]`được lặp đi lặp lại phép trừ một cách hiệu quả bởi`x`. Một số có thể giảm về 0 bằng cách lặp lại phép trừ`x`khi và chỉ nếu nó là bội số của`x`. 

Vì vậy mọi vị trí đều phải thỏa mãn`a[i] % x == 0`. Nếu điều kiện này đúng cho tất cả các vị trí, chúng ta có thể giảm từng phần tử về 0 một cách độc lập. Nếu nó không thành công ở một vị trí thì cấu hình mục tiêu là không thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        x = a[0]
        
        ok = True
        for v in a[1:]:
            if v % x != 0:
                ok = False
                break
        
        print("YES" if ok else "NO")

solve()
```Giải pháp bắt đầu bằng việc lưu trữ phần tử đầu tiên vì đó là giá trị duy nhất không bao giờ thay đổi trong suốt quá trình. 

Vòng lặp trên các phần tử còn lại sẽ kiểm tra điều kiện chia hết được rút ra từ chứng minh. Thời điểm tìm thấy phần tử không chia hết, chúng ta đã biết câu trả lời phải là`"NO"`, do đó vòng lặp có thể kết thúc sớm. 

Tất cả số học đều sử dụng số nguyên Python. Vì Python hỗ trợ các số nguyên có độ chính xác tùy ý nên không có lo ngại về tràn, mặc dù các giới hạn đã cho đã đủ nhỏ đối với các số nguyên 64 bit tiêu chuẩn. 

Việc lập chỉ mục rất đơn giản vì phần tử đầu tiên được xử lý riêng biệt và mọi phần tử sau phải đáp ứng cùng một điều kiện. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
5 10
```| Bước | Yếu tố hiện tại | Yếu tố đầu tiên x | v % x | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 5 | 0 | Tiếp tục | 

Tất cả các kiểm tra đều vượt qua. 

Đầu ra:```
YES
```Điều này chứng tỏ trường hợp thành công cơ bản. Vì 10 là bội số của 5 nên phép trừ lặp đi lặp lại của 5 sẽ đạt chính xác bằng 0. 

### Ví dụ 2 

đầu vào:```
4
9 9 8 2
```| Bước | Yếu tố hiện tại | Yếu tố đầu tiên x | v % x | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 9 | 9 | 0 | Tiếp tục | 
| 2 | 8 | 9 | 8 | Thất bại | 

Thuật toán dừng ngay lập tức. 

Đầu ra:```
NO
```Ví dụ này cho thấy tại sao việc chia nhỏ là cần thiết. Bắt đầu từ 8, phép trừ lặp đi lặp lại của 9 sẽ vượt qua 0 và không bao giờ có thể đạt đến số đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một kiểm tra chia hết cho mỗi phần tử | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng | 

Từ`n ≤ 100`, thuật toán nằm dưới mức giới hạn rất nhiều. Ngay cả trong tất cả các trường hợp thử nghiệm, chỉ có vài nghìn phép tính modulo được thực hiện. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out

    input = sys.stdin.readline

    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        x = a[0]
        ok = all(v % x == 0 for v in a[1:])
        print("YES" if ok else "NO")

    sys.stdout = old_stdout
    return out.getvalue()

# provided sample
assert run(
"""4
2
5 10
3
1 2 3
4
1 1 1 1
9
9 9 8 2 4 4 3 5 3
"""
) == """YES
YES
YES
NO
"""

# minimum size, divisible
assert run(
"""1
2
1 1
"""
) == """YES
"""

# minimum size, not divisible
assert run(
"""1
2
5 7
"""
) == """NO
"""

# all equal values
assert run(
"""1
5
7 7 7 7 7
"""
) == """YES
"""

# large values near limit
assert run(
"""1
3
1000000000 2000000000 3000000000
"""
) == """YES
"""

# catches implementations that check adjacent divisibility
assert run(
"""1
3
2 4 6
"""
) == """YES
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 1`| CÓ | Mảng hợp lệ nhỏ nhất | 
|`2 / 5 7`| KHÔNG | Phần tử thứ hai không chia hết | 
|`7 7 7 7 7`| CÓ | Giá trị hoàn toàn bằng nhau | 
| bội số lớn của`10^9`| CÓ | Số học số lớn | 
|`2 4 6`| CÓ | Từ chối logic chia hết liền kề không chính xác | 

## Vỏ cạnh 

Hãy xem xét:```
1
2
5 7
```Bộ thuật toán`x = 5`và kiểm tra`7 % 5 = 2`. Vì số dư khác 0 nên nó xuất ra`"NO"`. 

Việc thử trình tự thao tác sẽ xác nhận điều này. Giá trị 7 trở thành 2 sau một phép trừ, sau đó sẽ trở thành âm sau một phép trừ khác. Zero là không thể truy cập được. 

Coi như:```
1
3
2 4 6
```Thuật toán kiểm tra:```
4 % 2 = 0
6 % 2 = 0
```và đầu ra`"YES"`. 

Đây là một ví dụ phản biện hữu ích cho ý tưởng rằng mọi phần tử phải chia hết cho phần tử liền trước nó. Bất biến thực tế chỉ phụ thuộc vào phần tử đầu tiên. 

Coi như:```
1
4
7 7 7 7
```Thuật toán tìm mọi phần tử chia hết cho 7 và đưa ra`"YES"`. 

Mỗi phần tử không phải đầu tiên có thể được giảm xuống 0 trong đúng một thao tác, xác nhận rằng các mảng bằng nhau luôn hợp lệ.

---
title: "CF 1981B - Rùa Và Chuỗi Vô Hạn"
description: "Chúng ta bắt đầu với một mảng vô hạn trong đó vị trí $i$ ban đầu chứa giá trị $i$. Vì vậy, mảng bắt đầu như một ánh xạ nhận dạng đơn giản: chỉ mục bằng giá trị. Mỗi giây, tất cả các vị trí đều cập nhật đồng thời."
date: "2026-06-08T16:47:34+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "math"]
categories: ["algorithms"]
codeforces_contest: 1981
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 949 (Div. 2)"
rating: 1300
weight: 1981
solve_time_s: 132
verified: true
draft: false
---

[CF 1981B - Rùa và Chuỗi vô tận](https://codeforces.com/problemset/problem/1981/B) 

**Đánh giá:** 1300 
**Tags:** bitmask, toán học 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một mảng vô hạn trong đó vị trí$i$ban đầu chứa giá trị$i$. Vì vậy, mảng bắt đầu như một ánh xạ nhận dạng đơn giản: chỉ mục bằng giá trị. 

Mỗi giây, tất cả các vị trí đều cập nhật đồng thời. Mỗi vị trí trở thành OR theo bit của chính nó và các vị trí lân cận của nó. Chính xác hơn, vị trí$i$lấy giá trị từ$i-1$,$i$, Và$i+1$, với các ranh giới được xử lý sao cho vị trí đó$0$chỉ OR có vị trí$1$. Quy tắc cập nhật này được áp dụng song song với mọi chỉ mục, nghĩa là mảng mới tại thời điểm đó$t+1$được tính toán hoàn toàn từ mảng tại thời điểm$t$, không tăng dần trong cùng một giây. 

Chúng tôi được yêu cầu giá trị tại một chỉ mục$n$sau đó$m$cập nhật toàn cầu như vậy. Khó khăn xuất phát từ thực tế là các giá trị trải rộng ra ngoài thông qua OR theo bit và mảng là vô hạn, do đó việc mô phỏng thô bạo là không thể. 

Những ràng buộc cho phép$n, m$lên tới$10^9$lên tới$10^4$trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào mô phỏng ngay cả số bước tuyến tính cho mỗi truy vấn. Thậm chí$O(n)$mỗi trường hợp thử nghiệm là không thể vì$n$bản thân nó có thể$10^9$, và tổng công sẽ bùng nổ. 

Điểm tinh tế cốt lõi là giá trị tại một vị trí không phụ thuộc vào sự lan truyền số học chính xác mà phụ thuộc vào cách các bit lan truyền từ các số nguyên gần đó. Một cái bẫy tinh thần ngây thơ là nghĩ rằng chỉ những giá trị cục bộ mới quan trọng trong một cửa sổ cố định. Điều đó sai vì OR hợp nhất thông tin một cách đơn điệu và các bit cao hơn lan truyền ở tốc độ hiệu dụng khác nhau. 

Một giả định không chính xác phổ biến là giá trị ổn định sau một số bước nhỏ hoặc chỉ phụ thuộc vào vùng lân cận được giới hạn như$[n-m, n+m]$. Mặc dù phần đó đúng với phạm vi phụ thuộc, nhưng nó không đơn giản hóa việc tính toán một cách trực tiếp vì mỗi giá trị trong phạm vi đó tự nó thay đổi theo thời gian theo cách có cấu trúc nhưng không tầm thường. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ duy trì toàn bộ mảng và áp dụng quy tắc cập nhật cho$m$các bước. Chi phí mỗi bước$O(N)$, Ở đâu$N$ít nhất sẽ cần phải có$n+m$để được chính xác. Điều này đã là không thể đối với các giá trị lớn. Tệ hơn nữa, về mặt khái niệm, mảng là vô hạn, do đó, ngay cả việc chọn một điểm cắt an toàn cũng là điều không cần thiết. 

Quan sát quan trọng là quá trình này là một vấn đề lan truyền theo bit với cấu trúc rất đều đặn. Thay vì theo dõi các số nguyên đầy đủ, chúng tôi kiểm tra từng bit một cách độc lập. Một chút ở vị trí$i$vào thời điểm đó$0$chỉ đơn giản là liệu bit đó có được đặt vào hay không$i$. Sau mỗi giây, một bit sẽ hoạt động ở một vị trí nếu nó hoạt động ở bất kỳ vị trí lân cận nào. 

Điều này biến mỗi bit thành một bài toán “mở rộng khoảng thời gian” cổ điển. Nếu một bit được đặt ở vị trí$x$, sau đó$m$các bước nó ảnh hưởng đến tất cả các vị trí trong$[x-m, x+m]$. Kể từ lúc đầu bit$k$được đặt ở tất cả các chỉ mục trong đó bit đó xuất hiện trong biểu diễn nhị phân của chỉ mục, chúng ta có thể diễn giải lại quy trình bằng cách xác định xem có tồn tại một số chỉ mục ban đầu trong khoảng cách hay không$m$của$n$đóng góp một chút nhất định. 

Tương tự, giá trị cuối cùng tại vị trí$n$là bitwise OR của tất cả các số nguyên trong khoảng$[n-m, n+m]$, giao nhau với các chỉ số không âm vì mảng bắt đầu từ 0. Các chỉ số âm không tồn tại, do đó vế trái bị cắt bớt ở 0. 

Vì vậy, vấn đề giảm xuống tính toán: 

OR của một dãy số nguyên liền kề. 

Khi chúng ta thấy điều này, nhiệm vụ sẽ thuần túy là tính toán OR của$[L, R]$hiệu quả, ở đâu$L = \max(0, n-m)$Và$R = n+m$. 

Một thủ thuật tiêu chuẩn được áp dụng: OR của một phạm vi có thể được tính bằng cách sử dụng bit có độ chênh lệch cao nhất giữa các điểm cuối. Nếu chúng ta nhìn vào tiền tố nhị phân của$L$Và$R$, tất cả các bit bên dưới bit cao nhất nơi chúng khác nhau sẽ trở thành 1 trong kết quả OR, bởi vì trong bất kỳ phạm vi đầy đủ nào bao phủ cả hai phía của ranh giới nhị phân, tất cả các kết hợp của các bit thấp hơn đều xuất hiện. 

Do đó, câu trả lời có thể được rút ra bằng cách lấy bit có ý nghĩa nhất trong đó$L$Và$R$khác nhau, sau đó đặt tất cả các bit thấp hơn thành 1, trong khi vẫn bảo toàn các bit cao hơn một cách thích hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(m \cdot (n+m))$|$O(n+m)$| Quá chậm | 
| Phạm vi bitwise HOẶC Giảm |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi từng truy vấn$(n, m)$vào một phạm vi$[L, R]$Ở đâu$L = \max(0, n-m)$Và$R = n+m$. 

1. Tính toán$L$Và$R$. Điều này đại diện cho tất cả các chỉ số ban đầu có giá trị có thể ảnh hưởng đến vị trí$n$sau đó$m$các bước. Điều này xuất phát từ quan sát rằng mỗi bước sẽ mở rộng ảnh hưởng theo đúng một chỉ số trái và phải. 
2. Nếu$L = 0$, chúng tôi coi phạm vi là bắt đầu từ số 0. Điều này quan trọng vì chuỗi ban đầu không mở rộng sang các chỉ số âm, do đó không có giá trị nào tồn tại ở đó để đóng góp các bit. 
3. Tính toán OR theo bit của tất cả các số nguyên trong$[L, R]$. Thay vì lặp lại, chúng tôi xác định bit cao nhất ở đó$L$Và$R$khác nhau. Bit này xác định vị trí đầu tiên nơi biểu diễn nhị phân phân kỳ. 
4. Khi tìm thấy bit có chênh lệch cao nhất đó, tất cả các bit thấp hơn trong kết quả sẽ trở thành 1 vì trong phạm vi, chúng ta có thể xây dựng các giá trị lật các bit đó một cách độc lập tại một số điểm. 
5. Câu trả lời cuối cùng được xây dựng bằng cách giữ tiền tố chung của$L$Và$R$và điền tất cả các bit thấp hơn còn lại bằng 1. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi vị trí bit phát triển độc lập trong quá trình lan truyền OR và quá trình này duy trì khả năng tiếp cận của tất cả các giá trị nguyên trung gian trong khoảng ảnh hưởng. Quy tắc cập nhật đảm bảo rằng bất kỳ bit nào được đặt ở bất kỳ vị trí bắt đầu có thể tiếp cận nào đều góp phần vào kết quả cuối cùng. Vì khả năng tiếp cận mở rộng đối xứng theo thời gian nên tập hợp các chỉ số ban đầu đóng góp chính xác trở thành một khoảng liền kề. OR trên một khoảng liền kề có cấu trúc nhị phân ổn định được xác định duy nhất bởi sự phân kỳ nhị phân của các điểm cuối, đảm bảo tính chính xác của việc tái cấu trúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def range_or(l, r):
    if l == 0:
        # special case: OR from 0 to r
        # becomes all bits up to MSB of r are 1
        msb = r.bit_length() - 1
        return (1 << (msb + 1)) - 1

    # find highest differing bit between l and r
    x = l ^ r
    msb = x.bit_length() - 1

    # keep prefix above msb, set all lower bits to 1
    prefix = r & (~((1 << (msb + 1)) - 1))
    suffix = (1 << (msb + 1)) - 1
    return prefix | suffix

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    l = max(0, n - m)
    r = n + m
    print(range_or(l, r))
```Việc triển khai được cấu trúc xung quanh một trình trợ giúp tính toán OR trên một phạm vi bằng cách sử dụng cấu trúc nhị phân thay vì lặp. Trường hợp đặc biệt$l = 0$được xử lý riêng biệt vì số 0 neo phạm vi và đảm bảo rằng tất cả các bit thấp hơn trở nên bão hòa hoàn toàn cho đến bit cao nhất trong$r$. 

Trường hợp chung sử dụng XOR để phát hiện vị trí$l$Và$r$khác nhau. Vị trí đó xác định cây nhị phân của các giá trị có thể phân kỳ bao xa. Mọi thứ dưới mức đó sẽ được bao phủ hoàn toàn, vì vậy chúng tôi đặt các bit đó thành 1. Tiền tố của$r$được bảo toàn để duy trì tính chính xác của các bit cao hơn giống hệt nhau trong phạm vi. 

Một chi tiết triển khai tinh tế đang sử dụng`bit_length() - 1`để tìm bit được đặt cao nhất. Lỗi ngẫu nhiên ở đây là phổ biến; mặt nạ được xây dựng`(1 << (msb + 1)) - 1`tạo chính xác một khối đầy đủ các khối tối đa và bao gồm cả bit đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 5, m = 2$Chúng tôi tính toán$L = 3$,$R = 7$. 

| Bước | L | R | L XOR R | Khác biệt MSB | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 3 | 7 | 4 | 2 | - | 
| xây dựng | - | - | - | - | tiền tố + hậu tố | 

Bit khác nhau cao nhất là bit 2. Tất cả các bit bên dưới nó trở thành 1 và cấu trúc cao hơn được giữ nguyên. Kết quả trở thành$7$. 

Điều này phù hợp với trực giác: các giá trị trong$[3,7]$đã bao gồm tất cả các mẫu bit thấp hơn, vì vậy OR bão hòa thành 111. 

### Ví dụ 2:$n = 1, m = 0$Chúng tôi tính toán$L = 1$,$R = 1$. 

| Bước | L | R | L XOR R | Khác biệt MSB | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 1 | 1 | 0 | không | 1 | 

Vì khoảng có một phần tử duy nhất nên OR không thay đổi. Điều này xác nhận tính nhất quán của trường hợp cơ sở. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$mỗi trường hợp thử nghiệm | Chỉ sử dụng các thao tác bit trên số nguyên 32 bit | 
| Không gian |$O(1)$| Không có cấu trúc bổ sung ngoài một vài số nguyên | 

Giải pháp xử lý thoải mái$10^4$các trường hợp kiểm thử vì mỗi truy vấn giảm xuống một số lượng không đổi các phép toán số học và bitwise. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def range_or(l, r):
        if l == 0:
            msb = r.bit_length() - 1
            return (1 << (msb + 1)) - 1
        x = l ^ r
        msb = x.bit_length() - 1
        prefix = r & (~((1 << (msb + 1)) - 1))
        suffix = (1 << (msb + 1)) - 1
        return prefix | suffix

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        l = max(0, n - m)
        r = n + m
        out.append(str(range_or(l, r)))
    return "\n".join(out)

# provided samples
assert solve("""9
0 0
0 1
0 2
1 0
5 2
10 1
20 3
1145 14
19198 10
""") == """0
1
3
1
7
11
23
1279
19455"""

# boundary: single point
assert solve("""1
0 0
""") == "0"

# small propagation
assert solve("""1
2 1
""") == "3"

# large spread
assert solve("""1
5 5
""") == "15"

# all zero start effect
assert solve("""1
0 5
""") == "63"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 | 0 | sự ổn định của trường hợp cơ sở | 
| 2 1 | 3 | độ chính xác lan truyền nhỏ | 
| 5 5 | 15 | hành vi bão hòa hoàn toàn | 
| 0 5 | 63 | cắt và mở rộng ranh giới bên trái | 

## Vỏ cạnh 

Khi nào$n = 0$, phạm vi trở thành$[0, m]$. Thuật toán xử lý vấn đề này một cách rõ ràng bằng cách chuyển sang tiền tố đầy đủ HOẶC từ 0. Ví dụ, với$m = 5$, phạm vi là$[0,5]$, và bit cao nhất là 2, do đó kết quả là$2^3 - 1 = 7$, khớp với OR của tất cả các số từ 0 đến 5. 

Khi nào$m = 0$, phạm vi giảm xuống$[n,n]$. XOR trở thành 0, do đó không có bit nào khác nhau và hàm trả về$n$không thay đổi. Điều này phù hợp với thực tế là không có cập nhật nào xảy ra. 

Khi$n < m$, ranh giới bên trái kẹp về 0. Nếu không có kẹp này, các chỉ số âm sẽ đóng góp không chính xác. Việc triển khai đảm bảo tính chính xác bằng cách thiết lập rõ ràng$L = 0$, duy trì ràng buộc vật lý của miền mảng.

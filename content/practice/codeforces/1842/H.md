---
title: "CF 1842H - Số thực ngẫu nhiên và số thực ngẫu nhiên"
description: "Chúng ta có $n$ biến ngẫu nhiên độc lập, mỗi biến được chọn thống nhất từ ​​khoảng $[0,1]$. Giữa các cặp biến, chúng ta có các ràng buộc có dạng $$xi+xjle 1$$ hoặc $$xi+xjge 1.$$ Nhiệm vụ là tính xác suất để tất cả các ràng buộc giữ đồng thời."
date: "2026-06-09T06:16:39+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "graphs", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1842
codeforces_index: "H"
codeforces_contest_name: "CodeTON Round 5 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 3000
weight: 1842
solve_time_s: 115
verified: false
draft: false
---

[CF 1842H - Số thực ngẫu nhiên và số thực ngẫu nhiên](https://codeforces.com/problemset/problem/1842/H) 

**Đánh giá:** 3000 
**Tags:** bitmasks, dp, đồ thị, toán học, xác suất 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

chúng tôi có$n$các biến ngẫu nhiên độc lập, mỗi biến được chọn thống nhất từ ​​khoảng$[0,1]$. Giữa các cặp biến chúng ta được đưa ra các ràng buộc có dạng$$x_i+x_j\le 1$$hoặc$$x_i+x_j\ge 1.$$Nhiệm vụ là tính xác suất để tất cả các ràng buộc được giữ đồng thời. 

Quan sát quan trọng nhất là các biến là liên tục. Sự bình đẳng như$x_i+x_j=1$xảy ra với xác suất$0$, vì vậy chúng ta có thể tự do suy luận bằng cách sử dụng các bất đẳng thức chặt chẽ bất cứ khi nào nó đơn giản hóa lập luận. 

Ràng buộc$n\le 20$ngay lập tức gợi ý rằng các thuật toán hàm mũ trong$n$là có thể. Không gian trạng thái có kích thước$2^n$là khoảng một triệu, điều này là khả thi. Bất cứ điều gì liên quan đến tất cả các hoán vị của các biến sẽ yêu cầu$20!$khả năng và là vô vọng. 

Một điểm tinh tế là các ràng buộc không so sánh các biến riêng lẻ. Họ so sánh số tiền liên quan đến$1$. Cố gắng rời rạc hóa các giá trị của các biến hoặc lấy tích phân trực tiếp trên$[0,1]^n$nhanh chóng trở nên khó chữa. 

Một sai lầm dễ mắc phải khác là xử lý sai các vòng lặp tự thân. 

Ví dụ:```
1 1
1 1 1
```Hạn chế là$2x_1\ge 1$, có nghĩa là$x_1\ge \tfrac12$. Câu trả lời là$\tfrac12$, không$1$. 

Tương tự:```
1 1
0 1 1
```yêu cầu$x_1\le \tfrac12$, một lần nữa xác suất$\tfrac12$. 

Một cái bẫy khác là cấu trúc mâu thuẫn được tạo ra một cách gián tiếp.```
4 4
0 1 2
0 3 4
1 1 3
1 2 4
```Đây là mẫu thứ tư. Các ràng buộc buộc phải thực hiện một chu kỳ các yêu cầu về dấu không thể được thỏa mãn, vì vậy câu trả lời là$0$. Kiểm tra cục bộ trên mỗi cạnh là không đủ. 

## Phương pháp tiếp cận 

Quan điểm vũ phu là coi xác suất là thể tích của một vùng bên trong khối đơn vị$[0,1]^n$. Mọi ràng buộc đều cắt khối lập phương bằng một siêu phẳng. Việc tính toán chính xác thể tích thu được sẽ yêu cầu tích phân trên một đa giác phức tạp. Ngay cả đối với$n=20$, điều này hoàn toàn không thực tế. 

Quan sát chính xuất phát từ việc viết lại các biến xung quanh điểm giữa$1/2$. 

Định nghĩa$$y_i=x_i-\frac12.$$Mỗi$y_i$được phân bố đều trên$\left[-\frac12,\frac12\right]$. 

Bây giờ mọi ràng buộc đều trở thành$$y_i+y_j\le 0$$hoặc$$y_i+y_j\ge 0.$$Giả định$|y_i|<|y_j|$. 

Nếu như$y_i+y_j\le 0$, dấu của tổng được xác định bởi số hạng có độ lớn lớn hơn$y_j$. Từ$|y_j|$chiếm ưu thế, sự bất bình đẳng tương đương với việc yêu cầu$y_j<0$. 

Tương tự, nếu$y_i+y_j\ge 0$, nó tương đương với yêu cầu$y_j>0$. 

Đây là bước đột phá. Đối với mọi cạnh, chỉ điểm cuối có giá trị tuyệt đối lớn hơn mới quan trọng. 

Vì vậy, thay vì làm việc với các giá trị thực tế, chúng ta chỉ cần: 

1. Thứ tự các giá trị tuyệt đối$|y_i|$. 
2. Dấu hiệu của mỗi$y_i$. 

Khi những điều đó đã được sửa, mọi ràng buộc đều được thỏa mãn hoặc bị vi phạm. 

Các giá trị tuyệt đối là các biến thống nhất độc lập. Mọi thứ tự độ lớn của chúng đều xảy ra với xác suất$1/n!$. Dấu của mỗi biến độc lập dương hoặc âm với xác suất$1/2$. 

Như vậy bài toán trở thành: 

Đếm số cặp hợp lệ$$(\text{ordering of }|y|,\ \text{sign assignment})$$và chia cho$$2^n n!.$$Việc đếm có thể được thực hiện với tập con DP theo thứ tự độ lớn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tích hợp hình học Brute Force | Hàm mũ trong chiều liên tục | Rất lớn | Quá chậm | 
| Bitmask DP theo thứ tự cường độ |$O(n2^n)$|$O(2^n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1 

Biến đổi mọi biến:$$y_i=x_i-\frac12.$$Những ràng buộc trở thành$y_i+y_j\le 0$Và$y_i+y_j\ge 0$. 

### Bước 2 

Đối với mỗi biến, lưu trữ hai bitmask.`LE[i]`chứa tất cả các đỉnh nối với$i$bởi một hạn chế$y_i+y_j\le 0$.`GE[i]`chứa tất cả các đỉnh nối với$i$bởi một hạn chế$y_i+y_j\ge 0$. 

Vòng lặp tự được bao gồm một cách tự nhiên. 

### Bước 3 

Xử lý các biến theo thứ tự tăng dần$|y|$. 

Cho phép$S$là tập hợp các đỉnh đã được đặt. Mỗi đỉnh trong$S$có giá trị tuyệt đối nhỏ hơn mọi đỉnh bên ngoài$S$. 

Giả sử chúng ta muốn đặt đỉnh$i$Kế tiếp. 

Sau đó$i$trở thành cường độ lớn nhất trong số$S\cup\{i\}$. 

### Bước 4 

Đếm xem có thể chọn bao nhiêu dấu hiệu$i$. 

Nếu như$i$là dương thì mọi ràng buộc thuộc loại$\ge 0$sự cố xảy ra$i$phải hướng về một đỉnh vẫn chưa được đặt. Nếu không thì$i$sẽ là điểm cuối có cường độ lớn hơn và sẽ bị cạnh đó buộc phải dương. 

Vì vậy, tích cực được cho phép nếu$$GE[i]\cap(S\cup\{i\})=\varnothing.$$Nếu điều này được giữ, hãy thêm một chuyển tiếp. 

Tương tự, cho phép phủ định nếu$$LE[i]\cap(S\cup\{i\})=\varnothing.$$Nếu điều này được giữ, hãy thêm một quá trình chuyển đổi khác. 

Mỗi dấu hiệu được phép đóng góp chính xác một cách. 

### Bước 5 

Chạy một tập hợp con DP tiêu chuẩn. 

Cho phép$$dp[S]$$là số cách dựng hợp lệ của thứ tự và dấu hiệu của các đỉnh trong$S$. 

Khởi tạo$$dp[\varnothing]=1.$$Đối với mọi trạng thái và mọi đỉnh chưa được chọn, hãy áp dụng hai phép kiểm tra ở trên và cập nhật$dp[S\cup\{i\}]$. 

### Bước 6 

Sau khi hoàn tất,$dp[(1<<n)-1]$bằng số cặp hợp lệ$$(\text{ordering},\text{sign assignment}).$$Mọi phép gán dấu đều có xác suất$1/2^n$. 

Mọi thứ tự của các giá trị tuyệt đối đều có xác suất$1/n!$. 

Do đó câu trả lời là$$\frac{dp[(1<<n)-1]}{2^n n!}.$$Tất cả số học được thực hiện modulo$998244353$. 

### Tại sao nó hoạt động 

Đối với bất kỳ ràng buộc nào, chỉ điểm cuối có giá trị tuyệt đối lớn hơn mới xác định được tổng là dương hay âm. Khi thứ tự của các giá trị tuyệt đối được cố định, mỗi cạnh sẽ áp đặt yêu cầu về dấu trên chính xác một điểm cuối, cụ thể là điểm cuối sau theo thứ tự đó. 

DP xây dựng thứ tự từ cường độ nhỏ nhất đến lớn nhất. Khi đỉnh$i$được chèn vào, tất cả các đỉnh trong$S$có độ lớn nhỏ hơn. Bất kỳ cạnh nào giữa$i$và một đỉnh của$S$đã xác định được dấu hiệu nào$i$phải có. Hai thử nghiệm mặt nạ bit kiểm tra chính xác xem vẫn có thể dương tính hay âm tính. 

Mọi thứ tự và ký hiệu hợp lệ đều được tính chính xác một lần và mọi cấu hình được tính đều thỏa mãn tất cả các ràng buộc. Vì thứ tự dấu và độ lớn được phân bố đồng đều, chia cho$2^n n!$chuyển đổi số đếm thành xác suất mong muốn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def main():
    n, m = map(int, input().split())

    LE = [0] * n
    GE = [0] * n

    for _ in range(m):
        t, i, j = map(int, input().split())
        i -= 1
        j -= 1

        if t == 0:
            LE[i] |= 1 << j
            LE[j] |= 1 << i
        else:
            GE[i] |= 1 << j
            GE[j] |= 1 << i

    N = 1 << n
    dp = [0] * N
    dp[0] = 1

    for S in range(N):
        cur = dp[S]
        if cur == 0:
            continue

        for i in range(n):
            if (S >> i) & 1:
                continue

            T = S | (1 << i)

            if (GE[i] & T) == 0:
                dp[T] = (dp[T] + cur) % MOD

            if (LE[i] & T) == 0:
                dp[T] = (dp[T] + cur) % MOD

    ans = dp[N - 1]

    inv2 = pow(2, MOD - 2, MOD)
    for k in range(1, n + 1):
        ans = ans * pow(k, MOD - 2, MOD) % MOD
        ans = ans * inv2 % MOD

    print(ans)

if __name__ == "__main__":
    main()
```Hai chiếc mặt nạ`LE`Và`GE`mã hóa toàn bộ đồ thị ràng buộc. Vòng lặp tự động sẽ tự động đặt đỉnh vào trong mặt nạ của chính nó, đó chính xác là những gì chúng ta muốn. Ví dụ, một hạn chế$x_i+x_i\ge 1$trở thành một bit tự thân trong`GE[i]`, làm cho quá trình chuyển đổi tiêu cực không thể thực hiện được. 

Trạng thái DP chỉ lưu trữ tập con của các đỉnh đã được xử lý. Không cần thêm thông tin vì thứ tự được xây dựng lại từ thứ tự các đỉnh được chèn vào. 

biểu hiện`GE[i] & T`kiểm tra xem có bị cấm không`>=`cạnh đã trỏ đến một đỉnh có độ lớn nhỏ hơn. Nếu đúng như vậy, việc chọn dấu dương sẽ vi phạm một ràng buộc. 

Cuối cùng, chúng tôi nhân với nghịch đảo mô-đun của$2$Và$1,2,\dots,n$chia cho$2^n n!$. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 2
0 1 2
1 3 3
```Các mặt nạ là: 

| Đỉnh | LÊ | GE | 
| --- | --- | --- | 
| 1 | {2} | {} | 
| 2 | {1} | {} | 
| 3 | {} | {3} | 

Chuyển tiếp DP: 

| Tiểu bang | Đã thêm đỉnh | Biển báo được phép | 
| --- | --- | --- | 
| {} | 1 | + và - | 
| {} | 2 | + và - | 
| {} | 3 | chỉ + | 
| ... | ... | ... | 

Số đếm cuối cùng là$6$. 

có$3!\cdot 2^3 = 48$các cặp thứ tự/ký có khả năng ngang nhau. Như vậy$$\frac{6}{48}=\frac14.$$Modulo$998244353$, đây là`748683265`. 

Ví dụ này cho thấy cách các vòng lặp tự hạn chế dấu của một biến duy nhất mà không ảnh hưởng đến logic thứ tự. 

### Mẫu 2 

đầu vào:```
3 3
0 1 2
0 1 3
0 2 3
```Mỗi cạnh đều thuộc loại$\le 0$. 

Đối với bất kỳ cặp nào chúng tôi kiểm tra, điểm cuối có cường độ lớn hơn phải âm. 

Do đó, biến cường độ lớn nhất tổng thể phải âm. Hai biến còn lại có thể có một trong hai dấu. 

| Đỉnh có độ lớn lớn nhất | Dấu hiệu bắt buộc | 
| --- | --- | 
| 1 | tiêu cực | 
| 2 | tiêu cực | 
| 3 | tiêu cực | 

DP đếm tất cả các cặp thứ tự/ký hiệu hợp lệ và một lần nữa tạo ra xác suất$1/4$. 

Ví dụ này minh họa nguyên tắc trung tâm rằng chỉ điểm cuối có cường độ lớn hơn mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n2^n)$| Mọi tập hợp con đều thử tất cả các đỉnh chưa được chọn | 
| Không gian |$O(2^n)$| Một giá trị DP cho mỗi tập hợp con | 

Với$n\le 20$, chúng ta có nhiều nhất$2^{20}=1,048,576$tiểu bang. Độ phức tạp thu được nằm trong giới hạn, đặc biệt là trong DP mặt nạ bit có hằng số thấp. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    MOD = 998244353

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m = map(int, input().split())

    LE = [0] * n
    GE = [0] * n

    for _ in range(m):
        t, i, j = map(int, input().split())
        i -= 1
        j -= 1

        if t == 0:
            LE[i] |= 1 << j
            LE[j] |= 1 << i
        else:
            GE[i] |= 1 << j
            GE[j] |= 1 << i

    N = 1 << n
    dp = [0] * N
    dp[0] = 1

    for S in range(N):
        for i in range(n):
            if (S >> i) & 1:
                continue

            T = S | (1 << i)

            if (GE[i] & T) == 0:
                dp[T] = (dp[T] + dp[S]) % MOD

            if (LE[i] & T) == 0:
                dp[T] = (dp[T] + dp[S]) % MOD

    ans = dp[N - 1]
    inv2 = pow(2, MOD - 2, MOD)

    for k in range(1, n + 1):
        ans = ans * pow(k, MOD - 2, MOD) % MOD
        ans = ans * inv2 % MOD

    return str(ans)

# provided sample
assert run(
"""3 2
0 1 2
1 3 3
"""
) == "748683265"

# x1 >= 1/2
assert run(
"""1 1
1 1 1
"""
) == str((pow(2, 998244351, 998244353)))

# x1 <= 1/2
assert run(
"""1 1
0 1 1
"""
) == str((pow(2, 998244351, 998244353)))

# no constraints, probability = 1
assert run(
"""1 0
"""
) == "1"

# impossible configuration from sample 4
assert run(
"""4 4
0 1 2
0 3 4
1 1 3
1 2 4
"""
) == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn, không có ràng buộc | 1 | Biểu đồ trống | 
|$x_1+x_1\ge1$|$1/2$| Xử lý tự vòng lặp | 
|$x_1+x_1\le1$|$1/2$| Loại tự vòng đối diện | 
| Mẫu 1 |$1/4$| Các loại cạnh hỗn hợp | 
| Mẫu 4 | 0 | Mâu thuẫn toàn cầu | 

## Vỏ cạnh 

Hãy xem xét:```
1 1
1 1 1
```Vòng lặp tự được lưu trữ bên trong`GE[1]`. Khi DP cố gắng đặt đỉnh có dấu âm, điều kiện$$GE[1]\cap\{1\}\neq\varnothing$$chặn quá trình chuyển đổi đó. Chỉ còn lại dấu tích cực. Số DP trở thành$1$, và chia cho$2$xác suất sản lượng$1/2$. 

Bây giờ hãy xem xét:```
1 1
0 1 1
```Lý do tương tự áp dụng với`LE`. Chỉ cho phép dấu âm, một lần nữa tạo ra xác suất$1/2$. 

Cuối cùng, hãy xem xét:```
4 4
0 1 2
0 3 4
1 1 3
1 2 4
```Đỉnh nào trong số$\{1,3\}$có độ lớn lớn hơn phải dương. Đỉnh nào trong số$\{1,2\}$có độ lớn lớn hơn phải âm. Việc theo đuổi những yêu cầu này xung quanh biểu đồ sẽ tạo ra sự mâu thuẫn. Trong quá trình DP, mọi quá trình xây dựng một phần cuối cùng đều bị kẹt, do đó số đếm cuối cùng bằng 0 và thuật toán xuất ra$0$. Điều này chứng tỏ rằng DP tự động phát hiện sự không nhất quán toàn cầu thay vì dựa vào kiểm tra cục bộ.

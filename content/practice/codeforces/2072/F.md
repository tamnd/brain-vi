---
title: "CF 2072F - Tạm biệt cuộc đời nhân viên ngân hàng"
description: "Chúng ta bắt đầu với một tam giác có hàng đầu tiên chứa một giá trị k. Mỗi hàng sau được xây dựng từ hàng trước. Hai phần tử viền được sao chép từ hàng bên trên, trong khi mọi phần tử bên trong trở thành XOR của hai phần tử liền kề phía trên nó."
date: "2026-06-08T06:48:30+07:00"
tags: ["codeforces", "competitive-programming", "2-sat", "bitmasks", "combinatorics", "constructive-algorithms", "fft", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2072
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1006 (Div. 3)"
rating: 1700
weight: 2072
solve_time_s: 91
verified: true
draft: false
---

[CF 2072F - Tạm biệt cuộc đời nhân viên ngân hàng](https://codeforces.com/problemset/problem/2072/F) 

**Đánh giá:** 1700 
**Tags:** 2-sat, bitmasks, tổ hợp, thuật toán xây dựng, fft, toán, lý thuyết số 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một hình tam giác có hàng đầu tiên chứa một giá trị`k`. 

Mỗi hàng sau được xây dựng từ hàng trước. Hai phần tử viền được sao chép từ hàng bên trên, trong khi mọi phần tử bên trong trở thành XOR của hai phần tử liền kề phía trên nó. 

Nếu chúng ta biểu thị tam giác bằng`T`, sau đó chèo`n`chứa`n`những con số. Nhiệm vụ là xuất toàn bộ`n`-ném. 

Thoạt nhìn nó trông rất giống tam giác Pascal. Sự khác biệt duy nhất là phép cộng đã được thay thế bằng XOR. Sự giống nhau đó chính là chìa khóa để giải quyết vấn đề. 

Các ràng buộc đủ lớn nên việc xây dựng toàn bộ tam giác một cách rõ ràng là không thể. Một trường hợp thử nghiệm duy nhất có thể yêu cầu`n = 10^6`. Xây dựng tất cả các hàng lên đến mức đó sẽ yêu cầu khoảng$$1+2+\cdots+n = O(n^2)$$hoạt động, trở thành về$5 \cdot 10^{11}$cập nhật khi nào$n=10^6$. Điều đó hoàn toàn ngoài tầm với. 

Tổng của tất cả độ dài hàng được yêu cầu tối đa là$10^6$. Điều này cho chúng ta biết một điều quan trọng: một$O(n)$giải pháp cho mỗi trường hợp thử nghiệm có thể chấp nhận được vì tổng công việc trên tất cả các trường hợp thử nghiệm vẫn tuyến tính. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Coi như`n = 1`. 

đầu vào:```
1 52
```Đầu ra:```
52
```Không có sự lặp lại để áp dụng. Câu trả lời chỉ đơn giản là hàng đầu tiên. 

Một trường hợp tinh vi khác là khi chỉ số hàng được yêu cầu là lũy thừa của hai cộng một. 

Ví dụ:```
n = 9, k = 1
```Câu trả lời là:```
1 0 0 0 0 0 0 0 1
```Ở đây, nỗ lực ngây thơ nhằm tìm kiếm các mẫu số trong các hàng nhỏ thường thất bại. Cấu trúc đúng xuất phát từ tính chất chẵn lẻ của các hệ số nhị thức, chứ không phải từ bản thân các giá trị. 

Cạm bẫy cuối cùng là giả sử câu trả lời phụ thuộc vào các bit của`k`. Toàn bộ hàng chỉ được xác định bằng việc một hệ số nhất định là số lẻ hay số chẵn. Mọi mục nhập khác 0 đều chính xác`k`, không bao giờ là một phiên bản biến đổi nào đó của nó. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng sự tái phát. 

Bắt đầu từ`[k]`, liên tục xây dựng hàng tiếp theo. Mỗi vị trí bên trong là XOR của hai giá trị lân cận từ hàng trước và các đường viền được sao chép. 

Cách tiếp cận này đúng vì nó tuân theo định nghĩa một cách chính xác. Thật không may, nó yêu cầu xây dựng tất cả các hàng từ`1`bởi vì`n`. Tổng khối lượng công việc là$$1+2+\cdots+n = O(n^2).$$Vì`n = 10^6`, như vậy là quá chậm. 

Sự truy hồi rất giống với tam giác Pascal. Trong tam giác Pascal, phần tử ở hàng`r`và cột`c`bằng$$\binom{r}{c}.$$Thay thế phép cộng bằng XOR có nghĩa là chúng ta đang làm việc hiệu quả theo modulo 2. Vì XOR là phép cộng trên trường GF(2), nên mỗi vị trí trong hàng`n`bằng với$$k \cdot \left(\binom{n-1}{j-1}\bmod 2\right).$$Nếu hệ số nhị thức là số lẻ thì giá trị là`k`. Nếu nó chẵn thì giá trị là`0`. 

Vấn đề bây giờ được giảm xuống để xác định liệu$$\binom{n-1}{j-1}$$thật kỳ quặc. 

Một hệ quả cổ điển của định lý Lucas phát biểu rằng$$\binom{a}{b}$$là số lẻ chính xác khi mọi bit được đặt vào`b`cũng được thiết lập trong`a`. Ở dạng bitwise:$$(b \,\&\, \sim a)=0.$$Tương đương,$$(a \,\&\, b)=b.$$Cho phép$$m=n-1.$$Đối với vị trí`j`, định nghĩa$$r=j-1.$$Sau đó, mục nhập là`k`nếu như`(r & m) == r`, nếu không thì`0`. 

Giờ đây, mỗi phần tử có thể được tạo độc lập trong thời gian không đổi, đưa ra giải pháp tuyến tính tổng thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 

|---|------|---| 

| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 

| Tối ưu | O(n) | O(1) không bao gồm đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`k`. 
2. Tính toán`m = n - 1`. 

Mỗi hệ số liên tiếp`n`tương ứng với một hệ số nhị thức từ hàng`m`của tam giác Pascal. 
3. Đối với từng vị trí`j`từ`0`ĐẾN`n-1`, kiểm tra xem`(j & m) == j`. 

Đây chính xác là điều kiện mà`C(m, j)`thật kỳ quặc. 
4. Nếu điều kiện đúng, hãy nối thêm`k`để trả lời. 

Hệ số lẻ có nghĩa là một bản sao của`k`tồn tại sau khi tích lũy XOR. 
5. Nếu không thì nối thêm`0`. 

Một hệ số chẵn bị loại bỏ theo XOR. 
6. Xuất hàng được tạo. 

### Tại sao nó hoạt động 

Mọi phần tử của tam giác được hình thành bằng cách áp dụng nhiều lần XOR, đó là phép cộng modulo 2. Do đó, phép truy toán giống hệt với tam giác Pascal được tính trên GF(2). 

Hệ số giá trị ban đầu`k`ở vị trí`j`liên tiếp`n`chính xác là`C(n-1, j)`. Vì XOR chỉ quan tâm đến tính chẵn lẻ nên hệ số lẻ đóng góp một bản sao của`k`, trong khi hệ số chẵn không đóng góp gì. 

Định lý Lucas cho chúng ta biết rằng`C(m, j)`là số lẻ một cách chính xác khi mỗi tập bit của`j`cũng có mặt ở`m`. Kiểm tra bitwise`(j & m) == j`kiểm tra chính xác điều kiện đó. Do đó, mỗi vị trí được phân loại chính xác thành`k`hoặc`0`, chứng minh tính đúng đắn của thuật toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, k = map(int, input().split())
        m = n - 1

        row = []
        for r in range(n):
            if (r & m) == r:
                row.append(str(k))
            else:
                row.append("0")

        out.append(" ".join(row))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo đặc tính toán học một cách trực tiếp. 

Biến`m = n - 1`tương ứng với chỉ số hàng tam giác Pascal. Đối với mọi vị trí`r`, đại diện cho`j - 1`trong lập chỉ mục dựa trên một, chúng tôi kiểm tra xem tất cả các bit của`r`được chứa bên trong`m`. 

Nếu như`(r & m) == r`, hệ số nhị thức tương ứng là số lẻ nên giá trị là`k`. Nếu không thì nó là`0`. 

Không có hình tam giác nào được xây dựng. Mã tạo hàng được yêu cầu trực tiếp theo thời gian tuyến tính. 

Lỗi triển khai phổ biến nhất là trộn lẫn các chỉ số dựa trên một và dựa trên 0. Việc kiểm tra tính chẵn lẻ phải được áp dụng cho`r = j - 1`, đó chính xác là những gì biến vòng lặp đại diện. 

Một lỗi phổ biến khác là cố gắng tính toán các hệ số nhị thức một cách rõ ràng. Giá trị của chúng trở nên to lớn từ lâu trước khi`n`đạt đến giới hạn và phép kiểm tra tính chẵn lẻ sẽ tránh được tất cả các tính toán như vậy. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, k = 16
```Sau đó`m = n - 1 = 2`. 

| r | Nhị phân r | m | (r & m) == r | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 0 | 00 | 10 | Có | 16 | 
| 1 | 01 | 10 | Không | 0 | 
| 2 | 10 | 10 | Có | 16 | 

Kết quả:```
16 0 16
```Ví dụ này cho thấy chỉ những vị trí có hệ số nhị thức lẻ mới tồn tại được. 

### Ví dụ 2 

đầu vào:```
n = 9, k = 1
```Sau đó`m = 8`, có biểu diễn nhị phân là`1000`. 

| r | Nhị phân r | Tình trạng | 
| --- | --- | --- | 
| 0 | 0000 | Đúng | 
| 1 | 0001 | Sai | 
| 2 | 0010 | Sai | 
| 3 | 0011 | Sai | 
| 4 | 0100 | Sai | 
| 5 | 0101 | Sai | 
| 6 | 0110 | Sai | 
| 7 | 0111 | Sai | 
| 8 | 1000 | Đúng | 

Đầu ra:```
1 0 0 0 0 0 0 0 1
```Điều này chứng tỏ cấu trúc lũy thừa hai xuất hiện một cách tự nhiên từ định lý Lucas. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một thử nghiệm bit thời gian không đổi cho mỗi phần tử đầu ra | 
| Không gian | O(1) không bao gồm đầu ra | Chỉ có một vài biến được lưu trữ | 

Bởi vì tổng của tất cả`n`giá trị trên các trường hợp thử nghiệm là nhiều nhất`10^6`, tổng số lần kiểm tra bit được thực hiện cũng nhiều nhất`10^6`. Điều này dễ dàng phù hợp với giới hạn thời gian và mức sử dụng bộ nhớ vẫn ở mức tối thiểu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string

import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, k = map(int, input().split())
        m = n - 1

        row = []
        for r in range(n):
            row.append(str(k if (r & m) == r else 0))

        out.append(" ".join(row))

    return "\n".join(out)

# provided sample
assert run(
"""5
1 5
2 10
3 16
9 1
1 52
"""
) == (
"""5
10 10
16 0 16
1 0 0 0 0 0 0 0 1
52"""
)

# minimum size
assert run(
"""1
1 7
"""
) == "7"

# n = 2
assert run(
"""1
2 11
"""
) == "11 11"

# power-of-two row index
assert run(
"""1
5 3
"""
) == "3 0 0 0 3"

# all positions survive
assert run(
"""1
4 9
"""
) == "9 9 9 9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, k=7`|`7`| Hàng nhỏ nhất có thể | 
|`n=2, k=11`|`11 11`| Tuyên truyền biên giới | 
|`n=5, k=3`|`3 0 0 0 3`| Cấu trúc sức mạnh của hai | 
|`n=4, k=9`|`9 9 9 9`| Mọi hệ số đều lẻ khi`n-1=3`| 

## Vỏ cạnh 

### Trường hợp cạnh 1: Hàng một phần tử 

đầu vào:```
1
1 52
```Thuật toán tính toán`m = 0`. 

Vì`r = 0`:```
(0 & 0) == 0
```là đúng, vì vậy đầu ra là:```
52
```Không cần xử lý đặc biệt. 

### Trường hợp cạnh 2: Biên lũy thừa hai hàng 

đầu vào:```
1
9 1
```Đây`m = 8`, nhị phân`1000`. 

Chỉ một`r = 0`Và`r = 8`thỏa mãn`(r & m) == r`. 

Các đầu ra thuật toán:```
1 0 0 0 0 0 0 0 1
```Điều này phát hiện các triển khai giả định không chính xác các hàng trong tam giác Pascal vẫn dày đặc. 

### Trường hợp Edge 3: Mọi hệ số lẻ 

đầu vào:```
1
4 9
```Đây`m = 3`, nhị phân`11`. 

Mọi`r`TRONG`{0,1,2,3}`chỉ sử dụng các bit đã có trong`m`, vậy mọi vị trí đều thỏa mãn điều kiện. 

Đầu ra:```
9 9 9 9
```Điều này xác minh rằng thuật toán xử lý các hàng không xảy ra việc hủy trong XOR.

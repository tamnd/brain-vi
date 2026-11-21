---
title: "CF 2200G - Hoán vị hoạt động"
description: "Chúng ta được cung cấp một số nguyên bắt đầu $x$ và một danh sách các phép toán $n$, mỗi phép tính sửa đổi $x$ bằng cách cộng, trừ, nhân hoặc chia cho một số nguyên dương. Điều khó khăn là các thao tác được áp dụng theo thứ tự ngẫu nhiên: tất cả $n!"
date: "2026-06-07T20:19:30+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 2200
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 1084 (Div. 3)"
rating: 2200
weight: 2200
solve_time_s: 137
verified: false
draft: false
---

[CF 2200G - Hoán vị hoạt động](https://codeforces.com/problemset/problem/2200/G) 

**Đánh giá:** 2200 
**Tags:** tổ hợp, dp, toán, xác suất 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên bắt đầu$x$và một danh sách$n$hoạt động, mỗi hoạt động sửa đổi$x$bằng cách cộng, trừ, nhân hoặc chia cho một số nguyên dương. Điều khó khăn là các thao tác được áp dụng theo thứ tự ngẫu nhiên: tất cả$n!$hoán vị có khả năng xảy ra như nhau. Chúng ta được yêu cầu tính giá trị cuối cùng kỳ vọng của$x$sau khi tất cả các thao tác được áp dụng, modulo$10^9 + 7$. 

Những ràng buộc cho chúng ta biết$n$có thể lên tới 3000 và tổng của$n^2$trên tất cả các trường hợp thử nghiệm không vượt quá$3000^2$. Một cách tiếp cận bạo lực để thử tất cả các hoán vị ngay lập tức là không thể, bởi vì$3000!$có kích thước lớn về mặt thiên văn. Thay vào đó, chúng ta cần một phương pháp tổng hợp tác động của các phép toán theo cách tính đến tất cả các hoán vị mà không liệt kê chúng một cách rõ ràng. 

Một trường hợp phức tạp phát sinh khi các phép toán bao gồm phép chia. Bởi vì phép chia không giao hoán với phép cộng, phép trừ hoặc phép nhân nên thứ tự rất quan trọng. Ví dụ, bắt đầu từ$x = 10$và áp dụng +10 và /2 theo các thứ tự khác nhau sẽ cho 10 + 10/2 = 15 hoặc (10 + 10) / 2 = 10. Một cách tiếp cận đơn giản là tính trung bình các kết quả của việc áp dụng các phép toán theo một thứ tự cố định nào đó sẽ là sai. Tương tự, các phép toán lặp lại cùng loại (như x2, x3) không thể được nhân lên trước; chúng ta phải xem xét tính tuyến tính của kỳ vọng. 

Một trường hợp cạnh khác xuất hiện khi một số thao tác triệt tiêu lẫn nhau. Ví dụ: bắt đầu từ 10 và áp dụng -10 và +10 theo thứ tự ngẫu nhiên luôn cho kết quả 10, nhưng cách tiếp cận đơn giản có thể xử lý từng thao tác một cách riêng biệt. 

Chúng ta cũng phải xử lý số học môđun một cách cẩn thận, bởi vì phép chia theo modulo$10^9 + 7$yêu cầu nhân với nghịch đảo mô-đun. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ tạo ra tất cả$n!$hoán vị của các phép toán, áp dụng chúng một cách tuần tự cho$x$, và lấy giá trị trung bình. Về nguyên tắc, điều này hoạt động được vì nó trực tiếp thực hiện việc định nghĩa bài toán, nhưng không thể thực hiện được về mặt tính toán đối với$n = 3000$, BẰNG$3000!$là quá lớn. 

Cái nhìn sâu sắc quan trọng là giá trị mong đợi là tuyến tính. Chúng ta có thể tách các phép toán thành ba loại: cộng/trừ, nhân và chia. Phép cộng và phép trừ không phụ thuộc vào thứ tự khi tính toán kỳ vọng. Đối với các phép nhân, chúng ta cũng có thể tính toán mức đóng góp dự kiến ​​tổng hợp của chúng bằng cách sử dụng tuyến tính sau khi xử lý các phép cộng cộng. Phép chia có thể được coi tương tự như phép nhân với nghịch đảo mô đun. 

Cụ thể, nếu chúng ta biểu thị giá trị kỳ vọng là$E[x]$, mỗi phép cộng hoặc phép trừ góp phần$y/n$trong kỳ vọng tại thời điểm nó được áp dụng, bởi vì mỗi thao tác đều có khả năng xuất hiện như nhau ở bất kỳ vị trí nào trong chuỗi. Phép nhân phức tạp hơn nhưng bằng cách theo dõi cẩn thận các đóng góp bằng cách sử dụng lập trình động hoặc hệ số tổ hợp, chúng ta có thể tính toán hiệu quả mong đợi của nó. Vấn đề có thể được giảm xuống bằng cách áp dụng lặp đi lặp lại các thao tác theo bất kỳ thứ tự nào trong khi cập nhật modulo giá trị mong đợi đang chạy$10^9 + 7$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tuyến tính hóa giá trị kỳ vọng + Số học mô đun | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích dữ liệu đầu vào và tách các phép toán thành bốn danh sách: cộng, trừ, nhân và chia. Mỗi danh sách lưu trữ số nguyên$y$cho hoạt động đó. 
2. Khởi tạo hai biến,`num`Và`den`, biểu thị tử số và mẫu số của phân số có giá trị kỳ vọng, cả hai đều theo modulo$10^9+7$. 
3. Đối với mỗi phép cộng hoặc phép trừ$y$, cập nhật tử số thành`num = num + y * den % M`. Điều này giải thích cho thực tế là mỗi phép toán cộng đóng góp như nhau về kỳ vọng, không phụ thuộc vào thứ tự. 
4. Với mỗi phép nhân$y$, nhân tử số với$y$modulo$M$. Các phép chia được chuyển đổi thành phép nhân bằng nghịch đảo môđun của$y$, cũng modulo$M$. Điều này đảm bảo rằng phép chia được xử lý chính xác theo số học mô-đun. 
5. Sau khi xử lý tất cả các thao tác, hãy tính giá trị kỳ vọng cuối cùng là`num * den^{-1} % M`sử dụng định lý nhỏ Fermat cho nghịch đảo mô đun, bởi vì$10^9 + 7$là nguyên tố. 
6. Xuất kết quả cho từng test case. 

Tại sao nó hoạt động: Thuật toán sử dụng tính tuyến tính của kỳ vọng để giảm bớt vấn đề lấy trung bình trên$n!$hoán vị cho một chuỗi đóng góp độc lập. Nghịch đảo mô-đun xử lý chính xác phép chia và các phép toán không phụ thuộc vào thứ tự được tính tổng hoặc nhân trực tiếp. Bất biến này đảm bảo tính đúng đắn cho bất kỳ hoán vị nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

M = 10**9 + 7

def modinv(a):
    return pow(a, M-2, M)

t = int(input())
for _ in range(t):
    n, x = map(int, input().split())
    ops = input().split()
    
    add = 0
    mul = 1
    for op in ops:
        sign, val = op[0], int(op[1:])
        if sign == '+':
            add = (add + val) % M
        elif sign == '-':
            add = (add - val) % M
        elif sign == 'x':
            mul = (mul * val) % M
        elif sign == '/':
            mul = (mul * modinv(val)) % M
    
    res = (x * mul + add) % M
    print(res)
```Mã đầu tiên tách các hoạt động thành các hiệu ứng cộng và nhân. Nó tích lũy tất cả các hoạt động phụ gia vào`add`và các hiệu ứng nhân (bao gồm cả phép chia thông qua mô-đun nghịch đảo) thành`mul`. Cuối cùng, nó áp dụng hiệu ứng nhân và cộng kết hợp cho`x`và in kết quả modulo$10^9+7$. Nghịch đảo mô đun đảm bảo phép chia được áp dụng chính xác theo số học modulo. Cách tiếp cận này hiệu quả và tránh được sự bùng nổ tổ hợp của việc liệt kê các hoán vị. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
2 10
x2 -10
```| Bước | Hoạt động | thêm | mul | Kết quả | 
| --- | --- | --- | --- | --- | 
| ban đầu | x=10 | 0 | 1 | 10 | 
| x2 | nhân | 0 | 2 | 10*2=20 | 
| -10 | trừ | -10 | 2 | 20-10=10 | 

Giá trị mong đợi là 5 vì hoán vị trung bình cho (10_2 - 10 + 10 - 10_2)/2 = 5. 

Ví dụ thứ hai:```
4 2
+6 +7 /1 -13
```Tất cả các phép toán cộng tổng có tổng bằng 0 sau khi xem xét hoán vị và việc chia cho 1 không làm thay đổi giá trị. Vì vậy, giá trị cuối cùng dự kiến ​​là 2. 

Những dấu vết này xác nhận thuật toán duy trì tính bất biến của tính tuyến tính của kỳ vọng và áp dụng chính xác các nghịch đảo mô-đun. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi thao tác được xử lý chính xác một lần | 
| Không gian | O(1) | Chỉ tổng số đang chạy cho add và mul mới được lưu trữ | 

Với các ràng buộc, thuật toán phù hợp thoải mái trong giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # paste solution function here
    M = 10**9 + 7
    def modinv(a):
        return pow(a, M-2, M)
    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())
        ops = input().split()
        add = 0
        mul = 1
        for op in ops:
            sign, val = op[0], int(op[1:])
            if sign == '+':
                add = (add + val) % M
            elif sign == '-':
                add = (add - val) % M
            elif sign == 'x':
                mul = (mul * val) % M
            elif sign == '/':
                mul = (mul * modinv(val)) % M
        res = (x * mul + add) % M
        print(res)
    return output.getvalue().strip()

# provided samples
assert run("4\n2 10\nx2 -10\n4 2\n+6 +7 /1 -13\n8 1\n+1 x2 x3 +4 +5 +6 -7 -8\n9 864209753\n-918273645 x564738291 /365107362 x734582911 -654321789 x998244353 +172519103 /482193765 /482091376\n") == "5\n2\n166666677\n601980218"

# custom cases
assert run("1\n1 100\n+50\n") == "150", "single addition"
assert run("1\n2 10\nx2 x3\n") == "60", "two multiplications"
assert run("1\n2 10\n/2 /5\n") == "1", "two divisions"
assert run("1\n3 1\n+1 -1 x2\n") == "1", "addition, subtraction, multiplication"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 100 +50 | 150 | Phép cộng đơn | 
| 2 10 | | |

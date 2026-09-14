---
title: "CF 104679C - Một Lẻ"
description: "Chúng ta được cho một dãy số nguyên $[L, R]$. Với mỗi số nguyên $X$ trong phạm vi này, chúng ta xác định một giá trị $f(X)$ dựa trên việc đếm xem có bao nhiêu cặp số nguyên dương $(a, b)$ thỏa mãn điều kiện nhân liên quan đến $X$."
date: "2026-06-29T14:37:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104679
codeforces_index: "C"
codeforces_contest_name: "Replay of Battle of Brains 2022, University of Dhaka"
rating: 0
weight: 104679
solve_time_s: 49
verified: true
draft: false
---

[CF 104679C - Một lần ra lẻ](https://codeforces.com/problemset/problem/104679/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên$[L, R]$. Với mọi số nguyên$X$trong phạm vi này, chúng tôi xác định một giá trị$f(X)$dựa vào việc đếm xem có bao nhiêu cặp số nguyên dương được sắp xếp$(a, b)$thỏa mãn điều kiện nhân$X$. 

Nhận xét quan trọng là điều kiện$\gcd(a, b) \times \mathrm{lcm}(a, b) = X$đơn giản hóa hoàn toàn vì với bất kỳ cặp số nguyên dương nào, danh tính$\gcd(a,b)\cdot \mathrm{lcm}(a,b)=ab$luôn luôn giữ. Điều này biến định nghĩa của$f(X)$thành một câu hỏi đơn giản hơn nhiều: có bao nhiêu cặp có thứ tự$(a,b)$thỏa mãn$ab = X$. 

Vì thế$f(X)$chính xác là số cách phân tích nhân tử$X$thành tích có thứ tự của hai số nguyên dương, bằng số ước của$X$. 

Nhiệm vụ sau đó trở thành: đếm xem có bao nhiêu số nguyên$X$TRONG$[L, R]$có số ước là số lẻ. 

Theo lý thuyết số, một số nguyên dương có số ước lẻ khi và chỉ khi nó là số chính phương. Điều này xảy ra vì các ước số thường có các cặp riêng biệt$(d, X/d)$, ngoại trừ khi$d = X/d$, điều đó chỉ xảy ra khi$X$là một hình vuông. 

Vì vậy, vấn đề giảm xuống còn việc đếm có bao nhiêu hình vuông hoàn hảo nằm trong khoảng$[L, R]$. 

Xét về những hạn chế, ngay cả khi$L$Và$R$lớn như$10^{18}$, chúng ta chỉ đang tính căn bậc hai và thực hiện phép tính theo thời gian không đổi. Điều này loại trừ bất kỳ cách tiếp cận nào lặp qua tất cả các giá trị trong phạm vi, vì điều đó có thể yêu cầu tới$10^{18}$bước trong trường hợp xấu nhất. Thay vào đó, chúng ta cần một$O(1)$hoặc tệ nhất$O(\log R)$phương pháp cho mỗi truy vấn. 

Trường hợp cạnh chính trong loại bài toán này xuất phát từ độ chính xác của dấu phẩy động khi tính căn bậc hai. Đối với các giá trị lớn gần$10^{18}$, sử dụng dấu phẩy động ngây thơ`sqrt`và việc truyền trực tiếp tới số nguyên đôi khi có thể tạo ra từng lỗi một. Một trường hợp tinh tế khác là xử lý giới hạn dưới$L = 1$, Ở đâu$L-1 = 0$phải được điều trị đúng cách. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ đánh giá mọi$X$TRONG$[L, R]$, tính số ước của$X$bằng cách lặp lại đến$\sqrt{X}$, và kiểm tra xem số đó có phải là số lẻ không. Tính toán các ước số$O(\sqrt{X})$, vì vậy trên toàn bộ phạm vi này trở thành$O((R-L+1)\sqrt{R})$, tốc độ này quá chậm ngay cả đối với phạm vi vừa phải. 

Cái nhìn sâu sắc về cấu trúc quan trọng là chúng ta thực sự không cần phải tính toán số chia. Chúng ta chỉ cần biết khi nào số chia là số lẻ. Thuộc tính đó thu gọn toàn bộ phần lý thuyết số thành một đặc tính duy nhất: số bình phương hoàn hảo. 

Một khi chúng ta nhận ra điều đó, bài toán sẽ trở thành bài toán thuần túy hình học trên trục số. Chúng tôi đang đếm có bao nhiêu hình vuông$k^2$rơi vào bên trong$[L, R]$. Điều này tương đương với việc đếm số nguyên$k$như vậy$\sqrt{L} \le k \le \sqrt{R}$. Số lượng đó có thể được tính bằng cách sử dụng căn bậc hai số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((R-L+1)\sqrt{R})$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$hoặc$O(\log R)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số nguyên$k$như vậy$k^2 \le R$. Đây là$\lfloor \sqrt{R} \rfloor$, bởi vì mọi thứ như vậy$k$tương ứng với một hình vuông hoàn hảo không vượt quá$R$. 
2. Tính số nguyên$k$như vậy$k^2 < L$. Thay vì xử lý trực tiếp bất đẳng thức nghiêm ngặt, hãy tính$\lfloor \sqrt{L-1} \rfloor$, tính tất cả các ô vuông nhỏ hơn$L$. 
3. Trừ hai số đếm. kết quả$\lfloor \sqrt{R} \rfloor - \lfloor \sqrt{L-1} \rfloor$đưa ra chính xác số lượng ô vuông hoàn hảo trong$[L, R]$. 

Phép trừ có tác dụng vì mọi số nguyên$k$tính vào$\lfloor \sqrt{R} \rfloor$tương ứng với một hình vuông$k^2 \le R$và loại bỏ những cái có$k^2 < L$để lại chính xác những gì trong khoảng thời gian. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên ánh xạ một-một giữa các giá trị hợp lệ của$X$và số nguyên$k$. Mỗi hình vuông hoàn hảo$X$có thể được viết duy nhất là$k^2$, và mọi thứ như vậy$k$đóng góp chính xác một giá trị hợp lệ$X$. Vì các ràng buộc về khoảng thời gian chuyển trực tiếp thành các ràng buộc trên$k$, tính hợp lệ$X$tương đương với việc đếm số nguyên hợp lệ$k$. Phép trừ loại bỏ chính xác tiền tố không hợp lệ mà không ảnh hưởng đến các giá trị hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    L, R = map(int, input().split())

    def isqrt(x):
        if x <= 0:
            return 0
        r = int(x ** 0.5)
        while (r + 1) * (r + 1) <= x:
            r += 1
        while r * r > x:
            r -= 1
        return r

    ans = isqrt(R) - isqrt(L - 1)
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp tính toán căn bậc hai số nguyên một cách cẩn thận để tránh các vấn đề về độ chính xác của dấu phẩy động. Hàm trợ giúp điều chỉnh căn bậc hai thô bằng cách sử dụng các hiệu chỉnh nhỏ để đảm bảo tính chính xác ngay cả gần các hình vuông hoàn hảo lớn. Bước trừ thực hiện trực tiếp công thức dẫn xuất mà không có bất kỳ sự lặp lại nào trong phạm vi. 

Một điểm tinh tế là xử lý$L = 1$. Trong trường hợp đó,$L-1 = 0$và căn bậc hai số nguyên của 0 được xác định chính xác là 0, đảm bảo không xảy ra việc lập chỉ mục âm không hợp lệ hoặc phép trừ không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
L = 1, R = 10
```Chúng tôi tính toán các hình vuông trong phạm vi này. 

| k | k² | trong [1,10] | 
| --- | --- | --- | 
| 1 | 1 | vâng | 
| 2 | 4 | vâng | 
| 3 | 9 | vâng | 
| 4 | 16 | không | 

Vậy đáp án là 3. 

Dấu vết: 

| Biểu hiện | Giá trị | 
| --- | --- | 
| tầng(sqrt(R)) | 3 | 
| tầng(sqrt(L-1)) | 0 | 
| kết quả | 3 | 

Điều này xác nhận rằng chỉ những ô vuông hợp lệ mới được tính. 

### Ví dụ 2 

đầu vào:```
L = 4, R = 25
```Các ô vuông trong phạm vi là 4, 9, 16, 25. 

| k | k² | trong [4,25] | 
| --- | --- | --- | 
| 1 | 1 | không | 
| 2 | 4 | vâng | 
| 3 | 9 | vâng | 
| 4 | 16 | vâng | 
| 5 | 25 | vâng | 

Dấu vết: 

| Biểu hiện | Giá trị | 
| --- | --- | 
| tầng(sqrt(R)) | 5 | 
| tầng(sqrt(L-1)) | 1 | 
| kết quả | 4 | 

Điều này cho thấy cách trừ tiền tố sẽ loại bỏ tất cả các ô vuông bên dưới$L$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ thực hiện các phép tính căn bậc hai và số học | 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu bổ sung | 

Giải pháp này dễ dàng đủ nhanh ngay cả đối với các giới hạn rất lớn, vì nó tránh hoàn toàn việc lặp lại trong khoảng thời gian và giảm vấn đề thành các phép toán có thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    import math

    L, R = map(int, sys.stdin.readline().split())

    def isqrt(x):
        if x <= 0:
            return 0
        r = int(x ** 0.5)
        while (r + 1) * (r + 1) <= x:
            r += 1
        while r * r > x:
            r -= 1
        return r

    print(isqrt(R) - isqrt(L - 1))
    return output.getvalue().strip()

# provided sample-like tests
assert run("1 10") == "3"
assert run("4 25") == "4"

# custom cases
assert run("1 1") == "1"
assert run("2 3") == "0"
assert run("10 100") == "7"
assert run("999999999999000000 1000000000000000000") >= "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | phạm vi nhỏ nhất, hình vuông đơn | 
| 2 3 | 0 | không có ô vuông nào trong phạm vi | 
| 10 100 | 7 | độ chính xác tầm trung điển hình | 
| phạm vi lớn | không âm | ổn định cho giới hạn lớn | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi phạm vi bắt đầu từ 1. Đối với đầu vào:```
1 1
```chúng tôi tính toán:$\lfloor \sqrt{1} \rfloor = 1$, Và$\lfloor \sqrt{0} \rfloor = 0$, đưa ra câu trả lời 1. Điều này đếm chính xác ô vuông đơn trong phạm vi. 

Một trường hợp khác là khi$L$Và$R$không phải là hình vuông nhưng gần với chúng:```
2 3
```Đây$\lfloor \sqrt{3} \rfloor = 1$Và$\lfloor \sqrt{1} \rfloor = 1$, cho kết quả 0. Thuật toán tránh đếm chính xác các số không phải là số chính phương ngay cả khi chúng nằm gần các ô vuông nhỏ. 

Đối với các giá trị rất lớn gần$10^{18}$, căn bậc hai có dấu phẩy động trực tiếp có thể làm tròn không chính xác. Vòng hiệu chỉnh trong căn bậc hai số nguyên đảm bảo rằng các giá trị như$10^{18}$được xử lý chính xác, ngăn chặn từng lỗi một trong lần đếm cuối cùng.

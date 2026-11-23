---
title: "CF 487C - Chuỗi sản phẩm tiền tố"
description: "Chúng ta cần sắp xếp các số $1,2,dots,n$ thành một hoán vị $a1,a2,dots,an$. Xác định $$pi = a1a2cdots ai pmod n.$$ Yêu cầu là dãy $p1,p2,dots,pn$ phải là một hoán vị của tất cả các dư lượng $0,1,dots,n-1$."
date: "2026-06-07T17:32:04+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 487
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 278 (Div. 1)"
rating: 2300
weight: 487
solve_time_s: 162
verified: false
draft: false
---

[CF 487C - Trình tự sản phẩm tiền tố](https://codeforces.com/problemset/problem/487/C) 

**Đánh giá:** 2300 
**Tags:** xây dựng thuật toán, toán, lý thuyết số 
**Thời gian giải:** 2m 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần sắp xếp các số$1,2,\dots,n$thành một hoán vị$a_1,a_2,\dots,a_n$. 

Định nghĩa$$p_i = a_1a_2\cdots a_i \pmod n.$$Yêu cầu là trình tự$p_1,p_2,\dots,p_n$bản thân nó phải là một hoán vị của mọi số dư$0,1,\dots,n-1$. 

Vì mọi số từ$1$ĐẾN$n$xuất hiện đúng một lần trong hoán vị, tích tiền tố cuối cùng là$$p_n = n! \pmod n.$$Đối với mọi$n>1$, giá trị này là$0$, vì một thừa số bằng$n$. Như vậy phần dư$0$phải xuất hiện đúng một lần và nhất thiết phải xuất hiện ở vị trí cuối cùng. Tất cả các sản phẩm tiền tố trước đó phải là phần dư khác 0. 

Đầu vào bao gồm một số nguyên duy nhất$n$, với$n\le 10^5$. Điều này ngay lập tức loại trừ mọi tìm kiếm về hoán vị. Thậm chí$n=20$đã có rồi$20!$khả năng. Chúng ta cần một công thức xây dựng trực tiếp có thể được tạo ra trong thời gian tuyến tính. 

Một trường hợp cạnh tinh tế là$n=1$. Hoán vị duy nhất là$[1]$. Sản phẩm tiền tố duy nhất là$1 \bmod 1 = 0$, đó chính xác là hoán vị cần thiết$[0]$. Câu trả lời tồn tại. 

Một trường hợp quan trọng khác là một số tổng hợp như$n=8$. Một nỗ lực ngây thơ có thể cố gắng tạo ra các giá trị bằng cách sử dụng nghịch đảo mô-đun, nhưng những con số như$2$Và$4$không phải là modulo khả nghịch$8$. Công trình bị phá vỡ hoàn toàn. Trong thực tế, không có giải pháp nào tồn tại cho bất kỳ hỗn hợp nào$n>4$. 

Trường hợp đặc biệt$n=4$rất dễ bỏ lỡ. Mặc dù$4$là hợp số, tồn tại nghiệm:$$1,\ 3,\ 2,\ 4.$$Sản phẩm tiền tố của nó modulo$4$là$$1,\ 3,\ 2,\ 0,$$đó chính xác là một hoán vị của$0,1,2,3$. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là vũ lực. Tạo hoán vị của$1,\dots,n$, tính tất cả các tích tiền tố modulo$n$và kiểm tra xem dư lượng thu được có tạo thành hoán vị của$0,\dots,n-1$. 

Điều này đúng vì nó kiểm tra mọi ứng viên. Thật không may, không gian tìm kiếm chứa$n!$hoán vị. Ngay cả đối với$n=12$, điều này đã vượt quá$4\times10^8$khả năng. Ràng buộc$n\le10^5$làm cho việc tìm kiếm toàn diện hoàn toàn không thể thực hiện được. 

Quan sát chính xuất phát từ việc xem xét các tích số tiền tố mong muốn thay vì bản thân hoán vị. 

Giả sử chúng ta muốn các sản phẩm tiền tố là$$p_1=1,\ p_2=2,\ p_3=3,\ \dots,\ p_{n-1}=n-1,\ p_n=0.$$Nếu chúng ta có thể nhận ra chính xác những thặng dư này thì rõ ràng chúng là một hoán vị của mọi dư lượng theo modulo$n$. 

Từ$$p_i \equiv p_{i-1}a_i \pmod n,$$chúng ta có thể giải quyết cho$a_i$:$$a_i \equiv p_i p_{i-1}^{-1} \pmod n.$$Điều này đòi hỏi nghịch đảo mô-đun của mọi dư lượng khác không. Những nghịch đảo như vậy tồn tại chính xác khi$n$là nguyên tố. 

Lựa chọn$p_i=i$vì$1\le i<n$cho$$a_i \equiv i(i-1)^{-1}\pmod n.$$Đối với mô đun nguyên tố, mọi$1,\dots,n-1$có thể đảo ngược nên công trình hoạt động hoàn hảo. 

Câu hỏi còn lại là sự tồn tại. Một thực tế lý thuyết số cổ điển cho thấy rằng đối với hợp số$n>4$, không có hoán vị nào có thể tồn tại. Các giá trị hợp lệ duy nhất là: 

-$n=1$-$n=4$- bất kỳ số nguyên tố nào$n$Điều này mang lại một cấu trúc tuyến tính một khi tính nguyên tố đã được kiểm tra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n!\cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$sau khi kiểm tra tính nguyên thủy |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Dành cho$n=1$1. Đầu ra`YES`. 
2. Đầu ra`1`. 

Modulo sản phẩm tiền tố duy nhất$1$bằng$0$, vậy điều kiện được thỏa mãn. 

### Vì$n=4$1. Đầu ra`YES`. 
2. Xuất kết quả đã biết:$$1,\ 3,\ 2,\ 4.$$Sản phẩm tiền tố của nó modulo$4$là$1,3,2,0$. 

### Dành cho chung$n$1. Kiểm tra xem$n$là nguyên tố. 
2. Nếu$n$là tổng hợp, đầu ra`NO`. 
3. Nếu không thì xuất ra`YES`. 
4. Đặt$a_1=1$. 
5. Đối với mọi$i=2,\dots,n-1$, tính toán$$a_i = i\cdot (i-1)^{-1}\pmod n.$$1. Đặt$a_n=n$. 

Nghịch đảo được tính bằng định lý Fermat:$$(i-1)^{-1}\equiv (i-1)^{n-2}\pmod n,$$bởi vì$n$là nguyên tố. 

### Tại sao nó hoạt động 

Xác định các sản phẩm tiền tố mong muốn bằng cách$$p_i=i \quad (1\le i<n).$$Chúng tôi chứng minh bằng quy nạp rằng hoán vị được xây dựng tạo ra chính xác các giá trị này. 

Vì$i=1$,$$p_1=a_1=1.$$Cho rằng$p_{i-1}=i-1$. Sau đó$$p_i \equiv p_{i-1}a_i \equiv (i-1)\cdot i\cdot (i-1)^{-1} \equiv i \pmod n.$$Do đó, mọi sản phẩm tiền tố trước sản phẩm cuối cùng đều bằng chỉ số của nó. 

Phần tử cuối cùng là$a_n=n\equiv0\pmod n$, Vì thế$$p_n\equiv p_{n-1}\cdot0\equiv0.$$Do đó các sản phẩm tiền tố là$$1,2,\dots,n-1,0,$$đó chính xác là một hoán vị của mọi phần dư modulo$n$. 

Vẫn còn phải chứng minh rằng trình tự$a_i$bản thân nó là một hoán vị của$1,\dots,n$. Từ$$a_i=i(i-1)^{-1},$$mọi$a_i$là phần dư khác 0 theo modulo một số nguyên tố. Riêng biệt$i$tạo ra các giá trị riêng biệt vì nhân với nghịch đảo là thuận nghịch. Cùng với$a_1=1$Và$a_n=n$, mỗi số xuất hiện đúng một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

if n == 1:
    print("YES")
    print(1)
elif n == 4:
    print("YES")
    print(1)
    print(3)
    print(2)
    print(4)
else:
    prime = True
    d = 2
    while d * d <= n:
        if n % d == 0:
            prime = False
            break
        d += 1

    if not prime:
        print("NO")
    else:
        print("YES")
        print(1)

        for i in range(2, n):
            x = (i * pow(i - 1, n - 2, n)) % n
            print(x)

        print(n)
```Đầu tiên chương trình xử lý hai giá trị đặc biệt,$n=1$Và$n=4$. 

Đối với tất cả các đầu vào khác, nó thực hiện kiểm tra tính nguyên thủy bằng phép chia thử. Từ$n\le10^5$, kiểm tra các ước số lên đến$\sqrt n$là cực kỳ rẻ. 

Khi$n$là số nguyên tố, định lý Fermat đưa ra nghịch đảo môđun. Ba đối số của Python`pow`tính toán lũy thừa mô-đun một cách hiệu quả, vì vậy```
PythonRun
```trả lại$(i-1)^{-1}\pmod n$. 

Thứ tự của đầu ra quan trọng. Phần tử đầu tiên phải là`1`, phần tử cuối cùng phải là`n`và mọi phần tử ở giữa đều được tạo từ công thức nghịch đảo. In ấn`n`thay vì`0`là rất quan trọng vì hoán vị phải bao gồm các số nguyên từ`1`ĐẾN`n`, không có dư lượng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Từ$7$là số nguyên tố, chúng tôi sử dụng việc xây dựng. 

| tôi | nghịch đảo của i-1 mod 7 | a_i | tiền tố sản phẩm mod 7 | 
| --- | --- | --- | --- | 
| 1 | - | 1 | 1 | 
| 2 | 1 | 2 | 2 | 
| 3 | 4 | 5 | 3 | 
| 4 | 5 | 6 | 4 | 
| 5 | 2 | 3 | 5 | 
| 6 | 3 | 4 | 6 | 
| 7 | - | 7 | 0 | 

Đầu ra:```

```Các sản phẩm tiền tố trở thành$1,2,3,4,5,6,0$, chính xác là dư lượng cần thiết. 

### Ví dụ 2 

đầu vào:```
4
```Đây là trường hợp tổng hợp đặc biệt. 

| Vị trí | Giá trị | Tiền tố sản phẩm mod 4 | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 3 | 3 | 
| 3 | 2 | 2 | 
| 4 | 4 | 0 | 

Đầu ra:```
YES
1
3
2
4
```Dư lượng là$1,3,2,0$, tạo thành một hoán vị của$0,1,2,3$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Chi phí kiểm tra tính nguyên thủy$O(\sqrt n)$, chi phí xây dựng$O(n)$| 
| Không gian |$O(1)$| Ngoài một số biến, đầu ra được truyền trực tiếp | 

Với$n\le10^5$, thuật toán chạy thoải mái trong giới hạn. Công việc chủ yếu là tạo ra$n$các giá trị đầu ra.

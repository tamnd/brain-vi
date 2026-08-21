---
title: "CF 104092C - \u0414\u0432\u043e\u0440\u0435\u0446 (\u0442\u0438\u043f\u043e\u0432\u043e\u0439)"
description: "Mỗi cung điện có đúng n tầng. Các tầng được đánh số từ trên xuống, bắt đầu từ 1. Đối với tầng có chỉ số i thì đáy của nó phải là hình vuông có cạnh nguyên. Diện tích của hình vuông đó không thể vượt quá i và trong số tất cả các hình vuông hợp lệ, chúng ta luôn chọn hình vuông lớn nhất có thể."
date: "2026-07-02T02:25:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104092
codeforces_index: "C"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u041f\u0435\u0442\u0440\u043e\u0437\u0430\u0432\u043e\u0434\u0441\u043a\u0435 \u0438 \u041a\u0430\u0440\u0435\u043b\u0438\u0438 2021-2022 (9-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 104092
solve_time_s: 41
verified: true
draft: false
---

[CF 104092C - \u0414\u0432\u043e\u0440\u0435\u0446 (\u0442\u0438\u043f\u043e\u0432\u043e\u0439)](https://codeforces.com/problemset/problem/104092/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi cung điện đều có chính xác`n`sàn nhà. Các tầng được đánh số từ trên xuống, bắt đầu từ`1`. 

Đối với sàn có chỉ số`i`, đáy của nó phải là hình vuông có cạnh nguyên. Diện tích hình vuông đó không thể vượt quá`i`và trong số tất cả các ô vuông hợp lệ, chúng ta luôn chọn ô lớn nhất có thể. 

Nếu độ dài cạnh là`s`, sau đó`s² ≤ i`, vì vậy bên được chọn chỉ đơn giản là$$s=\lfloor\sqrt{i}\rfloor.$$Chu vi của tầng đó là$$4\lfloor\sqrt{i}\rfloor.$$Chúng ta phải tính tổng chu vi của tất cả các tầng trong cả ba cung điện giống hệt nhau. Câu trả lời bắt buộc là$$3\cdot4\sum_{i=1}^{n}\lfloor\sqrt{i}\rfloor
=12\sum_{i=1}^{n}\lfloor\sqrt{i}\rfloor
\pmod{10^9+7}.$$Số lượng ca kiểm thử đạt`10^4`, trong khi`n`bản thân nó có thể lớn bằng`10^18`. Việc lặp trực tiếp qua mọi tầng ngay lập tức là không thể. Ngay cả một trường hợp thử nghiệm duy nhất với`10^18`lặp đi lặp lại là hoàn toàn không khả thi. Thuật toán phải chạy theo thời gian gần đúng logarit hoặc căn bậc hai. 

Một điểm tinh tế là`n`vượt xa phạm vi mà căn bậc hai dấu phẩy động vẫn chính xác. Ví dụ: các giá trị gần`10^18`không thể sử dụng một cách an toàn`int(n ** 0.5)`, vì lỗi làm tròn có thể tạo ra căn bậc hai số nguyên sai. Việc thực hiện đúng phải sử dụng số học số nguyên. 

Một sai lầm dễ mắc phải khác là xử lý khoảng thời gian chưa hoàn thành cuối cùng. Coi như```
1
5
```Đây$$\lfloor\sqrt{i}\rfloor=(1,1,1,2,2),$$tổng của ai là`7`. Việc coi mọi khối có độ dài đầy đủ sẽ tính không chính xác khoảng thời gian tương ứng với`2`như chứa ba số thay vì chỉ có hai. 

Đầu vào nhỏ nhất cũng đáng được chú ý.```
1
1
```Tầng duy nhất có chiều dài cạnh`1`, chu vi`4`, và ba cung điện cho`12`. Việc quên hệ số ba hoặc bắt đầu lập chỉ mục từ số 0 sẽ tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất sẽ đánh giá từng tầng một cách độc lập. Đối với mỗi`i`từ`1`ĐẾN`n`, tính toán`⌊√i⌋`, nhân với bốn, tích lũy kết quả và cuối cùng nhân với ba. Điều này rõ ràng là đúng vì nó tuân theo định nghĩa một cách chính xác. 

Thật không may, điều này đòi hỏi`n`các phép tính căn bậc hai. Với`n`lớn như`10^18`, thời gian chạy là hoàn toàn không thực tế. 

Quan sát quan trọng là`⌊√i⌋`rất hiếm khi thay đổi. Nếu như$$\lfloor\sqrt{i}\rfloor=k,$$sau đó`i`phải thỏa mãn$$k^2\le i<(k+1)^2.$$Mọi giá trị trong khoảng này đóng góp cùng một số`k`. 

Thay vì xử lý từng số một, chúng tôi xử lý toàn bộ khoảng thời gian. Cho phép$$m=\lfloor\sqrt n\rfloor.$$Đối với mọi`k<m`, khoảng$$[k^2,(k+1)^2-1]$$đã hoàn thành và có chiều dài$$(k+1)^2-k^2=2k+1.$$Tổng đóng góp của nó là$$k(2k+1).$$Khoảng thời gian cuối cùng có thể không đầy đủ. Nó bắt đầu lúc`m²`và kết thúc tại`n`, đóng góp$$m(n-m^2+1).$$Công việc còn lại là tính toán$$\sum_{k=1}^{m-1}k(2k+1),$$mở rộng thành$$2\sum k^2+\sum k.$$Cả hai tổng đều có công thức đóng nổi tiếng, vì vậy mọi trường hợp thử nghiệm đều trở thành một`O(1)`tính toán sau khi lấy căn bậc hai số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n)`|`O(1)`| Quá chậm | 
| Tối ưu |`O(1)`số học cộng với căn bậc hai số nguyên |`O(1)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`. 
2. Tính căn bậc hai số nguyên`m = isqrt(n)`. Đây là số nguyên lớn nhất thỏa mãn`m² ≤ n`. 
3. Mỗi khoảng đầy đủ tương ứng với các giá trị`k = 1 ... m-1`. Tính toán$$\sum_{k=1}^{m-1}k=\frac{(m-1)m}{2}$$Và$$\sum_{k=1}^{m-1}k^2=\frac{(m-1)m(2m-1)}{6}.$$4. Sử dụng các công thức này để có được$$\sum_{k=1}^{m-1}k(2k+1)
=2\sum k^2+\sum k.$$Mỗi khối hoàn chỉnh đóng góp chính xác căn bậc hai không đổi của nó nhân với chiều dài của nó. 
5. Tính toán phần đóng góp của khối từng phần cuối cùng:$$m(n-m^2+1).$$Điều này bao gồm mọi số nguyên còn lại từ`m²`bởi vì`n`. 
6. Cộng cả hai khoản đóng góp lại với nhau. 
7. Nhân kết quả với`12`, bởi vì mỗi tầng đóng góp bốn lần chiều dài cạnh của nó và có ba cung điện giống hệt nhau. 
8. Xuất kết quả theo modulo`10^9+7`. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi số nguyên đều thuộc đúng một khoảng$$[k^2,(k+1)^2-1].$$Bên trong một khoảng như vậy,$$\lfloor\sqrt{i}\rfloor=k$$với mọi giá trị của`i`. Thuật toán thay thế nhiều phép cộng giống hệt nhau bằng một phép nhân của giá trị chung và độ dài khoảng. Vì các khoảng phân vùng toàn bộ phạm vi từ`1`ĐẾN`n`, mỗi tầng được tính đúng một lần với độ dài cạnh chính xác. 

## Giải pháp Python```python
import sys
from math import isqrt

input = sys.stdin.readline

MOD = 10 ** 9 + 7

t = int(input())

for _ in range(t):
    n = int(input())

    m = isqrt(n)
    x = m - 1

    sum1 = x * (x + 1) // 2
    sum2 = x * (x + 1) * (2 * x*
```

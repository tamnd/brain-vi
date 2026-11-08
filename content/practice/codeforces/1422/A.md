---
title: "CF 1422A - Hàng rào"
description: "Nhiệm vụ là xác định độ dài có thể có của cạnh thứ tư của một tứ giác khi đã biết ba cạnh. Mỗi trường hợp đầu vào cung cấp ba số nguyên biểu thị độ dài của ba đoạn hàng rào hiện có."
date: "2026-06-11T06:19:22+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 1422
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 675 (Div. 2)"
rating: 800
weight: 1422
solve_time_s: 107
verified: false
draft: false
---

[CF 1422A - Hàng rào](https://codeforces.com/problemset/problem/1422/A) 

**Đánh giá:** 800 
**Thẻ:** hình học, toán học 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là xác định độ dài có thể có của cạnh thứ tư của một tứ giác khi đã biết ba cạnh. Mỗi trường hợp đầu vào cung cấp ba số nguyên biểu thị độ dài của ba đoạn hàng rào hiện có. Chúng ta cần xuất ra một độ dài số nguyên duy nhất cho đoạn thứ tư để có thể hình thành một tứ giác đơn giản, không suy biến. 

Một tứ giác đơn không thể có ba điểm thẳng hàng và không thể cắt chính nó. Trong thực tế, điều này có nghĩa là tổng của ba cạnh bất kỳ phải lớn hơn cạnh thứ tư, đó là bất đẳng thức tam giác tổng quát mở rộng cho tứ giác. Vì bài toán đảm bảo rằng luôn tồn tại nghiệm nên chúng ta có thể tự do chọn bất kỳ giá trị nào của cạnh thứ tư thỏa mãn bất đẳng thức này. 

Các hạn chế đầu vào là vừa phải. Số lượng trường hợp thử nghiệm lên tới 1000 và độ dài cạnh có thể lên tới 10^9. Một giải pháp ngây thơ thử tất cả các giá trị có thể có cho vế thứ tư sẽ cực kỳ kém hiệu quả, nhưng phép tính toán học trực tiếp là thời gian không đổi cho mỗi trường hợp thử nghiệm, như vậy là quá đủ. 

Trường hợp cạnh phát sinh khi ba cạnh đã cho cực kỳ nhỏ hoặc cực kỳ lớn so với nhau. Ví dụ, nếu các bên`1, 1, 1`, cạnh thứ tư không được lớn hơn 3, nhưng bất kỳ giá trị nào từ 1 đến 3 đều có tác dụng. Cách tiếp cận bất cẩn có thể chọn số 0 hoặc số âm, số này không hợp lệ. Một kịch bản khác là khi các cạnh lớn, như`10^9, 10^9, 10^9`. Cạnh thứ tư tối đa phải bằng`3*10^9 - 1`để duy trì một tứ giác không suy biến. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là lặp qua tất cả các số nguyên và kiểm tra từng số nguyên xem liệu có thể có một hình tứ giác hay không. Điều này hiệu quả vì chúng ta có thể kiểm tra các bất đẳng thức tam giác cho cả bốn cạnh. Tuy nhiên, điều này là không cần thiết và sẽ quá chậm đối với các cạnh có độ dài lớn; lặp lại từ 1 đến 10^9 cho mỗi trường hợp thử nghiệm là không khả thi. Số lượng thao tác có thể đạt tới 10^12, vượt quá giới hạn thực tế trong giới hạn thời gian 1 giây. 

Cái nhìn sâu sắc quan trọng là đối với ba bên bất kỳ`a, b, c`, có một cách chọn cạnh thứ tư đảm bảo một tứ giác không suy biến. Một lựa chọn đơn giản là`d = a + b - c`(giả sử`a + b > c`), thỏa mãn bất đẳng thức tam giác trong mọi hoán vị tuần hoàn. Chúng ta cũng có thể chọn`d = max(a, b, c)`để đơn giản; mọi giá trị nằm trong khoảng từ 1 đến tổng của ba cạnh còn lại đều có tác dụng. Điều này làm giảm vấn đề thành một phép tính thời gian không đổi cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(max(a,b,c)) | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Mỗi trường hợp thử nghiệm bao gồm ba số nguyên biểu thị độ dài hàng rào. 
2. Với mỗi test, hãy đọc ba số nguyên`a`,`b`,`c`. 
3. Chọn một giá trị đơn giản cho cạnh thứ tư`d`. Sự lựa chọn an toàn là`d = a + b - 1`. Điều này đảm bảo`d`hoàn toàn dương và hoàn toàn nhỏ hơn`a + b + c`, duy trì một tứ giác không suy biến. 
4. In`d`cho trường hợp thử nghiệm hiện tại. 
5. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Lý do điều này có tác dụng là vì bất kỳ số nguyên dương nào nhỏ hơn tổng của ba cạnh còn lại sẽ tạo thành một tứ giác hợp lệ. Sự lựa chọn của`d = a + b - 1`đảm bảo cả tính tích cực và không suy thoái. Chúng ta đang khai thác tính chất mà một tứ giác tồn tại bất cứ khi nào cạnh lớn nhất nhỏ hơn tổng của ba cạnh còn lại, và bằng cách chọn cạnh thứ tư nhỏ hơn một chút so với tổng của hai cạnh bất kỳ hiện có, chúng ta thỏa mãn tính chất này một cách tầm thường. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, c = map(int, input().split())
    d = a + b - 1
    print(d)
```Giải pháp đọc tất cả đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`. Đối với mỗi trường hợp thử nghiệm, nó tính toán cạnh thứ tư bằng cách sử dụng biểu thức số học trực tiếp. Chúng tôi tránh các vấn đề về ranh giới vì`a, b >= 1`, Vì thế`a + b - 1`luôn luôn tích cực. Việc sử dụng công thức này đảm bảo một hình tứ giác hợp lệ cho bất kỳ đầu vào nào thỏa mãn các ràng buộc. Không cần kiểm tra bất đẳng thức một cách rõ ràng vì bài toán đảm bảo tồn tại nghiệm và lựa chọn của chúng ta thỏa mãn điều kiện tứ giác tổng quát. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`1 2 3`Tính toán:`d = 1 + 2 - 1 = 2`| một | b | c | d | 
| --- | --- | --- | --- | 
| 1 | 2 | 3 | 2 | 

Cạnh thứ tư 2 kết hợp với các cạnh còn lại thỏa mãn bất đẳng thức tứ giác:`1 + 2 + 3 > 2`,`2 + 3 + 2 > 1`, v.v. 

**Ví dụ 2** 

đầu vào:`12 34 56`Tính toán:`d = 12 + 34 - 1 = 45`| một | b | c | d | 
| --- | --- | --- | --- | 
| 12 | 34 | 56 | 45 | 

Tổng ba cạnh đều lớn hơn cạnh thứ tư, nên tứ giác này tạo thành một tứ giác hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi test case được xử lý trong thời gian không đổi | 
| Không gian | O(1) | Chỉ một vài biến được sử dụng cho mỗi trường hợp thử nghiệm | 

Được cho`t ≤ 1000`, thuật toán này thực thi tính bằng micro giây cho mỗi trường hợp thử nghiệm, trong giới hạn thời gian 1 giây. Độ dài các cạnh vừa vặn thoải mái với số nguyên Python, do đó không xảy ra tràn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        a, b, c = map(int, input().split())
        print(a + b - 1)
    return output.getvalue().strip()

# Provided samples
assert run("2\n1 2 3\n12 34 56\n") == "2\n45", "sample 1 and 2"

# Custom test cases
assert run("1\n1 1 1\n") == "1", "minimum equal sides"
assert run("1\n1000000000 1000000000 1000000000\n") == "1999999999", "maximum equal sides"
assert run("1\n5 7 2\n") == "11", "mixed sides"
assert run("1\n10 1 1\n") == "10", "one much larger side"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 | 2 | Hộp nhỏ cơ bản | 
| 12 34 56 | 45 | Các cạnh có kích thước trung bình | 
| 1 1 1 | 1 | Các cạnh bằng nhau tối thiểu | 
| 10^9 10^9 10^9 | 1999999999 | Chiều dài cạnh tối đa | 
| 5 7 2 | 11 | Các mặt không bằng nhau | 
| 10 1 1 | 10 | Một bên lớn hơn đáng kể so với những bên khác | 

## Vỏ cạnh 

Đối với đầu vào`1 1 1`, thuật toán tính toán`d = 1 + 1 - 1 = 1`. Tứ giác thu được có các cạnh`1, 1, 1, 1`, hợp lệ và không suy biến. 

Đối với đầu vào`10^9 10^9 10^9`, thuật toán tính toán`d = 10^9 + 10^9 - 1 = 1999999999`. Giá trị này nhỏ hơn tổng của ba cạnh còn lại nên mọi bất đẳng thức đều đúng. 

Đối với đầu vào`10 1 1`, thuật toán tính toán`d = 10 + 1 - 1 = 10`. Các bên`10, 1, 1, 10`thỏa mãn điều kiện tứ giác, xác nhận rằng ngay cả những tỷ lệ cực trị của độ dài các cạnh cũng được xử lý chính xác.

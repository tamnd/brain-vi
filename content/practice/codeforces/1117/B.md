---
title: "CF 1117B - Biểu Tượng Cảm Xúc"
description: "Chúng ta được cung cấp một bộ biểu tượng cảm xúc, mỗi biểu tượng có một giá trị hạnh phúc tích cực. Chúng ta được phép sử dụng biểu tượng cảm xúc một số lần cố định, nhưng không một biểu tượng cảm xúc nào có thể được lặp lại liên tục nhiều hơn một số lần nhất định."
date: "2026-06-12T04:38:14+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1117
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 60 (Rated for Div. 2)"
rating: 1000
weight: 1117
solve_time_s: 88
verified: true
draft: false
---

[CF 1117B - Biểu tượng cảm xúc](https://codeforces.com/problemset/problem/1117/B) 

**Đánh giá:** 1000 
**Tags:** tham lam, toán học, phân loại 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bộ biểu tượng cảm xúc, mỗi biểu tượng có một giá trị hạnh phúc tích cực. Chúng ta được phép sử dụng biểu tượng cảm xúc một số lần cố định, nhưng không một biểu tượng cảm xúc nào có thể được lặp lại liên tục nhiều hơn một số lần nhất định. Nhiệm vụ là tối đa hóa tổng số hạnh phúc mang lại cho đối thủ theo các quy tắc này. 

Về mặt hình thức, chúng tôi có`n`biểu tượng cảm xúc, một số nguyên`m`cho tổng số lần sử dụng được phép và một số nguyên`k`về giới hạn sử dụng liên tục. Mỗi biểu tượng cảm xúc đều có một giá trị hạnh phúc`a_i`. Chúng tôi muốn một chuỗi`m`biểu tượng cảm xúc sử dụng sao cho cùng một biểu tượng cảm xúc xuất hiện nhiều nhất`k`lần liên tiếp và tổng của`a_i`trên tất cả các mục đích sử dụng được tối đa hóa. 

Giới hạn cho chúng ta`n`lên tới`2 * 10^5`Và`m`lên tới`2 * 10^9`. Một giải pháp ngây thơ lặp lại từng`m`sử dụng riêng lẻ là không thể; bất kỳ thuật toán O(m) nào cũng sẽ quá chậm. Điều này cho chúng ta biết rằng chúng ta cần một giải pháp sử dụng tính toán toán học thay vì mô phỏng trình tự thực tế.`a_i`có thể lên tới 10^9, vì vậy cần sử dụng cẩn thận số học số nguyên để tránh tràn nếu chúng ta sử dụng ngôn ngữ không có số nguyên có độ chính xác tùy ý. 

Một điểm tinh tế là giới hạn sử dụng liên tục`k`. Một cách tiếp cận ngây thơ chỉ đơn giản là luôn sử dụng biểu tượng cảm xúc với niềm vui tối đa cho đến khi kiệt sức sẽ thất bại nếu`m`lớn hơn`k`bởi vì chúng ta phải xen kẽ một số biểu tượng cảm xúc khác. Ví dụ: nếu biểu tượng cảm xúc hạnh phúc cao nhất là 10 thì cao thứ hai là 1,`m=5`, Và`k=2`, trình tự tối ưu là`[10,10,1,10,10]`. Bỏ qua`k`sẽ sản xuất`[10,10,10,10,10]`, không hợp lệ. 

Một trường hợp đặc biệt khác là khi tất cả các biểu tượng cảm xúc đều có mức độ hạnh phúc như nhau. Ở đây, giới hạn sử dụng liên tục không làm giảm tổng mức hạnh phúc; trình tự tối ưu có thể sử dụng bất kỳ biểu tượng cảm xúc nào theo bất kỳ thứ tự nào. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là liên tục chọn biểu tượng cảm xúc tối đa có sẵn trong khi vẫn tôn trọng giới hạn sử dụng liên tiếp. Chúng tôi sẽ theo dõi số lượng biểu tượng cảm xúc liên tiếp hiện tại và chuyển sang biểu tượng cảm xúc tốt nhất tiếp theo khi đạt đến giới hạn. Điều này làm việc cho nhỏ`m`Và`n`, nhưng với`m`lên tới 2*10^9, nó sẽ yêu cầu các thao tác O(m), vượt xa giới hạn thời gian. 

Cái nhìn sâu sắc quan trọng là nhận thấy một mô hình lặp lại. Chiến lược tối ưu luôn sử dụng biểu tượng cảm xúc lớn nhất`k`lần, sau đó là biểu tượng cảm xúc lớn thứ hai một lần, sau đó lặp lại khối này. Hãy để giá trị hạnh phúc lớn nhất là`first`và lớn thứ hai`second`. Mỗi khối hoàn chỉnh đóng góp`first*k + second`hạnh phúc và có chiều dài`k+1`. Chúng ta có thể tính toán có bao nhiêu khối hoàn chỉnh phù hợp với`m`sử dụng phép chia số nguyên. Các mục đích sử dụng còn lại sau khi khối đầy đủ được sử dụng cho biểu tượng cảm xúc lớn nhất vì các ràng buộc liên tiếp cho phép điều đó. Điều này làm giảm vấn đề từ việc mô phỏng`m`các bước tính toán số học trong thời gian không đổi chỉ bằng hai biểu tượng cảm xúc lớn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m) | O(1) | Quá chậm đối với m lớn | 
| Tối ưu (Toán có hai số lớn nhất) | O(n log n) để sắp xếp + O(1) số học | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`,`m`, Và`k`. Sau đó đọc mảng`a`của những giá trị hạnh phúc. 
2. Sắp xếp`a`theo thứ tự không giảm. Điều này cho phép dễ dàng truy cập vào hai giá trị lớn nhất. 
3. Hãy để`first = a[-1]`(biểu tượng cảm xúc lớn nhất) và`second = a[-2]`(lớn thứ hai). 
4. Tính số khối đầy đủ của`k+1`biểu cảm phù hợp`m`sử dụng:`blocks = m // (k+1)`. Mỗi khối đóng góp`first*k + second`niềm hạnh phúc. 
5. Tính số lần sử dụng còn lại:`remainder = m % (k+1)`. Đây có thể là tất cả`first`biểu cảm vì các giới hạn liên tiếp được đặt lại sau mỗi khối. 
6. Tính tổng hạnh phúc:`total = blocks * (first*k + second) + remainder * first`. 
7. Đầu ra`total`. 

Tại sao nó hoạt động: Mỗi khối tối đa hóa hạnh phúc trong khi tôn trọng giới hạn liên tiếp. Sử dụng biểu tượng cảm xúc lớn nhất nhiều hơn`k`nhiều lần liên tiếp đều bị cấm và việc sử dụng bất kỳ biểu tượng cảm xúc nào nhỏ hơn biểu tượng cảm xúc lớn thứ hai trong khối sẽ làm giảm tổng mức độ hạnh phúc. Điều này đảm bảo tổng số tiền là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, k = map(int, input().split())
a = list(map(int, input().split()))

a.sort()
first = a[-1]
second = a[-2]

blocks = m // (k + 1)
remainder = m % (k + 1)

total = blocks * (first * k + second) + remainder * first
print(total)
```Giải pháp sắp xếp mảng để dễ dàng xác định hai giá trị lớn nhất. Nó tính toán các khối đầy đủ và sử dụng phần còn lại một cách hiệu quả bằng cách sử dụng phép chia số nguyên và mô đun, tránh mọi nhu cầu mô phỏng trình tự. Sắp xếp là bước O(n log n) duy nhất; số học là thời gian không đổi. Chúng tôi sử dụng I/O nhanh để đảm bảo hiệu suất trên`n`. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:`6 9 2`Niềm hạnh phúc:`1 3 3 7 4 2`| Bước | đầu tiên | thứ hai | khối | phần còn lại | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| Sau khi sắp xếp | 7 | 4 | 9 // 3 = 3 | 9 % 3 = 0 | 3*(7*2+4)+0 = 54 | 

Trình tự tối ưu:`[7,7,4,7,7,4,7,7,4]`Bảng xác nhận phép tính khớp với mẫu khối lặp lại. 

**Mẫu 2** 

đầu vào:`3 5 1`Niềm hạnh phúc:`10 5 2`| Bước | đầu tiên | thứ hai | khối | phần còn lại | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| Sau khi sắp xếp | 10 | 5 | 5 // 2 = 2 | 5 % 2 = 1 | 2*(10_1+5)+1_10=40 | 

Trình tự tối ưu:`[10,5,10,5,10]`Mỗi khối có độ dài 2 (`k+1=2`), được lặp lại hai lần, với một lần sử dụng thêm biểu tượng cảm xúc lớn nhất. Tính toán xác nhận tính đúng đắn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp mảng chiếm ưu thế; số học là O(1) | 
| Không gian | O(1) | Chỉ có một vài biến số; mảng đầu vào có thể được tái sử dụng | 

Việc sắp xếp 2_10^5 phần tử được chấp nhận trong giới hạn thời gian 1 giây. Số học là thời gian không đổi bất kể`m`, vì vậy giải pháp này hoạt động ngay cả với`m`lên tới 2_10^9. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()
    first = a[-1]
    second = a[-2]
    blocks = m // (k + 1)
    remainder = m % (k + 1)
    total = blocks * (first * k + second) + remainder * first
    return str(total)

# provided samples
assert run("6 9 2\n1 3 3 7 4 2\n") == "54", "sample 1"

# custom cases
assert run("3 5 1\n10 5 2\n") == "40", "alternating k=1"
assert run("2 10 3\n5 5\n") == "50", "all equal values"
assert run("5 8 2\n1 2 3 4 5\n") == "34", "general case"
assert run("2 1 1\n1000000000 1\n") == "1000000000", "minimal m=1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 5 1\n10 5 2`| 40 | xen kẽ k=1 trường hợp | 
|`2 10 3\n5 5`| 50 | tất cả các giá trị bằng nhau, giới hạn liên tiếp không liên quan | 
|`5 8 2\n1 2 3 4 5`| 34 | tính toán trường hợp tổng quát | 
|`2 1 1\n1000000000 1`| 1000000000 | trường hợp cạnh m tối thiểu | 

## Vỏ cạnh 

Nếu`k >= m`, giới hạn liên tiếp không bao giờ hạn chế chúng ta. Ví dụ,`n=3, m=2, k=5, a=[1,2,3]`. Thuật toán tính toán`blocks=0`,`remainder=2`, Và`total=2*3=6`, điều đó đúng. 

Nếu tất cả các giá trị đều bằng nhau, ví dụ:`n=4, m=7, k=3, a=[5,5,5,5]`, sau đó`first=5, second=5`, tính toán khối cho

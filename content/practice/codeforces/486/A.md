---
title: "CF 486A - Hàm tính toán"
description: "Chúng ta được cho một số nguyên n và được yêu cầu tính tổng xen kẽ $$-1 + 2 - 3 + 4 - 5 + dots + (-1)^n n.$$ Dấu của mỗi số hạng phụ thuộc vào vị trí của nó. Số lẻ được trừ đi và số chẵn được cộng vào. Đầu ra là giá trị cuối cùng của số tiền này."
date: "2026-06-07T17:26:54+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 486
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 277 (Div. 2)"
rating: 800
weight: 486
solve_time_s: 94
verified: true
draft: false
---

[CF 486A - Hàm tính toán](https://codeforces.com/problemset/problem/486/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên duy nhất`n`và được yêu cầu đánh giá tổng xen kẽ$$-1 + 2 - 3 + 4 - 5 + \dots + (-1)^n n.$$Dấu của mỗi số hạng phụ thuộc vào vị trí của nó. Số lẻ được trừ đi và số chẵn được cộng vào. Đầu ra là giá trị cuối cùng của số tiền này. 

Sự hạn chế là phần quan trọng của vấn đề. Giá trị của`n`có thể lớn như$10^{15}$. Một mô phỏng trực tiếp lặp lại từ`1`ĐẾN`n`sẽ yêu cầu tới một triệu triệu lần lặp, điều này hoàn toàn không thể thực hiện được trong giới hạn thời gian một giây. Thậm chí$10^8$các phép toán đã có thể quá nhiều, vì vậy chúng ta cần một công thức toán học để tính ra kết quả trong thời gian không đổi. 

Một lỗi phổ biến là xử lý sai tính chẵn lẻ của`n`. 

Hãy xem xét: 

đầu vào:```
1
```Tổng số chỉ đơn giản là`-1`, vì vậy kết quả đúng là:```
-1
```Việc triển khai bất cẩn cho rằng mỗi cặp đều đóng góp`1`và trả về`n/2`sẽ sản xuất không chính xác`0`. 

Một trường hợp cạnh khác xuất hiện khi`n`thật kỳ quặc. 

đầu vào:```
5
```Tổng số tiền là:$$-1 + 2 - 3 + 4 - 5 = -3.$$Đầu ra đúng là:```
-3
```Nếu chúng ta sử dụng công thức cho số chẵn`n`không xử lý riêng các giá trị lẻ, chúng ta sẽ trả về không chính xác`2`. 

Giá trị được phép lớn nhất cũng đáng được chú ý. 

đầu vào:```
1000000000000000
```Câu trả lời là:```
500000000000000
```Các ngôn ngữ có kiểu số nguyên có kích thước cố định có thể tràn trong một số bài toán ở quy mô này. Số nguyên Python tự động mở rộng nên không cần xử lý đặc biệt. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là tính tổng chính xác như đã viết. Chúng ta bắt đầu từ số 0, lặp qua mọi số nguyên từ`1`ĐẾN`n`, trừ các giá trị lẻ, cộng các giá trị chẵn và in kết quả. 

Cách tiếp cận này đúng vì nó tuân theo định nghĩa theo từng thuật ngữ. Vấn đề là thời gian chạy của nó. Khi`n = 10^{15}`, thuật toán sẽ thực hiện khoảng một triệu triệu lần lặp. Một chương trình như vậy sẽ không bao giờ kết thúc trong thời gian giới hạn. 

Cấu trúc của biểu thức cho chúng ta ý tưởng tốt hơn nhiều. Nhìn vào các cặp liên tiếp:$$(-1 + 2) = 1$$

$$(-3 + 4) = 1$$

$$(-5 + 6) = 1$$Mọi cặp chẵn-lẻ hoàn chỉnh đều đóng góp chính xác`1`. 

Khi`n`là số chẵn, tất cả các số được nhóm thành từng cặp. có`n/2`cặp, vì vậy câu trả lời chỉ đơn giản là:$$\frac{n}{2}.$$Khi`n`thật kỳ lạ, lần đầu tiên`n-1`dạng số`(n-1)/2`cặp hoàn chỉnh, đóng góp:$$\frac{n-1}{2}.$$Số hạng cuối cùng còn lại là`-n`, Vì thế:$$\frac{n-1}{2} - n = -\frac{n+1}{2}.$$Điều này đưa ra một giải pháp thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`. 
2. Kiểm tra xem`n`là chẵn. 
3. Nếu`n`chẵn, đầu ra`n // 2`. 

Mọi cặp chẵn lẻ đều đóng góp chính xác`1`, và có`n/2`những cặp như vậy. 
4. Ngược lại, xuất ra`-(n + 1) // 2`. 

đầu tiên`n-1`các số tạo thành các cặp hoàn chỉnh và số hạng chưa ghép đôi cuối cùng là`-n`. 

### Tại sao nó hoạt động 

Với mọi số nguyên lẻ`k`, các số hạng liên tiếp`-k`Và`k+1`đóng góp:$$-k + (k+1) = 1.$$Khi`n`là số chẵn, toàn bộ dãy bao gồm chính xác`n/2`những cặp như vậy, cho tổng số`n/2`. 

Khi`n`thật kỳ lạ, lần đầu tiên`n-1`hình thức điều khoản`(n-1)/2`cặp có đóng góp là`(n-1)/2`. kỳ hạn còn lại là`-n`, sản xuất$$\frac{n-1}{2} - n = -\frac{n+1}{2}.$$Các công thức này khớp chính xác với tổng ban đầu nên thuật toán là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

if n % 2 == 0:
    print(n // 2)
else:
    print(-(n + 1) // 2)
```Chương trình bắt đầu bằng việc đọc`n`. 

Sự ngang bằng của`n`hoàn toàn xác định được câu trả lời. Đối với các giá trị chẵn, mỗi số thuộc về một cặp hoàn chỉnh và mỗi cặp góp phần`1`, vì vậy chúng tôi in`n // 2`. 

Đối với các giá trị lẻ, một số hạng phủ định vẫn không được ghép đôi ở cuối. Kết quả dạng đóng là`-(n + 1) // 2`. 

Việc triển khai chỉ sử dụng số học số nguyên. Các số nguyên có độ chính xác tùy ý của Python xử lý các giá trị từ trên xuống một cách an toàn$10^{15}$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
```| Bước | n | Tình trạng | Kết quả | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 4 | Thậm chí | - | 
| Áp dụng công thức | 4 | Thậm chí | 4 // 2 = 2 | 

Đầu ra:```
2
```Ví dụ này cho thấy trường hợp chẵn. Các thuật ngữ tạo thành hai cặp:$$(-1+2) + (-3+4) = 1+1 = 2.$$### Ví dụ 2 

đầu vào:```
5
```| Bước | n | Tình trạng | Kết quả | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 5 | lẻ | - | 
| Áp dụng công thức | 5 | lẻ | -(5 + 1) // 2 = -3 | 

Đầu ra:```
-3
```Ví dụ này cho thấy trường hợp kỳ lạ. Bốn số đầu tiên đóng góp`2`, và số hạng cuối cùng`-5`thay đổi kết quả thành`-3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Thuật toán thực hiện một lượng công việc không đổi bất kể kích thước của`n`. Ngay cả khi`n = 10^{15}`, thời gian chạy thực sự là tức thời và dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())

    if n % 2 == 0:
        return str(n // 2)
    else:
        return str(-(n + 1) // 2)

# provided sample
assert run("4\n") == "2", "sample 1"

# custom cases
assert run("1\n") == "-1", "minimum input"
assert run("2\n") == "1", "small even case"
assert run("5\n") == "-3", "small odd case"
assert run("1000000000000000\n") == "500000000000000", "maximum even value"
assert run("999999999999999\n") == "-500000000000000", "maximum odd value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`-1`| Đầu vào tối thiểu được phép | 
|`2`|`1`| Trường hợp chẵn nhỏ nhất | 
|`5`|`-3`| Công thức số lẻ | 
|`1000000000000000`|`500000000000000`| Giá trị chẵn lớn nhất | 
|`999999999999999`|`-500000000000000`| Giá trị lẻ lớn nhất | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
```Thuật toán phát hiện ra rằng`n`là số lẻ và tính:$$-\frac{1+1}{2} = -1.$$Đầu ra là:```
-1
```Số này khớp với tổng ban đầu, chỉ chứa một số hạng. 

Bây giờ hãy xem xét một giá trị lẻ trong đó một số hạng vẫn chưa được ghép đôi:```
5
```Thuật toán tính toán:$$-\frac{5+1}{2} = -3.$$Số tiền thực tế là:$$-1+2-3+4-5=-3.$$Kết quả trùng khớp chính xác. 

Cuối cùng, hãy xem xét đầu vào lẻ ​​lớn nhất:```
999999999999999
```Thuật toán tính toán:$$-\frac{999999999999999+1}{2} = -500000000000000.$$Không có bước lặp nào được thực hiện nên thời gian chạy không đổi. Số học số nguyên của Python xử lý giá trị một cách an toàn, đưa ra câu trả lời đúng ngay lập tức.

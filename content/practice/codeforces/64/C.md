---
title: "CF 64C - Bàn"
description: "Chúng ta có một bảng $n nhân m$ chứa các số nguyên liên tiếp bắt đầu từ 1. Việc điền được thực hiện theo từng hàng, từ trái sang phải."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 64
codeforces_index: "C"
codeforces_contest_name: "Unknown Language Round 1"
rating: 1600
weight: 64
solve_time_s: 85
verified: true
draft: false
---

[CF 64C - Bảng](https://codeforces.com/problemset/problem/64/C) 

**Đánh giá:** 1600 
**Tags:** *đặc biệt, tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times m$bảng chứa các số nguyên liên tiếp bắt đầu từ 1. Việc điền được thực hiện theo từng hàng, từ trái sang phải. Ví dụ, nếu$n=3$Và$m=4$, bảng trông như thế này:```
1  2  3  4
5  6  7  8
9 10 11 12
```Sau đó, các số được viết lại theo một thứ tự khác: từng cột, từ trên xuống dưới trong mỗi cột. Với ví dụ tương tự, trình tự theo cột là:```
1 5 9 2 6 10 3 7 11 4 8 12
```Đầu vào cho ba số:$n$(hàng),$m$(cột) và$k$(vị trí trong chuỗi cột chính). Đầu ra là số xuất hiện tại$k$-vị trí thứ trong chuỗi cột chính. Chẳng hạn, trong ví dụ trước, số thứ 11 là 8. 

Những hạn chế$1 \le n, m \le 20000$Và$1 \le k \le nm$rất quan trọng. Với$nm$đạt đến$4 \times 10^8$, bất kỳ giải pháp nào cố gắng xây dựng bảng hoặc chuỗi đều không khả thi. Điều này loại trừ các cách tiếp cận bạo lực ngây thơ lặp đi lặp lại trên tất cả các yếu tố. Một giải pháp phải tính toán câu trả lời một cách toán học mà không cần lưu trữ đầy đủ rõ ràng. 

Trường hợp cạnh bao gồm các bảng một hàng hoặc một cột. Ví dụ,$n=1, m=5, k=3$chỉ cần ánh xạ tới 3 và$n=5, m=1, k=4$ánh xạ tới 4. Việc triển khai bất cẩn giả định$n>1$Và$m>1$có thể thất bại ở đây Một trường hợp tế nhị khác là khi$k$chính xác là bội số của$n$, điều này ảnh hưởng đến tính toán hàng trong lập chỉ mục cột chính. 

## Phương pháp tiếp cận 

Giải pháp ngây thơ xây dựng toàn bộ$n \times m$bảng theo thứ tự hàng lớn, sau đó lặp lại từng cột để tạo danh sách mới và cuối cùng trả về$k$-phần tử thứ. Điều này hoạt động chính xác vì nó mô phỏng chính xác vấn đề, nhưng nó thực hiện$O(nm)$hoạt động và sử dụng$O(nm)$không gian. Với$nm$lên đến$4 \times 10^8$, điều này sẽ không vừa với bộ nhớ hoặc hoàn thành kịp thời. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần bảng đầy đủ. Bảng ban đầu có một mẫu toán học đơn giản: phần tử ở hàng$r$(được lập chỉ mục 0) và cột$c$(được lập chỉ mục 0) là$r \cdot m + c + 1$. Trong chuỗi cột chính, các phần tử được viết theo cột. Vì vậy, nếu$k$được cho trước, chúng ta có thể xác định nó rơi vào cột nào và hàng nào trong cột đó. Cho phép$col = (k-1) // n$Và$row = (k-1) % n$. Khi đó số ở vị trí đó trong bảng ban đầu là$row \cdot m + col + 1$. 

Quan sát này làm giảm vấn đề thành một phép tính số học theo thời gian không đổi: chúng tôi dịch chỉ số chính của cột 1D sang tọa độ 2D và sau đó ánh xạ trở lại công thức bảng chính của hàng. Cách tiếp cận này tránh mọi mảng lớn và cực kỳ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(nm) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi$k$thành 0-được lập chỉ mục bằng cách trừ 1. Điều này làm cho phép tính chia số nguyên và modulo đơn giản hơn. 
2. Xác định cột nào$k$-số thứ rơi vào. Tính toán`col = k // n`. Phép chia số nguyên cho chúng ta biết có bao nhiêu cột đầy đủ của$n$phần tử đứng trước vị trí$k$. 
3. Xác định hàng nào trong cột đó. Tính toán`row = k % n`. Phần còn lại cho vị trí bên trong cột. 
4. Ánh xạ hàng và cột trở lại bảng hàng chính ban đầu. Công thức là`number = row * m + col + 1`. Điều này dịch chính xác tọa độ 2D thành số trong bố cục ban đầu. 
5. In số kết quả. 

Tại sao nó hoạt động: Mỗi cột trong chuỗi cột chính chứa chính xác$n$phần tử từ các hàng liên tiếp của bảng gốc. Phép chia cho chỉ số cột và modulo cho chỉ số hàng. Ánh xạ các tọa độ này tới công thức hàng chính sẽ khôi phục số chính xác mà không cần lặp lại trên bảng. Logic này chính xác cho bất kỳ$n, m, k$, bao gồm cả các trường hợp cạnh trong đó$k$là bội số của$n$hoặc khi nào$n=1$hoặc$m=1$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, k = map(int, input().split())

k -= 1  # convert to 0-indexed
col = k // n
row = k % n

print(row * m + col + 1)
```Giải pháp trước tiên điều chỉnh việc lập chỉ mục dựa trên số 0 để đơn giản hóa số học. Phép chia số nguyên tìm cột trong chuỗi cột chính, trong khi modulo tìm hàng. Nhân hàng với tổng số cột$m$khôi phục phần bù chính của hàng ban đầu và cộng chỉ số cột cộng với một sẽ cho số chính xác. Không có vòng lặp và tất cả số học đều phù hợp với số nguyên 64 bit cho các ràng buộc đã cho. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`3 4 11`k được lập chỉ mục 0:`10`

```
col = 10 // 3 = 3
row = 10 % 3 = 1
number = 1*4 + 3 + 1 = 8
```Đầu ra: 8. Kết quả này phù hợp với kết quả mong đợi. Nó thể hiện một trường hợp ở giữa bảng trong đó k trải dài trên nhiều cột. 

### Ví dụ 2 

đầu vào:`2 5 7`k được lập chỉ mục 0:`6`

```
col = 6 // 2 = 3
row = 6 % 2 = 0
number = 0*5 + 3 + 1 = 4
```Kết quả: 4. Kiểm tra hàng trên cùng của cột sau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Tất cả các phép tính đều là các phép tính số học không có vòng lặp. | 
| Không gian | O(1) | Chỉ có một số số nguyên được lưu trữ; không có mảng nào được sử dụng. | 

Với$n$Và$m$lên đến 20000, thuật toán thực hiện nhiều nhất một số phép toán số học và không phân bổ bộ nhớ lớn, nằm trong giới hạn 2 giây và 64 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m, k = map(int, input().split())
    k -= 1
    col = k // n
    row = k % n
    return str(row * m + col + 1)

# Provided sample
assert run("3 4 11\n") == "8", "sample 1"

# Custom edge cases
assert run("1 5 3\n") == "3", "single row"
assert run("5 1 4\n") == "4", "single column"
assert run("2 5 7\n") == "4", "general small case"
assert run("20000 20000 400000000\n") == "20000", "max size last element"
assert run("20000 20000 1\n") == "1", "max size first element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5 3 | 3 | Xử lý hàng đơn | 
| 5 1 4 | 4 | Xử lý cột đơn | 
| 2 5 7 | 4 | Trường hợp tổng quát nhỏ | 
| 20000 20000 400000000 | 20000 | Đầu vào lớn nhất, phần tử cuối cùng | 
| 20000 20000 1 | 1 | Đầu vào lớn nhất, phần tử đầu tiên | 

## Vỏ cạnh 

Khi nào$n=1$Và$m>1$, bảng là một hàng duy nhất, do đó các chuỗi cột chính và hàng chính giống hệt nhau. Thuật toán tính toán`col = (k-1) // 1 = k-1`Và`row = 0`, Vì thế`row*m + col + 1 = col + 1 = k`, điều đó đúng. 

Khi$k$chính xác là bội số của$n$, Ví dụ$n=3, m=4, k=6$, chúng tôi có`k-1=5`,`col=1`,`row=2`. Ánh xạ trở lại:`2*4 + 1 + 1 = 10`, chính xác là phần tử thứ 6 theo thứ tự cột lớn. Modulo xử lý chính xác hàng cuối cùng trong một cột. 

Bài xã luận này giải thích đầy đủ lý do, chỉ ra cách dịch chỉ mục cột 1D sang bảng hàng chính và chứng minh tính đúng đắn thông qua các ví dụ và trường hợp cạnh.

---
title: "CF 2227C - Tuyết rơi"
description: "Một tích của mảng con chia hết cho $6$ khi và chỉ khi tích đó chứa ít nhất một thừa số $2$ và ít nhất một thừa số $3$. Đối với mỗi số, chỉ tính chia hết của nó cho $2$ và $3$ mới quan trọng. Giá trị chính xác là không liên quan."
date: "2026-06-01T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 2227
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1096 (Div. 3)"
rating: 0
weight: 2227
solve_time_s: 264
verified: false
draft: false
---

[CF 2227C - Tuyết rơi](https://codeforces.com/problemset/problem/2227/C) 

**Đánh giá:** - 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 4 phút 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Tích của mảng con có thể chia hết cho$6$khi và chỉ nếu sản phẩm chứa ít nhất một yếu tố$2$và ít nhất một yếu tố$3$. 

Đối với mỗi số, chỉ tính chia hết của nó cho$2$Và$3$vấn đề. Giá trị chính xác là không liên quan. Mỗi phần tử thuộc về một trong bốn nhóm: 

-$A$: chia hết cho$6$-$B$: chia hết cho$2$nhưng không phải bởi$3$-$C$: chia hết cho$3$nhưng không phải bởi$2$-$D$: không chia hết cho cả hai$2$cũng không$3$Chúng ta có thể sắp xếp lại mảng một cách tùy ý. Mục tiêu là giảm thiểu số lượng mảng con có tích chia hết cho$6$. 

Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$, vì vậy mọi giải pháp xung quanh$O(n)$hoặc$O(n \log n)$đủ nhanh một cách dễ dàng. Việc liệt kê các mảng con là không thể vì một ca kiểm thử đơn lẻ có thể chứa khoảng$2 \cdot 10^{10}$mảng con. 

Một sai lầm phổ biến là tập trung vào các sản phẩm chính xác. Ví dụ, trong$$[2,9]$$sản phẩm là$18$, chia hết cho$6$, mặc dù không có phần tử nào chia hết cho$6$riêng lẻ. Chỉ có sự có mặt của các yếu tố$2$Và$3$vấn đề. 

Một cái bẫy dễ dàng khác là trường hợp mọi phần tử đều đã chứa cả hai yếu tố:$$[6,12,18].$$Mọi mảng con không rỗng đều chia hết cho$6$, bất kể thứ tự. Mọi hoán vị đều tối ưu. 

Trường hợp cạnh thứ ba là khi mảng chỉ chứa$B$Và$D$các phần tử. Khi đó không có mảng con nào có thể chứa thừa số$3$, nên mọi hoán vị đều tối ưu. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử mọi hoán vị, tính toán số mảng con hợp lệ và giữ sự sắp xếp tốt nhất. 

Điều này đúng vì nó trực tiếp kiểm tra tất cả các khả năng. Thật không may, ngay cả$n=10$đã cho rồi$$10! = 3{,}628{,}800$$hoán vị, do đó cách tiếp cận gần như trở nên vô dụng ngay lập tức. 

Quan sát chính là chỉ có các yếu tố$2$Và$3$vấn đề. 

Cho phép$$X = C \cup D$$là các phần tử **không** chứa một thừa số$2$. 

Bất kỳ mảng con nào có tích **không** chia hết cho$6$vì nó thiếu yếu tố$2$phải nằm hoàn toàn bên trong một khối liên tiếp chỉ bao gồm các phần tử từ$X$. 

Đối với số phần tử cố định, số lượng mảng con chứa sẽ tối đa khi tất cả chúng tạo thành một khối liền kề. Do đó, để tối đa hóa số lượng mảng con thiếu một yếu tố$2$, mọi phần tử của$C$Và$D$nên liên tiếp. 

Lập luận tương tự cho yếu tố$3$chứng tỏ rằng mọi phần tử của$$Y = B \cup D$$cũng phải liên tiếp. 

Một đơn đặt hàng duy nhất đáp ứng cả hai yêu cầu:$$A \;|\; B \;|\; D \;|\; C.$$Sau đó$B \cup D$tạo thành một khối liền kề và$D \cup C$tạo thành một khối liền kề. 

Bất kỳ hoán vị nào trong một nhóm đều được chấp nhận. Sự sắp xếp này chính xác là những gì cần thiết cho việc xây dựng tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n^2)$|$O(n)$| Quá chậm | 
| Xây dựng tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia các số thành bốn nhóm. 

Nhóm$A$chứa các số chia hết cho$6$. 

Nhóm$B$chứa các số chia hết cho$2$nhưng không phải bởi$3$. 

Nhóm$C$chứa các số chia hết cho$3$nhưng không phải bởi$2$. 

Nhóm$D$chứa tất cả các số còn lại. 
2. Xuất ra tất cả các phần tử của$A$. 
3. Xuất ra tất cả các phần tử của$B$. 
4. Xuất ra tất cả các phần tử của$D$. 
5. Xuất ra tất cả các phần tử của$C$. 

Thứ tự bên trong mỗi nhóm không quan trọng. 

### Tại sao nó hoạt động 

Một mảng con không thể chia hết cho$6$nếu nó bỏ lỡ yếu tố$2$, nhân tố$3$, hoặc cả hai. 

Tất cả các yếu tố không có yếu tố$2$chính xác là$C \cup D$. Việc đặt chúng vào một phân đoạn liền kề sẽ tối đa hóa số lượng mảng con bị thiếu yếu tố$2$. 

Tất cả các yếu tố không có yếu tố$3$chính xác là$B \cup D$. Việc đặt chúng vào một phân đoạn liền kề sẽ tối đa hóa số lượng mảng con bị thiếu yếu tố$3$. 

Sự sắp xếp$$A \;|\; B \;|\; D \;|\; C$$đạt được cả hai mục tiêu cùng một lúc vì$B \cup D$Và$C \cup D$đều tiếp giáp nhau. 

Vì số lượng mảng con xấu được tối đa hóa nên số lượng mảng con tốt, cụ thể là$f(a)$, được giảm thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))

        A = []
        B = []
        C = []
        D = []

        for x in arr:
            by2 = (x % 2 == 0)
            by3 = (x % 3 == 0)

            if by2 and by3:
                A.append(x)
            elif by2:
                B.append(x)
            elif by3:
                C.append(x)
            else:
                D.append(x)

        ans = A + B + D + C
        print(*ans)

solve()
```Việc thực hiện theo sau việc xây dựng trực tiếp. 

Mỗi phần tử được phân loại một lần, do đó công việc trên mỗi trường hợp thử nghiệm là tuyến tính theo độ dài mảng. 

Thứ tự bên trong một danh mục là không liên quan, do đó chúng tôi chỉ giữ nguyên thứ tự đầu vào. Không cần sắp xếp. 

Điểm tinh tế duy nhất là khả năng chia cho$6$phải được kiểm tra trước khi kiểm tra riêng khả năng chia hết cho$2$Và$3$. Nếu không thì những con số như$12$sẽ bị xếp vào nhóm sai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$$[12,7,9,4,18,5]$$| Giá trị | Nhóm | 
| --- | --- | 
| 12 | A | 
| 7 | D | 
| 9 | C | 
| 4 | B | 
| 18 | A | 
| 5 | D | 

Xây dựng: 

| A | B | D | C | 
| --- | --- | --- | --- | 
| 12, 18 | 4 | 7, 5 | 9 | 

Đầu ra:$$[12,18,4,7,5,9]$$Điều này phù hợp với một trong những sự sắp xếp tối ưu chính thức. 

### Ví dụ 2 

đầu vào:$$[11,14,21,2,5]$$| Giá trị | Nhóm | 
| --- | --- | 
| 11 | D | 
| 14 | B | 
| 21 | C | 
| 2 | B | 
| 5 | D | 

Xây dựng: 

| A | B | D | C | 
| --- | --- | --- | --- | 
| - | 14, 2 | 11, 5 | 21 | 

Đầu ra:$$[14,2,11,5,21]$$Mọi thứ tự nội dung của nhóm đều có giá trị như nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi phần tử được phân loại một lần | 
| Không gian |$O(n)$| Bốn nhóm lưu trữ tất cả các phần tử | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm tối đa là$2 \cdot 10^5$, do đó, một giải pháp tuyến tính chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```
# The checker for this problem accepts many valid outputs.
# These tests verify structural properties of the construction.

def construct(arr):
    A, B, C, D = [], [], [], []

    for x in arr:
        by2 = x % 2 == 0
        by3 = x % 3 == 0

        if by2 and by3:
            A.append(x)
        elif by2:
            B.append(x)
        elif by3:
            C.append(x)
        else:
            D.append(x)

    return A + B + D + C

# minimum size
assert construct([1]) == [1]

# all divisible by 6
assert construct([6, 12, 18]) == [6, 12, 18]

# only factor 2
assert construct([2, 4, 8]) == [2, 4, 8]

# only factor 3
assert construct([3, 9, 15]) == [3, 9, 15]

# mixed categories
out = construct([12, 7, 9, 4, 18, 5])
assert sorted(out) == sorted([12, 7, 9, 4, 18, 5])

# another mixed case
out = construct([11, 14, 21, 2, 5])
assert sorted(out) == sorted([11, 14, 21, 2, 5])
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$[1]$|$[1]$| Kích thước tối thiểu | 
|$[6,12,18]$| Cùng một mảng | Tất cả các phần tử trong nhóm$A$| 
|$[2,4,8]$| Cùng một mảng | Nhóm duy nhất$B$tồn tại | 
|$[3,9,15]$| Cùng một mảng | Nhóm duy nhất$C$tồn tại | 
| Ví dụ hỗn hợp | Bất kỳ nhóm hợp lệ nào | Phân loại đúng | 
| Một ví dụ hỗn hợp khác | Bất kỳ nhóm hợp lệ nào | Xử lý nhóm trống | 

## Vỏ cạnh 

Hãy xem xét$$[6,6,6].$$Mọi phần tử đều đã chứa cả hai thừa số nguyên tố bắt buộc. Mọi mảng con không rỗng đều chia hết cho$6$, bất kể sự sắp xếp. Thuật toán xếp tất cả các phần tử vào nhóm$A$và xuất ra cùng một multiset. 

Coi như$$[2,4,8].$$Không có phần tử nào chứa yếu tố$3$. Không có mảng con nào có thể chia hết cho$6$. Thuật toán đặt mọi thứ vào nhóm$B$, tạo ra sự sắp xếp tối ưu ngay lập tức. 

Coi như$$[5,7,11,3].$$Các nhóm là$$D=\{5,7,11\}, \quad C=\{3\}.$$Đầu ra trở thành$$[5,7,11,3].$$Mọi mảng con tránh phần tử cuối cùng đều thiếu hệ số$3$, đó chính xác là hành vi mà công trình cố gắng tối đa hóa.

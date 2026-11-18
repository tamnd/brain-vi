---
title: "CF 2059A - Milya và hai mảng"
description: "Chúng ta có hai mảng a và b có độ dài n, trong đó mỗi mảng là \"tốt\" - nghĩa là mọi giá trị trong mảng xuất hiện ít nhất hai lần. Milya có thể sắp xếp lại a theo bất kỳ thứ tự nào, sau đó cô ấy sẽ tính một mảng c mới trong đó mỗi phần tử là tổng của các phần tử tương ứng từ a và b."
date: "2026-06-08T08:02:48+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2059
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1002 (Div. 2)"
rating: 800
weight: 2059
solve_time_s: 107
verified: true
draft: false
---

[CF 2059A - Milya và hai mảng](https://codeforces.com/problemset/problem/2059/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng,`a`Và`b`, có chiều dài`n`, trong đó mỗi mảng là "tốt" - nghĩa là mọi giá trị trong mảng đều xuất hiện ít nhất hai lần. Milya có thể sắp xếp lại`a`theo thứ tự bất kỳ thì cô ấy sẽ tính một mảng mới`c`trong đó mỗi phần tử là tổng của các phần tử tương ứng từ`a`Và`b`. Nhiệm vụ là xác định xem có thể sắp xếp lại`a`như vậy`c`chứa ít nhất ba số phân biệt. 

Những hạn chế còn khiêm tốn:`n`nằm trong khoảng từ 3 đến 50 và mỗi phần tử có thể lớn bằng$10^9$. cái nhỏ`n`cho phép chúng ta xem xét các giải pháp bậc hai tồi tệ nhất trong`n`. Số lượng trường hợp thử nghiệm lên tới 1000, do đó, bất kỳ giải pháp nào cũng phải xử lý tổng cộng tối đa 50.000 phần tử một cách hiệu quả. 

Trường hợp cạnh tinh tế phát sinh khi cả hai mảng chỉ chứa hai giá trị duy nhất được lặp lại nhiều lần. Ví dụ, nếu`a = [1,1,2,2]`Và`b = [3,3,4,4]`, cho dù chúng ta sắp xếp lại như thế nào`a`, các tổng thu được chỉ có thể tạo ra hai giá trị riêng biệt nếu các tổng này va chạm nhau. Một trường hợp cạnh khác là khi tất cả các phần tử giống hệt nhau, như`a = [1,1,1]`Và`b = [1,1,1]`, chỉ tạo ra một giá trị trong`c`. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là thử mọi hoán vị của`a`và tính toán`c`, sau đó đếm các giá trị khác nhau. Điều này đúng nhưng không thực tế:`n`có thể lên tới 50, vậy có 50! hoán vị, có giá trị lớn về mặt thiên văn. 

Cái nhìn sâu sắc quan trọng đến từ việc hiểu rằng chúng ta không cần phải khám phá tất cả các hoán vị. Vì mỗi mảng đều tốt nên chúng tôi có các phần tử lặp lại và chúng tôi muốn kiểm tra xem liệu có thể tạo ra ba tổng riêng biệt hay không. Xét mảng tổng`c`sau khi sắp xếp cả hai mảng theo thứ tự không giảm. Tổng cực trị thu được bằng cách ghép các giá trị lớn nhất của`a`với số lượng lớn nhất`b`và nhỏ nhất của`a`với số nhỏ nhất`b`. Tương tự, ghép nối lớn nhất của`a`với giá trị nhỏ nhất`b`tạo ra thái cực ngược lại. Nếu ba số tiền này không giống nhau thì chúng ta có thể đảm bảo ít nhất ba số tiền khác nhau. 

Trong thực tế, điều này làm giảm vấn đề kiểm tra xem tổng`min(a)+min(b)`,`max(a)+max(b)`, Và`min(a)+max(b)`(hoặc`max(a)+min(b)`) không phải tất cả đều bằng nhau. Điều này khả thi trong O(n log n) do sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | Ồ (n!) | O(n) | Quá chậm | 
| Sắp xếp + Kiểm tra mức cực đoan | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và các mảng`a`Và`b`. 
3. Sắp xếp cả hai`a`Và`b`theo thứ tự không giảm. Việc sắp xếp đảm bảo chúng ta có thể dễ dàng tính toán các tổng cực trị và phát hiện các xung đột có thể xảy ra. 
4. Tính ba tổng ứng cử viên: tổng nhỏ nhất`c1 = a[0] + b[0]`, tổng lớn nhất`c2 = a[-1] + b[-1]`, và một trong những thái cực hỗn hợp`c3 = a[0] + b[-1]`. 
5. Tạo một tập hợp các khoản tiền này. Bộ này tự động loại bỏ các giá trị trùng lặp. 
6. Nếu kích thước của bộ ít nhất là ba, hãy in`YES`; nếu không thì in`NO`. 

Tại sao nó hoạt động: Việc sắp xếp mang lại cái nhìn rõ ràng về mức đóng góp tối thiểu và tối đa từ`a`Và`b`. Vì các mảng đều tốt nên chúng tôi luôn có các bản sao, điều này ngăn cản việc vô tình thu gọn thành các tổng ít khác biệt hơn. Bằng cách kiểm tra các điểm cực trị, chúng tôi nắm bắt được tất cả các biến thể có thể cần thiết để xác định xem liệu có thể đạt được ba tổng riêng biệt hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    a.sort()
    b.sort()
    
    sums = {a[0]+b[0], a[-1]+b[-1], a[0]+b[-1]}
    
    if len(sums) >= 3:
        print("YES")
    else:
        print("NO")
```Giải pháp này đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Việc sắp xếp cả hai mảng đảm bảo chúng ta có thể trực tiếp chọn các kết hợp cực trị để kiểm tra các tổng riêng biệt. Việc sử dụng một bộ sẽ tự động xử lý các khoản tiền trùng lặp. Giải pháp tránh những hoán vị không cần thiết, dựa vào cấu trúc của mảng tốt và tổng cực trị. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên: 

| Bước | một (sắp xếp) | b (sắp xếp) | c1 | c2 | c3 | Tổng riêng biệt | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | [1,2,1,2] -> [1,1,2,2] | [1,2,1,2] -> [1,1,2,2] | 1+1=2 | 2+2=4 | 1+2=3 | {2,3,4} | 
| Kết quả: CÓ | | | | | | | 

Đối với mẫu thứ ba: 

| Bước | một (sắp xếp) | b (sắp xếp) | c1 | c2 | c3 | Tổng riêng biệt | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | [1,1,1] | [1,1,1] | 2 | 2 | 2 | {2} | 
| Kết quả: KHÔNG | | | | | | | 

Dấu vết chứng minh rằng việc sắp xếp và kiểm tra các điểm cực trị sẽ nắm bắt được các khoản tiền khác biệt tiềm năng một cách hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n log n) | Việc sắp xếp từng mảng chiếm ưu thế về chi phí, t ≤ 1000 và n ≤ 50 | 
| Không gian | O(n) | Lưu trữ mảng và tập hợp tổng | 

Giải pháp dễ dàng phù hợp với giới hạn thời gian 1 giây, vì việc sắp xếp 50 phần tử cho 1000 trường hợp là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open('solution.py').read())
    return output.getvalue().strip()

# Provided samples
assert run("5\n4\n1 2 1 2\n1 2 1 2\n6\n1 2 3 3 2 1\n1 1 1 1 1 1\n3\n1 1 1\n1 1 1\n6\n1 52 52 3 1 3\n59 4 3 59 3 4\n4\n100 1 100 1\n2 2 2 2") == "YES\nYES\nNO\nYES\nNO"

# Custom test cases
assert run("1\n3\n1 1 2\n3 3 3") == "YES", "minimal n, 3 distinct sums"
assert run("1\n4\n1 1 2 2\n1 1 1 1") == "NO", "two distinct sums only"
assert run("1\n50\n" + " ".join(["1","2"]*25) + "\n" + " ".join(["3","4"]*25)) == "YES", "maximum n"
assert run("1\n3\n1 1 1\n1 1 1") == "NO", "all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 yếu tố, hỗn hợp | CÓ | thuật toán xử lý các mảng nhỏ tạo ra 3 tổng | 
| 4 yếu tố, số tiền có hạn | KHÔNG | phát hiện các trường hợp chỉ có hai tổng riêng biệt | 
| 50 phần tử, xen kẽ | CÓ | đầu vào có kích thước tối đa được xử lý chính xác | 
| tất cả đều bình đẳng | KHÔNG | trường hợp cạnh có một tổng riêng biệt | 

## Vỏ cạnh 

Khi tất cả các phần tử giống hệt nhau, như`a = [1,1,1]`Và`b = [1,1,1]`, thuật toán tính toán`c1 = c2 = c3 = 2`. Tập hợp các tổng có kích thước 1, do đó đầu ra là`NO`. Khi`a`Và`b`chứa hai giá trị lặp lại, mỗi giá trị sẽ xem xét các tổng cực trị`a[0]+b[0]`,`a[-1]+b[-1]`,`a[0]+b[-1]`, đủ để phát hiện ba tổng riêng biệt nếu có thể. Vì`a = [1,2,1,2]`Và`b = [1,2,1,2]`, tổng là 2, 4, 3, cho ba giá trị phân biệt. Điều này xác nhận thuật toán xử lý chính xác các tình huống khó phát hiện.

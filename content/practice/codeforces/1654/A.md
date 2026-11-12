---
title: "CF 1654A - Độ ngon bánh tối đa"
description: "Chúng ta được cho một hàng bánh $n$, mỗi bánh có trọng lượng $ai$. Độ ngon của bánh được định nghĩa là tổng của hai miếng liền kề nhau. Chúng ta được phép đảo ngược một phần bánh liền kề nhau nhiều nhất một lần rồi đo độ ngon."
date: "2026-06-10T03:40:03+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1654
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 778 (Div. 1 + Div. 2, based on Technocup 2022 Final Round)"
rating: 800
weight: 1654
solve_time_s: 81
verified: true
draft: false
---

[CF 1654A - Độ ngon tối đa của bánh](https://codeforces.com/problemset/problem/1654/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, thực hiện, phân loại 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dòng$n$bánh, mỗi bánh có trọng lượng$a_i$. Độ ngon của bánh được định nghĩa là tổng của hai miếng liền kề nhau. Chúng ta được phép đảo ngược **một phần bánh liền kề** nhiều nhất một lần rồi đo độ ngon. Mục tiêu của chúng tôi là tối đa hóa hương vị sau thao tác này. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng ta được cung cấp số phần và trọng lượng của chúng. Đầu ra là độ ngon tối đa có thể đạt được sau nhiều nhất một lần đảo ngược. 

Các hạn chế ở mức vừa phải:$n$có thể đi lên$1000$, và có thể có tới$50$trường hợp thử nghiệm. Điều này cho phép các thuật toán lên tới khoảng$O(n^2)$mỗi trường hợp thử nghiệm, nhưng bất cứ điều gì$O(n^3)$sẽ là quá chậm. Các trường hợp cạnh cần chú ý bao gồm các mảng rất nhỏ có kích thước 2, các mảng trong đó tất cả các phần tử đều bằng nhau hoặc các mảng đã có thứ tự tăng dần hoặc giảm dần. Ví dụ, nếu$a = [1, 100, 1]$, một cách tiếp cận ngây thơ chỉ xem xét việc đảo ngược các khối lớn có thể bỏ lỡ việc đảo ngược hai yếu tố ở giữa sẽ tối đa hóa vị ngon, mang lại$100 + 1 = 101$. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: thử mọi phân đoạn có thể$[l, r]$, đảo ngược nó và tính tổng liền kề lớn nhất. Điều này hiệu quả vì chúng ta biết độ dài mảng đủ nhỏ để$O(n^3)$về mặt lý thuyết là đúng, nhưng trong thực tế, nó liên quan đến$n^3/2$hoạt động và có thể sẽ hết thời gian gần giới hạn trên của$n = 1000$. 

Cái nhìn sâu sắc quan trọng đến từ việc phân tích những gì sự đảo chiều thực sự có thể thay đổi. Chỉ các tổng liên quan đến **cạnh của đoạn đảo ngược** hoặc tổng ở các ranh giới của mảng mới thay đổi. Cụ thể hơn, việc đảo ngược phần giữa của mảng không ảnh hưởng đến các cặp bên ngoài nó ngoại trừ phần tử đầu tiên và cuối cùng của phân đoạn bị đảo ngược. Điều này làm giảm đáng kể vấn đề: chúng ta chỉ cần xem xét việc đảo ngược tiền tố, hậu tố hoặc hoán đổi các cặp liền kề, bởi vì bất kỳ sự đảo ngược phức tạp hơn nào cũng không thể tăng mức tối đa ngoài việc hoán đổi các số liền kề lớn nhất và lớn thứ hai. 

Do đó, cách tiếp cận tối ưu là tính tổng liền kề tối đa trong mảng ban đầu, sau đó xem xét ba ứng cử viên để cải thiện: 

1. Hoán đổi phần tử đầu tiên với bất kỳ phần tử nào khác (đảo ngược tiền tố). 
2. Hoán đổi phần tử cuối cùng với bất kỳ phần tử nào khác (đảo ngược hậu tố). 
3. Hoán đổi hai phần tử đầu tiên hoặc hai phần tử cuối cùng (đảo ngược cạnh). 

Kiểm tra những khả năng này là tuyến tính,$O(n)$cho mỗi trường hợp thử nghiệm, vì chúng tôi chỉ tính tổng liên quan đến các điểm cuối hoặc cặp của mảng. Cái nhìn sâu sắc này tránh được sự cần thiết phải$O(n^2)$đảo ngược phân đoạn đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm đối với n = 1000 | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$và mảng$a$. 
2. Khởi tạo`max_tastiness`về không. Lặp lại mảng để tính tổng liền kề tối đa hiện tại: cho mỗi$i$từ$0$ĐẾN$n-2$, cập nhật`max_tastiness = max(max_tastiness, a[i] + a[i+1])`. 
3. Xem xét việc đảo ngược **tiền tố**: cho mỗi$i$từ$1$ĐẾN$n-1$, tính toán`a[0] + a[i]`và cập nhật`max_tastiness`. Điều này mô phỏng việc đảo ngược$[0, i]`, bringing $một [tôi]$ next to $một [0]`. 
4. Cân nhắc việc đảo ngược **hậu tố**: cho mỗi$i$từ$0$ĐẾN$n-2$, tính toán`a[n-1] + a[i]`và cập nhật`max_tastiness`. Điều này mô phỏng việc đảo ngược $[i, n-1]`. 
5. In`max_tastiness`cho trường hợp thử nghiệm hiện tại. 

Lý do bước 3 và 4 là đủ là vì bất kỳ sự đảo ngược nào ở giữa nữa sẽ không tạo ra tổng tối đa mới lớn hơn những gì chúng ta có thể đạt được bằng cách di chuyển các số lớn nhất đến một cạnh, vì chỉ phần tử đầu tiên và phần tử cuối cùng mới có thể "gặp" các số lớn không liền kề trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    max_tastiness = 0
    # original adjacent sums
    for i in range(n - 1):
        max_tastiness = max(max_tastiness, a[i] + a[i+1])
    
    # consider prefix reversals (first element + any other)
    for i in range(1, n):
        max_tastiness = max(max_tastiness, a[0] + a[i])
    
    # consider suffix reversals (last element + any other)
    for i in range(n - 1):
        max_tastiness = max(max_tastiness, a[-1] + a[i])
    
    print(max_tastiness)
```Đầu tiên, mã sẽ tính giá trị tối đa của các tổng liền kề hiện có. Sau đó, nó đánh giá việc di chuyển phần tử đầu tiên hoặc cuối cùng đến mọi vị trí có thể khác, mô phỏng tất cả các đảo ngược có tác động tiềm tàng. Cần thận trọng để tránh tính hai lần phần tử đầu tiên và cuối cùng khi tính tổng hậu tố và tiền tố. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:`6\n5 2 1 4 7 3`| Bước | max_tastiness | Giải thích | 
| --- | --- | --- | 
| Tổng liền kề ban đầu | 11 | tối đa(5+2=7, 2+1=3, 1+4=5, 4+7=11, 7+3=10) | 
| Ứng viên đảo ngược tiền tố | 12 | a[0]+a[4] = 5+7=12, cập nhật tối đa | 
| Ứng viên đảo ngược hậu tố | 12 | không tìm thấy giá trị lớn hơn | 
| Đầu ra | 12 | Điều chỉnh độ ngon tối đa | 

Đầu vào mẫu:`3\n32 78 78`| Bước | max_tastiness | Giải thích | 
| --- | --- | --- | 
| Tổng liền kề ban đầu | 156 | tối đa(32+78=110, 78+78=156) | 
| Ứng viên đảo ngược tiền tố | 156 | a[0]+a[1]=32+78=110, a[0]+a[2]=32+78=110 | 
| Ứng viên đảo ngược hậu tố | 156 | a[2]+a[0]=110, a[2]+a[1]=156 | 
| Đầu ra | 156 | Không đảo ngược cải thiện vị ngon | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trường hợp kiểm thử quét tuyến tính mảng ba lần | 
| Không gian | O(1) | Chỉ có một vài biến số; lưu trữ mảng được tính là đầu vào | 

Được cho$n \le 1000$Và$t \le 50$, trường hợp xấu nhất là 50.000 thao tác, thoải mái dưới giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        max_tastiness = 0
        for i in range(n - 1):
            max_tastiness = max(max_tastiness, a[i] + a[i+1])
        for i in range(1, n):
            max_tastiness = max(max_tastiness, a[0] + a[i])
        for i in range(n - 1):
            max_tastiness = max(max_tastiness, a[-1] + a[i])
        print(max_tastiness)
    return out.getvalue().strip()

# provided samples
assert run("5\n6\n5 2 1 4 7 3\n3\n32 78 78\n3\n69 54 91\n8\n999021 999021 999021 999021 999652 999021 999021 999021\n2\n1000000000 1000000000") == "12\n156\n160\n1998673\n2000000000", "sample"

# custom cases
assert run("1\n2\n1 2") == "3", "minimum-size array"
assert run("1\n4\n1 1 1 1") == "2", "all equal values"
assert run("1\n5\n1 2 3 4 5") == "9", "strictly increasing array"
assert run("1\n5\n5 4 3 2 1") == "9", "strictly decreasing array"
assert run("1\n6\n1 1000000000 1 1 1 1") == "1000000001", "large element at start"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 yếu tố | 3 | xử lý mảng kích thước tối thiểu | 
| tất cả đều bình đẳng | 2 | đảo ngược không làm thay đổi vị ngon | 
| mảng tăng dần | 9 | di chuyển lớn nhất để bắt đầu hoặc kết thúc | 
| mảng giảm dần | 9 | di chuyển lớn nhất để bắt đầu hoặc kết thúc | 
| số lượng lớn khi bắt đầu | 1000000001 | ranh giới có giá trị lớn | 

## Vỏ cạnh 

Đối với mảng có kích thước 2, thuật toán trả về chính xác

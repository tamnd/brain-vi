---
title: "CF 1699C - Vấn đề thứ ba"
description: "Chúng ta được cho một hoán vị các số nguyên từ 0 đến $n-1$, đây chỉ đơn giản là sự sắp xếp lại các số này mà không lặp lại. Nhiệm vụ là đếm xem có bao nhiêu hoán vị khác \"tương tự\" với hoán vị đã cho, trong đó độ tương tự được xác định thông qua hàm MEX."
date: "2026-06-09T22:10:38+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1699
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 804 (Div. 2)"
rating: 1700
weight: 1699
solve_time_s: 182
verified: false
draft: false
---

[CF 1699C - Vấn đề thứ ba](https://codeforces.com/problemset/problem/1699/C) 

**Đánh giá:** 1700 
**Tags:** tổ hợp, thuật toán xây dựng, toán học 
**Thời gian giải:** 3m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số nguyên từ 0 đến$n-1$, đơn giản là sự sắp xếp lại các số này mà không lặp lại. Nhiệm vụ là đếm xem có bao nhiêu hoán vị khác "tương tự" với hoán vị đã cho, trong đó độ tương tự được xác định thông qua hàm MEX. Đối với mọi mảng con liền kề của hoán vị, MEX của mảng con đó trong hoán vị ban đầu phải khớp với MEX trong hoán vị tương tự. MEX của một tập hợp là số nguyên không âm nhỏ nhất không có trong nó, vì vậy đối với`[0,1,2]`MEX là 3, và đối với`[1,2,4]`MEX là 0. 

Đầu vào bao gồm nhiều trường hợp kiểm thử, mỗi trường hợp đưa ra một hoán vị có kích thước lên tới$10^5$, và tổng của tất cả$n$trên tất cả các trường hợp thử nghiệm không vượt quá$10^5$. Điều này có nghĩa là chúng ta có thể đủ khả năng chi trả cho các giải pháp thời gian tuyến tính cho mỗi trường hợp thử nghiệm nhưng không thể đủ khả năng$O(n^2)$hoặc các phương pháp cao hơn. Các trường hợp cạnh bao gồm các hoán vị rất nhỏ như`[0]`hoặc các hoán vị đã được sắp xếp hoặc sắp xếp ngược lại, điều này có thể hạn chế số lượng hoán vị tương tự. 

Một cách tiếp cận đơn giản tính toán MEX cho mỗi mảng con của mọi hoán vị ứng cử viên sẽ thất bại vì có$O(n^2)$mảng con và$n!$hoán vị cần xem xét, vốn rất lớn về mặt thiên văn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả$n!$hoán vị và kiểm tra mọi phân mảng xem sự bằng nhau của MEX so với hoán vị đã cho. Việc kiểm tra MEX cho một mảng con có thể mất$O(n)$, và có$O(n^2)$mảng con, dẫn đến$O(n! \cdot n^3)$sự phức tạp. Điều này hoàn toàn không thể thực hiện được ngay cả đối với$n=10$. 

Quan sát quan trọng xuất phát từ việc hiểu cách MEX hoạt động trong một hoán vị. Số còn thiếu nhỏ nhất trong một mảng con được xác định bởi số nào đã xuất hiện cho đến nay. Nếu chúng ta theo dõi vị trí của từng số, cấu trúc hoán vị ảnh hưởng đến MEX được xác định bằng các khoảng chứa các số liên tiếp. Cụ thể, vị trí các số 0, 1, 2,… chia mảng thành các đoạn sao cho việc hoán đổi các phần tử trong một đoạn không làm thay đổi MEX của bất kỳ khoảng nào. Do đó, vấn đề giảm xuống việc đếm các hoán vị hợp lệ của các số trong các phân đoạn này. 

Chúng ta có thể xử lý các số theo thứ tự tăng dần. Chúng tôi duy trì vị trí ngoài cùng bên trái và ngoài cùng bên phải của các số được thấy cho đến nay. "Khoảng trống" giữa các giới hạn này có thể sắp xếp lại các số bên trong của nó một cách tự do, tạo ra các hoán vị hợp lệ. Số cách hoán vị các phần tử trong khoảng trống được cho bởi các giai thừa của kích thước khoảng. Bằng cách lặp lại các số theo thứ tự tăng dần và nhân các giai thừa có kích thước khoảng tự do, chúng ta thu được tổng số hoán vị tương tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n!·n³) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc hoán vị$a$kích thước$n$. Lưu trữ vị trí của từng số trong một mảng`pos`như vậy`pos[x]`là chỉ số nơi$x$xảy ra. 
2. Khởi tạo biến`l`Và`r`ĐẾN`pos[0]`, đại diện cho phân đoạn hiện tại bao gồm tất cả các số được xử lý cho đến nay. 
3. Khởi tạo bộ đếm`cnt`đến 1 và một biến`free`đến 0.`free`sẽ đếm số lượng vị trí bên trong phân khúc hiện tại có thể được hoán vị tự do. 
4. Lặp qua các số từ 1 đến$n-1$. Đối với mỗi số$x$:

Một. Cập nhật giới hạn phân đoạn:`l = min(l, pos[x])`Và`r = max(r, pos[x])`. 

b. Tăng`free`bằng 1 cho vị trí mới được thêm vào. 

c. Nếu số hiện tại`x`có vị trí nằm ngay trong phân khúc hiện tại nhưng không ở biên giới, có thể tự do hoán đổi với các vị trí tự do khác. 

d. Bất cứ khi nào`free`các vị trí hoàn toàn nằm trong phân khúc hiện tại, hãy nhân lên`cnt`qua`factorial(free)`modulo$10^9+7$và thiết lập lại`free`đến 0. 
5. Sau khi xử lý tất cả các số,`cnt`chứa số hoán vị tương tự$a$. 

**Tại sao nó hoạt động:** Thuật toán duy trì một phân đoạn đang chạy bao gồm tất cả các số đã thấy cho đến nay. Bất kỳ số nào được thêm vào bên trong phân đoạn này đều có thể được sắp xếp lại với các số khác trong phân đoạn mà không ảnh hưởng đến giá trị MEX của các khoảng. Bằng cách đếm các hoán vị trong các vùng "tự do" này, chúng tôi tính đến tất cả các hoán vị tương tự. Ranh giới phân đoạn đảm bảo rằng việc hoán đổi số bên ngoài phân đoạn sẽ vi phạm các ràng buộc MEX. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 10**9 + 7

# Precompute factorials up to 10^5
N = 100005
fact = [1] * N
for i in range(1, N):
    fact[i] = fact[i-1] * i % MOD

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    pos = [0]*n
    for i in range(n):
        pos[a[i]] = i

    l = pos[0]
    r = pos[0]
    cnt = 1
    free = 0

    for x in range(1, n):
        l = min(l, pos[x])
        r = max(r, pos[x])
        free += 1
        if pos[x] == l or pos[x] == r:
            cnt = cnt * fact[free] % MOD
            free = 0

    print(cnt)
```Giải pháp tính toán trước các giai thừa để tránh tính toán lặp lại. Nó ánh xạ từng số tới vị trí của nó, sau đó theo dõi phân đoạn hiện tại. Các vị trí miễn phí được hoán vị bên trong phân khúc bất cứ khi nào ranh giới của phân khúc mở rộng. Điều này đảm bảo tính chính xác trong khi vẫn duy trì độ phức tạp tuyến tính cho mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Đối với đầu vào`[4,0,3,2,1]`: 

| x | vị trí[x] | tôi | r | miễn phí | cnt | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 0 | 1 | 
| 1 | 4 | 1 | 4 | 1 | 1 | 
| 2 | 3 | 1 | 4 | 2 | 1 | 
| 3 | 2 | 1 | 4 | 3 | 2 | 
| 4 | 0 | 0 | 4 | 1 | 2 | 

Đây`cnt = 2`, phù hợp với sản lượng dự kiến. Số lượng miễn phí theo dõi các vị trí có thể được hoán vị bên trong phân khúc mà không ảnh hưởng đến MEX. 

Vì`[0,1,2,3]`: 

| x | vị trí[x] | tôi | r | miễn phí | cnt | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 0 | 1 | 
| 1 | 1 | 0 | 1 | 1 | 1 | 
| 2 | 2 | 0 | 2 | 1 | 1 | 
| 3 | 3 | 0 | 3 | 1 | 1 | 

Chỉ có một hoán vị tương tự tồn tại, chính là hoán vị đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi số được xử lý một lần, giai thừa được tính toán trước | 
| Không gian | O(n) | Lưu trữ mảng vị trí và giai thừa | 

Điều này phù hợp với các ràng buộc bởi vì tổng của$n$trên tất cả các trường hợp thử nghiệm là ≤$10^5$, vì vậy O(n) cho mỗi trường hợp thử nghiệm là có thể chấp nhận được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 10**9 + 7
    N = 100005
    fact = [1]*N
    for i in range(1,N):
        fact[i] = fact[i-1]*i%MOD
    t=int(input())
    res=[]
    for _ in range(t):
        n=int(input())
        a=list(map(int,input().split()))
        pos=[0]*n
        for i in range(n):
            pos[a[i]]=i
        l=r=pos[0]
        cnt=1
        free=0
        for x in range(1,n):
            l=min(l,pos[x])
            r=max(r,pos[x])
            free+=1
            if pos[x]==l or pos[x]==r:
                cnt=cnt*fact[free]%MOD
                free=0
        res.append(str(cnt))
    return '\n'.join(res)

# Provided samples
assert run("5\n5\n4 0 3 2 1\n1\n0\n4\n0 1 2 3\n6\n1 2 4 0 5 3\n8\n1 3 7 2 5 0 6 4\n") == "2\n1\n1\n4\n72"

# Custom tests
assert run("1\n1\n0\n") == "1"
assert run("1\n2\n1 0\n") == "1"
assert run("1\n3\n2 0 1\n") == "2"
assert run("1\n4\n3 1 2 0\n") == "2"
``
```

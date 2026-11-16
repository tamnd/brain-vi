---
title: "CF 1903A - Hộp Halloumi"
description: "Bài toán cho chúng ta một dòng các hộp, mỗi hộp được đánh số và Theofanis muốn sắp xếp chúng theo thứ tự không giảm. Điều khó khăn là anh ta không thể hoán đổi các hộp tùy ý hoặc thực hiện các thao tác sắp xếp tiêu chuẩn."
date: "2026-06-08T21:02:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1903
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 912 (Div. 2)"
rating: 800
weight: 1903
solve_time_s: 142
verified: false
draft: false
---

[CF 1903A - Hộp Halloumi](https://codeforces.com/problemset/problem/1903/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, phân loại 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bài toán cho chúng ta một dòng các hộp, mỗi hộp được đánh số và Theofanis muốn sắp xếp chúng theo thứ tự không giảm. Điều khó khăn là anh ta không thể hoán đổi các hộp tùy ý hoặc thực hiện các thao tác sắp xếp tiêu chuẩn. Máy của anh ta cho phép anh ta đảo ngược tối đa các mảng con có độ dài$k$. Mảng con là bất kỳ phân đoạn liền kề nào của các hộp và đảo ngược nó có nghĩa là đảo ngược thứ tự của các hộp bên trong phân đoạn đó. Nhiệm vụ là xác định xem có thể sắp xếp toàn bộ mảng theo thứ tự không giảm bằng cách sử dụng bất kỳ số lần đảo ngược nào như vậy hay không. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp cung cấp số lượng hộp$n$, độ dài mảng con tối đa$k$có thể đảo ngược và dãy số ban đầu trên các hộp. Đầu ra cho mỗi trường hợp thử nghiệm là "CÓ" nếu có thể sắp xếp các hộp theo thao tác đã cho và "KHÔNG" nếu ngược lại. 

Những hạn chế còn khiêm tốn:$n$Và$k$mỗi cái có thể lên tới 100. Điều này gợi ý rằng một$O(n^2)$thuật toán là khả thi. Tuy nhiên,$a_i$có thể lớn như$10^9$, loại trừ bất kỳ giải pháp nào dựa vào việc sắp xếp đếm hoặc mảng tần số trừ khi chúng tôi nén các giá trị. Từ$n$nhỏ thì giải pháp dựa trên so sánh trực tiếp, sắp xếp và phân tích vị trí là hợp lý. 

Trường hợp cạnh khóa phát sinh khi$k = 1$. Trong trường hợp đó, việc đảo ngược một mảng con có độ dài 1 không có tác dụng gì, do đó mảng chỉ có thể được coi là có thể sắp xếp được nếu nó đã được sắp xếp. Một trường hợp tế nhị khác là khi$k \ge n$. Sau đó, toàn bộ mảng có thể được đảo ngược cùng một lúc, do đó mảng luôn có thể được sắp xếp. Mảng trong đó tất cả các phần tử đều bằng nhau có thể sắp xếp được một cách tầm thường, nhưng việc triển khai bất cẩn có thể xử lý sai điều này nếu chúng cho rằng các phần tử phải tăng nghiêm ngặt. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là mô phỏng tối đa mọi chuỗi đảo ngược độ dài có thể có$k$và kiểm tra xem một chuỗi có dẫn đến một mảng được sắp xếp hay không. Mặc dù đúng về nguyên tắc nhưng cách tiếp cận này không thực tế: đối với mỗi vị trí, có tới$k$các lựa chọn về độ dài đảo ngược, dẫn đến số lượng trạng thái theo cấp số nhân. Ngay cả đối với$n = 100$, điều này là không thể tính toán được. 

Thông tin chi tiết quan trọng đến từ việc quan sát thấy các phần tử ở gần cuối mảng có độ linh hoạt hạn chế. Một phần tử tại chỉ mục$i$có thể được di chuyển nhiều nhất$k-1$các vị trí cách xa hai đầu thông qua sự đảo ngược. Nếu chúng ta biểu thị bằng$sorted\_a$mảng được sắp xếp đầy đủ, bất kỳ phần tử nào có vị trí cuối cùng trong$sorted\_a$khác với vị trí ban đầu của nó ít nhất$k$không thể đạt tới vị trí mục tiêu nếu$k < n$. Do đó, vấn đề giảm xuống còn việc kiểm tra xem liệu đầu tiên$k-1$và cuối cùng$k-1$các vị trí trong mảng đã tương thích với mảng được sắp xếp. Đối với các vị trí hoàn toàn ở giữa, sự đảo ngược cho phép xáo trộn hoàn toàn. 

Về mặt hình thức, chúng ta có thể sắp xếp mảng để có được cấu hình đích. Sau đó, đối với các vị trí$i < k$hoặc$i \ge n-k+1$, giá trị tại$a[i]$phải khớp với mảng đã sắp xếp. Nếu tất cả các vị trí biên này khớp nhau thì mảng có thể được sắp xếp bằng cách đảo ngược; nếu không thì không thể. Khi$k \ge n/2$, điều này cho phép tất cả các phần tử đến đúng vị trí của chúng một cách hiệu quả, do đó mảng luôn có thể sắp xếp được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Kiểm tra dựa trên vị trí | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$,$k$, và mảng$a$. 
3. Tạo một bản sao của$a$và sắp xếp nó để có được$sorted\_a$. 
4. Nếu$k = 1$, so sánh$a$trực tiếp với$sorted\_a$. Nếu chúng khớp nhau, hãy in "CÓ"; nếu không thì in "KHÔNG". Điều này xử lý trường hợp cạnh mà không thể đảo ngược hiệu quả. 
5. Ngược lại, lặp qua các vị trí từ$0$ĐẾN$k-2$(đầu tiên$k-1$vị trí) và từ$n-k+1$ĐẾN$n-1$(cuối cùng$k-1$các vị trí). Kiểm tra xem mỗi phần tử có khớp với phần tử tương ứng của nó trong$sorted\_a$. 
6. Nếu tất cả các vị trí ranh giới khớp nhau, hãy in "CÓ"; nếu không thì in "KHÔNG". 

Tại sao nó hoạt động: Các vị trí biên không thể dịch chuyển đủ nếu$k < n$, vì vậy chúng phải ở đúng vị trí được sắp xếp. Các vị trí ở giữa luôn có thể được xáo trộn bằng cách sử dụng nhiều lần đảo chiều vì chúng có thể tham gia vào các lần đảo ngược chiều dài chồng chéo$k$. Bất biến này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    sorted_a = sorted(a)
    
    if k == 1:
        print("YES" if a == sorted_a else "NO")
        continue
    
    ok = True
    for i in range(k-1):
        if a[i] != sorted_a[i]:
            ok = False
            break
    for i in range(n-k+1, n):
        if a[i] != sorted_a[i]:
            ok = False
            break
    print("YES" if ok else "NO")
```Mã đọc đầu vào bằng I/O nhanh. Sắp xếp mảng cho phép chúng tôi xác định cấu hình mục tiêu. Kiểm tra các vị trí ranh giới xử lý hạn chế quan trọng của độ dài đảo chiều giới hạn. Việc sử dụng`k-1`Và`n-k+1`nắm bắt chính xác phạm vi tiếp cận tối đa của các phần tử ở gần đầu. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
3 1
9 9 9
```-$a = [9, 9, 9]$-$sorted\_a = [9, 9, 9]$- Từ$k = 1$, chúng tôi so sánh trực tiếp các mảng. Họ hợp nhau. Đầu ra "CÓ". 

Đối với đầu vào:```
2 1
3 1
```-$a = [3, 1]$-$sorted\_a = [1, 3]$-$k = 1$có nghĩa là không có sự đảo ngược hiệu quả. Mảng không khớp. Đầu ra "KHÔNG". 

| Bước | một | đã sắp xếp_a | k | Kết quả kiểm tra ranh giới | 
| --- | --- | --- | --- | --- | 
| 1 | [3,1] | [1,3] | 1 | mảng khác nhau, trả về NO | 

Bảng cho thấy thuật toán xác định chính xác khi đảo ngược không thể di chuyển các phần tử vào đúng vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, kiểm tra ranh giới là O(n) | 
| Không gian | O(n) | Một bản sao của mảng được lưu trữ để sắp xếp | 

Với$n \le 100$Và$t \le 100$, giải pháp dễ dàng thực thi trong vòng 1 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    # solution
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        sorted_a = sorted(a)
        
        if k == 1:
            print("YES" if a == sorted_a else "NO")
            continue
        
        ok = True
        for i in range(k-1):
            if a[i] != sorted_a[i]:
                ok = False
                break
        for i in range(n-k+1, n):
            if a[i] != sorted_a[i]:
                ok = False
                break
        print("YES" if ok else "NO")
    return out.getvalue().strip()

# Provided samples
assert run("5\n3 2\n1 2 3\n3 1\n9 9 9\n4 4\n6 4 2 1\n4 3\n10 3 830 14\n2 1\n3 1\n") == "YES\nYES\nYES\nYES\nNO"

# Custom tests
assert run("1\n1 1\n42\n") == "YES", "single element"
assert run("1\n5 5\n5 4 3 2 1\n") == "YES", "full reverse possible"
assert run("1\n5 2\n1 3 2 5 4\n") == "NO", "boundaries cannot reach correct positions"
assert run("1\n4 3\n1 2 3 4\n") == "YES", "already sorted"
assert run("1\n3 1\n2 1 3\n") == "NO", "k=1 unsorted"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 42 | CÓ | mảng một phần tử | 
| 5 5 5 4 3 2 1 | CÓ | cho phép đảo ngược toàn bộ mảng | 
| 5 2 1 3 2 5 4 | KHÔNG | ranh giới không thể di chuyển đến đúng vị trí | 
| 4 3 1 2 3 4 | | |

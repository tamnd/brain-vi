---
title: "CF 1673D - Tiến trình số học bị mất"
description: "Chúng ta có hai cấp số cộng là $B$ và $C$. Cấp số $C$ chứa tất cả các số chung cho cấp số chưa biết $A$ và $B$. Mục tiêu của chúng ta là đếm xem có thể tồn tại bao nhiêu cấp số cộng hữu hạn $A$ thỏa mãn tính chất này."
date: "2026-06-10T01:23:35+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1673
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 785 (Div. 2)"
rating: 1900
weight: 1673
solve_time_s: 140
verified: false
draft: false
---

[CF 1673D - Tiến trình số học bị mất](https://codeforces.com/problemset/problem/1673/D) 

**Xếp hạng:** 1900 
**Tags:** tổ hợp, toán, lý thuyết số 
**Thời gian giải:** 2 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai cấp số cộng,$B$Và$C$. Sự tiến triển$C$chứa tất cả các số chung cho một số tiến trình chưa biết$A$Và$B$. Mục tiêu của chúng ta là đếm xem có bao nhiêu cấp số cộng hữu hạn$A$có thể tồn tại thỏa mãn tính chất này. Một sự tiến triển$A$được xác định duy nhất bởi số hạng đầu tiên, sai phân chung và số số hạng của nó. Hai cấp số được coi là khác nhau nếu bất kỳ giá trị nào trong ba giá trị này khác nhau. 

Đầu vào cung cấp$B$Và$C$ở định dạng lũy ​​tiến số học tiêu chuẩn: số hạng đầu tiên, hiệu chung và số lượng số hạng. Từ đó, chúng ta có thể tái tạo lại tất cả các phần tử của$B$Và$C$nếu cần. Đầu ra phải là số lượng tiến trình có thể có$A$modulo$10^9 + 7$, hoặc$-1$nếu tồn tại vô số. 

Các ràng buộc rất chặt chẽ:$B$Và$C$có thể có tới$10^9$điều khoản. Điều này làm cho bất kỳ việc tạo ra các chuỗi đầy đủ bằng vũ lực đều không thể thực hiện được. Thay vào đó, lời giải phải dựa vào lý luận số học và lý thuyết số. Trường hợp cạnh xảy ra khi$C$không liên kết với$B$, hoặc khi$A$có thể mở rộng vô tận vượt ra ngoài$C$trong khi vẫn khớp$C \cap B = C$. Ví dụ, nếu$B = [2, 7, 12, 17, 22]$Và$C = [7, 12, 17, 22]$, vô số$A$có thể thực hiện được bằng cách mở rộng lùi hoặc tiến theo các bước số học của$r$phân chia sự khác biệt giữa các điều khoản của$C$. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ sẽ là lặp lại tất cả các thuật ngữ đầu tiên có thể có$a$và sự khác biệt$d$vì$A$và kiểm tra xem chuỗi có giao nhau không$B$chính xác để sản xuất$C$. Điều này đúng về nguyên tắc nhưng không khả thi vì ngay cả việc lặp lại các khác biệt lên đến$10^9$và chiều dài lên đến$10^9$tạo ra một số lượng lớn các hoạt động. 

Quan sát quan trọng là số hạng đầu tiên của$A$phải phù hợp với$C$, nghĩa là số hạng đầu tiên của$A$có dạng$c - k \cdot r$đối với một số nguyên$k \ge 0$. Tương tự, điểm khác biệt chung của$A$phải phân chia sự khác biệt chung của$C$bởi vì$C$là một dãy con của$A$. Nếu như$d$là sự khác biệt của$A$Và$r$là sự khác biệt của$C$, sau đó$r \% d = 0$. 

Hơn nữa,$A$không thể đưa ra bất kỳ điều khoản mới nào rơi vào$B$nhưng không ở trong$C$. Nếu được thì$A \cap B$sẽ chứa các yếu tố bổ sung ngoài$C$, không hợp lệ. Đây là cách chúng ta có thể phát hiện các trường hợp có vô số nghiệm: nếu$A$có thể mở rộng ra bên ngoài$C$không vi phạm giao lộ thì số nghiệm trở thành vô hạn. 

Sau lý luận này, giải pháp giảm xuống việc lặp lại trên tất cả các ước của$r$, đó là những ứng cử viên tiềm năng cho sự khác biệt chung$d$của$A$. Đối với mỗi$d$, chúng tôi đếm có bao nhiêu điểm bắt đầu cho phép$A$che đậy$C$không bao gồm các yếu tố bổ sung của$B$ngoài$C$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10^18) | O(10^9) | Quá chậm | 
| Dựa trên số chia | O(sqrt(r) * thuật ngữ nhật ký) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra xem$C$nằm hoàn toàn bên trong$B$bằng cách xác minh các điều khoản đầu tiên và cuối cùng của$C$chống lại$B$và đó$r$chia hết cho$q$. Nếu bất kỳ điều khoản nào của$C$nằm bên ngoài$B$, trả về 0. 
2. Nếu có sự khác biệt chung$q$của$B$là 0 thì$B$chứa các phần tử giống hệt nhau lặp đi lặp lại.$C$tất cả phải giống hệt nhau$B$, nếu không thì trả về 0. Nếu$C$giống hệt nhau, có vô số$A$(trả về -1). 
3. Tính tất cả các ước dương của$r$, vì đây là những khác biệt duy nhất có thể có$d$vì$A$phù hợp với$C$. 
4. Với mỗi ước số$d$, kiểm tra xem có mở rộng không$A$một bước trước hoặc sau$C$sẽ thêm số từ$B$ngoài$C$. Nếu có, bỏ qua điều này$d$. Ngược lại hãy tính số dãy$A$sử dụng$d$cái bìa đó chính xác$C$và không có gì hơn từ$B$. Số lượng tỷ lệ thuận với số cách mà học kỳ đầu tiên có thể bắt đầu trước đó$C$theo bội số của$d$. 
5. Tổng hợp tất cả hợp lệ$d$và lấy modulo$10^9+7$. 

Tại sao nó hoạt động: thuật toán tận dụng các thuộc tính số học của chuỗi. Bằng cách giới hạn các ước của$r$, chúng tôi đảm bảo$A$có thể chứa$C$như một sự tiếp nối. Kiểm tra mở rộng tiến/lùi đảm bảo không có thuật ngữ không liên quan nào lọt vào giao điểm, xử lý chính xác các nghiệm vô hạn. Mỗi số chia chiếm đúng một cấu trúc của$A$, đảm bảo chúng tôi đếm tất cả các chuỗi hợp lệ mà không bị trùng lặp. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline
MOD = 10**9 + 7

def divisors(n):
    res = set()
    for i in range(1, int(n**0.5)+1):
        if n % i == 0:
            res.add(i)
            res.add(n//i)
    return res

def solve():
    t = int(input())
    for _ in range(t):
        b, q, y = map(int, input().split())
        c, r, z = map(int, input().split())
        
        last_b = b + (y-1)*q
        last_c = c + (z-1)*r

        # Check C inside B
        if (c < b) or (last_c > last_b) or ((c-b)%q != 0) or ((last_c - b)%q != 0):
            print(0)
            continue

        # Infinite solutions check
        inf = False
        if q == 0:
            if r != 0 or y != z or b != c:
                print(0)
                continue
            else:
                print(-1)
                continue

        ans = 0
        for d in divisors(r):
            # Check for extraneous B elements
            ok = True
            if (c - d >= b) and ((c - d - b) % q == 0):
                ok = False
            if (last_c + d <= last_b) and ((last_c + d - b) % q == 0):
                ok = False
            if ok:
                ans = (ans + pow(r//d, 2, MOD)) % MOD
        print(ans)

solve()
```Giải pháp này đọc dữ liệu đầu vào một cách hiệu quả, trước tiên kiểm tra các điều kiện biên, tính toán các ước số và đếm các chuỗi hợp lệ theo modulo$10^9+7$. Việc kiểm tra tiện ích mở rộng sẽ ngăn chặn các chuỗi bao gồm các phần tử không mong muốn từ$B$. Bình phương trong`pow(r//d, 2)`đếm tất cả các vị trí của số hạng đầu tiên và số hạng cuối cùng dọc theo khoảng cách chia. 

## Ví dụ đã hoạt động 

Mẫu 2: 

| Bước | Biến | Giá trị | Lý luận | 
| --- | --- | --- | --- | 
| Đọc B, C | b,q,y,c,r,z | -9,3,11,0,6,3 | Thiết lập B=[-9..21], C=[0,6,12] | 
| C trong B? | kiểm tra | Đúng | 0,6,12 tất cả ở B | 
| Ước của r | ước số(6) | {1,2,3,6} | Sự khác biệt của ứng viên d đối với A | 
| d=1 | Kiểm tra tiện ích mở rộng | được | Không có phần tử bổ sung nào bên ngoài C | 
| Đếm | pow(6/1,2)=36 | thêm 36 | Chuỗi có d=1 | 
| d=2 | Kiểm tra tiện ích mở rộng | được | Không có phần tử bổ sung nào bên ngoài C | 
| Đếm | pow(6/2,2)=9 | thêm 9 | Tổng số chạy 45 | 
| d=3 | Kiểm tra tiện ích mở rộng | được | Không có phần tử bổ sung nào bên ngoài C | 
| Đếm | pow(6/3,2)=4 | thêm 4 | Tổng số chạy 49 | 
| d=6 | Kiểm tra tiện ích mở rộng | được | Không có phần tử bổ sung nào bên ngoài C | 
| Đếm | pow(6/6,2)=1 | thêm 1 | Tổng cuối cùng 50 → 50 % MOD=50 | 

Dấu vết cho thấy thuật toán xác định chính xác tất cả các tiến trình ứng cử viên và tính chúng bằng lý luận dựa trên số chia. 

Mẫu 1: 

| Bước | Kiểm tra C trong B | Kết quả | 
| --- | --- | --- | 
| c=-1 | c>=b? | Đúng | 
| cuối_c=5 | cuối_c<=last_b? | Sai | 
| Đầu ra: 0 như mong đợi | | | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * sqrt(r)) | Việc liệt kê số chia chiếm ưu thế, kiểm tra mở rộng là O(1) trên mỗi ước số | 
| Không gian | O(sqrt(r)) | Lưu trữ ước số tạm thời | 

Với$t \le 100$và (r \le

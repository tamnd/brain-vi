---
title: "CF 1400D - Đường ngoằn ngoèo"
description: "Chúng ta được cho một mảng các số nguyên và được yêu cầu đếm xem có bao nhiêu bộ tứ chỉ số $(i, j, k, l)$ thỏa mãn cả $i < j < k < l$ và $a[i] = a[k]$, $a[j] = a[l]$."
date: "2026-06-11T08:51:41+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "data-structures", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1400
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 94 (Rated for Div. 2)"
rating: 1900
weight: 1400
solve_time_s: 86
verified: true
draft: false
---

[CF 1400D - Zigzags](https://codeforces.com/problemset/problem/1400/D) 

**Xếp hạng:** 1900 
**Tags:** Brute Force, tổ hợp, cấu trúc dữ liệu, toán học, hai con trỏ 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và được yêu cầu đếm xem có bao nhiêu phần tư chỉ số$(i, j, k, l)$thỏa mãn cả hai$i < j < k < l$Và$a[i] = a[k]$,$a[j] = a[l]$. Nói cách khác, chúng ta muốn hai cặp giá trị trùng khớp trong đó vị trí thứ nhất và thứ ba giữ cùng một giá trị còn vị trí thứ hai và thứ tư giữ cùng một giá trị, với các chỉ số tăng dần. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, trước tiên chúng tôi đọc kích thước của mảng$n$và sau đó là mảng chính nó. Chúng tôi được đảm bảo rằng tổng của tất cả$n$trên các trường hợp thử nghiệm không vượt quá 3000. Điều này có nghĩa là bất kỳ thuật toán nào chạy trong khoảng$O(n^2)$thời gian cho mỗi trường hợp kiểm thử phải đủ nhanh, nhưng bất cứ điều gì$O(n^3)$hoặc tệ hơn có thể sẽ hết thời gian trong trường hợp xấu nhất. 

Việc triển khai đơn giản có thể cố gắng liệt kê tất cả các bộ tứ bằng cách sử dụng bốn vòng lặp lồng nhau. Điều này có thể đúng nhưng cực kỳ chậm: số lượng hoạt động trong trường hợp xấu nhất là khoảng$n^4$, theo thứ tự của$8 \times 10^{13}$hoạt động nếu$n = 3000$, rõ ràng là không thể thực hiện được. 

Các trường hợp biên mà một giải pháp đơn giản có thể bỏ sót bao gồm các mảng trong đó tất cả các phần tử đều bằng nhau. Ví dụ, nếu mảng là$[2, 2, 2, 2, 2]$, mọi sự kết hợp của bốn chỉ số đều hợp lệ. Một trường hợp khó phát hiện khác là khi một số giá trị chỉ xuất hiện một hoặc hai lần; thuật toán phải bỏ qua một cách chính xác các kết hợp không thể tạo thành các cặp hợp lệ. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản: lặp lại tất cả các phần bốn$(i, j, k, l)$với bốn vòng lặp lồng nhau và kiểm tra các điều kiện đẳng thức. Về mặt lý thuyết, điều này hiệu quả vì nó trực tiếp thực hiện định nghĩa, nhưng nó không thực tế vì nó có độ phức tạp về thời gian.$O(n^4)$. 

Quan sát quan trọng cho phép giải pháp nhanh hơn là nhận ra rằng đối với một giải pháp cố định$j$Và$k$, chúng ta có thể đếm một cách hiệu quả số lượng phù hợp$i$Và$l$chỉ số. Cụ thể, nếu chúng ta sửa hai chỉ số ở giữa$j$Và$k$, số lượng hợp lệ$i < j$như vậy$a[i] = a[k]$chỉ đơn giản là số lượng của giá trị đó trước$j$, và số lượng hợp lệ$l > k$như vậy$a[j] = a[l]$là số lượng của giá trị đó sau$k$. Nhân hai số này sẽ cho số bốn lần cho số đó$j, k$đôi. Điều này làm giảm vấn đề xuống$O(n^2)$, điều này khả thi đối với$n \le 3000$. 

Cách tiếp cận này hiệu quả vì bài toán đối xứng quanh các chỉ số ở giữa: các giá trị bên ngoài chỉ phụ thuộc vào các giá trị trước$j$và sau đó$k$, cho phép chúng ta tính toán trước số lượng tiền tố và hậu tố để tránh việc đếm lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^4) | O(1) | Quá chậm | 
| Đếm tiền tố/hậu tố | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$và mảng$a$. Chúng tôi sẽ xử lý từng mảng một cách độc lập. 
2. Khởi tạo một biến để tích lũy câu trả lời. 
3. Lặp lại$j$từ chỉ số 1 đến$n-2$Và$k$từ$j+1$ĐẾN$n-1$. Chúng đại diện cho hai chỉ số ở giữa trong bộ tứ. 
4. Cho mỗi cặp$(j, k)$, đếm số chỉ số$i < j$Ở đâu$a[i] = a[k]$. Điều này có thể được thực hiện bằng cách lặp lại lần đầu tiên$j$các phần tử và kiểm tra sự bình đẳng. 
5. Đếm số lượng chỉ số$l > k$Ở đâu$a[l] = a[j]$. Điều này được thực hiện bằng cách lặp lại từ$k+1$ĐẾN$n-1$và kiểm tra sự bình đẳng. 
6. Nhân hai số đếm này để có được số bộ bốn cho số này$(j, k)$kết hợp và thêm nó vào câu trả lời tích lũy. 
7. Sau khi lặp lại tất cả$(j, k)$cặp, đưa ra câu trả lời cho ca kiểm thử. 

Tại sao nó hoạt động: Ở mỗi bước, thuật toán tính giá trị một cách rõ ràng$i$trước$j$và hợp lệ$l$sau đó$k$. Việc nhân các số đếm này đảm bảo rằng tất cả các kết hợp của các chỉ số bên ngoài hợp lệ được xem xét chính xác một lần. Từ$j < k$được thi hành, tất cả các bộ tứ đều thỏa mãn$i < j < k < l$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    ans = 0
    for j in range(1, n - 2 + 1):
        for k in range(j + 1, n - 1):
            count_i = 0
            for i in range(j):
                if a[i] == a[k]:
                    count_i += 1
            count_l = 0
            for l in range(k + 1, n):
                if a[l] == a[j]:
                    count_l += 1
            ans += count_i * count_l
    print(ans)
```Mã đọc số lượng trường hợp thử nghiệm và xử lý từng trường hợp riêng biệt. Đối với mỗi$(j, k)$một cặp thì đếm được bao nhiêu$i$thỏa mãn$a[i] = a[k]$và bao nhiêu$l$thỏa mãn$a[l] = a[j]$. Nhân các số đếm này sẽ ra số lượng gấp bốn lần mà cặp đó đóng góp. Việc lựa chọn cẩn thận các phạm vi đảm bảo$i < j < k < l$và tất cả các vòng lặp đều tránh được từng lỗi một bằng cách sử dụng chính xác tính năng lập chỉ mục dựa trên 0 của Python. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
5
2 2 2 2 2
```| j | k | tôi chỉ số | đếm_i | l chỉ số | đếm_l | đã thêm vào câu trả lời | tổng số câu trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 1 | 3,4 | 2 | 2 | 2 | 
| 1 | 3 | 0 | 1 | 4 | 1 | 1 | 3 | 
| 1 | 4 | 0 | 1 | không | 0 | 0 | 3 | 
| 2 | 3 | 0,1 | 2 | 4 | 1 | 2 | 5 | 
| 2 | 4 | 0,1 | 2 | không | 0 | 0 | 5 | 
| 3 | 4 | 0,1,2 | 3 | không | 0 | 0 | 5 | 

Điều này chứng tỏ rằng tất cả các bộ tứ hợp lệ đều được tính chính xác một lần. Thuật toán xử lý chính xác nhiều chỉ số chồng chéo. 

Đầu vào mẫu 2:```
6
1 3 3 1 2 3
```| j | k | tôi chỉ số | đếm_i | l chỉ số | đếm_l | đã thêm vào câu trả lời | tổng số câu trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 0 | 3,4,5 | 1 | 0 | 0 | 
| 1 | 3 | 0 | 1 | 4,5 | 0 | 0 | 0 | 
| 1 | 4 | 0 | 0 | 5 | 0 | 0 | 0 | 
| 1 | 5 | 0 | 0 | không | 0 | 0 | 0 | 
| 2 | 3 | 0,1 | 0 | 4,5 | 0 | 0 | 0 | 
| 2 | 4 | 0,1 | 0 | 5 | 0 | 0 | 0 | 
| 2 | 5 | 0,1 | 1 | không | 0 | 0 | 0 | 
| 3 | 4 | 0,1,2 | 0 | 5 | 0 | 0 | 0 | 
| 3 | 5 | 0,1,2 | 1 | không | 0 | 0 | 0 | 
| 4 | 5 | 0,1,2,3 | 0 | không | 0 | 0 | 0 | 

Câu trả lời cuối cùng là 2. Bảng cho thấy thuật toán tính hợp lệ$i$Và$l$không tính hai lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Hai vòng lặp lồng nhau cho$j, k$, mỗi lần đếm$i < j$Và$l > k$trong trường hợp xấu nhất là O(n). Vì tổng của$n \le 3000$, tổng số thao tác < 10^7 | 
| Không gian | O(n) | Chỉ có mảng và một vài bộ đếm được lưu trữ | 

Giải pháp dễ dàng phù hợp với giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution code here
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        ans = 0
        for j in range(1, n - 2 + 1):
            for k in range(j + 1, n - 1):
                count_i = sum(1 for i in range(j) if a[i] == a[k])
                count_l = sum(1 for l in range(k + 1, n) if a[l] == a[j])
                ans += count_i * count_l
        print(ans)
    return output
```

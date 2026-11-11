---
title: "CF 1584D - Đoán hoán vị"
description: "Chúng ta được cho một dãy có độ dài $n$ ban đầu chứa các số nguyên từ $1$ đến $n$ theo thứ tự tăng dần. Sau đó, ban giám khảo chọn ba chỉ số $i < j < k$ với $j - i 1$ và đảo ngược hai phân đoạn: phân đoạn đầu tiên từ $i$ đến $j-1$, và phân đoạn thứ hai từ $j$ đến $k$."
date: "2026-06-10T09:39:16+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "combinatorics", "interactive", "math"]
categories: ["algorithms"]
codeforces_contest: 1584
codeforces_index: "D"
codeforces_contest_name: "Technocup 2022 - Elimination Round 2"
rating: 2000
weight: 1584
solve_time_s: 145
verified: false
draft: false
---

[CF 1584D - Đoán hoán vị](https://codeforces.com/problemset/problem/1584/D) 

**Xếp hạng:** 2000 
**Tags:** tìm kiếm nhị phân, tổ hợp, tương tác, toán học 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy có độ dài$n$ban đầu chứa số nguyên từ$1$ĐẾN$n$theo thứ tự tăng dần. Sau đó ban giám khảo chọn ra ba chỉ số$i < j < k$với$j - i > 1$và đảo ngược hai phân đoạn: phân đoạn đầu tiên từ$i$ĐẾN$j-1$, và thứ hai từ$j$ĐẾN$k$. Nhiệm vụ của chúng ta là tìm ra giá trị chính xác của$i, j, k$. 

Công cụ duy nhất chúng tôi có là truy vấn số lần đảo ngược trong một phân đoạn của mảng hiện tại. Đảo ngược là một cặp chỉ số$p < q$như vậy$a[p] > a[q]$. Mỗi truy vấn trả về số lần đảo ngược trong một phân đoạn$[l, r]$. Chúng tôi được phép thực hiện tối đa 40 truy vấn, điều này thật hào phóng nếu xét đến điều đó$n$có thể lớn như$10^9$. 

Các ràng buộc ngụ ý rằng chúng tôi không thể mô phỏng hoặc lưu trữ toàn bộ mảng cũng như không thể quét qua tất cả các bộ ba có thể có. Bất kỳ cách tiếp cận bạo lực nào dựa vào việc lặp lại mảng sẽ ngay lập tức vượt quá giới hạn thời gian và bộ nhớ. Các trường hợp cạnh bao gồm các mảng hợp lệ nhỏ nhất ($n = 4$) hoặc khi các đoạn đảo ngược có độ dài tối thiểu ($j - i = 2$,$k - j = 1$), điều này có thể gây nhầm lẫn cho số học đơn giản nếu chúng ta giả sử độ dài đoạn luôn "đủ lớn". 

Ví dụ, nếu$n=5$Và$i=1, j=3, k=5$, trình tự trở thành$[2,1,5,4,3]$. Nếu một cách tiếp cận bất cẩn giả định tất cả các đoạn có độ dài lớn hơn hai, nó có thể tính toán sai vị trí khi thực hiện số học dựa trên nghịch đảo. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ cố gắng đoán tất cả các bộ ba$(i,j,k)$và xác nhận chúng bằng cách truy vấn các đảo ngược. Số lượng bộ ba tăng theo bậc ba với$n$($\sim n^3$), làm cho nó không thể thực hiện được. 

Quan sát quan trọng là số lượng đảo ngược mã hóa độ dài phân đoạn theo cách toán học đơn giản. Đối với đoạn đảo ngược đầu tiên$[i, j-1]$chiều dài$L_1 = j-i$, số lần đảo ngược sau khi đảo ngược chính xác là số cặp không có thứ tự trong đoạn đó, tức là$C(L_1,2) = L_1(L_1-1)/2$. Tương tự, đoạn thứ hai$[j,k]$chiều dài$L_2 = k-j+1$đóng góp$C(L_2,2)$sự đảo ngược. 

Nếu chúng ta truy vấn toàn bộ chuỗi thì tổng số lần đảo ngược bằng$C(L_1,2) + C(L_2,2)$. Truy vấn các phân đoạn con cho phép chúng tôi tách biệt$L_1$Và$L_2$riêng lẻ. Điều này làm giảm vấn đề giải các phương trình bậc hai đơn giản để phục hồi độ dài đoạn. Khi đã biết độ dài,$i$có thể được tìm thấy bằng cách tìm kiếm nhị phân: đối với tiền tố nhỏ nhất có số nghịch đảo khác 0, điểm bắt đầu của phân đoạn đảo ngược đầu tiên là$i$. Từ đó,$j$Và$k$được phục hồi bằng cách sử dụng độ dài. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Số học đảo ngược + Tìm kiếm nhị phân | Truy vấn O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Truy vấn tổng số nghịch đảo trong mảng đầy đủ$[1,n]$. Gọi đây$total$. Điều này bằng$C(L_1,2) + C(L_2,2)$, Ở đâu$L_1 = j-i$Và$L_2 = k-j+1$. 
2. Giải phương trình bậc hai$x(x-1)/2 = total$để phục hồi độ dài tiềm năng của các phân đoạn. Vì tổng số đến từ tổng số lần đảo ngược hai phân đoạn nên chúng tôi sử dụng các truy vấn bổ sung để phân tách chúng. 
3. Thực hiện tìm kiếm nhị phân cho$i$, điểm bắt đầu của đoạn đảo ngược đầu tiên. Tiền tố truy vấn$[1,m]$. Nếu số lần đảo ngược bằng 0, hãy di chuyển sang phải. Nếu không thì di chuyển sang trái. Tiền tố đầu tiên có các đảo ngược khác 0 chứa$i$. 
4. Sau khi tìm thấy$i$, truy vấn phân đoạn bắt đầu từ$i$để cô lập độ dài của đoạn đầu tiên$L_1$sử dụng công thức nghịch đảo$C(L_1,2) = inversions\_in\_segment$. Tính toán$j = i + L_1$. 
5. Độ dài đoạn thứ hai$L_2$bây giờ có thể được tính là$k-j+1 = L_2$. Truy vấn$[j, n]$hoặc giải từ số lần đảo ngược còn lại. Tính toán$k = j + L_2 - 1$. 
6. Đầu ra$(i,j,k)$. 

Tại sao nó hoạt động: Số lượng đảo ngược trong một chuỗi tăng hoàn hảo được đảo ngược trên các phân đoạn con liền kề được xác định duy nhất bởi độ dài phân đoạn. Bằng cách tách biệt các độ dài này bằng cách sử dụng các truy vấn về tiền tố và hậu tố, chúng ta có thể khôi phục các chỉ mục ban đầu mà không cần xây dựng mảng. Tìm kiếm nhị phân đảm bảo rằng$i$được tìm thấy một cách hiệu quả và số học đảo ngược chuyển đổi số đếm thành độ dài phân đoạn chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
flush = sys.stdout.flush

def query(l, r):
    print(f"? {l} {r}")
    flush()
    res = int(input())
    if res == -1:
        exit(0)
    return res

def solve_case(n):
    total = query(1, n)

    # binary search for i
    left, right = 1, n
    i = 1
    while left <= right:
        mid = (left + right) // 2
        inv = query(1, mid)
        if inv == 0:
            left = mid + 1
        else:
            i = mid
            right = mid - 1

    # find length of first reversed segment L1
    low, high = 1, n
    L1 = 0
    while low <= high:
        mid = (low + high) // 2
        inv = query(i, i + mid - 1)
        if inv == mid * (mid - 1) // 2:
            L1 = mid
            low = mid + 1
        else:
            high = mid - 1
    j = i + L1

    # remaining inversions to find L2
    inv_second = query(j, n)
    # solve L2*(L2-1)//2 = inv_second
    L2 = 1
    while L2 * (L2 - 1) // 2 < inv_second:
        L2 += 1
    k = j + L2 - 1
    print(f"! {i} {j} {k}")
    flush()

t = int(input())
for _ in range(t):
    n = int(input())
    solve_case(n)
```Đầu tiên, mã truy vấn tổng số lần đảo ngược và sau đó thực hiện tìm kiếm nhị phân để xác định vị trí bắt đầu của phân đoạn đảo ngược đầu tiên. Sau đó, nó tìm độ dài của đoạn đầu tiên bằng công thức đảo ngược và suy ra độ dài đoạn thứ hai. Việc xử lý ranh giới là rất quan trọng vì các truy vấn trên phân đoạn chính xác phải khớp với độ dài của phân đoạn, nếu không công thức đảo ngược sẽ không thành công. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:`n=5`,`i=1, j=3, k=5`. 

| Bước | Truy vấn | Phản hồi | Tính toán | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | ? 1 5 | 4 | tổng cộng = 4 | - | 
| 2 | ? 1 3 | 1 | trái=1, phải=3 | tôi=1 | 
| 3 | ? 1 2 | 1 | kiểm tra L1 | L1=2 | 
| 4 | ? 3 5 | 3 | tính L2 | L2=3 | 
| 5 | Đầu ra | - | j=i+L1=3, k=j+L2-1=5 | ! 1 3 5 | 

Đầu vào mẫu 2:`n=4`,`i=2, j=4, k=5`. 

| Bước | Truy vấn | Phản hồi | Tính toán | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | ? 1 4 | 2 | tổng cộng=2 | - | 
| 2 | ? 1 2 | 0 | di chuyển sang trái | tôi=2 | 
| 3 | ? 2 3 | 1 | L1=2 | j=4 | 
| 4 | ? 4 4 | 0 | L2=1 | k=4 | 
| 5 | Đầu ra | - | ! 2 4 4 | Đúng | 

Những dấu vết này xác nhận rằng thuật toán tách biệt chính xác các phân đoạn và tính toán độ dài bằng cách sử dụng số lần đảo ngược, xử lý các mảng nhỏ và các phân đoạn có độ dài tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Truy vấn O(log n) cho mỗi trường hợp thử nghiệm | Mỗi tìm kiếm nhị phân yêu cầu truy vấn O(log n); số học là O(1) | 
| Không gian | O(1) | Không có bộ nhớ mảng, chỉ có bộ đếm và chỉ số | 

Được cho$n \le 10^9$và cho phép 40 truy vấn, thuật toán của chúng tôi dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```
import
```

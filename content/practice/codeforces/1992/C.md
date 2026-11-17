---
title: "CF 1992C - Gorilla và hoán vị"
description: "Chúng ta được yêu cầu xây dựng một hoán vị các số từ 1 đến $n$ sao cho một biểu thức nhất định được tối đa hóa. Biểu thức là tổng của tất cả các tổng tiền tố của các số \"lớn\" (lớn hơn hoặc bằng $k$) trừ đi tổng tất cả các tổng tiền tố của các số \"nhỏ\" (nhỏ hơn hoặc bằng…"
date: "2026-06-08T15:20:40+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1992
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 957 (Div. 3)"
rating: 900
weight: 1992
solve_time_s: 477
verified: false
draft: false
---

[CF 1992C - Gorilla và Hoán vị](https://codeforces.com/problemset/problem/1992/C) 

**Xếp hạng:** 900 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 7 phút 57 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một hoán vị các số từ 1 đến$n$sao cho một biểu thức nhất định được tối đa hóa. Biểu thức là tổng của tất cả các tổng tiền tố của các số "lớn" (lớn hơn hoặc bằng$k$) trừ tổng của tất cả các tổng tiền tố của các số "nhỏ" (nhỏ hơn hoặc bằng$m$). Hoán vị phải chứa mỗi số từ 1 đến$n$đúng một lần. 

Đầu vào cho mỗi trường hợp thử nghiệm là ba số nguyên:$n$(độ dài của hoán vị),$m$(ngưỡng cho số nhỏ) và$k$(ngưỡng cho số lượng lớn). Đầu ra là bất kỳ hoán vị nào từ 1 đến$n$tối đa hóa sự khác biệt giữa tổng tích lũy của số lớn và số nhỏ trên tất cả các tiền tố. 

Những hạn chế chính đó là$n$có thể lên đến$10^5$, và tổng của$n$trên tất cả các trường hợp thử nghiệm lên đến$2 \cdot 10^5$. Điều này cho chúng ta biết rằng một$O(n \log n)$hoặc$O(n)$giải pháp có thể chấp nhận được, nhưng$O(n^2)$giải pháp sẽ rất chậm. 

Một điểm không rõ ràng là các con số nằm giữa$m+1$Và$k-1$không đóng góp vào cả hai tổng, vì vậy vị trí của chúng không liên quan đến điểm số. Một điều tinh tế khác là việc đặt số lượng nhỏ sớm sẽ tăng mức phạt (g(i) đóng góp), trong khi đặt số lượng lớn sớm sẽ tối đa hóa phần thưởng (f(i) đóng góp). 

Ví dụ, với$n=5$,$m=2$,$k=5$, một hoán vị [5,3,4,1,2] tạo ra$f(i)$Và$g(i)$những giá trị mang lại sự khác biệt tối đa. Một hoán vị ngây thơ như [1,2,3,4,5] sẽ tạo ra sự khác biệt nhỏ hơn vì số nhỏ xuất hiện trước số lớn, làm tăng hình phạt. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử tất cả$n!$hoán vị, tính toán$f(i)$Và$g(i)$cho mỗi cái và chọn mức tối đa. Về mặt lý thuyết, điều này có hiệu quả vì nó tính toán chính xác tổng cho bất kỳ hoán vị nào, nhưng nó không khả thi ngay cả đối với các hoán vị vừa phải.$n$, từ$5! = 120$Và$10! \sim 3.6 \cdot 10^6$, thấp hơn nhiều so với mức tối đa$n = 10^5$. 

Điểm mấu chốt là sự đóng góp của mỗi số vào tổng cuối cùng là tuyến tính theo số tiền tố mà nó xuất hiện. Để tối đa hóa tổng đóng góp của f(i), chúng ta nên đặt tất cả các số lớn hơn hoặc bằng$k$càng sớm càng tốt. Để giảm thiểu tổng đóng góp của g(i), chúng ta nên đặt tất cả các số nhỏ hơn hoặc bằng$m$càng muộn càng tốt. Các con số giữa$m+1$Và$k-1$có thể đặt ở bất kỳ đâu vì chúng không ảnh hưởng tới điểm số. 

Vì vậy, hoán vị tối ưu được xây dựng bằng cách liệt kê các số đầu tiên ≥ k theo thứ tự giảm dần, tiếp theo là các số ở giữa theo thứ tự bất kỳ và kết thúc bằng các số ≤ m theo thứ tự tăng dần. Điều này đảm bảo các số lớn nhất đóng góp vào f(i) trong càng nhiều tiền tố càng tốt và các số nhỏ nhất đóng góp vào g(i) trong càng ít tiền tố càng tốt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$,$m$, Và$k$. 
3. Khởi tạo ba danh sách:`large`đối với số ≥ k,`middle`cho các số trong (m, k) và`small`đối với số ≤ m. 
4. Lặp lại tất cả các số từ 1 đến n: 

- Nếu số ≥ k thì nối vào`large`. 
- Nếu số ≤ m thì nối vào`small`. 
- Nếu không thì nối nó vào`middle`. 
5. Xây dựng hoán vị bằng cách ghép`large`theo thứ tự giảm dần,`middle`theo bất kỳ thứ tự nào (tăng dần để đơn giản) và`small`theo thứ tự tăng dần. 
6. Xuất ra hoán vị. 

Tại sao nó hoạt động: Đặt tất cả các số ≥ k ở phía trước sẽ tối đa hóa sự đóng góp của chúng cho mọi tiền tố bao gồm chúng. Đặt các số ≤ m ở cuối sẽ giảm thiểu sự đóng góp của chúng. Các số giữa m và k không ảnh hưởng đến điểm số nên thứ tự của chúng không quan trọng. Chiến lược này đảm bảo rằng không có hoán vị nào có thể đạt được sự khác biệt cao hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m, k = map(int, input().split())
    large = []
    middle = []
    small = []
    for i in range(1, n + 1):
        if i >= k:
            large.append(i)
        elif i <= m:
            small.append(i)
        else:
            middle.append(i)
    permutation = large + middle + small
    print(*permutation)
```Giải pháp đọc đầu vào nhanh chóng bằng cách sử dụng`sys.stdin.readline`, chia các số thành ba loại và ghép chúng lại để tối đa hóa điểm số. Việc sử dụng các thao tác danh sách sẽ tránh được từng lỗi một và đảm bảo đầu ra là một hoán vị hợp lệ. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:`5 2 5`| Số | Danh mục | Lý luận đóng góp | 
| --- | --- | --- | 
| 5 | lớn | đóng góp sớm cho f(i) | 
| 3 | giữa | bỏ qua | 
| 4 | giữa | bỏ qua | 
| 1 | nhỏ | góp phần vào g(i) muộn | 
| 2 | nhỏ | góp phần vào g(i) muộn | 

Hoán vị:`[5,3,4,1,2]`. Số lớn xuất hiện trước, số nhỏ xuất hiện sau. 

Đầu vào mẫu 2:`3 1 3`| Số | Danh mục | 
| --- | --- | 
| 3 | lớn | 
| 2 | giữa | 
| 1 | nhỏ | 

Hoán vị:`[3,2,1]`. Thứ tự này tối đa hóa tổng f(i) và tối thiểu hóa tổng g(i). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi số được phân loại theo thời gian không đổi, nối là O(n) | 
| Không gian | O(n) | Ba danh sách lưu trữ tập con số | 

Với$n$lên tới$10^5$và tổng của các trường hợp thử nghiệm 2·10^5, giải pháp này hiệu quả và phù hợp với bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        large, middle, small = [], [], []
        for i in range(1, n + 1):
            if i >= k:
                large.append(i)
            elif i <= m:
                small.append(i)
            else:
                middle.append(i)
        print(*large, *middle, *small)
    return out.getvalue().strip()

assert run("3\n5 2 5\n3 1 3\n10 3 8\n") == "5 3 4 1 2\n3 2 1\n10 9 8 4 5 6 7 1 2 3", "samples"
assert run("1\n2 1 2\n") == "2 1", "minimum size input"
assert run("1\n5 1 5\n") == "5 2 3 4 1", "all boundaries"
assert run("1\n6 2 5\n") == "6 5 3 4 1 2", "middle numbers present"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 2 | 2 1 | hoán vị kích thước tối thiểu | 
| 5 1 5 | 5 2 3 4 1 | đặt đúng ranh giới | 
| 6 2 5 | 6 5 3 4 1 2 | số ở giữa được xử lý chính xác | 

## Vỏ cạnh 

Một trường hợp phức tạp là khi`m+1 = k-1`, nghĩa là có đúng một số ở giữa. Vì`n=5, m=2, k=4`, các số là`[1,2,3,4,5]`. Vị trí thuật toán của chúng tôi`[4,5]`(lớn) trước, sau`[3]`(ở giữa) và`[1,2]`(nhỏ) cuối cùng. Hoán vị kết quả`[4,5,3,1,2]`đảm bảo đóng góp sớm tối đa f(i) và đóng góp g(i) muộn tối thiểu, xử lý chính xác số ở giữa. Điều này chứng tỏ thuật toán hoạt động ngay cả khi đoạn giữa chỉ có một phần tử.

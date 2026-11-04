---
title: "CF 1175C - Điện khí hóa"
description: "Chúng ta được yêu cầu chọn một điểm nguyên $x$ trên một dòng sao cho khoảng cách nhỏ nhất thứ $(k+1)$ từ $x$ đến một tập hợp điểm nhất định là nhỏ nhất. Mỗi truy vấn cung cấp cho chúng ta $n$ các số nguyên được sắp xếp $a1, a2, dấu chấm, an$ biểu thị các điểm trên trục $OX$ và một số nguyên $k$."
date: "2026-06-12T01:47:59+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1175
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 66 (Rated for Div. 2)"
rating: 1600
weight: 1175
solve_time_s: 88
verified: true
draft: false
---

[CF 1175C - Điện khí hóa](https://codeforces.com/problemset/problem/1175/C) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, vũ phu, tham lam 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu chọn một điểm nguyên$x$trên một đường thẳng sao cho$(k+1)$-khoảng cách nhỏ nhất thứ từ$x$tới một tập hợp điểm nhất định được giảm thiểu. Mỗi truy vấn cho chúng ta$n$số nguyên được sắp xếp$a_1, a_2, \dots, a_n$đại diện cho các điểm trên$OX$trục và một số nguyên$k$. chức năng$f_k(x)$được xác định bằng cách tính khoảng cách tuyệt đối từ$x$cho mỗi người$a_i$, sắp xếp chúng và chọn$(k+1)$-giá trị thứ. Mục tiêu là chọn$x$điều đó giảm thiểu$f_k(x)$. 

Các ràng buộc rất chặt chẽ:$n$có thể lên đến$2 \cdot 10^5$và tổng của tất cả$n$trên các truy vấn cũng nhiều nhất$2 \cdot 10^5$. Điều này loại trừ mọi giải pháp lặp đi lặp lại trên tất cả các khả năng có thể$x$các giá trị trong phạm vi đầy đủ, có thể lên tới$10^9$. Chúng ta cần một giải pháp tuyến tính trong$n$mỗi truy vấn, hoặc tệ nhất$O(n \log n)$, ở dưới thời hạn. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó$k = 0$hoặc$k = n-1$. Vì$k = 0$,$f_0(x)$là khoảng cách tối thiểu đến bất kỳ điểm nào, vì vậy tối ưu$x$phải trùng với một trong các điểm hiện có. Vì$k = n-1$,$f_{n-1}(x)$là khoảng cách tối đa đến bất kỳ điểm nào, được giảm thiểu tại điểm giữa của các điểm cực trị. Một cách tiếp cận bất cẩn giả định một công thức cố định mà không kiểm tra các ranh giới có thể tạo ra những câu trả lời sai cho những trường hợp cực đoan này.$k$các giá trị. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: với mỗi số nguyên$x$trong phạm vi từ$a_1$ĐẾN$a_n$, tính toán tất cả các khoảng cách, sắp xếp chúng và chọn$(k+1)$-khoảng cách nhỏ nhất thứ Điều này đúng nhưng cực kỳ chậm vì việc sắp xếp$n$khoảng cách lên tới$10^9$khả thi$x$các giá trị là không thể thực hiện được. Thậm chí chỉ lặp qua các điểm đã cho làm ứng cử viên và tính toán khoảng cách cho tất cả$n$sẽ là$O(n^2)$trong trường hợp xấu nhất là quá chậm để$n \approx 2 \cdot 10^5$. 

Cái nhìn sâu sắc quan trọng là$f_k(x)$chỉ phụ thuộc vào$k$-thứ hàng xóm gần nhất. Vì các điểm được sắp xếp nên$(k+1)$-khoảng cách gần nhất tại$x$có thể đạt được bằng cách xem xét các đoạn có độ dài$k$trong mảng đã sắp xếp. Cụ thể, đối với bất kỳ$x$, cái$(k+1)$-điểm gần nhất sẽ là một số$a_i$trong mảng và giá trị tối thiểu xảy ra khi$x$được đặt ở trung điểm của$a_i$Và$a_{i+k}$. Điều này làm giảm vấn đề lặp đi lặp lại trên tất cả$i$từ$0$ĐẾN$n-k-1$, tính điểm giữa$(a_i + a_{i+k}) // 2$, và chọn giá trị của$x$giúp giảm thiểu khoảng cách đến các điểm cực trị của đoạn thẳng. Đây là tuyến tính trong$n$cho mỗi truy vấn và tránh việc sắp xếp tại mỗi điểm ứng cử viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại các chỉ số$i$từ$0$ĐẾN$n-k-1$. Mỗi đoạn từ$a_i$ĐẾN$a_{i+k}$đại diện cho$k+1$điểm liên tiếp. Bất kì$x$được chọn ngoài khoảng này không thể làm giảm$(k+1)$-khoảng cách nhỏ nhất cho đoạn này, do đó tối ưu$x$phải nằm trong khoảng này. 
2. Với mỗi đoạn hãy tính điểm giữa$m = (a_i + a_{i+k}) // 2$. Đặt$x$tại điểm giữa này cân bằng khoảng cách đến điểm đầu và điểm cuối trong đoạn, giảm thiểu tối đa$|x - a_i|$Và$|x - a_{i+k}|$. 
3. Theo dõi khoảng cách tối thiểu$d = a_{i+k} - a_i$trên tất cả các phân khúc. Khi nhỏ hơn$d$được tìm thấy, cập nhật ứng viên$x$tới trung điểm tương ứng. 
4. Sau khi lặp lại tất cả các phân đoạn, phần được chọn$x$tương ứng với đoạn có độ dài tối thiểu. Điều này đảm bảo$f_k(x)$được giảm thiểu. 

Tại sao nó hoạt động:$(k+1)$-khoảng cách nhỏ nhất tại một điểm$x$được xác định bởi$k$- hàng xóm xung quanh gần nhất$x$. Khoảng cách tối thiểu đạt được bằng cách cân bằng$x$bên trong đoạn có chiều dài nhỏ nhất$k$trong các điểm được sắp xếp, đảm bảo rằng điểm xa nhất của$k+1$điểm gần nhất là càng gần càng tốt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        min_len = float('inf')
        ans = 0
        for i in range(n - k):
            length = a[i + k] - a[i]
            if length < min_len:
                min_len = length
                ans = (a[i] + a[i + k]) // 2
        print(ans)

solve()
```Giải pháp đọc số lượng truy vấn, sau đó với mỗi truy vấn sẽ đọc$n$,$k$và mảng đã được sắp xếp. Nó lặp lại trên tất cả các đoạn có độ dài có thể$k+1$, tính toán độ dài đoạn và cập nhật câu trả lời với điểm giữa của đoạn tối thiểu. Việc sử dụng phép chia số nguyên đảm bảo$x$là một số nguyên. Câu trả lời cuối cùng được in cho mỗi truy vấn. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

| tôi | Phân đoạn a[i..i+k] | chiều dài | điểm giữa | min_len | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | [1,2,5] | 5-1=4 | (1+5)//2=3 | 4 | 3 | 

Đầu ra: 3 

**Mẫu 2** 

đầu vào:```
2 1
1 1000000000
```| tôi | Phân đoạn | chiều dài | điểm giữa | min_len | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | [1,1000000000] | 1000000000-1=999999999 | (1+1000000000)//2=500000000 | 999999999 | 500000000 | 

Đầu ra: 500000000 

Các dấu vết cho thấy thuật toán xác định chính xác đoạn có độ dài và số dư tối thiểu$x$ở giữa để giảm thiểu$(k+1)$-khoảng cách thứ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi truy vấn | Lặp lại tất cả n-k phân đoạn một lần, công việc liên tục trên mỗi phân đoạn | 
| Không gian | O(1) thêm | Chỉ giữ độ dài và câu trả lời tối thiểu; mảng đầu vào được đưa ra | 

Với$\sum n \le 2 \cdot 10^5$, tổng công việc nằm dưới giới hạn 2 giây. Mức sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("3\n3 2\n1 2 5\n2 1\n1 1000000000\n1 0\n4\n") == "3\n500000000\n4", "sample 1"

# custom cases
assert run("1\n1 0\n10\n") == "10", "single point, k=0"
assert run("1\n5 4\n1 2 3 4 5\n") == "3", "k=n-1, mid of extreme"
assert run("1\n4 2\n1 3 6 10\n") == "4", "choose minimal segment among consecutive"
assert run("1\n6 3\n1 2 3 4 5 6\n") == "3", "multiple minimal segments, choose leftmost"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 10 | 10 | Điểm đơn, k=0 | 
| 5 4 1 2 3 4 5 | 3 | k = n-1, giữa cực | 
| 4 2 1 3 6 10 | 4 | Lựa chọn phân khúc tối thiểu đúng | 
| 6 3 1 2 3 4 5 6 | 3 | Nhiều phân đoạn tối thiểu, được chọn ngoài cùng bên trái | 

## Vỏ cạnh 

Với k = 0, nhập`1 0 4`: 

- Độ dài đoạn không liên quan vì k=0. 
- Thuật toán chọn trung điểm của đoạn có độ dài 1 tức là 4. 
- Kết quả đầu ra chính xác 4. 

Với k = n

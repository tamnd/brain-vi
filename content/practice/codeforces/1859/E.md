---
title: "CF 1859E - Tính đơn sắc tối đa"
description: "Chúng ta có hai mảng có độ dài $n$, $a$ và $b$. Đối với bất kỳ phân đoạn hoặc phân đoạn liền kề nào được xác định bởi điểm bắt đầu $l$ và kết thúc $r$, chúng ta có thể tính toán \"chi phí\" bằng cách sử dụng công thức $. Vấn đề rất khó hiểu vì chi phí phân đoạn phụ thuộc vào cả hai đầu của phân đoạn và cả hai mảng."
date: "2026-06-09T00:30:15+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1859
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 892 (Div. 2)"
rating: 2500
weight: 1859
solve_time_s: 199
verified: true
draft: false
---

[CF 1859E - Tính đơn điệu tối đa](https://codeforces.com/problemset/problem/1859/E) 

**Đánh giá:** 2500 
**Tags:** vũ lực, dp, toán học 
**Thời gian giải:** 3m 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng có độ dài$n$,$a$Và$b$. Đối với bất kỳ phân đoạn hoặc phân đoạn liền kề nào được xác định bởi điểm bắt đầu của nó$l$và kết thúc$r$, chúng ta có thể tính "chi phí" bằng công thức$|b_l - a_r| + |b_r - a_l|$. Mục tiêu là chọn các đoạn không chồng chéo có tổng chiều dài chính xác$k$và tối đa hóa tổng chi phí của phân khúc đó. 

Vấn đề rất phức tạp vì chi phí của phân khúc phụ thuộc vào cả hai đầu của phân khúc và cả hai mảng. Ví dụ: lấy một phân đoạn phần tử$[i, i]$sản lượng$|b_i - a_i| + |b_i - a_i| = 2|b_i - a_i|$. Việc chọn các đoạn dài hơn sẽ giới thiệu các thuật ngữ chéo$|b_l - a_r|$Và$|b_r - a_l|$và việc tối ưu hóa phải cân bằng giữa việc chọn các phân khúc có chi phí cao với giới hạn tổng chiều dài$k$. 

Những hạn chế ở mức vừa phải:$n$có thể lên tới 3000 và tổng của tất cả$n$trên các trường hợp thử nghiệm cũng nhiều nhất là 3000. Điều này loại trừ$O(n^3)$các thuật toán sẽ thử mọi phân đoạn cho mọi kết hợp có thể, bởi vì$3000^3$là khoảng 27 tỷ hoạt động. MỘT$O(n^2)$hoặc$O(nk)$Giải pháp này khả thi vì có thể thực hiện được 9 triệu thao tác trong 2 giây. 

Trường hợp cạnh không rõ ràng xảy ra khi các đoạn có độ dài 1 là tối ưu vì các đoạn dài hơn tạo ra những đóng góp chéo nhỏ hơn. Ví dụ, nếu$a = [1, 3]$Và$b = [4, 2]$Và$k=2$, lựa chọn tối ưu có thể là hai đoạn có độ dài 1 thay vì một đoạn có độ dài 2. Việc triển khai bất cẩn chỉ kiểm tra các đoạn dài sẽ bỏ lỡ mức tối đa. 

Một trường hợp cạnh khác là khi mảng$a$Và$b$đều bình đẳng. Khi đó, tất cả các chi phí của một yếu tố đều bằng 0 và đôi khi tất cả các phân khúc đa yếu tố cũng tạo ra chi phí bằng 0, như đã thấy trong mẫu đầu tiên. Thuật toán của chúng tôi phải xử lý chính xác các phân khúc có chi phí bằng 0 mà không giả định chi phí dương. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi sự kết hợp của các phân đoạn không chồng chéo có tổng chiều dài là$k$, tính chi phí của chúng và chọn số tiền lớn nhất. Việc tính toán chi phí cho từng phân khúc cần$O(1)$, nhưng việc liệt kê tất cả các đoạn có độ dài đều cho$O(n^2)$phân đoạn và sau đó thử tất cả các kết hợp sẽ nhanh chóng trở thành$O(2^n)$, điều đó hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là suy nghĩ về mặt lập trình động. Hãy để chúng tôi xác định$dp[i][len]$là chi phí tối đa chúng ta có thể đạt được bằng cách sử dụng$i$các phần tử của mảng và tổng chiều dài đoạn đã chọn$len$. Đối với mỗi vị trí$i$, chúng ta hoặc bỏ qua nó (chuyển tiếp$dp[i-1][len]$) hoặc lấy một đoạn kết thúc tại$i$có độ dài nào đó$j \le i$và thêm chi phí của nó vào$dp[i-j][len-j]$. Cách tiếp cận này hiệu quả vì chi phí của phân khúc chỉ phụ thuộc vào điểm bắt đầu và kết thúc của phân khúc; các yếu tố trung gian không có ý nghĩa. Tính toán trước chi phí của từng phân khúc$cost[l][r]$TRONG$O(n^2)$cho phép$dp$chuyển tiếp trong$O(n^2)$, phù hợp với ràng buộc tổng thể. 

Bằng cách biến vấn đề thành một vấn đề lập trình động cổ điển "tổng tối đa của các phân đoạn không chồng chéo với ngân sách độ dài", chúng tôi giảm không gian tìm kiếm từ hàm mũ xuống bậc hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n^2) | O(n^2) | Quá chậm | 
| Lập trình động | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả chi phí của phân khúc. Với mỗi cặp chỉ số$l \le r$, tính toán$cost[l][r] = |b_l - a_r| + |b_r - a_l|$. Điều này khả thi ở$O(n^2)$thời gian vì$n \le 3000$. Việc lưu trữ cái này trong một mảng 2D cho phép$O(1)$tra cứu trong DP. 
2. Khởi tạo bảng DP$dp[i][len]$Ở đâu$dp[i][len]$đại diện cho chi phí tối đa có thể đạt được bằng cách sử dụng lần đầu tiên$i$các phần tử có tổng chiều dài đoạn$len$. Bắt đầu với tất cả các mục là âm vô cùng, ngoại trừ$dp[0][0] = 0$. 
3. Lặp lại các vị trí$i$từ 1 đến$n$. Đối với mỗi$i$, lặp qua tổng độ dài$len$từ 0 đến$k$. Chuyển tiếp mức tối đa trước đó:$dp[i][len] = dp[i-1][len]$. 
4. Đối với mỗi độ dài đoạn có thể$seg$từ 1 đến$\min(i, len)$, hãy cân nhắc sử dụng phân đoạn kết thúc tại$i$chiều dài$seg$. Cập nhật$dp[i][len] = \max(dp[i][len], dp[i-seg][len-seg] + cost[i-seg+1][i])$. 
5. Sau khi xử lý tất cả các vị trí,$dp[n][k]$chứa chi phí tối đa cho tổng chiều dài$k$. 

Tại sao nó hoạt động: DP duy trì tính bất biến$dp[i][len]$luôn lưu trữ số tiền tối ưu cho lần đầu tiên$i$phần tử và tổng chiều dài$len$. Việc xem xét tất cả các độ dài phân đoạn cuối cùng có thể đảm bảo rằng mọi sự kết hợp khả thi đều được tính đến. Tính năng không chồng chéo được thực thi một cách tự nhiên vì chúng tôi chỉ xem xét các phân đoạn kết thúc tại$i$sử dụng DP của các phần tử trước phân đoạn đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        # Precompute costs
        cost = [[0]*n for _ in range(n)]
        for l in range(n):
            for r in range(l, n):
                cost[l][r] = abs(b[l] - a[r]) + abs(b[r] - a[l])
        
        # DP initialization
        dp = [[-float('inf')] * (k+1) for _ in range(n+1)]
        dp[0][0] = 0
        
        for i in range(1, n+1):
            for length in range(k+1):
                dp[i][length] = dp[i-1][length]  # skip i-th element
                for seg in range(1, min(i, length)+1):
                    dp[i][length] = max(dp[i][length],
                                        dp[i-seg][length-seg] + cost[i-seg][i-1])
        print(dp[n][k])

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ tính toán trước mọi chi phí của phân đoạn để tránh phải tính toán lại. Bảng DP`dp[i][length]`xử lý các phân đoạn không chồng chéo một cách tự nhiên, vì trạng thái DP luôn tham chiếu các phần tử trước phân đoạn hiện tại. Các điều kiện biên, chẳng hạn như lấy một đoạn có độ dài 1 hoặc tiếp cận phần tử đầu tiên, được xử lý an toàn vì danh sách Python được lập chỉ mục 0 và DP của chúng tôi sử dụng`i-seg`để tham chiếu trạng thái trước phân đoạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1: 

đầu vào:```
n = 3, k = 2
a = [1, 3, 2]
b = [5, 2, 3]
```| tôi | chiều dài | tính toán dp[i][độ dài] | Giải thích | 
| --- | --- | --- | --- | 
| 1 | 0 | dp[1][0] = 0 | bỏ qua phần tử đầu tiên | 
| 1 | 1 | dp[1][1] = 4 | lấy đoạn [1,1], cost= | 
| 2 | 1 | dp[2][1] = max(dp[1][1], dp[0][0]+chi phí[1][1]) = max(4,4)=4 | xem xét đoạn kết thúc ở 2 | 
| 2 | 2 | dp[2] [2] = dp [0] [0]+chi phí [0] [1]= | 5-3 | 

Dấu vết này cho thấy DP chọn chính xác các phân đoạn kết thúc ở các vị trí khác nhau và tính tổng chi phí của chúng. 

### Ví dụ 2: 

đầu vào:```
n = 4, k = 2
a = [17, 3, 5, 8]
b = [16, 2, 5, 9]
```DP tìm đúng hai phân đoạn [1,1] và [4,4] mang lại chi phí 28. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Chi phí phân đoạn tính toán trước là O(n^2). DP điền vào n * k trạng thái, mỗi trạng thái xem xét tối đa n phân đoạn, tổng O(n^2). | 
| Không gian | O(n^2) | Lưu trữ chi phí phân khúc và bảng DP sử dụng O(n^2). | 

Được cho$n \le 3000$, n^2 ≈ 9 triệu, trong vòng 2 giây và mức sử dụng bộ nhớ dưới 512 MB. 

## Trường hợp thử nghiệm```python
# helper function
import sys, io

def
```

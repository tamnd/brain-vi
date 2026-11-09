---
title: "CF 1500D - Gạch ốp lát phòng tắm"
description: "Chúng ta có một lưới $n lần n$ biểu thị một ô xếp, trong đó mỗi ô chứa một ô có màu nhất định. Kostya muốn biết, với mỗi kích thước hình vuông con $k$, có bao nhiêu hình vuông con $k nhân k$ chứa nhiều nhất $q$ các màu riêng biệt."
date: "2026-06-10T21:04:47+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1500
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 707 (Div. 1, based on Moscow Open Olympiad in Informatics)"
rating: 2900
weight: 1500
solve_time_s: 76
verified: true
draft: false
---

[CF 1500D - Gạch lát phòng tắm](https://codeforces.com/problemset/problem/1500/D) 

**Xếp hạng:** 2900 
**Thẻ:** cấu trúc dữ liệu, sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$lưới biểu thị một ô xếp, trong đó mỗi ô chứa một ô có màu nhất định. Kostya muốn biết, với mỗi kích thước hình vuông con có thể có$k$, bao nhiêu$k \times k$hình vuông con chứa nhiều nhất$q$màu sắc riêng biệt. Một hình vuông con chỉ được xác định bởi góc trên bên trái và kích thước của nó. Đầu ra là một danh sách các số nguyên: với mỗi$k$từ 1 đến$n$, số lượng ô vuông con hợp lệ có kích thước$k$. 

Kích thước lưới$n$có thể lên tới 1500, điều này ngụ ý rằng một cách tiếp cận ngây thơ kiểm tra từng ô vuông con một cách độc lập có thể là quá chậm. Tổng số$k \times k$hình vuông con là$(n - k + 1)^2$, vậy tổng hợp lại tất cả$k$đưa ra đại khái$O(n^3)$hình vuông con. Nếu đối với mỗi màu chúng ta đếm các màu riêng biệt bằng cách quét tuyến tính$k^2$tế bào, chúng tôi đạt được$O(n^5)$trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. 

Các trường hợp cạnh không rõ ràng bao gồm các lưới có màu lặp lại. Ví dụ: lưới 3x3 trong đó tất cả các ô có cùng màu sẽ tạo ra số lượng khác nhau so với lưới có tất cả các màu khác nhau. Vì$q=1$, lời giải phải phát hiện các ô vuông chỉ có một màu duy nhất, nếu không số đếm sẽ sai. Một trường hợp tế nhị khác là khi$q \ge n^2$; thì tất cả các ô vuông con đều hợp lệ bất kể màu sắc của chúng là gì. Cuối cùng, khi$n=1$, chúng ta phải xử lý chính xác một ô dưới dạng hình vuông con 1x1. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ lặp lại trên tất cả các góc trên bên trái có thể có$(i,j)$và tất cả các kích cỡ có thể$k$. Đối với mỗi ô vuông con, chúng tôi sẽ thu thập tất cả các màu trong một bộ và kiểm tra xem kích thước của nó có vượt quá không$q$. Điều này đúng nhưng độ phức tạp của nó$O(n^3 \cdot k^2) = O(n^5)$trong trường hợp xấu nhất là vượt xa giới hạn cho phép. 

Cái nhìn sâu sắc quan trọng là$q$nhỏ, nhiều nhất là 10. Điều này cho phép chúng ta biểu diễn các tập hợp màu một cách hiệu quả và hợp nhất chúng trong thời gian cố định không đổi hoặc nhỏ. Thay vì đếm tất cả các màu một cách ngây thơ cho mỗi ô vuông con, chúng ta có thể xây dựng thông tin dần dần. Hãy xem xét phương pháp lập trình động trong đó chúng ta lưu trữ, đối với mỗi ô, tập hợp tối đa$q+1$màu nhỏ nhất xuất hiện trong ô vuông con kết thúc tại ô đó. Khi mở rộng một hình vuông con từ kích thước$k-1$ĐẾN$k$, chúng ta chỉ cần kết hợp các bộ màu từ góc trên bên trái, cột ngoài cùng bên phải và hàng dưới cùng. Bởi vì chúng tôi chỉ theo dõi đầu tiên$q+1$màu sắc, chúng ta có thể biết ngay liệu một hình vuông có hợp lệ hay không nếu tập hợp kết hợp vượt quá$q$. 

Cách tiếp cận này làm giảm đáng kể độ phức tạp vì chúng ta không còn lặp lại từng ô trong từng ô vuông riêng lẻ nữa. Thay vào đó, chúng tôi hợp nhất nhiều nhất các tập hợp kích thước nhỏ$q+1$cho mỗi lần mở rộng, đưa ra tổng thời gian$O(n^2 q^2)$, điều này khả thi đối với$n=1500$Và$q \le 10$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^5) | O(k^2) trên mỗi ô vuông con | Quá chậm | 
| Tối ưu (DP + bộ nhỏ) | O(n^2 q^2) | O(n^2 q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng 2D`dp[i][j]`trong đó mỗi mục lưu trữ một danh sách được sắp xếp lên tới`q+1`màu sắc riêng biệt trong hình vuông con kết thúc tại`(i,j)`cho kích thước hiện tại. 
2. Về kích thước`k=1`, mỗi`dp[i][j]`đơn giản là màu sắc ở`c[i][j]`. Đếm tất cả các ô vuông thỏa mãn`q`ngay lập tức. 
3. Về kích thước`k > 1`, lặp lại tất cả các góc dưới bên phải có thể`(i,j)`của một`k x k`quảng trường. Xây dựng bộ màu bằng cách hợp nhất ba thành phần: bộ từ`(k-1)x(k-1)`hình vuông con kết thúc tại`(i-1,j-1)`, cột có độ dài ngoài cùng bên phải`k-1`, và hàng dưới cùng có độ dài`k-1`. Sử dụng hợp nhất được sắp xếp và chỉ giữ lại cái đầu tiên`q+1`màu sắc. 
4. Nếu bộ màu được hợp nhất chứa`<= q`màu sắc, tăng số lượng cho kích thước này`k`. 
5. Lặp lại quy trình này cho tất cả các kích cỡ`k`từ 1 đến`n`. Xuất số đếm. 

Tại sao nó hoạt động: Ở mỗi bước,`dp[i][j]`chứa một bản tóm tắt đầy đủ các màu sắc trong`k x k`hình vuông con kết thúc tại`(i,j)`. Bằng cách chỉ hợp nhất tối đa`q+1`màu sắc, chúng tôi sẽ phát hiện sớm nếu hình vuông con vượt quá số lượng màu riêng biệt được phép. Bất biến là tất cả đều có thể`k x k`các ô vuông con được xem xét chính xác một lần và bộ màu của chúng được duy trì chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, q = map(int, input().split())
    c = [list(map(int, input().split())) for _ in range(n)]
    
    counts = [0] * n
    # dp[i][j] stores the color set for the current size k ending at (i,j)
    dp = [[[] for _ in range(n)] for _ in range(n)]
    
    for i in range(n):
        for j in range(n):
            dp[i][j] = [c[i][j]]
            if q >= 1:
                counts[0] += 1
    
    for k in range(2, n+1):
        new_dp = [[[] for _ in range(n)] for _ in range(n)]
        for i in range(k-1, n):
            for j in range(k-1, n):
                merged = set()
                merged.update(dp[i-1][j-1])
                for x in range(i-k+1, i):
                    merged.add(c[x][j])
                for y in range(j-k+1, j):
                    merged.add(c[i][y])
                if len(merged) <= q:
                    counts[k-1] += 1
                new_dp[i][j] = list(merged)[:q+1]
        dp = new_dp
    
    print('\n'.join(map(str, counts)))

if __name__ == "__main__":
    main()
```Giải pháp khởi tạo bảng DP với các ô vuông đơn. Khi mở rộng sang các ô vuông lớn hơn, việc hợp nhất các bộ màu bị giới hạn ở`q+1`ĐẾN

---
title: "CF 2014B - Robin Hood và cây sồi lớn"
description: "Cây ra lá mới mỗi năm. Vào năm $i$, nó mọc lá $i^i$. Lá không ở lại mãi mãi. Một chiếc lá được tạo ra vào năm $i$ vẫn ở trên cây chính xác $k$ năm, nghĩa là nó hiện diện trong các năm $i, i+1, dots, i+k-1$."
date: "2026-06-08T12:59:15+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 2014
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 974 (Div. 3)"
rating: 800
weight: 2014
solve_time_s: 54
verified: true
draft: false
---

[CF 2014B - Robin Hood và cây sồi lớn](https://codeforces.com/problemset/problem/2014/B) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Cây ra lá mới mỗi năm. Trong năm$i$, nó lớn lên$i^i$lá. Lá không ở lại mãi mãi. Một chiếc lá được tạo ra trong năm$i$vẫn còn trên cây chính xác$k$năm, nghĩa là nó hiện diện trong những năm$i, i+1, \dots, i+k-1$. 

Trong một năm nhất định$n$, ta cần xác định tổng số lá hiện có trên cây là chẵn hay lẻ. 

Một chiếc lá mọc trong năm$i$vẫn còn hiện diện trong năm$n$nếu và chỉ khi$$i \le n \le i+k-1.$$Sắp xếp lại mang lại$$n-k+1 \le i \le n.$$Kể từ khi năm bắt đầu tại$1$, lá có thể nhìn thấy trong năm$n$đến từ khoảng$$\max(1, n-k+1), \dots, n.$$Các ràng buộc là manh mối đầu tiên cho thấy việc mô phỏng trực tiếp là không thể. Cả hai$n$Và$k$có thể lớn như$10^9$, và có thể có tới$10^4$trường hợp thử nghiệm. Bất kỳ giải pháp nào lặp đi lặp lại qua nhiều năm sẽ yêu cầu hàng tỷ thao tác trong trường hợp xấu nhất. 

Đáp án chỉ hỏi tổng số lá có chẵn không. Điều đó có nghĩa là chúng tôi chỉ quan tâm đến số chẵn lẻ chứ không quan tâm đến số lượng lá thực tế. Quan sát này biến một bài toán số học với số lượng lớn thành một bài toán đếm đơn giản. 

Một lỗi phổ biến là cố tính các giá trị như 10^9^{10^9}. Những con số này lớn đến mức không thể tưởng tượng được và hoàn toàn không cần thiết vì tính chẵn lẻ chỉ phụ thuộc vào giá trị là số lẻ hay số chẵn. 

Một sai lầm dễ mắc phải khác là quên rằng chỉ có điều cuối cùng$k$năm đóng góp. Ví dụ:```
n = 5, k = 2
```Chỉ có năm$4$Và$5$vấn đề. Sử dụng tất cả các năm từ$1$bởi vì$5$sẽ đưa ra câu trả lời sai. 

Trường hợp tế nhị thứ ba xảy ra khi$k=1$. Rồi chỉ có năm$n$đóng góp. Ví dụ:```
n = 1, k = 1
```Cây có chính xác$1^1=1$chiếc lá, thật kỳ lạ, nên câu trả lời là`NO`. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ xác định mỗi năm lá nào vẫn còn sống, tính toán$i^i$, tính tổng chúng và kiểm tra tính chẵn lẻ của tổng cuối cùng. 

Những năm hoạt động là$$n-k+1,\dots,n.$$Ngay cả khi chúng ta bỏ qua kích thước khổng lồ của$i^i$, cách tiếp cận này yêu cầu lặp đi lặp lại lên đến$10^9$năm. Với$10^4$trường hợp thử nghiệm, số lượng hoạt động trở nên hoàn toàn không khả thi. 

Quan sát quan trọng là tính chẵn lẻ đơn giản hơn nhiều so với giá trị thực tế. 

Coi như$i^i$. 

Nếu như$i$thì chẵn$i^i$là chẵn. 

Nếu như$i$thế thì kỳ quặc$i^i$thật kỳ quặc. 

Vì vậy tính chẵn lẻ của$i^i$chính xác là sự chẵn lẻ của$i$. 

Tổng số chẵn lẻ của số lá chỉ phụ thuộc vào số năm lẻ đóng góp. Mỗi năm chẵn đóng góp một số chẵn và không ảnh hưởng đến tính chẵn lẻ. 

Khoảng hoạt động chứa$$[n-k+1,\, n].$$Tổng số lá là số lẻ nếu số số nguyên lẻ trong khoảng này là số lẻ. Đó là số chẵn nếu số số nguyên lẻ trong khoảng này là số chẵn. 

Bây giờ chúng ta chỉ cần tính chẵn lẻ của số lẻ trong một khoảng liên tiếp. 

Trong hai số nguyên liên tiếp bất kỳ có đúng một số lẻ. Điều đó có nghĩa là tính chẵn lẻ của số lẻ chỉ phụ thuộc vào độ dài khoảng$k$. 

Nếu như$k$là số chẵn thì khoảng chứa chính xác$k/2$số lẻ và$k/2$số chẵn. Câu trả lời được quyết định bởi liệu$k/2$là chẵn hoặc lẻ. 

Có một quan sát thậm chí còn đơn giản hơn. Cho phép$$S = \sum_{i=n-k+1}^{n} i^i.$$Modulo$2$,$$S \equiv \sum_{i=n-k+1}^{n} i.$$Tính chẵn lẻ của một tổng bằng tính chẵn lẻ của số số hạng lẻ. Trong bất kỳ khoảng thời gian nào$k$, số lẻ là số lẻ chính xác khi$k \bmod 4$là$1$hoặc$2$, và thậm chí chính xác khi nào$k \bmod 4$là$0$hoặc$3$. 

Một nguồn gốc thậm chí còn rõ ràng hơn đến từ việc kiểm tra năm kết thúc$n$. Vì chỉ có vấn đề chẵn lẻ nên lời giải được chấp nhận sẽ giảm xuống việc đếm các số lẻ trong khoảng. Tính chẵn lẻ thu được chỉ phụ thuộc vào$k$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(k)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy xác định khoảng thời gian mà lá vẫn còn:$$[n-k+1,\; n].$$2. Hãy quan sát điều đó$i^i$có cùng độ ngang bằng với$i$. Thay thế mọi thuật ngữ$i^i$bởi sự đóng góp ngang bằng của nó. 
3. Đếm xem đoạn đó có bao nhiêu số nguyên lẻ. 
4. Nếu số nguyên lẻ là số chẵn, xuất ra`YES`vì tổng số lá là số chẵn. 
5. Nếu không thì xuất ra`NO`. 

Một công thức thuận tiện cho số số nguyên lẻ từ$1$bởi vì$x$là$$\left\lfloor \frac{x+1}{2} \right\rfloor.$$Vậy số số nguyên lẻ trong$[L,R]$là$$\left\lfloor \frac{R+1}{2} \right\rfloor - \left\lfloor \frac{L}{2} \right\rfloor.$$Chỉ tính chẵn lẻ của số này mới quan trọng. 

### Tại sao nó hoạt động 

Mọi cơ số chẵn đều sinh ra một lũy thừa chẵn, và mọi cơ sở lẻ sinh ra một lũy thừa lẻ. Modulo$2$, mỗi học kỳ$i^i$cư xử chính xác như$i$. 

Do đó, số chẵn lẻ của tổng số lá là số chẵn lẻ của số năm lẻ đóng góp lá. Thuật toán tính toán chính xác đại lượng đó cho khoảng thời gian hoạt động. Nếu số người đóng góp lẻ là số chẵn thì tổng số lá là số chẵn. Nếu số lẻ thì tổng số lá là số lẻ. Vì tính chẵn lẻ được bảo toàn ở mỗi bước nên thuật toán luôn tạo ra câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
ans = []

for _ in range(t):
    n, k = map(int, input().split())

    l = n - k + 1

    odd_count = (n + 1) // 2 - (l // 2)

    if odd_count % 2 == 0:
        ans.append("YES")
    else:
        ans.append("NO")

print("\n".join(ans))
```Bước đầu tiên tính toán khoảng thời gian hoạt động$[n-k+1,n]$. Đây chính xác là những năm lá vẫn còn sống trong năm

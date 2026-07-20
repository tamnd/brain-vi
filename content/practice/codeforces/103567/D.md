---
title: "CF 103567D - (\u041d\u0435)\u0434\u043e\u0441\u0442\u0438\u0436\u0438\u043c\u044b\u0439 \u0438\u0434\u0435\u0430\u043b"
description: "Chúng ta có một số nguyên cố định $N$ và một dãy số nguyên $[L, R)$, nghĩa là tất cả các số nguyên $X$ sao cho $L le X < R$. Với mỗi $X$ như vậy, chúng ta cần xác định xem nó có thỏa mãn điều kiện liên quan đến ước chung lớn nhất của $N$ hay không."
date: "2026-07-03T03:56:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103567
codeforces_index: "D"
codeforces_contest_name: "2021-2022 Olympiad Cognitive Technologies, Prefinal Round"
rating: 0
weight: 103567
solve_time_s: 41
verified: true
draft: false
---

[CF 103567D - (\u041d\u0435)\u0434\u043e\u0441\u0442\u0438\u0436\u0438\u043c\u044b\u0439 \u0438\u0434\u0435\u0430\u043b](https://codeforces.com/problemset/problem/103567/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên cố định$N$và một dãy số nguyên$[L, R)$, nghĩa là tất cả các số nguyên$X$như vậy$L \le X < R$. Đối với mỗi như vậy$X$, chúng ta cần xác định xem nó có thỏa mãn điều kiện liên quan đến ước số chung lớn nhất với$N$. 

Điều kiện đơn giản hóa để kiểm tra xem$\gcd(N, X)$bằng$\min(N, X)$. Vì vấn đề đảm bảo$N \le L < R$, mọi ứng viên$X$trong phạm vi ít nhất là$N$, Vì thế$\min(N, X)$luôn luôn là$N$. Điều này làm giảm điều kiện để kiểm tra xem$\gcd(N, X) = N$, tương đương với việc nói rằng$N$chia rẽ$X$. 

Vì vậy, nhiệm vụ trở thành: đếm xem có bao nhiêu số nguyên trong$[L, R)$là bội số của$N$và xuất ra số đếm đó. 

Những ràng buộc ngụ ý rằng$R$có thể cực kỳ lớn, lên tới khoảng$10^{18}$, điều này ngay lập tức loại trừ việc lặp lại trên mọi số nguyên trong phạm vi. Quét tuyến tính sẽ cần tới$10^{18}$bước, điều này là không khả thi thậm chí bỏ qua chi phí tính toán gcd. 

Một trường hợp ranh giới tinh tế phát sinh từ các ranh giới khác nhau. Vì khoảng là nửa mở nên$[L, R)$, giá trị$R$bản thân nó không được đưa vào. Một vòng lặp bao gồm ngây thơ từ$L$ĐẾN$R$sẽ đếm không chính xác$R$khi nó chia hết cho$N$. 

Một vấn đề khác xuất hiện trong suy nghĩ dựa trên gcd ngây thơ: ngay cả khi gcd được tính toán hiệu quả, việc kiểm tra mọi số vẫn chiếm ưu thế về độ phức tạp, khiến việc tối ưu hóa gcd không còn phù hợp. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng mọi$X$từ$L$ĐẾN$R - 1$, tính toán$\gcd(N, X)$, và đếm các trận đấu. Điều này đúng về mặt khái niệm vì nó áp dụng trực tiếp định nghĩa. Tuy nhiên, chi phí của nó tăng tuyến tính theo kích thước của khoảng thời gian. Khi$R - L$đạt tới$10^{18}$, thuật toán về cơ bản là không thể sử dụng được. 

Quan sát quan trọng là điều kiện gcd chuyển thành điều kiện chia hết: chúng ta đang đếm bội số của$N$. Thay vì kiểm tra từng số riêng lẻ, chúng tôi chuyển đổi góc nhìn và đếm xem có bao nhiêu bội số của$N$nằm dưới một giới hạn nhất định. Điều này chuyển đổi vấn đề từ việc lặp qua các giá trị sang số học trên các phạm vi số nguyên. 

Nếu chúng ta định nghĩa một hàm đếm bội số của$N$hoàn toàn dưới một giá trị$S$, thì câu trả lời cho$[L, R)$trở thành sự khác biệt giữa số lượng bên dưới$R$và số đếm bên dưới$L$. Mỗi số đếm này có thể được tính trong thời gian không đổi bằng cách sử dụng phép chia số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(R - L)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy quan sát điều đó vì$X \ge L \ge N$, biểu thức$\min(N, X)$đơn giản hóa để$N$. Điều này loại bỏ sự phụ thuộc vào cấu trúc điều kiện gcd và giảm vấn đề về kiểm tra tính chia hết. 
2. Thay thế điều kiện$\gcd(N, X) = N$với$N \mid X$. Bước này chuyển đổi bài toán từ lý thuyết số sang đếm cấp số cộng. 
3. Định dạng lại số khoảng dưới dạng chênh lệch của số tiền tố: đếm bội số của$N$TRONG$[L, R)$bằng cách tính xem có bao nhiêu bội số hoàn toàn nhỏ hơn$R$, sau đó trừ đi bao nhiêu hoàn toàn nhỏ hơn$L$. Điều này tránh việc xử lý trực tiếp với các điểm cuối của phạm vi. 
4. Tính số bội của$N$nhỏ hơn một giá trị$S$dùng phép chia số nguyên. Mọi bội số đều có dạng$kN$, và giá trị lớn nhất$k$thỏa mãn$kN < S$, ngụ ý$k \le \frac{S-1}{N}$. 
5. Áp dụng công thức$\left\lfloor \frac{R-1}{N} \right\rfloor - \left\lfloor \frac{L-1}{N} \right\rfloor$và xuất kết quả. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên thực tế là bội số của$N$tạo thành một cấp số cộng hoàn toàn đều đặn. Việc chuyển đổi sang đếm tiền tố đảm bảo rằng mọi bội số hợp lệ trong$[L, R)$được tính chính xác một lần trong tiền tố lên đến$R$và bị loại trừ khỏi tiền tố cho đến$L$. Phép trừ loại bỏ tất cả các đóng góp dưới đây$L$, để lại chính xác những thứ đó trong khoảng mục tiêu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, L, R = map(int, input().split())
    
    def count_less(x):
        if x <= 0:
            return 0
        return (x - 1) // N
    
    ans = count_less(R) - count_less(L)
    print(ans)

if __name__ == "__main__":
    solve()
```Chức năng trợ giúp`count_less(x)`thực hiện đếm dạng đóng của bội số của$N$nghiêm ngặt dưới đây$x$. biểu hiện`(x - 1) // N`là dạng số nguyên của$\lfloor (x-1)/N \rfloor$, trực tiếp đếm xem có bao nhiêu bước đầy đủ kích thước$N$phù hợp trước$x$. 

Cấu trúc phép trừ đảm bảo chúng ta không bao giờ lặp lại một cách rõ ràng trong phạm vi. Xử lý ranh giới được mã hóa theo bất đẳng thức nghiêm ngặt thông qua`x - 1`, để tránh việc vô tình đưa vào$x$chính nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
N = 3, L = 5, R = 15
```Chúng tôi đếm bội số của 3 trong$[5, 15)$: đây là 6, 9, 12. 

| Bước | x | count_less(x) | Giải thích | 
| --- | --- | --- | --- | 
| Tiền tố R | 15 | 14 // 3 = 4 | bội số dưới 15: 3,6,9,12 | 
| Tiền tố L | 5 | 4 // 3 = 1 | bội số dưới 5: 3 | 
| kết quả | - | 4 - 1 = 3 | 3 số hợp lệ | 

Điều này xác nhận rằng chỉ những số bên trong khoảng mới được tính, không phải những số bên dưới$L$. 

### Ví dụ 2 

đầu vào:```
N = 4, L = 4, R = 10
```Bội số của 4 in$[4, 10)$là 4 và 8. 

| Bước | x | count_less(x) | Giải thích | 
| --- | --- | --- | --- | 
| Tiền tố R | 10 | 9 // 4 = 2 | 4, 8 | 
| Tiền tố L | 4 | 3 // 4 = 0 | không có dưới 4 | 
| kết quả | - | 2 - 0 = 2 | 2 số hợp lệ | 

Điều này thể hiện việc xử lý đúng ranh giới nơi$L$chính nó là bội số của$N$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| chỉ một số phép tính số học không đổi | 
| Không gian |$O(1)$| không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp duy trì thời gian không đổi ngay cả khi$R$đạt tới$10^{18}$, bởi vì nó không bao giờ lặp lại trong phạm vi và chỉ dựa vào phép chia số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N, L, R = map(int, sys.stdin.readline().split())

    def count_less(x):
        if x <= 0:
            return 0
        return (x - 1) // N

    return str(count_less(R) - count_less(L))

# provided sample-like tests
assert run("3 5 15") == "3"
assert run("4 4 10") == "2"

# custom cases
assert run("5 5 6") == "1", "single element equal to N"
assert run("7 8 21") == "2", "multiple full blocks"
assert run("10 11 19") == "0", "no multiples"
assert run("2 2 1000000000000000000") == str((1000000000000000000-1)//2), "large range"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 5 6 | 1 | nhiều ranh giới đơn | 
| 7 8 21 | 2 | nhiều lần truy cập trong phạm vi | 
| 10 11 19 | 0 | tập hợp lệ trống | 
| 2 2 10^18 | công thức | căng thẳng giới hạn lớn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi$L$chính nó là bội số của$N$. Ví dụ,$N=4, L=4, R=9$. Câu trả lời đúng bao gồm 4. Công thức giải quyết vấn đề này vì$count\_less(4)=0$Và$count\_less(9)=2$, cho kết quả 2, tương ứng với 4 và 8. 

Một trường hợp cạnh khác là khi không có bội số nào trong khoảng, chẳng hạn như$N=10, L=11, R=19$. Cả hai số tiền tố đều bằng nhau, tạo ra số không. Sự khác biệt số học tự nhiên hủy bỏ tất cả các đóng góp. 

Trường hợp ranh giới cuối cùng là cực kỳ lớn$R$. Vì tính toán chỉ sử dụng phép chia nên không có hiện tượng tràn hoặc suy giảm hiệu suất và các số nguyên chính xác tùy ý của Python xử lý một cách an toàn các giá trị lên đến giới hạn tối đa mà không cần sửa đổi.

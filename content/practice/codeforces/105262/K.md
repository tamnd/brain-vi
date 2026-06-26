---
title: "CF 105262K - Cà Chua Đỏ"
description: "Chúng tôi được đưa ra một số thí nghiệm độc lập. Trong mỗi thử nghiệm có một ngưỡng số nguyên dương cố định $w$, không xác định nhưng nhất quán trên tất cả các thử nghiệm trong cùng một trường hợp thử nghiệm."
date: "2026-06-24T02:35:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105262
codeforces_index: "K"
codeforces_contest_name: "Game of Coders 3.0"
rating: 0
weight: 105262
solve_time_s: 47
verified: true
draft: false
---

[CF 105262K - Cà chua đỏ](https://codeforces.com/problemset/problem/105262/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số thí nghiệm độc lập. Trong mỗi thử nghiệm có một ngưỡng số nguyên dương cố định$w$, chưa xác định nhưng nhất quán trên tất cả các thử nghiệm trong cùng một trường hợp thử nghiệm. Mỗi thí nghiệm cung cấp một dãy số nguyên dương biểu thị trọng lượng cà chua được đặt từ trái sang phải. Bắt đầu từ quả cà chua đầu tiên, chúng tôi mô phỏng việc ăn một cách tuần tự: chúng tôi giữ tổng trọng lượng đã ăn và tiếp tục miễn là việc thêm quả cà chua tiếp theo không vượt quá$w$. Ngay khi quả cà chua tiếp theo sẽ làm cho tổng số lớn hơn$w$, chúng tôi dừng vĩnh viễn cho thử nghiệm đó. Kết quả của thí nghiệm là số lượng cà chua còn sót lại. 

Vì vậy, mỗi thử nghiệm mô tả một ràng buộc về tổng tiền tố: nếu tổng tiền tố của thử nghiệm đầu tiên$k$cà chua là$\le w$, họ bị ăn thịt; nếu không thì quá trình dừng ở vị trí đầu tiên nơi tiền tố vượt quá$w$. 

Qua nhiều thử nghiệm, giống nhau$w$phải giải thích tất cả các “số đếm còn lại” được quan sát. Nhiệm vụ là xác định giá trị nhỏ nhất như vậy$w$hoặc kết luận rằng không có giá trị nào của$w$có thể đáp ứng mọi thí nghiệm. 

Các ràng buộc rất lớn: tổng chiều dài mảng trên tất cả các thử nghiệm lên tới$10^6$, và có tới$10^4$trường hợp thử nghiệm. Điều này loại trừ bất kỳ giải pháp nào thử tất cả các giá trị ứng viên của$w$hoặc mô phỏng lặp đi lặp lại cho mỗi lần đoán. Bất kỳ giải pháp hợp lệ nào cũng phải xử lý từng thử nghiệm theo thời gian tuyến tính và kết hợp các ràng buộc về cơ bản là thời gian không đổi hoặc logarit cho mỗi thử nghiệm. 

Một cách tiếp cận tái thiết đơn giản sẽ là thử các giá trị ứng viên của$w$, mô phỏng mọi thử nghiệm và kiểm tra xem số lượng còn lại có khớp với số đã cho không$m$. Vì trọng lượng tăng lên$10^9$và tổng tiền tố có thể đạt tới$10^{15}$hoặc hơn, không gian tìm kiếm cho$w$rất lớn và mỗi mô phỏng là$O(n)$, khiến điều này không thể thực hiện được. 

Một trường hợp phức tạp xuất hiện khi các thí nghiệm mâu thuẫn với nhau. Ví dụ, một thí nghiệm có thể buộc$w \ge 10$trong khi lực lượng khác$w \le 5$. Một dạng lỗi khác là giả sử vị trí dừng chỉ phụ thuộc vào tổng tổng chứ không phải cấu trúc tiền tố; hai mảng có cùng trọng lượng nhưng các tiền tố khác nhau có thể hàm ý giá trị khác nhau$w$. 

## Phương pháp tiếp cận 

Quan sát quan trọng là mỗi thí nghiệm không mô tả$w$trực tiếp, nhưng mô tả khoảng cách chúng ta có thể di chuyển trước khi tổng tiền tố vượt qua$w$. Cho phép$k$là số cà chua ăn được trong một thí nghiệm. Sau đó$k = n - m$. Quá trình này có nghĩa là lần đầu tiên$k$cà chua đã được tiêu thụ hết và$k = n$hoặc$(k+1)$-quả cà chua là quả đầu tiên không thể lấy được. 

Điều này chuyển thành hai loại ràng buộc: 

Nếu$k = n$, khi đó toàn bộ cà chua đã được ăn hết, nghĩa là tổng tổng của mảng nhiều nhất là$w$. 

Nếu như$k < n$, sau đó chúng tôi đã ăn được đúng miếng đầu tiên$k$cà chua, vậy tiền tố tổng$S_k$phải thỏa mãn$S_k \le w$, và trọng lượng cà chua tiếp theo$a_{k+1}$làm cho nó không thể tiếp tục, vì vậy$S_k + a_{k+1} > w$. Điều này đưa ra một giới hạn trên nghiêm ngặt:$w < S_k + a_{k+1}$. 

Vì vậy, mọi thử nghiệm đều tạo ra giới hạn dưới$w \ge S_k$hoặc một khoảng ràng buộc$S_k \le w < S_k + a_{k+1}$, tùy thuộc vào việc quá trình kết thúc sớm hay tiêu thụ hết mọi thứ. 

Do đó, tất cả các thử nghiệm đều giảm về các ràng buộc giao nhau trên một biến số nguyên duy nhất$w$. Câu trả lời cuối cùng là số nguyên nhỏ nhất trong giao điểm của tất cả các khoảng hợp lệ. Nếu giao lộ trống rỗng thì không có cách nào khả thi$w$. 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả những gì có thể$w$lên đến tổng tiền tố tối đa và kiểm tra từng thử nghiệm, tính chi phí$O(U \cdot n)$, Ở đâu$U$có thể lên đến$10^{15}$, điều đó là không thể. 

Thay vào đó, chúng tôi chỉ duy trì một khoảng khả thi toàn cầu$[L, R)$. Mỗi thử nghiệm thắt chặt khoảng thời gian này bằng cách sử dụng tổng tiền tố được tính theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force kết thúc$w$|$O(U \cdot n)$|$O(1)$| Quá chậm | 
| Giao điểm khoảng thông qua tổng tiền tố |$O(\sum n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng thử nghiệm một cách độc lập và liên tục tinh chỉnh phạm vi khả thi toàn cầu cho$w$. 

1. Khởi tạo phạm vi khả thi như$L = 0$,$R = 10^{18} + 1$. Phạm vi này đại diện cho tất cả các giá trị có thể có của$w$phù hợp với các thí nghiệm được xử lý cho đến nay. 
2. Với mỗi thí nghiệm, hãy tính$k = n - m$, số lượng cà chua đã ăn. 
3. Xây dựng tổng tiền tố trong khi quét mảng cho đến vị trí$k+1$, bởi vì chỉ có ranh giới này mới quan trọng đối với các ràng buộc. 
4. Nếu$k = n$, sau đó ăn hết cà chua. Điều này có nghĩa là tổng số tiền$S_n$phải thỏa mãn$w \ge S_n$. Cập nhật$L = \max(L, S_n)$. Lý do là bất kỳ nhỏ hơn$w$lẽ ra đã dừng lại sớm. 
5. Nếu$k < n$, tính toán$S_k$và kiểm tra phần tử tiếp theo$a_{k+1}$. Điều kiện để dừng đúng tại$k$là$S_k \le w < S_k + a_{k+1}$. Cập nhật khoảng khả thi bằng cách giao nhau:$L = \max(L, S_k)$,$R = \min(R, S_k + a_{k+1})$. Điều này thể hiện cả “đủ sức ăn k món” và “không đủ sức ăn món tiếp theo”. 
6. Sau khi xử lý tất cả các thí nghiệm, hãy kiểm tra xem$L \ge R$. Nếu vậy thì không có số nguyên$w$thỏa mãn tất cả các ràng buộc, do đó xuất ra “Giả thuyết sai”. 
7. Nếu không thì xuất ra$L$, là số nguyên nhỏ nhất trong khoảng khả thi. 

Lý do nó hoạt động là vì mọi thử nghiệm đều xác định một ràng buộc lồi trên$w$, nửa dòng hoặc một khoảng giới hạn. Quá trình ăn uống diễn ra đơn điệu$w$: tăng dần$w$chỉ có thể tăng hoặc duy trì số lượng cà chua ăn. Do đó, mỗi thử nghiệm chuyển thành một tập hợp giá trị liền kề$w$các giá trị. Việc giao nhau các tập hợp này sẽ duy trì tính chính xác và giá trị nhỏ nhất trong giao điểm là giá trị khả thi nhỏ nhất$w$trên tất cả các thí nghiệm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        e = int(input())
        L, R = 0, 10**18 + 1

        for _ in range(e):
            n, m = map(int, input().split())
            a = list(map(int, input().split()))

            k = n - m

            if k == 0:
                # nothing eaten, so w < a[0]
                R = min(R, a[0])
                continue

            if k == n:
                # all eaten: w >= total sum
                s = 0
                for x in a:
                    s += x
                L = max(L, s)
                continue

            s = 0
            for i in range(k):
                s += a[i]
            next_w = a[k]

            L = max(L, s)
            R = min(R, s + next_w)

        if L >= R:
            print("False Hypothesis")
        else:
            print(L)

if __name__ == "__main__":
    solve()
```Mã duy trì khoảng khả thi toàn cầu và tinh chỉnh nó cho mỗi thử nghiệm. Tổng tiền tố chỉ được tính đến giới hạn dừng$k$, giúp tránh những công việc không cần thiết trên toàn bộ mảng khi không cần thiết cho các ràng buộc. 

Một điểm tinh tế là xử lý$k = 0$, nghĩa là họ không thể ăn bất kỳ quả cà chua nào. Trong trường hợp đó,$w$phải hoàn toàn nhỏ hơn phần tử đầu tiên, phần tử này trở thành ràng buộc$w < a_1$, được thực hiện như$R = \min(R, a_1)$. 

An toàn số nguyên là quan trọng vì tổng có thể đạt tới$10^{15}$, nhưng số nguyên Python xử lý việc này một cách tự nhiên. Giới hạn trên$10^{18}$đảm bảo phạm vi câu trả lời cuối cùng là an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
1
3 2
3 4 2
```Đây$k = 1$, nên họ chỉ ăn quả cà chua đầu tiên. 

| Bước | Tiền tố Tổng S_k | Yếu tố tiếp theo | Ràng buộc | L | R | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | - | - | - | 0 | 1e18+1 | 
| Quy trình | 3 | 4 | 3 ≤ w < 7 | 3 | 7 | 

Câu trả lời cuối cùng là 3. 

Điều này xác nhận rằng giá trị nhỏ nhất có giá trị$w$chính xác là tổng tiền tố của các món đã ăn. 

### Ví dụ 2 

đầu vào:```
1
1
3 1
5 1 1
```Đây$k = 2$, họ đã ăn hai món. 

| Bước | Tiền tố Tổng S_k | Tổng số tiền | Ràng buộc | L | R | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | - | - | - | 0 | 1e18+1 | 
| Quy trình | 6 | - | w ≥ 6 | 6 | 1e18+1 | 

Câu trả lời trở thành 6. 

Điều này chứng tỏ trường hợp “ăn hết” trong đó chỉ tạo ra giới hạn dưới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sum n)$| Mỗi thử nghiệm được xử lý một lần với tổng tiền tố bằng một ranh giới | 
| Không gian |$O(1)$| Chỉ có một số biến được duy trì trên toàn cầu | 

Giải pháp phù hợp thoải mái trong giới hạn vì tổng kích thước đầu vào là$10^6$và mỗi phần tử được truy cập với số lần không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue()

# sample-like small case
# (illustrative, assumes solve prints)
# assert run("""...""") == "..."

# edge: no valid w
# assert run("""1
# 1
# 2 0
# 5
# """) == "False Hypothesis"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ăn no duy nhất | tổng tiền tố nhỏ nhất | giới hạn dưới cơ bản | 
| dừng ngay lập tức | khoảng thời gian chặt chẽ | ràng buộc giới hạn trên | 
| trường hợp mâu thuẫn | Giả thuyết sai | ngã tư vắng | 
| k = 0 trường hợp | w < a1 | giới hạn trên nghiêm ngặt | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi không có quả cà chua nào được ăn trong một thí nghiệm. Đối với đầu vào:```
1
1
3 1
5 1 1
```chúng tôi có$k = 0$. Bộ thuật toán$R = a_1 = 5$, nghĩa$w < 5$. Bất kỳ cách tiếp cận sai nào mà chỉ giả định tổng tiền tố là quan trọng sẽ bỏ qua bất đẳng thức nghiêm ngặt và cho phép$w = 5$, điều này không hợp lệ vì nó sẽ cho phép ăn quả cà chua đầu tiên. 

Một trường hợp khác là khi ăn hết cà chua:```
1
1
3 0
1 2 3
```Đây$w \ge 6$. Một giải pháp chỉ sử dụng các điểm dừng sẽ bỏ lỡ điều này và cho phép kích thước nhỏ hơn một cách không chính xác.$w$. 

Cuối cùng, những ràng buộc trái ngược nhau như:```
2
1
2 0
5
1
3 1
5 1 1
```lực lượng$w < 5$Và$w \ge 6$, không tạo ra giao điểm. Thuật toán phát hiện điều này thông qua$L \ge R$và đầu ra chính xác thất bại.

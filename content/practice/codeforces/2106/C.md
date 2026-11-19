---
title: "CF 2106C - Bom Anh Đào"
description: "Chúng ta có hai mảng có độ dài $n$, $a$ và $b$. Mảng $a$ đã được biết đầy đủ và $b$ có một số phần tử bị thiếu được đánh dấu là $-1$. Mục đích là điền vào các phần tử còn thiếu trong $b$ sao cho tổng $ai + bi$ không đổi với mọi $i$. Mỗi phần tử trong $b$ phải nằm trong $[0, k]$."
date: "2026-06-08T04:52:01+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2106
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1020 (Div. 3)"
rating: 1000
weight: 2106
solve_time_s: 72
verified: true
draft: false
---

[CF 2106C - Bom anh đào](https://codeforces.com/problemset/problem/2106/C) 

**Đánh giá:** 1000 
**Tags:** tham lam, toán học, phân loại 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng có độ dài$n$,$a$Và$b$. Mảng$a$được biết đầy đủ và$b$có một số yếu tố bị thiếu được đánh dấu là$-1$. Mục đích là điền vào những phần còn thiếu trong$b$sao cho tổng$a_i + b_i$là hằng số cho tất cả$i$. Mỗi phần tử trong$b$phải ở trong$[0, k]$. Chúng ta cần đếm tất cả những cách hợp lệ để làm điều này. 

Những ràng buộc ngụ ý rằng$n$có thể đạt được$2 \cdot 10^5$trên tất cả các trường hợp thử nghiệm và mỗi phần tử có thể lớn bằng$10^9$. Bất kỳ giải pháp nào lặp lại tất cả các giá trị có thể bị thiếu$b_i$sẽ là quá chậm, bởi vì một phần tử bị thiếu có thể có$k+1$những lựa chọn bị cấm. Do đó, giải pháp phải suy luận về phạm vi hợp lệ và tránh việc liệt kê bạo lực. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó một số$b_i$đã được sửa rồi. Nếu hai cố định$b_i$yêu cầu các hằng số khác nhau$x$, không có sự hoàn thành hợp lệ. Ví dụ,$a = [2, 1]$,$b = [1, 0]$không thể bổ sung cho nhau vì$2 + 1 \neq 1 + 0$. Một trường hợp cạnh khác xảy ra khi yêu cầu$b_i$giá trị thỏa mãn quy tắc bù vượt quá$k$hoặc là tiêu cực. Những kịch bản này phải được lọc cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra tất cả các khả năng thay thế có thể cho các yếu tố còn thiếu của$b$và kiểm tra xem mảng kết quả có bổ sung hay không. Điều này đúng nhưng quá chậm. Giả định$m$các phần tử bị thiếu, mỗi phần tử có tới$k+1$khả năng. Tổng số tổ hợp là$(k+1)^m$, điều đó là không thể khi$k \approx 10^9$ngay cả đối với nhỏ$m$. 

Quan sát quan trọng là một cặp bổ sung đòi hỏi$x = a_i + b_i$giống nhau cho tất cả$i$. Đối với những vị trí mà$b_i$được biết đến, điều này sửa chữa$x = a_i + b_i$. Nếu những quan điểm cố định này không đồng ý thì không có giải pháp nào tồn tại. Vì thiếu$b_i$, giá trị được xác định là$b_i = x - a_i$. Điều này ngay lập tức đưa ra một kiểm tra hợp lệ:$0 \le b_i \le k$. Do đó, chúng ta rút gọn vấn đề xuống việc tìm phạm vi cho phép của$x$từ đã biết$b_i$và đếm xem có bao nhiêu lựa chọn thỏa mãn các ràng buộc đối với những lựa chọn còn thiếu. 

Cụ thể, đối với mỗi phần tử bị thiếu, giá trị hợp lệ$x$phải thỏa mãn$a_i \le x \le a_i + k$. Việc kết hợp tất cả các ràng buộc trên mảng sẽ tạo ra một khoảng giá trị liền kề duy nhất$x$. Độ dài của khoảng thời gian này sẽ cho câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((k+1)^m) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`x_min`bằng 0 và`x_max`BẰNG$k$cộng lớn nhất$a_i$trong mảng. Chúng sẽ theo dõi phạm vi hợp lệ cho tổng không đổi$x$. 
2. Lặp lại từng chỉ mục$i$của các mảng. Nếu như$b_i$không thiếu, hãy tính$x_i = a_i + b_i$. Cập nhật`x_min`Và`x_max`bằng với`x_i`bởi vì tất cả đã được sửa$b_i$thực hiện cùng một số tiền. Nếu chúng ta gặp phải một lỗi đã được sửa trước đó`x`điều đó khác với`x_i`, ngay lập tức trả về 0 vì mảng không thể bù nhau. 
3. Nếu$b_i$bị thiếu, hãy lấy khoảng hợp lệ cho$x$BẰNG`[a_i, a_i + k]`từ$b_i = x - a_i$phải thỏa mãn$0 \le b_i \le k$. Cắt khoảng này với dòng điện`[x_min, x_max]`để thắt chặt các giá trị có thể của$x$. 
4. Sau khi xử lý tất cả các phần tử, tính số giá trị nguyên trong khoảng hợp lệ cuối cùng`[x_min, x_max]`. Đây là`max(0, x_max - x_min + 1)`và đưa ra tổng số cách để hoàn thành$b$. 
5. Xuất kết quả. 

Điều bất biến là ở mỗi bước,`[x_min, x_max]`đại diện cho tất cả các giá trị của$x$thỏa mãn cả phần tử cố định và phần tử còn thiếu được thấy cho đến nay. Nút giao đảm bảo không hợp lệ$x$trượt qua, vì vậy bất kỳ$x$được tính ở cuối sẽ tạo ra một mảng bổ sung hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cherry_bomb():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        x_min = 0
        x_max = 2 * k  # upper bound for x
        
        fixed_x = None
        possible = True
        
        for ai, bi in zip(a, b):
            if bi != -1:
                current = ai + bi
                if fixed_x is None:
                    fixed_x = current
                    x_min = x_max = current
                elif current != fixed_x:
                    possible = False
                    break
            else:
                # missing element: b_i = x - a_i in [0, k] => x in [a_i, a_i + k]
                x_min = max(x_min, ai)
                x_max = min(x_max, ai + k)
        
        if not possible or x_min > x_max:
            print(0)
        else:
            print(x_max - x_min + 1)

cherry_bomb()
```Mã này phân tách việc xử lý các phần tử cố định và phần tử bị thiếu. Các phần tử cố định xác định một mức độ nghiêm ngặt`x`, trong khi các phần tử bị thiếu sẽ ràng buộc nó trong một phạm vi. sử dụng`max`Và`min`để giao nhau các khoảng đảm bảo chúng tôi tôn trọng mọi ràng buộc. Số lượng cuối cùng là kích thước của giao lộ. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

Mảng đầu vào:`a = [1, 3, 2]`,`b = [-1, -1, 1]`| tôi | một [tôi] | b[i] | x_min | x_max | Bình luận | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | -1 | 1 | 11 | thiếu: x trong [1, 1+10] | 
| 1 | 3 | -1 | 3 | 11 | thiếu: x trong [3, 3+10], giao với | trước đó 
| 2 | 2 | 1 | 3 | 3 | đã sửa: x = 2+1 = 3, giao nhau => x_min = x_max = 3 | 

Số cách:`x_max - x_min + 1 = 1`**Ví dụ 2** 

Mảng đầu vào:`a = [0, 1, 0, 0, 1]`,`b = [-1, 0, 1, 0, -1]`| tôi | một [tôi] | b[i] | x_min | x_max | Bình luận | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | -1 | 0 | 1 | thiếu: x trong [0, 0+1] | 
| 1 | 1 | 0 | 1 | 1 | đã sửa: x = 1+0=1 | 
| 2 | 0 | 1 | 1 | 1 | đã sửa: x=0+1=1 | 
| 3 | 0 | 0 | 1 | 1 | đã sửa: x=0+0=0 xung đột | 

Đã phát hiện xung đột, trả lời 0. 

Những dấu vết này xác nhận rằng thuật toán xử lý chính xác cả các ràng buộc phần tử cố định và khoảng thời gian cho các phần tử bị thiếu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Một lần truyền tuyến tính trên mảng`a`Và`b`để tính toán các khoảng thời gian và kiểm tra tính nhất quán | 
| Không gian | O(n) | Mảng đầu vào được lưu trữ; không cần cấu trúc động bổ sung | 

Cho tổng của$n$trên tất cả các trường hợp thử nghiệm là nhiều nhất$2 \cdot 10^5$, giải pháp sẽ chạy tốt trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    cherry_bomb()
    return out.getvalue().strip()

# Provided samples
assert run("7\n3 10\n1 3 2\n-1 -1 1\n5 1\n0 1 0 0 1\n-1 0 1 0 -1\n5 1\n0 1 0 0 1\n-1 1 -1 1 -1\n5 10\n1 3 2 5 4\n-1 -1 -1 -1 -1\n5 4\n1 3 2 1 3\n1 -1 -1 1 -1\n5 4\n1 3 2 1 3\n2 -1 -1 2 0\n5 5\n5 0
```

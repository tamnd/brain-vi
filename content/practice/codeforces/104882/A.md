---
title: "CF 104882A - A+B?"
description: "Chúng ta đang đếm các cặp số nguyên $(a, b)$ thỏa mãn một ràng buộc tuyến tính đơn giản: tổng của chúng được cố định bằng một giá trị cho trước $n$, và cả hai số phải nằm trong một khoảng đối xứng có tâm ở mức 0."
date: "2026-06-28T09:17:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104882
codeforces_index: "A"
codeforces_contest_name: "Voronezh State University - Sitronics contest II"
rating: 0
weight: 104882
solve_time_s: 37
verified: true
draft: false
---

[CF 104882A - A+B?](https://codeforces.com/problemset/problem/104882/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang đếm các cặp số nguyên$(a, b)$thỏa mãn một ràng buộc tuyến tính đơn giản: tổng của chúng được cố định bằng một giá trị nhất định$n$và cả hai số phải nằm trong một khoảng đối xứng có tâm là 0. Khoảng thời gian được xác định bởi$p$: cả hai$a$Và$b$phải ở giữa$-10^p$Và$10^p$, bao gồm. 

Vì vậy, nhiệm vụ không phải là tìm kiếm các cặp trong hai chiều một cách độc lập. Một lần$a$được chọn,$b$buộc phải là$n - a$. Câu hỏi duy nhất là có bao nhiêu giá trị nguyên của$a$giữ cả hai$a$Và$n-a$nằm trong phạm vi cho phép. 

Đầu vào chỉ là hai số nguyên, một số xác định tổng mục tiêu và một số xác định độ lớn của các giá trị được phép. Đầu ra là số lựa chọn số nguyên hợp lệ của$a$, tương ứng trực tiếp với các cặp hợp lệ. 

Các ràng buộc rất nhỏ về độ phức tạp của cấu trúc:$p \le 9$, vậy giới hạn lớn nhất là$10^9$. Điều đó có nghĩa là phạm vi hợp lệ luôn là khoảng kích thước liền kề nhiều nhất$2 \cdot 10^9 + 1$, do đó, bất kỳ giải pháp nào liệt kê tất cả các giá trị có thể có của$a$đã ở mức giới hạn nhưng vẫn khả thi trong trường hợp xấu nhất, mặc dù không cần thiết. 

Một sai lầm ngây thơ xuất hiện khi một người cố gắng độc lập lựa chọn$a$Và$b$và sau đó lọc theo tổng, cấu trúc đếm kép thực sự là một chiều. Một lỗi phổ biến khác là quên rằng cả hai giới hạn phải được thực thi đồng thời trên cả hai biến. 

Tình huống cạnh cụ thể xảy ra khi khoảng quá chặt đến mức không có giải pháp nào tồn tại. Ví dụ, nếu$n = 25$Và$p = 1$, phạm vi là$[-10, 10]$. Không có hai số nào trong phạm vi đó có tổng bằng 25, vì vậy câu trả lời phải là 0. Bất kỳ phương pháp nào chỉ kiểm tra một bên của ràng buộc sẽ báo cáo sai số lượng dương. 

## Phương pháp tiếp cận 

Cách giải thích brute-force rất đơn giản: lặp lại tất cả các số nguyên có thể$a$trong phạm vi cho phép, tính$b = n - a$, và kiểm tra xem$b$cũng nằm trong phạm vi tương tự. Mỗi hợp lệ$a$đóng góp một cặp hợp lệ. Điều này đúng vì mỗi cặp hợp lệ đều tương ứng với chính xác một lựa chọn$a$. 

Vấn đề với cách tiếp cận này là kích thước của không gian tìm kiếm. Phạm vi của$a$có chiều dài$2 \cdot 10^p + 1$, cái nào cho$p = 9$là về$2 \cdot 10^9$. Điều đó vượt xa những gì có thể lặp lại trong vòng một giây. 

Quan sát quan trọng là ràng buộc giảm xuống để giao nhau giữa hai khoảng. Từ$a \in [-10^p, 10^p]$, chúng tôi cũng cần$n - a \in [-10^p, 10^p]$, sắp xếp lại thành một ràng buộc khoảng khác trên$a$:$a \in [n - 10^p, n + 10^p]$. Rất hợp lệ$a$các giá trị nằm ở giao điểm của hai khoảng:$$[-10^p, 10^p] \cap [n - 10^p, n + 10^p]$$Khi bài toán trở thành giao điểm khoảng, câu trả lời chỉ là độ dài của giao điểm đó theo số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(10^p)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính giới hạn$M = 10^p$. Điều này xác định phạm vi được phép cho cả hai biến. Toàn bộ vấn đề bị hạn chế bởi giá trị duy nhất này. 
2. Chuyển đổi điều kiện$a \in [-M, M]$trực tiếp vào khoảng đầu tiên$I_1 = [-M, M]$. 
3. Viết lại điều kiện$b = n - a \in [-M, M]$vào một hạn chế về$a$. 

Điều này trở thành:$$-M \le n - a \le M$$tương đương với:$$n - M \le a \le n + M$$đưa ra khoảng thời gian thứ hai$I_2 = [n - M, n + M]$. 
4. Tính giao điểm của hai khoảng này. Ranh giới bên trái là:$$L = \max(-M, n - M)$$và ranh giới bên phải là:$$R = \min(M, n + M)$$5. Nếu$L > R$, các khoảng không trùng nhau và không có giá trị$a$tồn tại, vì vậy câu trả lời là 0. 
6. Ngược lại, số giá trị nguyên trong giao điểm là$R - L + 1$, đó là câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Mỗi cặp hợp lệ$(a, b)$được xác định duy nhất bởi$a$, và các ràng buộc trên$a$chính xác là yêu cầu mà cả hai$a$Và$n-a$nằm trong một khoảng cố định. Điều này chuyển thành hai ràng buộc khoảng độc lập trên$a$. Giao điểm của chúng nắm bắt chính xác tập hợp khả thi mà không bị tính quá mức hoặc thiếu sót. Việc tính toán làm giảm hệ thống ràng buộc hai biến ban đầu thành một bài toán đếm khoảng duy nhất, bảo toàn tất cả các giải pháp hợp lệ và loại trừ tất cả các giải pháp không hợp lệ bằng cách xây dựng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, p = map(int, input().split())
    M = 10 ** p

    L = max(-M, n - M)
    R = min(M, n + M)

    if L > R:
        print(0)
    else:
        print(R - L + 1)

if __name__ == "__main__":
    solve()
```Giải pháp tính toán phạm vi tối đa cho phép đối với$a$theo cả hai ràng buộc và đếm trực tiếp có bao nhiêu số nguyên nằm trong phần chồng lấp đó. Chi tiết triển khai quan trọng là tính toán đối xứng cả hai giới hạn và áp dụng giao điểm tối đa tối thiểu đơn giản. Rủi ro từng cái một xuất hiện trong lần đếm cuối cùng; bản chất bao gồm của các khoảng nguyên đòi hỏi phải thêm 1 vào$R - L$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Đây$M = 10$. Các hạn chế là:$a \in [-10, 10]$Và$a \in [7 - 10, 7 + 10] = [-3, 17]$. 

| Bước | Ứng cử viên L | Ứng viên R | Khoảng thời gian | 
| --- | --- | --- | --- | 
| I1 | -10 | 10 | [-10, 10] | 
| I2 | -3 | 17 | [-3, 17] | 
| Giao lộ | -3 | 10 | [-3, 10] | 

Số số nguyên là$10 - (-3) + 1 = 14$. 

Điều này xác nhận thuật toán chỉ đếm chính xác các giá trị của$a$giữ cả hai biến trong phạm vi. 

### Ví dụ 2 

đầu vào:```

```Ở đây (M

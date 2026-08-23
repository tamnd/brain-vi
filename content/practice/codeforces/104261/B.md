---
title: "CF 104261B - Khám phá Sao Diêm Vương!"
description: "Chúng ta được cho một số nguyên $n$ và chúng ta cần đánh giá một tổng cụ thể được xây dựng từ số dư của phép chia. Với mọi số nguyên $i$ từ 1 đến $n$, chúng ta chia $n$ cho $i$ và lấy số dư, sau đó cộng tất cả các số dư đó lại với nhau. Nhiệm vụ là tính toán tổng số này một cách hiệu quả."
date: "2026-07-01T21:40:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104261
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 03-24-23 Div. 2 (Beginner)"
rating: 0
weight: 104261
solve_time_s: 71
verified: true
draft: false
---

[CF 104261B - Khám phá Sao Diêm Vương!](https://codeforces.com/problemset/problem/104261/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên duy nhất$n$và chúng ta cần đánh giá một phép tính tổng cụ thể được xây dựng từ số dư của phép chia. Với mọi số nguyên$i$từ 1 đến$n$, chúng tôi chia$n$qua$i$và lấy số dư rồi cộng tất cả số dư đó lại với nhau. Nhiệm vụ là tính toán tổng số này một cách hiệu quả. 

Về mặt hình thức, kết quả cần tìm là$$\sum_{i=1}^{n} (n \bmod i)$$Giới hạn kích thước đầu vào$n \le 10^5$đã loại trừ bất kỳ giải pháp nào tính toán lại mô-đun bên trong vòng lặp lồng nhau trên phạm vi lớn cho mỗi trường hợp thử nghiệm. Việc đánh giá trực tiếp thực hiện$n$hoạt động modulo, đó là$O(n)$, và điều đó đã được chấp nhận ở đây. Bất cứ điều gì tệ hơn tuyến tính, chẳng hạn như cố gắng mô phỏng hành vi chia hoặc tính toán lại phần dư thông qua phép trừ lặp đi lặp lại, sẽ thất bại trong giới hạn thời gian nếu được mở rộng ra ngoài cấu trúc vòng lặp đơn này. 

Điểm tinh tế chính là mặc dù công thức trông giống như một vòng lặp đơn giản, nhưng nhiều cách giải thích ngây thơ lại cố gắng mở rộng hoặc đơn giản hóa nó một cách không chính xác, đặc biệt bằng cách cố gắng nhóm các thuật ngữ mà không hiểu cách thức thực hiện.$n \bmod i$ứng xử trong phạm vi khác nhau của$i$. 

Vỏ ngoài có kích thước tối thiểu nhưng vẫn đáng để kiểm tra. 

Vì$n = 1$, biểu thức trở thành$1 \bmod 1 = 0$, vì vậy câu trả lời là 0. Một lỗi phổ biến là cho rằng phần dư luôn dương hoặc bắt đầu vòng lặp từ 2 do nhầm lẫn, điều này sẽ không tạo ra kết quả đầu ra. 

Đối với lớn hơn$n$, các giá trị còn lại thay đổi đáng kể: đối với các ước số nhỏ$i$, phần còn lại là$n - i \cdot \lfloor n/i \rfloor$, trong khi đối với$i > n/2$, thương số là 1 và phần còn lại đơn giản hóa thành$n - i$. Một nỗ lực bất cẩn để tối ưu hóa mà không tôn trọng cấu trúc này có thể tính hai lần hoặc xử lý sai các chuyển tiếp. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu tuân theo định nghĩa. Đối với mỗi$i$từ 1 đến$n$, tính toán$n \bmod i$và tích lũy kết quả. Điều này đúng vì nó đánh giá định nghĩa theo nghĩa đen mà không cần chuyển đổi. Thời gian chạy là tuyến tính trong$n$, với chính xác$n$các phép toán modulo và phép cộng. 

Từ$n \le 10^5$, giải pháp mạnh mẽ này đã phù hợp một cách thoải mái trong giới hạn thời gian. Không cần đến các kỹ thuật tối ưu hóa nâng cao như nhóm ước số hoặc thủ thuật chuỗi hài, vì bài toán không yêu cầu xử lý nhiều trường hợp thử nghiệm hoặc các ràng buộc lớn hơn. 

Cái nhìn sâu sắc quan trọng chỉ đơn giản là nhận ra rằng vấn đề đã ở dạng tính toán đơn giản nhất. Biểu thức không ẩn cấu trúc lặp lại cần nén và bản thân hoạt động mô đun là thời gian không đổi trong Python. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$từ đầu vào. Đây là giới hạn trên của phạm vi tổng. 
2. Khởi tạo biến tích lũy`ans`về 0. Điều này sẽ lưu trữ tổng số còn lại. 
3. Lặp lại$i$từ 1 đến$n$, bao gồm. Mỗi giá trị của$i$đại diện cho một ứng cử viên chia trong biểu thức$n \bmod i$. 
4. Đối với mỗi$i$, tính số dư của phép chia$n$qua$i$, sau đó thêm nó vào`ans`. Điều này trực tiếp phù hợp với định nghĩa toán học của vấn đề. 
5. Sau khi vòng lặp hoàn tất, xuất ra`ans`. 

### Tại sao nó hoạt động 

Mỗi số hạng trong tổng là độc lập và được xác định hoàn toàn bởi cặp$(n, i)$. Thuật toán đánh giá mọi giá trị hợp lệ$i$chính xác một lần và mỗi lần đóng góp$n \bmod i$được tính toán chính xác như đã xác định. Vì phép cộng có tính chất kết hợp và giao hoán trên các số nguyên nên việc tích lũy các giá trị này theo bất kỳ thứ tự nào sẽ tạo ra kết quả giống như phép tính tổng. Không có phép tính gần đúng hoặc phép biến đổi nào được đưa ra, do đó tính chính xác được tính trực tiếp từ việc đánh giá theo từng thuật ngữ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
ans = 0

for i in range(1, n + 1):
    ans += n % i

print(ans)
```Giải pháp đọc đầu vào một lần và duy trì một bộ tích lũy duy nhất. Vòng lặp chạy từ 1 đến$n$, đảm bảo bao gồm tất cả các ước số cần thiết. biểu thức`n % i`được tính toán trực tiếp cho mỗi lần lặp, khớp chính xác với định nghĩa. 

Không có trường hợp ranh giới phức tạp nào trong quá trình thực hiện nhưng phải cẩn thận để bao gồm$i = n$, từ$n \bmod n = 0$, và việc loại trừ nó sẽ làm giảm tổng số không chính xác một chút. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
```Chúng tôi tính toán từng thuật ngữ: 

| tôi | 5% tôi | Tổng Chạy | 
| --- | --- | --- | 
| 1 | 0 | 0 | 
| 2 | 1 | 1 | 
| 3 | 2 | 3 | 
| 4 | 1 | 4 | 
| 5 | 0 | 4 | 

Đầu ra là 4. 

Dấu vết này cho thấy phần còn lại dao động như thế nào tùy thuộc vào cách$i$chia rẽ$n$. Các đóng góp không đơn điệu và chỉ có đánh giá trực tiếp mới nắm bắt được cấu trúc chính xác. 

### Ví dụ 2 

đầu vào:```
6
```| tôi | 6% tôi | Tổng Chạy | 
| --- | --- | --- | 
| 1 | 0 | 0 | 
| 2 | 0 | 0 | 
| 3 | 0 | 0 | 
| 4 | 2 | 2 | 
| 5 | 1 | 3 | 
| 6 | 0 | 3 | 

Đầu ra là 3. 

Ví dụ này nhấn mạnh rằng nhiều số hạng ban đầu đóng góp bằng 0 vì chúng chia$n$chính xác. Những đóng góp khác 0 chỉ đến từ các số không chia, điều này giải thích tại sao tổng có xu hướng tương đối nhỏ so với$n^2$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi lặp lại một lần trên tất cả các số nguyên từ 1 đến n, thực hiện modulo và phép cộng theo thời gian không đổi mỗi lần | 
| Không gian | O(1) | Chỉ có một biến tích lũy duy nhất được sử dụng | 

Được cho$n \le 10^5$, MỘT$O(n)$vòng lặp đủ nhanh trong Python, tối đa là$10^5$lặp và số học đơn giản cho mỗi lần lặp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    ans = 0
    for i in range(1, n + 1):
        ans += n % i
    return str(ans)

# provided sample
assert run("5\n") == "4"

# minimum input
assert run("1\n") == "0"

# small case with full divisibility structure
assert run("6\n") == "3"

# all primes behavior check
assert run("7\n") == str(sum(7 % i for i in range(1, 8)))

# larger boundary
assert run("100000\n") == str(sum(100000 % i for i in range(1, 100001)))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | trường hợp cạnh nhỏ nhất | 
| 6 | 3 | nhiều ước số chính xác | 
| 100000 | tổng tính toán | sự tỉnh táo về hiệu suất giới hạn trên | 

## Vỏ cạnh 

cho$n = 1$, vòng lặp chỉ chạy một lần với$i = 1$. Việc tính toán là$1 \bmod 1 = 0$, do đó bộ tích lũy vẫn ở mức 0 và đầu ra đúng. 

Đối với các giá trị ở đó$n$là số nguyên tố, tất cả các số hạng ngoại trừ$i = 1$Và$i = n$đóng góp các giá trị khác 0. Ví dụ,$n = 7$tạo ra phần còn lại$0,1,2,3,2,1,0$và thuật toán tích lũy chúng trực tiếp mà không cần dựa vào bất kỳ logic trường hợp đặc biệt nào. 

Đối với lớn$n$, chẳng hạn như$100000$, vòng lặp vẫn thực thi trong giới hạn vì nó chỉ thực hiện phép tính số học đơn giản trên mỗi lần lặp. Không có trạng thái trung gian nào vượt quá một số nguyên duy nhất, do đó mức sử dụng bộ nhớ không đổi và ổn định.

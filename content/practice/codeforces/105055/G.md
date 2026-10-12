---
title: "CF 105055G - Thần Đèn"
description: "Chúng ta được cung cấp một chuỗi có độ dài $N$ bao gồm ba loại ký tự: dấu ngoặc đơn mở cố định, dấu ngoặc đơn đóng cố định và vị trí ký tự đại diện được viết là ?. Mỗi ký tự đại diện có thể được thay thế độc lập bằng ( hoặc )."
date: "2026-06-28T00:23:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105055
codeforces_index: "G"
codeforces_contest_name: "UDESC Selection Contest 2023-2"
rating: 0
weight: 105055
solve_time_s: 31
verified: true
draft: false
---

[CF 105055G - Thần đèn](https://codeforces.com/problemset/problem/105055/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi có độ dài$N$gồm ba loại ký tự: dấu ngoặc đơn mở cố định, dấu ngoặc đơn đóng cố định và vị trí ký tự đại diện được viết dưới dạng`?`. Mỗi ký tự đại diện có thể được thay thế độc lập bằng một trong hai`(`hoặc`)`. 

Sau khi thay thế tất cả các ký tự đại diện, chúng ta thu được chuỗi dấu ngoặc đơn chuẩn. Một chuỗi như vậy là hợp lệ nếu nó có thể được hiểu là một chuỗi chính xác của các dấu ngoặc đơn trùng khớp, nghĩa là các tiền tố không bao giờ có nhiều dấu ngoặc đóng hơn dấu ngoặc mở và tổng số dấu ngoặc mở và đóng là bằng nhau. 

Nhiệm vụ là đếm xem có bao nhiêu phép gán của các dấu chấm hỏi tạo ra một chuỗi cân bằng hợp lệ và xuất ra số đếm này theo modulo.$10^9 + 7$. 

Ràng buộc$N \le 3000$ngay lập tức loại bỏ việc ép buộc tàn bạo tất cả$2^{\#?}$bài tập. Trong trường hợp xấu nhất, chuỗi toàn là dấu chấm hỏi, do đó có$2^{3000}$những khả năng vượt xa sự liệt kê khả thi. Ngay cả việc kiểm tra một bài tập cũng mất$O(N)$, vì vậy vũ lực là hoàn toàn không thể thực hiện được. 

Điều này thúc đẩy chúng tôi hướng tới một giải pháp lập trình động giúp xây dựng các tiền tố hợp lệ theo từng bước. 

Một trường hợp thất bại tinh vi đối với lý luận ngây thơ là giả định rằng chỉ có tổng số`(`Và`)`vấn đề. Ví dụ: các chuỗi như`"())("`có số lượng bằng nhau nhưng không hợp lệ do mất cân bằng tiền tố. Một cạm bẫy khác là việc tham lam ghép các cặp từ trái sang phải mà không cân nhắc các lựa chọn trong tương lai; các ký tự đại diện có thể sửa chữa hoặc phá vỡ sự cân bằng sau này, vì vậy các quyết định cục bộ không an toàn. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp đi lặp lại trên tất cả$2^k$thay thế các dấu chấm hỏi, xây dựng chuỗi kết quả và kiểm tra xem nó có cân bằng hay không bằng cách sử dụng ngăn xếp hoặc quét bộ đếm. Điều này đúng vì nó trực tiếp kiểm tra định nghĩa về tính hợp lệ. Tuy nhiên, nếu$k = N$, điều này đã yêu cầu$2^{3000}$cấu hình và mỗi chi phí kiểm tra tính hợp lệ$O(N)$, dẫn đến thời gian hàm mũ không thể chạy được. 

Quan sát quan trọng là tính hợp lệ của tiền tố dấu ngoặc đơn chỉ phụ thuộc vào số lượng dấu ngoặc đơn mở chưa khớp mà chúng ta hiện có. Khi quét từ trái sang phải, chúng tôi chỉ cần theo dõi số dư hiện tại, được xác định như sau:$$\text{balance} = \#(\text{open}) - \#(\text{close})$$Một chuỗi hợp lệ nếu số dư này không bao giờ trở thành số âm và kết thúc bằng 0. 

Điều này cho thấy việc lập trình động theo các vị trí và sự cân bằng. Ở mỗi chỉ mục, chúng tôi xem xét các chuyển đổi tùy thuộc vào việc ký tự đó là cố định hay ký tự đại diện. Từ một tiểu bang$(i, b)$, chúng tôi chuyển đến$i+1$và cập nhật$b$bằng +1 hoặc -1 tùy thuộc vào ký tự được chọn. 

Điều này làm giảm vấn đề đếm đường dẫn trong biểu đồ kích thước lớp$O(N^2)$, vì số dư dao động từ$0$ĐẾN$N$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^k \cdot N)$|$O(N)$| Quá chậm | 
| DP qua vị trí và sự cân bằng |$O(N^2)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một mảng DP trong đó`dp[b]`biểu thị số cách xử lý tiền tố cho đến vị trí hiện tại sao cho số dư hiện tại chính xác$b$và tiền tố là hợp lệ (số dư không bao giờ âm). 

Chúng tôi xử lý các ký tự từ trái sang phải. 

1. Khởi tạo`dp[0] = 1`. Điều này thể hiện tiền tố trống có số dư bằng 0 theo đúng một cách. 
2. Với mỗi ký tự trong chuỗi ta xây dựng một mảng DP mới`ndp`ban đầu tất cả đều là số không. 
3. Nếu ký tự hiện tại là`(`, mọi trạng thái có sự cân bằng$b$chuyển tiếp sang$b+1$. Chúng tôi chỉ cho phép điều này nếu$b+1 \le N$. Điều này phản ánh rằng chúng ta không có lựa chọn nào khác: nhân vật phải tăng cường sự cân bằng. 
4. Nếu ký tự hiện tại là`)`, mọi trạng thái có sự cân bằng$b > 0$chuyển tiếp sang$b-1$. Hoa với$b = 0$không hợp lệ cho quá trình chuyển đổi này vì chúng tôi không thể đóng tiền tố chưa khớp. 
5. Nếu ký tự hiện tại là`?`, chúng tôi xem xét cả hai quá trình chuyển đổi: coi nó như`(`và như`)`. Cái đầu tiên tăng số dư, cái thứ hai giảm số dư nếu có thể. 
6. Sau khi xử lý từng ký tự, ta thay thế`dp`với`ndp`. 
7. Sau khi xử lý hết các ký tự thì đáp án là`dp[0]`, vì một chuỗi hoàn chỉnh hợp lệ phải kết thúc với số dư bằng 0. 

Lý do chính khiến điều này có hiệu quả là sự cân bằng mô tả đầy đủ liệu tiền tố có hợp lệ hay không; không có thông tin lịch sử khác được yêu cầu. 

### Tại sao nó hoạt động 

Tại bất kỳ tiền tố nào, mỗi phép gán một phần hợp lệ đều được mô tả duy nhất bởi vị trí và số dư hiện tại của nó. Bất kỳ hai chuỗi thành phần nào có cùng vị trí và số dư đều có khả năng mở rộng trong tương lai giống hệt nhau vì giá trị trong tương lai chỉ phụ thuộc vào việc không chuyển sang âm và kết thúc bằng 0. Do đó, DP phân chia tất cả các cấu trúc hợp lệ thành các lớp tương đương rời rạc được lập chỉ mục theo sự cân bằng và các chuyển tiếp duy trì tính chính xác và đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

n = int(input().strip())
s = input().strip()

dp = [0] * (n + 2)
dp[0] = 1

for ch in s:
    ndp = [0] * (n + 2)
    if ch == '(':
        for b in range(n):
            if dp[b]:
                ndp[b + 1] = (ndp[b + 1] + dp[b]) % MOD
    elif ch == ')':
        for b in range(1, n + 1):
            if dp[b]:
                ndp[b - 1] = (ndp[b - 1] + dp[b]) % MOD
    else:
        for b in range(n + 1):
            if dp[b]:
                ndp[b + 1] = (ndp[b + 1] + dp[b]) % MOD
                if b > 0:
                    ndp[b - 1] = (ndp[b - 1] + dp[b]) % MOD
    dp = ndp

print(dp[0])
```Mảng DP có kích thước$n+2$để đáp ứng các quá trình chuyển đổi một cách an toàn mà không cần kiểm tra ranh giới ở mỗi bước. Mỗi bản cập nhật ký tự sẽ tạo một mảng mới để tránh các trạng thái ghi đè vẫn cần thiết trong cùng một lần lặp. 

Chi tiết triển khai quan trọng là đảm bảo chúng tôi không bao giờ cho phép chuyển đổi số dư âm, điều này được thực thi bằng cách bắt đầu`)`lặp từ số dư 1 và bằng cách kiểm tra`b > 0`trong trường hợp ký tự đại diện. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
()(?
```Chúng tôi theo dõi trạng thái DP dưới dạng phân phối số dư. 

| bước | char | trạng thái dp (khác 0) | 
| --- | --- | --- | 
| 0 | ban đầu | {0: 1} | 
| 1 |`(`| {1: 1} | 
| 2 |`)`| {0: 1} | 
| 3 | | |

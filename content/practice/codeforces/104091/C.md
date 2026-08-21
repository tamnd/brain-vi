---
title: "CF 104091C - \u0411\u0443\u0434\u044c \u043d\u0430\u0447\u0435\u043a\u0443!"
description: "Chúng ta cần đếm xem có bao nhiêu số thập phân có đúng n chữ số thỏa mãn quy tắc kề đặc biệt. Một số được gọi là đẹp nếu mọi cặp chữ số cạnh nhau tạo thành số có hai chữ số chia hết cho 3."
date: "2026-07-02T02:27:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104091
codeforces_index: "C"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u041f\u0435\u0442\u0440\u043e\u0437\u0430\u0432\u043e\u0434\u0441\u043a\u0435 \u0438 \u041a\u0430\u0440\u0435\u043b\u0438\u0438 2022-2023"
rating: 0
weight: 104091
solve_time_s: 41
verified: true
draft: false
---

[CF 104091C - \u0411\u0443\u0434\u044c \u043d\u0430\u0447\u0435\u043a\u0443!](https://codeforces.com/problemset/problem/104091/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đếm chính xác có bao nhiêu số thập phân`n`các chữ số thỏa mãn quy tắc kề đặc biệt. 

Một số được gọi là đẹp nếu mọi cặp chữ số cạnh nhau tạo thành số có hai chữ số chia hết cho`3`. Ví dụ,`12754`đẹp vì`12`,`27`,`75`, Và`54`đều chia hết cho`3`. Mặt khác,`1221`không đẹp vì`22`không chia hết cho`3`. 

Chữ số đầu tiên không thể bằng 0 vì số đó phải có chính xác`n`chữ số. Mọi chữ số còn lại có thể bằng 0 nếu điều kiện chia hết được thỏa mãn. 

Đầu vào chứa một số nguyên duy nhất`n`, Ở đâu`2 ≤ n ≤ 27`. Đầu ra là tổng số lượng đẹp`n`các chữ số. 

Giá trị của`n`là cực kỳ nhỏ. Ngay cả một thuật toán có độ phức tạp tỷ lệ thuận với`n × 100`hoặc`n × 1000`dễ dàng phù hợp trong giới hạn. Kiểm tra kỹ lưỡng từng`n`số có chữ số là không thể vì có`9 × 10^(n-1)`ứng viên. Để có giá trị lớn nhất,`n = 27`, đây là đại khái`9 × 10^26`những con số vượt xa mọi tính toán thực tế. 

Một trường hợp tinh tế là chữ số đầu tiên. Ví dụ, khi`n = 2`, số`03`thỏa mãn quy tắc kề vì`03 = 3`chia hết cho`3`, nhưng đó không phải là số có hai chữ số hợp lệ. Thuật toán phải cấm các số 0 đứng đầu. 

Một sai lầm dễ dàng khác là hiểu sai điều kiện. Cặp đôi`75`chia hết cho`3`, nhưng không có chữ số riêng lẻ nào có thuộc tính đặc biệt. Ví dụ,`12`là hợp lệ bởi vì`12`chia hết cho`3`, trong khi`13`không phải vì`13 % 3 = 1`. 

Một quan sát thú vị hơn đến từ sự chia hết cho`3`. Từ```
10a + b ≡ a + b (mod 3),
```một cặp có thể chia hết cho`3`chính xác khi hai chữ số có thặng dư mà tổng chia hết cho`3`. Giá trị thập phân thực tế không bao giờ cần phải tính toán. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là tạo ra mọi`n`số chữ số, kiểm tra từng cặp liền kề và đếm những số hợp lệ. Điều này rõ ràng là đúng vì mọi ứng viên đều được kiểm tra độc lập. Thật không may, thời gian chạy tỷ lệ thuận với`9 × 10^(n-1)`, trở thành đại khái`10^27`hoạt động trong trường hợp xấu nhất. 

Điều kiện chỉ liên quan đến các chữ số lân cận. Khi chữ số trước đó được cố định, các chữ số trước đó không còn quan trọng nữa. Đây chính xác là tình huống mà lập trình động trên chữ số cuối cùng có hiệu quả. 

Quan sát quan trọng là sự chia hết cho`3`chỉ phụ thuộc vào dư lượng của hai chữ số lân cận. Chúng ta có thể xây dựng số từ trái sang phải mà chỉ nhớ chữ số cuối cùng. Nếu chúng ta đã biết có bao nhiêu tiền tố hợp lệ kết thúc bằng chữ số`d`, thì mỗi chữ số`x`thỏa mãn`(10d + x) % 3 == 0`có thể mở rộng các tiền tố đó. 

Chỉ có mười chữ số cuối cùng có thể, vì vậy mỗi lớp của chương trình động chỉ chứa mười trạng thái. Mỗi trạng thái thử tối đa mười lần chuyển đổi, chỉ đưa ra khoảng một trăm phép tính cho mỗi chữ số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10ⁿ × n) | O(1) | Quá chậm | 
| DP tối ưu | O(n × 100) | O(10) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`dp`kích thước`10`, Ở đâu`dp[d]`lưu trữ số tiền tố hợp lệ kết thúc bằng chữ số`d`. 
2. Khởi tạo chữ số đầu tiên. chữ số`1`bởi vì`9`mỗi dạng một tiền tố có độ dài hợp lệ`1`, vậy thiết lập`dp[1]`bởi vì`dp[9]`ĐẾN`1`. Rời khỏi`dp[0]`bằng`0`bởi vì số 0 đứng đầu bị cấm. 
3. Lặp lại`n - 1`lần, một lần cho mỗi vị trí còn lại. 
4. Tạo một mảng mới`next_dp`chứa đầy số không. 
5. Với mọi chữ số có thể có trước đó`a`và mọi chữ số tiếp theo có thể có`b`, kiểm tra xem`(10 * a + b) % 3 == 0`. Nếu đúng như vậy, mọi tiền tố hợp lệ đều kết thúc bằng`a`có thể được mở rộng bởi`b`, vì vậy hãy thêm`dp[a]`ĐẾN`next_dp[b]`. 
6. Thay thế`dp`với`next_dp`. 
7. Sau khi xử lý tất cả các vị trí, tính tổng mọi giá trị trong`dp`. Mỗi trạng thái đại diện cho các số hợp lệ kết thúc bằng một chữ số cuối cùng khác nhau, vì vậy tổng của chúng là câu trả lời bắt buộc. 

### Tại sao nó hoạt động 

Bất biến quy hoạch động là sau khi xử lý`k`chữ số,`dp[d]`bằng số lượng hợp lệ`k`tiền tố chữ số có chữ số cuối cùng là`d`. 

Việc khởi tạo là chính xác vì mọi chữ số khác 0 tạo thành chính xác một tiền tố hợp lệ có độ dài bằng một. 

Trong quá trình chuyển đổi, mỗi tiện ích mở rộng được xem xét chính xác một lần. Quá trình chuyển đổi được phép chính xác khi cặp liền kề mới được tạo chia hết cho`3`. Không có tiện ích mở rộng không hợp lệ nào được thêm vào và không có tiện ích mở rộng hợp lệ nào bị bỏ qua. 

Bằng quy nạp theo chiều dài được xử lý, bất biến vẫn đúng sau mỗi lần lặp. Rốt cuộc`n`các chữ số đã được xử lý, mọi chữ số đều đẹp`n`số có chữ số thuộc về chính xác một trạng thái theo chữ số cuối cùng của nó, vì vậy tổng tất cả các trạng thái sẽ cho câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

dp = [0] * 10
for d in range(1, 10):
    dp[d] = 1

for _ in range(n - 1):
    ndp = [0] * 10
    for a in range(10):
        if dp[a] == 0:
            continue
        for b in range(10):
            if (10 * a + b) % 3 == 0:
                ndp[b] += dp[a]
    dp = ndp

print(sum(dp))
```Việc khởi tạo đại diện cho tất cả các tiền tố một chữ số có thể có. Chữ số 0 bị loại trừ vì số cuối cùng phải chứa chính xác`n`chữ số. 

Mỗi lần lặp lại mở rộng các tiền tố thêm một chữ số. Một mảng mới được sử dụng vì các chuyển đổi cho vị trí hiện tại không được ảnh hưởng đến các chuyển đổi khác trong cùng một lớp. 

Việc bỏ qua các trạng thái có số đếm bằng 0 là không cần thiết để đảm bảo tính chính xác nhưng nó tránh được những công việc không cần thiết. 

Số nguyên Python tự động tăng đến kích thước tùy ý, do đó câu trả lời phù hợp ngay cả khi nó có thể vượt quá phạm vi của số nguyên 32 bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào```
2
```Khởi tạo: 

| Chiều dài | dp (các mục khác 0) | 
| --- | --- | 
| 1 | 1:1 2:1 3:1 4:1 5:1 6:1 7:1 8:1 9:1 | 

Sau khi xử lý chữ số thứ hai: 

| Chữ số trước | Được phép chữ số tiếp theo | 
| --- | --- | 
| 1 | 2, 5, 8 | 
| 2 | 1, 4, 7 | 
| 3 | 0, 3, 6, 9 | 
| 4 | 2, 5, 8 | 
| 5 | 1, 4, 7 | 
| 6 | 0, 3, 6, 9 | 
| 7 | 2, 5, 8 | 
| 8 | 1, 4, 7 | 
| 9 | 0, 3, 6, 9 | 

Tổng số cuối cùng là`30`. 

Ví dụ này cho thấy rằng mọi phần mở rộng chỉ phụ thuộc vào chữ số trước đó chứ không phụ thuộc vào toàn bộ

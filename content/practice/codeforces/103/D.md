---
title: "CF 103D - Đã đến lúc tấn công người chăn bò"
description: "Chúng tôi có một loạt các trọng lượng bò. Đối với mọi truy vấn (a, b), chúng ta liên tục lấy các vị trí a, a + b, a + 2b, ... cho đến khi chỉ số vượt quá n và chúng ta phải xuất tổng của tất cả các giá trị đã truy cập."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "sortings"]
categories: ["algorithms"]
codeforces_contest: 103
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 80 (Div. 1 Only)"
rating: 2100
weight: 103
solve_time_s: 108
verified: false
draft: false
---

[CF 103D - Đã đến lúc tấn công người chăn bò](https://codeforces.com/problemset/problem/103/D) 

**Xếp hạng:** 2100 
**Tags:** bạo lực, cấu trúc dữ liệu, sắp xếp 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một loạt các trọng lượng bò. Đối với mỗi truy vấn`(a, b)`, chúng tôi liên tục đảm nhận vị trí`a, a + b, a + 2b, ...`cho đến khi chỉ số vượt quá`n`và chúng ta phải xuất ra tổng của tất cả các giá trị đã truy cập. 

Nếu mảng là`[1, 2, 3, 4, 5]`và truy vấn là`(2, 2)`, chúng tôi đảm nhận vị trí`2, 4`, vậy câu trả lời là`2 + 4 = 6`. 

Phần quan trọng là quy mô. Cả số lượng bò và số lượng truy vấn đều có thể đạt tới`3 * 10^5`. Việc mô phỏng trực tiếp cho mọi truy vấn có thể trở nên cực kỳ tốn kém. 

Giả sử chúng ta xử lý một truy vấn bằng cách nhảy liên tục`b`. Số lượng phần tử được truy cập là khoảng`n / b`. 

Nếu như`b = 1`, một truy vấn chạm vào tất cả`n`các phần tử. Với`3 * 10^5`truy vấn, điều đó sẽ trở thành về$3 \cdot 10^5 \times 3 \cdot 10^5 = 9 \cdot 10^{10}$hoạt động vượt xa giới hạn. 

Cấu trúc của các truy vấn rất quan trọng. Kích thước bước nhảy nhỏ tạo ra chuỗi dài, trong khi kích thước bước nhảy lớn tạo ra chuỗi ngắn. Sự bất đối xứng này là quan sát chính đằng sau giải pháp được chấp nhận. 

Ngoài ra còn có một số trường hợp khó xử lý. 

Hãy xem xét đầu vào này:```
5
1 2 3 4 5
1
5 3
```Câu trả lời đúng là:```
5
```Sau khi đảm nhận vị trí`5`, vị trí tiếp theo sẽ là`8`, nằm ngoài mảng. Một điều kiện vòng lặp bất cẩn như`while pos < n`thay vì`<= n`sẽ bỏ qua phần tử hợp lệ cuối cùng. 

Một trường hợp tế nhị khác xuất hiện khi`b = 1`.```
4
10 20 30 40
1
2 1
```Câu trả lời đúng là:```
90
```bởi vì chúng tôi lấy`20 + 30 + 40`. 

Đây là độ dài truy vấn trong trường hợp xấu nhất. Bất kỳ giải pháp nào tính toán lại các khoản tiền này nhiều lần sẽ hết thời gian chờ. 

Một vấn đề khác xuất hiện với việc lập chỉ mục. Sự cố sử dụng vị trí dựa trên 1, nhưng danh sách Python dựa trên 0.```
3
5 6 7
1
1 2
```Câu trả lời đúng là:```
12
```bởi vì chúng tôi đảm nhận vị trí`1`Và`3`, cho`5 + 7`. 

Nếu chúng ta quên chuyển đổi chỉ số đúng cách, chúng ta có thể vô tình tính tổng`6`thay vì. 

Cuối cùng, số tiền có thể trở nên lớn. Mỗi giá trị có thể lên tới`10^9`, và chúng ta có thể cộng tới`3 * 10^5`các phần tử. Kết quả có thể vượt quá phạm vi số nguyên 32 bit, do đó việc triển khai phải sử dụng số học 64 bit. Python tự động xử lý việc này. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force tuân theo định nghĩa truy vấn trực tiếp. 

Đối với một truy vấn`(a, b)`, bắt đầu ở vị trí`a`, liên tục thêm phần tử hiện tại vào câu trả lời, sau đó chuyển sang`a + b`,`a + 2b`, v.v. cho đến khi rời khỏi mảng. 

Điều này hoạt động vì bản thân truy vấn mô tả rõ ràng tiến trình số học của các chỉ mục. 

Chi phí của một truy vấn tỷ lệ thuận với số lượng vị trí được truy cập, đại khái là`n / b`. 

Khi`b`lớn, điều này thực sự rất nhanh. Ví dụ, nếu`b = 1000`, chỉ về`300`vị trí được truy cập ngay cả khi`n = 3 * 10^5`. 

Vấn đề xuất phát từ các giá trị nhỏ của`b`. 

Nếu có nhiều truy vấn`b = 1`, mọi truy vấn sẽ quét gần như toàn bộ mảng. Trong trường hợp xấu nhất, độ phức tạp tổng cộng trở thành$O(p \cdot n)$quá chậm. 

Quan sát quan trọng là chỉ có một số ít kích thước bước nhảy nhỏ khác biệt. 

Nếu chúng ta chọn một ngưỡng`B ≈ sqrt(n)`, thì: 

Đối với các truy vấn có`b > B`, lực lượng vũ phu rẻ vì mỗi truy vấn truy cập nhiều nhất`n / B`các phần tử. 

Đối với các truy vấn có`b <= B`, chúng ta có thể tính toán trước tất cả các câu trả lời. 

Điều này hoạt động vì số lượng nhỏ`b`các giá trị bị hạn chế. 

Đối với kích thước bước nhảy cố định`b`, định nghĩa:$dp[i] = w_i + dp[i+b]$Ở đâu`dp[i]`đại diện cho câu trả lời cho truy vấn`(i, b)`. 

Chúng tôi tính toán ngược lại từ`n`xuống`1`. Sau khi được xây dựng, mọi truy vấn với điều này`b`có thể được trả lời trong thời gian liên tục. 

Tổng chi phí tiền xử lý có thể quản lý được vì chúng tôi chỉ thực hiện việc này trong khoảng`sqrt(n)`kích cỡ bước nhảy khác nhau. 

Độ phức tạp cuối cùng trở nên đại khái:$O(n\sqrt{n} + p\sqrt{n})$dễ dàng phù hợp trong giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(p · n) trường hợp xấu nhất | O(1) | Quá chậm | 
| Tối ưu | O(n√n + p√n) | O(n√n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng trọng lượng bò bằng cách sử dụng chỉ mục dựa trên 1. 

Việc sử dụng lập chỉ mục dựa trên 1 sẽ giữ logic truy vấn giống hệt với câu lệnh và tránh chuyển đổi chỉ mục lặp lại. 
2. Chọn giá trị ngưỡng`B = int(sqrt(n)) + 1`. 

Truy vấn có kích thước bước nhảy lớn hơn`B`đủ ngắn để xử lý trực tiếp. Các truy vấn có kích thước bước nhảy nhỏ hơn đáng được xử lý trước. 
3. Tạo bảng DP trong đó`dp[b][i]`lưu trữ câu trả lời cho truy vấn`(i, b)`cho tất cả`b <= B`. 
4. Đối với mọi kích thước bước nhảy nhỏ`b`từ`1`ĐẾN`B`, tính toán ngược bảng. 

Sự tái phát là: 

nếu`i + b <= n`, nếu không thì: 

sự tái phát:

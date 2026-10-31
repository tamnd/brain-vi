---
title: "CF 106347B - \u0426\u0432\u0435\u0442\u043e\u0447\u043d\u044b\u0439 \u043c\u0430\u0433\u0430\u0437\u0438\u043d"
description: "Chúng tôi có một cửa hàng hoa với n loại hoa khác nhau. Một bó hoa hợp lệ cần chính xác một bông hoa của mỗi loại, do đó, để tạo ra một bó hoa sẽ tiêu tốn một bông hoa từ mỗi loại. Cửa hàng đã sở hữu một[i] hoa loại i."
date: "2026-06-25T08:04:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106347
codeforces_index: "B"
codeforces_contest_name: "\u0412\u044b\u0441\u0448\u0430\u044f \u043f\u0440\u043e\u0431\u0430 - 2024. \u0417\u0430\u043a\u043b\u044e\u0447\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f"
rating: 0
weight: 106347
solve_time_s: 29
verified: false
draft: false
---

[CF 106347B - \u0426\u0432\u0435\u0442\u043e\u0447\u043d\u044b\u0439 \u043c\u0430\u0433\u0430\u0437\u0438\u043d](https://codeforces.com/problemset/problem/106347/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một cửa hàng hoa với`n`các loại hoa khác nhau. Một bó hoa hợp lệ cần chính xác một bông hoa của mỗi loại, do đó, để tạo ra một bó hoa sẽ tiêu tốn một bông hoa từ mỗi loại. Shop đã sở hữu rồi`a[i]`loại hoa`i`. Nếu hết loại nào đó, Petya có thể mua thêm loại hoa đó từ cơ sở, trả một rúp cho mỗi bông hoa đã mua. 

Đối với mỗi giá trị truy vấn`x`, chúng ta cần xác định số lượng bó hoa hoàn chỉnh lớn nhất mà Petya có thể làm nếu anh ấy được phép chi nhiều nhất`x`rúp cho những bông hoa thêm. 

Khó khăn chính là các truy vấn độc lập. Chúng tôi không thể mô phỏng việc mua hoa riêng biệt cho từng truy vấn vì cả số lượng loại hoa và số lượng truy vấn đều có thể đạt tới`100000`. Bất kỳ giải pháp nào thực hiện công việc tỷ lệ thuận với`n * q`sẽ yêu cầu lên tới`10^10`vượt xa giới hạn 1 giây thông thường cho phép. Chúng ta cần xử lý trước thông tin về hoa một lần và trả lời từng truy vấn một cách nhanh chóng. 

Số lượng ban đầu của mỗi loại hoa có thể rất lớn, lên tới`10^9`, vì vậy việc lưu trữ mọi số lượng bó hoa có thể có hoặc mô phỏng từng bông hoa riêng lẻ là không thể. Thay vào đó, thuật toán phải hoạt động với các phạm vi và công thức số học. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Hãy xem xét một loại hoa duy nhất:```
Input:
1 3
5
0 1 10
```Đầu ra đúng là:```
5 6 15
```Với một loại hoa, mỗi bông hoa đã là một bó hoa hoàn chỉnh. Giải pháp giả định một bó hoa luôn cần nhiều loại hoặc quên trường hợp này có thể trả về giá trị không chính xác. 

Một trường hợp quan trọng khác là khi ban đầu cửa hàng có số tiền không đồng đều:```
Input:
2 3
1 0
1 2 5
```Câu trả lời là:```
1 1 3
```Với một đồng rúp, Petya mua một bông hoa loại thứ hai và làm một bó hoa. Với hai rúp, anh ta vẫn không thể làm được hai bó hoa vì loại thứ nhất thiếu một bông hoa bổ sung cho bó hoa thứ hai. Một cách tiếp cận bất cẩn chỉ tính tổng số hoa có sẵn có thể cho rằng hai rúp là đủ. 

Trường hợp thứ ba là khi một loại hoa giới hạn số lượng bó hoa:```
Input:
3 1
100 100 0
10
```Câu trả lời là:```
10
```Mặc dù hai loại có nhiều hoa nhưng loại thứ ba yêu cầu phải mua tất cả. Số lượng bó hoa phụ thuộc vào việc cân đối các loại chứ không phụ thuộc vào tổng số lượng hoa. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là kiểm tra từng bó hoa có thể. Giả sử chúng ta muốn biết liệu`k`bó hoa là có thể. Kiểu`i`yêu cầu`k`hoa, nhưng cửa hàng đã có rồi`a[i]`, vậy số tiền còn thiếu là:```
max(0, k - a[i])
```Tổng số tiền cần thiết là tổng số tiền còn thiếu của tất cả các loại. Nếu giá trị lớn nhất`x`, sau đó`k`bó hoa có thể được thực hiện. 

Việc kiểm tra này là chính xác vì mỗi bông hoa bị thiếu đều có cùng mức giá và điều quan trọng duy nhất là mỗi loại bị thiếu bao nhiêu bông hoa. 

Vấn đề là số lần kiểm tra. Câu trả lời có thể lớn như`sum(a[i]) + x`, có thể đạt tới khoảng`10^14`. Ngay cả khi chúng tôi sử dụng tìm kiếm nhị phân cho mọi truy vấn, mỗi truy vấn sẽ cần khoảng 50 lần kiểm tra và mỗi lần kiểm tra sẽ quét tất cả`n`các loại hoa. Sự phức tạp trở thành:```
O(q * n * log(answer))
```quá chậm đối với`n, q = 100000`. 

Quan sát chính là chi phí sản xuất`k`bó hoa có hình dạng rất đơn giản. 

Đối với một cố định`k`, mỗi loại có`a[i] >= k`đóng góp chi phí bằng không. Mỗi loại có`a[i] < k`đóng góp chính xác`k - a[i]`. Nếu chúng ta sắp xếp số lượng hoa hiện có thì như`k`phát triển, tập hợp các loại hoa yêu cầu mua hoa chỉ thay đổi khi`k`vượt qua một trong các giá trị hiện có. 

Sau khi sắp xếp:```
b[0] <= b[1] <= ... <= b[n-1]
```để đếm số bó hoa mục tiêu`k`, giả sử chính xác`t`các loại có`b[i] < k`. Số tiền cần có là:```
t * k - (b[0] + b[1] + ... + b[t-1])
```Phần khó khăn là không kiểm tra một giá trị của`k`, nhưng trả lời nhiều câu hỏi. Chúng ta cần hiểu bài toán nghịch đảo: với một ngân sách nhất định`x`, cái nào lớn nhất`k`nơi có chi phí nhiều nhất`x`? 

Bởi vì hàm chi phí chỉ tăng khi`k`tăng lên, chúng ta có thể tìm kiếm câu trả lời theo hệ nhị phân. Tổng mảng và tiền tố được sắp xếp cho phép chúng tôi tính toán chi phí của bất kỳ ứng cử viên nào`k`nhanh chóng. 

Đối với một ứng cử viên`k`, ta thấy có bao nhiêu loại hoa có ít hơn`k`hoa bằng cách sử dụng tìm kiếm nhị phân. Sau đó, chúng tôi sử dụng tổng tiền tố của các loại đó để tính toán chính xác số tiền cần thiết. 

Phương pháp vũ phu hoạt động vì mọi số lượng bó hoa của ứng cử viên đều có thể được xác minh một cách độc lập. Nhận xét rằng hàm xác minh là đơn điệu cho phép chúng ta tìm kiếm câu trả lời thay vì thử mọi số có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * q * log(câu trả lời)) | O(1) | Quá chậm | 
| Tối ưu | O((n + q) * log(n) * log(câu trả lời)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp số lượng hoa hiện có và xây dựng tổng tiền tố. 

Thứ tự sắp xếp cho phép chúng ta nhanh chóng tìm thấy tất cả các loại hoa có ít hơn số lượng hoa đã chọn. Tổng tiền tố cho phép tính tổng chi phí còn thiếu mà không cần lặp qua các loại đó. 

1. Đối với mỗi ngân sách truy vấn`x`, tìm kiếm nhị phân số lượng bó hoa tối đa có thể. 

Không gian trả lời đơn điệu. Nếu như`k`bó hoa có thể được thực hiện với`x`rúp, thì bất kỳ số lượng bó hoa nhỏ hơn nào cũng có thể được thực hiện với cùng một ngân sách. 

1. Đối với mọi điểm giữa tìm kiếm nhị phân`mid`, tính số tiền cần thiết để`mid`bó hoa. 

Tìm vị trí đầu tiên có số lượng hoa được sắp xếp ít nhất`mid`. Hãy để vị trí này được`pos`. đầu tiên`pos`các loại cần thêm hoa và tổng chi phí của chúng là:```
pos * mid - prefix[pos]
```Các loại còn lại đã có đủ hoa cho`mid`bó hoa. 

1. Nếu chi phí tính toán tối đa`x`, di chuyển tìm kiếm nhị phân lên trên. 

Điều này có nghĩa`mid`bó hoa có thể đạt được, vì vậy có thể có một câu trả lời lớn hơn. 

1. Nếu không, hãy di chuyển tìm kiếm nhị phân xuống dưới. 

Ngân sách không đủ để`mid`bó hoa, vì vậy mọi số lượng lớn hơn cũng là không thể. 

Tại sao nó hoạt động: 

chức năng`cost(k)`, đại diện cho số tiền tối thiểu cần thiết để thực hiện`k`bó hoa, không giảm. Tăng dần`k`không bao giờ có thể làm giảm số lượng hoa bị thiếu. Tìm kiếm nhị phân dựa chính xác vào thuộc tính này. Đối với mỗi giá trị được kiểm tra, tổng tiền tố sẽ tính toán chi phí mua tối thiểu thực sự, do đó việc tìm kiếm chỉ loại bỏ các phạm vi không thể và giữ lại tất cả các câu trả lời có thể. Khi việc tìm kiếm kết thúc, giá trị lớn nhất khả thi`k`là câu trả lời cần thiết.

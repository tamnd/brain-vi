---
title: "CF 104882G - Khối Bà"
description: "Chúng ta được cung cấp một chuỗi các kết quả xúc xắc, mỗi lần ném một kết quả. Ở mỗi vị trí, Masha được phép công bố bất kỳ số nào từ 1 đến 6, không phụ thuộc vào kết quả thực và điểm của cô ấy là tổng của tất cả các số được công bố. Vòng xoắn là một “quy tắc giám sát” gắn liền với giá trị 6."
date: "2026-06-28T09:18:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104882
codeforces_index: "G"
codeforces_contest_name: "Voronezh State University - Sitronics contest II"
rating: 0
weight: 104882
solve_time_s: 34
verified: false
draft: false
---

[CF 104882G - Khối của bà](https://codeforces.com/problemset/problem/104882/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các kết quả xúc xắc, mỗi lần ném một kết quả. Ở mỗi vị trí, Masha được phép công bố bất kỳ số nào từ 1 đến 6, không phụ thuộc vào kết quả thực và điểm của cô ấy là tổng của tất cả các số được công bố. 

Vòng xoắn này là một “quy tắc giám sát” gắn liền với giá trị 6. Nếu người bà nghe thấy ba số sáu được thông báo liên tiếp, bà sẽ ngay lập tức chỉ kiểm tra lần ném gần đây nhất. Nếu giá trị được kiểm tra đó không khớp với kết quả xúc xắc thực, Masha sẽ bị bắt. Sau lần kiểm tra này, chuỗi sáu số liên tiếp được đặt lại. 

Vì vậy, mối nguy hiểm duy nhất đến từ việc tạo ra ba con số sáu đã được công bố. Mọi thứ khác đều không bị ràng buộc ngoại trừ tại thời điểm sáu lần thứ ba liên tiếp xảy ra, vị trí đó phải trung thực, nghĩa là nó phải bằng số tiền thực tế. 

Nhiệm vụ là chọn một chuỗi đã công bố tối đa hóa tổng số tiền trong khi không bao giờ kích hoạt trạng thái bị bắt. 

Đầu vào là chuỗi xúc xắc thực tế. Đầu ra là tổng tối đa có thể có của chuỗi được công bố theo quy tắc trên. 

Với n lên đến 100000, bất kỳ chiến lược bậc hai nào đối với tiền tố hoặc trạng thái đều không thể thực hiện được. Cần có chương trình động tuyến tính hoặc gần tuyến tính vì chuyển tiếp 10^5 là mục tiêu tự nhiên trong giới hạn một giây. 

Một cách tiếp cận ngây thơ sẽ cố gắng mô phỏng tất cả các chuỗi có thể được công bố. Ngay cả khi chúng tôi giới hạn giá trị ở mức 6 để đạt được mức tăng tối đa, chúng tôi vẫn phải đối mặt với một ràng buộc phụ thuộc vào hai quyết định cuối cùng. Điều đó đã gợi ý sự phụ thuộc vào nhà nước hơn là những lựa chọn tham lam độc lập. 

Một trường hợp thất bại tinh vi xuất hiện khi tham lam xuất ra 6 ở mọi nơi. 

Ví dụ: nếu đầu vào là:```
n = 3
a = [1, 1, 1]
```Chiến lược tham lam tạo ra 6, 6, 6, nhưng điều này tạo ra tình huống ba sáu ở vị trí thứ ba, buộc tính trung thực ở vị trí 3. Vì a3 là 1, điều này sẽ gây ra sự không khớp và Masha bị bắt. Vì vậy, dù số tiền tham lam có cao nhưng vẫn vi phạm quy luật. 

Một trường hợp đặc biệt khác là khi chuỗi thực tế đã chứa nhiều số 6:```

```

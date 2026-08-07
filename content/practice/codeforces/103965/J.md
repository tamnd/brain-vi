---
title: "CF 103965J - \u0423\u0431\u043e\u0440\u043a\u0430 \u043b\u0438\u0441\u0442\u044c\u0435\u0432"
description: "Chúng ta được cho một chuỗi các kích thước cọc lá, nhưng những cọc này được đặt trên một hàng dài các vị trí được đánh số từ 1 đến c."
date: "2026-07-02T06:36:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103965
codeforces_index: "J"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2022-2023, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 103965
solve_time_s: 28
verified: false
draft: false
---

[CF 103965J - \u0423\u0431\u043e\u0440\u043a\u0430 \u043b\u0438\u0441\u0442\u044c\u0435\u0432](https://codeforces.com/problemset/problem/103965/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các kích thước cọc lá, nhưng những cọc này được đặt trên một hàng dài các vị trí được đánh số từ 1 đến c. Chỉ n trong số các vị trí này thực sự chứa các cọc khác 0, cụ thể là các vị trí từ 1 đến n lưu trữ các giá trị từ a1 đến an, trong khi tất cả các vị trí sau n đến c đều trống. 

Chúng ta được phép chọn bất kỳ đoạn vị trí liền kề nào có độ dài cố định k, ở bất kỳ đâu trong phạm vi [1, c]. Đối với mỗi phân đoạn như vậy, chúng tôi tính tổng giá trị của tất cả các cọc nằm bên trong nó. Các vị trí bên ngoài 1..n không đóng góp gì vì chúng trống. 

Nhiệm vụ là tìm đoạn có độ dài k sao cho tổng này nhỏ nhất. 

Hạn chế chính là c có thể lớn tới 10^9, điều này khiến không thể coi đây là một cửa sổ trượt bình thường trên một mảng đầy đủ. Số phần tử thực tế khác 0 chỉ có n tối đa 10^5, do đó, mọi giải pháp đúng đều phải tránh lặp lại trên phạm vi tọa độ đầy đủ và thay vào đó là lý do về cách các cửa sổ tương tác với vùng hoạt động nhỏ. 

Một ý tưởng ngây thơ là xây dựng một mảng có kích thước c và trượt một cửa sổ có độ dài k. Điều này thất bại ngay lập tức vì ngay cả việc lặp lại trên c cũng là không thể. 

Ý tưởng ngây thơ thứ hai là chỉ xem xét hoàn toàn các cửa sổ trong phạm vi 1..n. Điều này bỏ lỡ các câu trả lời tối ưu hợp lệ khi cửa sổ mở rộng sang vùng trống ngoài n, điều này có thể làm giảm tổng đáng kể. 

Một trường hợp tinh tế xuất hiện khi k đủ lớn để một cửa sổ có thể nằm hoàn toàn trong vùng trống. Ví dụ: nếu n = 5, c = 10, k = 4 thì chọn [6, 9] mang lại tổng 0, là tối ưu, mặc dù nó không chạm vào bất kỳ giá trị a nào. Bất kỳ cách tiếp cận nào bỏ qua vùng trống sẽ thất bại ở đây. 

Một trường hợp phức tạp khác xuất hiện khi k lớn hơn n. Khi đó, mọi cửa sổ hợp lệ chạm vào bất kỳ phần tử không trống nào đều phải bao gồm một hậu tố của mảng và việc suy luận thuần túy về các mảng con có độ dài cố định của a sẽ trở nên không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xây dựng rõ ràng một mảng b có kích thước c trong đó b[i] = a[i] với i ≤ n và b[i] = 0 nếu không, sau đó đánh giá mọi đoạn có độ dài k. Tổng mỗi cửa sổ sẽ được tính theo O(k), cho độ phức tạp tổng thể là O(c · k), vượt xa mọi giới hạn khả thi vì c có thể là 10^9. 

Cấu trúc của vấn đề cho phép đơn giản hóa mạnh mẽ hơn nhiều. Những đóng góp khác 0 duy nhất được giới hạn ở các vị trí từ 1 đến n và bao giờ hết

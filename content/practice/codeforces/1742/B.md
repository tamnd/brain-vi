---
title: "CF 1742B - Tăng"
description: "Chúng tôi được cung cấp nhiều mảng độc lập. Đối với mỗi cái, chúng ta được phép sắp xếp lại các phần tử của nó một cách tùy ý và chúng ta cần quyết định xem có thể sắp xếp chúng sao cho mọi phần tử đều nhỏ hơn phần tử tiếp theo hay không."
date: "2026-06-09T16:12:19+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1742
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 827 (Div. 4)"
rating: 800
weight: 1742
solve_time_s: 86
verified: false
draft: false
---

[CF 1742B - Đang tăng](https://codeforces.com/problemset/problem/1742/B) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều mảng độc lập. Đối với mỗi cái, chúng ta được phép sắp xếp lại các phần tử của nó một cách tùy ý và chúng ta cần quyết định xem có thể sắp xếp chúng sao cho mọi phần tử đều nhỏ hơn phần tử tiếp theo hay không. 

Nói một cách cụ thể hơn, chúng tôi đang kiểm tra xem liệu nhiều tập giá trị có thể được chuyển thành một chuỗi không có giá trị liền kề bằng nhau sau khi sắp xếp hoặc sắp xếp lại hay không. 

Những hạn chế là nhỏ. Mỗi ca kiểm thử có tối đa 100 phần tử và có nhiều nhất 100 ca kiểm thử, do đó ngay cả một$O(n \log n)$sắp xếp cho mỗi trường hợp thử nghiệm là nhanh chóng thoải mái. Điều này ngay lập tức loại trừ mọi nhu cầu về thủ thuật tối ưu hóa ngoài việc sắp xếp hoặc đếm tiêu chuẩn. 

Trường hợp cạnh chính là khi tồn tại các bản sao. Ví dụ: nếu tất cả các phần tử đều giống nhau`[5, 5, 5]`, không có sự sắp xếp lại nào có thể làm cho dãy tăng một cách nghiêm ngặt vì trật tự chặt chẽ cấm sự bình đẳng ở bất cứ đâu. Mặt khác, nếu tất cả các phần tử đều khác biệt thì việc sắp xếp chúng luôn tạo ra một dãy tăng dần hợp lệ. 

Một trường hợp tinh tế khác là khi có sự trùng lặp nhưng thoạt nhìn không rõ ràng, chẳng hạn như`[1, 2, 2, 3]`. Mặc dù hầu hết các yếu tố đều khác biệt, việc lặp đi lặp lại`2`ngăn chặn sự gia tăng nghiêm ngặt, vì bất kỳ đơn đặt hàng nào cũng sẽ đặt cả hai bản sao của`2`đâu đó trong trình tự và chúng phải vi phạm tính nghiêm ngặt. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử mọi hoán vị của mảng và kiểm tra xem có hoán vị nào đang tăng nghiêm ngặt hay không. Điều này đúng vì nó khám phá toàn bộ không gian giải pháp. Tuy nhiên, số hoán vị là$n!$, điều này trở nên không khả thi ngay cả đối với các giá trị vừa phải như$n = 15$, nơi mà điều này đã vượt quá hàng tỷ cấu hình. 

Nhận xét quan trọng là quyền tự do sắp xếp không thực sự mang lại cho chúng ta nhiều bậc tự do trong bài toán này. Điều kiện chúng ta muốn chỉ phụ thuộc vào việc liệu chúng ta có thể gán thứ hạng tăng dần nghiêm ngặt cho tất cả các phần tử hay không. Sắp xếp cho thấy cấu trúc tốt nhất có thể ngay lập tức. Nếu sau khi sắp xếp có bất kỳ phần tử lân cận bằng nhau nào, thì việc sắp xếp lại không thể tránh được việc chúng nằm liền kề ở đâu đó theo thứ tự giá trị và việc tăng nghiêm ngặt trở nên không thể. 

Vì vậy, vấn đề được rút gọn thành một kiểm tra đơn giản: sắp xếp mảng và đảm bảo mọi cặp liền kề đều tăng nghiêm ngặt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n!)$|$O(n)$| Quá chậm | 
| Tối ưu (sắp xếp) |$O(n \log n)$| | |

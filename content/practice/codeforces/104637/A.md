---
title: "CF 104637A - Đậu đỏ và đậu xanh"
description: "Chúng tôi được phát cho hai đống đồ không thể phân biệt được là đậu đỏ và đậu xanh. Trong mỗi trường hợp thử nghiệm, chúng ta phải quyết định xem có thể chia tất cả các hạt đậu thành nhiều nhóm hay không, trong đó mỗi nhóm chứa cả hai màu và trong mỗi nhóm sự khác biệt giữa số lượng màu đỏ và…"
date: "2026-06-29T16:58:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104637
codeforces_index: "A"
codeforces_contest_name: "\u041c\u0438\u0441\u0438\u0441 2023 \u043e\u0441\u0435\u043d\u044c - \u0431\u0430\u0437\u043e\u0432\u0430\u044f \u043c\u0430\u0442\u0435\u043c\u0430\u0442\u0438\u043a\u0430, \u0443\u0441\u043b\u043e\u0432\u0438\u044f, \u0446\u0438\u043a\u043b\u044b"
rating: 0
weight: 104637
solve_time_s: 30
verified: false
draft: false
---

[CF 104637A - Đậu đỏ và đậu xanh](https://codeforces.com/problemset/problem/104637/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 30s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được phát cho hai đống đồ không thể phân biệt được là đậu đỏ và đậu xanh. Trong mỗi trường hợp thử nghiệm, chúng ta phải quyết định xem có thể chia tất cả các hạt đậu thành nhiều nhóm hay không, trong đó mỗi nhóm chứa cả hai màu và trong mỗi nhóm, sự khác biệt giữa số lượng đậu đỏ và xanh không vượt quá một ngưỡng cố định. 

Về mặt hình thức, mỗi gói phải chứa ít nhất một hạt đỏ và một hạt xanh, và nếu một gói chứa$r_i$màu đỏ và$b_i$đậu xanh rồi$|r_i - b_i| \le d$. Tất cả đậu phải được xếp vào các gói không có thức ăn thừa. 

Các ràng buộc cho phép lên đến$10^9$đậu của mỗi màu và lên đến$1000$các trường hợp thử nghiệm, vì vậy mọi giải pháp đều phải có thời gian không đổi cho mỗi trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào cố gắng xây dựng các gói một cách rõ ràng hoặc mô phỏng phân phối sẽ thất bại ngay lập tức vì số lượng hạt quá lớn. 

Một trường hợp lỗi tinh vi xuất hiện khi một màu lớn hơn đáng kể so với màu kia. Ví dụ, nếu$r = 6, b = 1, d = 4$, người ta có thể thử đặt tất cả các hạt đậu vào một gói, nhưng điều đó vi phạm điều kiện khác biệt vì$|6 - 1| = 5 > 4$. Việc chia thành nhiều gói không giúp ích gì vì mỗi gói phải chứa ít nhất một trong mỗi màu và tổng số mất cân bằng được duy trì trên tất cả các gói. 

Một trường hợp cạnh khác là khi$d = 0$. Trong trường hợp đó, mỗi gói phải có số lượng đậu đỏ và đậu xanh chính xác bằng nhau. Điều này ngay lập tức buộc tổng số lượng cũng phải bằng nhau; nếu không thì sự mất cân bằng nào đó sẽ vẫn không thể giải quyết được. 

Quan sát quan trọng là các gói không làm thay đổi sự khác biệt tổng thể giữa tổng màu đỏ và màu xanh. Mỗi gói đóng góp một sự khác biệt cục bộ được giới hạn bởi$d$, nhưng vì mỗi gói phải chứa ít nhất một màu trong mỗi màu nên tổng số gói bị hạn chế bởi chồng nhỏ hơn. Điều này dẫn đến một tình trạng bất bình đẳng đơn giản về số lượng toàn cầu. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ cố gắng xây dựng các gói một cách rõ ràng. Chúng ta có thể liên tục tạo một gói tin hợp lệ bằng cách chọn một số$r_i \ge 1$Và$b_i \ge 1$như vậy$|r_i - b_i| \le d$, trừ đi tổng số cho đến khi không còn hạt đậu nào. Điều này nhanh chóng trở nên mơ hồ vì có nhiều lựa chọn hợp lệ ở mỗi bước và việc khám phá tất cả các khả năng sẽ dẫn đến sự phân nhánh theo cấp số nhân. Ngay cả một công trình tham lam cũng không đáng tin cậy vì những lựa chọn sớm có thể cản trở tính khả thi sau này. 

Sự đơn giản hóa cơ cấu quan trọng xuất phát từ việc quan sát những gì thực sự hạn chế tính khả thi. Mỗi gói phải chứa ít nhất một hạt đỏ và một hạt xanh, vì vậy nếu chúng ta tạo thành$k$gói tin thì chúng ta phải có$k \le r$Và$k \le b$. Như vậy$k \le \min(r,b)$. Ngoài ra, việc phân phối đậu trên các gói không thể làm giảm sự mất cân bằng tổng thể$r-b$; nó chỉ được phân vùng trên các gói. Sự mất cân bằng tồi tệ nhất trong bất kỳ gói nào ít nhất là sự mất cân bằng trung bình trải rộng trên các gói.$\frac{|r-b|}{k}$. Vì mỗi gói cho phép tối đa$d$, chúng tôi cần$$\frac{|r-b|}{k} \le d.$$Để thực hiện điều này dễ dàng nhất có thể, chúng tôi tối đa hóa$k$, vì vậy chúng tôi thiết lập$k = \min(r,b)$. Điều này đưa ra điều kiện cần và đủ:$$|r-b| \le d \cdot \min(r,b).$$Điều kiện này cũng đủ vì chúng ta luôn có thể phân phối các hạt một cách cân bằng, cho mỗi gói ít nhất một trong các giá trị nhỏ nhất.

---
title: "CF 103466I - Trạm vũ trụ"
description: "Chúng ta có một tập hợp các trạm vũ trụ được đánh số từ 0 đến n. Mỗi trạm có một giá trị năng lượng nguyên và trạm 0 là điểm bắt đầu của chúng tôi."
date: "2026-07-03T06:49:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103466
codeforces_index: "I"
codeforces_contest_name: "The 2019 ICPC Asia Nanjing Regional Contest"
rating: 0
weight: 103466
solve_time_s: 19
verified: false
draft: false
---

[CF 103466I - Trạm vũ trụ](https://codeforces.com/problemset/problem/103466/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các trạm vũ trụ được đánh số từ 0 đến n. Mỗi trạm có một giá trị năng lượng nguyên và trạm 0 là điểm bắt đầu của chúng tôi. Lúc đầu, chúng ta đã sở hữu năng lượng bằng giá trị của trạm 0 và bất cứ khi nào chúng ta ghé thăm một trạm, chúng ta sẽ vĩnh viễn nhận được năng lượng của trạm đó. 

Việc di chuyển bị hạn chế: chúng ta chỉ được phép di chuyển từ vị trí hiện tại đến một trạm có năng lượng không lớn hơn năng lượng mà chúng ta đang nắm giữ. Vì năng lượng chỉ tăng lên khi chúng ta ghé thăm các trạm, điều này có nghĩa là bất kỳ chuyển động nào cũng phải tuân theo một ngưỡng động không bao giờ giảm theo thời gian. 

Nhiệm vụ là đếm xem có bao nhiêu thứ tự khác nhau ghé thăm tất cả các trạm đúng một lần theo quy tắc này, bắt đầu từ trạm 0. Câu trả lời là cần có modulo 1e9 + 7. 

Hạn chế về cấu trúc chính là n có thể lớn tới 100000, trong khi năng lượng là các số nguyên rất nhỏ giới hạn từ 0 đến 50. Sự không khớp này ngay lập tức báo hiệu rằng giải pháp không thể phụ thuộc trực tiếp vào số lượng trạm theo phương pháp bậc hai hoặc bậc ba, mà thay vào đó phải nén các trạng thái bằng cách sử dụng phạm vi giá trị nhỏ của năng lượng. 

Một cách tiếp cận ngây thơ cố gắng mô phỏng tất cả các hoán vị có thể xảy ra rõ ràng sẽ thất bại vì ngay cả n = 20 cũng đã tạo ra 20! những khả năng, điều không thể thực hiện được. Ngay cả DP trên các tập hợp con cũng sẽ bùng nổ vượt quá 2^n. 

Trường hợp phức tạp xuất hiện khi nhiều trạm chia sẻ cùng một giá trị năng lượng. Ví dụ: nếu tất cả các trạm có năng lượng bằng nhau thì mọi hoán vị đều hợp lệ vì ràng buộc không bao giờ chặn chuyển động. Câu trả lời trong trường hợp đó phải là (n+1)! modulo M. Bất kỳ cách tiếp cận nào xử lý không chính xác các chuyển đổi năng lượng bằng nhau là bị hạn chế sẽ bị tính thiếu rất nhiều. 

Một trường hợp góc khác là khi trạm 0 có năng lượng tối đa. Khi đó tất cả các trạm khác phải có năng lượng ≤ a0, do đó, mọi thứ tự đều hợp lệ. Điều này kiểm tra xem thuật toán có xử lý chính xác thực tế là ngưỡng ban đầu đã bao gồm tất cả n đủ điều kiện hay không.

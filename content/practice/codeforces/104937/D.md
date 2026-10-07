---
title: "CF 104937D - Dãy số K-Tốt"
description: "Chúng ta có một chuỗi ban đầu a phải xuất hiện dưới dạng tiền tố của một chuỗi dài hơn b. Các giá trị trong b là các số nguyên từ 1 đến M. Sau tiền tố này, chúng ta được phép thêm nhiều phần tử một cách tự do."
date: "2026-06-28T18:15:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104937
codeforces_index: "D"
codeforces_contest_name: "MITIT 2024 Advanced Round"
rating: 0
weight: 104937
solve_time_s: 37
verified: false
draft: false
---

[CF 104937D - Chuỗi K-Good](https://codeforces.com/problemset/problem/104937/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi ban đầu`a`phải xuất hiện dưới dạng tiền tố của một chuỗi dài hơn`b`. Các giá trị trong`b`là các số nguyên giữa`1`Và`M`. Sau tiền tố này, chúng tôi được phép thêm nhiều phần tử một cách tự do. 

Một dãy được gọi là “K-good” nếu mỗi cặp phần tử liên tiếp khác nhau tối đa một`K`. Chúng ta không nhìn vào tất cả các dãy con của`b`, chỉ những dãy con có hiệu liền kề cũng thỏa mãn ràng buộc này. 

Hạn chế mang tính toàn cầu và mang tính đối nghịch: ở trình tự cuối cùng`b`, mọi dãy con K-tốt phải có độ dài tối đa`L`. Nói cách khác, không thể trích xuất một dãy con K-good dài, ngay cả khi chúng ta bỏ qua các phần tử. 

Nhiệm vụ là mở rộng`a`thành một chuỗi dài hơn`b`có độ dài tối đa có thể trong khi không bao giờ cho phép bất kỳ chuỗi con K-good nào vượt quá độ dài`L`. 

Khó khăn chính là các dãy con có thể bỏ qua nhiều phần tử tùy ý. Vì vậy ngay cả khi chúng ta tách các giá trị cách xa nhau trong`b`, một dãy con vẫn có thể chọn một “đường dẫn” đi qua chúng miễn là mỗi bước thay đổi nhiều nhất`K`. 

Các ràng buộc ngụ ý rằng một giải pháp phải gần tuyến tính cho mỗi trường hợp thử nghiệm. Với tối đa`2⋅10^5`kiểm tra và tổng số`N ≤ 4⋅10^5`, bất kỳ bậc hai hoặc thậm chí`O(N√N)`xây dựng cho mỗi bài kiểm tra là không thể. Chúng ta cần một đặc tính tham lam hoặc cấu trúc về cách hành xử của các chuỗi K-good. 

Trường hợp cạnh tinh tế xuất hiện khi tiền tố đã bão hòa giới hạn: 

Ví dụ, nếu`a = [1, 2, 3]`,`K = 1`,`L = 3`, thì bất kỳ tiện ích mở rộng nào cho phép tiếp tục như`4, 5`vẫn có thể không an toàn vì các chuỗi tiếp theo có thể bỏ qua và tạo thành chuỗi dài. Một cách tiếp cận ngây thơ chỉ kiểm tra những khác biệt liền kề trong`b`thất bại hoàn toàn vì ràng buộc là về các chuỗi con chứ không phải về chính chuỗi đó. 

Một trường hợp cạnh khác là khi`K = 0`. Khi đó, bất kỳ chuỗi con K-good nào cũng chỉ có thể bao gồm các giá trị bằng nhau, do đó ràng buộc trở thành giới hạn tần số trên mỗi giá trị. Nhiều giải pháp tham lam sẽ bị phá vỡ ở đây nếu chúng giả định sự kết nối giữa các giá trị. 

## Phương pháp tiếp cận 

Khó khăn chính là hiểu được chuỗi con K-good thực sự mã hóa điều gì. Nó không phải là cấu trúc tùy tiện; đó là một bước đi trên dòng số nguyên trong đó mỗi bước di chuyển nhiều nhất`K`. Điều này có nghĩa là một chuỗi con tương ứng với một chuỗi trong đó các giá trị được chọn liên tiếp nằm trong khoảng cách`K`. 

Nếu chúng ta coi các giá trị là các nút trên một dòng thì mọi giá trị đều kết nối với tất cả các giá trị trong`[x-K, x+K]`. Dãy con K-good chính xác là một đường dẫn trong biểu đồ ẩn này. Ràng buộc nói rằng không có đường dẫn nào có thể dài hơn`L`. 

Một cách giải thích vũ phu sẽ cố gắng hiểu

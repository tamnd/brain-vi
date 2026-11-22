---
title: "CF 328A - Kiểm tra IQ"
description: "Chúng ta được cấp chính xác bốn số nguyên trong một dãy, mỗi số từ 1 đến 1000. Mục đích là xác định xem dãy này tạo thành cấp số cộng hay cấp số nhân. Nếu đúng như vậy, chúng ta phải tính phần tử tiếp theo của cấp số nhân."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 328
codeforces_index: "A"
codeforces_contest_name: "Testing Round 8"
rating: 1800
weight: 328
solve_time_s: 71
verified: false
draft: false
---

[CF 328A - Kiểm tra IQ](https://codeforces.com/problemset/problem/328/A) 

**Đánh giá:** 1800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp chính xác bốn số nguyên trong một dãy, mỗi số từ 1 đến 1000. Mục đích là xác định xem dãy này tạo thành cấp số cộng hay cấp số nhân. Nếu đúng như vậy, chúng ta phải tính phần tử tiếp theo của cấp số nhân. Nếu chuỗi không khớp với mẫu nào hoặc nếu số tiếp theo không phải là số nguyên thì chúng ta trả về 42. 

Một cấp số cộng được xác định bởi sự khác biệt không đổi giữa các số hạng liên tiếp. Cho bốn phần tử$a_1, a_2, a_3, a_4$, sự khác biệt$a_2-a_1$,$a_3-a_2$, Và$a_4-a_3$tất cả đều phải bình đẳng. Một cấp số nhân được xác định bằng tỷ số không đổi giữa các số hạng liên tiếp. Đối với các phần tử khác 0$b_1, b_2, b_3, b_4$, các tỉ số$b_2/b_1$,$b_3/b_2$, Và$b_4/b_3$tất cả phải bằng nhau và tỷ lệ không được bằng 0 hoặc 1. 

Các ràng buộc rất nhỏ: chính xác là bốn số, mỗi số được giới hạn bởi 1 và 1000. Điều này có nghĩa là bất kỳ thuật toán nào kiểm tra sự khác biệt hoặc tỷ lệ bằng một vài phép tính số học sẽ chạy ngay lập tức. Sự tinh tế nằm ở các trường hợp đặc biệt, đặc biệt khi tỷ lệ không phải là số nguyên, chuỗi chứa các số lặp lại hoặc phép chia đơn giản gây ra lỗi dấu phẩy động. 

Một số trường hợp biên không rõ ràng bao gồm các chuỗi như [2, 2, 2, 2], cả số học và hình học, các chuỗi có hiệu số học là âm, các chuỗi như [1, 2, 4, 8] là các chuỗi hình học với tỷ lệ nguyên và các chuỗi như [1, 3, 9, 28], không khớp với chuỗi nào. Ngoài ra, các chuỗi như [1, 2, 4, 8] sẽ tạo ra phần tử tiếp theo là 16, nhưng chuỗi như [1, 2, 4, 9] sẽ trả về 42 vì tỷ lệ 9/4 không bằng các tỷ lệ trước đó. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: tính các hiệu liên tiếp cho số học và các tỷ số liên tiếp cho hình học. Kiểm tra xem tất cả sự khác biệt có bằng nhau hay tất cả các tỷ lệ đều bằng nhau. Nếu một giữ, tính phần tử tiếp theo. Nếu cả hai đều thất bại, trả về 42. Điều này hoạt động trong thời gian không đổi cho bốn phần tử. 

Việc triển khai đơn giản phải xử lý số học số nguyên một cách cẩn thận. Đối với cấp số nhân, phép chia phải chính xác. Việc sử dụng phép chia dấu phẩy động rất rủi ro vì 3/2 trong Python là 1,5 và việc kiểm tra đẳng thức có thể không thành công do làm tròn. Chúng ta phải kiểm tra xem mỗi phép chia có tạo ra một số nguyên hay không trước khi so sánh

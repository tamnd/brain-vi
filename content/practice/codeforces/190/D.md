---
title: "CF 190D - Mật mã không bí mật"
description: "Chúng ta được cho một dãy số nguyên và giá trị ngưỡng $k$. Nhiệm vụ là đếm xem có bao nhiêu mảng con liền kề chứa một số giá trị xuất hiện ít nhất $k$ lần bên trong mảng con đó."
date: "2026-06-03T01:19:49+07:00"
tags: ["codeforces", "competitive-programming", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 190
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 120 (Div. 2)"
rating: 1900
weight: 190
solve_time_s: 24
verified: false
draft: false
---

[CF 190D - Cypher không bí mật](https://codeforces.com/problemset/problem/190/D) 

**Xếp hạng:** 1900 
**Thẻ:** hai con trỏ 
**Thời gian giải:** 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên và một giá trị ngưỡng$k$. Nhiệm vụ là đếm xem có bao nhiêu mảng con liền kề chứa một số giá trị xuất hiện ít nhất$k$times inside that subarray.

 Tương tự, đối với mỗi phân đoạn của mảng, chúng tôi đang kiểm tra xem có tồn tại phần tử có tần số bên trong phân đoạn đó đạt tới$k$và chúng tôi muốn đếm tất cả các phân đoạn nơi điều này xảy ra. 

Các ràng buộc rất lớn:$n \le 4 \cdot 10^5$. Bất kỳ cách tiếp cận nào kiểm tra tất cả các mảng con một cách rõ ràng đều ngay lập tức quá chậm vì số lượng mảng con quá lớn.$O(n^2)$, đạt khoảng$8 \cdot 10^{10}$trong trường hợp xấu nhất. Ngay cả việc quét tuyến tính trên mỗi mảng con cũng không thể thực hiện được. Điều này buộc chúng ta phải áp dụng chiến lược đếm hai con trỏ tuyến tính hoặc gần tuyến tính hoặc ngoại tuyến. 

Trường hợp cạnh tinh tế xuất hiện khi$k = 1$. In this case every subarray is valid, because any element already appears at least once. Ví dụ, đối với mảng$[5, 7, 5]$, mọi mảng con đều đủ điều kiện và câu trả lời là$\frac{n(n+1)}{2} = 6$. Một cách triển khai đơn giản giả định rằng nó phải theo dõi các lần xuất hiện lặp lại có thể vẫn hoạt động, nhưng nó thường làm phức tạp thêm trường hợp tầm thường này. 

Một trường hợp góc khác xảy ra khi tất cả các phần tử đều khác biệt và$k \ge 2$. Ví dụ,$[1, 2, 3, 4]$với$k = 2$có câu trả lời$0$. Nhiều sai sót

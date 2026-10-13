---
title: "CF 105167J - Sự trì hoãn quá nhiều"
description: "Chúng ta được cung cấp một dãy các số nguyên riêng biệt, biểu thị mức nhiệt của các giá đỡ máy chủ được sắp xếp thành một hàng. Chúng tôi được phép sắp xếp lại mảng này bằng cách sử dụng các hoán đổi liền kề, trong đó mỗi hoán đổi trao đổi các phần tử lân cận."
date: "2026-06-27T10:36:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105167
codeforces_index: "J"
codeforces_contest_name: "ETH Zurich Competitive Programming Contest Spring 2024"
rating: 0
weight: 105167
solve_time_s: 17
verified: false
draft: false
---

[CF 105167J - Trì hoãn quá nhiều](https://codeforces.com/problemset/problem/105167/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 17s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy các số nguyên riêng biệt, biểu thị mức nhiệt của các giá đỡ máy chủ được sắp xếp thành một hàng. Chúng tôi được phép sắp xếp lại mảng này bằng cách sử dụng các hoán đổi liền kề, trong đó mỗi hoán đổi trao đổi các phần tử lân cận. 

Sau khi sắp xếp lại, chúng tôi đánh giá chất lượng cấu hình bằng cách tổng hợp sự khác biệt tuyệt đối giữa từng cặp phần tử liền kề. Về mặt hình thức, đối với một hoán vị$b$, điểm số là$$\sum_{i=2}^{n} |b_{i-1} - b_i|.$$Nhiệm vụ có hai phần cho mỗi trường hợp thử nghiệm. Đầu tiên, chúng ta phải xác định giá trị lớn nhất có thể có của điểm này trên tất cả các hoán vị của mảng. Thứ hai, trong số tất cả các hoán vị đạt được mức tối đa này, chúng ta phải tính số lượng hoán đổi liền kề tối thiểu cần thiết để đạt được một cách sắp xếp tối ưu như vậy. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$trường hợp thử nghiệm với tổng số$n \le 4 \cdot 10^5$. Điều này buộc một giải pháp cơ bản tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào cố gắng hoán vị hoặc mô phỏng hoán đổi một cách rõ ràng sẽ thất bại vì ngay cả (O(n^2)\

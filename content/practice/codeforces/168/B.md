---
title: "CF 168B - Pháp sư và phép thuật tối thiểu"
description: "Chúng tôi được cung cấp toàn bộ tệp văn bản đại diện cho một câu thần chú. Câu thần chú bao gồm các dòng và mỗi dòng thuộc một trong hai loại. Một dòng được khuếch đại nếu ký tự không phải dấu cách đầu tiên của nó là . Đối với những dòng này, khoảng trắng có ý nghĩa và phải giữ nguyên chính xác như khi chúng xuất hiện."
date: "2026-06-02T16:55:02+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 168
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 114 (Div. 2)"
rating: 1700
weight: 168
solve_time_s: 27
verified: false
draft: false
---

[CF 168B - Pháp sư và phép thuật tối thiểu](https://codeforces.com/problemset/problem/168/B) 

**Đánh giá:** 1700 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp toàn bộ tệp văn bản đại diện cho một câu thần chú. Câu thần chú bao gồm các dòng và mỗi dòng thuộc một trong hai loại. 

Một dòng được **khuếch đại** nếu ký tự không phải dấu cách đầu tiên của nó là`#`. Đối với những dòng này, khoảng trắng có ý nghĩa và phải giữ nguyên chính xác như khi chúng xuất hiện. Các đường khuếch đại cũng đóng vai trò là rào cản ngăn các đường lân cận được hợp nhất qua chúng. 

Mọi dòng khác đều **không khuếch đại**. Trong những dòng như vậy, khoảng trắng không có ý nghĩa gì và phải được loại bỏ. Hơn nữa, các dòng mới giữa các dòng không khuếch đại liên tiếp là không cần thiết, vì vậy những dòng đó phải được nối với nhau thành một dòng đầu ra duy nhất. 

Nhiệm vụ là xóa bao nhiêu dấu cách và ngắt dòng theo quy định cho phép mà vẫn giữ được ý nghĩa của câu thần chú. 

Tổng kích thước đầu vào tối đa là$2^{20}$byte, khoảng một triệu ký tự. Điều đó ngay lập tức gợi ý rằng bất kỳ giải pháp nào cũng phải xử lý văn bản về cơ bản một lần. MỘT$O(n)$quét qua tất cả các ký tự đủ nhanh một cách dễ dàng, trong khi việc xây dựng lại các chuỗi lớn nhiều lần có thể trở nên tốn kém nếu thực hiện bất cẩn

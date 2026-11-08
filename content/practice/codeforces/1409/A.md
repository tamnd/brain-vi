---
title: "CF 1409A - Một bài toán về hai số nguyên khác"
description: "Chúng ta được cho hai số nguyên và chúng ta muốn chuyển đổi số thứ nhất thành số thứ hai bằng cách sử dụng một chuỗi các bước di chuyển. Mỗi lần di chuyển cho phép chúng ta chọn bất kỳ kích thước bước số nguyên nào từ 1 đến 10 và cộng hoặc trừ nó khỏi giá trị hiện tại."
date: "2026-06-11T07:33:39+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1409
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 667 (Div. 3)"
rating: 800
weight: 1409
solve_time_s: 90
verified: false
draft: false
---

[CF 1409A - Lại một bài toán về hai số nguyên](https://codeforces.com/problemset/problem/1409/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai số nguyên và chúng ta muốn chuyển đổi số thứ nhất thành số thứ hai bằng cách sử dụng một chuỗi các bước di chuyển. Mỗi lần di chuyển cho phép chúng ta chọn bất kỳ kích thước bước số nguyên nào từ 1 đến 10 và cộng hoặc trừ nó khỏi giá trị hiện tại. 

Quá trình này về cơ bản là một bước đi trên dòng số nguyên. Bắt đầu ở vị trí`a`, chúng tôi muốn đạt được vị trí`b`. Mỗi bước sẽ di chuyển chúng ta sang trái hoặc phải một khoảng từ 1 đến 10 và chúng ta muốn giảm thiểu số bước như vậy. 

Số lượng quan trọng là sự khác biệt tuyệt đối giữa các con số. Hướng đi không quan trọng, chỉ là họ cách nhau bao xa. 

Các ràng buộc đủ lớn để chúng ta không thể mô phỏng mọi chuỗi di chuyển có thể xảy ra. Với tối đa 2 × 10^4 trường hợp thử nghiệm và giá trị lên tới 10^9, bất kỳ giải pháp nào khám phá các trạng thái hoặc cố gắng tìm kiếm tham lam các khả năng sẽ quá chậm. Chúng ta cần tính toán theo thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ là cho rằng chúng ta phải luôn sử dụng những bước lớn một cách tham lam mà không có cấu trúc. Ví dụ: người ta có thể thử trừ đi 10 nhiều lần cho đến khi gần đạt mục tiêu và sau đó sửa phần còn lại. Điều này có hiệu quả nhưng phải được giải thích cẩn thận vì việc vượt quá được cho phép và đôi khi có thể có lợi. 

Một sự hiểu lầm đơn giản hơn là coi vấn đề này giống như vấn đề hạn chế đổi tiền xu ở nhiều tiểu bang. Điều đó sẽ gây ra sự phức tạp không cần thiết khi cấu trúc thực sự sụp đổ thành một đối số chia đơn giản. 

## Phương pháp tiếp cận 

Cách giải thích brute-force là xem đây là bài toán đường đi ngắn nhất trên các số nguyên, trong đó mỗi nút`x`kết nối với tối đa 20 hàng xóm:`x + k`Và`x - k`vì`k`từ 1 đến 10. BFS từ`a`ĐẾN`b`sẽ luôn tìm được số lần di chuyển tối thiểu vì mỗi cạnh có trọng số bằng nhau. Tuy nhiên, phạm vi giá trị lên tới 10^9, khiến không gian trạng thái trở nên lớn về mặt thiên văn. Ngay cả việc khám phá một phần nhỏ cũng không thể thực hiện được vì đồ thị về cơ bản là vô hạn đối với các mục đích thực tế. 

các

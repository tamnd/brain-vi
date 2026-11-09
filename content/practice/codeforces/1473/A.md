---
title: "CF 1473A - Thay thế các bộ phận"
description: "Chúng ta được cung cấp một mảng các số nguyên dương và một giá trị ngưỡng, $d$. Trong một thao tác, chúng ta có thể chọn bất kỳ phần tử nào và thay thế nó bằng tổng của hai phần tử riêng biệt bất kỳ."
date: "2026-06-11T00:27:37+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1473
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 102 (Rated for Div. 2)"
rating: 800
weight: 1473
solve_time_s: 164
verified: false
draft: false
---

[CF 1473A - Thay thế các phần tử](https://codeforces.com/problemset/problem/1473/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện, toán học, sắp xếp 
**Thời gian giải:** 2m 44s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên dương và một giá trị ngưỡng,$d$. Trong một thao tác, chúng ta có thể chọn bất kỳ phần tử nào và thay thế nó bằng tổng của hai phần tử riêng biệt bất kỳ. Mục đích là để xác định xem, sau một số chuỗi thao tác, mọi phần tử của mảng có thể nhỏ hơn hoặc bằng$d$. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp đưa ra mảng và$d$, và chúng ta phải xuất ra CÓ hoặc KHÔNG cho mỗi trường hợp. 

Với những hạn chế,$n$có thể lên tới 100,$d$lên tới 100 và mỗi phần tử lên tới 100. Điều này cho chúng ta biết rằng mô phỏng thô sơ của mọi chuỗi hoạt động có thể chỉ khả thi đối với các mảng nhỏ; tổng số hoạt động tiềm năng là tổ hợp trong$n$, do đó việc liệt kê tất cả các chuỗi sẽ rất lớn về mặt thiên văn. Vì vậy, chúng ta phải tìm một thuộc tính cho phép chúng ta đưa ra quyết định mà không cần mô phỏng mọi thao tác. 

Trường hợp cạnh không rõ ràng xảy ra khi hầu hết các phần tử đều ở trên$d$nhưng hai phần tử nhỏ nhất đủ nhỏ để tổng của chúng không vượt quá$d$. Ví dụ, hãy xem xét$a = [1, 2, 5]$với$d = 3$. Phần tử lớn nhất ở trên$d$, nhưng hai tổng nhỏ nhất bằng$3$, do đó thay thế số lớn nhất bằng tổng của chúng sẽ mang lại tất cả các phần tử$\le d$. Một cách tiếp cận bất cẩn có thể chỉ kiểm tra phần tử tối đa và không nhận thấy sự chuyển đổi tiềm năng này. 

Một trường hợp cạnh khác là khi tất cả các phần tử ban đầu đều ở bên dưới$d$, trong trường hợp đó không cần thực hiện thao tác nào. Ngược lại, nếu tổng của hai phần tử nhỏ nhất lớn hơn$d$, không có thao tác nào có thể rút gọn các phần tử lớn hơn nên câu trả lời ngay là KHÔNG. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử lặp đi lặp lại mọi sự kết hợp của ba chỉ số, thay thế các phần tử và kiểm tra xem liệu tất cả các phần tử có nằm dưới mức đó hay không$d$. Mặc dù đúng về nguyên tắc nhưng tốc độ này quá chậm vì mỗi bước có$O(n^3)$các lựa chọn và chúng ta có thể cần phải lặp lại các thao tác nhiều lần. Vì$n = 100$, ngay cả một lần lặp cũng sẽ cần tới$10^6$các bước và trình tự có thể dài tùy ý. 

Quan sát quan trọng là chỉ có hai phần tử nhỏ nhất mới có tác dụng khử bất kỳ phần tử lớn hơn nào. Bất kỳ phần tử nào lớn hơn$d$chỉ có thể thay thế bằng tổng của hai phần tử khác. Tổng nhỏ nhất có thể là tổng của hai phần tử nhỏ nhất. Nếu tổng này nhỏ hơn hoặc bằng$d$, chúng ta luôn có thể thay thế bất kỳ phần tử lớn nào bằng tổng đó.

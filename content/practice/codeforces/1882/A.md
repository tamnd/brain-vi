---
title: "CF 1882A - Dãy số tăng dần"
description: "Chúng ta cần xây dựng một chuỗi mới $b$ có độ dài $n$. Mọi phần tử của $b$ phải là số nguyên dương. Dãy số phải tăng dần và với mọi vị trí $i$, chúng ta không được chọn $bi = ai$."
date: "2026-06-08T22:34:43+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1882
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 899 (Div. 2)"
rating: 800
weight: 1882
solve_time_s: 42
verified: false
draft: false
---

[CF 1882A - Trình tự tăng dần](https://codeforces.com/problemset/problem/1882/A) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một chuỗi mới$b$chiều dài$n$. 

Mỗi phần tử của$b$phải là số nguyên dương. Trình tự phải tăng dần và với mọi vị trí$i$, chúng tôi bị cấm chọn$b_i = a_i$. 

Trong số tất cả các chuỗi thỏa mãn các quy tắc này, chúng ta muốn giá trị nhỏ nhất có thể có của phần tử cuối cùng$b_n$. 

Đầu vào đưa ra một số trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được mảng$a$và chúng ta phải xác định giá trị cuối cùng tối thiểu có thể đạt được. 

Những hạn chế là rất nhỏ. Mỗi test case có tối đa 100 phần tử và có tối đa 100 test case. Thậm chí một$O(n^2)$giải pháp sẽ hoàn toàn ổn. Điều này có nghĩa là chúng ta nên tập trung vào việc tìm kiếm chiến lược đúng đắn đơn giản nhất thay vì lo lắng về việc tối ưu hóa nâng cao. 

Phần khó khăn là việc chọn một giá trị quá lớn ở một số vị trí có thể buộc tất cả các giá trị sau này cũng trở nên lớn hơn. Vì chúng ta chỉ quan tâm đến việc giảm thiểu phần tử cuối cùng nên mọi vị trí phải được gán giá trị nhỏ nhất để giữ cho chuỗi hợp lệ. 

Một lỗi phổ biến là chỉ tránh giá trị bị cấm$a_i$và quên rằng dãy số phải luôn tăng một cách nghiêm ngặt. 

Ví dụ:```
n = 2
a = [1, 2]
```Lựa chọn$b_1 = 2$tránh$a_1$, nhưng sau đó$b_2$phải lớn hơn 2 và khác 2 nên đáp án là 3. Lựa chọn tốt hơn là$b_1 = 2$,$b_2 = 3$, vẫn cho kết quả 3, nhưng ví dụ này cho thấy mọi lựa chọn đều ảnh hưởng đến vị trí trong tương lai. 

Một sai lầm dễ mắc phải khác là bắt đầu từ 0 và chỉ tăng liên tục khi gặp giá trị bị cấm. 

Ví dụ:```
n = 1
a = [1]
```Số nguyên dương nhỏ nhất là 1 nhưng bị cấm. Câu trả lời đúng là 2. 

Việc triển khai bất cẩn mà bỏ qua tính tích cực có thể xuất ra 0 hoặc 1 không chính xác. 

Một trường hợp tinh tế hơn xảy ra khi giá trị hợp lệ nhỏ nhất tiếp theo bằng$a_i$.```
n = 3
a = [1, 3, 4]
```Trình tự tối ưu là:```
b = [2, 4, 5]
```Ở vị trí 2, giá trị nhỏ nhất lớn hơn phần tử trước đó là 3, nhưng 3 bị cấm nên ta phải sk

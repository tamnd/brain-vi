---
title: "CF 279D - Số lượng biến tối thiểu"
description: "Chúng ta được cho một dãy các số nguyên dương phân biệt. Trình tự không phải là tùy ý, nó phải được tạo từng bước bằng cách sử dụng tập hợp các biến. Lúc đầu, mỗi biến đều chứa 0. Giá trị đầu tiên của chuỗi là đặc biệt."
date: "2026-06-05T05:53:28+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp"]
categories: ["algorithms"]
codeforces_contest: 279
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 171 (Div. 2)"
rating: 2200
weight: 279
solve_time_s: 157
verified: false
draft: false
---

[CF 279D - Số lượng biến tối thiểu](https://codeforces.com/problemset/problem/279/D) 

**Đánh giá:** 2200 
**Thẻ:** bitmask, dp 
**Thời gian giải:** 2m 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các số nguyên dương phân biệt. Trình tự không phải là tùy ý, nó phải được tạo từng bước bằng cách sử dụng tập hợp các biến. 

Lúc đầu mỗi biến đều chứa`0`. 

Giá trị đầu tiên của chuỗi là đặc biệt. Chúng tôi có thể viết`a₁`trực tiếp vào bất kỳ biến nào. 

Đối với mọi giá trị sau này`a_t`, chúng ta phải chọn hai biến, đọc nội dung hiện tại của chúng, cộng chúng và gán kết quả cho một biến nào đó. Giá trị tạo ra ở bước`t`phải chính xác`a_t`. 

Các biến có thể được sử dụng lại. Việc gán giá trị mới cho một biến sẽ ghi đè lên bất cứ giá trị nào được lưu trữ ở đó trước đó. 

Câu hỏi đặt ra không phải là liệu trình tự có thể được tạo ra hay không. Chúng ta phải tìm số lượng biến nhỏ nhất để có thể thực hiện được toàn bộ quá trình. Nếu không có số lượng biến nào hoạt động, câu trả lời là`-1`. 

Ràng buộc làm thay đổi mọi thứ là`n ≤ 23`. Bản thân các giá trị có thể lớn bằng`10^9`, vì vậy chúng tôi không thể xây dựng DP dựa trên các con số. Giá trị nhỏ của`n`gợi ý mạnh mẽ rằng mọi giá trị được tạo ra phải được xử lý như một đối tượng riêng lẻ và được biểu thị bằng một bit trong mặt nạ. 

Một quan sát tinh tế là mọi giá trị từng được lưu trữ trong một biến đều`0`hoặc một trong các phần tử trình tự đã được tạo ra. Vì tất cả các giá trị chuỗi đều khác biệt và dương nên một giá trị không bao giờ có thể được tạo lại một cách ngẫu nhiên dưới dạng phần tử chuỗi sau này. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Coi như```
2
1 5
```Giá trị thứ hai phải được lấy dưới dạng tổng của các giá trị đã tồn tại. Giá trị dương duy nhất có sẵn là`1`, vậy tổng duy nhất có thể là`1+1=2`hoặc`1+0=1`. sản xuất`5`là không thể, vì vậy câu trả lời đúng là`-1`. 

Một cái bẫy khác là cho phép một giá trị được sử dụng hai lần trong cùng một tổng.```
2
3 6
```Sau khi sản xuất`3`, chúng ta có thể tính toán`6 = 3 + 3`. Tuyên bố cho phép`i = j`, vì vậy chỉ có một bản sao được lưu trữ của`3`là cần thiết. Việc triển khai yêu cầu hai biến khác nhau sẽ từ chối trường hợp này một cách không chính xác. 

Cái bẫy thứ ba là quên rằng việc gán một giá trị mới sẽ ghi đè nhiều nhất một giá trị hiện có.```
3
1 2 3
```Sau khi sản xuất`2`, chúng ta có thể ghi đè lên biến đang giữ`1`khi tạo`3`, nhưng chúng tôi không thể xóa một số giá trị tại

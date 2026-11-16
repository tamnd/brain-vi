---
title: "CF 190C - STL"
description: "Chúng ta được cung cấp một chuỗi các mã thông báo được đọc theo thứ tự, mỗi mã thông báo là int hoặc cặp. Các mã thông báo này được cho là tạo thành một biểu thức kiểu lồng nhau hợp lệ của một ngữ pháp rất cụ thể."
date: "2026-06-03T01:19:24+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar"]
categories: ["algorithms"]
codeforces_contest: 190
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 120 (Div. 2)"
rating: 1500
weight: 190
solve_time_s: 43
verified: false
draft: false
---

[CF 190C - STL](https://codeforces.com/problemset/problem/190/C) 

**Đánh giá:** 1500 
**Thẻ:** dfs và tương tự 
**Thời gian giải:** 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các mã thông báo được đọc theo thứ tự, mỗi mã thông báo là một trong hai`int`hoặc`pair`. Các mã thông báo này được cho là tạo thành một biểu thức kiểu lồng nhau hợp lệ của một ngữ pháp rất cụ thể. Ngữ pháp hoàn toàn nhị phân: mọi`int`là một loại lá, và mọi`pair`kết hợp chính xác hai loại đã hình thành trước đó thành một loại mới. 

Nhiệm vụ là xác định xem liệu chúng ta có thể chèn dấu ngoặc nhọn và dấu phẩy để chuỗi trở thành loại có cấu trúc chính xác hay không và nếu có thì hãy xây dựng lại cấu trúc chính xác đó. Nếu có thể có nhiều cấu trúc hợp lệ thì chúng ta sẽ có sự mơ hồ, nhưng vấn đề đảm bảo tính duy nhất bất cứ khi nào có một giải pháp. 

Điều này ngay lập tức gợi ý rằng chúng ta đang phân tích cú pháp cấu trúc cây nhị phân từ một luồng giống như đặt hàng trước trong đó mỗi nút bên trong sử dụng chính xác hai nút con. Khó khăn là đầu vào không có cấu trúc rõ ràng, vì vậy chúng ta phải suy ra việc phân nhóm hoàn toàn từ chuỗi. 

Ràng buộc`n ≤ 10^5`(Ở đâu`n`là số lượng`int`token) ngụ ý tổng số lượng token cũng là`O(n)`, vì vậy mọi nghiệm đều phải tuyến tính hoặc gần tuyến tính. Phương pháp phân tích cú pháp bậc hai hoặc quay lui sẽ bùng nổ vì mọi quyết định về vị trí có thể phân nhánh thành nhiều cách diễn giải đệ quy. 

Một trường hợp phức tạp xuất hiện khi cấu trúc không thể thực hiện được mặc dù số lượng khớp một cách hời hợt. Ví dụ: một chuỗi như`int pair`đã vi phạm ngữ pháp vì một kiểu hoàn chỉnh phải bắt đầu bằng một cấu trúc có thể mở rộng chứ không được kết thúc sớm. Một trường hợp thất bại khác là khi chúng ta có quá nhiều`int`mã thông báo sớm, không còn chỗ cho việc chờ xử lý`pair`mở rộng. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ cố gắng mô phỏng tất cả các cách có thể để gán dấu ngoặc đơn xung quanh chuỗi, về cơ bản là thử mọi hình dạng cây nhị phân có lá`int`mã thông báo và các nút nội bộ là`pair`. Mỗi`pair`tiêu thụ hai cây con, do đó, điều này tương đương với việc liệt kê tất cả các cấu trúc cây nhị phân đầy đủ trên các vị trí chuỗi và kiểm tra xem chuỗi mã thông báo có khớp với nhãn hợp lệ hay không. 

Số lượng các cấu trúc như vậy tăng theo cấp số nhân. Ngay cả với độ dài vừa phải, vụ nổ số Catalan khiến điều này không thể thực hiện được. Sự kém hiệu quả cốt lõi là chúng ta liên tục xem xét lại các hậu tố giống nhau của chuỗi dưới các hình dạng cây từng phần khác nhau. 

Quan sát quan trọng là ngữ pháp có tính quyết định nếu chúng ta hiểu nó như một vấn đề xây dựng luồng. Mọi

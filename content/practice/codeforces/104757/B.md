---
title: "CF 104757B - Dải đường B"
description: "Chúng tôi được cung cấp hai nhóm địa điểm cho khách hàng, mỗi nhóm nằm trên một con đường thẳng tắp riêng biệt. Các con đường song song và có một khoảng cách cố định theo chiều dọc giữa chúng."
date: "2026-06-28T22:47:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104757
codeforces_index: "B"
codeforces_contest_name: "2023-2024 ICPC East North America Regional Contest (ECNA 2023)"
rating: 0
weight: 104757
solve_time_s: 42
verified: false
draft: false
---

[CF 104757B - B Road Band](https://codeforces.com/problemset/problem/104757/B)

 **Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai nhóm địa điểm cho khách hàng, mỗi nhóm nằm trên một con đường thẳng tắp riêng biệt. Các con đường song song và có một khoảng cách cố định theo chiều dọc giữa chúng. Một số điểm truy cập không dây phải được đặt, nhưng tất cả chúng phải nằm trên đường trung điểm giữa hai con đường, tạo thành một đường ngang duy nhất có thể đặt được. 

Mỗi khách hàng kết nối với điểm truy cập gần nhất và chi phí phục vụ một khách hàng là bình phương khoảng cách Euclide đến điểm truy cập gần nhất đó. Mục tiêu là đặt các điểm truy cập và chỉ định khách hàng đến điểm gần nhất sao cho tổng bình phương khoảng cách trên tất cả khách hàng càng nhỏ càng tốt. 

Each customer is therefore defined by a horizontal position and a fixed vertical offset to the center line. Phần dọc của mọi khoảng cách đều giống nhau đối với tất cả các điểm truy cập, do đó, việc tối ưu hóa có ý nghĩa duy nhất diễn ra dọc theo trục ngang, nơi chúng tôi phân cụm các điểm trên một đường thành k nhóm một cách hiệu quả. 

Các ràng buộc chỉ ra tổng số khoảng hai nghìn khách hàng và nhiều nhất là một trăm điểm truy cập. A solution that tries all ways to assign customers to centers would explode combinatorially. Ngay cả một chương trình động đơn giản với chuyển tiếp bậc ba cũng sẽ quá chậm, do đó cấu trúc của hàm chi phí phải được khai thác. 

A subtle issue is that every squared distance includes a constant vertical component. If ignored, the answer will be wrong by a fixed offset. Một sai lầm phổ biến khác là coi vấn đề là cần phương tiện k 2D tùy ý; hình học thu gọn về một chiều sau khi tách phần đóng góp theo chiều dọc không đổi. 

## Phương pháp tiếp cận 

A direct interpretation is to consider every way of placing k centers and assigning each customer to its nearest one. Even if we fix center positions, deciding assignments remains coupled with those positions, and the search space is continuous. Điều này làm cho vũ lực về cơ bản là không thể thực hiện được. 

Điều quan trọng cần lưu ý là tất cả các điểm truy cập đều nằm trên cùng một đường ngang và khoảng cách theo chiều dọc của mọi khách hàng đến đường đó là cố định. Khoảng cách bình phương từ một khách hàng ở vị trí nằm ngang x đến tâm ở vị trí a trở thành tổng của một số hạng không đổi từ khoảng cách dọc và một số hạng nằm ngang (x − a) 2. Vì phần hằng số không phụ thuộc vào phép gán nên nó có thể được thêm vào cuối. 

Điều này làm giảm vấn đề đặt k tâm trên một đường thẳng để giảm thiểu tổng bình phương độ lệch của các điểm so với tâm gần nhất của chúng. Khi các điểm được sắp xếp theo tọa độ x, mọi giải pháp tối ưu đều phân chia chúng thành k đoạn liền kề. Mỗi phân đoạn được phục vụ bởi một trung tâm được đặt ở vị trí trung bình của các điểm của nó, vì điều đó giảm thiểu sai số bình phương trong phân khúc. 

Vì vậy, nhiệm vụ trở thành một vấn đề phân đoạn k 1D cổ điển: phân chia mảng đã sắp xếp thành k khối liền kề để giảm thiểu tổng độ lệch bình phương so với giá trị trung bình của mỗi khối. 

Chúng tôi tính toán chi phí của bất kỳ phân khúc nào một cách hiệu quả bằng cách sử dụng tổng tiền tố x và x². Sau đó, chúng tôi sử dụng quy hoạch động trong đó dp[t][i] biểu thị chi phí tối thiểu để trang trải i điểm đầu tiên với cụm t. A direct transition over all previous split points gives O(k n²), which is too slow in the worst case.

 Cấu trúc của hàm chi phí đảm bảo các điểm phân chia tối ưu là đơn điệu, do đó áp dụng tối ưu hóa chia để trị. Điều này làm giảm DP xuống O(k n log n), đủ nhanh cho n lên tới 2000 và k lên tới 100. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân công vũ lực | Hàm mũ | Hàm mũ | Quá chậm | 
| DP không tối ưu hóa | O(k n2) | O(k n) | Quá chậm | 
| DP được tối ưu hóa (phân chia và chinh phục) | O(k n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên chúng tôi hợp nhất tất cả các tùy chỉnh

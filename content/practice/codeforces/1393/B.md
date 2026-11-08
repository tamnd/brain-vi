---
title: "CF 1393B - Applejack và Kho lưu trữ"
description: "Applejack muốn xây dựng hai nhà kho bằng ván từ nhà kho: một hình vuông và một hình chữ nhật. Mỗi cạnh của kho sử dụng chính xác một tấm ván, vì vậy, hình vuông cần bốn tấm ván có cùng chiều dài và hình chữ nhật cần hai cặp tấm ván có chiều dài bằng nhau."
date: "2026-06-11T09:58:05+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1393
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 662 (Div. 2)"
rating: 1400
weight: 1393
solve_time_s: 314
verified: false
draft: false
---

[CF 1393B - Applejack và Kho lưu trữ](https://codeforces.com/problemset/problem/1393/B) 

**Đánh giá:** 1400 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, tham lam, triển khai 
**Thời gian giải:** 5 phút 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Applejack muốn xây dựng hai nhà kho bằng ván từ nhà kho: một hình vuông và một hình chữ nhật. Mỗi cạnh của kho sử dụng chính xác một tấm ván, vì vậy, hình vuông cần bốn tấm ván có cùng chiều dài và hình chữ nhật cần hai cặp tấm ván có chiều dài bằng nhau. Cô ấy nhận được một luồng cập nhật từ nhà kho, thêm một tấm ván có độ dài nhất định hoặc loại bỏ một tấm ván. Sau mỗi lần cập nhật, chúng ta cần xác định xem có thể chọn các tấm ván để xây dựng đồng thời cả hình vuông và hình chữ nhật hay không. 

Dữ liệu đầu vào cung cấp số lượng ván ban đầu và độ dài của chúng, theo sau là một loạt sự kiện. Đầu ra là một chuỗi các phản hồi "CÓ" hoặc "KHÔNG", một phản hồi cho mỗi sự kiện, cho biết liệu bộ ván hiện tại có cho phép xây dựng cả hai kho hay không. 

Những hạn chế rất đáng kể: lên đến$10^5$ván và$10^5$sự kiện, với chiều dài tấm ván lên đến$10^5$. Điều này loại trừ bất kỳ cách tiếp cận nào lặp qua tất cả các bảng cho mỗi truy vấn, vì điều đó sẽ dẫn đến$O(n \cdot q)$phức tạp, có thể đạt tới$10^{10}$hoạt động. Chúng ta cần một phương pháp duy trì số lượng hiệu quả và tính khả thi của truy vấn trong thời gian không đổi. 

Các trường hợp khó khăn không rõ ràng phát sinh từ việc sử dụng ván chồng chéo. Ví dụ: có chính xác bốn tấm ván có chiều dài bằng nhau và hai tấm ván dài khác cho phép xây dựng cả hình vuông và hình chữ nhật, nhưng việc kiểm tra đơn giản về "bốn tấm ván bất kỳ + hai cặp bất kỳ" có thể giả định sai nếu nó không tính đến các tấm ván chồng lên nhau. Sự phân bố thưa thớt cũng có vấn đề: có nhiều chiều dài mà mỗi tấm chỉ có một hoặc hai tấm ván là không đủ, ngay cả khi tổng số tấm ván cao. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ lưu trữ tất cả các tấm ván trong một tập hợp nhiều tập hợp và, đối với mỗi sự kiện, cố gắng tạo thành tất cả các kết hợp có thể có của bốn và hai cặp để kiểm tra xem có thể tạo được hình vuông và hình chữ nhật hay không. Điều này đúng về mặt logic nhưng quá chậm: việc kiểm tra tất cả các kết hợp là$O(n^2)$hoặc tệ hơn cho mỗi truy vấn. 

Cách tiếp cận tối ưu tận dụng thực tế là chúng ta chỉ quan tâm đến số lượng ván có ít nhất là hai (đối với cạnh hình chữ nhật) hoặc bốn (đối với hình vuông). Chúng ta có thể duy trì hai bộ đếm: một bộ đếm cho số chiều dài tấm ván có ít nhất hai tấm ván (`pairs`) và một cái khác cho số chiều dài có ít nhất bốn tấm ván (`quads`). Mỗi bản cập nhật sẽ tăng hoặc giảm các bộ đếm này khi số lượng ván vượt qua các ngưỡng. Sau mỗi lần cập nhật, chúng tôi có thể xác định tính khả thi bằng một điều kiện đơn giản: 

1. Phải có ít nhất một`quad`để tạo thành hình vuông. 
2. Phải có ít nhất hai`pairs`tổng cộng, nhưng một`pair`có thể đến từ`quad`dùng cho hình vuông. 

Điều này tránh việc kiểm tra từng plan riêng lẻ và giảm từng truy vấn xuống mức cập nhật và kiểm tra liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) mỗi truy vấn | O(n) | Quá chậm | 
| Tối ưu | O(1) mỗi truy vấn | O(1e5) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng tần số`count`được lập chỉ mục theo chiều dài tấm ván để lưu trữ số lượng tấm ván cho mỗi chiều dài. Cũng duy trì`pairs`cho số lượng ≥2 và`quads`cho số lượng ≥4. 
2. Xử lý sơ bộ các tấm ván ban đầu. Với mỗi độ dài, hãy tăng`pairs`truy cập nếu số đếm đạt 2 và tăng`quads`nếu nó đạt tới 4. 
3. Đối với mỗi sự kiện, hãy cập nhật tần suất cho chiều dài tấm ván bị ảnh hưởng. Nếu bản cập nhật vượt qua ngưỡng 2 hoặc 4, hãy điều chỉnh`pairs`hoặc`quads`tương ứng. Bộ đếm tăng nếu đạt đến ngưỡng, giảm nếu ngưỡng không còn được đáp ứng. 
4. Sau khi cập nhật tần suất, hãy kiểm tra xem có thể xây dựng kho lưu trữ hay không. Điều kiện là`quads ≥ 1`Và`pairs ≥ 4`.`pairs ≥ 4`đảm bảo chúng ta có đủ ván cho hình vuông và hình chữ nhật cùng một lúc. 
5. Xuất "CÓ" nếu điều kiện đúng, nếu không thì "KHÔNG". 

Tại sao nó hoạt động: Bằng cách đếm chiều dài với ít nhất hai và bốn tấm ván và cập nhật số lượng này dần dần, chúng tôi nắm bắt được tất cả các cách có thể để tạo thành hình vuông và hình chữ nhật mà không cần liệt kê các kết hợp. Điều bất biến là`quads`số lượng luôn phản ánh số độ dài có sẵn để tạo thành một hình vuông,

---
title: "CF 1875D - Sứa và Mex"
description: "Chúng ta được cho một mảng các số nguyên không âm. Nhiệm vụ là xóa từng phần tử khỏi mảng và sau mỗi lần xóa, chúng tôi thêm MEX của mảng còn lại vào tổng số đang chạy. Mục tiêu là giảm thiểu tổng số này bằng cách chọn thứ tự xóa một cách tối ưu."
date: "2026-06-09T00:57:52+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 1875
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 901 (Div. 2)"
rating: 1600
weight: 1875
solve_time_s: 30
verified: false
draft: false
---

[CF 1875D - Sứa và Mex](https://codeforces.com/problemset/problem/1875/D) 

**Đánh giá:** 1600 
**Thẻ:** dp 
**Thời gian giải:** 30s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên không âm. Nhiệm vụ là xóa từng phần tử khỏi mảng và sau mỗi lần xóa, chúng tôi thêm MEX của mảng còn lại vào tổng số đang chạy. Mục tiêu là giảm thiểu tổng số này bằng cách chọn thứ tự xóa một cách tối ưu. MEX của một mảng là số nguyên không âm nhỏ nhất không có trong mảng, do đó, chuỗi các giá trị MEX phụ thuộc hoàn toàn vào phần tử nào bị xóa trước. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp có một mảng có độ dài tối đa 5000. Tổng của tất cả độ dài mảng trong các trường hợp thử nghiệm tối đa là 5000. Mỗi phần tử có thể rất lớn, tối đa$10^9$. Điều này có nghĩa là chúng ta không thể sử dụng mảng tần số trực tiếp được lập chỉ mục theo các giá trị phần tử. Thuật toán của chúng tôi phải tránh các hoạt động tỷ lệ thuận với phần tử tối đa. 

Một giải pháp đơn giản tính toán lại MEX sau mỗi lần xóa là quá chậm, vì mỗi lần tính toán lại yêu cầu quét hoặc duy trì toàn bộ mảng, điều này$O(n^2)$tổng cộng. Các trường hợp cạnh phát sinh khi mảng chứa các phần tử lặp lại hoặc khi ban đầu thiếu một số số nhỏ, vì những số này xác định các giá trị MEX ban đầu. Ví dụ, trong mảng`[1,2]`, MEX ban đầu là 0. Một cách tiếp cận bất cẩn luôn xóa phần tử lớn nhất trước tiên có thể làm tăng tổng số một cách không cần thiết một cách không chính xác. Tương tự, các mảng có khoảng trống trong một số số nguyên không âm đầu tiên cần phải xóa cẩn thận để tránh thêm các giá trị MEX không cần thiết. 

## Phương pháp tiếp cận 

Giải pháp brute-force là thử mọi hoán vị của việc xóa, tính toán lại MEX sau mỗi lần xóa và tính tổng chúng. Điều này đảm bảo số tiền tối thiểu, nhưng nó mang tính giai thừa trong$n$và hoàn toàn không thực tế đối với$n = 5000$. 

Quan sát quan trọng là sự đóng góp của mỗi số vào tổng số chỉ phụ thuộc vào số lượng bản sao của các số nhỏ hơn MEX còn lại. Khi một số bị xóa ảnh hưởng đến MEX hiện tại, MEX sẽ giữ nguyên hoặc tăng lên. Chúng ta có thể khai thác điều này bằng cách suy nghĩ về số lần đếm tần số của mỗi số, chỉ đối với các số lên đến$n$, bởi vì MEX không thể vượt quá$n$. 

Chúng tôi duy trì một mảng tần số cho các số$0$ĐẾN$n$và mô phỏng việc xóa theo hai giai đoạn. Đầu tiên, chúng tôi loại bỏ đủ các số trùng lặp nhỏ hơn MEX hiện tại để tránh tăng nó, giảm thiểu tổng số. Sau đó, chúng tôi loại bỏ các số bằng MEX hiện tại để chỉ tăng nó khi không thể tránh khỏi. Bằng cách sử dụng lập trình động, chúng tôi theo dõi tổng số tối thiểu để loại bỏ một số phần tử nhất định, biết chúng tôi có thể xóa bao nhiêu bản sao mà không tăng MEX. Điều này làm giảm vấn đề từ độ phức tạp giai thừa sang bậc hai trong$n$, phù hợp với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu (Tần số + DP) | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của mỗi số nguyên trong mảng nhỏ hơn hoặc bằng$n$. Bỏ qua các số lớn hơn$n$vì chúng không ảnh hưởng đến MEX cho đến khi tất cả các số từ 0 đến n bị loại bỏ. 
2. Khởi tạo bảng lập trình động`dp[i][j]`Ở đâu`i`là ứng cử viên hiện tại của MEX và`j`là số lượng bản sao bổ sung được loại bỏ ngoài bản sao đầu tiên của mỗi số. Đặt tất cả các giá trị ban đầu thành vô cùng ngoại trừ`dp[0][0] = 0`. 
3. Lặp lại các ứng cử viên MEX từ 0 đến$n$. Đối với mỗi ứng viên`i`, hãy cân nhắc việc xóa`k`trùng lặp (0  k  tần số[i]) của`i`trước khi MEX đạt tới`i`. Cập nhật`dp[i+1][new_j]`BẰNG`dp[i][j] + (frequency[i] - k)`, cái gì_]()

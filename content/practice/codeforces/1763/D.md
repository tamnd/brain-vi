---
title: "CF 1763D - Hoán vị Bitonic hợp lệ"
description: "Chúng ta được yêu cầu đếm số hoán vị của các số từ 1 đến n là bitonic và thỏa mãn hai vị trí cố định. Hoán vị bit là hoán vị tăng nghiêm ngặt đến một chỉ số cực đại nhất định, sau đó giảm nghiêm ngặt sau đó."
date: "2026-06-09T13:36:57+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "implementation", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1763
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 840 (Div. 2) and Enigma 2022 - Cybros LNMIIT"
rating: 2200
weight: 1763
solve_time_s: 70
verified: false
draft: false
---

[CF 1763D - Hoán vị Bitonic hợp lệ](https://codeforces.com/problemset/problem/1763/D) 

**Đánh giá:** 2200 
**Tags:** tổ hợp, dp, thực hiện, toán, lý thuyết số 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm số hoán vị của các số từ 1 đến n là **bitonic** và thỏa mãn hai vị trí cố định. Hoán vị bit là hoán vị tăng nghiêm ngặt đến một chỉ số cực đại nhất định, sau đó giảm nghiêm ngặt sau đó. Đối với mỗi trường hợp thử nghiệm, đầu vào cung cấp các vị trí`i`Và`j`cùng với những con số`x`Và`y`phải chiếm giữ những vị trí đó. Đầu ra là số hoán vị bitonic tuân theo các ràng buộc này, modulo$10^9 + 7$. 

Các ràng buộc cho phép tối đa 100 trường hợp thử nghiệm với`n`lên tới 100. Điều này loại trừ việc tạo ra tất cả các hoán vị một cách rõ ràng vì n! tăng nhanh, thậm chí với n = 15 số hoán vị vẫn vượt quá 10^12. Chúng ta cần một cách tiếp cận tổ hợp hơn là liệt kê một cách bạo lực. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó đỉnh của hoán vị bit bị buộc phải ở trước hoặc sau một trong các vị trí cố định, điều này có thể làm cho số đếm bằng 0. Ví dụ, nếu`i = 1`với`x`bằng số lớn nhất, nó không thể là một phần của tiền tố tăng dần, do đó không tồn tại hoán vị bit hợp lệ. Một trường hợp phức tạp khác là khi hai số cố định`x`Và`y`buộc các điều kiện trái ngược nhau về đỉnh điểm; ví dụ, nếu`x < y`Nhưng`i > j`, sau đó`x`sẽ phải xuất hiện trước`y`theo thứ tự giảm dần, điều này là không thể. 

## Phương pháp tiếp cận 

**Cách tiếp cận ngây thơ** là tạo ra tất cả n! các hoán vị và đếm những hoán vị bitonic và thỏa mãn các ràng buộc. Điều này hiệu quả với n 6 nhưng thậm chí thất bại với n = 10 do tăng trưởng giai thừa. Lý do đúng rất đơn giản: bạn có thể xác minh rõ ràng hình dạng hoán vị và vị trí cố định. Nó không thành công với n lớn hơn vì 100! hoạt động hoàn toàn không thể thực hiện được. 

**Thông tin chi tiết quan trọng** là chúng ta không cần tạo hoán vị. Khi chúng tôi sửa đỉnh của mảng bitonic, phía bên trái phải chứa tất cả các số nhỏ hơn đỉnh theo thứ tự tăng dần và phía bên phải tất cả các số nhỏ hơn đỉnh theo thứ tự giảm dần. Bằng cách coi đỉnh là một biến và chia mảng thành "trái của đỉnh" và "bên phải của đỉnh", chúng ta có thể mô hình hóa vấn đề theo kiểu tổ hợp. Số lượng giảm xuống **sự kết hợp** về cách phân phối các số nhỏ hơn hoặc lớn hơn mức đỉnh trên hai phân đoạn. Những hạn chế`B_i = x`Và`B_j = y`cho phép chúng tôi cắt bỏ những cấu hình không thể thực hiện được ngay lập tức. 

Việc quan sát thấy mỗi cạnh của đỉnh được xác định hoàn toàn bởi những con số nào ở đó cho phép chúng ta tính toán câu trả lời một cách hiệu quả bằng cách sử dụng các giai thừa và tổ hợp, thay vì lặp lại các hoán vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tổ hợp / Giai thừa | O(n^2) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính trước giai thừa và nghịch đảo giai thừa modulo$10^9 + 7$lên tới n = 100. Điều này sẽ cho phép chúng ta tính toán các kết hợp một cách hiệu quả. 
2. Đối với mỗi trường hợp thử nghiệm, hãy khởi tạo bộ đếm cho các hoán vị bit hợp lệ. 
3. Lặp lại tất cả các vị trí cao nhất có thể`k`từ 1 đến n. Giá trị cực đại phải thỏa mãn`1 ≤ peak ≤ n`và không thể trùng với các số cố định khác trừ khi một trong các vị trí cố định đó chính xác bằng`k`. 
4. Kiểm tra xem một trong các vị trí cố định có`i`hoặc`j`nằm trước, tại hoặc sau đỉnh. Nếu như`i`là trước đỉnh điểm,`B_i = x`phải nhỏ hơn mức đỉnh. Nếu như`i`là sau đỉnh điểm,`B_i = x`phải nhỏ hơn đỉnh nhưng chiếm vị trí trong đoạn giảm dần. Nếu như`i = k`, sau đó`x`phải bằng đỉnh. Áp dụng logic tương tự cho`j`Và`y`. Nếu những điều kiện này không thành công, hãy bỏ qua đỉnh này. 
5. Đếm xem có bao nhiêu số nhỏ hơn đỉnh và bao nhiêu số lớn hơn. Cho phép`a`các số ở bên trái (tăng dần) và `b

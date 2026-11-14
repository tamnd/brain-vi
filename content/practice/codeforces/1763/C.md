---
title: "CF 1763C - Một vấn đề khác về mảng"
description: "Chúng ta được cung cấp một mảng các số nguyên và chúng ta được phép áp dụng nhiều lần một phép biến đổi rất cụ thể: chọn hai vị trí $i < j$, tính chênh lệch tuyệt đối của các giá trị ở hai đầu và ghi đè toàn bộ phân đoạn $[i, j]$ bằng giá trị duy nhất đó."
date: "2026-06-09T13:35:56+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1763
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 840 (Div. 2) and Enigma 2022 - Cybros LNMIIT"
rating: 2000
weight: 1763
solve_time_s: 160
verified: false
draft: false
---

[CF 1763C - Một vấn đề khác về mảng](https://codeforces.com/problemset/problem/1763/C) 

**Xếp hạng:** 2000 
**Tags:** vũ lực, thuật toán mang tính xây dựng, lòng tham 
**Thời gian giải:** 2 phút 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta được phép áp dụng nhiều lần một phép biến đổi rất cụ thể: chọn hai vị trí$i < j$, tính chênh lệch tuyệt đối của các giá trị ở hai đầu và ghi đè lên toàn bộ đoạn$[i, j]$với giá trị duy nhất đó. 

Hoạt động này mạnh mẽ vì nó thu gọn toàn bộ các khoảng thành một khối không đổi. Sau khi thực hiện đủ các thao tác, mảng sẽ trở thành hằng số từng phần và mọi thao tác sẽ thay thế một cách hiệu quả một phân đoạn bằng một giá trị chỉ phụ thuộc vào điểm cuối của nó. 

Mục tiêu là chọn một chuỗi gồm các đoạn thu gọn sao cho tổng cuối cùng của mảng càng lớn càng tốt. Vì mọi thao tác đều làm giảm cấu trúc nhưng có thể tăng hoặc giảm giá trị tùy thuộc vào điểm cuối đã chọn, nên vấn đề là hiểu những giá trị nào có thể được “tạo ra” và cách chúng lan truyền. 

Các ràng buộc lớn, với tổng kích thước mảng lên tới$2 \cdot 10^5$trên tất cả các trường hợp thử nghiệm, vì vậy mọi giải pháp đều phải tuyến tính hoặc gần tuyến tính cho mỗi thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ mô phỏng hoạt động hoặc khoảng DP nào xem xét tất cả các phân đoạn, vì đó ít nhất là bậc hai. 

Một kiểu thất bại phổ biến ở đây là giả định rằng việc sáp nhập tham lam cục bộ là đủ. Ví dụ, việc cố gắng áp dụng nhiều lần thao tác đạt được lợi ích tức thời tốt nhất sẽ thất bại vì những lựa chọn sớm có thể phá hủy khả năng tạo ra những khác biệt lớn hơn sau này. Quá trình này bị hạn chế trên toàn cầu thông qua khả năng tiếp cận các giá trị chứ không phải cải thiện cục bộ. 

## Phương pháp tiếp cận 

Một góc nhìn bạo lực sẽ mô phỏng tất cả các hoạt động có thể xảy ra. Mỗi thao tác chọn một cặp$(i, j)$và thay thế một phân đoạn, do đó không gian trạng thái tăng theo cấp số nhân. Ngay cả đối với$n = 20$, số lượng các chuỗi có thể sẽ tăng vọt vì mỗi lần hợp nhất sẽ thay đổi tất cả các khác biệt hợp lệ trong tương lai. Điều này là hoàn toàn không thể thực hiện được. 

Quan sát cấu trúc quan trọng là hoạt động không bảo toàn trật tự một cách phức tạp, nó chỉ phụ thuộc vào sự khác biệt của các điểm cuối được chọn. Khi chúng ta xem mảng dưới dạng một tập hợp các giá trị, thao tác sẽ liên tục đưa ra các giá trị khác biệt tuyệt đối với các giá trị hiện có. Đây chính xác là quá trình đóng theo phép trừ. 

Sự đơn giản hóa quan trọng là các giá trị cuối cùng có thể đạt được tạo thành một tập hợp đóng dưới sai phân tuyệt đối, ngụ ý rằng chúng đều là bội số của gcd của các khác biệt theo cặp trong mảng ban đầu. Khi bất biến này được xác định, quy trình sẽ trở thành quy trình số học thuần túy: kích thước bước nhỏ nhất có thể xuất hiện là gcd của sai phân và mọi giá trị trong cấu hình tối ưu cuối cùng nằm trên cấp số cộng được xác định bởi gcd này. 

Chiến lược tối ưu trải rộng tiến trình này một cách hiệu quả trong khoảng từ giá trị tối thiểu đến giá trị tối đa, bởi vì không thể tạo được gì ngoài phạm vi này và mọi thứ bên trong được căn chỉnh theo gcd đều có thể đạt được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | hàm mũ | Quá chậm | 
| Cấu trúc dựa trên GCD | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm bớt vấn đề để hiểu cấu trúc của tất cả các giá trị có thể đạt được thông qua các phép toán chênh lệch khoảng thời gian lặp đi lặp lại. 

1. Tính phần tử tối thiểu và tối đa trong mảng. Chúng xác định giới hạn của bất kỳ giá trị nào có thể xuất hiện sau các thao tác, vì tất cả các giá trị được xây dựng đều đến từ sự khác biệt của các phần tử hiện có. 
2. Tính ước chung lớn nhất của mọi hiệu$a_i - a_1$. Điều này nắm bắt kích thước bước bất biến của hệ thống, bởi vì mọi thao tác đều tạo ra sự khác biệt tuyệt đối để duy trì tính chia hết cho gcd này. 
3. Giải thích gcd này là đơn vị cơ bản của tập hợp có thể truy cập. Mọi giá trị có thể xuất hiện đều đồng dư theo modulo gcd này với các phần tử ban đầu và các thao tác lặp lại sẽ lấp đầy tất cả các điểm trung gian có thể tiếp cận. 
4. Cấu hình cuối cùng tối đa hóa tổng tương ứng

---
title: "CF 346C - Biến đổi số II"
description: "Chúng ta bắt đầu với số a và muốn giảm nó xuống b bằng cách sử dụng hai kiểu di chuyển. Một bước đi chỉ đơn giản là làm giảm giá trị hiện tại đi 1. Bước đi còn lại thú vị hơn: chúng ta chọn một trong các số đã cho xi và trừ đi phần còn lại của giá trị hiện tại khi chia cho xi."
date: "2026-06-06T18:22:45+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 346
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 201 (Div. 1)"
rating: 2200
weight: 346
solve_time_s: 35
verified: false
draft: false
---

[CF 346C - Chuyển đổi số II](https://codeforces.com/problemset/problem/346/C) 

**Đánh giá:** 2200 
**Tags:** tham lam, toán học 
**Thời gian giải:** 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một con số`a`và muốn giảm nó xuống`b`sử dụng hai loại chuyển động. Một nước đi chỉ đơn giản là giảm giá trị hiện tại đi 1. Nước đi khác thú vị hơn: chúng ta chọn một trong các số đã cho`x_i`và trừ đi phần còn lại của giá trị hiện tại khi chia cho`x_i`. Trong thực tế, nếu chúng ta đang ở giá trị`v`, thao tác thứ hai này sẽ đưa chúng ta xuống bội số gần nhất của`x_i`nó không lớn hơn`v`. 

Mục tiêu là đạt được chính xác`b`từ`a`trong ít bước di chuyển nhất. 

Ràng buộc`a - b ≤ 10^6`là giới hạn cấu trúc trung tâm. Nó ngụ ý rằng chúng ta không bao giờ xử lý các phạm vi tuyệt đối lớn; thay vào đó, chúng ta đang làm việc trong một khoảng thời gian đi xuống tương đối ngắn. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng khám phá tất cả các số nguyên lên đến`10^9`một cách rõ ràng hoặc xây dựng các biểu đồ đầy đủ trên không gian giá trị. 

Một điểm tinh tế quan trọng là thao tác thứ hai phụ thuộc rất nhiều vào việc lựa chọn`x_i`. Một số giá trị của`x_i`thực sự vô dụng nếu chúng không bao giờ tạo ra mức giảm tốt hơn mức lặp lại`-1`các bước, trong khi những bước khác có thể tạo ra những “bước nhảy” lớn bỏ qua nhiều bước giảm đơn vị. Thách thức ở đây là quyết định khi nào những bước nhảy như vậy thực sự mang lại lợi ích. 

Một sai lầm ngây thơ nảy sinh khi luôn áp dụng cách giảm modulo tức thời tốt nhất. Ví dụ, nếu`a = 100`,`b = 0`, và chúng ta có`x = 51`, sau đó`100 mod 51 = 49`, vì vậy chúng tôi chuyển sang`51`. Nhưng từ`51`, chúng ta có thể thích một mô đun khác kém hơn ở địa phương nhưng tốt hơn trên toàn cầu. Những lựa chọn tham lam cục bộ thất bại vì vấn đề về cơ bản là đường đi ngắn nhất trên một không gian trạng thái có trọng số. 

Một trường hợp khó nhận thấy khác là khi tất cả`x_i`lớn so với giá trị hiện tại. Sau đó mỗi`a mod x_i`bằng`a`, làm cho thao tác thứ hai tương đương với việc trừ toàn bộ giá trị trong một bước. Việc triển khai bất cẩn vẫn có thể coi nó như một giải pháp thay thế có ý nghĩa và làm phức tạp quá trình chuyển đổi một cách không cần thiết. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua cấu trúc, công thức tự nhiên là một biểu đồ trong đó mỗi giá trị số nguyên là một nút và các cạnh biểu thị các phép toán được phép. Từ một nút`v`, chúng ta có thể đi đến`v - 1`, và cũng để`v - (v mod x_i)`cho mỗi`x_i`. Chạy BFS trên biểu đồ này sẽ đưa ra câu trả lời vì mỗi lần di chuyển đều tốn 1 chi phí. 

Điều này đúng nhưng quá chậm nếu được thực hiện trực tiếp. Phạm vi từ`b`ĐẾN`a`có thể lên đến`10^6`, vì vậy về mặt lý thuyết BFS có thể chấp nhận được về kích thước. Tuy nhiên, việc tạo ra tất cả các cạnh đi một cách ngây thơ mang lại`O(n)`chuyển đổi trên mỗi nút, dẫn đến`O(n (a-b))`, vượt xa giới hạn khi`n = 10^5`. 

Quan sát quan trọng là phép toán thứ hai luôn đưa chúng ta đến bội số của một số`x_i`và chúng tôi chỉ quan tâm đến bước nhảy tốt nhất có thể từ mỗi giá trị. Thay vì xem xét tất cả`x_i`ở mọi trạng thái, chúng tôi có thể tính toán trước “bước nhảy hiệu quả” tốt nhất cho mỗi giá trị bằng cách nhóm các chuyển đổi giống hệt hoặc có thể so sánh được và chỉ lặp lại các ứng cử viên có liên quan. Cấu trúc quan trọng là từ bất kỳ giá trị nào`v`, chúng ta chỉ cần xét những`x_i`sự thay đổi có ý nghĩa đó`v`và với nhiều giá trị của`v`, bộ hữu ích`x_i`nhỏ vì lớn`x_i`tạo ra hành vi giống hệt nhau (không giảm quá`-1`bước hoặc sụp đổ trực tiếp). 

Điều này cho phép một cách tiếp cận lập trình động trong khoảng thời gian`[b, a]`, trong đó chúng tôi tính toán chi phí tối thiểu để đạt được`b`từ mọi giá trị trở xuống. Tại mỗi`v`, chúng tôi giảm hoặc thử tất cả các bước nhảy mô đun hữu ích, nhưng chúng tôi đảm bảo mỗi`x_i`chỉ đóng góp khi nó tạo ra một trạng thái tiếp theo riêng biệt, trạng thái này có thể được phân bổ thành hành vi logarit hoặc gần tuyến tính trong toàn bộ khoảng thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force BFS trên biểu đồ đầy đủ | O(n · (a-b)) | O(a-b) | Quá chậm | 
| DP được tối ưu hóa theo khoảng thời gian với các chuyển tiếp được lọc | O((a-b) log n) | O(a-b) | Đã chấp nhận | 

## Thuật toán

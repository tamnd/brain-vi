---
title: "CF 1040A - Điệu nhảy Palindrome"
description: "Chúng tôi được sắp xếp một hàng vũ công, mỗi người đảm nhận một vị trí cố định. Cuối cùng, mọi vũ công đều phải mặc bộ đồ trắng hoặc bộ đồ đen, và một số bộ đồ này đã được cố định trong khi những bộ đồ khác vẫn chưa quyết định."
date: "2026-06-16T18:07:42+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1040
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 507 (Div. 2, based on Olympiad of Metropolises)"
rating: 1000
weight: 1040
solve_time_s: 148
verified: false
draft: false
---

[CF 1040A - Khiêu vũ Palindrome](https://codeforces.com/problemset/problem/1040/A) 

**Đánh giá:** 1000 
**Thẻ:** tham lam 
**Thời gian giải:** 2m 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được sắp xếp một hàng vũ công, mỗi người đảm nhận một vị trí cố định. Cuối cùng, mọi vũ công đều phải mặc bộ đồ trắng hoặc bộ đồ đen, và một số bộ đồ này đã được cố định trong khi những bộ đồ khác vẫn chưa quyết định. Mục tiêu là gán màu cho tất cả các vị trí chưa quyết định sao cho chuỗi màu cuối cùng đọc giống nhau từ trái sang phải và từ phải sang trái. 

Chúng tôi không được phép sắp xếp lại các vũ công và không được phép thay bộ đồ đã được chỉ định. Quyền tự do duy nhất chúng ta có là chọn màu cho các vị trí được đánh dấu là không xác định. Mỗi bộ đồ trắng đều có giá`a`, và mỗi bộ đồ đen có giá`b`, vì vậy chúng tôi muốn giảm thiểu tổng chi phí gán màu cho các vị trí không xác định trong khi vẫn đảm bảo chuỗi cuối cùng là một bảng màu. 

Ràng buộc`n ≤ 20`có nghĩa là chuỗi cực kỳ nhỏ. Ngay cả các thuật toán thử mọi phép gán vị trí chưa biết về mặt kỹ thuật cũng khả thi vì không gian tìm kiếm nhiều nhất là`2^20`, tức là khoảng một triệu khả năng. Nó đủ nhỏ đối với Python, nhưng vẫn không cần thiết nếu chúng ta khai thác trực tiếp tính đối xứng. 

Khó khăn chính là tính nhất quán giữa các vị trí được phản ánh. Mỗi chỉ số`i`phải phù hợp với đối tác đối xứng của nó`n - 1 - i`. Bất kỳ mâu thuẫn nào ngay lập tức khiến nhiệm vụ trở nên bất khả thi. 

Một số trường hợp phức tạp xuất hiện một cách tự nhiên: 

Nếu một cặp được nhân đôi chứa các màu cố định xung đột nhau, chẳng hạn như`0`ở một bên và`1`mặt khác, không có sự hoàn thành hợp lệ. Ví dụ, đầu vào`n = 2`,`0 1`không bao giờ có thể trở thành một palindrome vì cả hai vị trí đều cố định và không tương thích. 

Nếu cả hai bên đều không xác định thì quyết định hoàn toàn dựa trên chi phí và cả hai phải được gán cùng một màu, nếu không thì tính đối xứng sẽ bị phá vỡ. 

Nếu một bên cố định và một bên không xác định được thì vị trí không xác định buộc phải khớp với bên cố định, cho dù đắt hơn bên thay thế. 

Một cách tiếp cận ngây thơ xử lý từng vị trí một cách độc lập sẽ thất bại vì nó bỏ qua cấu trúc phụ thuộc do tính đối xứng gây ra. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ gán một màu (trắng hoặc đen) cho mọi vị trí được đánh dấu là không xác định. Đối với mỗi phép gán hoàn chỉnh, chúng tôi kiểm tra xem chuỗi kết quả có phải là một bảng màu hay không và tính giá trị của nó. Vì mỗi lên đến`n`vị trí có thể phân nhánh thành hai lựa chọn, điều này dẫn đến tối đa`2^n`cấu hình và với mỗi cấu hình, chúng tôi quét mảng trong`O(n)`để xác nhận tính đối xứng. Độ phức tạp trong trường hợp xấu nhất trở thành`O(n·2^n)`, vẫn còn ở ranh giới nhưng chỉ được chấp nhận vì`n ≤ 20`. 

Cấu trúc của vấn đề làm cho điều này trở nên không cần thiết. Ràng buộc palindrome không phụ thuộc vào tương tác toàn cầu; nó phân hủy rõ ràng thành các ràng buộc độc lập trên các cặp được nhân đôi`(i, n-1-i)`. Mỗi cặp có thể được giải quyết một cách cô lập. Khi chúng tôi quan sát thấy điều này, vấn đề sẽ giảm xuống còn việc quyết định nhiệm vụ nhất quán rẻ nhất cho mỗi cặp. 

Đối với mỗi cặp, chỉ có một vài tình huống có thể xảy ra: cả hai đều cố định một cách nhất quán, cả hai đều cố định không nhất quán, một cố định và một không xác định, hoặc cả hai đều không xác định. Mỗi trường hợp có thể được giải quyết một cách tham lam bằng cách chọn phép gán hợp lệ duy nhất hoặc rẻ nhất. 

Điều này làm giảm vấn đề từ tìm kiếm theo cấp số nhân trên toàn bộ chuỗi sang quét tuyến tính theo cặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · 2^n) | O(n) | Quá chậm nhưng có thể | 
| Tham lam theo cặp | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi bằng cách ghép các chỉ số đối xứng`i`Và`j = n - 1 - i`. 

1. Cho mỗi cặp`(i, j)`, kiểm tra các giá trị`c[i]`Và`c[j]`. Ràng buộc palindrome buộc chúng phải bằng nhau trong cấu hình cuối cùng, vì vậy chúng ta giải quyết chúng cùng nhau. 
2. Nếu cả hai vị trí đều cố định và giá trị của chúng khác nhau thì không có cách nào hợp lệ để điều hòa chúng, do đó câu trả lời là không thể ngay lập tức. Điều này là do không được phép chỉ định lại các bộ quần áo cố định. 
3. Nếu cả hai vị trí đều cố định và bằng nhau thì không cần thêm chi phí vì không cần phải mua gì. 
4. Nếu đúng một vị trí cố định và vị trí còn lại chưa xác định thì vị trí chưa biết phải khớp với vị trí cố định. Chúng tôi thêm chi phí mua màu đó cho vị trí không xác định. Điều này là bắt buộc vì bất kỳ lựa chọn nào khác sẽ phá vỡ tính đối xứng. 
5. Nếu không xác định được cả 2 vị trí thì ta chọn màu rẻ hơn

---
title: "CF 104885A - \u0414\u0435\u0442\u0430\u043b\u0438 \u0438 \u0440\u0435\u0441\u0443\u0440\u0441\u044b"
description: "Chúng tôi được giao một cơ sở sản xuất với một số công nhân. Mỗi công nhân sản xuất một số lượng cố định các bộ phận giống hệt nhau và mỗi bộ phận tiêu thụ một lượng kim loại cố định. Tất cả kim loại được sản xuất sau đó được đóng gói vào các thùng chứa, trong đó mỗi thùng chứa chỉ có thể chứa một trọng lượng giới hạn."
date: "2026-06-28T09:07:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104885
codeforces_index: "A"
codeforces_contest_name: "Municipal stage of ROI in Nizhny Novgorod 2023"
rating: 0
weight: 104885
solve_time_s: 28
verified: false
draft: false
---

[CF 104885A - \u0414\u0435\u0442\u0430\u043b\u0438 \u0438 \u0440\u0435\u0441\u0443\u0440\u0441\u044b](https://codeforces.com/problemset/problem/104885/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao một cơ sở sản xuất với một số công nhân. Mỗi công nhân sản xuất một số lượng cố định các bộ phận giống hệt nhau và mỗi bộ phận tiêu thụ một lượng kim loại cố định. Tất cả kim loại được sản xuất sau đó được đóng gói vào các thùng chứa, trong đó mỗi thùng chứa chỉ có thể chứa một trọng lượng giới hạn. Mỗi thùng chứa đầy có giá một số tiền cố định. 

Nhiệm vụ là tính toán tổng chi phí để mua đủ thùng chứa để chứa tất cả kim loại cần thiết cho sản xuất. 

Cụ thể, tổng lượng kim loại được xác định bằng cách nhân số lượng công nhân, số bộ phận trên mỗi công nhân và lượng kim loại cần thiết cho mỗi bộ phận. Điều này mang lại một tổng trọng lượng duy nhất. Vì các thùng chứa có sức chứa hạn chế nên chúng tôi phải xác định cần bao nhiêu thùng chứa đầy đủ để chứa tổng trọng lượng này, làm tròn số vì các thùng chứa một phần vẫn được thanh toán như những thùng chứa đầy đủ. Cuối cùng, chi phí có được bằng cách nhân số lượng container với giá mỗi container. 

Mặc dù tuyên bố này bị cắt xén một phần, cấu trúc vẫn nhất quán với bài toán cổ điển “phân chia trần sau tổng hợp”. 

Đầu vào ngụ ý là bốn hoặc năm số nguyên biểu thị các thông số sản xuất và giá container. Đầu ra là một số nguyên duy nhất: tổng chi phí. 

Từ quan điểm phức tạp, tất cả các đại lượng đều có thể đủ lớn để tích của ba số nguyên có thể đạt tới khoảng 10^18 nếu không được xử lý cẩn thận. Điều này ngay lập tức loại trừ mọi phương pháp mô phỏng hoặc đóng gói lặp lại. Giải pháp phải hoạt động trong thời gian không đổi chỉ bằng các phép toán số học. 

Một vấn đề tế nhị xuất hiện khi xử lý phép chia: nếu tổng kim loại chia hết cho dung tích thùng chứa thì chúng ta không được thêm thùng chứa bổ sung. Ngược lại, nếu còn thừa dù rất ít cũng vẫn cần thêm một thùng chứa. 

Một ví dụ về một cạm bẫy tiềm ẩn: 

đầu vào: 

N = 2, M = 3, K = 4, L = 10, S = 5 

Tổng số kim loại là 2 × 3 × 4 = 24. Mỗi thùng chứa 10, vì vậy chúng ta cần trần nhà (24/10) = 3 thùng. Giá là 3 × 5 = 15. 

Cách tiếp cận phân chia số nguyên đơn giản mà không làm tròn số sẽ tính toán 24 // 10 = 2 vùng chứa và đưa ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để suy nghĩ về vấn đề này là mô phỏng việc đóng gói kim loại vào thùng chứa mỗi lần một kg. Chúng tôi tính toán tổng số kim loại và sau đó liên tục trừ đi dung tích thùng chứa cho đến khi không còn lại gì, đếm xem có bao nhiêu thùng chứa được sử dụng. Điều này đúng vì nó phản ánh trực tiếp quá trình vật lý của việc đổ đầy thùng chứa. Tuy nhiên, cách tiếp cận này trở nên không cần thiết khi chúng ta nhận thấy rằng chỉ có tổng trọng lượng mới quan trọng chứ không phải sự phân bố riêng lẻ của các bộ phận. 

Sự kém hiệu quả xuất hiện khi tổng kim loại trở nên lớn. Nếu tổng số ở mức 10^18 thì việc mô phỏng ngay cả một bước đơn vị là không thể. Ngay cả việc mô phỏng phép trừ theo từng vùng chứa cũng sẽ yêu cầu tới 10^18 thao tác trong trường hợp xấu nhất, vượt xa mọi giới hạn khả thi. 

Quan sát quan trọng là quá trình này giảm xuống mức chia số nguyên bằng cách làm tròn số. Khi đã biết tổng kim loại, số lượng thùng chứa chỉ phụ thuộc vào việc chia theo dung tích có phần dư hay không. Điều này thu gọn toàn bộ quá trình đóng gói thành một biểu thức số học có thời gian không đổi. 

Chúng tôi tính tổng kim loại là N × M × K, sau đó tính mức trần của tổng kim loại chia cho L và nhân với S. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hung bạo | | | |

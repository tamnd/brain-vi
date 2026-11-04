---
title: "CF 1152E - Neko và hồi tưởng"
description: "Nhiệm vụ là xây dựng lại một mảng các số nguyên dương a, cho hai mảng b' và c'. Các mảng này được tạo từ a qua hai giai đoạn: đầu tiên, bằng cách lấy tất cả các cặp liên tiếp trong a để tạo thành b và c, trong đó mỗi bi là giá trị nhỏ nhất và mỗi ci là giá trị lớn nhất của cặp (ai…"
date: "2026-06-12T02:57:05+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1152
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 554 (Div. 2)"
rating: 2400
weight: 1152
solve_time_s: 53
verified: false
draft: false
---

[CF 1152E - Neko và Hồi tưởng](https://codeforces.com/problemset/problem/1152/E) 

**Đánh giá:** 2400 
**Tags:** thuật toán xây dựng, dfs và tương tự, đồ thị 
**Thời gian giải:** 53s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là xây dựng lại một mảng các số nguyên dương,`a`, cho hai mảng`b'`Và`c'`. Các mảng này được tạo ra từ`a`qua hai giai đoạn: đầu tiên, bằng cách lấy tất cả các cặp liên tiếp vào`a`hình thành`b`Và`c`, mỗi nơi`b_i`là mức tối thiểu và mỗi`c_i`là mức tối đa của cặp`(a_i, a_{i+1})`; thứ hai, bằng cách hoán vị`b`Và`c`với một số hoán vị chưa biết`p`để sản xuất`b'`Và`c'`. Thách thức là đảo ngược quá trình này và tạo ra bất kỳ mảng gốc hợp lệ nào`a`hoặc xác định rằng không tồn tại mảng như vậy. 

Đầu vào bao gồm một số nguyên`n`và hai mảng`b'`Và`c'`, mỗi chiều dài`n-1`. Đầu ra là hợp lệ`a`chiều dài`n`hoặc`-1`nếu việc tái thiết là không thể. Những hạn chế`2 ≤ n ≤ 10^5`Và`1 ≤ b'_i, c'_i ≤ 10^9`đề xuất rằng các thuật toán có độ phức tạp trên`O(n log n)`có thể là quá chậm, trong khi các giải pháp tuyến tính hoặc gần tuyến tính lại khả thi. 

Các trường hợp cạnh không rõ ràng bao gồm các chuỗi trong đó nhiều cặp có cùng mức tối thiểu hoặc tối đa, chẳng hạn như`b' = [1,1,1]`Và`c' = [2,2,2]`. Một cách tiếp cận tham lam ngây thơ gán các giá trị một cách tuần tự mà không xem xét xung đột có thể tạo ra kết quả tiêu cực hoặc trình tự không nhất quán. Một kịch bản phức tạp khác phát sinh khi các phần tử liên tiếp phải thỏa mãn các ràng buộc tối thiểu-tối đa xung đột từ`b'`Và`c'`, Ví dụ`b' = [2,3]`Và`c' = [3,2]`. Thuật toán phải xác minh rằng mọi cặp đều tương thích và mảng cuối cùng`a`không vi phạm bất kỳ mối quan hệ tối thiểu-tối đa nào. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ thử mọi hoán vị của`b'`Và`c'`, cố gắng xây dựng lại`a`và xác minh tính nhất quán với các định nghĩa tối thiểu-tối đa. Vì`n-1`các phần tử, có`(n-1)!`hoán vị. Mỗi hoán vị sẽ yêu cầu xây dựng lại`a`và kiểm tra tính hợp lệ, tức là O(n) cho mỗi hoán vị. Cách tiếp cận này đúng về mặt lý thuyết nhưng không khả thi đối với`n`lên đến`10^5`bởi vì`(n-1)!`có kích thước lớn về mặt thiên văn, vượt xa mọi giới hạn tính toán. 

Nhận xét quan trọng là mỗi phần tử của`a`xuất hiện đúng hai cặp min-max liên tiếp: for element`a_i`(trừ cái đầu tiên và cái cuối cùng),`a_i`là một phần của`b_{i-1}`,`c_{i-1}`,`b_i`, Và`c_i`. Vì vậy, chúng ta có thể mô hình hóa vấn đề này như một bài toán đồ thị: mỗi cặp`(b'_i, c'_i)`xác định một cạnh giữa hai phần tử liên tiếp của`a`, trong đó một phần tử là giá trị tối thiểu và phần tử còn lại là giá trị tối đa. Bằng cách coi mỗi cặp là một cạnh vô hướng và cố gắng gán các điểm cuối một cách nhất quán, vấn đề sẽ giảm xuống việc sắp xếp các cặp này thành một chuỗi trong đó các giá trị khớp với nhau tại các nút được chia sẻ. Chúng ta chỉ cần xem xét hai khả năng cho cặp đầu tiên và truyền bá các lựa chọn một cách tuần tự. Nếu tại bất kỳ bước nào các ràng buộc tối thiểu-tối đa không thể được thỏa mãn thì việc xây dựng lại là không thể. 

Cái nhìn sâu sắc làm giảm độ phức tạp từ giai thừa sang tuyến tính, bởi vì chúng tôi chỉ khám phá tối đa hai chuỗi dựa trên hai phép gán có thể có của cặp đầu tiên. Việc truyền bá đảm bảo rằng mọi phần tử tiếp theo đều khớp hoặc dẫn đến mâu thuẫn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n-1)!) | O(n) | Quá chậm | 
| Phân công tuần tự với sự lan truyền | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo danh sách`a`chiều dài`n`với phần giữ chỗ. Hãy xem xét cặp đầu tiên`(b'_0, c'_0)`. Giao phó`a[0] = b'_0`Và`a[1] = c'_0`và cũng giữ khả năng đảo ngược`a[0] = c'_0`,`a[1] = b'_0`. Hai chuỗi ban đầu này đại diện cho hai lựa chọn bắt đầu duy nhất. 
2. Lặp lại qua các cặp còn lại`i = 1`ĐẾN`n-2`. Đối với mỗi trình tự đang được xem xét, hãy kiểm tra tính tương thích của dòng điện`b'_i`Và`c'_i`với phần tử được gán cuối cùng`a[i]`. Có hai phép gán hợp lệ: hoặc`a[i+1] = b'_i`Và`a[i] = max(a[i], c'_i)`nếu nhất quán, hoặc`a[i+1] = c'_i`Và`a[i] = min(a[i], b'_i)`nếu nhất quán. Loại bỏ bất kỳ trình tự nào dẫn đến xung đột. 
3. Nếu sau khi xử lý tất cả các cặp, một hoặc cả hai chuỗi vẫn hợp lệ, hãy xuất ra bất kỳ chuỗi hợp lệ nào. Nếu cả hai chuỗi đều thất bại, xuất ra`-1`. 
4. Việc truyền bá dựa trên tính bất biến mà mỗi`a[i]`phải thỏa mãn cả ràng buộc tối thiểu và tối đa của các cặp liền kề. Bằng cách kiểm tra cả hai khả năng và loại bỏ các chuỗi không nhất quán ngay lập tức, chúng tôi đảm bảo rằng chỉ các mảng khả thi mới tồn tại. 

Tại sao nó hoạt động: Mỗi cạnh`(b'_i, c'_i)`phải kết nối hai phần tử liên tiếp trong`a`theo một thứ tự nào đó. Việc truyền bá đảm bảo rằng mọi cặp đều được xem xét và phần tử được chia sẻ giữa các cặp liên tiếp là nhất quán. Nếu không có chuỗi nào thỏa mãn bất biến này thì không tồn tại mảng hợp lệ. Vì mỗi cặp được xử lý cũ

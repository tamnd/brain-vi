---
title: "CF 104925I - Cạnh nổi loạn"
description: "Chúng ta được cho một đồ thị có hướng trên các đỉnh được dán nhãn từ 1 đến n, với một gốc phân biệt ở đỉnh 1. Nhiệm vụ là chọn một tập hợp các cạnh có hướng tạo thành một cây bao trùm bắt nguồn từ 1, nghĩa là mọi đỉnh đều có thể tiếp cận được từ 1, và mọi đỉnh ngoại trừ gốc đều có…"
date: "2026-06-28T07:54:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104925
codeforces_index: "I"
codeforces_contest_name: "Osijek Competitive Programming Camp, Fall 2023. Day 6: Estonian Contest (The 2nd Universal Cup. Stage 19: Estonia)"
rating: 0
weight: 104925
solve_time_s: 38
verified: false
draft: false
---

[CF 104925I - Cạnh nổi loạn](https://codeforces.com/problemset/problem/104925/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có hướng trên các đỉnh được dán nhãn từ 1 đến n, với một gốc phân biệt ở đỉnh 1. Nhiệm vụ là chọn một tập hợp các cạnh có hướng tạo thành một cây bao trùm bắt nguồn từ 1, nghĩa là mọi đỉnh đều có thể tiếp cận được từ 1, và mọi đỉnh ngoại trừ gốc có chính xác một cạnh đi vào trong tập hợp đã chọn. Nếu chúng ta bỏ qua hướng, các cạnh được chọn cũng phải tạo thành một cây, do đó không được phép có chu trình vô hướng. Trong số tất cả các cấu trúc hợp lệ như vậy, chúng ta muốn tổng trọng lượng cạnh nhỏ nhất có thể. 

Cấu trúc đặc biệt của đầu vào là ràng buộc chính: hầu hết mọi cạnh đều đi từ đỉnh được lập chỉ mục nhỏ hơn đến đỉnh được lập chỉ mục lớn hơn, ngoại trừ chính xác một cạnh có thể vi phạm trật tự này. Điều này có nghĩa là biểu đồ gần như là một DAG khi được sắp xếp theo chỉ mục, ngoại trừ một cạnh “lùi” duy nhất có thể trỏ từ chỉ mục lớn hơn đến chỉ mục nhỏ hơn. 

Các ràng buộc đủ lớn để bất kỳ giải pháp nào gần hơn tuyến tính cho mỗi trường hợp thử nghiệm sẽ hết thời gian chờ. Tổng các đỉnh trong tất cả các trường hợp thử nghiệm chỉ là 2·10^5 và các cạnh 5·10^5, do đó cần phải có giải pháp xung quanh O(n + m) hoặc O((n + m) log n). Bất cứ điều gì như chạy thuật toán MST được định hướng chung như thuật toán của Edmonds đều không cần thiết và thực tế quá chậm đối với cấu trúc này. 

Một cách tiếp cận đơn giản là coi nó như một bài toán MST được định hướng đầy đủ và chạy một thuật toán chung cho mỗi trường hợp thử nghiệm. Điều đó có thể đúng nhưng quá chậm vì thuật toán của Edmonds gần như là O(mn) khi triển khai đơn giản, điều này hoàn toàn không khả thi ở những giới hạn này. 

Có hai dạng thất bại tinh vi xuất phát từ việc bỏ qua cấu trúc. 

Đầu tiên, nếu chúng ta tham lam chọn cạnh đầu vào nhỏ nhất cho mỗi nút một cách độc lập, chúng ta có thể vô tình tạo ra một chu trình có hướng. Ví dụ: giả sử 2 → 3, 3 → 4 và 4 → 2 đều được chọn là các cạnh đến tốt nhất. Mặc dù mỗi lựa chọn đều tối ưu cục bộ nhưng chúng tạo thành một chu trình và vi phạm yêu cầu phát triển dạng cây. 

Thứ hai, cạnh lùi đơn có thể tạo ra một lối tắt giúp cải thiện chi phí, nhưng việc sử dụng nó một cách mù quáng có thể tạo ra một chu trình với các cạnh thuận đã được chọn. Ví dụ: nếu cạnh lùi là u → v và đã có đường dẫn từ v đến u trong cấu trúc tiến, việc chọn nó làm cha của v sẽ tạo ra một chu trình có hướng. 

Những vấn đề này buộc chúng ta phải kết hợp cẩn thận giữa tính tối ưu cục bộ với sự an toàn của chu trình toàn cầu. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua hoàn toàn cạnh lùi, biểu đồ sẽ trở thành DAG trong đó mọi cạnh đi từ chỉ mục nhỏ hơn đến chỉ mục lớn hơn. Trong cấu trúc như vậy, quá trình phân nhánh bắt nguồn từ 1 rất dễ dàng: mọi nút v chọn cạnh đến có trọng số tối thiểu từ một số u < v. Điều này là an toàn vì bất kỳ đường đi nào trong cấu trúc kết quả đều làm tăng chỉ số một cách nghiêm ngặt, do đó các chu kỳ là không thể. Điều này tạo ra một ứng cử viên cây bao trùm có định hướng hợp lệ và chi phí của nó dễ dàng tính toán. 

Giải pháp cơ bản này là tối ưu cho phần DAG vì mỗi nút sẽ giảm thiểu cạnh đến của nó một cách độc lập và không có ràng buộc tổng thể nào can thiệp. 

Sự phức tạp phát sinh từ cạnh đơn u → v trong đó u > v. Cạnh này phá vỡ cấu trúc đơn điệu và có thể cung cấp cạnh đến cho v rẻ hơn bất kỳ cạnh u < v nào. Tuy nhiên, nếu chúng ta chỉ thay thế cha mẹ của v bằng u, chúng ta có thể đưa ra một chu trình có hướng vì v có thể đã đến u qua các cạnh trước. 

Quan sát quan trọng là tất cả các cạnh phía trước đều tăng chỉ số, do đó, bất kỳ chu trình có hướng nào liên quan đến cạnh phía sau đều phải đi qua một đường tăng nghiêm ngặt từ v đến u. Điều này có nghĩa là nếu chúng ta xây dựng cây cơ sở trước, khả năng tiếp cận từ v đến u là cố định và có thể được kiểm tra trong cây đó. Nếu u không nằm trong cây con của v thì việc chuyển cha của v sang u là an toàn.

Vì vậy, vấn đề giảm xuống còn việc xây dựng cây cơ sở từ các cạnh tiến, tính toán cấu trúc cây con và sau đó kiểm tra xem cạnh lùi có thể thay thế cạnh gốc hiện tại của điểm cuối của nó hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực (MST được chỉ đạo chung) | O(nm) | O(n + m) | Quá chậm | 
| Tham lam theo chỉ số + điều chỉnh đơn | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp theo hai giai đoạn: cây cơ sở chỉ sử dụng các cạnh phía trước, sau đó cải tiến có kiểm soát bằng cách sử dụng một cạnh phía sau. 

1. Với mỗi đỉnh v, hãy tính cạnh đến rẻ nhất từ ​​tất cả các cạnh u → v với u < v. Điều này xác định một ứng cử viên cha cho mọi nút ngoại trừ nút gốc. Bước này hiệu quả vì trong số các cạnh tiến, bất kỳ cạnh cha hợp lệ nào cũng phải đến từ một chỉ mục nhỏ hơn và việc chọn chỉ mục rẻ nhất là tối ưu cục bộ. 
2. Xây dựng cây định hướng T từ các con trỏ cha đã chọn này. Cấu trúc không theo chu kỳ vì mọi cạnh đều đi từ chỉ số nhỏ hơn đến chỉ số lớn hơn, vì vậy mọi đường dẫn đều tăng nhãn một cách nghiêm ngặt. Điều này đảm bảo không có chu kỳ định hướng. 
3. Chạy DFS từ gốc 1 trên cây này để tính thời gian vào và ra cho mỗi nút. Những thời điểm này cho phép chúng tôi kiểm tra mối quan hệ tổ tiên trong O(1) cho mỗi truy vấn. 
4. Xác định cạnh lùi đặc biệt u → v trong đó u > v. Hãy xem xét sử dụng nó làm cha của v thay vì cha hiện tại của nó trong T. 
5. Tính toán mức cải thiện chi phí như current_parent_weight[v] − w(u → v). Nếu giá trị này không dương, chúng ta bỏ qua cạnh lùi. 
6. Kiểm tra xem u có nằm trong cây con của v trong T hay không bằng cách sử dụng dấu thời gian DFS. Nếu u ở trong cây con của v, việc thay thế cây cha sẽ tạo ra một chu trình vì khi đó v sẽ đến được u thông qua các con cháu của nó. 
7. Nếu u không có trong cây con của v, chúng ta có thể áp dụng cải tiến một cách an toàn. Cập nhật câu trả lời bằng cách trừ đi phần cải thiện từ chi phí cơ bản. 

### Tại sao nó hoạt động 

Cấu trúc đường cơ sở là tối ưu trong số tất cả các nhánh bao trùm chỉ sử dụng các cạnh phía trước vì mỗi nút đều giảm thiểu cạnh đến của nó một cách độc lập mà không tạo ra chu kỳ. Bất kỳ sai lệch nào so với những lựa chọn này đều phải liên quan đến lợi thế lạc hậu. 

Vì chỉ có một cạnh lùi nên mọi cải tiến hợp lệ đối với cấu trúc cây chỉ có thể thay thế cạnh gốc của điểm cuối của nó; nó không thể cơ cấu lại bội số

---
title: "CF 1527A - Và Rồi Có K"
description: "Giả sử một thuật toán nhân được gọi trực tuyến nếu nó tạo ra bit đầu ra $(k+1)$st của tích $uv$, bắt đầu từ bit ít quan trọng nhất, chỉ sau khi đọc các bit đầu vào $k+1$ đầu tiên của mỗi toán hạng, cũng từ ít quan trọng nhất đến quan trọng nhất."
date: "2026-06-10T17:13:31+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks"]
categories: ["algorithms"]
codeforces_contest: 1527
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 721 (Div. 2)"
rating: 800
weight: 1527
solve_time_s: 159
verified: false
draft: false
---

[CF 1527A - Và Sau Đó Là K](https://codeforces.com/problemset/problem/1527/A) 

**Đánh giá:** 800 
**Thẻ:** bitmask 
**Thời gian giải:** 2m 39s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

Gọi thuật toán nhân là _online_ nếu nó tạo ra$(k+1)$bit đầu ra thứ nhất của sản phẩm$uv$, bắt đầu từ bit ít quan trọng nhất, chỉ sau khi đọc bit đầu tiên$k+1$các bit đầu vào của mỗi toán hạng, cũng từ ít quan trọng nhất đến quan trọng nhất. Đó là,$i$bit đầu ra thứ chỉ phụ thuộc vào bit đầu tiên$i$bit của mỗi đầu vào. Vấn đề là xác định, đối với các loại máy tự động khác nhau, thời gian tối thiểu trên mỗi bit đầu ra có thể đạt được bằng thuật toán nhân trực tuyến và mô tả đặc điểm của các thuật toán đó nhanh nhất có thể. Chúng ta sẽ ký hiệu bằng$m$Và$n$độ dài bit của toán hạng, với$m \le n$. 

Chúng ta hãy xem xét ba loại automata tự nhiên: 

1. **Automata trạng thái hữu hạn (FSA)**, có số lượng trạng thái cố định, hữu hạn, độc lập với$n$. 
2. **Máy tự động đẩy xuống (PDA)**, có điều khiển trạng thái hữu hạn và một ngăn xếp không giới hạn. 
3. **Máy Turing (TM)**, có bộ điều khiển trạng thái hữu hạn và (các) băng đọc/ghi không giới hạn. 

Mục đích là để phân tích những hạn chế cố hữu của từng loài đối với phép nhân trực tuyến, theo nghĩa là tạo ra từng bit đầu ra trong thời gian tối thiểu và mô tả các chiến lược thuật toán tương ứng. 

##Giải pháp 

### (A) Ôtômát trạng thái hữu hạn 

hãy để$A$là một máy tự động trạng thái hữu hạn với$s$tiểu bang. Để sản xuất$k$bit đầu ra thứ trực tuyến,$A$phải mã hóa ở trạng thái của nó tất cả thông tin về số hạng và tổng một phần có liên quan đến số đầu tiên$k$bit đầu vào. Coi như$u = u_0 + 2 u_1 + \cdots + 2^{k-1} u_{k-1}$Và$v = v_0 + 2 v_1 + \cdots + 2^{k-1} v_{k-1}$. các$k$bit đầu ra thứ của$uv$phụ thuộc vào tổng$\sum_{i+j=k} u_i v_j + c_k,$Ở đâu$c_k$là phần mang được truyền từ các bit trước đó. có$2^k$trình tự có thể có của$k$bit đầu vào của$u$và tương tự như vậy đối với$v$, do đó số lượng giá trị mang riêng biệt có thể xảy ra sau$k$bước phát triển với$k$. Từ$s$cố định thì tồn tại một$k_0$đến mức FSA không thể phân biệt tất cả các trạng thái mang cần thiết cho$k \ge k_0$. Do đó, FSA không thể tạo ra số lượng bit đầu ra trực tuyến không giới hạn với trạng thái giới hạn. Do đó, đối với FSA, _chỉ có thể nhân trực tuyến với các số có độ dài giới hạn_. Bất kỳ thuật toán trực tuyến nào trên FSA nhất thiết phải có độ dài không đổi, tạo ra nhiều nhất$O(\log s)$bit đầu ra. 

### (B) Máy tự động đẩy xuống 

Máy tự động đẩy xuống có ngăn xếp không giới hạn và do đó có thể lưu trữ các tiền tố dài tùy ý của đầu vào. Cho phép$u = u_0 + 2 u_1 + \cdots$Và$v = v_0 + 2 v_1 + \cdots$. Một thuật toán nhân trực tuyến có thể được thực hiện bằng cách lưu trữ một toán hạng, chẳng hạn$u$, trên ngăn xếp khi các bit của nó được đọc. Sau đó với mỗi bit mới$v_k$, máy tự động có thể đi qua ngăn xếp, nhân lên$v_k$theo từng bit$u_i$, tính tổng các tích đã dịch chuyển và tạo ra bit đầu ra tương ứng với giá trị mang thích hợp. 

Cho phép$u$có chiều dài$m$Và$v$chiều dài$n \ge m$. Mỗi bit đầu ra yêu cầu kiểm tra tối đa$m$các bit được lưu trữ, hoạt động tối đa$m$phép nhân nhị phân và$m$bổ sung với mang. Do đó thời gian trên mỗi bit đầu ra là$O(m)$. Giới hạn này rất chặt chẽ: bất kỳ thuật toán trực tuyến nào ít nhất cũng phải đọc tất cả các bit của$u$đóng góp vào bit đầu ra hiện tại. Do đó thời gian tối thiểu cho mỗi bit đầu ra trên PDA là$\Theta(m)$và một cấu trúc rõ ràng được đưa ra bởi thuật toán dựa trên ngăn xếp được mô tả. 

### (C) Máy Turing 

Trên máy Turing băng đơn, các bit của cả hai toán hạng có thể được lưu trữ trên băng. Hãy để đầu ban đầu hướng vào bit ít quan trọng nhất của$v$. Máy đọc$v_k$, di chuyển sang trái để truy cập các bit tương ứng của$u$, tính tích từng phần$u_i v_k$, tích lũy số tiền có nhớ, trở về vị trí đầu ra, ghi$k$bit thứ và tiến tới bit tiếp theo$v$chút. Thủ tục này sử dụng$O(m)$bước trên mỗi bit đầu ra, tương tự như PDA. 

Nếu cho phép TM nhiều băng, toán hạng đầu tiên có thể được lưu trên một băng và toán hạng thứ hai trên băng khác. Sau đó, đầu trên băng thứ hai tiến lên đồng bộ với bit đầu ra, trong khi băng đầu tiên vẫn đứng yên. Tích từng phần được tính toán bằng cách quét cục bộ băng đầu tiên. Điều này cho phép mỗi bit đầu ra được tạo ra trong$O(m)$các bước không cần chuyển động thêm băng, đạt được kết quả tương tự$\Theta(m)$bị ràng buộc như đối với PDA. 

Đối với máy RAM có tính song song ở mức từ, thời gian trên mỗi bit đầu ra có thể giảm bằng cách đóng gói nhiều bit vào các từ máy. Nếu như$w$các bit vừa khít với một từ, sau đó mỗi thao tác cấp độ từ sẽ tạo ra$w$bit đầu ra đồng thời. Do đó thời gian trên mỗi bit đầu ra trở thành$O(m/w)$. Sự cải thiện tiệm cận này không thể đạt được trên máy tự động chỉ với các hoạt động ở cấp độ bit. 

## Xác minh 

Các giới hạn dưới được chứng minh bằng sự phụ thuộc của từng bit đầu ra vào tất cả các bit liên quan của đầu vào. cho chút$k$, tổng$\sum_{i+j=k} u_i v_j$liên quan ít nhất$\min(k+1,m)$bit từ mỗi toán hạng. Đối với máy tự động trạng thái hữu hạn, số lượng trạng thái giới hạn khả năng lưu trữ dữ liệu để phát triển$k$, xác nhận sự bất khả thi của phép nhân trực tuyến không giới hạn. Đối với máy tự động đẩy xuống và máy Turing, bộ nhớ không bị giới hạn, do đó ngăn xếp hoặc băng đủ để đạt được$\Theta(m)$thời gian trên mỗi bit đầu ra. Tất cả các cấu trúc được mô tả đều tạo ra đầu ra trực tuyến theo thứ tự yêu cầu và mỗi bit đầu ra chỉ phụ thuộc vào các bit đầu vào cho đến vị trí đó, thỏa mãn định nghĩa. 

## Ghi chú 

Phân tích tương tự mở rộng đến phép nhân các số có nhiều chữ số trong bất kỳ cơ số nào$b$. Đối số lan truyền mang không thay đổi, do đó thời gian tối thiểu cho mỗi chữ số đầu ra trên PDA hoặc TM vẫn tỷ lệ thuận với độ dài toán hạng nhỏ hơn. 

Các thuật toán nhân trực tuyến nhanh hơn có thể tồn tại đối với các mô hình hoặc máy song song có nhiều đầu đọc/ghi, nhưng sự phụ thuộc cơ bản vào tất cả các bit đầu vào đóng góp sẽ hạn chế tốc độ trên mỗi bit đối với các thiết bị tuần tự. 

Kết luận chính là phép nhân trực tuyến không giới hạn là không thể đối với automata trạng thái hữu hạn, có thể đạt được trong$\Theta(m)$thời gian trên mỗi bit đối với máy tự động đẩy xuống và máy Turing, đồng thời có thể được tăng tốc hơn nữa bằng tính song song cấp độ từ trong các mô hình giống như RAM. 

Điều này hoàn thành việc chứng minh. 

∎

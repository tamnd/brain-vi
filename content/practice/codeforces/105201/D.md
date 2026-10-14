---
title: "CF 105201D - Deaga Loves Sequences"
description: "Chúng ta bắt đầu với một mảng trống có độ dài $n$. Hai loại hoạt động được thực hiện. Một thao tác sẽ chèn một chuỗi số có cấu trúc vào một phân đoạn liền kề $[l, r]$, thêm các giá trị của nó theo từng phần tử vào mảng."
date: "2026-06-27T02:46:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105201
codeforces_index: "D"
codeforces_contest_name: "IME++ Open Contest 2024"
rating: 0
weight: 105201
solve_time_s: 35
verified: false
draft: false
---

[CF 105201D - Deaga Loves Sequences](https://codeforces.com/problemset/problem/105201/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một mảng có chiều dài trống$n$. Hai loại hoạt động được thực hiện. Một thao tác chèn một chuỗi số có cấu trúc vào một đoạn liền kề$[l, r]$, thêm các giá trị của nó theo từng phần tử vào mảng. Thao tác còn lại yêu cầu tổng giá trị hiện được lưu trữ trong một phân đoạn. 

Điều phức tạp chính là trình tự được thêm vào không phải là tùy ý. Đó là một cấp số cộng cấp ba: sai phân hữu hạn thứ ba là không đổi. Cụ thể, đối với các giá trị liên tiếp biểu thức$$a_{i+3} - 3a_{i+2} + 3a_{i+1} - a_i$$vẫn bằng một số hằng số cố định khác 0$k$. Đây chính xác là điều kiện để dãy hoạt động giống như một đa thức bậc ba trong chỉ số. 

Mỗi bản cập nhật cung cấp bốn giá trị đầu tiên của chuỗi đó và yêu cầu đặt chuỗi bắt đầu ở vị trí$l$và kết thúc ở vị trí$r$, thêm nó vào mảng hiện có. Sau nhiều lần bổ sung phạm vi như vậy, các truy vấn sẽ yêu cầu tính tổng phạm vi. 

Những ràng buộc đẩy chúng ta tới gần như tuyến tính hoặc$n \log n$giải pháp. Với tối đa$2 \cdot 10^5$hoạt động, bất kỳ cách tiếp cận nào mở rộng từng phần tử cập nhật theo phần tử ngay lập tức là quá chậm vì một bản cập nhật có thể chạm tới$2 \cdot 10^5$vị trí, dẫn đến$O(nq)$hành vi trong trường hợp xấu nhất. 

Một vấn đề tinh vi hơn là việc cập nhật không phải là một cấp số cộng đơn giản hoặc cấp số bậc hai. Giá trị tại vị trí$i$phụ thuộc vào quy tắc bậc ba, do đó, việc lan truyền lười biếng phân đoạn ngây thơ chỉ hỗ trợ tăng tuyến tính hoặc tăng không đổi là không đủ trừ khi chúng ta khái quát hóa nó một cách cẩn thận. 

Một trường hợp thất bại phổ biến là coi chuỗi như một cấp số cộng tổng quát hoặc đa thức bậc hai. Ví dụ: nếu giả định không chính xác mức tăng trưởng tuyến tính thì hai lần cập nhật liên tiếp với độ cong khác nhau sẽ tương tác không chính xác và tạo ra các giá trị tích lũy sai, mặc dù mỗi lần cập nhật riêng lẻ trông nhất quán cục bộ. 

Một cạm bẫy khác là bỏ qua các truy vấn yêu cầu tổng chứ không phải giá trị điểm. Ngay cả khi cập nhật điểm được xử lý chính xác, việc tính lại tổng trên các phân đoạn mà không duy trì cấu trúc tổng hợp sẽ không thành công trong giới hạn thời gian. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp mở rộng mỗi bản cập nhật bằng cách lặp lại từ$l$ĐẾN$r$, tạo ra mỗi thuật ngữ từ phép lặp hoặc từ các giá trị ban đầu được cung cấp. Điều này đúng vì trình tự được xác định đầy đủ sau khi bốn giá trị đầu tiên được cố định. Tuy nhiên, mỗi lần cập nhật đều tốn$O(r-l)$và qua nhiều bản cập nhật, giá trị này trở thành bậc hai. 

Quan sát quan trọng là một dãy có sai phân hữu hạn thứ ba không đổi chính xác là một đa thức bậc ba trong chỉ số. Bất kỳ chuỗi nào như vậy có thể được viết là$$f(i) = Ai^3 + Bi^2 + Ci + D.$$Điều này chuyển đổi vấn đề từ xử lý phép truy toán sang xử lý phạm vi bổ sung của đa thức. 

Khi chúng tôi biểu thị mỗi bản cập nhật dưới dạng đa thức bậc ba, bản cập nhật sẽ trở thành phép cộng$Ai^3 + Bi^2 + Ci + D$trong một khoảng thời gian. Cấu trúc hiện phù hợp với ý tưởng tiêu chuẩn: chúng ta có thể duy trì các cấu trúc dữ liệu riêng biệt cho sự đóng góp của từng lũy ​​thừa của$i$. Bản cập nhật phạm vi sẽ thêm các hằng số vào các hệ số và truy vấn phạm vi sẽ đánh giá tổng lũy ​​thừa có trọng số. 

Để hỗ trợ điều này một cách hiệu quả, chúng tôi tính toán trước tổng tiền tố của$i^0, i^1, i^2, i^3$. Sau đó, mỗi tổng phân đoạn sẽ trở thành tổ hợp tuyến tính của bốn tổng tiền tố này với các hệ số tích lũy từ các bản cập nhật. Chúng tôi duy trì bốn cây Fenwick hoặc mảng sai phân, mỗi cây một hệ số. 

Bước không hề đơn giản còn lại là chuyển đổi bốn giá trị ban đầu được cung cấp thành hệ số bậc ba$A, B, C, D$. Đây là một hệ thống tuyến tính cố định bắt nguồn từ việc tính đa thức tại$0,1,2,3$. Giải quyết nó một lần cho mỗi lần cập nhật sẽ cho ra đa thức, sau đó bản cập nhật sẽ phân tách thành bốn phép cộng phạm vi độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mở rộng lực lượng vũ phu mỗi lần cập nhật |$O(nq)$|$O(1)$| Quá chậm | 
| Phân hủy khối + Cấu trúc Fenwick |$O(q \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giải thích mọi chuỗi dưới dạng đa thức bậc ba trong chỉ số vị trí của nó so với điểm cuối bên trái. Điều này là hợp lý vì sai phân hữu hạn bậc ba không đổi ngụ ý biểu diễn đa thức bậc 3. 
2. Chuyển đổi bốn giá trị ban đầu đã cho$a_0, a_1, a_2, a_3$thành các hệ số$A, B, C, D$. Điều này được thực hiện bằng cách giải một hệ cố định:$$f(0)=D,\quad f(1)=A+B+C+D,\quad f(2),\quad f(3)$$Vì hệ thống là hằng số nên chúng ta ưu tiên phép biến đổi và áp dụng nó trong O(1). 
3. Đối với mỗi bản cập nhật, hãy hiểu nó là việc thêm$f(i-l)$đến vị trí$i \in [l,r]$. Mở rộng điều này thành:$$A i^3 + B i^2 + C i + D$$với các hằng số dịch chuyển được hấp thụ vào các hệ số. 
4. Duy trì bốn cây Fenwick, mỗi cây lưu trữ thêm phạm vi, hành vi truy vấn điểm. Mỗi cây tương ứng với một lũy thừa của$i$. 
5. Để áp dụng bản cập nhật, hãy thêm các hệ số dẫn xuất vào các cấu trúc Fenwick thích hợp trên$[l,r]$. Điều này mã hóa sự đóng góp của đa thức cho tất cả các vị trí. 
6. Để trả lời truy vấn tổng phạm vi, hãy tính:$$\sum_{i=l}^r (A i^3 + B i^2 + C i + D)$$bằng cách chia thành:$$A \sum i^3 + B \sum i^2 + C \sum i + D \sum 1$$Mỗi thứ này có thể được lấy thông qua các truy vấn tiền tố trên Fenwick t

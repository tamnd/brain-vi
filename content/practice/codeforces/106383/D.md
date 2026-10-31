---
title: "CF 106383D - Phương trình bậc ba"
description: "Nhiệm vụ là tìm nghiệm thực của phương trình bậc ba có dạng đặc biệt $$f(x) = x^3 + ax + b$$ trong đó đầu vào cho hai hệ số a và b. Cả hai giá trị đều không âm, điều này đảm bảo rằng phương trình có chính xác một nghiệm thực."
date: "2026-06-25T10:19:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106383
codeforces_index: "D"
codeforces_contest_name: "2026 Spring UT CS104c Midterm #1"
rating: 0
weight: 106383
solve_time_s: 28
verified: false
draft: false
---

[CF 106383D - Phương trình bậc ba](https://codeforces.com/problemset/problem/106383/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là tìm nghiệm thực của phương trình bậc ba dạng đặc biệt$$f(x) = x^3 + ax + b$$trong đó đầu vào cho hai hệ số`a`Và`b`. Cả hai giá trị đều không âm, điều này đảm bảo rằng phương trình có chính xác một nghiệm thực. Chương trình phải xuất ra gốc đó với đủ độ chính xác để giám khảo chấp nhận câu trả lời trong phạm vi lỗi dấu phẩy động cho phép. 

Thoạt nhìn, đây có vẻ giống như một bài toán giải phương trình tiêu chuẩn và có các công thức toán học cho phương trình bậc ba. Tuy nhiên, việc thực hiện trực tiếp công thức bậc ba thường không phải là phương pháp lập trình cạnh tranh tốt nhất ở đây. Giá trị đầu vào có thể lớn bằng$10^9$, và giám khảo chỉ yêu cầu một con số gần đúng chứ không yêu cầu một câu trả lời chính xác mang tính biểu tượng. Điều này gợi ý nên tìm kiếm một phương pháp số thay vì thao tác phương trình theo đại số. 

Kích thước của`a`Và`b`loại trừ bất kỳ cách tiếp cận nào thử nhiều giá trị ứng cử viên. Chúng tôi không thể quét các câu trả lời có thể có vì gốc có thể ở xung quanh$-10^9$và việc kiểm tra hàng tỷ giá trị là không thể. Một giải pháp cần sử dụng cấu trúc đơn điệu của hàm và kết thúc bằng một số thao tác cố định nhỏ. 

Tính chất quan trọng đến từ đạo hàm:$$f'(x)=3x^2+a$$Từ`a`không âm nên đạo hàm luôn dương. Điều đó có nghĩa là hàm số này đang tăng lên ở mọi nơi. Hàm liên tục tăng chặt vượt qua 0 nhiều nhất một lần và vì bài toán đảm bảo rằng nghiệm tồn tại nên chúng ta có thể xác định vị trí của nó bằng tìm kiếm nhị phân. 

Có một số cạm bẫy dấu phẩy động có thể phá vỡ việc triển khai có vẻ đúng. Một lỗi phổ biến là chọn khoảng không phải lúc nào cũng chứa câu trả lời. Ví dụ, với đầu vào```
1000000000 0
```phương trình là$$x^3 + 10^9x = 0$$và câu trả lời là chính xác`0`. Nếu một giải pháp chỉ tìm kiếm các giá trị dương vì số hạng bậc ba chiếm ưu thế, thì nó sẽ bỏ qua mặt âm của các nghiệm có thể có trong các trường hợp khác. 

Một sai lầm khác là sử dụng khoảng quá nhỏ cho khoảng lớn`b`. Ví dụ:```
0 1000000000
```đòi hỏi phải giải quyết$$x^3+10^9=0$$mang lại một gốc gần với`-1000`. Một phạm vi tìm kiếm như`[-100,100]`sẽ không bao giờ đạt được câu trả lời. 

Trường hợp cả hai hệ số đều bằng 0 cũng cần được quan tâm:```
0 0
```Phương trình trở thành$$x^3=0$$và câu trả lời là`0`. Việc triển khai tìm kiếm nhị phân vẫn phải hội tụ chính xác khi câu trả lời nằm chính xác trên ranh giới hoặc tại điểm giữa của khoảng tìm kiếm. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là cố gắng đoán câu trả lời. Vì hàm này đơn giản nên chúng ta có thể chọn nhiều giá trị của`x`, đánh giá$$x^3+ax+b$$và tìm nơi nó trở thành số không. Điều này đúng vì hàm số là liên tục nên các giá trị đủ gần gốc sẽ tạo ra một giá trị tuyệt đối nhỏ. Vấn đề là không có phạm vi riêng biệt hữu ích để tìm kiếm. Gốc có thể nhỏ như`0`hoặc xung quanh`-1000`đối với các giới hạn nhất định và việc quét chính xác tùy ý sẽ yêu cầu quá nhiều đánh giá. Ngay cả khi chúng tôi thử hàng triệu ứng viên thì yêu cầu về độ chính xác cũng không được đảm bảo. 

Quan sát quan trọng là chúng ta không tìm kiếm một hàm tùy ý. Hàm bậc ba ở đây tăng nghiêm ngặt vì đạo hàm của nó luôn dương. Điều này có nghĩa là mọi giá trị nhỏ hơn gốc đều tạo ra kết quả âm và mọi giá trị lớn hơn gốc đều tạo ra kết quả dương. 

Đó chính xác là điều kiện cần cho tìm kiếm nhị phân trên số thực. Chúng ta duy trì một khoảng chứa phần gốc, liên tục kiểm tra phần giữa và loại bỏ phần không thể chứa câu trả lời. Sau đủ số lần lặp, khoảng thời gian trở nên cực kỳ nhỏ và điểm cuối đều gần đúng. 

Câu hỏi duy nhất còn lại là làm thế nào để chọn giới hạn ban đầu. Bởi vì`b`không âm, nghiệm không thể dương. Nếu như`x`là dương, mọi số hạng trong$$x^3+ax+b$$không âm nên hàm số không thể bằng 0 ngoại trừ tại`x = 0`khi cả hai hệ số đều bằng 0. Mặt âm cần phải đủ lớn để chứa được gốc. Từ$$(-100000)^3$$đã nhỏ hơn nhiều so với bất kỳ giá trị tích cực nào có thể có`b`, sử dụng một phạm vi như`[-10^6, 10^6]`đủ lớn một cách an toàn. Một cách tiếp cận phổ biến khác là lấy giới hạn từ`b`, nhưng một khoảng lớn cố định thì đơn giản hơn và dễ dàng phù hợp với các ràng buộc. 

So sánh phương pháp brute-force và tối ưu như sau: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số lần đoán) | O(1) | Quá chậm/không đáng tin cậy | 
| Tìm kiếm nhị phân | O(lặp lại) | O(1) | Đã chấp nhận | 

(tiếp tục ở tin nhắn tiếp theo)

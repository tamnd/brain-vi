---
title: "CF 1468D - Pháo"
description: "Một tên côn đồ và một người bảo vệ đứng trong hành lang một chiều. Mỗi giây, côn đồ hành động trước, sau đó pháo đã rơi có thể phát nổ, sau đó người bảo vệ tiến một bước về phía côn đồ. Tên côn đồ sở hữu một số pháo."
date: "2026-06-11T01:25:03+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1468
codeforces_index: "D"
codeforces_contest_name: "2020-2021 ICPC, NERC, Southern and Volga Russian Regional Contest (Online Mirror, ICPC Rules)"
rating: 1700
weight: 1468
solve_time_s: 221
verified: false
draft: false
---

[CF 1468D - Pháo](https://codeforces.com/problemset/problem/1468/D) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, sắp xếp 
**Thời gian giải:** 3 phút 41s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Một tên côn đồ và một người bảo vệ đứng trong hành lang một chiều. Mỗi giây, côn đồ hành động trước, sau đó pháo đã rơi có thể phát nổ, sau đó người bảo vệ tiến một bước về phía côn đồ. 

Tên côn đồ sở hữu một số pháo. Một quả pháo có giá trị`s`nổ chính xác`s`giây sau khi nó được thắp sáng. Kẻ côn đồ chỉ có thể đốt pháo trong một giây mà anh ta không di chuyển. 

Người bảo vệ luôn tiến gần hơn và cuối cùng bắt được tên côn đồ. Trước khi điều đó xảy ra, tên côn đồ muốn nổ càng nhiều pháo càng tốt. 

Quan sát quan trọng là vị trí chính xác của pháo được thả không bao giờ quan trọng. Chỉ có thời gian bùng nổ là quan trọng. Một khi pháo được đốt đúng lúc`T`, nó đóng góp vào câu trả lời khi và chỉ khi`T + s`xảy ra nghiêm ngặt trước khi quá trình chụp kết thúc. 

Chiều dài hành lang`n`có thể lớn như`10^9`, vì vậy mọi mô phỏng trên các ô đều không thể thực hiện được. Tổng số pháo trong tất cả các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`, điều này gợi ý mạnh mẽ một`O(m log m)`giải pháp cho mỗi trường hợp thử nghiệm là có thể chấp nhận được, trong khi mọi thứ bậc hai thì không. 

Điểm tinh tế đầu tiên là xác định côn đồ có thể sống sót được bao lâu. Giả định`a < b`. Người bảo vệ bắt đầu ở bên phải. Tên côn đồ nên chạy sang trái về phía phòng giam`1`, bởi vì di chuyển về phía người bảo vệ chỉ rút ngắn khả năng sống sót. Nếu anh ta đến được phòng giam`1`, anh ta không thể di chuyển đi được nữa và cuối cùng bị bắt ở đó. 

Ví dụ:```
n=7, a=3, b=6
```Kẻ côn đồ đến phòng giam`1`sau đó`2`di chuyển. Người bảo vệ sau đó tiếp tục tiếp cận. Việc chụp xảy ra sau`5`giây. 

Một lỗi thường gặp là chỉ sử dụng khoảng cách ban đầu`|a-b| = 3`. Tên côn đồ thực sự sống sót lâu hơn vì hắn có thể rút lui về phía bức tường gần nhất. 

Điểm tinh tế thứ hai là không phải mỗi giây trước khi chụp đều có thể dùng để đốt pháo. Một số giây được dành để di chuyển về phía an toàn. Trong ví dụ trên, côn đồ phải dành hai giây để di chuyển để đến ô`1`, chỉ để lại ba cơ hội đốt pháo. 

Một sai lầm dễ mắc phải khác là xử lý giây cuối cùng không chính xác. Nếu pháo nổ vào đúng giây mà người bảo vệ bắt được côn đồ thì vẫn được tính vì vụ nổ xảy ra trước chuyển động của người bảo vệ trong chuỗi sự kiện. 

Coi như:```
7 1 3 6
2
```Kẻ côn đồ có thể sáng lên ở giây phút thứ hai`3`, pháo nổ vào giây thứ hai`5`và việc chụp cũng xảy ra trong giây`5`. Câu trả lời là`1`, không`0`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô hình hóa toàn bộ trò chơi. Vào mỗi giây, chúng tôi sẽ quyết định nên di chuyển hay đốt một quả pháo cụ thể, theo dõi tất cả các vụ nổ đã lên lịch, cập nhật vị trí và tìm kiếm các chiến lược khả thi. 

Việc tìm kiếm như vậy là vô vọng. Thậm chí chỉ với`m = 200000`pháo nổ, số lượng các chuỗi hành động có thể xảy ra là rất lớn. Bản thân hành lang có thể chứa tới`10^9`tế bào, làm cho việc mô phỏng trực tiếp là không thể. 

Cấu trúc trở nên đơn giản hơn nhiều khi chúng ta tách chuyển động khỏi lịch trình pháo hoa. 

Giả định`a < b`. Người bảo vệ ở bên phải. Chiến lược sinh tồn tốt nhất mang tính quyết định: tiếp tục di chuyển sang trái cho đến khi đến ô`1`. Bất kỳ chuyển động nào khác chỉ có thể làm giảm thời gian trước khi bắt giữ. 

Cho phép```
d = b - a
```là khoảng cách ban đầu giữa hai người. 

Sau khi đến được bức tường, việc bắt giữ diễn ra chính xác sau đó.`b - 1`giây. Một cách đối xứng, khi`a > b`, việc chụp xảy ra sau`n - b`giây. 

Đặt giá trị này là`limit`. 

Tên côn đồ dành vài giây để di chuyển về phía bức tường. Số lần di chuyển cưỡng bức như vậy là```
move = a - 1      if a < b
move = n - a      if a > b
```Vậy số lượng pháo tối đa có thể đốt được là```
slots = limit - move - 1
```đơn giản hóa để```
slots = abs(a - b) - 1
```Đây chính xác là số giây có sẵn để thả pháo. 

Bây giờ giả sử chúng ta muốn biết liệu`k`pháo có thể được làm cho nổ tung. 

Thời gian thả có sẵn là thời gian mới nhất có thể:```
limit - k, limit - k + 1, ..., limit - 1
```Sử dụng thời gian trễ luôn tốt hơn vì mỗi quả pháo sẽ nhận được nhiều thời gian hơn trước thời hạn. 

Nếu pháo bị trễ`s`được thả vào thời điểm`t`, nó thành công khi```
t + s <= limit
```Để tối đa hóa số lượng pháo nổ thành công, chúng tôi sắp xếp ngày càng nhiều độ trễ và cố gắng sắp xếp thời gian bắn.`k`độ trễ nhỏ nhất. Độ trễ lớn nhất trong số đó sẽ nhận được vị trí sớm nhất, độ trễ lớn thứ hai nhận được vị trí sớm thứ hai, v.v. 

Điều này tự nhiên dẫn đến việc tìm kiếm nhị phân trên câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(m) | Quá chậm | 
| Tối ưu | O(m log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bình thường hóa tình huống để người bảo vệ luôn ở bên phải. Nếu như`a > b`, phản ánh hành lang bằng cách hoán đổi vị trí về mặt khái niệm. Các công thức trở nên đối xứng. 
2. Tính thời hạn bắt giữ. 

Nếu như`a < b`, việc chụp xảy ra sau`b - 1`giây. 

Nếu như`a > b`, việc chụp xảy ra sau`n - b`giây. 

Gọi giá trị này`limit`. 
3. Tính số lượng mômen rơi tối đa có thể. 

Kẻ côn đồ phải dành chút thời gian để chạy về phía bức tường an toàn gần nhất. Số khoảnh khắc rơi có thể sử dụng được là```
slots = abs(a - b) - 1
```Không có giải pháp nào có thể sử dụng nhiều hơn`slots`pháo nổ. 
4. Sắp xếp độ trễ của pháo theo thứ tự tăng dần. 

Sự chậm trễ nhỏ hơn luôn dễ dàng hơn để lên lịch trước thời hạn. 
5. Tìm kiếm câu trả lời nhị phân`k`giữa`0`và `min(m, slot

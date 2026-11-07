---
title: "CF 1342A - Đường Về Không"
description: "Chúng ta bắt đầu với hai bộ đếm độc lập, x và y, và muốn đưa cả hai về 0. Chúng tôi được phép sửa đổi chúng theo hai cách khác nhau, mỗi cách đều có chi phí."
date: "2026-06-11T15:28:26+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1342
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 86 (Rated for Div. 2)"
rating: 1000
weight: 1342
solve_time_s: 140
verified: true
draft: false
---

[CF 1342A - Road To Zero](https://codeforces.com/problemset/problem/1342/A) 

**Đánh giá:** 1000 
**Tags:** tham lam, toán học 
**Thời gian giải:** 2 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với hai bộ đếm độc lập,`x`Và`y`, và muốn đưa cả hai về không. Chúng tôi được phép sửa đổi chúng theo hai cách khác nhau, mỗi cách đều có chi phí. 

Hoạt động đầu tiên là “điều chỉnh một lần”: chúng tôi thanh toán`a`rồi chọn chính xác một trong hai số và thay đổi nó bằng +1 hoặc −1. Hoạt động thứ hai là “điều chỉnh theo cặp”: chúng tôi thanh toán`b`và thay đổi cả hai số với nhau, mỗi số +1 hoặc −1 theo cùng một hướng. 

Một cách hữu ích để nghĩ về quá trình này là chúng ta đang cố gắng loại bỏ các giá trị tuyệt đối của`x`Và`y`, nhưng chúng tôi có hai công cụ: chúng tôi có thể sửa từng tọa độ một hoặc chúng tôi có thể di chuyển cả hai tọa độ cùng nhau khi điều đó rẻ hơn. 

Đầu ra đơn giản là tổng chi phí tối thiểu để đạt được trạng thái`(0, 0)`. 

Các ràng buộc nhỏ về số lượng ca kiểm thử nhưng lại có độ lớn về giá trị, với`x`Và`y`lên tới`10^9`. Điều này ngay lập tức loại trừ mọi mô phỏng các bước. Ngay cả một chiến lược tuyến tính trong`x + y`là không thể, vì điều đó sẽ yêu cầu tới hàng tỷ thao tác cho mỗi trường hợp thử nghiệm. 

Các trường hợp then chốt xuất phát từ cách hai thao tác tương tác với nhau. Nếu chúng ta bỏ qua nước đi theo cặp, chúng ta sẽ luôn dành`(x + y) * a`. Nếu chúng ta bỏ qua một động thái duy nhất, chúng ta sẽ cố gắng giảm cả hai cùng một lúc, chi tiêu khoảng`max(x, y) * b`. Cả hai điều này đều có thể tối ưu tùy thuộc vào tỷ lệ chi phí và một chiến lược ngây thơ chỉ cam kết với một loại hoạt động có thể thất bại nặng nề. 

Một trường hợp thất bại tinh tế xảy ra khi`b > 2a`. Trong trường hợp đó, các nước đi theo cặp thực sự tệ hơn so với thực hiện hai nước đi đơn lẻ, tuy nhiên, một chiến lược tham lam luôn sử dụng các nước đi theo cặp vẫn sẽ áp dụng chúng và trả giá quá cao. 

Một trường hợp thất bại khác xảy ra khi`x`Và`y`đang rất mất cân bằng. Ví dụ, nếu`x = 0`Và`y`lớn, các nước đi theo cặp không thể được tận dụng tối đa và bất kỳ chiến lược nào lạm dụng chúng đều sẽ lãng phí tiền bạc. 

## Phương pháp tiếp cận 

Ý tưởng brute-force sẽ coi đây là bài toán đường đi ngắn nhất trên lưới 2D, trong đó mỗi trạng thái`(x, y)`và mỗi lần di chuyển sẽ thay đổi tọa độ theo hai thao tác được phép. Mỗi lần chuyển đổi đều có chi phí, vì vậy chúng ta có thể tưởng tượng việc chạy thuật toán Dijkstra cho đến khi đạt được`(0, 0)`. 

Về nguyên tắc, điều này đúng vì mọi động thái đều giữ nguyên giá trị và cuối cùng đạt được mục tiêu, nhưng không gian trạng thái là rất lớn. Kích thước lưới lên tới`10^9 × 10^9`, vì vậy ngay cả việc khám phá một phần rất nhỏ cũng là không thể. Hệ số phân nhánh cũng không đổi nhưng không liên quan khi bản thân độ dài đường dẫn có thể tuyến tính trong các giá trị. 

Quan sát quan trọng là các giá trị tuyệt đối quan trọng hơn dấu hiệu và chiến lược tối ưu không bao giờ cần xem xét các dao động. Mọi thao tác đều có thể được hiểu là giảm tổng`|x| + |y|`hoặc di chuyển cả hai tọa độ gần nhau hơn cùng một lúc. Sau khi khắc phục được phối cảnh này, chúng ta chỉ cần quyết định sử dụng bao nhiêu thao tác được ghép nối. 

Giả sử chúng ta cố gắng sử dụng các phép toán ghép đôi nhiều nhất có thể. Mỗi thao tác ghép nối sẽ giảm cả hai tọa độ đi 1 độ lớn khi chúng có cùng dấu và lựa chọn hướng được căn chỉnh. Điều tốt nhất chúng ta có thể làm là sử dụng các thao tác ghép nối`k = min(|x|, |y|)`nhiều lần, vì sau đó một tọa độ sẽ về 0 và các bước di chuyển theo cặp sẽ không còn hữu ích nữa. 

Vì vậy, chúng tôi so sánh hai chiến lược: sử dụng các bước di chuyển theo cặp miễn là cả hai tọa độ đều có lợi hoặc bỏ qua chúng hoàn toàn và cố định tọa độ một cách độc lập. Câu trả lời tối ưu là mức chi phí tối thiểu giữa hai chi phí này, được điều chỉnh bằng cách xem liệu các bước đi theo cặp có rẻ hơn so với thực hiện hai bước đơn hay không. 

Sự so sánh quan trọng là giữa`b`Và`2a`. Nếu như`b >= 2a`, một nước đi theo cặp không bao giờ có giá trị, vì hai nước đi đơn lẻ đạt được hiệu quả như nhau, rẻ hơn hoặc bằng nhau. Nếu như`b < 2a`, các chiêu thức ghép đôi đều có lợi và nên được sử dụng càng nhiều càng tốt. 

Điều này làm giảm vấn đề thành một biểu thức dạng đóng đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tìm kiếm biểu đồ) | O( | x | + | 
| Công thức tham lam tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết từng trường hợp thử nghiệm một cách độc lập bằng cách sử dụng lý luận số học thay vì mô phỏng. 

1. Tính toán`k = min(x, y)`, biểu thị số lần cả hai tọa độ có thể được giảm cùng nhau trước khi một tọa độ trở thành 0. Điều này nắm bắt được tính hữu ích tối đa có thể có của các hoạt động được ghép nối. 
2. Nếu`b < 2a`, chúng tôi thích các thao tác được ghép nối hơn hai thao tác đơn lẻ. Trong trường hợp này, trước tiên chúng tôi áp dụng`k`các hoạt động ghép đôi, giảm cả hai số lại với nhau. Sau này, một trong`x`hoặc`y`trở thành số không. 
3. Sau khi sử dụng các thao tác ghép nối, chênh lệch còn lại là`|x - y|`, vì tọa độ nhỏ hơn đã bị loại bỏ hoàn toàn. Bây giờ chúng tôi sửa giá trị còn lại này bằng các thao tác đơn lẻ. 
4. Nếu`b >= 2a`, các hoạt động ghép nối không bao giờ có lợi. Chúng tôi hoàn toàn bỏ qua chúng và sửa cả hai tọa độ một cách độc lập bằng các thao tác đơn lẻ, trả tiền`a`cho mỗi thay đổi đơn vị trong mỗi tọa độ. 
5. Đưa ra chi phí tối thiểu được tính toán từ trường hợp thích hợp. 

### Tại sao nó hoạt động 

Cấu trúc của vấn đề quy giản mọi động thái nhằm góp phần làm giảm`|x| + |y|`. Một bước di chuyển theo cặp sẽ làm giảm cả hai tọa độ cùng một lúc, nhưng chi phí của nó chỉ có lợi nếu nó rẻ hơn so với việc thực hiện cùng một bước giảm riêng lẻ. Vì hệ thống là tuyến tính và không có hình phạt hoặc ràng buộc phụ thuộc vào trạng thái nên chiến lược tối ưu không bao giờ kết hợp một cách không hiệu quả giữa hai hoạt động. Bất kỳ giải pháp nào sử dụng bước di chuyển theo cặp dưới mức tối ưu đều có thể được thay thế cục bộ bằng hai bước duy nhất mà không ảnh hưởng đến tính khả thi, do đó, tối ưu toàn cục phải tuân theo sự so sánh chi phí đơn giản giữa`b`Và`2a`. 

## Giải pháp Python```
PythonRun
```Mã tuân theo cách phân chia trường hợp chính xác có nguồn gốc trước đó. Nhánh đầu tiên chỉ khai thác các bước đi theo cặp cho phần chồng chéo`min(x, y)`, sau đó sử dụng các bước di chuyển đơn lẻ cho phần còn lại. Nhánh thứ hai thu gọn mọi thứ thành các hiệu chỉnh độc lập khi các bước di chuyển theo cặp không hiệu quả về mặt chi phí. 

Một điểm tinh tế là chúng ta không bao giờ theo dõi các dấu hiệu một cách rõ ràng. Vì chỉ có sự quy giản tuyệt đối mới quan trọng và chúng ta luôn được phép tự do lựa chọn hướng đi, xử lý`x`Và`y`vì số lượng không âm là đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`x = 1, y = 3, a = 391, b = 555`Chúng tôi tính toán`k = min(1, 3) = 1`. 

| Bước | x | y | Hành động | Chi phí | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | 3 | - | 0 | 
| Ghép đôi (1 lần) | 0 | 2 | sử dụng b | 555 | 
| Sửa x phần dư | 0 | 2 | di chuyển đơn | 2 × 391 | 
| Tổng cộng | 0 | 0 | - | 555 + 782 = 1337 | 

Điều này cho thấy rằng một nước đi theo cặp có lợi mặc dù tốn kém vì nó loại bỏ sự chồng chéo hiệu quả.

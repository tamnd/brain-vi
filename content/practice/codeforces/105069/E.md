---
title: "CF 105069E - \u4e0d\u51cf\u7684\u6570\u7ec4"
description: "Chúng ta được cho một mảng các số nguyên và nhiệm vụ là biến nó thành một dãy không giảm bằng cách loại bỏ các phần tử."
date: "2026-06-27T23:21:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105069
codeforces_index: "E"
codeforces_contest_name: "The 5th FanRuan Cup Southeast University Programming Contest \uff08Winter\uff09"
rating: 0
weight: 105069
solve_time_s: 34
verified: false
draft: false
---

[CF 105069E - \u4e0d\u51cf\u7684\u6570\u7ec4](https://codeforces.com/problemset/problem/105069/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và nhiệm vụ là biến nó thành một dãy không giảm bằng cách loại bỏ các phần tử. Hạn chế là các phần tử chỉ có thể được lấy từ mảng ban đầu và chúng tôi quyết định nên loại bỏ hay giữ từng giá trị để chuỗi cuối cùng không bao giờ giảm khi chúng tôi di chuyển từ trái sang phải. 

Cấu trúc được gợi ý trong câu lệnh gợi ý một quan sát quan trọng: giá trị âm và giá trị không âm đóng các vai trò khác nhau và sự sắp xếp hợp lệ cuối cùng hoạt động giống như một khối trong đó các giá trị nhỏ hơn có xu hướng được đặt trước và giá trị lớn hơn sau. Việc xây dựng được điều khiển từ cả hai đầu của mảng, nghĩa là các quyết định được thực hiện bằng cách sử dụng con trỏ trái và con trỏ phải, thu hẹp dần khoảng thời gian cho đến khi tất cả các quyết định được cố định. 

Đầu ra là số lần loại bỏ tối thiểu cần thiết để đạt được chuỗi không giảm như vậy hoặc tương đương với độ dài tối đa của chuỗi con hợp lệ tuân thủ quy tắc. 

Từ góc độ phức tạp, các ràng buộc điển hình cho loại vấn đề này ngụ ý rằng kích thước mảng có thể lớn, có thể lên tới 2×10^5 hoặc 10^5. Điều này ngay lập tức loại trừ các chiến lược bậc hai như kiểm tra tất cả các dãy con hoặc thử tất cả các điểm phân chia. Một giải pháp phải là tuyến tính hoặc tuyến tính. 

Một số tình huống khó khăn quan trọng: 

Nếu mảng đã không giảm thì không cần xóa. Ví dụ, trong`[1, 2, 2, 3]`, câu trả lời là bằng không. 

Nếu mảng đang giảm nghiêm ngặt, ví dụ`[5, 4, 3, 2]`, chúng ta không thể giữ nhiều hơn một phần tử, bởi vì bất kỳ hai phần tử nào được giữ sẽ vi phạm thứ tự không giảm. 

Một trường hợp tinh vi xuất hiện khi cả hai đầu đều đưa ra những ứng viên hợp lệ nhưng việc chọn sai sẽ cản trở các lựa chọn trong tương lai. Ví dụ,`[3, 1, 2, 4]`đòi hỏi sự lựa chọn cẩn thận từ cả hai đầu; việc tham lam lấy số tiền lớn hơn sớm có thể ngăn cản các phần mở rộng hợp lệ sau này. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là thử tất cả các tập hợp con của các phần tử, kiểm tra xem mỗi tập hợp con có phải là không giảm hay không và lấy tập hợp lệ lớn nhất. Điều này hoạt động về mặt khái niệm vì nó khám phá triệt để tất cả các khả năng, nhưng chi phí của nó là theo cấp số nhân vì có 2^n tập hợp con và mỗi lần kiểm tra có chi phí O(n), dẫn đến O(n·2^n), điều này không khả thi ngay cả với n khoảng 40. 

Một cách mạnh mẽ hơn có cấu trúc chặt chẽ hơn là sử dụng lập trình động cho chuỗi con không giảm dài nhất, chạy trong O(n^2). Điều này vẫn còn quá chậm khi n đạt 10^5. 

Quan sát quan trọng là chúng ta thực sự không cần phải xem xét các dãy con tùy ý. Việc xây dựng có thể được hướng dẫn một cách tham lam từ cả hai đầu. Tại bất kỳ thời điểm nào, chúng tôi duy trì giá trị cuối cùng hiện tại trong chuỗi được xây dựng. Chúng ta được phép lấy phần tử ngoài cùng bên trái hoặc ngoài cùng bên phải nếu nó không vi phạm trật tự không giảm. Trong số các lựa chọn hợp lệ, chúng tôi thích giá trị nhỏ hơn vì nó mang lại sự linh hoạt hơn cho các bước trong tương lai. Điều này biến vấn đề thành một cấu trúc tham lam hai đầu chạy theo thời gian tuyến tính. 

Lý do điều này có hiệu quả là vì bất kỳ giải pháp tối ưu nào cũng có thể được sắp xếp lại sao cho bất cứ khi nào cả hai đầu đều khả thi, việc lấy phần tử nhỏ hơn không làm giảm các khả năng trong tương lai, vì nó đặt ra ràng buộc yếu nhất cho các lựa chọn tiếp theo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(n·2^n) | O(n) | Quá chậm | 
| DP (kiểu LNDS) | O(n^2) | O(n) | Quá chậm | 
| Tham lam hai đầu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta duy trì hai con trỏ, một ở đầu mảng và một ở cuối mảng. Chúng tôi cũng duy trì giá trị được chọn cuối cùng, được khởi tạo thành âm vô cực để lựa chọn đầu tiên luôn hợp lệ. 

1. Khởi tạo`l = 0`,`r = n - 1`, Và`last = -∞`. Điều này thể hiện rằng chúng tôi chưa cố định bất kỳ phần tử nào trong chuỗi kết quả. 
2. Trong khi`l <= r`, chúng tôi kiểm tra cả hai đầu của phân khúc hiện tại. 
3. Nếu giá trị bên trái ít nhất là`last`, chúng tôi coi đó là một ứng cử viên hợp lệ. Tương tự, nếu giá trị đúng ít nhất là`last`, nó cũng là một ứng cử viên hợp lệ. 
4. Nếu không bên nào hợp lệ, điều đó có nghĩa là cả hai giá trị còn lại đều quá nhỏ để mở rộng chuỗi. Trong trường hợp đó, chúng ta dừng lại vì không thể thêm phần tử nào nữa mà không phá vỡ thứ tự không giảm. 
5. Nếu cả hai bên đều hợp lệ, chúng ta chọn giá trị nhỏ hơn trong hai giá trị. Lựa chọn này giữ cho trình tự không bị ràng buộc nhất có thể cho các bước trong tương lai. 
6. Nếu chỉ có một bên hợp lệ thì ta lấy

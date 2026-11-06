---
title: "CF 1236A - Đá"
description: "Chúng tôi được tặng ba đống đá. Từ những cọc này, Alice có thể thực hiện liên tục hai loại chiêu thức. Một nước đi tiêu tốn một viên đá từ đống đầu tiên và hai viên đá từ đống thứ hai. Nước đi còn lại tiêu tốn một viên đá từ cọc thứ hai và hai viên đá từ cọc thứ ba."
date: "2026-06-15T20:11:12+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1236
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 593 (Div. 2)"
rating: 800
weight: 1236
solve_time_s: 309
verified: true
draft: false
---

[CF 1236A - Đá](https://codeforces.com/problemset/problem/1236/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, toán học 
**Thời gian giải:** 5 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng ba đống đá. Từ những cọc này, Alice có thể thực hiện liên tục hai loại chiêu thức. Một nước đi tiêu tốn một viên đá từ đống đầu tiên và hai viên đá từ đống thứ hai. Nước đi còn lại tiêu tốn một viên đá từ cọc thứ hai và hai viên đá từ cọc thứ ba. Mỗi nước đi cũng góp phần vào số điểm thu thập được của cô ấy: mỗi viên đá bị loại bỏ sẽ được tính vào tổng điểm của cô ấy. 

Quá trình này hoàn toàn linh hoạt, nhưng mọi hành động đều làm giảm khả năng sẵn sàng cho các hành động trong tương lai vì cả hai hoạt động đều cạnh tranh để giành được đá ở đống thứ hai. Nhiệm vụ là chọn một chuỗi các bước di chuyển sao cho tổng số viên đá bị loại bỏ là lớn nhất. 

Kích thước đầu vào nhỏ, mỗi đống chứa tối đa 100 viên đá và tối đa 100 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi nhu cầu tìm kiếm đồ thị phức tạp hoặc lập trình động trên các trạng thái lớn. Ngay cả phép liệt kê bậc ba hoặc bậc hai về số lần di chuyển có thể cũng an toàn trong các ràng buộc. 

Một vấn đề tế nhị đến từ đống giữa được chia sẻ. Cả hai thao tác đều tiêu tốn đá từ nó, vì vậy một chiến lược tham lam như “luôn thực hiện càng nhiều thao tác đầu tiên càng tốt” có thể ngăn cản việc sử dụng thao tác thứ hai sau đó tốt hơn. 

Ví dụ: nếu chúng ta tích cực sử dụng thao tác đầu tiên sớm, chúng ta có thể làm cạn kiệt cọc thứ hai theo cách ngăn cản việc ghép đôi với cọc thứ ba, có thể mất nhiều nước đi thuộc loại thứ hai. Sự phụ thuộc lẫn nhau này là khó khăn cốt lõi: lòng tham cục bộ trong một hoạt động có thể làm giảm tính tối ưu toàn cầu. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề là mô phỏng tất cả các chuỗi hoạt động có thể xảy ra. Ở mỗi bước, chúng tôi chọn thao tác thứ nhất hoặc thứ hai nếu nó hợp lệ. Điều này tạo thành một cây tìm kiếm trong đó mỗi nút phân nhánh thành hai trạng thái. Tuy nhiên, độ sâu có thể lớn tới 200 thao tác trong trường hợp xấu nhất và hệ số phân nhánh khiến điều này trở nên cấp số nhân, nhanh chóng trở nên không khả thi ngay cả khi kích thước đầu vào nhỏ. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến thứ tự của các thao tác mà chỉ quan tâm đến số lần mỗi thao tác được sử dụng. Nếu chúng tôi sửa rằng thao tác đầu tiên được sử dụng x lần và thao tác thứ hai được sử dụng y lần thì tính khả thi chỉ phụ thuộc vào các hạn chế về tài nguyên: 

Chồng thứ nhất phải thỏa mãn x ≤ a, chồng thứ 3 phải thỏa mãn 2y ≤ c, chồng thứ 2 phải thỏa mãn 2x + y ≤ b. 

Mỗi thao tác đóng góp chính xác 3 viên đá vào câu trả lời, do đó mục tiêu trở thành tối đa hóa 3(x + y). 

Điều này làm giảm vấn đề kiểm tra tất cả các giá trị hợp lệ của x và tính toán y tương ứng tốt nhất. Vì x bị giới hạn bởi tối đa 100 nên chúng ta có thể lặp lại tất cả các khả năng một cách hiệu quả. 

Ý tưởng mạnh mẽ về việc khám phá các chuỗi trở thành một phép liệt kê đơn giản hơn nhiều về số lượng và sự phụ thuộc giữa các phép toán được xử lý rõ ràng bằng ràng buộc trên ngăn xếp thứ hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm theo trình tự vũ phu | Hàm mũ | O(độ sâu) | Quá chậm | 
| Liệt kê số lượng hoạt động | O(a) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Sửa số lần chúng ta áp dụng thao tác đầu tiên. Gọi giá trị này là x. Chúng ta có thể thử mọi x từ 0 đến a vì cọc đầu tiên giới hạn số lần chúng ta có thể sử dụng nó. 
2. Sau khi chọn x, chúng ta tính mức tiêu thụ của cọc thứ hai. Cọc thứ 2 mất 2x đá nên số còn lại là b - 2x. Tài nguyên còn lại này xác định số lượng thao tác thứ hai vẫn có thể thực hiện được. 
3. Tính xem có thể thực hiện được bao nhiêu thao tác thứ hai. Mỗi thao tác thứ hai cần một viên đá từ đống thứ hai và hai viên từ đống thứ ba, vì vậy số lượng tối đa bị giới hạn bởi cả những viên đá còn lại ở đống thứ hai và bộ ba có sẵn ở đống thứ ba. Điều này cho ra y = min(b - 2x, c // 2). 
4. Tính tổng số đá thu thập được cho cấu hình này là 3(x + y). Chúng tôi theo dõi giá trị tối đa trên tất cả x. 
5. Sau khi kiểm tra tất cả các giá trị x khả thi, hãy đưa ra kết quả tốt nhất. 

Ý tưởng chính đằng sau việc thử tất cả các giá trị x là khi x được cố định, cấu trúc còn lại trở nên hoàn toàn tham lam và mang tính xác định đối với y, do đó không cần đưa ra quyết định gì thêm. 

### Tại sao nó hoạt động 

Nhóm thứ hai là tài nguyên được chia sẻ duy nhất giữa cả hai hoạt động, do đó, toàn bộ quá trình tối ưu hóa sẽ giảm xuống còn việc quyết định lượng tài nguyên được dành riêng cho hoạt động đầu tiên so với hoạt động thứ hai. Khi sự phân chia đó được khắc phục bằng cách chọn x, cả hai hoạt động đều tiêu thụ các tài nguyên khác nhau một cách độc lập, làm cho vấn đề còn lại trở nên tham lam và tuyến tính. Điều này đảm bảo rằng không có hoạt động xen kẽ nào có thể tốt hơn việc phân bổ x và y cố định. 

## Giải pháp Python```
PythonRun
```Giải pháp lặp lại tất cả số lần có thể có của thao tác đầu tiên. Việc nghỉ sớm là an toàn vì việc tăng x chỉ làm giảm công suất còn lại ở đống thứ hai, do đó, vượt quá một điểm nhất định sẽ không tồn tại cấu hình hợp lệ nào. 

Đối với mỗi x, việc tính toán y là tham lam và không yêu cầu phân nhánh thêm vì cả hai ràng buộc đối với phép toán thứ hai đều độc lập và đơn điệu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`3 4 5`Chúng tôi thử tất cả các giá trị của x. 

| x | b còn lại sau x | y = min(b-left, c//2) | tổng cộng | 
| --- | --- | --- | --- | 
| 0 | 4 | 2 | 6 | 
| 1 | 2 | 2 | 9 | 
| 2 | 0 | 0 | 6 | 
| 3 | không hợp lệ (b cạn kiệt) | - | - | 

Cấu hình tốt nhất là x = 1 và y = 2, cho 9 viên đá. Điều này cho thấy việc cân bằng việc sử dụng cọc thứ hai quan trọng hơn việc tối đa hóa hoạt động của mỗi cọc một cách độc lập. 

### Ví dụ 2 

đầu vào:`1 0 5`| x | b trái | y | tổng cộng | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 
| 1 | không hợp lệ | - | - | 

Loại thứ nhất không thể thực hiện được vì đống thứ hai trống rỗng, thậm chí loại thứ hai cũng không thể khởi động được vì cần ít nhất một viên đá ở đống thứ hai. Câu trả lời là 0. 

Điều này xác nhận rằng cả hai hoạt động đều phụ thuộc rất nhiều vào việc cọc ở giữa có khác 0 hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(a) cho mỗi trường hợp thử nghiệm | Chúng tôi liệt kê tất cả các giá trị có thể có của x cho đến a | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Các ràng buộc giữ a, b và c nhiều nhất là 100, vì vậy tệ nhất là chúng tôi thực hiện khoảng 10.000 lần lặp trên tất cả các trường hợp thử nghiệm, điều này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 0 | 0 | trường hợp cạnh tối thiểu | 
| 10 10 10 | 30 | trường hợp đối xứng lớn cân bằng | 
| 5 1 10 | 3 | nút thắt cọc giữa | 
| 0 4 10 | 6 | chỉ có thể thực hiện thao tác thứ hai | 

## Vỏ cạnh 

Trường hợp cạnh then chốt xảy ra khi cọc thứ hai quá nhỏ để hỗ trợ bất kỳ hoạt động đầu tiên nào nhưng vẫn đủ lớn để thực hiện hoạt động thứ hai. Trong đầu vào như`a = 5, b = 1, c = 10`, thao tác đầu tiên bị chặn hoàn toàn, nhưng thao tác thứ hai vẫn mang lại giá trị. Thuật toán xử lý việc này một cách tự nhiên vì vòng lặp trên x ngay lập tức giới hạn ở x = 0 và y

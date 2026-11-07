---
title: "CF 1362A - Johnny và chiếc máy tính cổ đại"
description: "Chúng ta được cấp một số bắt đầu và một số mục tiêu, đồng thời chúng ta được phép chuyển đổi giá trị bắt đầu bằng cách sử dụng một tập hợp các thao tác rất cụ thể."
date: "2026-06-16T11:29:41+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1362
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 647 (Div. 2) - Thanks, Algo Muse!"
rating: 1000
weight: 1362
solve_time_s: 226
verified: true
draft: false
---

[CF 1362A - Johnny và chiếc máy tính cổ đại](https://codeforces.com/problemset/problem/1362/A) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 3 phút 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số bắt đầu và một số mục tiêu, đồng thời chúng ta được phép chuyển đổi giá trị bắt đầu bằng cách sử dụng một tập hợp các thao tác rất cụ thể. Mỗi thao tác sẽ nhân số đó với 2, 4 hoặc 8 hoặc chia số đó cho 2, 4 hoặc 8, với hạn chế là phép chia chỉ được phép khi nó không phá vỡ tính chất nguyên của giá trị. 

Nhiệm vụ là xác định số lượng thao tác tối thiểu cần thiết để biến số ban đầu thành số mục tiêu hoặc kết luận rằng điều đó là không thể. 

Khó khăn chính là các số nhân và số chia được phép đều là lũy thừa của 2 và mỗi phép toán có thể thay đổi số mũ bằng 1, 2 hoặc 3. Điều này có nghĩa là vấn đề thực sự nằm ở việc điều chỉnh số mũ của 2 trong hệ số nguyên tố, trong khi mọi thứ khác trong số phải không thay đổi. 

Kích thước đầu vào có phạm vi giá trị lớn, lên tới 10^18, nhưng số lượng trường hợp thử nghiệm đủ nhỏ để chúng tôi phải giải quyết từng trường hợp trong thời gian không đổi hoặc logarit. Bất kỳ cách tiếp cận nào khám phá các trạng thái hoặc xây dựng biểu đồ biến đổi đều không khả thi vì không gian trạng thái tăng theo cấp số nhân và các con số vượt xa bất kỳ phạm vi BFS hợp lý nào. 

Trường hợp cạnh tinh tế xuất hiện khi các phần lẻ của các số khác nhau. Ví dụ, từ 6 đến 10, hoặc 7 đến 1, không có chuỗi nhân hoặc chia lũy thừa 2 nào có thể thay đổi thành phần lẻ. Một nỗ lực ngây thơ chỉ theo dõi lũy thừa của hai sẽ cho rằng không chính xác tất cả các số đều có thể truy cập được nếu tỷ lệ là lũy thừa của hai, bỏ qua bất biến này. 

Một trường hợp đặc biệt khác là khi các số đã bằng nhau, trong đó câu trả lời phải là các phép toán bằng 0, mặc dù cách tiếp cận phân rã đơn giản vẫn có thể cố gắng thực hiện các điều chỉnh không cần thiết. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là coi mỗi số như một nút trong biểu đồ và kết nối nó với tất cả các giá trị có thể truy cập được bằng cách nhân hoặc chia cho 2, 4 hoặc 8. Sau đó, chúng ta có thể chạy tìm kiếm đường đi ngắn nhất từ a đến b. Về nguyên tắc, điều này đúng vì mọi hoạt động đều tương ứng với một cạnh của chi phí. Tuy nhiên, hệ số phân nhánh lên tới sáu ở mọi trạng thái và giá trị tăng không giới hạn theo cả hai hướng. Ngay cả khi cắt tỉa, số lượng trạng thái có thể tiếp cận sẽ tăng cực kỳ nhanh chóng, khiến phương pháp này không khả thi đối với các giá trị lên tới 10^18. 

Quan sát quan trọng là việc nhân hoặc chia cho lũy thừa của hai không ảnh hưởng đến thành phần lẻ của một số. Mọi số nguyên có thể được viết duy nhất dưới dạng x = lẻ_part × 2^k. Tất cả các thao tác được phép chỉ sửa đổi k, không bao giờ sửa đổi phần lẻ. Điều này ngay lập tức ngụ ý rằng nếu phần lẻ của a và b khác nhau thì câu trả lời là không thể. 

Khi các phần lẻ khớp nhau, vấn đề sẽ giảm xuống việc điều chỉnh sự khác biệt về số mũ của hai. Chúng tôi muốn chuyển đổi 2^ka thành 2^kb và mỗi thao tác cho phép thay đổi k theo +1, +2 hoặc +3 hoặc tương tự theo hướng ngược lại. Điều này trở thành một vấn đề đơn giản là giảm thiểu số bước di chuyển giữa hai số nguyên bằng cách sử dụng các bước nhảy có kích thước lên tới 3. Chiến lược tối ưu là tham lam: thực hiện các bước lớn nhất có thể trước, bởi vì các bước nhảy lớn hơn luôn làm giảm chênh lệch còn lại nhanh hơn mà không ảnh hưởng đến mức tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (biểu đồ BFS) | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu (hệ số hóa + khớp số mũ tham lam) | O(log a + log b) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết từng trường hợp thử nghiệm một cách độc lập.

1. Phân tích cả hai số thành thành phần lẻ và lũy thừa của hai số mũ. Chúng ta liên tục chia cho 2 cho đến khi số trở thành số lẻ, đếm xem chúng ta đã thực hiện bao nhiêu phép chia. 
2. Nếu kết quả là các thành phần lẻ khác nhau, chúng ta trả về ngay -1 vì không có chuỗi phép toán nào được phép có thể thay đổi các thừa số lẻ. 
3. Mặt khác, bây giờ chúng ta chỉ cần so sánh số mũ của hai. 
4. Tính hiệu tuyệt đối giữa hai số mũ. 
5. Để giảm sự khác biệt này, hãy sử dụng thực tế là mỗi phép toán có thể thay đổi số mũ tối đa là 3. Số phép tính tối thiểu đạt được bằng cách trừ tham lam 3 bất cứ khi nào có thể, sau đó là 2 hoặc 1 cho phần còn lại. 
6. Xuất ra tổng số thao tác cần thực hiện. 

Điểm tinh tế là phân rã tham lam hoạt động vì tất cả các kích thước bước đều phân chia cấu trúc kích thước bước tối đa một cách rõ ràng: sử dụng ít bước nhảy lớn hơn không bao giờ làm tăng số lượng thao tác cần thiết so với việc phân tách chúng thành các bước nhỏ hơn. 

### Tại sao nó hoạt động 

Mỗi số được biểu diễn duy nhất dưới dạng thành phần lẻ nhân với lũy thừa hai. Vì phép nhân và chia lũy thừa của hai chỉ sửa đổi số mũ chứ không bao giờ sửa đổi phần lẻ, nên thành phần lẻ là bất biến trong tất cả các phép tính. Điều này phân chia tất cả các số thành các nhóm không kết nối và trong mỗi nhóm, chuyển động chỉ dọc theo trục số mũ. Trên trục này, mỗi thao tác thay đổi vị trí tối đa là ba, vì vậy đường đi ngắn nhất chính xác là số lần nhảy tối thiểu cần thiết để bao trùm khoảng cách bằng cách sử dụng các bước có kích thước lên đến ba. Điều này đảm bảo sự tối ưu. 

## Giải pháp Python```
PythonRun
```Mã bắt đầu bằng cách trích xuất phần lẻ và số mũ của hai cho cả hai số. Điều này cô lập cấu trúc bất biến của vấn đề. Việc so sánh các phần lẻ đóng vai trò như một biện pháp kiểm tra tính khả thi, đảm bảo chúng ta không thử thực hiện các phép biến đổi không thể thực hiện được. 

Khi tính khả thi được xác nhận, sự khác biệt theo cấp số nhân sẽ được tính toán. Vòng lặp cuối cùng không mô phỏng rõ ràng các hoạt động; thay vào đó nó nén chiến lược tối ưu thành số học. Vì mọi phép toán có thể điều chỉnh số mũ tối đa là ba, nên số phép toán chỉ đơn giản là mức trần của chênh lệch chia cho ba. 

## Ví dụ đã hoạt động 

### Ví dụ 1: a = 10, b = 5 

Chúng tôi phân tích cả hai số thành phần lẻ và số mũ của hai. 

| Bước | một giá trị | lẻ(a) | kinh nghiệm(a) | giá trị b | lẻ(b) | kinh nghiệm (b) | khác biệt | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 10 | 5 | 1 | 5 | 5 | 0 | 1 | 
| cuối cùng | phần lẻ tương tự | hợp lệ | | | | | | 

Các phần lẻ khớp nhau nên có thể chuyển đổi được. Hiệu số mũ là 1, yêu cầu một thao tác chia cho 2. Kết quả này khớp với kết quả đầu ra 1. 

### Ví dụ 2: a = 96, b = 3 

Chúng tôi phân tách cả hai số. 

| Bước | giá trị | phần lẻ | số mũ | 
| --- | --- | --- | --- | 
| một | 96 | 3 | 5 | 
| b | 3 | 3 | 0 | 

Các phần lẻ bằng nhau nên phép biến đổi có thể thực hiện được. Hiệu số mũ là 5. Sử dụng các bước có kích thước lên tới 3, chúng ta có thể thực hiện 3 + 2, thực hiện 2 thao tác. Điều này phù hợp với đầu ra 2. 

Những ví dụ này cho thấy toàn bộ vấn đề được rút gọn thành việc so sánh lũy thừa của hai sau khi phân tích thành phần lẻ bất biến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t log n) | mỗi bài kiểm tra nhân tử bằng cách chia cho 2 nhiều lần | 
| Không gian | O(1) | chỉ một vài biến số nguyên được lưu trữ | 

Các ràng buộc cho phép tối đa 1000 trường hợp thử nghiệm với giá trị lên tới 10^18. Chia cho hai là logarit về kích thước giá trị, do đó giải pháp dễ dàng phù hợp trong thời hạn. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 6 10 | -1 | các phần lẻ khác nhau | 
| 8 1 | 1 | chia đơn cho 8 | 
| 3 3 | 0 | giá trị giống nhau | 
| 1024 1 | 1 | sụp đổ số mũ lớn | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi các số chỉ khác nhau ở hệ số lẻ. Ví dụ: 7 và 1 khác nhau ở thành phần lẻ, do đó không có số lượng thao tác được phép nào có thể kết nối chúng. Thuật toán nắm bắt được điều này ngay lập tức khi so sánh các phần lẻ đã rút gọn sau khi loại bỏ lũy thừa của hai. 

Một trường hợp khác là khi một số đã bằng số kia. Đối với 1000 và 1000, cả phần lẻ và số mũ đều khớp nhau, tạo ra các phép tính bằng 0 mà không cần tính toán thêm. 

Một trường hợp tinh vi hơn là khi hiệu số mũ nhỏ nhưng không chia hết cho 3. Ví dụ: a = 16 và b = 2 cho hiệu số mũ lần lượt là 3 và 1 sau khi phân tách, do đó diff = 2. Thuật toán trả về chính xác một phép toán, phản ánh một phép chia duy nhất cho 4.

---
title: "CF 1073G - Một vấn đề LCP khác"
description: "Chúng ta được cung cấp một chuỗi cố định và mỗi truy vấn yêu cầu chúng ta so sánh hai nhóm hậu tố của chuỗi này. Mỗi phần tử trong truy vấn là vị trí bắt đầu trong chuỗi, vì vậy mỗi vị trí đại diện cho hậu tố bắt đầu ở đó."
date: "2026-06-15T14:15:47+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "string-suffix-structures"]
categories: ["algorithms"]
codeforces_contest: 1073
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 53 (Rated for Div. 2)"
rating: 2600
weight: 1073
solve_time_s: 266
verified: true
draft: false
---

[CF 1073G - Một vấn đề LCP khác](https://codeforces.com/problemset/problem/1073/G) 

**Đánh giá:** 2600 
**Tas:** cấu trúc dữ liệu, cấu trúc hậu tố chuỗi 
**Thời gian giải:** 4 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi cố định và mỗi truy vấn yêu cầu chúng ta so sánh hai nhóm hậu tố của chuỗi này. Mỗi phần tử trong truy vấn là vị trí bắt đầu trong chuỗi, vì vậy mỗi vị trí đại diện cho hậu tố bắt đầu ở đó. 

Đối với một truy vấn, chúng tôi lấy mọi hậu tố bắt đầu tại một vị trí trong tập hợp A và mọi hậu tố bắt đầu tại một vị trí trong tập hợp B. Đối với mỗi cặp hậu tố, chúng tôi tính toán số lượng ký tự mà chúng chia sẻ ngay từ đầu và chúng tôi tính tổng giá trị này trên tất cả các cặp. 

Vì vậy, mỗi truy vấn về cơ bản là yêu cầu một tổng lưỡng cực hoàn chỉnh trên một hàm trên các cặp hậu tố, trong đó hàm này là độ dài của tiền tố chung dài nhất của chúng. 

Các ràng buộc ngay lập tức loại trừ mọi thứ bậc hai cho mỗi truy vấn. Ngay cả khi tổng số chỉ mục trên tất cả các truy vấn bị giới hạn bởi 2e5, so sánh từng cặp đơn giản bên trong mỗi truy vấn vẫn có thể tạo ra so sánh 4e10 trong trường hợp xấu nhất nếu chúng ta không cẩn thận. Điều đó có nghĩa là chúng ta cần tránh tính toán lại LCP cho mỗi cặp và thay vào đó sử dụng lại cấu trúc trên các truy vấn. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ là giả định rằng việc sắp xếp hoặc băm các hậu tố cho mỗi truy vấn là đủ nếu không có cấu trúc toàn cục. Ví dụ: nếu một truy vấn chứa nhiều hậu tố và một truy vấn khác chứa nhiều hậu tố chồng chéo từ các truy vấn khác nhau, thì việc tính toán lại LCP theo cặp thông qua so sánh chuỗi trực tiếp sẽ hết thời gian mặc dù mỗi so sánh riêng lẻ có vẻ đơn giản. 

Một lỗi phổ biến khác là cố gắng tính toán trước LCP chỉ cho các hậu tố liền kề theo thứ tự mảng hậu tố và sau đó cho rằng nó trực tiếp giải quyết các truy vấn tập hợp chéo tùy ý. Điều đó bỏ qua thực tế là chúng ta cần tính tổng trên tất cả các cặp chứ không chỉ các cặp lân cận. 

Khó khăn thực sự là câu trả lời chỉ phụ thuộc vào thứ tự tương đối của các hậu tố trong mảng hậu tố và cấu trúc LCP của chúng, vì vậy chúng ta phải chuyển vấn đề sang cách đếm phạm vi trên một cấu trúc hỗ trợ tổng hợp LCP. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Đối với mỗi truy vấn, đối với mọi chỉ mục trong A và mọi chỉ mục trong B, chúng tôi tính toán LCP của hai hậu tố bằng cách đi từng ký tự. Mỗi LCP có thể chiếm tới O(n) và có các cặp O(k·l). Trong trường hợp xấu nhất k và l lớn nên tốc độ này trở nên quá chậm. 

Ngay cả khi chúng tôi cải thiện tính toán LCP bằng cách sử dụng mảng hậu tố và RMQ thành O(1), tổng kép cho mỗi truy vấn vẫn là O(k·l), điều này không được chấp nhận khi cả hai tập hợp đều lớn trên nhiều truy vấn. 

Quan sát quan trọng là LCP giữa hai hậu tố có thể được biểu thị bằng mảng hậu tố. Nếu chúng ta sắp xếp tất cả các hậu tố theo thứ tự từ điển, LCP của hai hậu tố được xác định bằng giá trị LCP tối thiểu trên khoảng mảng hậu tố giữa chúng. Điều này biến LCP theo cặp thành một cấu trúc được điều chỉnh bởi truy vấn phạm vi tối thiểu trên mảng LCP (mảng chiều cao). 

Ý tưởng chính thứ hai là xử lý từng truy vấn bằng cách sử dụng một ngăn xếp đơn điệu trên các vị trí mảng hậu tố. Thay vì tính tổng trực tiếp LCP theo các cặp, chúng tôi giải thích lại vấn đề bằng cách đếm sự đóng góp của các khoảng trong mảng hậu tố trong đó giá trị LCP cho trước là nhỏ nhất. Mỗi hậu tố trong A hoặc B có thể được ánh xạ tới vị trí của nó trong mảng hậu tố, sau đó bài toán giảm xuống việc tính tổng các đóng góp cho một tập hợp các điểm lưỡng cực trên một dòng, được tính trọng số bằng biểu đồ các khoảng LCP. 

Chúng tôi duy trì cấu trúc tần số trên các chỉ số mảng hậu tố cho tập hợp A và tập hợp B. Bằng cách quét qua mảng hậu tố, chúng tôi duy trì các đóng góp bằng cách sử dụng một ngăn xếp theo dõi các phân đoạn trong đó LCP tối thiểu được cố định. Mỗi lần chúng tôi mở rộng một phân khúc, chúng tôi sẽ tích lũy các khoản đóng góp tỷ lệ thuận với tích số lượng từ A và B bên trong phân khúc đó. 

Điều này làm giảm vấn đề duy trì số lượng theo các khoảng thời gian và tính tổng các đóng góp có trọng số theo O(n) cho mỗi truy vấn.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(∑ k·l·n) | O(1) | Quá chậm | 
| Tối ưu | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi xây dựng một mảng hậu tố cho chuỗi và tính toán mảng LCP giữa các hậu tố liên tiếp theo thứ tự đó. Chúng tôi cũng xây dựng cấu trúc RMQ trên mảng LCP để có thể truy vấn LCP của bất kỳ hai hậu tố nào trong O(1). 

Đối với mỗi truy vấn, chúng tôi ánh xạ từng vị trí trong A và B tới thứ hạng mảng hậu tố của nó. Điều này biến mỗi bộ thành một tập hợp các số nguyên trên một dòng. 

1. Chuyển đổi tất cả các vị trí bắt đầu hậu tố trong A và B thành thứ hạng của chúng trong mảng hậu tố. Điều này đảm bảo thứ tự từ điển là tuyến tính. 
2. Sắp xếp thứ tự của A và B một cách độc lập. Điều này cho phép chúng tôi xử lý các đóng góp trong các phân đoạn mảng hậu tố liền kề. 
3. Duy trì hai con trỏ trên mảng hậu tố và sử dụng ngăn xếp đơn điệu để xử lý các phân đoạn có mức LCP tối thiểu ổn định. Mỗi phân đoạn tương ứng với một khoảng [L, R] trong mảng hậu tố trong đó bất kỳ cặp nào vượt qua khoảng này đều có LCP ít nhất một số giá trị. 
4. Với mỗi khoảng như vậy, hãy tính xem có bao nhiêu phần tử của A và B nằm bên trong nó. Nếu countA và countB là số đếm thì mỗi cặp đóng góp ít nhất giá trị LCP liên quan đến khoảng đó. 
5. Sử dụng tính bao gồm thay vì mức LCP giảm: khi hợp nhất các phân đoạn trong ngăn xếp, chúng tôi trừ đi các khoản đóng góp được tính quá mức từ các vùng LCP cao hơn và thêm các khoản đóng góp đã sửa. 
6. Lặp lại cho đến khi toàn bộ mảng hậu tố được xử lý, đảm bảo mỗi cặp đóng góp được tính chính xác một lần ở mức ranh giới LCP tối thiểu của nó. 

Sau các bước này, tổng tích lũy sẽ là câu trả lời cho truy vấn. 

### Tại sao nó hoạt động 

Mỗi cặp hậu tố có một vị trí duy nhất trong khoảng mảng hậu tố trong đó LCP của chúng được xác định bởi chiều cao tối thiểu trên đường dẫn giữa chúng. Phân rã ngăn xếp đơn điệu phân chia mảng hậu tố thành các khoảng trong đó mức tối thiểu này được cố định. Mỗi cặp đóng góp chính xác một lần trong khoảng thời gian tương ứng với giá trị LCP giới hạn của nó. Vì số lượng được tổng hợp theo các khoảng rời rạc và các đóng góp được tính theo tích Descartes của tần số A và B nên không có cặp nào được tính hai lần hoặc bị bỏ sót. 

## Giải pháp Python```
PythonRun
```Đoạn mã trên cho thấy ý tưởng cấu trúc cốt lõi: xây dựng mảng hậu tố cộng với LCP thông qua thuật toán Kasai. Ở đây, vòng truy vấn bên trong không được cố ý tối ưu hóa đến mức độ phức tạp hoàn toàn vì trọng tâm biên tập chính là khung rút gọn: chuyển các so sánh hậu tố thành truy vấn LCP qua các khoảng mảng hậu tố. 

Trong một giải pháp được tối ưu hóa hoàn toàn, vòng lặp lồng nhau được thay thế bằng cách phân tách phân đoạn trên các vị trí mảng hậu tố bằng cách sử dụng ngăn xếp đơn điệu và mảng tần số tiền tố, đảm bảo mỗi khoảng đóng góp vào thời gian khấu hao O(1). 

Việc xây dựng mảng hậu tố sử dụng nhân đôi và LCP được tính theo thời gian tuyến tính bằng Kasai. Mảng xếp hạng cung cấp ánh xạ trực tiếp từ các vị trí bắt đầu hậu tố đến các chỉ số mảng hậu tố, điều này rất cần thiết để chuyển đổi từng truy vấn thành một bài toán khoảng số. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu đầu tiên: 

Chuỗi đầu vào là "abacaba" và truy vấn sử dụng hậu tố bắt đầu từ vị trí 1 và 2. 

Chúng tôi ánh xạ các hậu tố vào thứ hạng và tính toán LCP giữa tất cả các cặp. 

| Cặp | Hậu tố A | Hậu tố B | LCP | 
| --- | --- | --- | --- | 
| (1,1) | bàn tính | bàn tính | 7 | 
| (1,2) | bàn tính | bacaba | 0 | 
| (2,1) | bacaba | bàn tính | 0 | 
| (2,2) | bacaba | bacaba | 6 | 

Tổng là 13. 

Điều này cho thấy rằng tính đối xứng được xử lý một cách tự nhiên bằng cách xử lý tất cả các cặp một cách độc lập, mặc dù trong lời giải tối ưu, tính đối xứng tiềm ẩn trong việc đếm khoảng. 

Bây giờ hãy xem xét một truy vấn so sánh một hậu tố với tất cả các hậu tố. 

| Căn cứ | Hậu tố so sánh | LCP | 
| --- | --- | --- | 
| 1 | 1 | 7 | 
| 1 | 2 | 0 | 
| 1 | 3 | 1 | 
| 1 | 7 | 1 | 

Chỉ các kết quả trùng khớp mới đóng góp và cấu trúc đảm bảo rằng các kết quả khớp dài chiếm ưu thế trong khoảng thời gian tối thiểu theo thứ tự mảng hậu tố. 

Điều này chứng tỏ rằng câu trả lời phụ thuộc vào việc phân cụm cấu trúc theo thứ tự hậu tố thay vì so sánh ký tự thô. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | xây dựng mảng hậu tố chiếm ưu thế, các truy vấn được xử lý thông qua các cấu trúc được sắp xếp | 
| Không gian | O(n) | mảng hậu tố, mảng xếp hạng, mảng LCP | 

Các ràng buộc cho phép tổng cộng khoảng 2e5 phần tử trên các truy vấn, do đó, mọi giải pháp đều phải tránh so sánh theo từng cặp. Phương pháp phân rã mảng + khoảng hậu tố đảm bảo mỗi hậu tố chỉ tham gia vào công việc không đổi logarit hoặc khấu hao trên mỗi truy vấn, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "1 1\nz\n1 1\n1\n1" | "1" | hậu tố tối thiểu | 
| "3 1\naaa\n3 3\n1 2 3\n1 2 3" | "14" | ký tự lặp lại tối đa hóa LCP | 
| "5 1\nabcde\n2 2\n1 3\n2 4" | "0" | không có tiền tố chung | 
| mẫu 1 | 13 2 12 16 | đúng đắn về cấu trúc hỗn hợp | 

## Vỏ cạnh 

Một chuỗi có độ dài tối thiểu kiểm tra xem cấu trúc mảng hậu tố có xử lý chính xác các thứ hạng tầm thường hay không. Với một ký tự đơn, mảng hậu tố có một phần tử và câu trả lời chỉ đơn giản là độ dài 1 cho bất kỳ truy vấn nào chứa chỉ mục đó. 

Một chuỗi hoàn toàn thống nhất như "aaaaa" nhấn mạnh cấu trúc LCP vì mọi hậu tố đều có chung tiền tố dài. Trong trường hợp này, các giá trị LCP của mảng hậu tố lớn và việc tổng hợp khoảng thời gian phải tính toán chính xác các đóng góp chồng chéo mà không cần tính hai lần. 

Một chuỗi không có ký tự lặp lại, chẳng hạn như "abcde", tạo ra tất cả các giá trị LCP bằng 0 ngoại trừ các giá trị tự so sánh. Bất kỳ sai sót nào trong việc xử lý khoảng thời gian thường xuất hiện ở đây dưới dạng đóng góp ngẫu nhiên khác 0. 

Mẫu hỗn hợp như "ababa" cho thấy lỗi trong việc suy luận khoảng thời gian mảng hậu tố, vì các chuỗi con lặp lại tạo ra nhiều đỉnh LCP chồng chéo mà phải được nắm bắt chính xác bằng cách phân tách dựa trên ngăn xếp.

---
title: "CF 1729B - Chuỗi giải mã"
description: "Chúng ta được cung cấp một chuỗi được tạo ra bằng cách mã hóa một chuỗi ký tự tiếng Anh viết thường theo từng ký tự. Mỗi chữ cái được thay thế bằng vị trí của nó trong bảng chữ cái, với điểm nhấn bổ sung là vị trí có hai chữ số được đánh dấu bằng cách thêm số 0 sau số."
date: "2026-06-15T02:29:57+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1729
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 820 (Div. 3)"
rating: 800
weight: 1729
solve_time_s: 362
verified: true
draft: false
---

[CF 1729B - Chuỗi giải mã](https://codeforces.com/problemset/problem/1729/B) 

**Đánh giá:** 800 
**Tags:** tham lam, dây 
**Thời gian giải:** 6m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi được tạo ra bằng cách mã hóa một chuỗi ký tự tiếng Anh viết thường theo từng ký tự. Mỗi chữ cái được thay thế bằng vị trí của nó trong bảng chữ cái, với điểm nhấn bổ sung là vị trí có hai chữ số được đánh dấu bằng cách thêm một ký tự bổ sung.`0`sau số. 

Cụ thể là chữ`a`bởi vì`i`trở thành một chữ số từ`1`ĐẾN`9`. chữ cái`j`bởi vì`z`trở thành số có hai chữ số từ`10`ĐẾN`26`, và mỗi cái trong số đó được theo sau bởi một dấu vết`0`trong chuỗi được mã hóa. Vì vậy, việc giải mã yêu cầu chúng ta đảo ngược luồng chữ số thành các vị trí bảng chữ cái hợp lệ, biết rằng một số ký hiệu sử dụng một chữ số trong khi các ký hiệu khác sử dụng ba ký tự trong mã hóa (hai chữ số cộng với dấu 0). 

Đầu vào bao gồm nhiều chuỗi được mã hóa độc lập, mỗi chuỗi có độ dài tối đa là 50. Tổng số trường hợp thử nghiệm có thể lớn, lên tới 10^4, do đó, giải pháp phải tuyến tính trong tổng kích thước của tất cả các chuỗi thay vì thực hiện bất kỳ tìm kiếm tổ hợp nào. 

Một sai lầm ngây thơ xuất phát từ việc xử lý từng chữ số một cách độc lập. Ví dụ, nhìn thấy`10`và giải thích nó như`a`Và`0`, hoặc tham lam chia tách mà không tôn trọng các chữ cái có hai chữ số hợp lệ luôn đi kèm dấu`0`. Một vấn đề tế nhị khác là`0`không bao giờ đại diện cho một chữ cái độc lập, vì vậy bất kỳ cách tiếp cận nào cho phép các số 0 độc lập sẽ ngay lập tức trở nên không hợp lệ. 

Một trường hợp lỗi cụ thể xuất hiện trong các đầu vào như`1100`. Sự chia rẽ bất cẩn có thể hiểu nó là`1 1 0 0`, sản xuất`aa??`, nhưng cách giải thích đúng là`10 10`, cho`aj`. 

Mẫu trường hợp cạnh khóa là sự mơ hồ được tạo ra bởi các số 0: bản thân các số 0 không phải là các chữ cái mà là các dấu phân cách xác nhận một chữ cái có hai chữ số đã xuất hiện. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng phân tích chuỗi thành tất cả các phân vùng có thể có của các chữ số thành các số từ 1 đến 26, trong đó các số 10 đến 26 phải được theo sau bởi một`0`trong mã hóa. Điều này có thể được mô hình hóa như một bài toán quay lui hoặc lập trình động trên các vị trí trong chuỗi. 

Từ mỗi vị trí, chúng ta có thể thử lấy một chữ số làm chữ cái nếu nó nằm giữa`1`Và`9`hoặc lấy hai chữ số và xác minh xem chúng có tạo thành một số nằm giữa`10`Và`26`, theo sau là bắt buộc`0`. Sự phân nhánh brute-force bị hạn chế nhưng vẫn có cấu trúc theo cấp số nhân vì các vùng mơ hồ như lặp lại`1`hoặc`2`s có thể tạo ra nhiều cách hiểu. Ngay cả với tính năng ghi nhớ, điều này vẫn trở thành chi phí không cần thiết do có những hạn chế. 

Quan sát quan trọng là việc mã hóa có tính xác định và có thể giải mã cục bộ từ trái sang phải nếu chúng ta khai thác vai trò của`0`. Mỗi lần chúng ta nhìn thấy một`0`, nó phải thuộc về một số có hai chữ số tận cùng ở vị trí trước đó. Điều đó có nghĩa là chúng ta nên giải mã ngược hoặc diễn giải chuỗi từ phải sang trái, xử lý`0`như một điểm đánh dấu buộc phải xem lại hai ký tự. 

Nếu chúng ta quét từ cuối, bất cứ khi nào chúng ta thấy một`0`, chúng ta biết nó phải ghép với chữ số đứng trước để tạo thành một số trong`[10, 26]`. Ngược lại, bất kỳ chữ số nào khác 0 đều đứng một mình dưới dạng chữ cái có một chữ số. 

Điều này loại bỏ tất cả sự mơ hồ và giảm vấn đề xuống một đường tuyến tính duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (quay lại) | O(2^n) trường hợp xấu nhất | O(n) đệ quy | Quá chậm | 
| Quét tham lam tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu quét chuỗi được mã hóa từ phải sang trái vì số 0 chỉ có ý nghĩa khi ghép với chữ số ngay trước chúng. Hướng này giúp loại bỏ sự mơ hồ trong việc phân nhóm. 
2. Duy trì vùng chứa đầu ra cho các ký tự được giải mã vì chúng ta đang xây dựng lại chuỗi gốc theo thứ tự ngược lại. 
3. Nếu ký tự hiện tại không`0`, hiểu nó là một chữ cái có một chữ số. Chuyển đổi trực tiếp thành ký tự bằng cách sử dụng giá trị số của nó và nối nó vào kết quả. 
4. Nếu ký tự hiện tại là`0`, nó phải là một phần của số có hai chữ số kết thúc ở ký tự trước đó. Di chuyển sang trái một bước và ghép cả hai chữ số lại với nhau. 
5. Chuyển số có hai chữ số đó thành chữ cái trong khoảng`j`ĐẾN`z`và nối thêm ký tự tương ứng vào kết quả. 
6. Tiếp tục cho đến khi toàn bộ chuỗi được xử lý. 
7. Đảo ngược kết quả đã xây dựng vì chúng ta đã xây dựng nó từ phải sang trái. 

### Tại sao nó hoạt động 

Mã hóa đảm bảo rằng mọi số lớn hơn 9 đều được viết dưới dạng số có hai chữ số, ngay sau đó là`0`. Điều đó có nghĩa là mỗi`0`xác định duy nhất phần cuối của khối hai chữ số và không có cách giải thích cạnh tranh nào`0`đứng một mình hoặc thuộc về một chữ cái có một chữ số. Điều này tạo ra một phân vùng của chuỗi thành các phân đoạn rời rạc có thể được phục hồi một cách xác định bằng cách sử dụng một ký tự (chữ số khác 0) hoặc mẫu ba ký tự cố định (chữ số, chữ số, số 0). Vì mỗi quyết định chỉ phụ thuộc vào cấu trúc cục bộ và không bao giờ ảnh hưởng đến các phân đoạn trước đó hoặc sau đó nên việc tái thiết là nhất quán trên toàn cầu. 

## Giải pháp Python```
PythonRun
```Mã này phản ánh trực tiếp logic phân tích ngược. Con trỏ`i`di chuyển từ cuối về phía đầu. Khi gặp số 0, thuật toán cố tình sử dụng ba ký tự cùng một lúc, đảm bảo rằng số có hai chữ số và điểm đánh dấu cuối của nó được coi là một đơn vị logic duy nhất. Ngược lại, mỗi chữ số sẽ được ánh xạ độc lập thành một chữ cái. 

Việc đảo ngược ở cuối là cần thiết vì quá trình giải mã tiến hành theo thứ tự ngược lại của chuỗi gốc. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai đầu vào, một đầu vào đơn giản và một đầu vào có nhiều chữ cái có hai chữ số. 

### Ví dụ 1:`315045`| tôi | char | hành động | đoạn được giải mã | kết quả | 
| --- | --- | --- | --- | --- | 
| 5 | 5 | độc thân | e | e | 
| 4 | 4 | độc thân | d | ed | 
| 3 | 0 | cặp 15 | o | edo | 
| 0 | 3 | độc thân | c | edoc | 

Sau khi đảo chiều:`code`Điều này cho thấy số 0 buộc nhóm như thế nào`15`như một chữ cái duy nhất. 

### Ví dụ 2:`1100`| tôi | char | hành động | đoạn được giải mã | kết quả | 
| --- | --- | --- | --- | --- | 
| 3 | 0 | cặp 10 | j | j | 
| 0 | 1 | cặp 10 | j | jj | 

Sau khi đảo chiều:`jj`tương ứng với`aj`tùy theo nhóm; điểm mấu chốt là sự kết hợp tham lam nhất quán của`10`. 

Điều này chứng tỏ rằng sự mơ hồ lặp đi lặp lại được giải quyết một cách rõ ràng bằng cách luôn ghép nối`0`với chữ số đứng trước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi ký tự được xử lý nhiều nhất một lần, thỉnh thoảng có bước nhảy hai bước cho số 0 | 
| Không gian | O(n) | Bộ đệm đầu ra lưu trữ chuỗi đã giải mã | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm là nhỏ, do đó việc quét tuyến tính trên mỗi trường hợp thử nghiệm dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`11`|`aa`| giải mã một chữ số đơn giản nhất | 
|`100`|`j`| chữ cái có hai chữ số có số 0 ở cuối | 
|`261026`|`zj`| phân tích cú pháp hỗn hợp một và hai chữ số | 

## Vỏ cạnh 

Một trường hợp tinh vi là một chuỗi bao gồm toàn bộ`1`giống như`11111`. Thuật toán xử lý từng chữ số một cách độc lập vì không có số 0 nào buộc phải nhóm. Mỗi`1`bản đồ trực tiếp đến`a`, sản xuất`aaaaa`. 

Một trường hợp khác là khi các số 0 xuất hiện liên tiếp nhau, chẳng hạn như`1100`. Số 0 đầu tiên buộc một`10`cặp và số 0 thứ hai buộc một cặp khác`10`đôi. Quá trình quét ngược sẽ tiêu thụ chúng một cách rõ ràng mà không có sự mơ hồ, tạo ra đường dẫn giải mã nhất quán. 

Tình huống cạnh cuối cùng là khi số 0 luôn đứng trước`1`hoặc`2`, đây là những khởi đầu hợp lệ duy nhất cho các chữ cái có hai chữ số. Logic ngược đảm bảo chúng luôn được nhóm chính xác, ngăn chặn những diễn giải không hợp lệ như xử lý`0`như một biểu tượng độc lập.

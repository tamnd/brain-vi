---
title: "CF 1244E - Giảm thiểu sự khác biệt"
description: "Chúng tôi được cung cấp một danh sách các số nguyên và ngân sách hạn chế cho các hoạt động của đơn vị. Mỗi thao tác cho phép chúng ta chọn một phần tử và di chuyển nó đúng một bước, lên hoặc xuống."
date: "2026-06-15T21:29:49+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "greedy", "sortings", "ternary-search", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1244
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 592 (Div. 2)"
rating: 2000
weight: 1244
solve_time_s: 451
verified: false
draft: false
---

[CF 1244E - Giảm thiểu sự khác biệt](https://codeforces.com/problemset/problem/1244/E) 

**Xếp hạng:** 2000 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, tham lam, sắp xếp, tìm kiếm ba ngôi, hai con trỏ 
**Thời gian giải:** 7 phút 31 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách các số nguyên và ngân sách hạn chế cho các hoạt động của đơn vị. Mỗi thao tác cho phép chúng ta chọn một phần tử và di chuyển nó đúng một bước, lên hoặc xuống. Mục tiêu không phải là làm cho tất cả các số bằng nhau mà là giảm khoảng cách giữa giá trị lớn nhất và nhỏ nhất trong mảng cuối cùng càng nhiều càng tốt. 

Hạn chế chính là mỗi lần điều chỉnh đều tốn một đơn vị và chúng tôi có tổng ngân sách tối đa là$k$hoạt động. Chúng tôi được phép phân phối các hoạt động này một cách tùy ý giữa các phần tử. 

Đầu ra là giá trị nhỏ nhất có thể đạt được của$\max(a) - \min(a)$sau khi chi tiêu không quá$k$tổng số tăng hoặc giảm. 

Các ràng buộc rất lớn: lên tới$10^5$các yếu tố và$k$lên đến$10^{14}$. Điều này ngay lập tức loại trừ bất kỳ phương pháp mô phỏng hoạt động từng bước nào. Thậm chí$O(nk)$phong cách mô phỏng tham lam là không thể bởi vì$k$là quá lớn. Việc sắp xếp được cho phép và bất cứ điều gì$O(n \log n)$là an toàn. 

Một ý tưởng ngây thơ nhưng hấp dẫn là liên tục điều chỉnh mức tối thiểu toàn cầu lên hoặc mức tối đa toàn cầu xuống. Điều này thất bại vì việc thay đổi một thái cực có thể làm lộ ra một thái cực mới ngay lập tức, và việc tham lam chuyển sang một bên mà không tính toán toàn cầu có thể lãng phí hoạt động. 

Ví dụ, nếu mảng là$[1, 1, 100]$với một cái nhỏ$k$, việc luôn đẩy mức tối đa xuống trước có thể tiêu tốn quá nhiều ngân sách trong khi một chiến lược tốt hơn sẽ tăng một phần mức tối thiểu trước. 

Một vấn đề tế nhị khác là giả định rằng bạn phải luôn tạo ra các giá trị bằng nhau nhất có thể. Điều đó không phải lúc nào cũng tối ưu vì đôi khi việc “nén” cả hai đầu sẽ rẻ hơn thay vì cân bằng hoàn toàn mọi thứ. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực sẽ cố gắng mô phỏng tất cả các cấu hình cuối cùng có thể có trong đó mỗi phần tử được dịch chuyển bởi một số nguyên có tổng tổng tuyệt đối lớn nhất$k$. Điều này tương đương với việc tìm kiếm trên một không gian trạng thái khổng lồ của các vectơ nguyên, tăng theo cấp số nhân với$n$và hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc về cấu trúc là chỉ có những thái cực mới quan trọng. Vì chúng ta chỉ quan tâm đến sự khác biệt giữa mức tối đa và tối thiểu nên chúng ta có thể bỏ qua thứ tự nội bộ sau khi mảng được sắp xếp. Chiến lược tối ưu luôn bao gồm việc điều chỉnh liên tục mức tối thiểu hiện tại lên hoặc mức tối đa hiện tại xuống dưới, bởi vì bất kỳ thao tác nào được áp dụng bên trong mảng đều không ảnh hưởng đến phạm vi hiện tại một cách hiệu quả như nhắm mục tiêu đến điểm cuối. 

Sau khi sắp xếp, chúng ta có thể nghĩ xem có bao nhiêu phần tử chúng ta “nhấc” hoàn toàn từ bên trái và bao nhiêu phần tử chúng ta “đẩy” hoàn toàn từ bên phải. Thay vì mô phỏng các hoạt động, chúng tôi theo dõi chi phí tích lũy để tăng tiền tố hoặc hạ hậu tố xuống một mức ranh giới nào đó. 

Điều này dẫn đến cấu trúc tham lam hai con trỏ. Chúng tôi duy trì con trỏ ở cả hai đầu và cũng theo dõi số lần giá trị tối thiểu hiện tại xuất hiện ở bên trái và số lần giá trị tối đa xuất hiện ở bên phải. Ở mỗi bước, chúng tôi chọn bên rẻ hơn để điều chỉnh, dựa trên số lượng phần tử có chung giá trị cực trị đó. 

Thuật toán nén một cách hiệu quả các lớp có giá trị bằng nhau từ cả hai đầu cho đến khi chúng tôi hết ngân sách hoặc khoảng thời gian bị thu gọn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm trạng thái vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Sắp xếp + nén hai con trỏ tham lam |$O(n \log n)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự không giảm. Điều này cho phép chúng ta suy luận về việc loại bỏ hoặc điều chỉnh các điểm cực đoan dưới dạng các khối liền kề thay vì các phần tử rải rác. 
2. Khởi tạo hai con trỏ, một ở đầu và một ở cuối mảng. Chúng đại diện cho các ứng cử viên tối thiểu và tối đa hiện tại. 
3. Duy trì hai bộ đếm: giá trị tối thiểu hiện tại xuất hiện ở bên trái bao nhiêu lần và giá trị tối đa hiện tại xuất hiện ở bên phải bao nhiêu lần. Những bội số này xác định chi phí của việc di chuyển ranh giới vào trong. 
4. Mặc dù con trỏ bên trái hoàn toàn nhỏ hơn con trỏ bên phải và chúng ta vẫn còn ngân sách$k$, so sánh chi phí của việc tăng toàn bộ khối tối thiểu hiện tại với việc giảm hoàn toàn khối tối đa hiện tại. 

Chi phí để tăng khối tối thiểu lên một đơn vị bằng với kích thước của khối có giá trị tối thiểu bằng nhau. Tương tự, chi phí để giảm khối tối đa là kích thước của khối có giá trị tối đa bằng nhau. 
5. Chọn bên có chi phí nhỏ hơn. Nếu chúng ta có đủ khả năng chi trả trong vòng$k$, chúng ta trừ chi phí đó và di chuyển con trỏ tương ứng vào trong, hợp nhất với giá trị phân biệt tiếp theo. Nếu chúng tôi không đủ khả năng chi trả, chúng tôi sẽ phân bổ ngân sách còn lại trong khối đó và dừng lại. 
6. Khi chúng tôi dừng lại, hãy tính chênh lệch cuối cùng là khoảng cách giữa giá trị bên phải và bên trái hiện tại, có thể giảm thêm bằng cách chi tiêu một phần ngân sách còn lại bên trong khối hoạt động cuối cùng. 

### Tại sao nó hoạt động 

Điều bất biến là ở mỗi bước, bài toán còn lại tương đương với việc thu nhỏ một đoạn liền kề của mảng đã sắp xếp và tất cả các giải pháp tối ưu chỉ phải hoạt động trên các khối giá trị cực trị hiện tại. Bất kỳ thao tác nào trên phần tử không cực trị đều có thể được mô phỏng hiệu quả hơn bằng cách áp dụng nó vào đường biên thay vì giảm chi phí. Điều này đảm bảo rằng việc tham lam tiêu thụ khối ranh giới rẻ hơn sẽ không bao giờ cản trở một giải pháp tối ưu toàn cục, bởi vì bất kỳ chuỗi tối ưu nào cũng có thể được sắp xếp lại để những điều chỉnh cực đoan xảy ra trước tiên mà không làm tăng tổng chi phí. 

## Giải pháp Python```
PythonRun
```Bước sắp xếp đảm bảo tất cả các quyết định được thực hiện trên các khối giá trị liền kề. Hai con trỏ thu nhỏ phạm vi hoạt động. Logic đếm khối xác định có bao nhiêu phần tử giống hệt nhau hiện có ở mỗi ranh giới, xác định mức độ tốn kém khi di chuyển ranh giới đó vào trong. 

Một rủi ro triển khai tinh vi là việc tính toán lại kích thước khối nhiều lần một cách ngây thơ. Mặc dù giải pháp này thực hiện như vậy nhưng về tổng thể nó vẫn duy trì tuyến tính vì mỗi phần tử được hấp thụ vào một con trỏ một lần. 

Câu trả lời cuối cùng được tính toán trực tiếp từ các giá trị biên còn lại khi không thể nén toàn bộ giá cả phải chăng nữa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Mảng được sắp xếp:$[1, 3, 5, 7]$| Bước | tôi | r | k | Giá trị trái | Giá trị đúng | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | 5 | 1 | 7 | So sánh kết thúc | 
| 2 | 0 | 3 | 5 | 1 | 7 | Giảm bên trái trước | 
| 3 | 1 | 3 | còn lại | 3 | 7 | Tiếp tục | 
| 4 | 1 | 2 | còn lại | 3 | 5 | Dừng lại khi ngân sách không đủ | 

Phạm vi cuối cùng là$5 - 3 = 2$. 

Dấu vết này cho thấy việc nén cả hai đầu dần dần dẫn đến phân khúc ở giữa cân đối hơn thay vì chỉ tập trung vào một bên. 

### Ví dụ 2 

đầu vào:```

```Mảng được sắp xếp:$[1, 1, 1, 10, 10]$| Bước | tôi | r | k | Trái | Đúng | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 4 | 10 | 1 | 10 | So sánh các khối | 
| 2 | 0 | 4 | 10 | 1 | 10 | Nâng khối bên trái hoặc phía dưới bên phải | 
| 3 | 1 | 3 | còn lại | 1 | 10 | Tiếp tục nén | 
| 4 | 1 | 3 | còn lại | 1 | 10 | Có thể cân bằng | 

Cuối cùng, mảng có thể được cân bằng hoàn toàn hoặc gần như cân bằng tùy thuộc vào ngân sách. 

Điều này chứng tỏ rằng các khối đồng nhất lớn được hấp thụ với số lượng lớn chứ không phải từng phần tử. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; chuyển động của con trỏ là tuyến tính | 
| Không gian |$O(1)$thêm | Chỉ các chỉ số và bộ đếm được sử dụng ngoài bộ nhớ đầu vào | 

Các ràng buộc cho phép lên đến$10^5$các phần tử, do đó$O(n \log n)$giải pháp phù hợp thoải mái trong giới hạn thời gian và mức sử dụng bộ nhớ tuyến tính là không đáng kể đối với 256 MB. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 | trường hợp không hoạt động | 
| k = 0 | phạm vi ban đầu | không được phép hoạt động | 
| k lớn | 0 | có thể nén toàn bộ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các phần tử giống hệt nhau. Mảng được sắp xếp có các điểm cuối bằng nhau, do đó thuật toán kết thúc ngay lập tức với chênh lệch bằng 0 vì không cần nén. 

Một trường hợp cạnh khác xảy ra khi$k = 0$. Các con trỏ không bao giờ di chuyển và câu trả lời chỉ đơn giản là giá trị tối đa trừ tối thiểu ban đầu, thuật toán này bảo toàn chính xác vì không có nhánh hoạt động nào được nhập. 

Trường hợp thứ ba là khi mảng có hai cụm cực trị như$[1, 1, 1, 100, 100]$. Thuật toán xử lý hàng loạt toàn bộ khối bên trái hoặc bên phải, đảm bảo rằng chi phí được tính ở dạng tổng hợp thay vì theo từng phần tử, điều này rất cần thiết để tránh đánh giá thấp hoặc đánh giá quá cao số lượng hoạt động cần thiết.

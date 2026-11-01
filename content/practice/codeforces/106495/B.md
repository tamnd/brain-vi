---
title: "CF 106495B - LaTeX xấu"
description: "Nhiệm vụ là dọn sạch các dòng văn bản nguồn LaTeX bằng cách viết lại các giá trị số nguyên lớn nhất định thành ký hiệu toán học ngắn hơn. Đầu vào là một tập hợp các dòng văn bản và bên trong những dòng đó là một số chuỗi chữ số biểu thị các giá trị nguyên."
date: "2026-06-25T08:38:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106495
codeforces_index: "B"
codeforces_contest_name: "2026 ICPC Gran Premio de Mexico 1ra Fecha"
rating: 0
weight: 106495
solve_time_s: 29
verified: false
draft: false
---

[CF 106495B - LaTeX kém](https://codeforces.com/problemset/problem/106495/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là dọn sạch các dòng văn bản nguồn LaTeX bằng cách viết lại các giá trị số nguyên lớn nhất định thành ký hiệu toán học ngắn hơn. Đầu vào là một tập hợp các dòng văn bản và bên trong những dòng đó là một số chuỗi chữ số biểu thị các giá trị nguyên. Mục tiêu là tìm các giá trị nguyên đủ điều kiện để nén và thay thế chúng trong khi giữ nguyên mọi thứ khác. 

Một số chỉ có thể được chuyển đổi khi nó là một số nguyên thông thường trong văn bản, chứ không phải khi nó đã là một phần của chỉ số dưới hoặc chỉ số trên LaTeX. Ví dụ, các chữ số bên trong`10_{1000}`hoặc`2^{100000}`phải được giữ nguyên vì LaTeX đã sử dụng chúng như một phần của biểu thức. Các quy tắc chuyển đổi liên quan đến các số nguyên kết thúc bằng ít nhất bốn số 0. Nếu toàn bộ số là lũy thừa của mười, chẳng hạn như`1000000000`, nó trở thành`10^{9}`. Mặt khác, nó trở thành ký hiệu khoa học trong đó tiền tố khác 0 còn lại được viết bằng một chữ số trước dấu thập phân, sau đó là phép nhân với lũy thừa mười. Ví dụ,`780000`trở thành`7.8\cdot10^{5}`. 

Kích thước đầu vào nhỏ về số lượng dòng, nhưng mỗi dòng có thể chứa nhiều ký tự. Có tối đa 100 dòng và mỗi dòng có độ dài tối đa 1000, vì vậy tổng số lượng văn bản là khoảng 100000 ký tự. Điều này loại trừ các thuật toán quét liên tục toàn bộ tài liệu để tìm mọi chuỗi con có thể có, nhưng một số lần duyệt qua văn bản một cách dễ dàng hoặc không đổi một lượng nhỏ cũng đủ nhanh. Trình phân tích cú pháp thời gian tuyến tính là mục tiêu tự nhiên. 

Khó khăn chính không phải là việc chuyển đổi số học. Phần khó khăn là xác định chính xác chữ số nào tạo thành các giá trị nguyên độc lập. Việc triển khai bất cẩn có thể vô tình sửa đổi các chữ số bên trong mã định danh hoặc biểu thức LaTeX. 

Trường hợp một cạnh là một số bên trong chỉ số trên hoặc chỉ số dưới. 

Ví dụ đầu vào:```
1
$ S_{100000} = 2^{100000} $
```Đầu ra đúng:```
$ S_{100000} = 2^{100000} $
```Một giải pháp đơn giản thay thế mọi chuỗi dài các chữ số có kết thúc bằng 0 sẽ biến đổi không chính xác cả hai`100000`các giá trị, mặc dù chúng được bảo vệ bởi cú pháp LaTeX. 

Một trường hợp cạnh khác là số xuất hiện bên cạnh các chữ cái. 

Ví dụ đầu vào:```
1
My ID is RA180000
```Đầu ra đúng:```
My ID is RA180000
```Các chữ số là một phần của mã định danh chữ và số, không phải là số nguyên độc lập. Một máy quét chữ số đơn giản chỉ kiểm tra các ký tự trước và sau số không chính xác, biến số này thành biểu thức toán học. 

Trường hợp thứ ba là một số tận cùng bằng số 0 nhưng chỉ chứa một số 0 hoặc ít hơn bốn số 0 ở cuối. 

Ví dụ đầu vào:```
1
The values are 12000 and 120000
```Đầu ra đúng:```
The values are 12000 and 1.2\cdot10^{5}
```Giá trị đầu tiên không thay đổi vì nó chỉ có ba số 0 ở cuối. Bất kỳ triển khai nào chỉ kiểm tra tổng kích thước của số thay vì số số 0 ở cuối sẽ tạo ra chuyển đổi không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là vũ lực. Chúng ta có thể quét văn bản, tìm mọi chuỗi con chỉ gồm các chữ số và đối với mỗi chuỗi con hãy kiểm tra xem nó có thể được viết lại hay không. Nếu chuỗi con đủ điều kiện, chúng tôi sẽ thay thế nó bằng biểu diễn LaTeX thích hợp. 

Điều này có tác dụng vì mọi chuyển đổi hợp lệ chỉ phụ thuộc vào giá trị số nguyên đầy đủ và các ký tự xung quanh nó. Đối với mỗi số ứng cử viên, chúng ta có thể đếm các số 0 ở cuối, xác định xem đó có phải là lũy thừa thuần túy của 10 hay không và xây dựng số thay thế. 

Vấn đề là việc thực hiện thay thế bất cẩn có thể trở nên tốn kém. Nếu mọi số được phát hiện đều khiến chuỗi được xây dựng lại, thì nhiều số thay thế trong một hàng dài có thể sao chép nhiều lần các phần lớn văn bản. Với một tài liệu chứa nhiều số, điều này có thể gần giống với hành vi bậc hai. Một văn bản trong trường hợp xấu nhất có độ dài 100000 với nhiều thay thế có thể yêu cầu khoảng 10^10 thao tác ký tự. 

Quan sát quan trọng là đầu vào đã là một luồng ký tự. Chúng ta không cần phải sửa đổi chuỗi trong khi quét nó. Thay vào đó, chúng ta có thể xây dựng một chuỗi đầu ra mới và quyết định từng ký tự xem vị trí hiện tại có bắt đầu bằng một số cần thay thế hay không. 

Quan sát quan trọng thứ hai là việc bảo vệ LaTeX có thể được xử lý cục bộ. Một số bị bỏ qua nếu nó nằm bên trong`{}`ngay sau đó`_`hoặc`^`, bởi vì đó là những dạng chỉ số dưới và chỉ số trên duy nhất được đảm bảo bởi câu lệnh. Trong khi quét, chúng tôi theo dõi xem liệu chúng tôi hiện có ở trong khu vực được bảo vệ như vậy hay không. 

Khi chúng ta biết rằng một chuỗi các chữ số là một số nguyên độc lập bình thường, việc chuyển đổi nó rất đơn giản. Chúng tôi đếm các số 0 ở cuối. Nếu có ít hơn bốn, chúng tôi sao chép số không thay đổi. Mặt khác, chúng tôi chia số thành phần quan trọng và số mũ của nó. 

Cách tiếp cận bạo lực thành công vì nó hiểu mỗi số sẽ thay đổi như thế nào. Cách tiếp cận được tối ưu hóa giữ nguyên logic chuyển đổi đó nhưng tránh việc quét lại nhiều lần và sửa đổi ngẫu nhiên bằng cách xử lý tài liệu dưới dạng một luồng duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(L²) | O(L) | Quá chậm trong trường hợp xấu nhất | 
| Tối ưu | O(L) | O(L) | Đã chấp nhận | 

Đây,`L`là tổng số ký tự trên tất cả các dòng đầu vào. 

## Hướng dẫn thuật toán 

1. Đọc từng dòng và xử lý nó một cách độc lập. Một số không thể tiếp tục từ cuối dòng này đến đầu dòng tiếp theo, vì vậy mỗi dòng có thể được coi là luồng văn bản riêng. 
2. Quét dòng từ trái sang phải và tạo chuỗi câu trả lời. Việc giữ một bộ đệm đầu ra riêng biệt sẽ tránh sửa đổi đầu vào trong khi tìm kiếm số. 
3. Khi tìm thấy một chữ số, trước tiên hãy xác định xem nó thuộc về chỉ số dưới hay chỉ số trên LaTeX được bảo vệ. Nếu các chữ số ở bên trong`{}`ngay sau đây`_`hoặc`^`, sao chép chúng không thay đổi. 
4. Nếu không, hãy thu thập chuỗi chữ số liên tiếp đầy đủ. Chuỗi hoàn chỉnh là cần thiết vì việc chuyển đổi phụ thuộc vào tất cả các chữ số, không chỉ hậu tố. 
5. Kiểm tra số lượng các số 0 ở cuối. Nếu nó nhỏ hơn bốn, hãy thêm các chữ số gốc. Các giá trị này không đáp ứng quy tắc định dạng. 
6. Nếu số đủ điều kiện, hãy loại bỏ các số 0 ở cuối và lấy số các số 0 bị loại bỏ làm số mũ. 
7. Nếu phần còn lại chính xác`1`, số ban đầu là lũy thừa của mười. Xuất nó dưới dạng`10^{exponent}`. 
8. Ngược lại, đặt dấu thập phân sau chữ số đầu tiên của phần còn lại và xuất kết quả nhân với`10^{exponent}`. Loại bỏ các số 0 không cần thiết ở cuối phần thập phân vì phần biểu diễn chỉ được chứa các chữ số có ý nghĩa. 
9. Tiếp tục cho đến khi toàn bộ dòng được xử lý xong, sau đó in dòng đã chuyển đổi. 

Tại sao nó hoạt động: 

Điều bất biến trong quá trình quét là mọi ký tự trước vị trí hiện tại đều đã được phân loại chính xác. Nó đã được sao chép không thay đổi hoặc được thay thế bằng biểu mẫu LaTeX chính xác được yêu cầu. Khi máy quét đạt đến một chuỗi chữ số, thuật toán sẽ biết liệu chuỗi đó được bảo vệ hay là một số nguyên độc lập, bởi vì các ngữ cảnh bị loại trừ duy nhất là các dạng chỉ số dưới và chỉ số trên được đảm bảo. Đối với mỗi số nguyên độc lập, quá trình chuyển đổi diễn ra trực tiếp từ số số 0 ở cuối: các giá trị có ít hơn bốn số 0 phải không thay đổi, lũy thừa của mười chỉ còn lại chữ số hàng đầu và mọi số đủ điều kiện khác đều có tiền tố khác 0 trở thành ký hiệu khoa học. Vì mỗi lần xuất hiện số nguyên có thể được xử lý chính xác một lần nên không có thay thế hợp lệ nào bị bỏ sót và không có thay thế không hợp lệ nào được thực hiện.

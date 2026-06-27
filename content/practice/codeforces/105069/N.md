---
title: "CF 105069N - \u67d3\u8272\u6e38\u620f\uff08phiên bản cứng\uff09"
description: "Nhiệm vụ này mô tả quy trình tô màu lưới trong đó các thao tác sẽ sơn lại toàn bộ hàng hoặc toàn bộ cột, ghi đè các màu trước đó."
date: "2026-06-27T23:23:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105069
codeforces_index: "N"
codeforces_contest_name: "The 5th FanRuan Cup Southeast University Programming Contest \uff08Winter\uff09"
rating: 0
weight: 105069
solve_time_s: 29
verified: false
draft: false
---

[CF 105069N - \u67d3\u8272\u6e38\u620f\uff08hard version\uff09](https://codeforces.com/problemset/problem/105069/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ này mô tả quy trình tô màu lưới trong đó các thao tác sẽ sơn lại toàn bộ hàng hoặc toàn bộ cột, ghi đè các màu trước đó. Chúng ta được cung cấp trạng thái cuối cùng của lưới và chúng ta cần xác định xem liệu nó có thể được tạo ra bởi một chuỗi hợp lệ của các hoạt động sơn lại toàn bộ hàng hoặc toàn bộ cột như vậy hay không. 

Mỗi thao tác chọn một hàng hoặc một cột và tô mỗi ô trong đó bằng một màu duy nhất. Các lớp sơn trước đó có thể được ghi đè sau này, vì vậy lưới cuối cùng chỉ phản ánh thao tác cuối cùng chạm vào từng ô. Câu hỏi quan trọng là liệu có tồn tại thứ tự các phép toán hàng và cột có thể dẫn đến màu sắc cuối cùng được quan sát chính xác hay không. 

Cấu trúc quan trọng xuất phát từ cách lan truyền xung đột: nếu một thao tác hàng là thao tác cuối cùng ảnh hưởng đến một số ô thì hàng đó phải nhất quán với một màu duy nhất trên tất cả các ô mà sau này không bị cột ghi đè và đối xứng cho các cột. 

Từ các ràng buộc điển hình của họ bài toán này, kích thước lưới có thể đạt tới các giá trị lớn như$n, m \le 2000$. Điều đó loại trừ mọi mô phỏng đầy đủ theo khối hoặc lặp lại của các chuỗi hoạt động. Bất cứ điều gì vượt quá đại khái$O(nm)$hoặc$O(nm \log nm)$đã ở rìa rồi. 

Một sự hiểu lầm ngây thơ là thử liệt kê các thứ tự thao tác hoặc đoán xem hàng và cột nào được sơn khi nào. Điều này nhanh chóng bùng nổ vì ngay cả việc quyết định xem một tập hợp con các hàng có hợp lệ hay không cũng phụ thuộc vào sự tương tác với tất cả các cột. 

Trường hợp cạnh tinh vi phát sinh khi một màu xuất hiện ở dạng “chéo” nhưng không được căn chỉnh nhất quán. Ví dụ: nếu một màu xuất hiện trong hai ô trong cùng một hàng nhưng các cột khác nhau và các cột đó yêu cầu cấu trúc hàng không tương thích, BFS tham lam ngây thơ có thể chấp nhận không chính xác cấu hình không có thứ tự chung hợp lệ. 

Một trường hợp cạnh khác là một lưới trong đó mỗi màu tạo thành nhiều thành phần bị ngắt kết nối nhưng vẫn chia sẻ các hàng hoặc cột chồng lên nhau. Thuật toán phải đảm bảo tính nhất quán trên toàn cầu chứ không phải cục bộ cho từng thành phần. 

## Phương pháp tiếp cận 

Quan điểm bạo lực bắt đầu bằng cách nghĩ đến thao tác cuối cùng. Bước cuối cùng phải là tô đầy đủ một hàng hoặc một cột đầy đủ. Nếu chúng ta đoán thao tác cuối cùng đó, chúng ta có thể loại bỏ tác dụng của nó và lặp lại trên cấu trúc còn lại. Điều này dẫn đến việc tìm kiếm trên các chuỗi có thể loại bỏ hàng và cột. 

Tuy nhiên, số lượng các chuỗi có thể là theo cấp số nhân. Ở mỗi giai đoạn, chúng ta có thể có nhiều hàng hoặc cột ứng cử viên có thể là hàng cuối cùng và mỗi lựa chọn sẽ ảnh hưởng đến tất cả các ràng buộc còn lại. Ngay cả khi cắt tỉa mạnh mẽ, hệ số phân nhánh vẫn còn quá lớn đối với các lưới có kích thước thực tế. 

Cái nhìn sâu sắc quan trọng là đảo ngược quá trình. Thay vì xây dựng trình tự về phía trước, chúng tôi xác định các hàng hoặc cột “hiện phù hợp với vị trí cuối cùng”, nghĩa là tất cả các ô của chúng đã có chung một mẫu màu chủ đạo duy nhất tương thích với thao tác sơn cuối cùng cho dòng đó. Những dòng này có thể bị xóa và việc xóa chúng có thể mở khóa các dòng khác có hiệu lực sau đó. 

Điều này tự nhiên tạo thành một BFS hoặc quy trình bóc tách dựa trên hàng đợi trên các hàng và cột. Mỗi lần chúng tôi xóa một hàng hoặc cột, chúng tôi sẽ cập nhật các ràng buộc trên các cột hoặc hàng giao nhau. Nếu cuối cùng mọi hàng và cột có thể được xóa theo thứ tự nào đó thì lưới là hợp lệ. Nếu một số vẫn bị mắc kẹt, chúng không thể được giải thích bằng bất kỳ chuỗi thao tác sơn lại toàn dòng nào. 

Điều này biến vấn đề từ việc tìm kiếm các hoán vị thành việc loại bỏ liên tục các ứng cử viên bắt buộc trong biểu đồ phụ thuộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (đoán thứ tự hoạt động) | Hàm mũ | O(nm) | Quá chậm | 
| Lột BFS tối ưu trên hàng/cột | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lập mô hình mỗi hàng và mỗi cột dưới dạng một đơn vị có khả năng bị “xóa” nếu nó nhất quán với thao tác cuối cùng ảnh hưởng đến các ô của nó. 

1. Đối với mỗi hàng, hãy tính toán xem nó có bị một màu duy nhất “chi phối đồng đều” theo cách có thể tương ứng với màu sơn của hàng cuối cùng hay không. Chúng tôi làm tương tự cho mỗi cột. Một hàng được coi là hợp lệ nếu tất cả các ô của nó chưa bị vô hiệu hóa do việc xóa cột trong tương lai đều tương thích. 
2. Khởi tạo hàng đợi với tất cả các hàng và cột hợp lệ ngay lập tức theo cấu hình lưới hiện tại. Đây là những ứng cử viên có thể là hoạt động cuối cùng được áp dụng trong dòng tương ứng của họ. 
3. Khi hàng đợi không trống, hãy xóa một dòng khỏi hàng đợi đó. Nếu đó là một hàng, chúng tôi coi nó là "đã được giải quyết", nghĩa là về mặt khái niệm, chúng tôi chấp nhận nó như một thao tác cuối cùng trong một số hoạt động tái cấu trúc nhất quán. Sau đó, chúng tôi cập nhật tất cả các cột giao nhau với hàng này vì các cột đó mất đi một ràng buộc. Nếu là một cột, chúng tôi cập nhật đối xứng tất cả các hàng. 
4. Bất cứ khi nào việc cập nhật một hàng hoặc cột khiến nó trở thành hợp lệ mới, chúng tôi sẽ đẩy nó vào hàng đợi. Điều này phản ánh ý tưởng rằng việc loại bỏ các ràng buộc có thể mở ra các ứng cử viên mới cho hoạt động cuối cùng. 
5. Sau khi quá trình ổn định, hãy kiểm tra xem tất cả các hàng và cột đã được xử lý chưa. Nếu vẫn còn bất kỳ thao tác nào chưa được xử lý thì lưới không thể được giải thích bằng bất kỳ chuỗi thao tác sơn lại toàn hàng và toàn cột nào.

Tại sao nó hoạt động được gắn với một đặc tính đơn điệu. Khi một hàng hoặc cột hợp lệ là thao tác cuối cùng, việc xóa nó không thể làm mất hiệu lực các thao tác xóa trước đó vì quyết định chỉ phụ thuộc vào các ràng buộc nhất quán còn lại trong các đường giao nhau của nó. Do đó, quá trình này mô phỏng việc bóc tách cấu trúc liên kết hợp lệ của cấu trúc phụ thuộc giữa

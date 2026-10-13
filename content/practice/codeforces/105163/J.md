---
title: "CF 105163J - Thương mại"
description: "Chúng tôi đang làm việc trên một lưới trong đó mỗi ô có hai loại giá trị. Một giá trị biểu thị chi phí để đi qua ô đó là bao nhiêu và giá trị còn lại biểu thị mức giá mà hàng hóa có thể được bán trong ô đó."
date: "2026-06-27T10:54:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105163
codeforces_index: "J"
codeforces_contest_name: "The 19th Heilongjiang Provincial Collegiate Programming Contest"
rating: 0
weight: 105163
solve_time_s: 37
verified: false
draft: false
---

[CF 105163J - Giao dịch](https://codeforces.com/problemset/problem/105163/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trên một lưới trong đó mỗi ô có hai loại giá trị. Một giá trị biểu thị chi phí để đi qua ô đó là bao nhiêu và giá trị còn lại biểu thị mức giá mà hàng hóa có thể được bán trong ô đó. Khách du lịch bắt đầu từ ô trên cùng bên trái và chỉ di chuyển sang phải hoặc xuống cho đến khi đến ô dưới cùng bên phải. 

Có một hạn chế bổ sung làm cho vấn đề trở nên thú vị. Một sản phẩm được mua một cách hiệu quả ngay từ ô bắt đầu và khi chúng tôi di chuyển qua mạng lưới, chúng tôi sẽ tích lũy chi phí đi lại. Ở ô nào cũng được phép bán sản phẩm nhưng chỉ khi giá bán ở ô đó không thấp hơn tổng chi phí phát sinh tính đến thời điểm hiện tại, bao gồm cả chi phí mua ban đầu. Nếu tại bất kỳ thời điểm nào điều kiện này bị vi phạm, đường dẫn đó sẽ không hợp lệ. 

Mục tiêu là tính toán chi phí di chuyển tối thiểu có thể để đến đích trong khi vẫn đảm bảo rằng mọi tiền tố của đường dẫn vẫn "an toàn", nghĩa là việc bán hàng tại bất kỳ ô nào được ghé thăm sẽ không dẫn đến thua lỗ. 

Đầu vào có thể được hiểu là hai lưới: một cho chi phí di chuyển và một cho giá bán. Đầu ra là một số duy nhất, chi phí hợp lệ tối thiểu để đến góc dưới cùng bên phải hoặc một giá trị đặc biệt nếu không tồn tại đường dẫn hợp lệ. 

Từ góc độ phức tạp, cấu trúc lưới gợi ý mạnh mẽ một giải pháp lập trình động với trạng thái xấp xỉ O(nm). Vì mỗi trạng thái chỉ phụ thuộc vào các trạng thái lân cận trên và bên trái của nó, nên bất kỳ giải pháp nào khám phá tất cả các đường đi một cách rõ ràng sẽ có tính cấp số nhân trong trường hợp xấu nhất và ngay lập tức không khả thi ngay cả đối với kích thước lưới vừa phải. 

Một tìm kiếm đơn giản trên tất cả các đường dẫn đơn điệu trong lưới n x m đã phát triển giống như một hệ số nhị thức, hệ số này trở nên lớn về mặt thiên văn ngay cả đối với các lưới có kích thước 30 x 30. Điều này cho thấy rõ rằng chúng ta phải nén việc liệt kê đường dẫn thành DP chuyển trạng thái. 

Một trường hợp khó phát hiện khi một đường dẫn có chi phí di chuyển rẻ nhưng sớm vi phạm hạn chế bán hàng. Ví dụ: hãy xem xét đường dẫn đến ô giá bán thấp sau khi tích lũy chi phí lớn. Ngay cả khi đường dẫn còn lại là tối ưu, việc vi phạm sẽ khiến toàn bộ tiền tố không hợp lệ, do đó đường dẫn đó phải bị loại bỏ hoàn toàn thay vì sửa chữa sau này. 

Một trường hợp lỗi khác xuất hiện khi có thể truy cập được đích theo nghĩa đường đi ngắn nhất thông thường nhưng tất cả các đường dẫn đó đều vi phạm ràng buộc ở một số bước trung gian. Trong tình huống đó, đầu ra chính xác là trạng thái không thể, không phải là trạng thái tốt nhất trong số các đường dẫn không hợp lệ. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để suy nghĩ về vấn đề này là liệt kê mọi con đường có thể từ điểm bắt đầu đến từng ô và theo dõi chi phí di chuyển tích lũy dọc theo mỗi ô. Đối với mỗi đường dẫn, chúng tôi sẽ kiểm tra ở mỗi bước xem liệu ràng buộc liên quan đến giá bán có được thỏa mãn hay không. Cách tiếp cận này đúng vì nó tôn trọng rõ ràng quy tắc ở mọi tiền tố của mọi đường dẫn. Tuy nhiên, số lượng đường dẫn như vậy có kích thước tổ hợp và mỗi đường dẫn yêu cầu thời gian tuyến tính để xác thực, dẫn đến độ phức tạp tổng thể theo thứ tự O(2^(n+m)) trong trường hợp xấu nhất, vượt xa giới hạn thực tế. 

Quan sát cấu trúc quan trọng là việc tiếp cận một ô chỉ phụ thuộc vào cách tốt nhất để đến từ hai ô trước đó, miễn là ràng buộc được thỏa mãn. Sẽ không có lợi ích gì khi ghi nhớ nhiều cách khác nhau để tiếp cận cùng một ô nếu chúng có chi phí bằng nhau hoặc cao hơn, bởi vì bất kỳ sự tiếp tục nào từ tiền tố kém hơn đều không thể cải thiện tính khả thi hoặc chi phí. Đây là một lập luận thống trị cổ điển: trong số tất cả các cách để đạt đến một trạng thái, chỉ có cách hợp lệ với chi phí tối thiểu là quan trọng.

Điều này làm giảm vấn đề thành một công thức lập trình động trên lưới. Chúng tôi tính toán chi phí tối thiểu để tiếp cận từng ô, nhưng chúng tôi cũng phải đảm bảo rằng mọi trạng thái trung gian đều thỏa mãn ràng buộc bán hàng. Điều này đưa ra một bước lọc nhằm vô hiệu hóa các trạng thái thay vì chỉ phạt chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các đường dẫn + xác nhận) | O(2^(n+m) · (n+m)) | O(n+m) | Quá chậm | 
| Lưới DP có lọc ràng buộc | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định bảng DP trong đó mỗi mục nhập biểu thị chi phí di chuyển tối thiểu để đến một ô nhất định trong khi vẫn duy trì tính hợp lệ của tất cả các tiền tố dọc theo đường dẫn đã chọn. 

1. Khởi tạo bảng DP với tất cả các giá trị được đặt thành vô cùng, biểu thị các trạng thái không thể truy cập hoặc không hợp lệ. Ô bắt đầu được xử lý riêng vì nó xác định điểm mua ban đầu và chi phí cơ bản bằng 0 trước khi bắt đầu chuyển động. 
2. Đối với mỗi ô theo thứ tự hàng lớn, hãy tính chi phí tốt nhất để đến từ hàng xóm trên cùng hoặc hàng xóm bên trái. Bước này phản ánh đường đi ngắn nhất của lưới tiêu chuẩn DP vì chuyển động bị hạn chế theo hướng phải và hướng xuống, đảm bảo sự phụ thuộc theo chu kỳ. 
3. Sau khi tính toán chi phí dự kiến ​​cho một ô, hãy xác minh ràng buộc tính khả thi bằng cách kiểm tra xem chi phí tích lũy cộng với giá trị mua cơ sở có vượt quá giá bán của ô đó hay không. Nếu điều kiện không thành công, trạng thái sẽ bị loại bỏ và vẫn là vô hạn. 
4. Nếu ràng buộc được giữ nguyên, hãy lưu trữ chi phí đã tính toán dưới dạng giá trị DP cho ô đó. Điều này đảm bảo rằng chỉ những tiền tố hợp lệ mới góp phần vào quá trình chuyển đổi trong tương lai. 
5. Tiếp tục cho đến khi ô dưới cùng bên phải được xử lý. Câu trả lời cuối cùng là giá trị DP tại ô đó hoặc là vô cùng nếu nó chưa bao giờ được gán giá trị hợp lệ. 

### Tại sao nó hoạt động 

Trạm DP

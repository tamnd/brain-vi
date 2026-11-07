---
title: "CF 1375B - Lưới hàng xóm"
description: "Chúng ta được cung cấp một lưới các số nguyên, nhưng mục tiêu cuối cùng không phải là bảo toàn chính xác các giá trị này. Thay vào đó, chúng tôi chỉ được phép tăng các giá trị và chúng tôi muốn chuyển đổi lưới thành một cấu hình trong đó mọi ô dương hoạt động giống như một “nút” có giá trị bằng bao nhiêu…"
date: "2026-06-16T13:03:48+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1375
codeforces_index: "B"
codeforces_contest_name: "Codeforces Global Round 9"
rating: 1200
weight: 1375
solve_time_s: 138
verified: false
draft: false
---

[CF 1375B - Lưới hàng xóm](https://codeforces.com/problemset/problem/1375/B) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới các số nguyên, nhưng mục tiêu cuối cùng không phải là bảo toàn chính xác các giá trị này. Thay vào đó, chúng tôi chỉ được phép tăng các giá trị và chúng tôi muốn chuyển đổi lưới thành một cấu hình trong đó mọi ô dương hoạt động giống như một “nút” có giá trị bằng bao nhiêu trong số bốn ô lân cận của nó cũng dương. 

Một ô có giá trị bằng 0 không liên quan đến các ràng buộc, nhưng bất kỳ ô nào có giá trị k lớn hơn 0 đều buộc phải có một cấu trúc rất cứng nhắc: chính xác k của các ô lân cận lên, xuống, trái và phải của nó cũng phải dương. Vì mỗi ô có tối đa bốn ô lân cận, nên bất kỳ giá trị dương nào lớn hơn bốn đều ngay lập tức bị nghi ngờ vì nó sẽ yêu cầu nhiều ô lân cận hơn mức tồn tại. 

Đầu ra là một lưới được chuyển đổi hợp lệ hoặc một câu lệnh rằng không tồn tại sự chuyển đổi nào như vậy. Điều quan trọng là chúng ta chỉ có thể tăng giá trị chứ không bao giờ giảm chúng, vì vậy chúng ta không thể “sửa” một ô bằng cách hạ nó xuống mức tuân thủ. Sự bất đối xứng này là nguyên nhân thúc đẩy toàn bộ công trình. 

Các ràng buộc cho phép tổng số lên tới 100.000 ô trên tất cả các trường hợp thử nghiệm, điều này cho thấy rõ ràng việc quét tuyến tính trên mỗi trường hợp thử nghiệm là đủ. Bất cứ điều gì liên quan đến mô phỏng toàn cầu lặp đi lặp lại hoặc quay lại cấu hình sẽ quá chậm. 

Một trường hợp lỗi tinh vi xuất hiện khi các giá trị lớn tồn tại ở các vùng thưa thớt. Ví dụ: một ô có giá trị 10 ở bất kỳ vị trí nào là không thể vì ngay cả mức tối đa trong lưới cũng là bốn. Một trường hợp phức tạp khác là khi các ô góc hoặc cạnh có giá trị cao, vì bậc của chúng thậm chí còn nhỏ hơn. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ coi đây là một vấn đề thỏa mãn ràng buộc. Mỗi ô đang hoạt động hoặc không hoạt động và các ô đang hoạt động phải đáp ứng một điều kiện mức độ chính xác. Người ta có thể thử chọn ô nào trở thành dương và sau đó kiểm tra tất cả các ràng buộc. Điều này nhanh chóng bùng nổ vì có thể có 2^(nm) tập hợp con và thậm chí việc cắt tỉa theo các ràng buộc cục bộ vẫn để lại cấu trúc hàm mũ do sự phụ thuộc lẫn nhau giữa các nước láng giềng. 

Quan sát quan trọng là chúng ta thực sự không cần phải bảo toàn những con số đã cho vượt quá mức khả thi. Vì chúng ta chỉ có thể tăng giá trị nên quyết định có ý nghĩa duy nhất là ô nào trở thành dương trong lưới cuối cùng. Khi một ô dương, chúng ta luôn có thể tăng giá trị của nó để phù hợp với số lượng lân cận tích cực mà chúng ta gán cho nó. Vì vậy, nhiệm vụ thực tế giảm xuống còn việc chọn một tập hợp các ô sao cho mỗi ô được chọn có tối đa bốn ô lân cận trong tập hợp đó, sau đó đặt chính xác giá trị của từng ô đã chọn theo số lượng đó. 

Điều này gợi ý nên thử một mẫu đơn giản đảm bảo mức độ giới hạn ở mọi nơi. Bí quyết tiêu chuẩn là sử dụng vùng chọn giống như bàn cờ. Nếu chúng ta chọn các ô theo mẫu không có hai ô được chọn nào liền kề nhau theo các hướng nhất định, chúng ta có thể đảm bảo mức độ thấp. Tuy nhiên, một bàn cờ thuần túy quá hạn chế vì nó buộc độ 0 ở mọi nơi, không phù hợp với cấu trúc tùy ý. 

Thay vào đó, cấu trúc đã biết là chọn các ô một cách tham lam trong khi vẫn đảm bảo rằng mỗi ô được chọn có tối đa hai ô lân cận được chọn. Một cách thuận tiện để thực thi điều này là coi lưới như một biểu đồ và chỉ chọn các ô có bốn ô lân cận không được chọn đồng thời. Trên thực tế, trước tiên chúng ta quyết định tập dương cuối cùng rồi gán các giá trị dưới dạng độ. 

Giải pháp CF thực tế còn đơn giản hóa hơn nữa: chúng tôi xác định trước tập dương cuối cùng là tất cả các ô không thuộc cấu hình bị cấm và sau đó gán cho mỗi ô dương số lượng lân cận dương của nó. Nếu giá trị được chỉ định đó nhỏ hơn giá trị ban đầu, chúng tôi sẽ tăng giá trị đó cho phù hợp, điều này được cho phép.

Sự đơn giản hóa quan trọng là vì chúng ta chỉ có thể tăng giá trị nên chúng ta không bao giờ bị buộc phải giảm mức độ mục tiêu. Do đó, khi chúng tôi sửa được ô nào dương, chúng tôi chỉ cần đảm bảo rằng đối với mỗi ô được chọn, số lượng ô lân cận được tính toán của nó ít nhất là

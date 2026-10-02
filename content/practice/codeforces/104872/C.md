---
title: "CF 104872C - Thi lấy bằng lái xe"
description: "Chúng ta được cung cấp một đường gồm các nút giao được sắp xếp thành một đường thẳng, trong đó mỗi cặp liền kề được nối với nhau bằng một con đường có độ dài nhất định. Mỗi giao lộ cũng chứa một lượng “tài nguyên băng”."
date: "2026-06-28T10:24:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104872
codeforces_index: "C"
codeforces_contest_name: "2023-2024 Russia Team Open, High School Programming Contest (VKOSHP XXIV)"
rating: 0
weight: 104872
solve_time_s: 25
verified: false
draft: false
---

[CF 104872C - Kỳ thi lấy bằng lái xe](https://codeforces.com/problemset/problem/104872/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đường gồm các nút giao được sắp xếp thành một đường thẳng, trong đó mỗi cặp liền kề được nối với nhau bằng một con đường có độ dài nhất định. Mỗi giao lộ cũng chứa một lượng “tài nguyên băng”. Đối với bất kỳ đoạn giao lộ nào được chọn từ$l$ĐẾN$r$, chúng tôi xem xét tất cả các con đường bên trong đoạn đó và thêm vào đó là một con đường tạm thời đóng đoạn đó thành một vòng bằng cách nối$l$Và$r$. 

Đối với một đoạn đã chọn, mọi con đường đều phải được bao phủ hoàn toàn bởi băng. Nước đá không có sẵn miễn phí trên đường; thay vào đó, nó được lưu trữ tại các nút giao thông và mỗi đơn vị băng ở một điểm cuối có thể được di chuyển để che phủ chiều dài đường. Mỗi giao lộ có thể phân phối băng cho các con đường lân cận, nhưng không thể đóng góp nhiều băng hơn mức hiện có. 

Nhiệm vụ của truy vấn là tính toán lượng băng bổ sung phải được thêm vào các nút giao thông để sau khi phân phối lại, tất cả các con đường trong đoạn tuần hoàn đã chọn có thể được che phủ hoàn toàn. 

Đầu vào hỗ trợ cập nhật: thay băng tại một nút, thay đổi độ dài đường và trả lời các truy vấn về tính khả thi của chu kỳ đoạn này. 

Khó khăn chính là mỗi truy vấn bao gồm một mảng con khác nhau cộng với cạnh đóng và các ràng buộc đủ lớn để không thể tính toán lại từ đầu cho mỗi truy vấn. 

Với tối đa$2 \cdot 10^5$giao lộ và truy vấn, bất kỳ giải pháp nào gần hơn với$O(n)$mỗi truy vấn sẽ thất bại. Thậm chí$O(n \log n)$mỗi truy vấn quá chậm. Chúng tôi cần đại khái$O(\log n)$mỗi bản cập nhật và truy vấn được kết hợp, điều này gợi ý rõ ràng về cây phân đoạn với thông tin được lưu trữ được thiết kế cẩn thận. 

Một trường hợp phức tạp xuất hiện khi một phân đoạn hầu như không khả thi trong tổng số băng nhưng vẫn không khả thi cục bộ. Ví dụ: một nút có thể có đủ tổng lượng băng nhưng được định vị sao cho cả hai con đường liền kề đều yêu cầu nhiều hơn mức nó có thể đóng góp đồng thời. Bất kỳ giải pháp đúng nào cũng phải ngầm xử lý các ràng buộc bão hòa cục bộ như vậy thay vì chỉ xử lý tổng toàn cục. 

## Phương pháp tiếp cận 

Cách tiếp cận mạnh mẽ sẽ xử lý từng truy vấn loại 3 một cách độc lập bằng cách trích xuất phân đoạn$[l, r]$, tính toán tất cả các nhu cầu về đường bao gồm cả mép cuối và mô phỏng cách phân phối băng. Người ta có thể thử thực hiện một nhiệm vụ tham lam: đối với mỗi giao lộ, đẩy băng sang các cạnh liền kề, theo dõi những thiếu sót còn lại. Điều này vốn đã không cần thiết, nhưng ngay cả khi được thực hiện một cách tối ưu thì nó vẫn tốn kém.$O(r-l+1)$mỗi truy vấn. 

Với$q$lên đến$2 \cdot 10^5$, hành vi trong trường hợp xấu nhất trở thành$O(nq)$, quá chậm. 

Quan sát chính là tính khả thi chỉ phụ thuộc vào sự tương tác ranh giới địa phương và sự đóng góp bổ sung dọc theo các phân đoạn. Mỗi cạnh bên trong được chia sẻ bởi chính xác hai nút và mỗi nút đóng góp vào tối đa hai cạnh liền kề. Điều này tạo ra một cấu trúc tương tự như luồng trên một đường dẫn trong đó các ràng buộc phân tách thành các đại lượng nhất quán với tiền tố. 

Chúng ta có thể diễn giải lại mỗi nút là có dung lượng$w_i$và mỗi cạnh yêu cầu luồng$d_i$. Đối với một chu kỳ phân đoạn, mỗi nút phải phân phối băng đến các cạnh tới của nó trong phân đoạn đó và bất kỳ sự thiếu hụt nào đều tương ứng với lượng băng cần thiết bổ sung. 

Điều này biến thành một mô hình cổ điển: chúng ta cần duy trì các tổng hợp phân khúc mô tả mức độ tích lũy “nhu cầu chưa từng có” khi kết hợp hai phân khúc. Đây chính xác là loại trạng thái mà cây phân đoạn có thể hợp nhất, trong đó mỗi nút không chỉ lưu trữ tổng mà còn lưu trữ một số lượng nhỏ các giá trị mất cân bằng ranh giới biểu thị lượng băng cần thêm tùy thuộc vào

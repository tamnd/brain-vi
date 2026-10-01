---
title: "CF 104871L - Đường dẫn được gắn nhãn"
description: "Chúng ta có một biểu đồ tuần hoàn có hướng trong đó mỗi cạnh mang một nhãn chuỗi. Nhãn của đường dẫn được hình thành bằng cách ghép các nhãn cạnh theo thứ tự truyền tải."
date: "2026-06-28T10:40:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104871
codeforces_index: "L"
codeforces_contest_name: "2023-2024 ICPC Central Europe Regional Contest (CERC 23)"
rating: 0
weight: 104871
solve_time_s: 27
verified: false
draft: false
---

[CF 104871L - Đường dẫn được gắn nhãn](https://codeforces.com/problemset/problem/104871/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 27s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một biểu đồ tuần hoàn có hướng trong đó mỗi cạnh mang một nhãn chuỗi. Nhãn của đường dẫn được hình thành bằng cách ghép các nhãn cạnh theo thứ tự truyền tải. Trong số tất cả các đường dẫn có thể có từ một nút bắt đầu cố định$s$đến bất kỳ nút nào$t$, chúng ta muốn đường dẫn có nhãn được nối nhỏ nhất về mặt từ điển và chúng ta phải xuất ra chuỗi đỉnh tương ứng cho mỗi nút. 

Khó khăn chính là nhãn cạnh không được cung cấp rõ ràng dưới dạng chuỗi trong bộ nhớ. Thay vào đó, chúng là chuỗi con của một chuỗi lớn$A$có chiều dài lên tới$10^6$. Mỗi cạnh tham chiếu một đoạn$(p_i, \ell_i)$, vì vậy việc đọc nhãn nhiều lần dưới dạng chuỗi sẽ tốn kém nếu chúng ta cụ thể hóa chúng. 

Biểu đồ là một DAG có tối đa 600 đỉnh và 2000 cạnh. Điều này gợi ý rõ ràng rằng chúng ta có thể dựa vào cấu trúc tôpô hoặc sự nới lỏng kiểu đường đi ngắn nhất, nhưng so sánh không phải là trọng số số, chúng là so sánh từ điển của các chuỗi nối có thể có tổng chiều dài rất lớn. 

Cách tiếp cận mạnh mẽ liệt kê tất cả các đường dẫn là không thể bởi vì ngay cả trong DAG, số lượng đường dẫn có thể tăng theo cấp số nhân. Thách thức thực sự là chúng ta phải so sánh các nhãn đường dẫn mà không bao giờ xây dựng chúng một cách rõ ràng. 

Một vài trường hợp tế nhị quan trọng: 

Một vấn đề là các nhãn trống được cho phép. Nếu có một cạnh từ$u$ĐẾN$v$với chuỗi trống thì$u \to v$nên hoạt động giống như một phần mở rộng từ điển không tốn chi phí. Ví dụ, nếu chúng ta có$s \to a$được gắn nhãn "b" và$s \to b$được gắn nhãn "", sau đó$b$nhỏ hơn về mặt từ điển, mặc dù nó không đóng góp ký tự nào. 

Một vấn đề khác là mối quan hệ tiền tố. Nếu một nhãn đường dẫn là tiền tố của một nhãn khác thì nhãn ngắn hơn sẽ nhỏ hơn về mặt từ điển. Một cách tiếp cận ngây thơ chỉ so sánh với sự không phù hợp đầu tiên mà không xử lý tình trạng cạn kiệt sẽ thất bại. 

Cuối cùng, trích xuất chuỗi con từ$A$phải được xử lý cẩn thận. Nếu chúng ta liên tục cắt các chuỗi, chúng ta có nguy cơ$O(\ell)$trên mỗi cạnh, tốc độ này quá chậm nếu nhãn lớn và được sử dụng lại nhiều lần. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là coi mỗi đường dẫn là một chuỗi: chạy DFS từ$s$, xây dựng mọi phép nối có thể có và giữ giá trị nhỏ nhất về mặt từ điển trên mỗi nút. Về nguyên tắc, điều này đúng vì mọi con đường đều được xem xét một cách rõ ràng. Tuy nhiên, số lượng đường dẫn trong DAG có thể theo cấp số nhân trong$n$, ví dụ như trong biểu đồ phân lớp trong đó mỗi lớp phân nhánh gấp đôi. Ngay cả với$n = 600$, điều này trở nên hoàn toàn không thể thực hiện được. 

Sự cải thiện tự nhiên tiếp theo là nghĩ đến việc thư giãn theo kiểu con đường ngắn nhất. Chúng tôi muốn, đối với mỗi nút, nhãn tốt nhất từ ​​​​$s$. Nếu nhãn có trọng số là số, chúng tôi sẽ chạy Dijkstra. Ở đây, “khoảng cách” là một chuỗi theo thứ tự từ điển, không mang tính cộng theo nghĩa số đơn giản. 

Quan sát quan trọng là việc so sánh từ điển giữa hai đường dẫn phụ thuộc vào tiền tố của chúng cho đến lần không khớp đầu tiên. Điều này gợi ý rằng chúng ta nên so sánh các đường dẫn tăng dần, từng ký tự, thay vì xây dựng các chuỗi đầy đủ. Vì tất cả các nhãn cạnh đều đến từ một chuỗi cố định$A$, chúng ta có thể so sánh các cạnh bằng cách quét các chuỗi con của chúng theo yêu cầu, nhưng quan trọng hơn, chúng ta có thể tránh việc xây dựng chuỗi đầy đủ bằng cách sử dụng tính năng mở rộng dựa trên mức độ ưu tiên trong đó chúng ta luôn mở rộng nhãn nhỏ nhất hiện tại đã biết. 

Điều này dẫn đến một quy trình giống như Dijkstra trong đó các trạng thái là các nút và mức độ ưu tiên là nhãn chuỗi được biết đến nhiều nhất cho đến nay. Khó khăn là các chuỗi không được lưu trữ rõ ràng; thay vào đó, chúng tôi so sánh các đường dẫn một cách lười biếng bằng cách sử dụng cấu trúc chuỗi con cơ bản. 

Chúng tôi biểu thị mỗi đường dẫn dự kiến ​​dưới dạng một nút cộng với một tham chiếu đến cách hình thành nhãn của nó và chúng tôi so sánh hai ứng cử viên bằng cách so sánh từng chuỗi nối cạnh của chúng theo từng ký tự bằng cách sử dụng chuỗi gốc$A$. Để tránh công việc lặp đi lặp lại, chúng tôi đảm bảo mỗi nút được hoàn thiện một lần với nhãn tối thiểu và sau đó chúng tôi truyền bá sự thư giãn. 

Vì biểu đồ là DAG nên chúng tôi cũng được hưởng lợi từ thực tế là khi một nút được hoàn thiện thì không có đường dẫn nào sau này có thể cải thiện nút đó theo cách tạo ra theo chu kỳ. Tuy nhiên, chúng ta vẫn dựa vào thứ tự ưu tiên vì các cạnh đến khác nhau có thể mang lại các nhãn nhỏ hơn về mặt từ điển ngay cả khi chúng dài hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê DFS Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Dijkstra với so sánh chuỗi ngầm định |$O((n+m)\log n \cdot L)$so sánh trường hợp xấu nhất |$O(n+m)$| Đã chấp nhận | 

Đây$L$thể hiện chi phí so sánh các chuỗi con, nhưng được khấu hao khi quét con trỏ vào$A$, nó vẫn có thể quản lý được. 

## Hướng dẫn thuật toán 

Đối với mỗi nút, chúng tôi duy trì nút tiền nhiệm được biết đến nhiều nhất và cạnh đã tạo ra nó, để chúng tôi có thể xây dựng lại các đường dẫn ở cuối. Chúng tôi cũng duy trì hàng đợi ưu tiên đối với các nút được khóa bằng nhãn tốt nhất hiện tại của chúng. 

Vì nhãn là các chuỗi được hình thành bằng cách nối nên chúng tôi không lưu trữ chuỗi đầy đủ. Thay vào đó, mỗi trạng thái giữ đủ thông tin để xây dựng lại và so sánh nhãn của nó: nút cha và cạnh được sử dụng để tiếp cận nó, với các nhãn cạnh được cung cấp dưới dạng các lát cắt của$A$. 

1. Khởi tạo tất cả các nút dưới dạng chưa được truy cập với nhãn vô hạn, ngoại trừ$s$, có nhãn trống. 
2. Đẩy$s$vào hàng đợi ưu tiên. 
3. Trong khi hàng đợi không trống, hãy trích xuất nút$u$có nhãn hiện tại nhỏ nhất về mặt từ điển trong số tất cả các ứng cử viên. Điều này đảm bảo chúng tôi luôn mở rộng tiền tố nổi tiếng nhất trên toàn cầu trước tiên. 
4. Đối với mỗi cạnh đi ra$u \to v$, tính toán nhãn ứng cử viên là phép nối của nhãn tốt nhất hiện tại của$u$cộng với chuỗi con của$A$tương ứng với cạnh đó. Thay vì hiện thực hóa nó, chúng tôi so sánh ứng cử viên này với nhãn hiệu tốt nhất hiện tại$v$bằng cách đi từng ký tự thông qua các biểu diễn ngầm. 
5. Nếu nhãn mới nhỏ hơn, hãy cập nhật$v$con trỏ cha của s tới$u$, lưu trữ cạnh và đẩy$v$vào hàng đợi ưu tiên. 
6. Tiếp tục cho đến khi tất cả các nút có thể truy cập được xử lý. 
7. Xây dựng lại từng đường dẫn bằng cách đi theo các con trỏ cha từ mỗi nút trở lại$s$, sau đó đảo ngược. 

Hoạt động tinh tế là bước 4 và 5: so sánh hai chuỗi ẩn. Chúng tôi so sánh bằng cách đồng thời đi qua c

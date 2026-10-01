---
title: "CF 104871F - Phát sinh chủng loại"
description: "Chúng tôi được cung cấp một cấu trúc đồ thị đặc biệt. Đối tượng cơ bản là một cái cây, nằm trong mặt phẳng, với các lá được sắp xếp theo thứ tự hình tròn. Sau đó, mỗi cặp lá liên tiếp theo thứ tự hình tròn này lại được nối thêm, tạo thành một vòng tròn trên các lá."
date: "2026-06-28T10:38:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104871
codeforces_index: "F"
codeforces_contest_name: "2023-2024 ICPC Central Europe Regional Contest (CERC 23)"
rating: 0
weight: 104871
solve_time_s: 39
verified: false
draft: false
---

[CF 104871F - Phát sinh loài](https://codeforces.com/problemset/problem/104871/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một cấu trúc đồ thị đặc biệt. Đối tượng cơ bản là một cái cây, nằm trong mặt phẳng, với các lá được sắp xếp theo thứ tự hình tròn. Sau đó, mỗi cặp lá liên tiếp theo thứ tự hình tròn này lại được nối thêm, tạo thành một vòng tròn trên các lá. Do đó, đồ thị cuối cùng là sự kết hợp của một cây và một chu trình đơn giản chạy qua tất cả các lá theo thứ tự nhúng của chúng. 

Nhiệm vụ là đếm số cách tô màu thích hợp của đồ thị cuối cùng này bằng cách sử dụng K màu, trong đó các đỉnh liền kề phải luôn có các màu khác nhau. Câu trả lời phải được tính theo modulo 1.000.000.007. 

Các ràng buộc cho phép lên tới 100.000 nút và tối đa 100.000 màu, điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng liệt kê các phép gán màu hoặc thậm chí lưu trữ trạng thái dày đặc trên mỗi đỉnh. Một cách tiếp cận hợp lệ phải chạy trong thời gian gần như tuyến tính ở mức N hoặc N log N ở mức tồi tệ nhất. 

Một quan sát cấu trúc quan trọng là đồ thị gần như là một cái cây. Điểm khác biệt duy nhất của một cái cây là tất cả các lá đều tạo thành một chu kỳ bổ sung. Một cách tiếp cận đơn giản sẽ coi đây là một vấn đề tô màu đồ thị tổng quát, nhưng cấu trúc cây hạn chế mạnh mẽ sự phụ thuộc và ràng buộc toàn cục duy nhất được đưa ra bởi chu kỳ lá đó. 

Một sai lầm ngây thơ là bỏ qua thứ tự nhúng của các lá và cho rằng mọi thứ tự đều có tác dụng. Ví dụ: nếu một cây có lá 1, 2, 3, 4 theo thứ tự hình tròn, việc kết nối chúng không chính xác sẽ làm thay đổi cấu trúc chu trình và do đó làm thay đổi số lượng màu. Chu trình được cố định bằng cách nhúng phẳng, không phải là sự kề cận của lá tùy ý. 

Một vấn đề tế nhị khác là các lá có chính xác là cấp 1 trên cây, nhưng sau khi thêm các cạnh chu kỳ, mỗi lá đều có cấp 3 trong biểu đồ cuối cùng. Một DP cây ngây thơ giả định các lá là điểm cuối độc lập sẽ thất bại vì các lá hiện bị ràng buộc lẫn nhau trong suốt chu trình. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ gán màu cho tất cả N nút và kiểm tra các ràng buộc lân cận. Điều này tiêu tốn khả năng K^N, điều này là không thể ngay cả đối với N nhỏ. 

Một lực lượng vũ phu có cấu trúc chặt chẽ hơn một chút sẽ sử dụng tính năng quay lui bằng cách cắt tỉa các cạnh. Điều này vẫn khám phá số lượng bài tập theo cấp số nhân trong trường hợp xấu nhất, bởi vì biểu đồ chứa một chu trình trên nhiều nút có thể có. Chỉ riêng chu kỳ lá hoạt động giống như một biểu đồ chu kỳ tiêu chuẩn, có số lượng màu đã tăng lên thành (K−1)^n cộng với các hiệu chỉnh tùy theo tính chẵn lẻ, nhưng ở đây nó được ghép với một cây, do đó các ràng buộc lan truyền vào bên trong. 

Quan sát quan trọng là đồ thị là một cây có chu trình đơn giản bổ sung chỉ trên các lá. Nếu chúng ta loại bỏ các cạnh chu kỳ, chúng ta sẽ phục hồi một cây và cây dễ dàng đếm màu thông qua DP. Khó khăn là các lá không còn là các điều kiện biên độc lập nữa vì chúng phải thỏa mãn các ràng buộc kề cận chu trình. 

Chúng tôi xử lý vấn đề này bằng cách root cây và thực hiện DP trong đó mỗi cây con tính toán một hàm mô tả cách nó hoạt động như một “giao diện ranh giới được tô màu” ở gốc của nó. Để tô màu cây, một nút chỉ cần đảm bảo rằng nó khác với nút gốc của nó, vì vậy hệ số đóng góp của cây con là phù hợp. Sự phức tạp xuất phát từ chu kỳ, trong đó tất cả các cặp đều rời khỏi một ràng buộc toàn cầu duy nhất. 

Ý tưởng trung tâm là giảm bớt vấn đề bằng cách đếm các màu của một chu trình trong đó mỗi đỉnh có một cây con đính kèm đóng góp một hệ số nhân tùy thuộc vào việc màu của đỉnh có bằng các ràng buộc cha của nó hay không. Điều này biến bài toán thành một DP chu kỳ với trạng thái chỉ phụ thuộc vào việc các màu lá liền kề có khớp hay không. 

Do đó, chúng tôi tách vấn đề thành hai lớp: một DP cây nén mỗi phần đính kèm của lá thành một trọng số tùy thuộc vào màu sắc của nó so với cha mẹ của nó và một DP chu kỳ trên các lá thực thi các ràng buộc kề cận.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(K^N) | O(N) | Quá chậm | 
| Phân rã cây + chu trình DP | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta root cây ở bất kỳ nút nào. Cấu trúc của cây đã cố định và lá đã được biết đến. 

1. Xây dựng danh sách kề cho phần cây, hiện tại bỏ qua các cạnh chu kỳ lá. Xác định tất cả các lá; số của họ là L. 
2. Chạy cây DP dựa trên DFS trong đó mỗi nút tính toán số lượng màu hợp lệ của cây con của nó với điều kiện là màu riêng của nó được cố định so với màu gốc của nó. Vì chỉ có các ràng buộc kề tồn tại trong cây nên mỗi nút chỉ cần đảm bảo nó khác với nút gốc và các cây con con độc lập sau khi màu nút được cố định. 

Đối với nút u, xác định dp[u] là số màu hợp lệ của cây con gốc tại u giả sử u có màu cố định khác với màu gốc của nó. Sự tái diễn là mỗi cây con v đóng góp một hệ số chỉ phụ thuộc vào dp[v], nhân với các cây con, vì các cây con con độc lập phụ thuộc vào màu của u. 

1. Sau khi tính dp, mỗi lá u có một giá trị biểu thị số cách tô màu cây con của nó dựa trên màu của u. Điều quan trọng là đối với các lá, điều này không quan trọng vì một lá không có con nào trong cây, nên dp[u] = 1. 
2. Bây giờ hãy xem xét chu kỳ được hình thành bởi các lá theo thứ tự nhất định của chúng. Mỗi lá u tham gia vào hai ràng buộc: một từ lá mẹ của nó trong cây và hai từ các lá lân cận của nó trong chu trình. Cây DP đã đảm bảo tính nhất quán với cây gốc, do đó ràng buộc còn lại chỉ ở các cạnh của chu trình. 
3. Với mỗi lá u, xác định một hàm đóng góp phụ thuộc vào việc màu của nó bằng hay khác với các lá lân cận trong chu kỳ của nó. Bởi vì tất cả các màu đều đối xứng, chúng tôi nén vấn đề này thành một bài toán tô màu chu kỳ tiêu chuẩn: đếm các màu của chu trình L sử dụng K màu với các ràng buộc kề, nhân với sự đóng góp của cây độc lập. 
4. Số lượng màu chu kỳ cho chu trình L là: 

(K−1)^L + (−1)^L (K−1), dẫn xuất từ polyno màu tiêu chuẩn

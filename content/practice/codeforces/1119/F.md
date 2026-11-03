---
title: "CF 1119F - Niyaz và Độ nhỏ"
description: "Chúng tôi được tặng một cây có trọng lượng. Đối với mọi giới hạn độ $x$, chúng ta có thể xóa bất kỳ tập hợp cạnh nào. Xóa một cạnh sẽ trả giá cho nó. Sau khi xóa, mỗi đỉnh phải có bậc nhiều nhất là $x$. Đối với mỗi $x$, chúng ta cần tổng trọng lượng bị xóa tối thiểu."
date: "2026-06-12T04:29:34+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "trees"]
categories: ["algorithms"]
codeforces_contest: 1119
codeforces_index: "F"
codeforces_contest_name: "Codeforces Global Round 2"
rating: 3400
weight: 1119
solve_time_s: 48
verified: false
draft: false
---

[CF 1119F - Niyaz và các cấp độ nhỏ](https://codeforces.com/problemset/problem/1119/F) 

**Đánh giá:** 3400 
**Tas:** cấu trúc dữ liệu, dp, cây 
**Thời gian giải:** 48s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cây có trọng lượng. 

Đối với mọi giới hạn độ$x$, chúng ta có thể xóa bất kỳ tập hợp cạnh nào. Xóa một cạnh sẽ trả giá cho nó. Sau khi xóa, mỗi đỉnh tối đa phải có bậc$x$. 

Đối với mỗi$x$, chúng ta cần tổng trọng lượng bị xóa tối thiểu. 

Cây có tới$250\,000$đỉnh. Bất kỳ giải pháp nào có thể giải quyết vấn đề một cách độc lập cho mọi$x$ngay lập tức bị loại trừ. Thậm chí một$O(n)$thuật toán trên mỗi giá trị của$x$sẽ trở thành$O(n^2)$, vượt xa giới hạn. 

Phần không rõ ràng là ràng buộc là cục bộ. Một đỉnh chỉ quan tâm đến việc có bao nhiêu cạnh sự cố tồn tại. Điều này gợi ý rằng DP sẽ quyết định, đối với mỗi cạnh, liệu nó sẽ được giữ lại hay bị xóa. 

## Phương pháp tiếp cận 

Giả sử$x$đã được sửa. 

Root cây tùy ý. 

Đối với một đỉnh$u$, định nghĩa:$f_u^0$: chi phí tối thiểu bên trong cây con của$u$khi cạnh từ$u$cho cha mẹ của nó được giữ lại.$f_u^1$: chi phí tối thiểu bên trong cây con của$u$khi cạnh từ$u$tới cha mẹ của nó sẽ bị xóa. 

Hãy xem xét một đứa trẻ$v$được kết nối bởi một cạnh của trọng lượng$w$. 

Nếu chúng ta giữ$(u,v)$, phần đóng góp là$f_v^0$. 

Nếu chúng ta xóa$(u,v)$, chúng tôi trả tiền$$w + f_v^1.$$Giá tăng thêm của việc xóa thay vì giữ là$$\Delta_v = w + f_v^1 - f_v^0.$$Bắt đầu từ trạng thái mà mọi cạnh con đều được giữ nguyên. 

Sau đó, mỗi cạnh con bị xóa sẽ thêm$\Delta_v$. 

Một số$\Delta_v$đã không tích cực rồi. Những cạnh như vậy phải luôn được xóa vì làm như vậy không bao giờ làm tăng câu trả lời. 

Sau khi buộc tất cả không tích cực$\Delta_v$, nhiệm vụ còn lại rất đơn giản: 

Một đỉnh bậc$d$tối đa phải kết thúc bằng bằng cấp$x$. 

Do đó nó phải xóa đủ các cạnh sự cố. Trong số tất cả các ứng viên còn lại, chúng tôi chọn kết quả tích cực nhỏ nhất$\Delta_v$. 

Điều này mang lại một$O(n\log n)$DP cho một giá trị cố định của$x$. 

Thật không may, chúng tôi cần tất cả các giá trị của$x$. 

Nhận xét quan trọng là khi$x$giảm, chỉ các đỉnh có bậc lớn hơn$x$bắt đầu áp đặt các ràng buộc. Các đỉnh đã có bậc nhiều nhất$x$không liên quan. 

Quá trình$x$từ$n-1$xuống tới$1$. 

Duy trì rừng cảm ứng chỉ bao gồm các đỉnh có bậc lớn hơn giới hạn hiện tại. 

Bất cứ khi nào$x$giảm đi một thì một loạt đỉnh mới sẽ hoạt động. 

DP chỉ được tính toán lại bên trong các thành phần hoạt động mới được hình thành. Để ủng hộ việc “lấy cái nhỏ nhất$k$deltas" hoạt động hiệu quả, mỗi đỉnh duy trì một cấu trúc cân bằng chứa ứng cử viên$\Delta$giá trị và tổng của những giá trị hiện được chọn. Các giải pháp ban đầu sử dụng nhiều tập hợp có khôi phục hoặc bộ ba nhị phân hỗ trợ chèn, xóa và "tổng của các giá trị nhỏ nhất"$k$các giá trị". 

Độ phức tạp thu được là$O(n\log n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Giải quyết mọi$x$độc lập |$O(n^2\log n)$|$O(n)$| Quá chậm | 
| Kích hoạt ngoại tuyến + cây DP |$O(n\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính bậc gốc của mỗi đỉnh. 
2. Nhóm các đỉnh theo bậc. 
3. Giới hạn quy trình$x=n-1,n-2,\ldots,1$. 
4. Khi giới hạn trở thành$x$, kích hoạt mọi đỉnh có bậc bây giờ lớn hơn$x$. 
5. Các đỉnh hoạt động tạo thành một khu rừng. 
6. Với mỗi thành phần hoạt động, hãy chạy cây DP. 
7. Dành cho mọi trẻ em$v$, tính toán$$\Delta_v=w+f_v^1-f_v^0.$$1. Tất cả đều không tích cực$\Delta_v$được thực hiện ngay lập tức vì việc xóa một cạnh như vậy không bao giờ tệ hơn. 
2. Hãy để$k$là số cạnh sự cố bổ sung phải được loại bỏ để thỏa mãn mức độ ràng buộc. 
3. Trong số tích cực còn lại$\Delta_v$, cộng tổng nhỏ nhất$k$các giá trị. 
4. Điều này mang lại$f_u^0$Và$f_u^1$. 
5. Tính tổng đóng góp của tất cả các thành phần hoạt động để có được câu trả lời cho vấn đề này$x$. 
6. Đối với$x=0$, mọi cạnh phải bị xóa, vì vậy câu trả lời đơn giản là tổng trọng số của tất cả các cạnh. 

### Tại sao nó hoạt động 

Đối với một cố định$x$, mỗi cạnh con đóng góp chi phí "giữ" hoặc chi phí "xóa". 

Sự khác biệt giữa những lựa chọn này chính xác là$\Delta_v$. 

Sau tất cả các lần xóa bắt buộc với giá trị không tích cực$\Delta_v$được thực hiện, mỗi lần xóa còn lại sẽ tăng mục tiêu lên một lượng dương. Để thỏa mãn giới hạn về mức độ, chiến lược tối ưu là trả mức tăng nhỏ nhất hiện có. Đây là một đối số trao đổi tiêu chuẩn: thay thế việc xóa đã chọn bằng một đối số lớn hơn$\Delta$chỉ có thể làm xấu đi câu trả lời. 

Quét ngoại tuyến hoạt động vì một đỉnh chỉ ảnh hưởng đến DP khi độ của nó vượt quá giới hạn hiện tại. BẰNG$x$giảm, các đỉnh sẽ hoạt động chính xác một lần, cho phép tất cả các bản cập nhật được tính cho một sự kiện kích hoạt duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n\log n)$| Mỗi lần kích hoạt và mỗi thao tác cấu trúc dữ liệu DP đều tiêu tốn thời gian logarit | 
| Không gian |$O(n)$| Cây, mảng DP và cấu trúc cân bằng | 

Các ràng buộc cho phép thực hiện khoảng vài triệu phép tính logarit.$O(n\log n)$vừa vặn thoải mái trong giới hạn 3 giây cho$n=250\,000$.

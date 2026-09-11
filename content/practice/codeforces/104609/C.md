---
title: "CF 104609C - Bida lục giác"
description: "Chúng ta đang mô phỏng một quả bóng bi-a bên trong một hình lục giác đều, trong đó chuyển động hoàn toàn đàn hồi: quả bóng chuyển động theo đường thẳng và phản xạ ra khỏi các cạnh có góc tới và góc phản xạ bằng nhau."
date: "2026-06-30T02:45:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104609
codeforces_index: "C"
codeforces_contest_name: "Udmurt SU + Izhevsk STU Contest 2012"
rating: 0
weight: 104609
solve_time_s: 41
verified: false
draft: false
---

[CF 104609C - Bi-a lục giác](https://codeforces.com/problemset/problem/104609/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang mô phỏng một quả bóng bi-a bên trong một hình lục giác đều, trong đó chuyển động hoàn toàn đàn hồi: quả bóng chuyển động theo đường thẳng và phản xạ ra khỏi các cạnh có góc tới và góc phản xạ bằng nhau. Hình lục giác được căn giữa tại điểm gốc, cố định về hướng và chuyển động bắt đầu từ một điểm bên trong nhất định với hướng ban đầu ngẫu nhiên đồng đều. 

Ba trong số sáu cạnh là ranh giới “mở”. Nếu quả bóng chạm vào bất kỳ mặt mở nào, nó sẽ ngay lập tức rời khỏi đa giác và quá trình kết thúc. Ba cạnh còn lại hoạt động bình thường và phản xạ quả bóng trở lại hình lục giác. 

Quá trình tiến triển trong các sự kiện va chạm rời rạc. Mỗi lần bóng chạm một bên, chúng ta tính một lần va chạm. Nhiệm vụ là tính xác suất để quả bóng rời khỏi hình lục giác chính xác ở lần va chạm thứ N, nghĩa là nó sống sót sau N−1 va chạm đầu tiên mà không thoát ra và thoát ra ngay lập tức ở lần va chạm thứ N. 

Đầu vào cung cấp số lần va chạm N và điểm bắt đầu bên trong hình lục giác. Vị trí chỉ quan trọng đối với việc xác định sự phân bố của phía va chạm đầu tiên, trong khi hành vi tiếp theo phụ thuộc vào động lực chơi bi-a. 

Các ràng buộc cho phép N lên tới 100. Điều này ngay lập tức loại trừ mọi mô phỏng liên tục về các góc hoặc hình học. Một cách tiếp cận ngây thơ mô phỏng các hướng ngẫu nhiên sẽ không thể thực hiện được vì câu trả lời là xác suất chính xác chứ không phải ước tính và không gian trạng thái của các góc liên tục là vô hạn. 

Trường hợp cạnh tinh vi phát sinh khi N = 0. Quả bóng không thể thoát ra mà không có bất kỳ va chạm nào, do đó xác suất bằng 0 trừ khi điểm bắt đầu đã nằm trên một ranh giới mở, điều này được loại trừ rõ ràng bởi đảm bảo rằng điểm đó nằm hoàn toàn bên trong hình lục giác. 

Một điểm tinh tế quan trọng khác là sau mỗi lần phản xạ, sự phân bố của va chạm tiếp theo chỉ phụ thuộc vào cạnh hiện tại chứ không phụ thuộc vào toàn bộ lịch sử hoặc vị trí chính xác. Một mô phỏng hình học đơn giản sẽ cố gắng theo dõi các tọa độ liên tục một cách không chính xác, điều này không cần thiết và không ổn định về mặt số lượng. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ cố gắng mô phỏng tất cả các quỹ đạo có thể có bằng cách lấy mẫu hướng và theo dõi phản xạ. Mỗi quỹ đạo là một đường tuyến tính từng phần và chúng ta sẽ kiểm tra xem nó có thoát ra ở va chạm thứ N hay không. Điều này đúng về mặt khái niệm nhưng không thể tính toán chính xác vì không gian các hướng là liên tục. Ngay cả các góc rời rạc cũng sẽ bùng nổ về mặt tính toán và vẫn không mang lại xác suất chính xác. 

Quan sát cấu trúc quan trọng là chuyển động bi-a trong một đa giác đều có sự phản xạ đàn hồi có thể biến thành chuyển động thẳng trên một mặt phẳng tuần hoàn. Trong chế độ xem được mở này, mỗi va chạm tương ứng với việc vượt qua một đường ranh giới giữa các bản sao phản chiếu của hình lục giác. Bởi vì hình lục giác là đều, tính đối xứng buộc quá trình quên gần như toàn bộ chi tiết hình học sau mỗi va chạm. Điều còn lại chỉ là bên nào bị đánh chứ không phải bên nào bị đánh. 

Điều này làm sụp đổ hệ thống liên tục thành một chuỗi Markov hữu hạn. Mỗi trạng thái tương ứng với bên hiện đang bị tấn công và mỗi lần chuyển đổi tương ứng với việc di chuyển từ bên này sang bên khác với xác suất cố định được xác định bởi tính đối xứng. Ba trong số các trạng thái này đang hấp thụ (các mặt bị loại bỏ) và khi được đưa vào, quá trình sẽ dừng lại. 

Do đó, bài toán quy về việc tính xác suất để chuỗi Markov đi vào trạng thái hấp thụ chính xác ở bước N. Điều này có thể được giải quyết bằng cách sử dụng quy hoạch động theo các bước và các cạnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng quỹ đạo) | Vô hạn / khó chữa | O(1) | Không khả thi | 
| Markov DP trên các cạnh | O(N · 6²) | O(6) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đánh dấu sáu cạnh của hình lục giác từ 0 đến 5, đánh dấu ba cạnh là phản xạ và ba cạnh là hấp thụ (các cạnh thoát ra). Điều này phân chia ranh giới thành trạng thái đầu cuối và không đầu cuối. 
2. Xác định phân bố xác suất về việc mỗi bên ở ngay sau va chạm. Ban đầu, phía va chạm thứ nhất được xác định thống nhất

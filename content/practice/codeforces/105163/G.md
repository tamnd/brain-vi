---
title: "CF 105163G - Mã màu xám"
description: "Việc xây dựng xác định một biểu đồ có hướng được xây dựng từ thao tác bit. Mỗi đỉnh là một số nguyên trong phạm vi đầy đủ có thể được hiểu là một chuỗi nhị phân có độ dài cố định."
date: "2026-06-27T10:54:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105163
codeforces_index: "G"
codeforces_contest_name: "The 19th Heilongjiang Provincial Collegiate Programming Contest"
rating: 0
weight: 105163
solve_time_s: 35
verified: false
draft: false
---

[CF 105163G - Mã giống màu xám](https://codeforces.com/problemset/problem/105163/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Việc xây dựng xác định một biểu đồ có hướng được xây dựng từ thao tác bit. Mỗi đỉnh là một số nguyên trong phạm vi đầy đủ có thể được hiểu là một chuỗi nhị phân có độ dài cố định. Từ mỗi đỉnh, chính xác hai cạnh có hướng được tạo: một cạnh có được bằng cách dịch chuyển trạng thái hiện tại sang trái một bit và buộc bit có ý nghĩa nhỏ nhất mới là 0, và một cạnh khác thực hiện tương tự nhưng buộc bit mới phải là 1, với mọi thứ bị cắt bớt trở lại độ rộng bit ban đầu. 

Nói cách khác, mỗi nút đại diện cho một chuỗi nhị phân có độ dài$n-1$và mỗi cạnh đi sẽ thêm 0 hoặc 1 trong khi loại bỏ bit cũ nhất. Mỗi cạnh có hướng có thể được gắn nhãn bằng bit được thêm vào, vì vậy mỗi cạnh tương ứng duy nhất với lựa chọn chữ số nhị phân. 

Nhiệm vụ không phải là về đường đi ngắn nhất hoặc khả năng tiếp cận. Đó là về việc đếm cấu trúc tổng thể trong biểu đồ này, cụ thể là số lượng mạch Euler. Chu trình Euler là một đường đi khép kín đi qua mọi cạnh có hướng đúng một lần. Bởi vì mọi nút đều có mức độ trong và mức độ bằng nhau, nên các mạch như vậy tồn tại và vấn đề giảm xuống việc đếm xem có bao nhiêu nút riêng biệt. 

Phần không tầm thường là kích thước đồ thị tăng theo cấp số nhân với$n$. Số đỉnh là$2^{n-1}$, và số cạnh là$2^n$. Ngay cả việc lưu trữ tất cả các cạnh một cách rõ ràng cũng không thể thực hiện được khi$n$là lớn. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng liệt kê các bước đi hoặc thậm chí xây dựng các cấu trúc tổ hợp lớn một cách rõ ràng, vì$2^n$phát triển vượt quá mọi giới hạn khả thi một lần$n$vượt quá khoảng 25. 

Một chế độ thất bại tinh tế xuất hiện nếu người ta cố gắng mô phỏng các chuyến đi Euler hoặc sử dụng phương pháp đếm ngược đơn giản. Ngay cả đối với$n = 10$, đồ thị đã có 512 đỉnh và 1024 cạnh, và số chu trình Euler về mặt thiên văn lớn hơn bất kỳ phép liệt kê nào có thể xử lý được. Bất kỳ cách tiếp cận nào khám phá hoán vị của các cạnh hoặc đường dẫn sẽ phát nổ theo giai thừa và thất bại rất lâu trước khi đạt đến giới hạn đầu vào dự kiến. 

Kết quả đầu ra chính xác là một số nguyên lớn duy nhất biểu thị số đếm này, vì vậy thách thức thực sự là chuyển đổi một bài toán đếm tổ hợp trên một đồ thị lớn thành một biểu thức dạng đóng. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ cố gắng xây dựng rõ ràng biểu đồ và đếm các mạch Euler bằng cách quay lui hoặc liệt kê kiểu Hierholzer kết hợp với ghi nhớ. Điều này chỉ có tác dụng trong những trường hợp rất nhỏ vì mỗi lựa chọn cạnh đều phân nhánh thành hai khả năng, và số lần đi qua Euler hợp lệ tăng theo cấp số nhân. Ngay cả khi áp dụng tính năng cắt tỉa, không gian trạng thái vẫn tỷ lệ thuận với hoán vị của các cạnh, gần bằng$(2^n)!$trong trường hợp khái niệm xấu nhất. Điều đó vượt xa mọi giới hạn tính toán. 

Cấu trúc của biểu đồ cho thấy một góc nhìn hữu ích hơn. Mỗi đỉnh tương ứng với một chuỗi nhị phân có độ dài cố định và các chuyển tiếp tương ứng với các bit dịch chuyển và nối thêm. Đây chính xác là biểu đồ de Bruijn của chuỗi nhị phân có độ dài$n-1$. Trong biểu đồ này, các mạch Euler được biết là tương ứng một-một với các chuỗi có thứ tự de Bruijn$n$, trong đó mỗi chuỗi nhị phân có độ dài$n$xuất hiện chính xác một lần dưới dạng chuỗi con liền kề của chuỗi tuần hoàn. 

Khi bài toán được nhận dạng là đếm các mạch Euler trong đồ thị de Bruijn, định lý BEST sẽ có thể áp dụng được. Nó phát biểu rằng số lượng chu trình Euler trong một đồ thị Euler có hướng bằng tích của các giai thừa bậc ngoài của đỉnh nhân với số cây khung có hướng bắt nguồn từ bất kỳ đỉnh nào. Trong biểu đồ cụ thể này, mọi đỉnh đều có bậc ngoài 2, điều này làm đơn giản hóa rất nhiều giai thừa, vì mọi đóng góp đều trở thành$1! \cdot 1!$-Cấu trúc giống như trên một phân bố đồng đều. 

Phần còn lại giảm xuống việc đếm các nhánh bao trùm trong đồ thị có tính đối xứng cao. Ma trận Laplacian của đồ thị này có cấu trúc khối đệ quy vì các đỉnh được phân chia tự nhiên theo tiền tố nhị phân của chúng. Sự đối xứng này cho phép một sự tái diễn xác định trên các phân số chính, cuối cùng sụp đổ thành một dạng hàm mũ đơn giản. 

Một sự đơn giản hóa quan trọng là đồ thị đều đặn đến mức phổ của nó hoạt động có thể dự đoán được dưới dạng đệ quy. Đa thức đặc trưng thỏa mãn phép truy toán nhân đôi, hàm ý rằng định thức của hàm Laplacian thứ liên quan tăng lên dưới dạng lũy ​​thừa của hai với số mũ gắn với số cạnh trừ đi các đỉnh cộng một. 

Điều này dẫn đến dạng đóng đã biết của đồ thị de Bruijn nhị phân: số chu trình Euler là$$2^{2^{n-1} - n}.$$Vì vậy, thay vì xây dựng hoặc phân tích biểu đồ một cách trực tiếp, vấn đề sẽ giảm xuống việc tính lũy thừa của hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đếm Euler cưỡng bức | số mũ trong$2^n$| Hàm mũ | Quá chậm | 
| Lý thuyết đồ thị (TỐT NHẤT + cấu trúc) |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$, xác định kích thước của đồ thị de Bruijn và do đó xác định số đỉnh$2^{n-1}$và các cạnh$2^n$. Bản thân biểu đồ không bao giờ được xây dựng. 
2. Nhận biết rằng cấu trúc tương ứng với đồ thị de Bruijn nhị phân trong đó mỗi chu trình Euler tương ứng duy nhất với một chuỗi tuần hoàn hợp lệ chứa mọi chuỗi nhị phân có độ dài$n$đúng một lần. 
3. Áp dụng kết quả đã biết thu được từ định lý BEST và phân tích định thức Laplacian của họ đồ thị này, từ đó cho số mạch Euler là$2^{2^{n-1} - n}$. 
4. Tính số mũ$2^{n-1} - n$. Bước này rất quan trọng vì bản thân số mũ có thể trở nên rất lớn ngay cả đối với$n$, do đó việc tính toán phải dựa vào các số nguyên có độ chính xác tùy ý. 
5. Đầu ra (2^{(2^{

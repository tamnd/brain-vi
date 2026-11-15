---
title: "CF 1864I - Kẻ Thống Trị Tương Lai"
description: "Giả sử các ô lưới là các đỉnh của đồ thị lưới $n lần n$. Vị trí cuối cùng của các số $1 ldots n^2$ là hợp lệ nếu mọi đỉnh ngoại trừ đỉnh chứa $1$ có ít nhất một đỉnh liền kề có số nhỏ hơn."
date: "2026-06-08T23:59:17+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1864
codeforces_index: "I"
codeforces_contest_name: "Harbour.Space Scholarship Contest 2023-2024 (Div. 1 + Div. 2)"
rating: 3500
weight: 1864
solve_time_s: 147
verified: false
draft: false
---

[CF 1864I - Kẻ thống trị tương lai](https://codeforces.com/problemset/problem/1864/I) 

**Đánh giá:** 3500 
**Tags:** đồ thị, tham lam 
**Thời gian giải:** 2m 27s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Cho các ô lưới là các đỉnh của một$n \times n$đồ thị lưới. Vị trí cuối cùng của các con số$1 \ldots n^2$là hợp lệ nếu mọi đỉnh ngoại trừ đỉnh chứa$1$có ít nhất một đỉnh liền kề có số nhỏ hơn. 

Một công thức cải tiến hữu ích thu được bằng cách xem xét các con số theo thứ tự tăng dần. Đỉnh chứa$1$là gốc. Mọi đỉnh khác phải có cạnh với một số nhỏ hơn được đặt trước đó. Điều đó có nghĩa là với mỗi ngưỡng$k$, tập hợp các đỉnh có nhãn lớn nhất$k$phải tạo ra một đồ thị con được kết nối. 

Bây giờ hãy xem xét quá trình trực tuyến. Các truy vấn đến theo một thứ tự cố định. Khi một ô được truy vấn, chúng ta phải gán ngay cho nó một trong các nhãn còn lại, giữ cho việc gán một phần hiện tại có thể mở rộng thành nhãn cuối cùng hợp lệ. Trong số tất cả các lựa chọn hợp lệ, chúng ta phải đưa ra nhãn lớn nhất có thể. 

Tổng số ô trong tất cả các trường hợp thử nghiệm nhiều nhất là$10^6$. Bất kỳ thuật toán nào liên tục tính toán lại kết nối từ đầu đều vô vọng. Thậm chí$O(n^2)$công việc cho mỗi truy vấn sẽ trở thành$O(n^4)$trong trường hợp xấu nhất. 

Những trường hợp nguy hiểm chính là những lúc việc loại bỏ ô được truy vấn sẽ ngắt kết nối phần vẫn chưa được gán của bảng. Việc triển khai ngây thơ chỉ kiểm tra mức độ cục bộ sẽ thất bại ngay lập tức. Trong đường đi gồm ba đỉnh, đỉnh giữa có bậc hai nhưng vẫn là điểm khớp. Câu trả lời phụ thuộc vào khả năng kết nối trong tương lai chứ không phải cấu trúc cục bộ. 

## Phương pháp tiếp cận 

Quan điểm vũ phu rất đơn giản. 

Tại bất kỳ thời điểm nào, hãy xem xét biểu đồ của các ô vẫn chưa được gán. Đối với ô được truy vấn$v$, hãy thử các nhãn còn lại từ lớn nhất đến nhỏ nhất. Một nhãn$x$khả thi nếu các nhãn còn lại nhỏ hơn$x$vẫn có thể được sắp xếp sao cho tất cả các đỉnh có số thấp vẫn được kết nối. Kiểm tra kết nối sau mỗi lựa chọn dự kiến ​​đã tốn phí$O(n^2)$, và có$O(n^2)$truy vấn. Sự phức tạp dẫn đến vượt xa giới hạn. 

Quan sát quan trọng là toàn bộ điều kiện ghi nhãn có thể được viết lại dưới dạng điều kiện kết nối. 

Giả sử chúng ta xử lý nhãn từ lớn nhất đến nhỏ nhất. Việc gán nhãn lớn nhất hiện tại tương đương với việc xóa đỉnh đó khỏi biểu đồ vẫn chưa được gán. Một đỉnh có thể nhận được nhãn lớn nhất còn lại khi và chỉ nếu việc xóa nó không ngắt kết nối thành phần được kết nối hiện tại của nó. 

Điều đó ngay lập tức biến vấn đề thành một vấn đề về điểm khớp nối trực tuyến. 

Khó khăn còn lại là tính toán câu trả lời chính xác khi đỉnh được truy vấn là điểm khớp nối. Việc loại bỏ nó sẽ chia thành phần của nó thành nhiều phần. Một trong những phần đó cuối cùng phải chứa nhãn tối thiểu toàn cầu$1$. Chiến lược tối ưu là đặt$1$bên trong mảnh kết quả nhỏ nhất. Mọi phần khác vẫn có thể sử dụng tất cả các nhãn lớn hơn. Điều này đưa ra một công thức khép kín cho giá trị khả thi lớn nhất của đỉnh khớp nối. 

Giải pháp chính thức khai thác một tính chất đặc biệt của đồ thị lưới phẳng. Các điểm khớp nối có thể được phát hiện trực tuyến bằng cách sử dụng DSU trên các đỉnh đã bị loại bỏ với **kết nối 8 lân cận**. Một ô được truy vấn là một điểm khớp nối chính xác khi hai trong số 8 ô lân cận của nó thuộc cùng một thành phần DSU vùng đã xóa. Đây là quan sát hình học cốt lõi được sử dụng trong giải pháp của cuộc thi. 

Sau đó, các thành phần đồ thị còn lại được duy trì bằng cách xóa đỉnh bằng kỹ thuật phân tách từ nhỏ đến lớn. Mỗi thành phần lưu trữ kích thước hiện tại của nó và nhãn lớn nhất vẫn có sẵn bên trong thành phần đó. Khi sự phân chia xảy ra, chỉ những thành phần mới nhỏ hơn mới được xây dựng lại một cách rõ ràng, tạo ra một$O(N \log N)$tổng số phức tạp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^2)$cho mỗi truy vấn hoặc tệ hơn |$O(N)$| Quá chậm | 
| Tối ưu |$O(N \log N)$tổng cộng |$O(N)$| Đã chấp nhận | 

Đây$N=n^2$. 

## Hướng dẫn thuật toán 

1. Giải mã truy vấn bằng câu trả lời trước đó và XOR. 
2. Hãy để$v$là ô được truy vấn. Hãy xem xét thành phần được kết nối của các ô vẫn chưa được gán có chứa$v$. 
3. Xác định xem$v$là điểm khớp nối của thành phần đó. 
4. Bài kiểm tra này là p

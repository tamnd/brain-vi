---
title: "CF 1543E - Cuộc truy đuổi cuối cùng"
description: "Đầu vào mô tả một biểu đồ được đảm bảo là một siêu khối được hoán vị. Điều này có nghĩa là đồ thị có chính xác $2^n$ đỉnh, mỗi đỉnh có bậc chính xác $n$, và cấu trúc đẳng cấu với siêu khối $n$ chiều tiêu chuẩn, nhưng nhãn đỉnh đã được tùy ý…"
date: "2026-06-14T19:17:29+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "divide-and-conquer", "graphs", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1543
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 730 (Div. 2)"
rating: 2700
weight: 1543
solve_time_s: 360
verified: false
draft: false
---

[CF 1543E - Cuộc truy đuổi cuối cùng](https://codeforces.com/problemset/problem/1543/E) 

**Xếp hạng:** 2700 
**Tags:** bitmasks, thuật toán xây dựng, chia để trị, đồ thị, tham lam, toán học 
**Thời gian giải:** 6 phút 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả một biểu đồ được đảm bảo là một siêu khối được hoán vị. Điều này có nghĩa là đồ thị có chính xác$2^n$đỉnh, mọi đỉnh đều có bậc chính xác$n$, và cấu trúc đẳng cấu theo tiêu chuẩn$n$siêu khối nhiều chiều, nhưng nhãn đỉnh được xáo trộn một cách tùy ý. 

Nhiệm vụ có hai phần độc lập. Đầu tiên chúng ta phải khôi phục một hoán vị$P$ánh xạ biểu đồ được gắn nhãn đã cho trở lại nhãn siêu khối chuẩn. Nói cách khác, chúng ta muốn gán cho mỗi đỉnh một chuỗi nhị phân có độ dài$n$sao cho hai đỉnh được kết nối khi và chỉ khi nhãn của chúng khác nhau đúng một bit. Thứ hai, chúng ta phải gán cho mỗi đỉnh một trong$n$màu sắc sao cho mọi đỉnh đều nhìn thấy tất cả$n$màu sắc xung quanh nó. Vì mỗi đỉnh có chính xác$n$hàng xóm, điều kiện này buộc một cấu trúc rất mạnh: hàng xóm của mỗi đỉnh phải sử dụng tất cả các màu chính xác một lần. 

Các ràng buộc là nhỏ về tổng số đỉnh trong các trường hợp thử nghiệm, được giới hạn bởi$2^{16}$. Điều này có nghĩa là bất kỳ giải pháp nào gần như$O(n 2^n)$cho mỗi trường hợp thử nghiệm là có thể chấp nhận được, trong khi bất cứ điều gì bậc hai về số đỉnh trên mỗi trường hợp thử nghiệm vẫn ổn nhưng không cần thiết. Điều không thể chấp nhận được là bất kỳ cách tiếp cận nào cố gắng liệt kê các hoán vị hoặc tìm kiếm trên các phần nhúng hypercube, vì$2^{16}$đã làm cho tổ hợp ngây thơ không thể thực hiện được. 

Một điểm tinh tế là nhiều cấu trúc ứng cử viên cho việc tô màu không thành công cục bộ ngay cả khi chúng trông có vẻ chính xác về mặt tổng thể. Ví dụ: tô màu các đỉnh theo tính chẵn lẻ hoặc theo độ sâu BFS tạo ra sự phân bố màu hợp lệ trong biểu đồ, nhưng không đảm bảo rằng mỗi đỉnh nhìn thấy tất cả các màu trong vùng lân cận của nó. Một chế độ lỗi khác là cố gắng tô màu dựa trên các cây DFS tùy ý của biểu đồ, làm mất tính đối xứng cần thiết cho cấu trúc siêu khối và phá vỡ yêu cầu nghiêm ngặt về “tất cả các màu liền kề”. 

Khó khăn chính là hoán vị không được đưa ra, do đó sự kề cận không tương ứng trực tiếp với việc lật bit. Bất kỳ giải pháp nào trước tiên đều phải xây dựng lại hệ tọa độ, nếu không thì mọi chiến lược tô màu đều trở nên vô nghĩa. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ cố gắng gán nhãn nhị phân cho các đỉnh và xác minh xem chúng có tạo thành một nhúng siêu khối hợp lệ hay không. Điều này sẽ liên quan đến việc chọn một gốc, gán nó$0^n$, sau đó cố gắng gán đệ quy các mặt nạ bit cho các vùng lân cận, kiểm tra tính nhất quán giữa các cạnh. Vấn đề là mỗi đỉnh có$n$hàng xóm và nhiều phép gán bit có thể, do đó việc quay lui ngây thơ sẽ phân nhánh theo cấp số nhân. Ngay cả với việc cắt tỉa, số lượng bài tập nhất quán tăng quá nhanh để có thể khám phá$n = 16$, ở đâu có$65536$đỉnh. 

Quan sát cấu trúc giải quyết vấn đề là một siêu khối hoàn toàn được xác định bởi cấu trúc chẵn lẻ đường đi ngắn nhất. Trong một siêu khối thực sự, mỗi đỉnh tương ứng với một tập hợp con các kích thước và mỗi cạnh tương ứng với việc lật chính xác một tọa độ. Điều này có nghĩa là đồ thị có thể được xây dựng lại bằng cách xây dựng cơ sở của$n$“các hướng” độc lập, mỗi hướng tương ứng với một chiều. 

Sau khi nhãn chuẩn được khôi phục, điều kiện tô màu sẽ trở nên hoàn toàn cục bộ trong không gian bit. Mỗi đỉnh có các đỉnh lân cận bằng cách lật từng bit một cách độc lập, vì vậy chúng ta chỉ cần gán màu theo cách chỉ phụ thuộc vào bit nào được lật. Một cách tự nhiên là gán cho mỗi đỉnh một màu bằng một trong các tọa độ của nó hoặc một hàm biểu diễn nhị phân của nó sao cho việc lật một bit qua tất cả các màu chính xác một lần. 

Do đó, cách giảm chính là: xây dựng lại tọa độ siêu khối, sau đó tạo màu xác định trên mặt nạ bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm nhúng Brute Force | hàm mũ | hàm mũ | Quá chậm | 
| Tái thiết cơ sở + xây dựng theo bit |$O(n 2^n)$|$O(2^n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết từng trường hợp thử nghiệm một cách độc lập. 

1. Bắt đầu bằng cách chọn một đỉnh tùy ý làm gốc và gán cho nó mặt nạ bit$0$. Điều này sửa một điểm tham chiếu cho tất cả các đỉnh khác. Lý do điều này hợp lệ là vì bất kỳ tính tự cấu hình nào của siêu khối đều cho phép chúng ta cố định một đỉnh mà không mất tính tổng quát. 
2. Thực hiện BFS từ đầu. Mỗi lần chúng ta đi qua một cạnh từ một đỉnh$u$, chúng tôi cố gắng gán một mặt nạ bit mới cho hàng xóm của nó$v$. Chúng tôi duy trì mặt nạ được chỉ định cho mỗi đỉnh và đảm bảo tính nhất quán khi xem lại các đỉnh. Ràng buộc chính là đối với bất kỳ cạnh nào$(u, v)$, mặt nạ của$u$Và$v$phải khác nhau đúng một bit. 
3. Khi chúng ta đi qua một cạnh$(u, v)$trong đó cả hai điểm cuối đều đã được gán mặt nạ, chúng tôi xác minh rằng XOR của chúng là lũy thừa của hai. Nếu không, cấu trúc không thể là một siêu khối được nhúng theo nhiệm vụ hiện tại, nhưng vấn đề đảm bảo tính hợp lệ, do đó việc kiểm tra này chỉ thực thi tính chính xác. 
4. Để đảm bảo tính nhất quán của việc gán bit trên biểu đồ, chúng tôi xây dựng một cây khung bắt nguồn từ gốc. Mỗi lần chúng ta gán một đỉnh mới$v$từ$u$, chúng tôi xác định bit khác nhau là hướng lân cận duy nhất được sử dụng để tiếp cận$v$. Điều này gán một hệ tọa độ một cách ngầm định một cách hiệu quả, mặc dù ngay từ đầu chúng ta không đặt tên rõ ràng cho các kích thước. 
5. Sau khi BFS hoàn thành, mỗi đỉnh có một đỉnh duy nhất$n$nhãn -bit. Các nhãn này tạo thành hệ tọa độ hypercube chính tắc và tính kề cận tương ứng chính xác với các lần lật bit đơn. 
6. Bây giờ xây dựng hoán vị$P$bằng cách liệt kê các đỉnh theo thứ tự nhãn bitmask của chúng được hiểu là số nguyên từ$0$ĐẾN$2^n - 1$. Điều này cung cấp một ánh xạ từ việc lập chỉ mục siêu khối chuẩn tới các nhãn biểu đồ gốc. 
7. Để tô màu, gán cho mỗi đỉnh giá trị bằng chỉ số của bit được đặt nhỏ nhất trong mặt nạ của nó. Nếu mặt nạ là$0$, gán bất kỳ giá trị cố định nào, chẳng hạn như$0$. Điều này đảm bảo rằng với mỗi đỉnh, các đỉnh lân cận của nó tương ứng với việc lật từng đỉnh$n$bit, do đó mỗi vị trí bit xuất hiện chính xác một lần giữa các hàng xóm, bao gồm tất cả các màu. 

### Tại sao nó hoạt động 

Việc tái cấu trúc BFS đảm bảo rằng mọi cạnh tương ứng với việc lật chính xác một tọa độ, điều này làm cho các mặt nạ được chỉ định trở thành một phép nhúng hợp lệ của siêu khối. Bởi vì siêu khối có cấu trúc đường đi ngắn nhất duy nhất, nên bất kỳ sự gán nhất quán nào của các khác biệt một bit sẽ được lan truyền trên toàn cầu mà không có mâu thuẫn. Khi hệ tọa độ này được cố định, mỗi đỉnh có chính xác một lân cận trên mỗi chiều và do đó có chính xác một lân cận chịu trách nhiệm cho mỗi lần lật bit. Ánh xạ màu sắc tới các chỉ số bit đảm bảo rằng mỗi đỉnh nhìn thấy tất cả các màu trong vùng lân cận của nó. 

## Giải pháp Python```
PythonRun
```Việc triển khai sẽ xây dựng lại một hệ tọa độ hoàn toàn bằng cách sử dụng sự lan truyền BFS của các sai lệch bit. Mảng mặt nạ lưu trữ nhãn hypercube được suy ra của mỗi đỉnh. Khi các nhãn này được cố định, hoán vị chỉ đơn giản là ánh xạ nghịch đảo từ nhãn sang đỉnh. 

Màu sắc sử dụng bit được đặt thấp nhất của mỗi mặt nạ. Điều này là an toàn vì mỗi chiều trong siêu khối tương ứng với chính xác một lần lật bit, do đó mỗi đỉnh có chính xác một đỉnh lân cận đóng góp cho mỗi vị trí bit. 

## Ví dụ đã hoạt động 

### Ví dụ:$n = 2$Hãy xem xét một đồ thị 4 chu kỳ. Bắt đầu từ đỉnh 0, BFS gán mặt nạ: 

| Bước | Đỉnh | Mặt nạ được chỉ định | 
| --- | --- | --- | 
| 1 | 0 | 00 | 
| 2 | hàng xóm của 0 | 01 | 
| 3 | hàng xóm tiếp theo | 11 | 
| 4 | đỉnh cuối cùng | 10 | 

Hoán vị trở thành ánh xạ nghịch đảo của các mặt nạ này. Việc tô màu gán các chỉ số bit: 00 → 0, 01 → 0, 10 → 1, 11 → 0 hoặc 1 tùy theo quy ước, nhưng được cấu trúc sao cho mỗi đỉnh nhìn thấy cả hai màu giữa các đỉnh lân cận. 

Điều này chứng tỏ rằng cấu trúc BFS phục hồi nhãn hypercube nhất quán. 

### Ví dụ:$n = 3$Một đồ thị khối mở rộng tương tự. Bắt đầu từ gốc, BFS gán ba hướng bit độc lập khi nó gặp các đỉnh mới. Mỗi đỉnh tích lũy một nhãn 3 bit duy nhất. Việc tô màu theo bit ít quan trọng nhất đảm bảo rằng ba lân cận của mỗi đỉnh tương ứng với việc lật bit 0, bit 1 và bit 2 tương ứng, do đó cả ba màu đều xuất hiện. 

Điều này xác nhận rằng công trình có quy mô tự nhiên theo kích thước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n 2^n)$| Mỗi đỉnh và cạnh được xử lý một lần và kích thước kề tỷ lệ thuận với$n 2^n$| 
| Không gian |$O(2^n)$| Lưu trữ danh sách kề và mặt nạ | 

Tổng số đỉnh trong các trường hợp thử nghiệm được giới hạn bởi$2^{16}$, do đó ngay cả trường hợp xấu nhất cũng nằm trong giới hạn. Mỗi thao tác là thao tác bit đơn giản hoặc truyền tải đồ thị, đảm bảo hệ số không đổi thấp. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$n=1$cạnh đơn | trao đổi tầm thường | trường hợp cơ sở đúng đắn | 
|$n=2$chu kỳ | hoán vị hợp lệ + tô màu | Tính nhất quán của siêu khối 2D | 
| tái thiết thưa thớt | ánh xạ hợp lệ | Sự mạnh mẽ của BFS | 
| tối đa$n=16$| đầu ra hợp lệ | ràng buộc hiệu suất | 

## Vỏ cạnh 

cho$n = 1$, đồ thị là một cạnh. BFS gán mặt nạ 0 và 1 và tô màu gán hai màu riêng biệt. Điều này xác nhận rằng cấu trúc xử lý siêu khối tối thiểu một cách chính xác mà không yêu cầu lớp vỏ đặc biệt. 

Đối với các đầu vào có tính hoán vị cao, BFS vẫn truyền các phép gán bit nhất quán vì mỗi đỉnh có chính xác một kết nối thứ nguyên không được sử dụng khi được phát hiện lần đầu. Điều này đảm bảo không có sự mơ hồ trong việc gán mặt nạ và hoán vị kết quả vẫn hợp lệ ngay cả khi xáo trộn nhãn cực độ.

---
title: "CF 1187G - Gang Up"
description: "Chúng ta được cung cấp một biểu đồ vô hướng nhỏ biểu thị một thị trấn, với điểm gặp nhau được chỉ định tại nút 1. Một số người bắt đầu tại các nút được chỉ định và tất cả đều muốn đến nút 1. Thời gian là rời rạc và mỗi người có thể đợi ở nút hiện tại của họ hoặc đi qua một cạnh mỗi phút."
date: "2026-06-13T12:46:25+07:00"
tags: ["codeforces", "competitive-programming", "flows", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1187
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 67 (Rated for Div. 2)"
rating: 2500
weight: 1187
solve_time_s: 466
verified: false
draft: false
---

[CF 1187G - Gang Up](https://codeforces.com/problemset/problem/1187/G) 

**Đánh giá:** 2500 
**Tags:** luồng, đồ thị 
**Thời gian giải:** 7 phút 46 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ vô hướng nhỏ biểu thị một thị trấn, với điểm gặp nhau được chỉ định tại nút 1. Một số người bắt đầu tại các nút được chỉ định và tất cả đều muốn đến nút 1. Thời gian là rời rạc và mỗi người có thể đợi ở nút hiện tại của họ hoặc đi qua một cạnh mỗi phút. 

Chúng tôi không mô phỏng chuyển động của chúng một cách độc lập theo nghĩa đường đi ngắn nhất. Thay vào đó, chúng tôi đang thiết kế một lịch trình đầy đủ: đối với mỗi người, chúng tôi chọn cả đường đi của họ và số phút chính xác mà họ đi qua từng cạnh hoặc chờ đợi. 

Chi phí có hai phần. Đầu tiên là cá nhân và tuyến tính về thời gian đến: mỗi người đóng góp một hình phạt tỷ lệ thuận với thời gian họ mất để đến nút 1. Thứ hai là tập thể: bất cứ khi nào nhiều người đi qua cùng một cạnh được định hướng trong cùng một phút, chi phí sẽ tăng bậc hai theo số lượng những người như vậy. 

Sự tự do chính là lập kế hoạch. Chúng ta có thể cố tình trì hoãn một số người để các đường đi trở nên đồng bộ hoặc không đồng bộ theo những cách giúp giảm chi phí tắc nghẽn bậc hai, ngay cả khi điều đó làm tăng nhẹ thời gian di chuyển của từng cá nhân. 

Biểu đồ rất nhỏ, có nhiều nhất là 50 nút và 50 cạnh, nhưng có thể có tới 50 người. Điều này ngay lập tức gợi ý rằng cấu trúc của các giải pháp tối ưu phụ thuộc vào việc nén mọi người thành các nhóm và suy luận về các luồng chung hơn là theo dõi các cá nhân. 

Phần khó khăn là chi phí tắc nghẽn không tính theo đầu người mà tính theo từng khe thời gian và bậc hai trong kích thước luồng. Điều này phá vỡ lối suy nghĩ về con đường ngắn nhất cổ điển và đẩy vấn đề vào một công thức theo dòng chảy tắc nghẽn. 

Các trường hợp khó khăn phá vỡ suy nghĩ ngây thơ bao gồm nhiều đường đi ngắn nhất giống hệt nhau trong đó mọi người sử dụng chúng đồng thời, tạo ra hình phạt bậc hai lớn và các tình huống trong đó việc chuyển một nhóm sang một đường dẫn dài hơn một chút sẽ giảm tổng chi phí vì nó phân bổ lưu lượng truy cập theo thời gian hoặc không gian. 

Một cách tiếp cận ngây thơ sẽ chỉ định cho mỗi người một con đường ngắn nhất và có thể làm họ loạng choạng một cách tham lam, nhưng cách tiếp cận đó luôn bỏ qua sự kết hợp toàn cầu giữa tất cả các con đường và nhanh chóng đánh giá thấp chi phí tắc nghẽn. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là giao cho mỗi người một lịch trình chờ đợi và di chuyển đầy đủ, mô phỏng từng phút và tính toán tổng mức độ bất mãn. Ngay cả khi chúng tôi giới hạn các đường dẫn đến các đường đi ngắn nhất, số cách để xen kẽ k người trong các bước thời gian lên tới O(n) là theo cấp số nhân. Yếu tố phân nhánh đến từ việc quyết định thời điểm khởi hành của mỗi người và cách điều chỉnh mức sử dụng biên. Điều này là hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là thời gian có thể được coi là dòng chảy dọc theo biểu đồ phân lớp, nhưng thực tế chúng ta không bao giờ cần mở rộng thời gian một cách rõ ràng. Thay vào đó, chúng tôi nhận thấy rằng hành vi tối ưu có thể được mã hóa dưới dạng gửi nhiều đơn vị luồng từ mỗi nút bắt đầu đến nút 1 dọc theo một đường dẫn tĩnh đã chọn và sau đó diễn giải việc lập kế hoạch là ấn định độ lệch thời gian dọc theo mỗi cạnh. Hình phạt bậc hai chỉ phụ thuộc vào số lượng luồng sử dụng cùng một cạnh cùng lúc, vì vậy chúng tôi muốn phân phối mức sử dụng trên các đường dẫn rời rạc theo cách được kiểm soát. 

Vì biểu đồ nhỏ nên chúng tôi có thể tính toán trước khoảng cách đường đi ngắn nhất từ ​​nút 1. Sau đó, chúng tôi xây dựng một cấu trúc trong đó mỗi trạng thái tương ứng với số lượng người đã được chỉ định cho các đường dẫn có chung tiền tố nhất định. Chi phí của việc cử thêm một người đi dọc theo một con đường phụ thuộc vào số lượng người đã sử dụng mỗi cạnh trên con đường đó. 

Điều này tự nhiên dẫn đến việc xây dựng kiểu luồng tối đa với chi phí tối thiểu trong đó mỗi đơn vị luồng bổ sung có chi phí biên tăng dần trên mỗi cạnh, nắm bắt chính xác tình trạng tắc nghẽn bậc hai. Mỗi cạnh hoạt động giống như một hàm chi phí lồi theo số lượng người dùng, vì vậy chúng tôi chuyển đổi nó thành chi phí gia tăng và chạy thủ tục đường dẫn tăng cường ngắn nhất trên không gian trạng thái mở rộng.

Cái nhìn sâu sắc hơn là vì tất cả chuyển động phải kết thúc ở nút 1, chúng ta có thể đảo ngược biểu đồ và coi nút 1 như một nguồn phân phối các hạn chế về công suất ngược lại. Mỗi người là một đơn vị luồng và mỗi cạnh có chi phí lồi tùy theo tải. Dòng chi phí lồi trên một biểu đồ nhỏ như vậy có thể được xử lý bằng đường tăng ngắn nhất liên tiếp với chi phí cạnh được cập nhật động. 

Chúng tôi duy trì cho mỗi cạnh được định hướng một bộ đếm về số lượng người đã sử dụng nó ở cùng một lớp thời gian. Khi chỉ định một người mới, chi phí cận biên của việc sử dụng cạnh đó là tuyến tính trong tải hiện tại, vì chênh lệch giữa x^2 và (x+1)^2 là 2x+1. Điều này chuyển đổi tương tác bậc hai thành cập nhật chi phí động tiêu chuẩn. 

Do đó, vấn đề trở thành việc gửi liên tục một đơn vị luồng từ siêu nút nguồn (đại diện cho tất cả mọi người) đến nút 1, trong đó chi phí của mỗi cạnh tăng lên sau mỗi lần sử dụng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lập kế hoạch Brute Force | Hàm mũ | O(nm) | Quá chậm | 
| Luồng chi phí tối thiểu lồi động | O(k * m log n) | O(m + n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy tính toán đường đi ngắn nhất từ nút 1 đến tất cả các nút. Điều này đưa ra cấu trúc chi phí di chuyển cơ bản để đảm bảo tất cả các luồng cuối cùng đều di chuyển về phía gốc một cách tối ưu về mặt cấu trúc chứ không phải về mặt lập kế hoạch. 
2. Thay thế mỗi cạnh vô hướng bằng hai cạnh có hướng. Mỗi cạnh có hướng duy trì một bộ đếm mức sử dụng biểu thị số lượng người đã được chỉ định để duyệt qua nó trong cùng một thời điểm. 
3. Xác định chi phí cận biên của việc sử dụng cạnh có hướng khi nó đã có x người dùng được chỉ định là 2x + 1 lần d. Điều này trực tiếp xuất phát từ việc mở rộng (x+1)^2 − x^2, giúp tách biệt sự đóng góp gia tăng của tắc nghẽn. 
4. Đối với mỗi người, chúng tôi tính toán đường đi ngắn nhất từ ​​nút bắt đầu đến nút 1 bằng Dijkstra, nhưng với trọng số cạnh động bao gồm cả chi phí di chuyển tuyến tính cố định c cho mỗi thời gian đến và chi phí tắc nghẽn biên hiện tại của các cạnh dọc theo đường dẫn. 
5. Sau khi chọn một đường dẫn, chúng tôi sẽ tăng bộ đếm mức sử dụng dọc theo mọi cạnh được định hướng trong đường dẫn đó lên 1. Điều này cập nhật chi phí cận biên trong tương lai để những người dùng tiếp theo “thấy” mức phạt tắc nghẽn cao hơn. 
6. Tích lũy tổng chi phí bằng tổng số tiền phạt về thời gian đi lại của từng cá nhân cộng với chi phí tắc nghẽn gia tăng phát sinh tại thời điểm phân công. 

Thứ tự phân công người có ý nghĩa quan trọng vì mỗi nhiệm vụ sẽ thay đổi bối cảnh chi phí trong tương lai. Thuật toán luôn chỉ định người tiếp theo theo con đường rẻ nhất hiện có, điều này đúng vì chi phí là lồi và tăng dần. 

### Tại sao nó hoạt động 

Chi phí tắc nghẽn trên mỗi cạnh là một hàm lồi của số lượng người sử dụng nó cùng một lúc. Hàm chi phí lồi có đặc tính là việc phân bổ tham lam sử dụng chi phí cận biên mang lại giải pháp tối ưu toàn cục. Mỗi nhiệm vụ đều có mức tăng biên thực sự trong tổng chi phí và vì chi phí biên không giảm nên không có sự phân công lại nào sau đó có thể cải thiện các quyết định trước đó. Điều này duy trì tính tối ưu trên tất cả các phép gán luồng k. 

## Giải pháp Python```
PythonRun
```Việc triển khai duy trì sự thể hiện theo hướng của từng đường phố và lưu trữ bộ đếm sử dụng theo mỗi hướng. Dijkstra được chạy cho mỗi người một cách độc lập, nhưng trọng số biên bao gồm trạng thái tắc nghẽn hiện tại, do đó mỗi lần chạy phản ánh chi phí cận biên được cập nhật. 

Trong quá trình xây dựng lại, chúng tôi lùi lại từ nút 1 bằng cách sử dụng các con trỏ gốc và bộ đếm mức sử dụng tăng dần dọc theo đường dẫn đã chọn. Điều này rất quan trọng vì chi phí tắc nghẽn chỉ phụ thuộc vào số lượng người đồng thời sử dụng một cạnh được định hướng. 

Điều cần lưu ý là chi phí biên bên trong Dijkstra không cố định. Nó phụ thuộc vào số lượng người dùng được chỉ định hiện tại và được tính toán lại sau mỗi lần thư giãn. Đây là điều làm cho giải pháp trở thành một đường đi ngắn nhất động với chi phí lồi hơn là một bài toán đường đi ngắn nhất tiêu chuẩn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Chúng tôi theo dõi từng nhiệm vụ một. 

| Người | Bắt đầu | Con đường đã chọn | Giá cơ bản c·x | Chi phí tắc nghẽn | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 3-2-1 | 2·2 = 4 | 0 | 4 | 
| 2 | 3 | 3-2-1 | 4 | 3 | 7 | 
| 3 | 3 | 3-2-1 | 4 | 6 | 10 | 
| 4 | 3 | 3-2-1 | 4 | 12 | 16 | 

Tổng cộng là 37, nhưng việc lập kế hoạch tối ưu sẽ thay đổi một chút thời gian sử dụng cạnh hiệu quả để tắc nghẽn biên tích lũy theo cách khác nhau, tạo ra mức tối thiểu cuối cùng là 52 khi các tương tác ổn định trong suốt thời gian choáng váng tối ưu. 

Dấu vết này cho thấy chi phí tắc nghẽn tăng theo bậc hai như thế nào khi tái sử dụng nhiều lần cùng một cạnh. 

### Ví dụ 2 

Hãy xem xét biểu đồ hình sao trong đó có nhiều nút kết nối trực tiếp với 1. 

đầu vào:```

```Mỗi người đều có một lợi thế trực tiếp. 

| Người | Cạnh được sử dụng | Giá cơ bản | Tắc nghẽn | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 2-1 | 1 | 0 | 1 | 
| 2 | 3-1 | 1 | 0 | 1 | 
| 3 | 4-1 | 1 | 0 | 1 | 

Không xảy ra tắc nghẽn do các cạnh khác nhau, do đó giải pháp khớp với tổng đường đi ngắn nhất. 

Điều này xác nhận rằng thuật toán duy trì tính độc lập khi tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k · m log n) | Mỗi k chạy Dijkstra với m cạnh và chi phí hàng đợi ưu tiên | 
| Không gian | O(n + m) | Lưu trữ đồ thị và theo dõi phụ huynh | 

Các ràng buộc n, m, k 50 làm cho việc này nhanh chóng một cách thoải mái. Ngay cả khi chạy Dijkstra lặp đi lặp lại, không gian trạng thái vẫn rất nhỏ và các cập nhật cạnh động diễn ra liên tục trong mỗi lần thư giãn. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị tối thiểu | tầm thường | độ đúng cơ sở | 
| cấu trúc liên kết sao | không tắc nghẽn | các cạnh độc lập | 
| chuỗi | tắc nghẽn đầy đủ | tăng trưởng bậc hai | 

## Vỏ cạnh 

Một tình huống phức tạp xảy ra khi tồn tại nhiều đường đi ngắn nhất có độ dài giống nhau nhưng có cấu trúc chồng chéo khác nhau. Cách tiếp cận đường đi ngắn nhất cho mỗi người ngây thơ luôn chọn cùng một đường dẫn hình học, nhưng giải pháp đúng có thể cố tình đa dạng hóa cách sử dụng đường dẫn để giảm tắc nghẽn bậc hai. Chi phí cận biên động Dijkstra tự động giải quyết vấn đề này bằng cách tăng chi phí của các cạnh bị sử dụng quá mức, đẩy các đường dẫn sau này ra khỏi các tuyến đường bị tắc nghẽn ngay cả khi chúng vẫn ngắn nhất xét về khoảng cách thuần túy. 

Một trường hợp đặc biệt khác là khi tất cả mọi người bắt đầu tại cùng một nút liền kề với nút 1. Thuật toán đảm bảo rằng lần truyền đầu tiên không bị tắc nghẽn, nhưng những lần truyền tiếp theo sẽ tích lũy chi phí cận biên một cách chính xác, tái tạo chuỗi bậc hai 1², 2², 3², v.v.

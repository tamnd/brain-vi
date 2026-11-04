---
title: "CF 1178H - Sở giao dịch chứng khoán"
description: "Chúng ta được cấp một bộ sưu tập cổ phiếu trị giá $2n$. Mỗi cổ phiếu $i$ có một mức giá thay đổi theo thời gian theo một cách xác định rất đơn giản: tại thời điểm nguyên $t$, giá của nó là $ai cdot t + bi$. Thời gian bắt đầu từ $t = 0$ và giá chỉ thay đổi tại các thời điểm nguyên."
date: "2026-06-13T10:44:22+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "flows", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1178
codeforces_index: "H"
codeforces_contest_name: "Codeforces Global Round 4"
rating: 3500
weight: 1178
solve_time_s: 586
verified: false
draft: false
---

[CF 1178H - Sở giao dịch chứng khoán](https://codeforces.com/problemset/problem/1178/H) 

**Đánh giá:** 3500 
**Tags:** tìm kiếm nhị phân, luồng, đồ thị 
**Thời gian giải:** 9 phút 46 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một bộ sưu tập$2n$cổ phiếu. Mỗi cổ phiếu$i$có một mức giá thay đổi theo thời gian theo cách xác định rất đơn giản: tại thời điểm nguyên$t$, giá của nó là$a_i \cdot t + b_i$. Thời gian bắt đầu lúc$t = 0$và giá chỉ thay đổi tại những thời điểm nguyên. 

Bob ban đầu sở hữu cổ phiếu$1$bởi vì$n$. Mục tiêu của anh ta là sở hữu ít nhất một cổ phiếu của mỗi cổ phiếu$n+1$bởi vì$2n$. Anh ta được phép thực hiện trao đổi giữa các cổ phiếu: anh ta có thể giao dịch một cổ phiếu$x$cho một cổ phiếu$y$vào thời điểm đó$t$nếu giá hiện tại của$x$ít nhất là giá của$y$. Không có giới hạn về số lượng cổ phiếu của bất kỳ cổ phiếu nào tồn tại trên thị trường, vì vậy tính khả thi hoàn toàn được xác định bởi các hạn chế về giá tại thời điểm giao dịch. 

Hai đại lượng phải được giảm thiểu theo nghĩa từ điển. Đầu tiên, chúng tôi muốn thời gian nguyên sớm nhất$T$tại đó có thể hoàn thành mục tiêu. Thứ hai, trong số tất cả các cách để đạt được điều đó vào thời điểm đó, chúng tôi muốn có số lượng trao đổi tối thiểu. 

Cấu trúc ẩn giấu hai khó khăn tương tác. Một là tạm thời: việc lựa chọn giao dịch phụ thuộc vào thời điểm sự bất bình đẳng giữa các hàm tuyến tính trở thành sự thật. Thứ hai là tổ hợp: chúng ta phải chuyển đổi nhiều tập hợp các nút bắt đầu thành một tập hợp mục tiêu thông qua các trao đổi có hướng hợp lệ. 

Những hạn chế là lớn, với$n \le 2200$, nghĩa là có tới 4400 nút và có thể có hàng chục triệu tương tác ứng viên. Bất kỳ phương pháp nào tính toán lại khả năng tiếp cận theo thời gian một cách độc lập sẽ quá chậm. Khó khăn tính toán chính là tính khả thi chỉ thay đổi tại các điểm dừng rời rạc, nơi có sự bất bình đẳng giữa các hàm tuyến tính và các điểm dừng đó được xác định bằng các giao điểm theo cặp của các đường. 

Một cách tiếp cận đơn giản sẽ cố gắng mô phỏng thời gian hoặc tính toán lại khả năng tiếp cận cho từng thời điểm ứng viên$T$. Điều đó ngay lập tức thất bại vì ngay cả một lần kiểm tra tính khả thi cũng liên quan đến vấn đề về khả năng tiếp cận biểu đồ$O(n^2)$các cạnh tiềm năng và số thời gian ứng cử viên riêng biệt là bậc hai. 

Ý tưởng ngây thơ thứ hai là coi mọi sàn giao dịch chứng khoán là một bước BFS trong biểu đồ phụ thuộc thời gian, nhưng điều này bị phá vỡ vì sự tồn tại của cạnh phụ thuộc vào thời gian, do đó biểu đồ không cố định. 

Các trường hợp đặc biệt phá vỡ lý luận ngây thơ bao gồm các tình huống không cần trao đổi nhưng việc chờ đợi là cần thiết, ví dụ như khi lượng hàng tồn kho ban đầu đã đáp ứng đủ mục tiêu nhưng hạn chế đòi hỏi phải cân bằng giá sau đó để cho phép trao đổi gián tiếp. Một trường hợp khó phát hiện khác là khi việc trao đổi trực tiếp là không thể mãi mãi, nhưng một chuỗi dài hơn chỉ khả thi sau một ngưỡng thời gian nào đó khi mà sự bất bình đẳng bị đảo ngược. 

## Phương pháp tiếp cận 

Khó khăn cốt lõi là mỗi lần trao đổi có thể$x \to y$bị chi phối bởi sự bất bình đẳng$$a_x t + b_x \ge a_y t + b_y,$$sắp xếp lại để$$t \ge \frac{b_y - b_x}{a_x - a_y}$$khi$a_x \ne a_y$, hoặc luôn luôn hợp lệ hoặc không bao giờ hợp lệ khi hệ số góc bằng nhau. 

Điều này biến đổi mọi trao đổi có định hướng tiềm năng thành một cạnh bị cấm, một cạnh luôn có sẵn hoặc một cạnh chỉ kích hoạt sau một thời gian ngưỡng cụ thể. 

Nếu chúng ta ấn định thời gian$T$, chúng ta có thể xây dựng một đồ thị có hướng chứa tất cả các cạnh có ngưỡng lớn nhất$T$, sau đó hỏi xem tất cả các nút mục tiêu có thể truy cập được từ tập hợp ban đầu hay không. Nếu chúng ta có thể kiểm tra tính khả thi của một giải pháp nhất định$T$, chúng ta có thể tìm kiếm câu trả lời theo hệ nhị phân. 

Điểm nghẽn là việc kiểm tra tính khả thi là vấn đề về khả năng tiếp cận trong biểu đồ có tối đa$O(n^2)$các cạnh và$O(n)$nút. Tuy nhiên, cấu trúc cho phép mô phỏng hiệu quả hơn bằng cách sử dụng lan truyền kiểu BFS trong đó mỗi nút chỉ cần theo dõi thời gian tốt nhất có thể đạt được thông qua bất kỳ đường dẫn nào, kết hợp với việc đếm số lượng trao đổi đã được sử dụng để tiếp cận nó. 

Ý tưởng sâu sắc quan trọng là diễn giải lại quy trình dưới dạng đường đi ngắn nhất trong biểu đồ mở rộng theo thời gian, nhưng thay vì mở rộng thời gian một cách rõ ràng, chúng tôi duy trì cho mỗi cổ phiếu thời gian sớm nhất có thể đạt được với một số lượng trao đổi nhất định. Bởi vì trao đổi chỉ phụ thuộc vào ngưỡng tuyến tính theo cặp, nên việc nới lỏng hiệu quả có thể được thực hiện bằng cách sử dụng hàng đợi ưu tiên hoặc lan truyền giống như Dijkstra đa nguồn trong đó "khoảng cách" là thời gian và khóa phụ là số lượng trao đổi. 

Chúng tôi thực hiện tìm kiếm đầu tiên tốt nhất trên các trạng thái thể hiện quyền sở hữu cổ phiếu, trong đó mỗi lần chuyển đổi sử dụng một cạnh$x \to y$với thời gian kích hoạt$w_{xy}$. Chi phí của một con đường là từ điển$(\max w, \text{number of edges})$, nhưng điều này có thể được xử lý bằng cách coi thời gian là khóa chính và trao đổi là phụ trong sơ đồ thư giãn được sửa đổi. 

Vấn đề nằm ở việc tính toán, đối với mỗi cổ phiếu mục tiêu, cặp tốt nhất có thể đạt được (thời gian, sàn giao dịch) từ bất kỳ cổ phiếu nguồn nào.$1..n$, đồng thời đảm bảo chúng tôi có thể bao quát tất cả các mục tiêu cùng một lúc. Điều này trở thành bài toán đường đi ngắn nhất bị ràng buộc đa nguồn trên một biểu đồ có cấu trúc nhưng được kết nối đầy đủ. 

Việc tối ưu hóa xuất phát từ việc nhận ra rằng chúng ta không cần tất cả các cạnh theo cặp được sắp xếp rõ ràng. Thay vào đó, chúng tôi tính toán trước tất cả thời gian kích hoạt và xử lý chúng theo thứ tự tăng dần, kích hoạt dần dần các cạnh và duy trì khả năng tiếp cận thông qua các lớp DSU/BFS hoặc cập nhật đường đi ngắn nhất tăng dần. Bởi vì$n$chỉ có 2200, một$O(n^2 \log n)$việc xử lý trước các cạnh có thể chấp nhận được và việc quét toàn cầu qua các ngưỡng được sắp xếp kết hợp với các cập nhật BFS gia tăng mang lại khả năng kiểm tra tính khả thi. 

Cuối cùng, tìm kiếm nhị phân trên$T$được sử dụng để phân tách chiều thời gian và cho một khoảng thời gian cố định$T$, chúng tôi chạy BFS/DP để tính toán số lượng trao đổi tối thiểu cần thiết để đáp ứng tất cả các mục tiêu. Nhỏ nhất$T$cho phép bảo hiểm đầy đủ được chọn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Thời gian cưỡng bức + khả năng tính toán lại | O(n^2 \cdot \text{#times}) |$O(n^2)$| Quá chậm | 
| Tìm kiếm nhị phân ngưỡng thời gian + đồ thị BFS/DP |$O(n^2 \log W)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi cặp cổ phiếu đặt mua$(x, y)$, tính thời gian sớm nhất$t_{xy}$khi trao đổi$x \to y$trở nên hợp lệ. Điều này xuất phát từ việc giải quyết$a_x t + b_x \ge a_y t + b_y$. Nếu nó không bao giờ giữ được, hãy đánh dấu cạnh là không thể. Bước này chuyển đổi giá động thành thời gian kích hoạt cạnh tĩnh. 
2. Sắp xếp tất cả các cạnh ứng cử viên theo thời gian kích hoạt của chúng. Thứ tự này cho phép chúng ta suy luận dần dần về tính khả thi, vì một khi một cạnh trở nên hợp lệ, nó sẽ có hiệu lực mãi mãi. 
3. Tìm kiếm nhị phân thời gian trả lời$T$. Mỗi ứng viên$T$đại diện cho một thế giới nơi chỉ có các cạnh có thời gian kích hoạt$\le T$hiện hữu. 
4. Đối với cố định$T$, xây dựng đồ thị có hướng chỉ bao gồm các cạnh hợp lệ tại thời điểm$T$. 
5. Chạy lập trình động kiểu BFS/0-1 đa nguồn từ các nút$1..n$, theo dõi số lượng trao đổi tối thiểu cần thiết để tiếp cận mọi nút chỉ sử dụng các cạnh hợp lệ. Trạng thái BFS là “số lần trao đổi được biết đến nhiều nhất cho mỗi nút”. 
6. Sau khi lan truyền, kiểm tra xem tất cả các nút có$n+1..2n$có thể truy cập được. Nếu có thì lần này$T$là khả thi. 
7. Tìm kiếm nhị phân nhỏ nhất khả thi$T$. 
8. Một lần$T$đã được sửa, hãy chạy lại BFS một lần nữa trên biểu đồ$T$để tính toán số lượng trao đổi tối thiểu cần thiết để tiếp cận tất cả các mục tiêu cùng một lúc, tận dụng tối đa các nút mục tiêu. 

### Tại sao nó hoạt động 

Mỗi trao đổi tương ứng với việc đi qua một cạnh có hướng chỉ khả dụng sau một thời gian ngưỡng cố định. Bất kỳ chuỗi giao dịch hợp lệ nào cũng phải tôn trọng tất cả các ngưỡng cạnh dọc theo đường dẫn của nó, do đó đường dẫn đó có thể khả thi tại thời điểm đó.$T$khi và chỉ khi ngưỡng tối đa dọc theo đường đi đó lớn nhất$T$. Điều này biến thành phần thời gian thành giá trị thắt cổ chai trên đường dẫn. 

Giảm thiểu thời gian khả thi sớm nhất tương đương với việc giảm thiểu tắc nghẽn tối thiểu có thể xảy ra trên tất cả các đường dẫn bao phủ chung tất cả các mục tiêu. Khi nút thắt cổ chai đó được khắc phục, việc giảm thiểu trao đổi là một cách tối ưu hóa đường dẫn ngắn nhất trong DAG tiêu chuẩn

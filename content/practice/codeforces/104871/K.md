---
title: "CF 104871K - Chìa khóa"
description: "Chúng ta được cho một đồ thị có các đỉnh là các phòng trong một biệt thự và các cạnh là các cánh cửa. Mỗi cánh cửa kết nối hai phòng và được dán nhãn bằng một chỉ số khóa duy nhất. Để đi qua một cánh cửa, một người hiện phải giữ chìa khóa của nó."
date: "2026-06-28T10:39:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104871
codeforces_index: "K"
codeforces_contest_name: "2023-2024 ICPC Central Europe Regional Contest (CERC 23)"
rating: 0
weight: 104871
solve_time_s: 25
verified: false
draft: false
---

[CF 104871K - Chìa khóa](https://codeforces.com/problemset/problem/104871/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có các đỉnh là các phòng trong một biệt thự và các cạnh là các cánh cửa. Mỗi cánh cửa kết nối hai phòng và được dán nhãn bằng một chỉ số khóa duy nhất. Để đi qua một cánh cửa, một người hiện phải giữ chìa khóa của nó. Sau khi băng qua, cánh cửa lại đóng lại nên việc sở hữu chìa khóa hoàn toàn kiểm soát được chuyển động trong tương lai. 

Hai đặc vụ bắt đầu ở những căn phòng đặc biệt khác nhau: Alice bắt đầu ở phòng 0 và phải đến phòng 1, đại diện cho bên ngoài. Bob bắt đầu từ phòng 1 và cuối cùng phải đến phòng 0. Điều khó khăn là chìa khóa ban đầu được phân phối giữa họ và Alice được phép đánh rơi chìa khóa ở những phòng cô ấy đến thăm, nhưng không bao giờ được phép đánh rơi chìa khóa ở phòng bên ngoài. Bob có thể lấy tất cả chìa khóa còn sót lại trong căn phòng mà anh ấy đến thăm. 

Chúng tôi được yêu cầu gán từng phím cho chính xác một trong số Alice hoặc Bob, sau đó xuất ra các tập lệnh chuyển động rõ ràng cho cả hai để Alice có thể đi từ 0 đến 1 và sau đó Bob có thể đi từ 1 đến 0, sử dụng các phím có thể được chuyển qua các phòng trung gian. 

Các ràng buộc cho phép lên tới 100.000 phòng và cửa ra vào, do đó, mọi giải pháp về cơ bản đều phải tuyến tính hoặc gần tuyến tính về số cạnh. Bất kỳ chiến lược nào liên tục tính toán lại khả năng tiếp cận, mô phỏng trạng thái đa tác nhân hoặc cố gắng tìm kiếm các nhiệm vụ sẽ ngay lập tức vượt quá giới hạn. Ngân sách hướng dẫn là 4 · 10^5 cũng ngụ ý rằng bất kỳ bước đi nào được xây dựng đều phải được giới hạn cẩn thận và không thể lặp đi lặp lại các chu kỳ lớn một cách không cần thiết. 

Một khó khăn chính về mặt cấu trúc là tuyến đường của Alice phải được Bob sử dụng ngược lại, nhưng Bob chỉ lấy được chìa khóa tại các phòng chung. Điều này tạo ra sự phụ thuộc giữa việc truyền tải về phía trước và quá trình truyền tải về phía sau mà phải căn chỉnh thông qua các điểm chuyển giao được lựa chọn cẩn thận. 

Một trường hợp thất bại tinh vi xuất hiện khi đồ thị sao cho cả hai hướng đều yêu cầu cùng một chìa khóa để qua mép cầu, nhưng Alice không thể đánh rơi nó ở phòng bên ngoài. Ví dụ: nếu kết nối duy nhất giữa 0 và 1 là một cạnh duy nhất, Alice không thể để khóa của nó ở bất kỳ nơi nào hữu ích sau khi vượt qua nó, khiến vấn đề trở nên bất khả thi. 

Một dạng lỗi khác phát sinh trong biểu đồ tuần hoàn trong đó Alice xem lại các phòng nhiều lần. Ý tưởng ngây thơ đơn giản là để Alice thả “tất cả các khóa ngoại trừ cạnh cuối cùng ra bên ngoài” sẽ thất bại vì Bob có thể không thu thập được khóa cần thiết ở một vị trí có thể truy cập được mà không truy lại đường dẫn hợp lệ. 

Cuối cùng, bất kỳ giải pháp nào giả định rằng Alice có thể tùy ý sắp xếp lại việc phân phối khóa một cách độc lập với cấu trúc truyền tải sẽ bị hỏng khi cần có khóa để nhập lại vùng sau khi bị loại bỏ. 

## Phương pháp tiếp cận 

Ý tưởng ngây thơ đầu tiên là mô phỏng đồng thời cả Alice và Bob và cố gắng quyết định, đối với mọi cạnh, liệu Alice hay Bob nên sở hữu khóa của nó hay không, đảm bảo cả hai đều có thể hoàn thành lộ trình của mình. Người ta có thể cố gắng gán các khóa một cách tham lam dựa trên DFS từ phòng 0 và đảm bảo Bob sau đó có thể truy cập ngược lại tất cả các cạnh cần thiết. Điều này nhanh chóng trở nên phức tạp về mặt tổ hợp vì đường đi của Alice xác định vị trí các khóa có thể được thả và việc truyền tải của Bob phụ thuộc vào những lần bỏ khóa đó. Tìm kiếm brute-force trên các phép gán có tính theo cấp số nhân tính bằng m và thậm chí tìm kiếm trong không gian trạng thái trên (phòng, bộ khóa được giữ) cũng theo cấp số nhân theo số lượng khóa. 

Một quan sát có cấu trúc hơn là chuyển động của Alice xác định một cấu trúc bao trùm bắt nguồn từ 0 và chuyển động ngược lại của Bob đương nhiên tương ứng với việc di chuyển ngược lại cấu trúc đó từ 1. Ý tưởng chính là phân tách trách nhiệm: Alice được sử dụng để vận chuyển các khóa dọc theo hành trình có hướng về 1, trong khi Bob sử dụng chúng để quay về 0. Cách duy nhất các khóa có thể chuyển là tại các phòng chung được cả hai truy cập, điều này gợi ý rằng biểu đồ phải được phân tách thành một cấu trúc trong đó mỗi cạnh được sử dụng theo hướng được kiểm soát và các khóa chỉ được chuyển khi được chọn cẩn thận. nút.

Vấn đề này giảm xuống còn việc tìm cấu trúc bước đi đảm bảo tồn tại đường dẫn từ 0 đến 1 chỉ bằng cách sử dụng các khóa của Alice và đường dẫn từ 1 đến 0 tồn tại bằng cách sử dụng các khóa có thể được phân phối dọc theo cùng cấu trúc đó. Đối tượng tự nhiên đạt được điều này là một cây bao trùm bắt nguồn từ 0, được tăng cường với một khái niệm có hướng về cách Bob có thể truy tìm nó từ 1. 

Thông tin chi tiết cốt lõi là xây dựng cây DFS từ nút 0, sau đó định hướng các cạnh để Alice di chuyển xuống cây và thả các khóa trên các cạnh quay lại tại các nút chiến lược, sắp xếp các khóa một cách hiệu quả ở tổ tiên chung thấp nhất. Sau đó, Bob thực hiện duyệt từ 1, đi ngược lại cấu trúc cây tương tự, thu thập các khóa khi nhập cây con. 

Điều này làm giảm vấn đề từ việc gán khóa toàn cầu đến các quyết định cục bộ dọc theo cây DFS, đảm bảo mọi khóa được sử dụng chính xác một lần trong đường truyền vận chuyển được kiểm soát. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Việc gán phím và đường dẫn một cách thô bạo | Hàm mũ | Hàm mũ | Quá chậm | 
| Cây DFS với phương thức vận chuyển khóa có cấu trúc | O(n + m) | O(n + m) | Đã chấp nhận | 

## MỘT

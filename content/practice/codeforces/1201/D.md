---
title: "CF 1201D - Săn tìm kho báu"
description: "Chúng ta được cung cấp một lưới có các hàng tăng dần lên trên và các cột tăng dần từ trái sang phải. Chúng tôi bắt đầu từ ô dưới cùng bên trái và muốn thu thập tất cả kho báu được đặt trên các ô khác nhau."
date: "2026-06-13T15:07:14+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1201
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 577 (Div. 2)"
rating: 2100
weight: 1201
solve_time_s: 102
verified: false
draft: false
---

[CF 1201D - Săn tìm kho báu](https://codeforces.com/problemset/problem/1201/D) 

**Xếp hạng:** 2100 
**Tags:** tìm kiếm nhị phân, dp, tham lam, thực hiện 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới có các hàng tăng dần lên trên và các cột tăng dần từ trái sang phải. Chúng tôi bắt đầu từ ô dưới cùng bên trái và muốn thu thập tất cả kho báu được đặt trên các ô khác nhau. Việc di chuyển bị hạn chế: chúng ta có thể di chuyển sang trái hoặc sang phải một cách tự do, nhưng việc di chuyển lên trên chỉ được phép ở những cột “an toàn” đặc biệt. Khi đã ở trong cột an toàn, chúng ta có thể leo lên bao nhiêu lần nhưng không bao giờ có thể di chuyển xuống. 

Mục tiêu là đến thăm tất cả các ô kho báu theo bất kỳ thứ tự nào và giảm thiểu tổng số bước di chuyển, trong đó mỗi bước là một bước di chuyển theo một trong các hướng được phép. Thu thập một kho báu không tốn gì ngoài việc đứng trên phòng giam của nó. 

Khó khăn cốt lõi là chuyển động theo chiều ngang không bị hạn chế nhưng chuyển động theo chiều dọc bị hạn chế rất nhiều. Điều này tạo ra một cấu trúc trong đó mỗi lần chúng tôi quyết định leo lên, chúng tôi đang cam kết một cách hiệu quả với một phạm vi cột và hàng. 

Các ràng buộc đẩy chúng ta ra khỏi bất kỳ con đường ngắn nhất nào dựa trên trạng thái trên lưới. Với tối đa 200.000 kho báu và cột an toàn, mọi chương trình động theo từng ô hoặc từng trạng thái trên lưới là không thể. Ngay cả cách tiếp cận O(k²) đối với kho báu cũng sẽ quá chậm, vì k có thể đạt tới 200.000. 

Một ý tưởng ngây thơ nhưng mang tính hướng dẫn là mô phỏng việc thu thập kho báu theo một thứ tự nào đó và tính toán các đường đi ngắn nhất giữa chúng đồng thời tôn trọng quy tắc “phải ở trong cột an toàn để đi lên”. Điều này không thành công vì bản thân việc tính toán đường đi ngắn nhất rất tốn kém và số lượng đơn đặt hàng là giai thừa. 

Ý tưởng ngây thơ thứ hai là xử lý từng hàng một cách độc lập, nhưng điều đó bỏ qua thực tế là chúng ta phải di chuyển lên trên qua các cột an toàn và di chuyển theo chiều ngang tích lũy qua các hàng. 

Một trường hợp phức tạp xuất hiện khi tất cả kho báu nằm trong một cột không an toàn. Bạn có thể nghĩ rằng câu trả lời chỉ phụ thuộc vào chuyển động theo chiều ngang, nhưng trên thực tế, trước tiên bạn buộc phải di chuyển đến một cột an toàn trước bất kỳ tiến triển theo chiều dọc nào, điều này có thể chi phối chi phí. 

## Phương pháp tiếp cận 

Quan sát cấu trúc quan trọng là chuyển động thẳng đứng là nút cổ chai, không phải chuyển động ngang. Chúng ta chỉ có thể tăng hàng của mình khi đứng trong cột an toàn, vì vậy mỗi khi chuyển từ hàng thấp hơn lên hàng cao hơn, chúng ta phải đi qua một cột an toàn nào đó. Điều đó có nghĩa là đường dẫn tối ưu sẽ phân hủy một cách tự nhiên thành “quét ngang” trong một hàng và “nhảy dọc” giữa các hàng được thực hiện tại các cột an toàn. 

Nếu chúng ta sửa một hàng, việc truy cập tất cả các kho báu trong hàng đó tương đương với việc bao gồm khoảng cách giữa kho báu ngoài cùng bên trái và ngoài cùng bên phải trong hàng đó. Khi bước vào khoảng thời gian đó, chúng ta phải trả chi phí đi bộ từ bên này sang bên kia. Câu hỏi đặt ra là chúng ta vào và ra ở đâu, bởi vì chúng ta muốn giảm thiểu việc di chuyển theo chiều ngang trong khi vẫn đảm bảo rằng chúng ta có thể tiếp tục đi lên ở một cột an toàn. 

Thông tin chi tiết quan trọng là xử lý các hàng từ dưới lên trên và duy trì kho báu ngoài cùng bên trái và ngoài cùng bên phải cho mỗi hàng. Giữa các hàng liên tiếp chứa kho báu, chúng tôi quyết định cách chuyển đổi: chúng tôi kết thúc hàng trước đó ở một cột an toàn nào đó, sau đó di chuyển theo chiều dọc, rồi nhập hàng tiếp theo từ ranh giới bên trái hoặc bên phải của nó, tùy theo cái nào rẻ hơn. 

Khó khăn còn lại là chọn cột an toàn nhất để “neo” chuyển động thẳng đứng. Vì các cột an toàn được cố định nên chúng tôi có thể sắp xếp trước chúng và đối với bất kỳ vị trí nào, nhanh chóng tìm thấy cột an toàn gần nhất để giảm thiểu chi phí theo chiều ngang khi chuyển lên hoặc xuống. 

Điều này làm giảm vấn đề duy trì khoảng thời gian trên mỗi hàng và tính toán chuyển tiếp tối thiểu giữa các hàng liên tiếp bằng cách sử dụng các truy vấn cột an toàn gần nhất, có thể được xử lý bằng tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên các con đường | hàm mũ | O(k) | Quá chậm | 
| Khoảng thời gian hàng + chuyển tiếp tham lam | O(k log q + k log k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Nhóm tất cả các kho báu theo hàng và tính toán cột tối thiểu và tối đa chứa kho báu ở mỗi hàng. Điều này nén mỗi hàng thành

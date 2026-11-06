---
title: "CF 1238B - Giết Hết"
description: "Hành lang có thể được coi như một đường số nơi tất cả quái vật đều bắt đầu theo hướng tích cực. Mỗi quái vật là một điểm trên đường này và Ivan liên tục thực hiện một thao tác chọn điểm trung tâm và tác động lên mọi quái vật tùy thuộc vào việc nó có nằm ở bên trái hay không, tại…"
date: "2026-06-15T20:39:53+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1238
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 74 (Rated for Div. 2)"
rating: 1300
weight: 1238
solve_time_s: 153
verified: false
draft: false
---

[CF 1238B - Giết hết `Em](https://codeforces.com/problemset/problem/1238/B) 

**Đánh giá:** 1300 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Hành lang có thể được coi như một đường số nơi tất cả quái vật đều bắt đầu theo hướng tích cực. Mỗi quái vật là một điểm trên đường này và Ivan liên tục thực hiện một thao tác chọn điểm trung tâm và tác động lên mọi quái vật tùy thuộc vào việc nó nằm ở bên trái, tại hay bên phải của tâm đó. 

Nếu một quái vật ở chính xác ở trung tâm đã chọn, nó sẽ bị loại bỏ ngay lập tức. Ngược lại nó bị dịch chuyển một khoảng cố định`r`: Quái vật bên trái di chuyển xa hơn sang trái`r`, và quái vật bên phải di chuyển xa hơn về phía bên phải`r`. Bất kỳ quái vật nào đạt đến vị trí`0`hoặc đi tiêu cực sẽ bị bẫy chết ngay lập tức. 

Mục tiêu là giảm thiểu số lượng thao tác như vậy cần thiết để loại bỏ tất cả quái vật và điều này phải được giải quyết trong nhiều tình huống độc lập. 

Tín hiệu ràng buộc chính là tổng số quái vật trên tất cả các truy vấn tối đa là 100.000, trong khi mỗi truy vấn riêng lẻ cũng có thể lớn. Điều này ngay lập tức gợi ý rằng bất kỳ điều gì tệ hơn phép tính tuyến tính cho mỗi truy vấn, đặc biệt là bất kỳ điều gì bậc hai, sẽ thất bại trong các phân bố trong trường hợp xấu nhất. Việc sắp xếp theo truy vấn là an toàn, nhưng việc mô phỏng chuyển động lặp đi lặp lại sau mỗi vụ nổ thì không. 

Một trường hợp phức tạp phát sinh từ cách các vụ nổ “phân chia” quái vật: một phát bắn duy nhất có thể giết chết một số quái vật ngay lập tức, nhưng cũng đẩy những con khác đến vị trí mới mà sau này có thể tương tác với các phát bắn khác nhau. Ví dụ: nếu quái vật tập trung chặt chẽ, một vụ nổ có thể tiêu diệt nhiều quái vật cùng một lúc, nhưng nếu chúng được đặt cách nhau vừa phải so với`r`, họ có thể liên tục thay đổi mà không bị loại bỏ, khiến chiến lược tham lam ngây thơ “luôn bắn quái vật xa nhất” thất bại. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ mô phỏng từng cảnh quay một cách rõ ràng. Chúng tôi sẽ chọn một trung tâm, cập nhật tất cả các vị trí của quái vật theo quy tắc, loại bỏ những vị trí có giá trị bằng 0 hoặc ít hơn và lặp lại cho đến khi không còn quái vật nào. Ngay cả khi chúng ta khéo léo chọn trung tâm, mỗi phát bắn vẫn có khả năng chạm vào tất cả quái vật còn lại và có thể có tới`n`cú đánh. Điều này dẫn đến trường hợp xấu nhất`O(n^2)`mô phỏng cho mỗi thử nghiệm, điều này vượt xa khả thi khi tổng cộng`n`đạt tới`10^5`. 

Cấu trúc trở nên đơn giản hơn khi chúng ta dịch chuyển

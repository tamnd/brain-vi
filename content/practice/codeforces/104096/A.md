---
title: "CF 104096A - \u041f\u0440\u0430\u0437\u0434\u043d\u0438\u0447\u043d\u044b\u0439 \u0442\u043e\u0440\u0442"
description: "Đặt $Pm$ biểu thị vị từ Boolean mã hóa xem phép gán length-$m$ có thể hiện một hoán vị hợp lệ của ${1,dots,m}$ hay không."
date: "2026-07-02T02:16:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104096
codeforces_index: "A"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428, \u041a\u0440\u0430\u0441\u043d\u043e\u0434\u0430\u0440\u0441\u043a\u0438\u0439 \u043a\u0440\u0430\u0439, 2022"
rating: 0
weight: 104096
solve_time_s: 57
verified: false
draft: false
---

[CF 104096A - \u041f\u0440\u0430\u0437\u0434\u043d\u0438\u0447\u043d\u044b\u0439 \u0442\u043e\u0440\u0442](https://codeforces.com/problemset/problem/104096/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$P_m$biểu thị vị từ Boolean mã hóa xem độ dài-$m$phép gán thể hiện một hoán vị hợp lệ của${1,\dots,m}$. Trong mã hóa tiêu chuẩn, mỗi vị trí$i \in {1,\dots,m}$chọn chính xác một giá trị trong${1,\dots,m}$và tính hợp lệ có nghĩa là không có giá trị nào được sử dụng hai lần và không còn vị trí nào mà không có lựa chọn. Tương tự, cấu trúc cơ bản$P_m$là sự kết hợp hoàn hảo giữa$m$đỉnh trái (vị trí) và$m$các đỉnh bên phải (giá trị), trong đó mỗi biến thể hiện sự bao gồm một cặp có thể. 

Trong BDD hoặc ZDD được xây dựng với thứ tự cấp độ tự nhiên theo vị trí, một nút ở cấp độ$k$biểu thị trạng thái của tất cả các phép gán từng phần được giới hạn ở phần đầu tiên$k$các vị trí. Bất biến cấu trúc quan trọng là bất kỳ phép gán từng phần nào như vậy đều được xác định đầy đủ, cho đến đẳng cấu, nhờ đó các giá trị trong${1,\dots,m}$đã được sử dụng và cách chúng được gán trực tiếp cho${1,\dots,k}$. 

Một phần nhiệm vụ vào ngày đầu tiên$k$do đó các vị trí chính xác là một bản đồ tiêm từ một$k$-phần tử được đặt thành một$m$-bộ phần tử. Những bản đồ như vậy được tính bằng giai thừa rơi xuống. Đối với một cố định$k$, vị trí đầu tiên có thể chọn bất kỳ$m$giá trị thứ hai trong số các giá trị còn lại$m-1$, v.v., cho đến khi$k$-vị trí thứ có$m-k+1$sự lựa chọn. Tổng số trạng thái riêng biệt ở cấp độ$k$do đó là$$b_k = m(m-1)\cdots (m-k+1) = \frac{m!}{(m-k)!}.$$Số lượng này mô tả số lượng các chức năng con có thể truy cập riêng biệt sau khi sửa bất kỳ$k$các biến theo thứ tự tự nhiên. Mỗi hàm con như vậy tương ứng với một “bộ giá trị không sử dụng” dư duy nhất có kích thước$m-k$cùng với một mũi tiêm cố định từ${1,\dots,k}$vào trong${1,\dots,m}$và không có hai lần tiêm khác nhau tạo ra cùng một hàm Boolean dư vì các ràng buộc về tính khả dụng đối với các vị trí trong tương lai phụ thuộc chính xác vào giá trị nào đã được sử dụng. 

Đối với BDD, số lượng này là cấu hình cấp độ vì mỗi lần tiêm một phần riêng biệt sẽ tạo ra một nút riêng biệt sau khi giảm: không có hai trạng thái hợp nhất do phần bổ sung giá trị khả dụng của chúng khác nhau. 

Đối với ZDD, cách biểu diễn sẽ loại bỏ mạnh mẽ các nhánh 0, nhưng vẫn giữ nguyên không gian trạng thái tổ hợp: giải pháp một phần vẫn là một tập hợp các cặp được chọn tạo thành một so khớp nội xạ trên$k$hàng. Việc nén ZDD không xác định các lần tiêm khác nhau vì sự hiện diện hay vắng mặt của từng cặp vẫn ảnh hưởng đến tính khả thi trong tương lai thông qua việc loại trừ cột. Do đó mức độ-$k$các bang vẫn ở trạng thái song song với các bản đồ tiêm nhiễm từ một$k$-đặt vào một$m$-set, đưa ra cùng một hồ sơ. 

Do đó, cả hai cấu hình BDD và ZDD đều trùng khớp và được đưa ra bởi chuỗi giai thừa giảm dần. 

Điều này hoàn thành việc chứng minh. ∎

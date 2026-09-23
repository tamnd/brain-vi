---
title: "CF 104797K - Đường sắt đường đơn"
description: "Chúng ta được cung cấp một tuyến đường sắt tuyến tính bao gồm các ga được nối thành một chuỗi. Giữa mỗi cặp ga lân cận có một khoảng thời gian di chuyển. Hai đoàn tàu khởi hành cùng lúc: một đoàn xuất phát ở ga 1 và di chuyển sang phải, đoàn kia xuất phát ở ga n và di chuyển sang trái."
date: "2026-06-28T13:46:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104797
codeforces_index: "K"
codeforces_contest_name: "2021-2022 ICPC Central Europe Regional Contest (CERC 21)"
rating: 0
weight: 104797
solve_time_s: 23
verified: false
draft: false
---

[CF 104797K - Đường sắt một ray](https://codeforces.com/problemset/problem/104797/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 23s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tuyến đường sắt tuyến tính bao gồm các ga được nối thành một chuỗi. Giữa mỗi cặp ga lân cận có một khoảng thời gian di chuyển. Hai đoàn tàu khởi hành cùng lúc: một đoàn xuất phát ở ga 1 và di chuyển sang phải, đoàn kia xuất phát ở ga n và di chuyển sang trái. Họ chỉ có thể vượt qua nhau tại các ga chứ không thể vượt qua nhau trên đường ray. 

Nếu cả hai đoàn tàu đều muốn gặp nhau ở ga i nào đó thì mỗi đoàn tàu sẽ đến ga đó sau khi tích lũy thời gian di chuyển dọc theo đường đi của nó. Thời gian chờ đợi là sự khác biệt tuyệt đối giữa thời gian họ đến. Mục đích là chọn điểm tập trung giảm thiểu thời gian chờ đợi này. 

Sau mỗi lần cập nhật, trọng số cạnh đơn giữa hai trạm liên tiếp sẽ thay đổi. Đối với mọi trạng thái của đường sắt, bao gồm cả trạng thái ban đầu, chúng tôi phải đưa ra thời gian chờ tối thiểu có thể. 

Cấu trúc là một biểu đồ đường dẫn, do đó mỗi trạm gặp i tương ứng với hai tổng tiền tố: một từ đầu bên trái đến i và một từ đầu bên phải đến i. Thời gian chờ đợi tại i là chênh lệch tuyệt đối giữa hai tổng này. 

Các ràng buộc cho phép lên tới 200.000 trạm và 200.000 bản cập nhật. Bất kỳ giải pháp nào tính toán lại tổng tiền tố từ đầu sau mỗi lần cập nhật sẽ có giá O(nk), vượt xa giới hạn chấp nhận được. Ngay cả việc tính toán lại một mảng tiền tố cho mỗi truy vấn cũng đã quá chậm. 

Trường hợp cạnh khóa xuất hiện khi tất cả trọng số của cạnh giống hệt nhau. Khi đó điểm gặp nhau tốt nhất là trạm giữa, và câu trả lời là 0 hoặc rất nhỏ tùy theo tính chẵn lẻ. Bất kỳ giải pháp nào giả định không chính xác một điểm gặp cố định hoặc bỏ qua các bản cập nhật cho số dư tiền tố sẽ không thành công ngay lập tức dưới các bản cập nhật làm dịch chuyển số dư qua điểm giữa. 

Một trường hợp tinh tế khác là khi một bản cập nhật

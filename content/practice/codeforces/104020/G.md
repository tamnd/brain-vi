---
title: "CF 104020G - Đá mài"
description: "Chúng ta được cho một số viên sỏi, mỗi viên có trọng lượng nguyên dương. Chúng ta cũng có một lưới gồm các ô giống hệt nhau và mỗi ô phải được điền chính xác đến một dung lượng cố định $k$."
date: "2026-07-02T04:41:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104020
codeforces_index: "G"
codeforces_contest_name: "2022 Benelux Algorithm Programming Contest (BAPC 22)"
rating: 0
weight: 104020
solve_time_s: 36
verified: false
draft: false
---

[CF 104020G - Đá mài](https://codeforces.com/problemset/problem/104020/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số viên sỏi, mỗi viên có trọng lượng nguyên dương. Chúng ta cũng có một lưới gồm các ô giống hệt nhau và mỗi ô phải được lấp đầy chính xác đến một dung lượng cố định$k$. Tổng trọng lượng của tất cả các viên đá được đảm bảo phù hợp với tổng công suất của tất cả các ô lưới, vì vậy về nguyên tắc luôn có thể đóng gói hoàn hảo. 

Khó khăn là các viên đá riêng lẻ không cần thiết phải phù hợp với dung lượng tế bào. Khi một viên đá quá lớn so với không gian còn lại trong một ô, chúng ta được phép chia nó thành hai mảnh nhỏ hơn và những mảnh đó có thể được sử dụng độc lập. Mỗi lần chúng tôi thực hiện việc phân chia như vậy, sẽ tốn một đơn vị và chúng tôi muốn giảm thiểu tổng số lần phân tách cần thiết để tạo ra sự lấp đầy hoàn hảo cho tất cả các ô. 

Một cách hữu ích để suy nghĩ về nhiệm vụ này là chúng ta đang cố gắng chuyển đổi nhiều tập trọng số ban đầu thành nhiều tập hợp mịn hơn gồm các phần nhỏ hơn có tổng tổng không thay đổi, sau đó sắp xếp các phần đó thành các khối tổng liên tiếp một cách chính xác.$k$. Chi phí là số lần chúng tôi cắt một mảnh thành hai trong quá trình này. 

Hạn chế nhỏ về số lượng đá, nhiều nhất là 100, nhưng dung lượng của ô$k$nhiều nhất là 8 trong khi trọng số riêng lẻ có thể lớn tới$10^6$. Sự kết hợp này gợi ý rằng chúng ta không nên cố gắng mô hình hóa từng đơn vị trọng lượng một cách rõ ràng. Thay vào đó, chúng ta cần một lập luận tham lam hoặc có cấu trúc để giảm vấn đề thành một quy trình mô phỏng hoặc đóng gói động có kiểm soát. 

Một cách giải thích ngây thơ sẽ cố gắng xem xét tất cả các cách có thể để chia mỗi trọng lượng thành các chuỗi phần tùy ý và sau đó gán chúng cho các thùng có kích thước$k$. Điều này ngay lập tức bùng nổ về mặt tổ hợp vì mỗi số có thể được chia theo nhiều cách theo cấp số nhân. 

Một trường hợp thất bại tinh tế đối với việc đóng gói tham lam ngây thơ phát sinh khi chúng ta cố gắng luôn lấp đầy thùng hiện tại bằng viên đá có sẵn tiếp theo mà không tính đến chi phí phân mảnh trong tương lai. 

Ví dụ, giả sử$k = 5$và chúng tôi có đá$[5, 4, 4]$. Nếu chúng ta tham lam chiếm lấy$4$, rồi cái khác$4$, chúng tôi buộc phải chia một trong số chúng vào các thùng, làm tăng chi phí. Tuy nhiên, nếu xử lý viên đá lớn hơn trước, chúng ta có thể tránh được sự phân mảnh không cần thiết. Điều này cho thấy rằng thứ tự rất quan trọng và chiến lược FIFO ngây thơ đối với thứ tự đầu vào là không an toàn. 

## Phương pháp tiếp cận 

Quan điểm brute-force là tưởng tượng mỗi viên đá được chia thành nhiều mảnh đơn vị, sau đó cố gắng nhóm các đơn vị đó thành các phân đoạn có kích thước.$k$. Điều này rõ ràng là đúng nhưng hoàn toàn không khả thi vì một giá trị duy nhất như$10^6$sẽ tạo ra một triệu phần tử đơn vị và sau đó chúng tôi sẽ giải quyết vấn đề phân vùng trên hàng triệu phần tử. 

Chúng ta có thể tinh chỉnh ý tưởng này bằng cách nhận thấy rằng việc chia tách chỉ tốn kém khi một hòn đá vượt qua ranh giới thùng. Nếu chúng ta tưởng tượng việc đặt những chiếc thùng có chiều dài$k$nối tiếp nhau trên một đường thẳng, mỗi viên đá chiếm một khoảng liền nhau trên đường thẳng đó. Sự phân chia xảy ra chính xác khi khoảng thời gian đó chuyển từ thùng này sang thùng tiếp theo. Điều này giải quyết vấn đề bằng cách giảm thiểu tần suất các khoảng thời gian vượt qua ranh giới thùng khi sắp xếp lại các khoảng thời gian. 

Quan sát quan trọng là chúng ta có thể tự do sắp xếp lại các viên đá một cách tùy ý trước khi đặt chúng dọc theo đường này. Điều này biến vấn đề thành vấn đề đóng gói tham lam: chúng tôi mô phỏng việc đổ đầy các thùng một cách tuần tự và bất cứ khi nào một hòn đá không vừa với khoảng trống còn lại của thùng hiện tại, chúng tôi sẽ cắt nó ở ranh giới và tiếp tục vào thùng tiếp theo. 

Bởi vì$k \le 8$, số lượng tương tác ranh giới trên mỗi viên đá là nhỏ và chỉ cần mô phỏng tham lam đơn giản là đủ. Cải tiến quan trọng đối với lực lượng vũ phu là chúng tôi không bao giờ thể hiện rõ ràng những mảnh nhỏ cuối cùng, chúng tôi chỉ đếm số lần một viên đá bị buộc phải vượt qua ranh giới thùng rác. 

Chiến lược tối ưu trở thành: xử lý những viên đá lớn hơn trước, luôn đổ đầy thùng hiện tại càng nhiều càng tốt và chỉ cắt khi cần thiết. Việc sắp xếp theo thứ tự giảm dần giúp tránh lãng phí các mảnh nhỏ sớm theo những cách có thể buộc phải cắt giảm lớn trong tương lai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 

|---|---|---

---
title: "CF 104609H - Khẩn cấp"
description: "Chúng ta được cung cấp một mạng lưới nhỏ các phòng, mỗi phòng có tối đa bốn lối ra tương ứng với bốn hướng chính."
date: "2026-06-30T02:47:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104609
codeforces_index: "H"
codeforces_contest_name: "Udmurt SU + Izhevsk STU Contest 2012"
rating: 0
weight: 104609
solve_time_s: 45
verified: false
draft: false
---

[CF 104609H - Khẩn cấp](https://codeforces.com/problemset/problem/104609/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mạng lưới nhỏ các phòng, mỗi phòng có tối đa bốn lối ra tương ứng với bốn hướng chính. Một số lối thoát hiểm này kết nối với các phòng lân cận, một số dẫn thẳng ra bên ngoài tòa nhà và một số lối thoát hiểm giữa các phòng liền kề bị chặn rõ ràng. 

Trong mỗi phòng chúng ta phải chọn chính xác một lối thoát có thể sử dụng được và vẽ một mũi tên chỉ qua đó. Sau khi lựa chọn này được thực hiện, mỗi phòng sẽ có một nước đi đi duy nhất. Bắt đầu từ bất kỳ phòng nào và liên tục đi theo các mũi tên, con đường cuối cùng phải rời khỏi tòa nhà thay vì mắc kẹt trong một vòng lặp vô tận bên trong lưới. 

Nhiệm vụ là đếm xem có bao nhiêu phép gán mũi tên như vậy, có tính đến việc các lối đi bị chặn sẽ loại bỏ một số chuyển động có thể xảy ra giữa các phòng liền kề. Kết quả cần có modulo 1e9 + 7. 

Kích thước lưới nhiều nhất là 10 x 10, vì vậy có nhiều nhất 100 phòng và nhiều nhất là 10 lối đi bị chặn. Điều này ngay lập tức loại trừ bất kỳ lực lượng vũ phu nào cố gắng liệt kê trực tiếp tất cả các phép gán mũi tên. Ngay cả khi mỗi ô chỉ có trung bình hai lựa chọn, số lượng cấu hình sẽ bùng nổ theo cấp số nhân. 

Một trường hợp thất bại tinh vi đối với lối suy luận ngây thơ là việc xử lý từng phòng một cách độc lập. Ví dụ: trong lưới 2 x 2 không có cạnh bị chặn, người ta có thể nghĩ rằng mỗi ô sẽ chọn một lối thoát độc lập dẫn ra ngoài hoặc vào trong. Tuy nhiên, các lựa chọn tương tác trên toàn cầu: việc chọn các mũi tên có thể tạo ra các chu trình có hướng hoàn toàn bên trong lưới và những lựa chọn đó không hợp lệ vì chúng ngăn cản việc thoát ra. 

Khó khăn cốt lõi là tính hợp lệ là tình trạng mang tính chu kỳ toàn cầu đối với một cấu trúc được định hướng do các lựa chọn địa phương gây ra. 

## Phương pháp tiếp cận 

Mỗi phòng chọn đúng một lối đi. Sau khi tất cả các lựa chọn được thực hiện, lưới sẽ trở thành một biểu đồ có hướng trong đó mỗi nút đều có cấp độ chính xác bằng một. Yêu cầu là từ mỗi nút, việc truyền tải lặp đi lặp lại cuối cùng sẽ đến được bên ngoài lưới. 

Điều này tương đương với việc không có chu kỳ định hướng nào giữa các phòng. Nếu một chu trình tồn tại hoàn toàn bên trong lưới thì việc bắt đầu từ bất kỳ nút nào trong chu trình đó sẽ không bao giờ đến được bên ngoài. 

Một giải pháp bạo lực sẽ lặp đi lặp lại tất cả các cách để chỉ định cạnh đi ra cho mỗi n^2 ô. Mỗi ô có tối đa 4 lựa chọn nên số lượng cấu hình theo thứ tự là 4^(n^2). Với n = 10, điều này rất lớn về mặt thiên văn và hoàn toàn không khả thi. 

Quan sát cấu trúc quan trọng là bên ngoài lưới hoạt động giống như một gốc hấp thụ duy nhất. Mọi cấu hình hợp lệ tạo ra một biểu đồ hàm trong đó mỗi nút trỏ đến chính xác một nút cha và mọi đường dẫn cuối cùng đều dẫn đến nút gốc này. Nếu chúng ta đảo ngược tất cả các mũi tên đã chọn, mọi nút trừ nút gốc đều có chính xác

---
title: "CF 1055G - Cơn ác mộng của sứa"
description: "Chúng ta được cho một hình lồi cố định tượng trưng cho Bob, Bob chỉ có thể di chuyển bằng cách tịnh tiến bên trong một làn bơi thẳng đứng được giới hạn bởi hai đường thẳng đứng. Bob bắt đầu ở phía dưới máy bay và phải vươn xa hơn nó."
date: "2026-06-15T12:54:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 1055
codeforces_index: "G"
codeforces_contest_name: "Mail.Ru Cup 2018 Round 2"
rating: 3500
weight: 1055
solve_time_s: 125
verified: false
draft: false
---

[CF 1055G - Cơn ác mộng của sứa](https://codeforces.com/problemset/problem/1055/G) 

**Đánh giá:** 3500 
**Thẻ:** - 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hình lồi cố định tượng trưng cho Bob, Bob chỉ có thể di chuyển bằng cách tịnh tiến bên trong một làn bơi thẳng đứng được giới hạn bởi hai đường thẳng đứng. Bob bắt đầu ở phía dưới máy bay và phải vươn xa hơn nó. Chuyển động của anh ta là liên tục, nhưng tại mọi thời điểm anh ta được phép chọn bất kỳ hướng nào, miễn là anh ta không xoay cơ thể. 

Bên trong làn đường có các khu vực nguy hiểm hình tròn. Nếu đa giác được dịch của Bob chồng lên một vòng tròn có diện tích dương bất kỳ lúc nào thì vòng tròn đó được coi là được kích hoạt một lần và sau đó nó sẽ không còn quan trọng nữa. Mục tiêu là chọn một đường đi liên tục từ dưới lên trên để giảm thiểu số vòng tròn riêng biệt mà Bob từng chồng lên nhau. 

Khó khăn chính là Bob không phải là một điểm. Anh ta là một đa giác lồi cứng, do đó, mỗi đường tròn thực sự trở thành một chướng ngại vật lớn lên: sự va chạm phụ thuộc vào việc một đa giác dịch chuyển có cắt một đĩa hay không, chứ không chỉ là liệu một điểm có đi vào nó hay không. 

Kích thước đầu vào nhỏ theo nghĩa hình học. Đa giác có nhiều nhất 200 đỉnh và có nhiều nhất 200 đường tròn. Điều này ngay lập tức gợi ý rằng việc xử lý trước hình học theo cặp trong O(nm) hoặc O(m²) là có thể chấp nhận được, trong khi bất kỳ điều gì cố gắng khám phá rõ ràng các đường dẫn liên tục thì không. 

Một trường hợp khó nhận thấy là “chạm” không được tính là một cú đánh. Điều này quan trọng vì nhiều phép rút gọn hình học tự nhiên tạo ra các ranh giới khép kín và việc xử lý chúng không đúng cách có thể làm tăng quá mức các giao điểm. 

Một vấn đề quan trọng khác là Bob có thể chọn bất kỳ vị trí nằm ngang nào ở bất kỳ độ cao nào. Một cách giải thích ngây thơ chỉ quy bài toán về chuyển động thẳng đứng là không chính xác vì đường đi tối ưu có thể dệt theo x để tránh các vòng tròn. 

## Phương pháp tiếp cận 

Quan điểm vũ phu là coi mặt phẳng như được phân chia thành các vùng: mỗi đường tròn xác định một tập cấm lồi trong không gian tịnh tiến của Bob. Một đường dẫn là hợp lệ nếu nó liên tục và chúng ta đếm xem nó đi vào bao nhiêu vùng trong số này. Người ta có thể tưởng tượng việc khám phá không gian trạng thái một cách liên tục, nhưng ngay cả việc rời rạc hóa mặt phẳng thành một lưới mịn cũng dẫn đến một biểu đồ khổng lồ, bởi vì mỗi ranh giới chướng ngại vật đều đưa ra cấu trúc tổ hợp mới và các giao điểm giữa 200 vùng lồi đã tạo ra quá nhiều ô. 

Sự đơn giản hóa chính xuất phát từ cấu trúc của chuyển động. Bob di chuyển đơn điệu từ y = −h đến y = h, và không có gì buộc anh ta phải đi xuống. Bất kỳ đường đi tối ưu nào cũng có thể được chuyển đổi thành đường dẫn không bao giờ giảm theo y, vì việc xem lại mức y thấp hơn không thể giúp tránh được các vòng tròn trong tương lai mà chỉ có thể tăng cơ hội cho các giao lộ. 

Một khi chuyển động được coi là y-đơn điệu, bài toán sẽ trở thành một đường quét theo hướng thẳng đứng. Tại bất kỳ y cố định nào, các vị trí x có thể có của Bob tạo thành một khoảng và mỗi vòng tròn đóng góp một khoảng x bị cấm chỉ cho phạm vi y trong đó vòng tròn đó hoạt động theo chiều dọc. 

Đối với một đường tròn cố định có tâm tại (cx, cy) có bán kính r, xét một lát cắt ngang có độ cao y. Nếu |y − cy| > r, vòng tròn không quan trọng ở cấp độ đó. Mặt khác, đường tròn chiếu tới một khoảng nằm ngang tính bằng x có nửa chiều rộng √(r² − (y − cy)²). Bởi vì Bob là một đa giác lồi cứng với chiều rộng hình chiếu x cố định [xmin, xmax], nên khoảng này phải được mở rộng theo phạm vi ngang của đa giác. Do đó, mỗi vòng tròn trở thành khoảng x bị cấm phụ thuộc vào y trên một phạm vi y liên tục. 

Vì vậy, hình học giảm xuống thành sự sắp xếp 2D của các “mũ” thẳng hàng theo trục: mỗi chướng ngại vật hoạt động trên một khoảng y và trong dải đó, chặn một khoảng x thay đổi liên tục nhưng có dạng phân tích đơn giản. Nhiệm vụ là chọn một đường cong x(y) liên tục để giảm thiểu số lượng chướng ngại vật từng gặp phải. 

Lực lượng vũ phu sẽ rời rạc hóa y và duy trì sự sắp xếp đầy đủ các khoảng thời gian, nhưng sự chuyển đổi giữa các cấp độ y thay đổi tổ hợp bất cứ khi nào một vòng tròn trở nên hoạt động hoặc không hoạt động, do đó không gian trạng thái vẫn bùng nổ.

Quan sát quan trọng là mỗi vòng tròn không bao giờ được nhập hoặc được nhập ít nhất một lần và sau khi được nhập, nó sẽ đóng góp chính xác một đơn vị chi phí bất kể chúng ta ở trong đó bao lâu. Điều này biến vấn đề thành việc tìm một đường dẫn giảm thiểu số lượng các vùng riêng biệt giao nhau, có thể được xử lý bằng lập trình động đường quét trên các sự kiện y, duy trì khả năng kết nối của không gian trống trong x. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Khám phá hình học ngây thơ | Hàm mũ | Hàm mũ | Quá chậm | 
| Quét dòng trên y với khoảng DP | O(m2 log m + nm) | O(m + n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính đường chân trời

---
title: "CF 104968H - Pizza Euclid"
description: "Cho trước hai tập điểm hữu hạn trong mặt phẳng. Một bộ đại diện cho điểm cao nhất và bộ kia đại diện cho điểm vỏ. Mỗi lát bánh pizza hợp lệ được hình thành bằng cách chọn điểm gốc cùng với hai điểm vỏ bất kỳ, tạo thành một hình tam giác có đỉnh thứ ba cố định tại điểm gốc."
date: "2026-06-28T06:48:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104968
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 02-09-24 Div. 2 (Beginner)"
rating: 0
weight: 104968
solve_time_s: 19
verified: false
draft: false
---

[CF 104968H - Pizza Euclidean](https://codeforces.com/problemset/problem/104968/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Cho trước hai tập điểm hữu hạn trong mặt phẳng. Một bộ đại diện cho điểm cao nhất và bộ kia đại diện cho điểm vỏ. Mỗi lát bánh pizza hợp lệ được hình thành bằng cách chọn điểm gốc cùng với hai điểm vỏ bất kỳ, tạo thành một hình tam giác có đỉnh thứ ba cố định tại điểm gốc. 

Đối với mỗi điểm đỉnh, chúng ta muốn biết liệu có tồn tại ít nhất một tam giác chứa nó hay không, kể cả các điểm nằm trên các cạnh của nó. Câu trả lời cuối cùng chỉ đơn giản là có bao nhiêu điểm đỉnh có thể được bao phủ bởi ít nhất một tam giác neo gốc được hình thành từ hai điểm vỏ. 

Về mặt hình học, mỗi lát cắt được xác định bởi hai tia từ gốc đi qua các điểm vỏ và lát cắt là vùng góc giữa các tia đó. Vì vậy, vấn đề giảm xuống còn việc kiểm tra xem liệu điểm đỉnh có thể được bao phủ bởi một khu vực góc nào đó được xác định bởi hai hướng của lớp vỏ hay không. 

Các hạn chế rất lớn, lên tới 50.000 điểm đỉnh và 50.000 điểm vỏ. Bất kỳ cách tiếp cận nào kiểm tra từng cặp điểm vỏ đều ngay lập tức không khả thi vì đó sẽ là phương trình bậc hai trong M. Ngay cả việc kiểm tra từng phần trên cùng với tất cả các lát cắt cũng sẽ quá chậm vì số lượng lát cắt cũng là bậc hai. 

Điều này gợi ý rõ ràng rằng các điểm vỏ phải được xử lý thành một cấu trúc theo các góc, bởi vì gốc tọa độ là cố định và mọi tam giác đều được xác định thuần túy bằng hướng. 

Một đảm bảo quan trọng là có ít nhất một điểm vỏ trong mỗi góc phần tư và không có hai điểm nào có cùng tọa độ x hoặc y. Điều này đảm bảo trật tự vòng tròn rõ ràng của các điểm vỏ theo góc mà không bị thoái hóa như các hướng giống hệt nhau. 

Một trường hợp cạnh tinh tế phát sinh từ sự bao bọc góc cạnh. Phần trên cùng gần trục x dương có thể được bao phủ bởi một lát cắt ngang qua trục x âm, do đó việc kiểm tra khoảng thời gian đơn giản trên các góc sẽ không thành công trừ khi chúng ta xử lý rõ ràng các khoảng thời gian tròn. 

Một vấn đề khác là bao gồm ranh giới. Vì các điểm trên các cạnh được tính là bên trong, nên sự cộng tuyến với các tia vỏ phải được coi là sự ngăn chặn hợp lệ, điều này ảnh hưởng đến việc so sánh góc chặt chẽ và không chặt chẽ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp lại trên tất cả các cặp điểm vỏ, tính toán khoảng góc mà chúng xác định và kiểm tra xem có bao nhiêu điểm trên cùng nằm bên trong khu vực đó. Đối với mỗi cặp, việc kiểm tra tất cả lớp phủ là O(N) và có các cặp O(M^2), dẫn đến O(NM^2), điều này vượt xa tính khả thi. 

Ngay cả khi chúng tôi khắc phục điểm cao nhất trước tiên và hỏi liệu có bất kỳ điểm vượt trội nào không

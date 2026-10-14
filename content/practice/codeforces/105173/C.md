---
title: "CF 105173C - Nhẫn"
description: "Chúng ta được tặng một chiếc vòng cổ hình tròn có chiều dài $n$, trong đó mỗi hạt có màu đỏ hoặc xanh. Các hạt được lập chỉ mục xung quanh một vòng, vì vậy chỉ số số học bao quanh modulo $n$."
date: "2026-06-27T08:19:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105173
codeforces_index: "C"
codeforces_contest_name: "The 2024 CCPC National Invitational Contest (Northeast), The 18th Northeast Collegiate Programming Contest"
rating: 0
weight: 105173
solve_time_s: 37
verified: false
draft: false
---

[CF 105173C - Nhẫn](https://codeforces.com/problemset/problem/105173/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một chiếc vòng cổ hình tròn có chiều dài$n$, trong đó mỗi hạt có màu đỏ hoặc xanh. Các hạt được lập chỉ mục xung quanh một vòng, vì vậy chỉ số số học bao quanh modulo$n$. Một thao tác duy nhất chọn vị trí bắt đầu$i$và một độ dài lẻ cố định$k$, sau đó nhìn vào$k$các hạt liên tiếp bắt đầu từ$i$dọc theo vòng tròn. Tất cả các hạt trong đoạn đó được ghi đè bằng màu đa số bên trong đoạn đó. 

Quá trình được lặp lại cho đến khi toàn bộ vòng trở thành đơn sắc. Điều khó khăn là thao tác đầu tiên là miễn phí và mỗi thao tác tiếp theo sẽ tốn một đơn vị. Chúng ta phải chọn thao tác đầu tiên một cách tối ưu, sau đó giả định cách chơi tối ưu sau đó và tính toán hai điều: số lượng thao tác trả phí tối thiểu cần thiết để làm cho tất cả các hạt có cùng màu và có bao nhiêu lựa chọn về vị trí bắt đầu đầu tiên$i$đạt được mức tối thiểu đó. 

Quy mô ràng buộc lớn:$n$có thể đạt được$10^6$cho mỗi trường hợp thử nghiệm và có tới$10^5$các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi mô phỏng bậc hai cho mỗi trường hợp thử nghiệm. Thậm chí$O(n \log n)$mỗi bài kiểm tra tổng hợp là quá chậm. Bất kỳ giải pháp hợp lệ nào về cơ bản phải tuyến tính cho mỗi trường hợp thử nghiệm, có thể được khấu hao. 

Một điểm tinh tế là thao tác đầu tiên sẽ thay đổi cấu hình theo cách tổng thể và các lựa chọn khác nhau về$i$có thể dẫn đến “độ khó” còn lại khác nhau. Một cách tiếp cận ngây thơ sẽ thử tất cả$n$đầu tiên di chuyển và mô phỏng quy trình, nhưng điều đó sẽ nhân chi phí tuyến tính lớn với$n$, điều đó là không thể thực hiện được. 

Các trường hợp cạnh thường phá vỡ lý luận ngây thơ bao gồm các chuỗi thống nhất, trong đó mọi thao tác đều vô hại và các chuỗi có tính chất xen kẽ cao, trong đó mọi cửa sổ đa số đều lật cấu trúc quan trọng. Một tình huống phức tạp khác là khi nhiều thao tác đầu tiên dẫn đến các cấu hình kết quả giống hệt nhau, do đó việc tính các giá trị hợp lệ khác nhau$i$đòi hỏi sự chăm sóc. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực rất đơn giản: đối với mỗi vị trí bắt đầu có thể$i$, áp dụng thao tác đầu tiên trên cửa sổ đó, sau đó áp dụng lặp lại các thao tác tối ưu cho đến khi chuỗi trở nên đồng nhất và đếm xem cần bao nhiêu bước. Ngay cả khi chúng ta áp dụng một chiến lược tham lam thông minh sau nước đi đầu tiên, chúng ta vẫn cần phải tính toán lại tác động của nước đi đầu tiên đó đối với tất cả mọi người.$n$sự lựa chọn. 

Nút thắt cốt lõi là mỗi bước đi đầu tiên sẽ sửa đổi$k$các vị trí liên tiếp trên một chu kỳ và làm điều này cho tất cả$n$bắt đầu đã có chi phí$O(nk)$xử lý trước trong trường hợp xấu nhất. Từ$k$có thể lớn như$n$, điều này suy biến thành$O(n^2)$cho mỗi trường hợp thử nghiệm, vượt xa giới hạn. 

Quan sát quan trọng là hoạt động đầu tiên chỉ quan trọng thông qua cách nó thay đổi cấu trúc đa số cục bộ và quá trình còn lại chỉ phụ thuộc vào chỗ còn tồn tại “sự không nhất quán”, chứ không phụ thuộc vào lịch sử chính xác. Bởi vì mọi thao tác đều thay thế một độ dài-$k$cửa sổ bằng một khối không đổi, quá trình tiến hóa luôn hướng tới việc loại bỏ các ranh giới xen kẽ và

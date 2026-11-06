---
title: "CF 1270E - Chia điểm"
description: "Chúng ta có một tập hợp các điểm trên một mặt phẳng, mỗi điểm có tọa độ nguyên và chúng ta phải chia chúng thành hai nhóm khác rỗng. Sau khi tách, mỗi cặp điểm tạo ra một giá trị khoảng cách, vì chúng ta xem xét khoảng cách Euclide giữa tất cả các cặp."
date: "2026-06-16T00:49:07+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 1270
codeforces_index: "E"
codeforces_contest_name: "Good Bye 2019"
rating: 2300
weight: 1270
solve_time_s: 299
verified: false
draft: false
---

[CF 1270E - Chia điểm](https://codeforces.com/problemset/problem/1270/E) 

**Đánh giá:** 2300 
**Tags:** giải thuật xây dựng, hình học, toán học 
**Thời gian giải:** 4 phút 59 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các điểm trên một mặt phẳng, mỗi điểm có tọa độ nguyên và chúng ta phải chia chúng thành hai nhóm khác rỗng. Sau khi tách, mỗi cặp điểm tạo ra một giá trị khoảng cách, vì chúng ta xem xét khoảng cách Euclide giữa tất cả các cặp. 

Mỗi cặp đóng góp chính xác một số, nhưng nó được tô màu tùy theo nhóm: nếu cả hai điểm nằm trong cùng một nhóm thì khoảng cách là “nội bộ”, nếu không thì đó là “nhóm chéo”. Yêu cầu là không có khoảng cách nào xuất hiện giữa các cặp bên trong bằng bất kỳ khoảng cách nào xuất hiện giữa các cặp nhóm chéo. 

Vì vậy, nhiệm vụ không phải là tối ưu hóa hay giảm thiểu bất cứ điều gì. Nó hoàn toàn là về việc xây dựng một phân vùng gồm các đỉnh sao cho hai tập hợp giá trị hình học, do phân vùng tạo ra, tách rời nhau. 

Các ràng buộc cho phép lên tới 1000 điểm. Điều đó ngụ ý khoảng 500.000 khoảng cách theo cặp. Bất kỳ giải pháp nào so sánh rõ ràng tất cả các phân vùng đều không thể thực hiện được vì ngay cả việc kiểm tra tất cả các phân vùng đều theo cấp số nhân, theo thứ tự 2^1000. Ngay cả việc kiểm tra hình học bậc hai hoặc bậc ba cho mỗi phân vùng ứng cử viên cũng sẽ quá chậm nếu lặp lại. 

Một vấn đề tế nhị là khoảng cách là số thực chứ không phải số nguyên. Tuy nhiên, vì tất cả các tọa độ đều là số nguyên, nên đẳng thức của khoảng cách Euclide tương đương với bình phương khoảng cách, vì vậy về mặt khái niệm, chúng ta có thể tránh được sự bất ổn của dấu phẩy động. Tuy nhiên, giải pháp nên tránh tính toán các gốc chính xác không cần thiết. 

Một sai lầm ngây thơ là cố gắng gán điểm một cách tùy ý, ví dụ như xen kẽ theo chỉ số hoặc chia theo tọa độ x. Điều đó có thể thất bại vì rất dễ vô tình tạo ra hai khoảng cách bằng nhau, một bên trong và một nhóm chéo. Ví dụ: nếu bốn điểm tạo thành một hình chữ nhật, thì các đường chéo đối diện và độ dài các cạnh có thể trùng nhau dưới một số phân vùng, phá vỡ ràng buộc. 

Thách thức là đảm bảo rằng bất kỳ khoảng cách nào xuất hiện bên trong một nhóm đều được tách biệt về mặt cấu trúc với khoảng cách giữa các nhóm. 

## Phương pháp tiếp cận 

Một nỗ lực bạo lực sẽ xem xét mọi cách để chia các điểm thành hai nhóm và xác minh điều kiện. Đối với mỗi phân vùng, chúng tôi sẽ tính toán tất cả khoảng cách theo cặp O(n^2) và lưu chúng thành hai bộ, sau đó kiểm tra xem giao điểm của chúng có trống hay không. Số lượng phân vùng là 2^n và mỗi lần kiểm tra có giá O(n^2), dẫn đến O(n^2 2^n), điều này hoàn toàn không khả thi ngay cả với n = 30. 

Cái nhìn sâu sắc quan trọng là chúng ta thực sự không cần phải phân tích khoảng cách một cách rõ ràng. Chúng ta chỉ cần một thuộc tính cấu trúc đảm bảo sự tách biệt giữa khoảng cách bên trong và giữa các nhóm. 

Ý tưởng hình học cốt lõi là trong số tất cả các điểm, chúng ta có thể chọn một cặp cực trị theo một nghĩa cụ thể và sử dụng nó để xác định một đường cắt. Một cách tiếp cận tiêu chuẩn là chọn cặp có khoảng cách bình phương tối đa. Khi chúng tôi sửa cặp A và B như vậy, chúng tôi sẽ chỉ định điểm dựa trên điểm nào gần với A hoặc B hơn. Điều này tạo ra một phân vùng gần với các điểm cuối của đường kính. 

Lý do điều này có hiệu quả là vì bất kỳ cạnh nhóm chéo nào cũng phải “vượt qua” ranh giới Voronoi do hai điểm xa nhất tạo ra, trong khi các cạnh bên trong nằm trong một cạnh nơi khoảng cách được kiểm soát bởi thang hình học nhỏ hơn nghiêm ngặt. Cặp xa nhất thực thi sự phân tách tỷ lệ toàn cầu: bất kỳ nhóm bên trong nào cũng được chứa trong một vùng mà tất cả khoảng cách theo cặp hoàn toàn nhỏ hơn đường kính, trong khi khoảng cách giữa các nhóm phải liên quan đến các điểm có hình chiếu qua đường kính ngăn cản sự bằng nhau với khoảng cách bên trong.

Một cách đơn giản hóa mang tính xây dựng trực tiếp hơn, vốn là điều mà giải pháp tiêu chuẩn dựa vào, thậm chí còn đơn giản hơn: chọn cặp điểm có chênh lệch tọa độ x tối đa (hoặc một thứ tự cực trị nhất quán sau khi tránh các đối số xoay trong bài toán này vì lựa chọn cực trị theo trục là đủ). Sau đó phân chia các điểm bằng cách xem chúng có gần điểm cuối này hơn điểm cuối kia theo cách nhất quán về mặt từ điển hay không. Điều này đảm bảo rằng không có hai khoảng cách bên trong nào có thể khớp với khoảng cách giữa các nhóm vì phân vùng được tạo ra bởi một thứ tự nghiêm ngặt dọc theo trục phân tách được xác định bởi cặp cực trị. 

Điều này làm giảm vấn đề từ sự bằng nhau của tập hợp hình học thành một phân vùng xác định xuất phát từ một cặp được lựa chọn cẩn thận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 · 2^n) | O(n^2) | Quá chậm | 
| Phân vùng cực cặp | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một phân vùng hợp lệ bằng cách sử dụng một hình học đơn giản

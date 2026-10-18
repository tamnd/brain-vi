---
title: "CF 105584G - Hai Bộ Thẻ"
description: "Chúng tôi được cung cấp một số bộ dữ liệu độc lập. Mỗi tập dữ liệu mô tả một thiết lập trò chơi với một số người tham gia và mỗi người tham gia báo cáo một số nguyên duy nhất là tổng của hai giá trị ẩn."
date: "2026-06-27T00:48:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105584
codeforces_index: "G"
codeforces_contest_name: "The 2024 ICPC Asia Japan Online First-Round Contest"
rating: 0
weight: 105584
solve_time_s: 27
verified: false
draft: false
---

[CF 105584G - Hai bộ thẻ](https://codeforces.com/problemset/problem/105584/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 27s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số bộ dữ liệu độc lập. Mỗi tập dữ liệu mô tả một thiết lập trò chơi với một số người tham gia và mỗi người tham gia báo cáo một số nguyên duy nhất là tổng của hai giá trị ẩn. 

Phía sau là hai bộ bài màu đỏ và xanh, mỗi bộ chứa chính xác một số nguyên cho mỗi người tham gia. Cả hai bộ bài đều chứa nhiều tập hợp số nguyên giống nhau, nhưng không xác định được cặp thẻ đỏ và xanh. Mỗi người tham gia nhận được một thẻ đỏ và một thẻ xanh và họ báo cáo tổng của hai giá trị mà họ nhận được. 

Nhiệm vụ là xác định xem có tồn tại bất kỳ phép gán số nguyên nào cho thẻ đỏ và xanh sao cho tất cả các tổng được báo cáo đều phù hợp với một số cặp của hai tập hợp giống hệt nhau hay không. Nếu có thể, chúng ta phải xây dựng một phép gán các cặp như vậy. Nếu không, chúng tôi báo cáo là không thể. 

Cấu trúc ẩn chính là chúng ta chia nhiều tập hợp thành hai bản sao giống hệt nhau, sau đó ghép các phần tử theo vị trí sau khi hoán vị và chỉ quan sát tổng theo cặp. 

Ràng buộc nhỏ trên mỗi tập dữ liệu, tối đa khoảng 70 người tham gia. Điều này ngay lập tức loại trừ việc liệt kê theo cấp số nhân đối với tất cả các hoán vị của phép gán, vì đó sẽ là thứ tự của 70 khả năng giai thừa, vượt xa mọi tính toán khả thi. Ngay cả các cấu trúc bạo lực O(n^4) trên các cặp cũng sẽ quá lớn nếu được lặp lại trên nhiều tập dữ liệu. 

Điểm tinh tế chính là chúng ta không được tự do gán các cặp tổng khớp nhau một cách độc lập. Giá trị tổng thể giống nhau phải xuất hiện chính xác hai lần, một lần trong mỗi bộ bài, kết hợp tất cả các lựa chọn trên toàn cầu. 

Một trường hợp thất bại phổ biến đối với việc ghép đôi tham lam ngây thơ là khi việc so khớp tổng cục bộ hoạt động nhưng tính nhất quán của nhiều tập hợp toàn cầu không thành công. 

Ví dụ: giả sử chúng ta tham lam chọn một giá trị cho người tham gia đầu tiên và cố gắng ép phần bù của nó vào bộ bài thứ hai một cách độc lập. Sau này chúng tôi có thể phát hiện ra rằng chúng tôi đã hết bản sao có giá trị cần thiết. Điều này xảy ra trong những trường hợp như: 

đầu vào: 

n = 3 

s = [0, 0, 0] 

Một cách tiếp cận đơn giản có thể gán (0,0) cho mỗi người tham gia, nhưng điều này giả định rằng chúng ta có thể sử dụng lại cùng một bộ nhiều tập hợp một cách tự do mà không cần theo dõi tính nhất quán toàn cục. Điều đó xảy ra ở đây, nhưng trong những trường hợp có cấu trúc chặt chẽ hơn như: 

đầu vào: 

n = 3 

s = [1, 1, 2] 

một nỗ lực tham lam có thể gán (0,1), (0,1), (1,1) phá vỡ sự bình đẳng của nhiều tập hợp. 

Vì vậy, khó khăn cốt lõi là tính nhất quán toàn cầu của hai bộ đa giống hệt nhau dưới các ràng buộc ghép nối. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là gán giá trị cho cả hai bộ bài một cách rõ ràng. Vì mỗi người tham gia đóng góp một cặp (a[i], b[i]) và cả hai tập hợp phải khớp nhau, nên chúng tôi có thể thử tạo tất cả các tập hợp có kích thước n có thể có và sau đó kiểm tra xem có tồn tại một cặp tạo ra tổng nhất định hay không. Điều này bao gồm việc chọn n số nguyên, nhân đôi chúng và sau đó ghép chúng thành từng cặp phù hợp với tổng. 

Số lượng nhiều tập hợp số nguyên có thể có trong phạm vi cho phép là rất lớn. Ngay cả việc giới hạn các giá trị trong phạm vi giới hạn xuất phát từ tổng cũng không giúp ích gì, vì các giá trị có thể âm và lên tới 10^9. Điều này làm cho việc liệt kê là không thể. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì nghĩ đến việc xây dựng hai bộ nhiều tập hợp rồi ghép nối chúng, chúng tôi trực tiếp xây dựng các cặp (a[i], b[i]) trong khi đảm bảo rằng

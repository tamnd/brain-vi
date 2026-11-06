---
title: "CF 1297F - Người Yêu Phim"
description: "Chúng tôi được cung cấp một số bộ phim, mỗi bộ phim có một khoảng thời gian để có thể xem trong rạp chiếu phim. Nếu một bộ phim được xem bên trong cửa sổ của nó, nó được coi là “đúng giờ”."
date: "2026-06-16T05:02:32+07:00"
tags: ["codeforces", "competitive-programming", "*special", "data-structures", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1297
codeforces_index: "F"
codeforces_contest_name: "Kotlin Heroes: Episode 3"
rating: 0
weight: 1297
solve_time_s: 138
verified: false
draft: false
---

[CF 1297F - Người hâm mộ phim](https://codeforces.com/problemset/problem/1297/F) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt, cấu trúc dữ liệu, tham lam, triển khai, sắp xếp 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số bộ phim, mỗi bộ phim có một khoảng thời gian để có thể xem trong rạp chiếu phim. Nếu một bộ phim được xem bên trong cửa sổ của nó, nó được coi là “đúng giờ”. Nếu nó được theo dõi sau khi thời hạn của nó kết thúc, nó sẽ trở thành một chiếc đồng hồ trực tuyến và phải chịu một hình phạt tương đương với việc đến muộn. 

Mỗi ngày Polycarp có thể xem nhiều nhất$m$phim. Nhiệm vụ là ấn định mỗi bộ phim vào một ngày sao cho không có ngày nào vượt quá khả năng này và mỗi bộ phim có đúng một ngày xem. Ngày được giao có thể ở bất kỳ thời gian nào, nhưng quan sát bên trong$[a_i, b_i]$không bị phạt trong khi theo dõi$b_i$thêm chi phí trễ là$t_i - b_i$. 

Nếu có thể lên lịch cho tất cả các bộ phim trong cửa sổ của chúng, câu trả lời sẽ phản ánh điều đó. Nếu không, chúng tôi vẫn phải tạo ra một lịch trình đầy đủ, nhưng trong số tất cả các lịch trình hợp lệ, chúng tôi muốn giảm thiểu độ trễ tồi tệ nhất trên tất cả các bộ phim buộc phải xem sau ngày kết thúc của chúng. 

Các ràng buộc buộc chúng tôi phải tránh xa mọi mô phỏng trong những ngày theo lịch thực tế. Số ngày có thể lên tới$10^9$, vì vậy việc lặp đi lặp lại hàng ngày là không thể. Điều quan trọng chỉ là thứ tự thời hạn và lượt đến chứ không phải giá trị tuyệt đối. 

Một cách tiếp cận đơn giản sẽ cố gắng mô phỏng mỗi ngày và gán tối đa$m$xem phim một cách tham lam. Điều này ngay lập tức bị phá vỡ khi khoảng cách giữa những ngày quan trọng quá lớn. Ví dụ: nếu một bộ phim bắt đầu vào ngày$1$và một cái khác vào ngày$10^9$, việc mô phỏng tất cả các ngày trung gian là không thể. 

Vấn đề tế nhị thứ hai xuất hiện khi nhiều bộ phim có cùng thời hạn chặt chẽ. Nếu chúng ta không sắp xếp thứ tự ưu tiên một cách chính xác, chúng ta có thể lấp đầy công suất sớm bằng những bộ phim có thời hạn lỏng lẻo và sau đó không thực hiện được những bộ phim khẩn cấp. Điều này dẫn đến tính không khả thi tiềm ẩn ngay cả khi có một lịch trình hợp lệ. 

## Phương pháp tiếp cận 

Một chiến lược vũ phu sẽ mô phỏng từng ngày. Mỗi ngày, chúng tôi thu thập tất cả các phim đã bắt đầu nhưng chưa được phân công, sau đó phân công cho$m$của họ. Chúng tôi sẽ chọn tùy ý hoặc có thể theo thời hạn sớm nhất. Điều này đúng về mặt tinh thần vì nó tôn trọng các ràng buộc về dung lượng và cửa sổ. 

Điểm thất bại là sự phức tạp. Số ngày lên tới (

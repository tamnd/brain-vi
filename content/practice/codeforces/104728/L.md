---
title: "CF 104728L - Ngõ Azur"
description: "Chúng tôi nhận được trạng thái cuối cùng của một chuỗi loot box sau vài ngày hoạt động. Mỗi ngày, người ta thu được một số bộ hộp, sau đó được sắp xếp nội bộ theo thứ tự độ hiếm không tăng dần và thêm vào chuỗi hiện có."
date: "2026-06-29T03:26:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104728
codeforces_index: "L"
codeforces_contest_name: "Huazhong University of Science of Technology Freshmen Cup 2023"
rating: 0
weight: 104728
solve_time_s: 54
verified: false
draft: false
---

[CF 104728L - Ngõ Azur](https://codeforces.com/problemset/problem/104728/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi nhận được trạng thái cuối cùng của một chuỗi loot box sau vài ngày hoạt động. Mỗi ngày, người ta thu được một số bộ hộp, sau đó được sắp xếp nội bộ theo thứ tự độ hiếm không tăng dần và thêm vào chuỗi hiện có. Trong nhiều ngày, điều này tạo ra một chuỗi dài nhưng ranh giới giữa các ngày sẽ bị mất. 

Thuộc tính cấu trúc quan trọng là mỗi ngày đóng góp một khối liền kề có giá trị không bao giờ tăng từ trái sang phải. Tuy nhiên, giữa hai ngày liên tiếp, sự gia tăng có thể xuất hiện do một ngày mới lại bắt đầu với hộp có độ hiếm cao. 

Chúng ta được đưa cho mảng cuối cùng và được yêu cầu suy luận về tất cả các cách có thể để chia nó thành đúng n ngày, với mọi n từ 1 đến m. Đối với n cố định, chúng tôi xem xét tất cả các phân đoạn hợp lệ tuân theo quy tắc rằng mỗi phân đoạn không tăng và chúng tôi muốn tổng chi phí tối thiểu có thể. Chi phí của một ngày phụ thuộc vào số lượng hộp tồn tại trong hệ thống vào cuối ngày đó, có nghĩa là các hộp trước đó được tính nhiều lần trong những ngày tiếp theo. 

Vì vậy, vấn đề không chỉ là tìm ra một phân khúc hợp lý mà còn là việc chọn một phân khúc có thể giảm thiểu sự đóng góp có trọng số của các vị trí phân khúc. 

Các ràng buộc đẩy chúng ta tới giải pháp O(m log m) hoặc O(m) vì m có thể lên tới 10^6. Bất kỳ giải pháp nào cố gắng liệt kê các phân đoạn hoặc chạy lập trình động trên tất cả các phân vùng đều không thể thực hiện được ngay lập tức. 

Một số trường hợp đặc biệt tiết lộ cấu trúc mà chúng ta phải tôn trọng. 

Ví dụ: nếu mảng có mức tăng nghiêm ngặt ở bất cứ đâu`[1, 3, 2]`, thì các vị trí trong một ngày không thể vượt qua mức tăng đó. Vì vậy, bất kỳ phân đoạn hợp lệ nào cũng phải cắt ở mọi vị trí mà`a[i] > a[i-1]`. Nếu chúng ta cố gắng đặt n nhỏ hơn số lượng phân đoạn bắt buộc thì không có cấu trúc hợp lệ nào tồn tại. 

Một trường hợp tinh vi khác là khi mảng đã hoàn toàn không tăng, chẳng hạn như`[5, 4, 3, 2]`. Khi đó không có sự cắt giảm bắt buộc nào và chúng ta hoàn toàn có quyền tự do phân chia thành bất kỳ số phân đoạn nào lên đến m. Tính linh hoạt này là điều cho phép tối ưu hóa: các lựa chọn cắt giảm khác nhau sẽ thay đổi chi phí. 

Quan sát quan trọng thứ ba là việc chia tách thêm một phân đoạn không tăng hợp lệ luôn bảo toàn tính hợp lệ. Đây là tôi

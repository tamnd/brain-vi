---
title: "CF 103061B - Riff lười biếng"
description: "Đặt $ct cdots c1$ biểu thị cách biểu diễn từ điển của một tổ hợp $(s,t)$-theo thứ tự giảm dần như trong (3), và đặt $bs cdots b1$ biểu thị cách biểu diễn kép được cho bởi vị trí của các số 0 như trong (5)."
date: "2026-07-04T01:08:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103061
codeforces_index: "B"
codeforces_contest_name: "Harbin Engineering University Collegiate Programming Contest 2021"
rating: 0
weight: 103061
solve_time_s: 158
verified: false
draft: false
---

[CF 103061B - Lazing Riff](https://codeforces.com/problemset/problem/103061/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 38 giây 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$c_t \cdots c_1$biểu thị sự biểu diễn từ điển của một$(s,t)$-tổ hợp theo thứ tự giảm dần như trong (3) và đặt$b_s \cdots b_1$biểu thị cách biểu diễn kép được cho bởi vị trí của các số 0 như trong (5). Thứ tự chéo đề cập đến việc so sánh đồng thời hai biểu diễn này theo các hướng ngược nhau:$c_t \cdots c_1$được sắp xếp theo thứ tự tăng dần về mặt từ điển, trong khi$b_s \cdots b_1$được sắp xếp giảm dần theo từ điển. 

Bổ đề S khẳng định rằng sự liên kết đối lập này làm cho các kết hợp liên tiếp hoạt động đơn điệu đối với cả hai biểu diễn, do đó một thay đổi cục bộ duy nhất trong một biểu diễn tương ứng với một thay đổi cục bộ được kiểm soát trong biểu diễn kia. Việc hoàn thành chứng minh của nó nhằm chỉ ra rằng sự ghép nối này giúp loại bỏ sự mơ hồ trong lựa chọn kế tiếp và đảm bảo rằng bước cập nhật trong quá trình tạo từ điển chỉ ảnh hưởng đến một hậu tố tối thiểu của cách biểu diễn. 

Cho phép$c_t \cdots c_1$Và$c'_t \cdots c'_1$là sự kết hợp liên tiếp theo thứ tự từ điển. Theo định nghĩa về thứ tự từ điển theo trình tự giảm dần, tồn tại chỉ số lớn nhất$j$như vậy$c_j < c'_j$, và cho tất cả$i > j$một người có$c_i = c'_i$. Việc xây dựng phần tử kế tiếp trong Thuật toán L cho thấy rằng$c_j$tăng chính xác$1$trong khi tất cả$c_{j-1}, \ldots, c_1$được đặt lại về giá trị khả thi tối thiểu phù hợp với (3). Sự thay đổi này bảo toàn tất cả các bất bình đẳng xác định một$(s,t)$-kết hợp và chỉ ảnh hưởng đến các mục ở chỉ số nhiều nhất$j$. 

Chuyển sang biểu diễn kép, mỗi$c_k$xác định vị trí duy nhất$b_\ell$của số 0 trong chuỗi nhị phân và tăng dần$c_j$qua$1$dịch chuyển chính xác một phần tử đã chọn qua phần tử chưa được chọn trước đó. Trong biểu diễn nhị phân, điều này tương ứng với việc trao đổi một mẫu cục bộ$01$với$10$tại một ranh giới được xác định duy nhất, vì$c_j$ghi lại vị trí của một$1$liên quan đến thứ tự của các chỉ số trong (4). Tất cả các chỉ số lớn hơn$c_j$không thay đổi, trong khi tất cả các chỉ số nhỏ hơn$c_j$được khởi tạo lại theo cấu hình tối thiểu theo yêu cầu của (3). Do đó trình tự kép$b_s \cdots b_1$thay đổi bằng một điều chỉnh đơn điệu trong hậu tố được xác định bởi cùng vị trí trục đó. 

Điều này thiết lập rằng theo thứ tự chéo, trình tự nguyên thủy$c_t \cdots c_1$tăng về mặt từ điển trong khi chuỗi kép$b_s \cdots b_1$giảm theo từ điển và cả hai thay đổi được giới hạn ở một hậu tố liền kề duy nhất được xác định bởi cùng một chỉ mục trục$j$. Do đó, mỗi bước kế tiếp tương ứng với một phép biến đổi cục bộ được xác định duy nhất không ảnh hưởng đến tọa độ không liên quan. 

Cấu trúc này hoàn thành bước quy nạp được yêu cầu trong Bổ đề S, vì hàm kế thừa được xác định rõ ràng, kết thúc sau khi sử dụng hết tất cả các giá trị có thể chấp nhận được.$j$và duy trì sự song ánh giữa các biểu diễn nguyên thủy và kép trong suốt quá trình tạo. Điều này hoàn thành việc chứng minh. ∎

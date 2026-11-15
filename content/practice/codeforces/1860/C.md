---
title: "CF 1860C - Trò chơi hoán vị"
description: "Chúng ta được cho một hoán vị và một trò chơi được chơi trên các vị trí của nó. Con chip không bắt đầu từ đâu cả. Alice bắt đầu bằng việc chọn bất kỳ vị trí nào và đặt con chip vào đó. Sau đó, người chơi luân phiên di chuyển chip."
date: "2026-06-09T00:22:08+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "games", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1860
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 153 (Rated for Div. 2)"
rating: 1400
weight: 1860
solve_time_s: 116
verified: false
draft: false
---

[CF 1860C - Trò chơi hoán vị](https://codeforces.com/problemset/problem/1860/C) 

**Đánh giá:** 1400 
**Tas:** cấu trúc dữ liệu, dp, trò chơi, tham lam 
**Thời gian giải:** 1 phút 56s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị và một trò chơi được chơi trên các vị trí của nó. 

Con chip không bắt đầu từ đâu cả. Alice bắt đầu bằng việc chọn bất kỳ vị trí nào và đặt con chip vào đó. Sau đó, người chơi luân phiên di chuyển chip. Từ vị trí`i`, con chip chỉ có thể di chuyển đến một vị trí`j`như vậy`j < i`Và`p[j] < p[i]`. Nói cách khác, mọi bước di chuyển đều đi sang trái và cũng đi đến một giá trị nhỏ hơn. 

Phần bất thường của trò chơi là điều kiện chiến thắng. Nếu một người chơi không thể di chuyển, người chơi đó sẽ thắng ngay lập tức. Điều này trái ngược với quy tắc chơi bình thường tiêu chuẩn. 

Đối với mỗi vị trí của hoán vị, chúng ta phải xác định xem việc chọn vị trí đó làm vị trí ban đầu có đảm bảo chiến thắng cho Alice bất kể quyết định của Bob hay không. Những vị trí như vậy được gọi là may mắn. Câu trả lời chính là số vị trí may mắn. 

Kích thước hoán vị lên tới`3 · 10^5`trên tất cả các trường hợp thử nghiệm. Bất kỳ giải pháp nào phân tích biểu đồ trò chơi riêng biệt với mọi vị trí bắt đầu đều quá chậm. Vì tổng kích thước đầu vào chỉ có vài trăm nghìn phần tử nên chúng ta cần một cái gì đó gần với kích thước tuyến tính hoặc`O(n log n)`. 

Một điểm tinh tế là điều kiện thắng đảo ngược. Trong các trò chơi công bằng thông thường, các vị trí cuối cùng sẽ bị mất do người chơi hiện tại không thể di chuyển. Ở đây các vị trí cuối cùng sẽ thắng vì không thể di chuyển ngay lập tức kết thúc trò chơi có lợi cho bạn. Nốt Rê

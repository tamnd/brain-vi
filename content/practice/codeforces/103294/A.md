---
title: "CF 103294A - Hành trình về nhà"
description: "Sự kết hợp $(s,t)$-ở dạng kép là một chuỗi giảm nghiêm ngặt $bs b{s-1} cdots b1 ge 0,$ trong đó ${b1,dots,bs}$ chính xác là vị trí của $0$’s trong chuỗi nhị phân có độ dài $n=s+t$. Thứ tự từ điển trên các bộ $(bs,dots,b1)$ so sánh các mục từ trái sang phải."
date: "2026-07-03T14:28:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103294
codeforces_index: "A"
codeforces_contest_name: "UTPC Contest 09-17-21 Div. 2 (Beginner)"
rating: 0
weight: 103294
solve_time_s: 58
verified: false
draft: false
---

[CF 103294A - Hành trình về nhà](https://codeforces.com/problemset/problem/103294/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Một$(s,t)$- Tổ hợp ở dạng kép là dãy giảm dần$b_s > b_{s-1} > \cdots > b_1 \ge 0,$Ở đâu${b_1,\dots,b_s}$chính xác là vị trí của$0$’ ở dạng chuỗi nhị phân có độ dài$n=s+t$. 

Thứ tự từ điển trên các bộ dữ liệu$(b_s,\dots,b_1)$so sánh các mục từ trái sang phải. Do đó, việc quét từ điển giảm dần tạo ra bộ dữ liệu đầu tiên có mục nhập đầu tiên tối đa và nói chung thay đổi tọa độ sớm nhất có thể; tọa độ sau này luôn được giữ ở mức lớn nhất mà các ràng buộc cho phép. 

Bộ dữ liệu hợp lệ lớn nhất là$b_s=n-1,\quad b_{s-1}=n-2,\quad \dots,\quad b_1=n-s,$vì đây là dãy giảm duy nhất với số đầu vào tối đa có thể. 

Để di chuyển tới bộ tiếp theo theo thứ tự từ điển giảm dần, mục tiêu là giảm$b_s$càng lâu càng tốt. Điều này khả thi chính xác trong khi$b_s > b_{s-1}+1$. Khi$b_s = b_{s-1}+1$, không giảm thêm nữa$b_s$duy trì sự nghiêm ngặt, vì vậy$b_s$phải được đặt lại về giá trị tối đa tương thích với tiền tố cố định$(b_{s-1},\dots)$sau đó$b_{s-1}$được thay đổi. Logic tương tự được áp dụng đệ quy cho các vị trí trước đó. 

Điều này dẫn đến việc tìm kiếm từ phải sang trái cho chỉ mục đầu tiên có thể giảm đi, cùng với việc khôi phục các giá trị tối đa ở bên phải của nó. 

Giới thiệu lính canh$b_0=-1,\qquad b_{s+1}=n.$### Thuật toán D (Kết hợp kép theo thứ tự từ điển giảm dần) 

D1. [Khởi tạo.] Đặt$b_j \leftarrow n-j$vì$1 \le j \le s$. 

D2. [Chuyến thăm.] Chuyến thăm$(b_s,\dots,b_1)$. 

D3. [Tìm vị trí.] Đặt$j \leftarrow s$. Trong khi$b_j = b_{j-1}+1$, bộ$b_j \leftarrow n-j$Và$j \leftarrow j-1$. 

D4. [Xong?] Nếu$j=0$, chấm dứt. 

D5. [Giảm và lấp đầy.] Đặt$b_j \leftarrow b_j - 1$. Vì$k$từ$1$ĐẾN$j-1$, bộ$b_k \leftarrow b_{k+1}-1.$Trở lại D2. 

Việc khởi tạo tạo ra bộ dữ liệu hợp lệ lớn nhất về mặt từ điển, vì mỗi bộ$b_j$được chọn càng lớn càng tốt trong khi vẫn duy trì mức giảm nghiêm ngặt. 

Trong bước D3, mọi chỉ mục$j$thỏa mãn$b_j=b_{j-1}+1$chính xác là một vị trí mà$b_j$ở giá trị tối thiểu cho phép với hậu tố hiện tại; thay thế$b_j \leftarrow n-j$khôi phục giá trị tối đa phù hợp với mức giảm trong tương lai ở các chỉ số trước đó. Quá trình quét dừng ở vị trí đầu tiên$j$nơi mà việc giảm là khả thi, có nghĩa là$b_j > b_{j-1}+1$. 

Bước D5 thực hiện lựa chọn nhỏ hơn tiếp theo theo từ điển tại vị trí$j$bằng cách giảm$b_j$qua$1$, sau đó buộc hậu tố trở thành mức độ hoàn thành giảm dần tối đa tương thích với tiền tố mới. Sự tái diễn$b_k=b_{k+1}-1$xây dựng lại một cách duy nhất chuỗi giảm nghiêm ngặt tối đa ở bên phải vị trí$j$. 

Mỗi quá trình chuyển đổi sẽ thay đổi tọa độ sớm nhất có thể về mặt từ điển, trong khi vẫn duy trì mức hoàn thành tối đa ở bên phải, điều này đảm bảo rằng không có bộ dữ liệu nào bị bỏ qua và không có bộ dữ liệu nào được lặp lại. 

Điều này hoàn thành việc chứng minh. ∎

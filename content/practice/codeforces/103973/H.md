---
title: "CF 103973H - Chuỗi con"
description: "Đặt $G=(V,E)$ là một đồ thị. Một tập $Ksubseteq V$ là một hạt nhân của $G$ khi nó độc lập và mọi đỉnh $vin Vsetminus K$ đều có một đỉnh lân cận trong $K$. Tập $Dsubseteq V$ là tập trội khi mọi đỉnh $vin Vsetminus D$ đều có một đỉnh lân cận trong $D$."
date: "2026-07-02T06:21:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103973
codeforces_index: "H"
codeforces_contest_name: "2022 Huazhong University of Science and Technology Freshmen Cup"
rating: 0
weight: 103973
solve_time_s: 71
verified: false
draft: false
---

[CF 103973H - Chuỗi con](https://codeforces.com/problemset/problem/103973/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$G=(V,E)$hãy là một đồ thị. một bộ$K\subseteq V$là một hạt nhân của$G$khi nó độc lập và mọi đỉnh$v\in V\setminus K$có một người hàng xóm ở$K$. một bộ$D\subseteq V$là tập trội khi mọi đỉnh$v\in V\setminus D$có một người hàng xóm ở$D$. Một tập hợp thống trị$D$là tối thiểu khi không có tập hợp con thích hợp của$D$là một tập thống trị. 

### (một) 

hãy để$K$là một hạt nhân của$G$. Với mọi đỉnh$v\in V\setminus K$, định nghĩa của kernel cho một đỉnh$u\in K$như vậy${u,v}\in E$. Do đó mọi đỉnh bên ngoài$K$liền kề với một đỉnh trong$K$, Vì thế$K$là một tập thống trị. 

Để thể hiện sự tối thiểu, hãy lấy bất kỳ đỉnh nào$u\in K$và xem xét$K\setminus{u}$. Từ$K$độc lập, không có hai đỉnh trong$K$liền kề nhau nên$u$không có hàng xóm ở$K$. Đặc biệt, không có đỉnh nào trong$K\setminus{u}$liền kề với$u$. Vì thế$u$không bị chi phối bởi$K\setminus{u}$. Điều này cho thấy$K\setminus{u}$không phải là tập trội. Vì điều này đúng với mọi$u\in K$, bộ$K$là sự thống trị tối thiểu. 

Điều này hoàn thành việc chứng minh. ∎ 

### (b) 

Số lượng tập hợp thống trị tối thiểu phụ thuộc vào cấu trúc cụ thể của biểu đồ Hoa Kỳ (18). Định nghĩa về hạt nhân và tập thống trị làm giảm vấn đề liệt kê tất cả các tập hợp thống trị độc lập của biểu đồ đó. Nếu không có đặc tả kề của đồ thị (18), thì không thể xây dựng được hệ thống tập hợp và không thể thực hiện đánh giá ZDD hoặc BDD để đếm các nghiệm. 

Do đó, giá trị được yêu cầu trong (b) được xác định bằng cách đánh giá ZDD cho các tập hợp đồ thị (18) chiếm ưu thế và trích xuất các phần tử tối thiểu thông qua đại số họ, nhưng kết quả bằng số không thể được rút ra chỉ từ thông tin được cung cấp ở đây. 

### (c) 

Một tập hợp bảy đỉnh chiếm ưu thế trong 36 đỉnh khác yêu cầu thông tin kề cận rõ ràng từ biểu đồ (18). Điều kiện là lân cận đóng của bảy đỉnh được chọn bao phủ ít nhất 36 đỉnh của đồ thị. 

Như với phần (b), việc xây dựng một tập hợp như vậy phụ thuộc vào cấu trúc cạnh chính xác của đồ thị (18). Nếu không có cấu trúc đó thì không thể thực hiện được việc xác minh cũng như tối ưu hóa. 

## Ghi chú 

Phần (a) có tính cấu trúc và đúng cho mọi đồ thị. Phần (b) và (c) là các trường hợp tính toán của các bài toán thống trị trên một đồ thị cố định cụ thể; họ yêu cầu dữ liệu mẫu rõ ràng của biểu đồ (18), không có trong đoạn trích được cung cấp.

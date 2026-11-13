---
title: "CF 1717F - Madoka và Phiên họp đầu tiên"
description: "Chúng ta được yêu cầu thao tác với một mảng b có kích thước n, ban đầu toàn là số 0, bằng cách thực hiện một chuỗi m phép toán được xác định bởi các cặp chỉ số (vi, ui). Với mỗi cặp, chúng ta chọn một trong hai thao tác ngược nhau: hoặc giảm b[vi] đi một và tăng b[ui] một hoặc ngược lại."
date: "2026-06-09T19:48:08+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "flows", "graph-matchings", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1717
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 818 (Div. 2)"
rating: 2500
weight: 1717
solve_time_s: 37
verified: false
draft: false
---

[CF 1717F - Madoka và Phiên đầu tiên](https://codeforces.com/problemset/problem/1717/F) 

**Đánh giá:** 2500 
**Thẻ:** thuật toán xây dựng, luồng, so khớp biểu đồ, đồ thị, triển khai 
**Thời gian giải:** 37s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu thao tác một mảng`b`kích thước`n`, ban đầu tất cả đều là số không, bằng cách thực hiện một loạt`m`các hoạt động được xác định bởi các cặp chỉ số`(v_i, u_i)`. Với mỗi cặp, chúng ta chọn một trong hai thao tác ngược nhau: hoặc giảm`b[v_i]`một và tăng`b[u_i]`bằng một hoặc ngược lại. Mục tiêu là thực hiện các thao tác này để cuối cùng, mỗi chỉ mục`i`với`s[i] = 1`thỏa mãn`b[i] = a[i]`. Nếu như`s[i] = 0`,`a[i]`được đảm bảo bằng 0 nên chúng ta không quan tâm đến giá trị cuối cùng của`b[i]`đối với những chỉ số như vậy. 

Quan sát không rõ ràng đầu tiên là bài toán tương đương với việc gán hướng cho các cạnh trong đồ thị vô hướng, trong đó các đỉnh tương ứng với các chỉ số và mỗi cặp phép toán`(v_i, u_i)`là một cạnh. Số cạnh đi trừ đi cạnh vào (hoặc ngược lại tùy theo lựa chọn) xác định sự khác biệt cuối cùng`b[i]`. Về cơ bản, chúng ta đang được hỏi liệu có tồn tại sự định hướng có hướng của các cạnh sao cho mạng lưới chạy ở mỗi đỉnh với`s[i] = 1`trận đấu`a[i]`. 

Những hạn chế ngụ ý rằng chúng ta cần một giải pháp hiệu quả. Với`n, m <= 10^4`và giới hạn thời gian 2 giây, chúng tôi không thể chấp nhận bất cứ điều gì tệ hơn đại khái`O(n*m)`trong thực tế và`O(n + m)`hoặc`O((n + m) log n)`sẽ là lý tưởng. Một cách tiếp cận ngây thơ thử tất cả`2^m`những lựa chọn có thể ngay lập tức là không thể. 

Các trường hợp đặc biệt bao gồm các tình huống trong đó tổng của các khác biệt mong muốn`a[i]`vì`s[i] = 1`không bằng 0, điều này làm cho bài toán không thể thực hiện được vì mỗi phép toán bảo toàn tổng của tất cả`b[i]`. Một trường hợp tinh tế khác là khi một số đỉnh bị cô lập khỏi các đỉnh quan trọng (`s[i] = 1`), khiến cho không thể thỏa mãn các giá trị mục tiêu của chúng nếu không có cạnh nào kết nối chúng với phần còn lại của biểu đồ. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ cố gắng kết hợp mọi hoạt động cho từng`m`cặp. Điều này đảm bảo tính đúng đắn, nhưng số lượng các khả năng là`2^m`, điều này là không thể thực hiện được đối với`m`lên tới`10^4`. Vấn đề chính là mỗi thao tác ảnh hưởng đến hai đỉnh và chúng ta phải thỏa mãn tổng thể một tập hợp các phương trình tuyến tính biểu thị chênh lệch thực ở các đỉnh với`s[i] = 1`. 

Cách tiếp cận tối ưu dựa vào sự hiểu biết sâu sắc rằng điều này

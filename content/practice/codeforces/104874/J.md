---
title: "CF 104874J - Chỉ là chữ số cuối cùng"
description: "Chúng ta được cung cấp một cấu trúc không tuần hoàn có hướng trên các nút có thứ tự $n$, trong đó các cạnh chỉ đi từ chỉ mục nhỏ hơn đến chỉ mục lớn hơn. Hãy coi nó như một biểu đồ đi xuống: từ mọi vị trí $i$, bạn chỉ có thể di chuyển đến các vị trí có chỉ số cao hơn $j i$ nếu tồn tại một đường nhỏ."
date: "2026-06-28T10:08:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104874
codeforces_index: "J"
codeforces_contest_name: "2019-2020 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104874
solve_time_s: 22
verified: false
draft: false
---

[CF 104874J - Chỉ là chữ số cuối cùng](https://codeforces.com/problemset/problem/104874/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 22s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một cấu trúc tuần hoàn có hướng trên$n$các nút có thứ tự, trong đó các cạnh chỉ đi từ chỉ mục nhỏ hơn đến chỉ mục lớn hơn. Hãy coi nó như một biểu đồ đi xuống: từ mọi vị trí$i$, bạn chỉ có thể di chuyển đến các vị trí có chỉ số cao hơn$j > i$nếu một dấu vết tồn tại. 

Ma trận đầu vào không mô tả trực tiếp các đường dẫn. Thay vào đó, nó mang lại cho mỗi cặp$i < j$, chỉ chữ số cuối cùng của số đường đi có hướng khác nhau từ$i$ĐẾN$j$. Đường dẫn là bất kỳ chuỗi nút nào trong đó mỗi cặp liên tiếp được kết nối bằng một đường nhỏ. Số đường đi thực tế có thể rất lớn nhưng chúng ta chỉ quan sát được nó theo modulo 10. 

Nhiệm vụ là xây dựng lại ma trận kề của đồ thị gốc, nghĩa là ta phải xác định cho từng cặp$i < j$liệu một dấu vết trực tiếp có tồn tại hay không. 

Những ràng buộc cho phép$n \le 500$. Bất kỳ giải pháp nào cố gắng đếm rõ ràng tất cả các đường dẫn giữa tất cả các cặp sẽ yêu cầu xử lý lên tới$O(n^2)$giá trị, mỗi giá trị tùy thuộc vào khả năng$O(n)$chuyển tiếp, đẩy về phía$O(n^3)$hoặc tệ hơn. Điều đó có thể chấp nhận được, nhưng chỉ khi các phép chuyển đổi là số học đơn giản; bất kỳ sự liệt kê theo cấp số nhân của các đường dẫn là không thể. 

Một vấn đề khó nhận thấy là chúng ta chỉ thấy số lượng đường dẫn theo modulo 10, làm mất hầu hết cấu trúc số học. Đặc biệt, các biểu đồ khác nhau có thể tạo ra cùng một ma trận chữ số cuối cùng, do đó việc xây dựng lại phải dựa vào các ràng buộc về cấu trúc thay vì đảo ngược số. 

Trường hợp cạnh khóa là khi có nhiều đường dẫn gián tiếp che dấu hoặc bắt chước các cạnh trực tiếp modulo 10. Ví dụ: nếu có 10 đường dẫn riêng biệt từ$i$ĐẾN$j$thông qua các giá trị trung gian, chữ số cuối cùng là 0, khiến cho thoạt nhìn không thể phân biệt được với “không có đường dẫn”. Điều này làm cho việc “phát hiện cạnh từ các số khác 0” ngây thơ không chính xác. 

Một trường hợp cạnh khác là một cặp nút không có cạnh trực tiếp nhưng nhiều đường dẫn gián tiếp vẫn tạo ra chữ số cuối cùng khác 0. Bất kỳ cách tiếp cận nào giả định “khác 0 hàm ý có lợi thế” sẽ thất bại ngay lập tức. 

## Phương pháp tiếp cận 

Điểm khởi đầu tự nhiên là suy nghĩ về mặt lập trình động trên các đường dẫn. Vì các cạnh chỉ đi về phía trước nên số lượng đường đi$dp[i][j]$có thể được tính như sau:$$dp[i][j] = \sum_{i \to k \to j} dp[i][k] \cdot adj[k][j]$$Nếu đã biết ma trận kề, việc tính toán tất cả số đường đi mod 10 rất đơn giản trong$O(n^3)$. Tuy nhiên, chúng ta đang giải bài toán ngược: chúng ta được cho$dp[i][j] \bmod 10$và phải phục hồi$adj[i][j]$. 

Một ý tưởng mạnh mẽ là quyết định từng cạnh$adj[i][j]$độc lập và kiểm tra tính nhất quán bằng cách tính toán lại tất cả số lượng đường dẫn. Đối với mỗi cặp, chúng ta có thể thử cả hai khả năng và xác nhận ma trận kết quả. Điều này dẫn đến một không gian tìm kiếm theo cấp số nhân có kích thước$2^{O(n^2)}$, điều này ngay lập tức không thể thực hiện được. 

Quan sát quan trọng là vì các cạnh chỉ đi từ chỉ số nhỏ hơn đến chỉ số lớn hơn nên chúng ta có thể xử lý các nút theo thứ tự tăng dần và duy trì tính chính xác của các hàng đã cố định. Khi quyết định các cạnh từ một nút$i$, tất cả đóng góp từ các nút trung gian$k > i$vẫn chưa được sửa, nhưng đóng góp từ các nút trước đó đã được xác định. Điều này tạo ra một hướng phụ thuộc cho phép tái thiết tăng dần. 

Th

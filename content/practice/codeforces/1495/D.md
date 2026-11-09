---
title: "CF 1495D - Cây BFS"
description: "Chúng ta được yêu cầu đếm, với mỗi cặp đỉnh $i, j$ trong một đồ thị liên thông vô hướng, số lượng cây bao trùm đồng thời hoạt động như cây BFS có gốc ở cả $i$ và $j$. Cây BFS có gốc tại $s$ duy trì khoảng cách ngắn nhất từ ​​$s$ đến mọi đỉnh khác."
date: "2026-06-10T22:01:06+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dfs-and-similar", "graphs", "math", "shortest-paths", "trees"]
categories: ["algorithms"]
codeforces_contest: 1495
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 706 (Div. 1)"
rating: 2600
weight: 1495
solve_time_s: 76
verified: false
draft: false
---

[CF 1495D - Cây BFS](https://codeforces.com/problemset/problem/1495/D) 

**Đánh giá:** 2600 
**Tags:** tổ hợp, dfs và tương tự, đồ thị, toán học, đường đi ngắn nhất, cây 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm cho mỗi cặp đỉnh$i, j$trong một đồ thị kết nối vô hướng, số lượng cây bao trùm đồng thời hoạt động như cây BFS có gốc ở cả hai$i$Và$j$. Một cây BFS có gốc tại$s$bảo toàn khoảng cách ngắn nhất từ$s$đến mọi đỉnh khác. Vì vậy, để một cây đủ tiêu chuẩn thành một cặp$i, j$, nó phải bảo toàn khoảng cách ngắn nhất từ ​​cả hai$i$Và$j$. 

Đầu vào là một đồ thị vô hướng với$n$đỉnh và$m$các cạnh và chúng ta phải tính$f(i, j)$modulo$998\,244\,353$cho tất cả$i, j$. Cho rằng$n \le 400$Và$m \le 600$, giải pháp phải tránh$O(n^3 \cdot 2^n)$hoặc các phương pháp liệt kê tổ hợp tương tự vì chúng vượt quá$10^8$hoạt động. Một giải pháp có độ phức tạp gần như$O(n^3 + n \cdot m)$là khả thi. 

Các trường hợp cạnh không rõ ràng bao gồm các đồ thị trong đó một số cặp đỉnh không được định vị duy nhất trên các đường đi ngắn nhất. Ví dụ: 4 chu kỳ$1-2-3-4-1$có nhiều cây BFS cho các đỉnh liền kề. Việc triển khai bất cẩn cho rằng một cây được xác định duy nhất bởi một BFS có thể đếm cây không chính xác. Một sự tinh tế khác nảy sinh khi hai đỉnh ở khoảng cách tối đa với nhau: chỉ những cây cụ thể duy trì khoảng cách với cả hai cùng một lúc. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ liệt kê tất cả các cây bao trùm, kiểm tra tính nhất quán của BFS từ cả hai gốc và đếm chúng. Điều này đúng nhưng không khả thi. Số lượng cây bao trùm tăng theo cấp số nhân với$n$và việc xác minh các điều kiện BFS cho từng cây là$O(n)$. Vì$n=400$, tổng số hoạt động vượt xa giới hạn khả thi. 

Cách tiếp cận tối ưu dựa vào thuộc tính khoảng cách BFS. Trước tiên, chúng tôi tính toán trước khoảng cách ngắn nhất của tất cả các cặp bằng cách sử dụng BFS từ mỗi đỉnh, tạo ra ma trận khoảng cách$d[i][j]$. Để một cây trở thành cây BFS từ cả hai$i$Và$j$, mỗi nút$v$phải nằm trên ít nhất một đường đi ngắn nhất từ$i$ĐẾN$j$hoặc chỉ kết nối với những hàng xóm duy trì khoảng cách ngắn nhất từ ​​cả hai gốc. Nói cách khác, cây BFS phải chọn cho mỗi nút$v$, các cạnh kết nối nó với cha mẹ ở đúng một bước gần hơn với gốc. 

Thuật toán rút gọn về việc kiểm tra, cho một cặp$i, j$, liệu đường đi giữa chúng có duy nhất về mặt khoảng cách và tính các lựa chọn cha mẹ hợp lệ cho mọi đỉnh khác hay không. Mỗi đỉnh$v$bên ngoài đường đi có thể gắn vào hàng xóm$u$như vậy$d[i][u] = d[i][v] - 1$Và$d[j][u] = d[j][v] - 1$. Tích số trên tất cả các đỉnh sẽ cho ra số lượng cây BFS cho cặp. Nếu bất kỳ đỉnh nào không thể thỏa mãn điều kiện này,$f(i, j) = 0$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^{n-2}) | O(n^2) | Quá chậm | 
| Tối ưu | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n, m$và lưu trữ các cạnh trong một kề

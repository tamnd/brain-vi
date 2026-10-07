---
title: "CF 104941F - Giải đấu vui nhộn"
description: "Chúng ta được cung cấp một tập hợp các thí sinh, mỗi thí sinh được mô tả bằng hai số nguyên. Hãy coi mỗi thí sinh đang thực hiện hai “nước đi”: nước đi đầu tiên là $ai$ và nước đi thứ hai là $bi$."
date: "2026-06-28T18:17:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104941
codeforces_index: "F"
codeforces_contest_name: "SLPC 2024 Open Division"
rating: 0
weight: 104941
solve_time_s: 34
verified: false
draft: false
---

[CF 104941F - Giải đấu thú vị](https://codeforces.com/problemset/problem/104941/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các thí sinh, mỗi thí sinh được mô tả bằng hai số nguyên. Hãy coi mỗi thí sinh đang thực hiện hai “nước đi”: nước đi đầu tiên là$a_i$, và bước thứ hai là$b_i$. Nếu chúng ta chọn hai thí sinh khác nhau$i$Và$j$, họ chơi hai trò chơi: trong trò chơi đầu tiên chúng tôi so sánh$a_i$vs$a_j$, và trong trò chơi thứ hai chúng ta so sánh$b_i$vs$b_j$. Điểm của mỗi ván đấu là chênh lệch tuyệt đối của các giá trị đã chọn. Vì vậy trò chơi đầu tiên góp phần$|a_i - a_j|$, đóng góp thứ hai$|b_i - b_j|$. 

Một cặp thí sinh bị coi là xấu nếu hai giá trị này trùng nhau, nghĩa là$$|a_i - a_j| = |b_i - b_j|.$$Nhiệm vụ là đếm xem có bao nhiêu cặp tốt, tức là có bao nhiêu cặp vi phạm sự bình đẳng này. 

Kích thước đầu vào tăng lên$n = 3 \cdot 10^5$, do đó, bất kỳ phép liệt kê bậc hai nào đối với các cặp đều ngay lập tức quá chậm. Lời giải phải gần tuyến tính hoặc$n \log n$, từ$n^2$có nghĩa là về$10^{10}$so sánh trong trường hợp xấu nhất 

Một trường hợp phức tạp là khi nhiều cặp có chung cấu trúc, điều này có thể khiến việc băm các khác biệt một cách ngây thơ bị xung đột hoặc bỏ sót các biến thể dấu hiệu. Ví dụ: nếu tất cả các thí sinh giống hệt nhau thì mọi cặp đều thỏa mãn sự bình đẳng, do đó câu trả lời là 0. Bất kỳ cách tiếp cận nào chỉ nắm bắt được một phần điều kiện sai phân tuyệt đối mà không xử lý tính đối xứng dấu sẽ thất bại trong các trường hợp như:$$(1, 5), (5, 1), (3, 3).$$Một tình huống khó khăn khác là khi sự khác biệt là như nhau nhưng đến từ các cấu hình ký hiệu khác nhau, chẳng hạn như:$$|a_i - a_j| = |b_i - b_j| \iff a_i - a_j = b_i - b_j \ \text{or}\ a_i - a_j = -(b_i - b_j).$$Thiếu một trong những trường hợp này sẽ dẫn đến việc đếm quá nhiều cặp tốt. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp rất đơn giản: lặp lại tất cả các cặp$i < j$và kiểm tra xem$|a_i - a_j| = |b_i - b_j|$. Điều này đúng nhưng chi phí$O(n^2)$so sánh. Với$n = 3 \cdot 10^5$, điều này vượt xa khả thi. 

Quan sát chính là loại bỏ điều kiện giá trị tuyệt đối bằng cách chia nó thành hai đẳng thức tuyến tính:$$a_i - a_j = b_i - b_j \quad \text{or} \quad a_i - a_j = -(b_i - b_j).$$Sắp xếp lại mang lại:$$a_i - b_i = a_j - b_j \quad \text{or} \quad a_i + b_i = a_j + b_j.$$Vì vậy, một cặp là “xấu” chính xác khi giá trị$a_i - b_i$trận đấu hoặc giá trị$a_i + b_i$trận đấu. Điều này làm giảm vấn đề thành việc đếm các cặp bằng nhau trong hai mảng dẫn xuất khác nhau. 

Tuy nhiên, việc tính tổng trực tiếp cả hai số đếm sẽ vượt quá các cặp trong đó cả hai đẳng thức đều giữ nguyên đồng thời. Điều đó xảy ra chính xác khi cả hai:$$a_i - b_i = a_j - b_j \quad \text{and} \quad a_i + b_i = a_j + b_j,$$ngụ ý$(a_i, b_i) = (a_j, b_j)$. Vì vậy, các bản sao phải được sửa chữa cẩn thận. 

Do đó chúng tôi tính toán: 

1. tổng số cặp, 
2. Trừ các cặp “xấu” thông qua các đẳng thức tổng và hiệu, xử lý cẩn thận các bản sao bằng cách đếm tần số. 

Giải pháp hiệu quả sử dụng bản đồ sắp xếp hoặc băm để đếm tần số của các khóa được chuyển đổi$a_i - b_i$Và$a_i + b_i$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n \log n)$hoặc$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta viết lại điều kiện thành hai hệ thống khóa độc lập:$d_i = a_i - b_i$Và$s_i = a_i + b_i$. Chúng tôi đếm có bao nhiêu cặp có cùng giá trị trong mỗi hệ thống. 

1. Tính tổng số cặp như sau$\frac{n(n-1)}{2}$. Đây là đường cơ sở để chúng tôi trừ đi các cặp “xấu”. 
2. Xây dựng bản đồ tần số cho tất cả các giá trị$d_i = a_i - b_i$. Với mỗi giá trị phân biệt xuất hiện$f$nhiều khi nó góp phần$\frac{f(f-1)}{2}$cặp xấu. Điều này tính tất cả các cặp thỏa mãn$a_i - b_i = a_j - b_j$. 
3. Xây dựng bản đồ tần số thứ hai cho tất cả các giá trị$s_i = a_i + b_i$. Tương tự, mỗi tần số$f$đóng góp$\frac{f(f-1)}{2}$cặp ở đâu$a_i + b_i = a_j + b_j$. 
4. Cộng các đóng góp từ cả hai bản đồ để có tổng số cặp thỏa mãn ít nhất một trong các đẳng thức xấu. Tổng này đếm đôi các cặp trong đó cả hai điều kiện đều đúng. 
5. Xác định các bản sao ở đâu

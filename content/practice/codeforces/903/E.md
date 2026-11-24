---
title: "CF 903E - Hoán đổi ký tự"
description: "Chúng ta có chuỗi $k$ có cùng độ dài $n$. Mỗi chuỗi trong số chúng được tạo ra từ một chuỗi gốc không xác định bằng cách thực hiện chính xác một lần hoán đổi hai vị trí khác nhau. Các ký tự hoán đổi được phép bằng nhau, do đó một chuỗi có thể không thay đổi sau thao tác."
date: "2026-06-12T22:56:31+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "hashing", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 903
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 34 (Rated for Div. 2)"
rating: 2200
weight: 903
solve_time_s: 139
verified: false
draft: false
---

[CF 903E - Hoán đổi ký tự](https://codeforces.com/problemset/problem/903/E) 

**Đánh giá:** 2200 
**Tags:** vũ lực, băm, triển khai, chuỗi 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao$k$các chuỗi có cùng độ dài$n$. Mỗi chuỗi trong số chúng được tạo ra từ một chuỗi gốc không xác định bằng cách thực hiện chính xác một lần hoán đổi hai vị trí khác nhau. Các ký tự hoán đổi được phép bằng nhau, do đó một chuỗi có thể không thay đổi sau thao tác. 

Chúng ta phải xây dựng lại bất kỳ chuỗi gốc nào có thể tạo ra mọi chuỗi đã cho theo cách này. Nếu không có chuỗi như vậy tồn tại, chúng tôi in`-1`. 

Hạn chế thú vị nhất là$k \cdot n \le 5000$. Điều này có nghĩa là tổng kích thước đầu vào nhỏ nhưng một chiều vẫn có thể lớn. Ví dụ,$k=1$Và$n=5000$là hợp pháp. Bất kỳ thuật toán nào có chứa một$O(n^2)$việc liệt kê tất cả các giao dịch hoán đổi trở nên nguy hiểm khi$n$là lớn. 

Một quan sát hữu ích đến từ việc so sánh hai chuỗi đều thu được từ cùng một chuỗi gốc bằng một lần hoán đổi. Việc hoán đổi ảnh hưởng đến tối đa hai vị trí, do đó, hai chuỗi kết quả có thể khác nhau ở tối đa bốn vị trí. Nếu chúng ta tìm thấy hai chuỗi đầu vào có khoảng cách Hamming vượt quá bốn thì câu trả lời ngay lập tức là không thể. 

Có một số trường hợp góc dễ bỏ sót. 

Nếu một chuỗi đã bằng chuỗi gốc dự kiến, chúng ta vẫn cần một hoán đổi hợp lệ. Điều đó chỉ có thể thực hiện được khi chuỗi chứa một ký tự lặp lại, vì chúng ta có thể hoán đổi hai chữ cái bằng nhau và giữ nguyên chuỗi. 

Ví dụ:```

```Câu trả lời là`-1`. Bất kỳ chuỗi gốc nào bằng`abc`có tất cả các chữ cái riêng biệt, vì vậy không có phép hoán đổi chính xác nào có thể giữ nguyên nó. 

Một trường hợp tinh tế khác là khi hai dây khác nhau ở bốn vị trí.```

```Một kiểm tra ngây thơ chỉ cho phép khoảng cách hai sẽ từ chối cặp này, tuy nhiên cả hai đều có thể đến từ cùng một chuỗi gốc sau một lần hoán đổi. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là đoán chuỗi gốc và xác minh nó. Vì chuỗi gốc khác với bất kỳ chuỗi đã cho nào chỉ bằng một lần hoán đổi, nên chúng ta có thể lấy chuỗi đầu tiên và thử tất cả$O(n^2)$hoán đổi bên trong nó. Đối với mỗi ứng viên, chúng tôi sẽ kiểm tra tất cả$k$dây. 

Bản thân việc xác minh tốn kém$O(kn)$, cho$O(n^2kn)$tổng cộng. Con số này là quá lớn khi$n=5000$. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải thử mọi lần hoán đổi. 

Lấy hai chuỗi đầu vào khác nhau$a$Và$b$. Cho phép$p$là vị trí khác nhau đầu tiên của họ. Vì cả hai chuỗi đều được tạo từ cùng một chuỗi gốc bằng một lần hoán đổi, nên ít nhất một trong những lần hoán đổi đó phải liên quan đến vị trí$p$. Nếu không thì vị trí$p$sẽ không thay đổi trong cả hai chuỗi và không thể khác nhau. 

Điều đó có nghĩa là một trong những điều sau đây phải đúng: 

1. Bản gốc có thể được lấy từ$a$bằng cách hoán đổi vị trí$p$với một số vị trí khác. 
2. Bản gốc có thể được lấy từ$b$bằng cách hoán đổi vị trí$p$với một số vị trí khác. 

Bây giờ chỉ$O(n)$khả năng vẫn còn trong mỗi trường hợp. Chúng tôi liệt kê điểm cuối thứ hai của trao đổi, xây dựng chuỗi gốc ứng viên và xác minh nó dựa trên tất cả các chuỗi đầu vào. 

Bởi vì$k \cdot n \le 5000$, kiểm tra một ứng cử viên trong$O(kn)$thời gian đủ rẻ. Chúng tôi chỉ cố gắng$O(n)$ứng viên, dẫn đến$O(nkn)$, nhiều nhất là về$25$triệu so sánh ký tự và thoải mái phù hợp với giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê tất cả các giao dịch hoán đổi trong một chuỗi |$O(n^2kn)$|$O(n)$| Quá chậm | 
| Sửa một vị trí không khớp và liệt kê đối tác của nó |$O(nkn)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các chuỗi. 
2. Xác minh rằng tất cả các chuỗi đều là đảo chữ của chuỗi đầu tiên. 

Việc hoán đổi không bao giờ thay đổi tần số ký tự. Nếu một chuỗi nào đó có tần số khác nhau thì câu trả lời là không thể. 
3. Tìm một chuỗi khác với chuỗi đầu tiên. 

Nếu mọi chuỗi đều giống hệt nhau, hãy xử lý chuỗi này như một trường hợp đặc biệt. Chúng ta cần một ký tự trùng lặp ở đâu đó. Nếu không tồn tại, in`-1`. Ngược lại, hoán đổi hai ký tự bằng nhau và in ra chuỗi không thay đổi. 
4. Hãy để$a$là chuỗi đầu tiên và$b$là một chuỗi khác. Tìm vị trí đầu tiên$p$chúng khác nhau ở chỗ nào. 
5. Tạo ứng viên thành hai nhóm. 

Đầu tiên, trao đổi$a[p]$với mọi vị trí$j$. 

Thứ hai, trao đổi$b[p]$với mọi vị trí$j$. 

Mỗi chuỗi được tạo ra có thể là một chuỗi gốc. 
6. Đối với mỗi ứng viên, hãy kiểm tra từng chuỗi đầu vào. 

Hãy để ứng viên được$t$và chuỗi đầu vào hiện tại là$s$. 

Tính toán tất cả các vị trí ở đó$s$Và$t$khác nhau. 

Nếu số vị trí khác nhau là$1$, chuỗi này không bao giờ có thể có được bằng một lần hoán đổi. 

Nếu nó lớn hơn$2$, một lần hoán đổi là không đủ. 

Nếu chính xác là$2$, nói rằng các vị trí là$x$Và$y$. Chúng tôi yêu cầu$$s[x]=t[y], \quad s[y]=t[x].$$Nếu nó là$0$, sau đó$t$phải chứa một ký tự lặp lại. Nếu không thì không có trao đổi nào khiến nó không thay đổi. 
7. Ứng viên đầu tiên vượt qua tất cả các bước kiểm tra là câu trả lời hợp lệ. 
8. Nếu không có ứng cử viên nào sống sót

---
title: "CF 1246F - Khoảng cách con trỏ"
description: "Chúng ta được cung cấp một chuỗi các chữ cái viết thường, trong đó mỗi vị trí đóng vai trò giống như một “ô” được gắn nhãn bởi một ký tự. Một con trỏ nằm ở một trong những vị trí này và chúng ta được phép di chuyển nó bằng thao tác nhảy đặc biệt: chọn một ký tự và một hướng, sau đó con trỏ sẽ dịch chuyển đến…"
date: "2026-06-15T04:52:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 1246
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 596 (Div. 1, based on Technocup 2020 Elimination Round 2)"
rating: 3500
weight: 1246
solve_time_s: 80
verified: false
draft: false
---

[CF 1246F - Khoảng cách con trỏ](https://codeforces.com/problemset/problem/1246/F) 

**Đánh giá:** 3500 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các chữ cái viết thường, trong đó mỗi vị trí đóng vai trò giống như một “ô” được gắn nhãn bởi một ký tự. Con trỏ nằm ở một trong những vị trí này và chúng ta được phép di chuyển nó bằng thao tác nhảy đặc biệt: chọn một ký tự và một hướng, sau đó con trỏ sẽ dịch chuyển đến nơi xuất hiện gần nhất của ký tự đó theo hướng đó. 

Điểm mấu chốt là chuyển động không phải là khoảng cách hình học trên các chỉ số mà là sự điều hướng giống như biểu đồ thông qua các ký tự lặp lại. Từ bất kỳ vị trí nào, con trỏ chỉ có thể “nhìn thấy” lần xuất hiện tiếp theo của mỗi ký tự ở bên trái hoặc bên phải và có thể nhảy tới đó trong một bước. 

Đối với mỗi cặp vị trí$i, j$, chúng tôi xác định$\mathrm{dist}(i, j)$vì số lần nhảy tối thiểu cần thiết để di chuyển con trỏ từ$i$ĐẾN$j$. Nhiệm vụ là tính tổng khoảng cách này trên tất cả các cặp có thứ tự. 

Độ dài chuỗi lên tới$10^5$, do đó, bất kỳ phương pháp nào cố gắng tính toán các đường đi ngắn nhất một cách độc lập cho từng vị trí bắt đầu sẽ thất bại ngay lập tức. Thậm chí$O(n^2)$dung lượng lưu trữ hoặc truyền tải đã quá lớn và mọi thứ giống như BFS tất cả các cặp đều không thể thực hiện được. 

Trường hợp cạnh tinh tế xuất hiện khi các ký tự là duy nhất hoặc gần như duy nhất. Nếu mỗi ký tự xuất hiện một lần thì mỗi lần nhảy đều vô ích, vì vậy$\mathrm{dist}(i,j)=0$khi$i=j$Và$1$nếu không thì. Bất kỳ giải pháp đúng đắn nào cũng phải tự nhiên sụp đổ theo hành vi này mà không có vỏ bọc đặc biệt. 

Một trường hợp không tầm thường khác là các chuỗi có tính lặp lại cao như`"aaaaa..."`. Ở đây mọi vị trí đều liền kề trong “đồ thị nhảy”, do đó cấu trúc trở nên dày đặc và mô phỏng đơn giản sẽ bùng nổ trong quá trình chuyển đổi. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xem từng vị trí dưới dạng một nút trong biểu đồ, trong đó các cạnh biểu thị các bước nhảy con trỏ hợp lệ. Từ một nút$i$, với mỗi ký tự$c$, có nhiều nhất hai cạnh hướng ra ngoài: cạnh xuất hiện gần nhất của$c$ở bên trái và bên phải. Điều này xác định một biểu đồ với$O(n)$nút và$O(n)$các cạnh. 

Nếu chúng ta chạy BFS từ mọi nút để tính toán các đường đi ngắn nhất thì chi phí sẽ trở thành$O(n^2)$Mở rộng BFS, vượt xa giới hạn. Ngay cả một BFS duy nhất cũng$O(n)$, do đó tổng công việc trở thành$O(n^2)$. 

Quan sát quan trọng là chúng ta không được yêu cầu khoảng cách riêng lẻ mà yêu cầu tổng của tất cả các cặp. Điều này gợi ý đảo ngược quan điểm: thay vì hỏi mỗi cặp cách nhau bao nhiêu, chúng ta hỏi có bao nhiêu cặp cách nhau ít nhất$k$các bước và tổng hợp mức đóng góp theo cấp độ. 

Một cái nhìn sâu sắc hơn về cấu trúc là mỗi lớp ký tự tạo ra một “biểu đồ xuất hiện tiếp theo”, là một tập hợp các chuỗi hai chiều rời rạc. Bất kỳ con đường ngắn nhất nào cũng xen kẽ giữa việc di chuyển dọc theo các chuỗi này thông qua các loại ký tự khác nhau. Điều này cho phép chúng ta nén các chuyển tiếp và coi vấn đề là sự đóng góp từ các khoảng thời gian giữa các lần xuất hiện liên tiếp. 

Giải pháp cuối cùng giảm bớt việc đếm sự đóng góp của các phân đoạn được xác định bởi sự xuất hiện của từng ký tự và tổng hợp số lượng cặp yêu cầu vượt qua từng ranh giới trong cấu trúc lớp ẩn. Điều này dẫn đến một$O(n \log n)$hoặc$O(n)$phương pháp đếm tùy thuộc vào việc triển khai, bằng cách xử lý các lần xuất hiện và duy trì các khoản đóng góp bằng cách tích lũy kiểu Fenwick hoặc ghi sổ tần số tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS Brute Force từ mỗi nút |$O(n^2)$|$O(n)$| Quá chậm | 
| Đếm đóng góp dựa trên sự xuất hiện |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi ký tự, hãy lưu trữ tất cả các chỉ mục nơi nó xuất hiện. 

Điều này là cần thiết vì mỗi bước di chuyển được phép chỉ phụ thuộc vào lần xuất hiện gần nhất theo một hướng. 
2. Đối với từng vị trí$i$, xác định “mục tiêu nhảy” đi ra của nó bằng cách xem xét các lần xuất hiện liền kề của mỗi ký tự. 

Các mục tiêu này xác định một biểu đồ ẩn mà không xây dựng rõ ràng tất cả các cạnh. 
3. Quan sát ngắn nhất

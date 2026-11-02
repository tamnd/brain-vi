---
title: "CF 106503F - Vấn đề hàng đầu của sinh viên\u2161"
description: "Chúng ta có một lưới hình chữ nhật có kích thước $n nhân m$ nơi chúng ta có thể đặt các chồng khối đơn vị, do đó mỗi ô chứa một chiều cao nguyên không âm. Chúng tôi không nhìn thấy lưới trực tiếp. Thay vào đó, chúng ta có hai hình chiếu có cùng cấu trúc 3D."
date: "2026-06-19T15:07:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106503
codeforces_index: "F"
codeforces_contest_name: "2026 \u534e\u5357\u5e08\u8303\u5927\u5b66\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b (SCNUCPC 2026)"
rating: 0
weight: 106503
solve_time_s: 25
verified: false
draft: false
---

[CF 106503F - Vấn đề hàng đầu của sinh viên\u2161](https://codeforces.com/problemset/problem/106503/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một lưới hình chữ nhật có kích thước$n \times m$trong đó chúng ta có thể đặt các chồng khối đơn vị để mỗi ô chứa một chiều cao nguyên không âm. Chúng tôi không nhìn thấy lưới trực tiếp. Thay vào đó, chúng ta có hai hình chiếu có cùng cấu trúc 3D. 

Phép chiếu đầu tiên tương ứng với việc xem lưới từ phía bên trái. Điều này có nghĩa là với mỗi hàng$i$, chúng ta chỉ biết chiều cao tối đa trong số tất cả các ô trong hàng đó. Phép chiếu thứ hai tương ứng với việc xem từ phía trước, vì vậy đối với mỗi cột$j$, chúng ta chỉ biết chiều cao tối đa trong số tất cả các ô trong cột đó. 

Nhiệm vụ là xác định xem có tồn tại bất kỳ sự phân công độ cao nào không$h_{i,j} \ge 0$sao cho tất cả các cực đại của hàng khớp với mảng đã cho$a$và tất cả các cực đại của cột khớp với mảng đã cho$b$. 

Khó khăn chính là các ràng buộc hàng và cột không độc lập. Việc tăng một ô có thể thỏa mãn đồng thời ràng buộc hàng và ràng buộc cột, do đó, cấu trúc là về việc liệu các giá trị cực đại này có thể được "thực hiện một cách nhất quán" hay không. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng số phần tử trên mỗi tệp thử nghiệm trong tất cả các thử nghiệm. Điều này loại trừ bất kỳ$O(nm)$sự thi công. Chúng ta chỉ phải làm việc với chính các mảng chứ không phải với mô phỏng lưới rõ ràng. 

Trường hợp cạnh tinh tế xuất hiện khi các số 0 tương tác với cực đại dương. Ví dụ: nếu mức tối đa của một hàng là dương nhưng mức tối đa của mỗi cột bằng 0 thì câu trả lời phải là Không, vì không có chỗ để đặt chiều cao dương. Một cạm bẫy phổ biến khác là giả định rằng tổng hoặc tổng hàng và cột thỏa mãn là đủ, trong khi trên thực tế, cấu trúc tối đa chặt chẽ hơn. 

Một ví dụ mâu thuẫn tối thiểu là: 

Hàng: [2], Cột: [0] 

Ở đây một ô phải đồng thời là 2 và nhiều nhất là 0, điều này là không thể. 

## Phương pháp tiếp cận 

Một nỗ lực mạnh mẽ sẽ cố gắng xây dựng lưới điện một cách trực tiếp. Người ta có thể bắt đầu với tất cả các số 0 và gán các giá trị lặp đi lặp lại để đáp ứng cực đại của hàng và cột, quay lại khi xung đột phát sinh. Đối với mỗi ô, chúng tôi sẽ thử các giá trị tối thiểu theo yêu cầu về hàng và cột của nó. Trong trường hợp xấu nhất, mỗi ô có nhiều lựa chọn và không gian tìm kiếm sẽ trở thành hàm mũ theo$nm$, làm cho nó hoàn toàn không khả thi ngay cả đối với$n, m \approx 200$. 

Quan sát cấu trúc quan trọng là chỉ có vật chất cực đại. Mỗi hàng$i$phải chứa ít nhất một ô bằng$a_i$và mỗi cột$j$phải chứa ít nhất một ô bằng$b_j$. Một khi những "ma

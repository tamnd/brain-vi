---
title: "CF 1539E - Trò chơi bài"
description: "Biểu thị mỗi số nguyên trong cơ sở $b$ dưới dạng danh sách các chữ số được liên kết đôi $$(v{n-1}, v{n-2}, dots, v0),$$ trong đó mỗi nút chứa một chữ số $vi trong {0, dấu chấm, b-1}$, cùng với các con trỏ $$text{left}(i), text{right}(i)$$ tới các chữ số liền kề."
date: "2026-06-10T14:43:52+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "data-structures", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1539
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 727 (Div. 2)"
rating: 2500
weight: 1539
solve_time_s: 125
verified: false
draft: false
---

[CF 1539E - Trò chơi bài](https://codeforces.com/problemset/problem/1539/E) 

**Đánh giá:** 2500 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, cấu trúc dữ liệu, dp, tham lam, triển khai 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** không 

##Giải pháp 
## Biểu diễn số nguyên 

Biểu diễn mỗi số nguyên trong cơ số$b$dưới dạng danh sách các chữ số được liên kết đôi$$(v_{n-1}, v_{n-2}, \dots, v_0),$$trong đó mỗi nút chứa một chữ số$v_i \in \{0, \dots, b-1\}$, cùng với con trỏ$$\text{left}(i), \text{right}(i)$$sang các chữ số liền kề. Con trỏ đầu chỉ chữ số có nghĩa nhất, con trỏ đuôi chỉ chữ số có nghĩa nhỏ nhất. 

Mỗi bản ghi số nguyên cũng lưu trữ một bit dấu. 

Cấp phát bộ nhớ sử dụng danh sách miễn phí các nút không được sử dụng. Phân bổ và phân bổ là các hoạt động liên tục bằng cách lấy hoặc trả lại các nút về danh sách miễn phí. 

## Chương trình con cộng 

Để tính toán$x + y$: 

1. Căn chỉnh các chữ số nhỏ nhất của$x$Và$y$. 
2. Duyệt qua cả hai danh sách từ chữ số ít quan trọng nhất đến chữ số có ý nghĩa nhất. 
3. Duy trì mang theo$c \in \{0,1\}$. 
4. Với mỗi vị trí, hãy tính$$s_i = x_i + y_i + c, \quad z_i = s_i \bmod b, \quad c = \lfloor s_i / b \rfloor.$$5. Phân bổ một nút mới cho mỗi chữ số$z_i$và liên kết nó vào danh sách kết quả. 
6. Nếu phần nhớ vẫn còn ở cuối, hãy phân bổ một nút mới quan trọng nhất. 

Điều này chạy theo thời gian tỷ lệ thuận với độ dài tối đa của đầu vào. 

## Chương trình con trừ 

Để tính toán$x - y$, trước tiên giả sử rằng$|x| \ge |y|$. 

1. Di chuyển từ chữ số có nghĩa nhỏ nhất. 
2. Duy trì khoản vay$c \in \{0,1\}$. 
3. Tính toán$$d_i = x_i - y_i - c.$$4. Nếu$d_i < 0$, bộ$d_i \leftarrow d_i + b$Và$c \leftarrow 1$, nếu không thì$c \leftarrow 0$. 
5. Bảo quản từng loại$d_i$trong một nút mới được phân bổ. 
6. Loại bỏ các số 0 ở đầu bằng cách đưa các nút trở lại danh sách miễn phí. 

Nếu như$|x| < |y|$, tính toán$y - x$và gắn dấu trừ. 

## chương trình con nhân 

Để tính toán$x \cdot y$: 

1. Khởi tạo kết quả bằng 0. 
2. Với mỗi chữ số$y_j$, tiến hành từ ít quan trọng nhất đến quan trọng nhất: 

- Nhân lên$x$theo chữ số$y_j$, tạo ra một phần sản phẩm. 
- Chuyển tích từng phần theo$j$vị trí (được thực hiện bằng offset trong cấu trúc danh sách). 
- Cộng tích từng phần đã dịch chuyển vào kết quả tích lũy bằng chương trình con cộng. 
3. Mỗi chữ số tích một phần được tính mang chính xác như trong phép nhân trường học. 

Mỗi nút trung gian được phân bổ động; không cần phân bổ trước. 

## Chương trình con chia 

Để tính toán$x / y$, cho rằng$y \ne 0$: 

1. Chuẩn hóa bằng cách đảm bảo chữ số đầu của$y$là khác không. 
2. Thực hiện phép chia dài từ chữ số có nghĩa nhất của$x$. 
3. Duy trì số dư hiện tại$r$, ban đầu bằng không. 
4. Ở mỗi bước, hãy mở rộng$r$bằng một chữ số từ$x$. 
5. Xác định chữ số lớn nhất$q_i \in \{0, \dots, b-1\}$như vậy$$q_i y \le r.$$6. Trừ$q_i y$từ$r$bằng cách sử dụng chương trình con trừ. 
7. Nối thêm$q_i$vào danh sách thương số. 

Tất cả các phép so sánh và phép nhân được thực hiện bằng cách sử dụng các thủ tục đã xác định trước đó. 

## Tính chính xác của việc quản lý bộ nhớ 

Tất cả các kết quả được xây dựng bằng cách sử dụng các nút mới được phân bổ từ danh sách miễn phí. Khi các chữ số bị xóa (ví dụ như số 0 ở đầu), các nút sẽ được đưa trở lại danh sách miễn phí ngay lập tức. Không cần tìm kiếm bộ nhớ trống. 

Do đó, thuật toán sử dụng phân bổ liên kết một cách hiệu quả và tôn trọng ràng buộc rằng bộ nhớ chỉ được sử dụng khi có sẵn. 

## Kết luận 

Bốn chương trình con cộng, trừ, nhân và chia hoạt động chính xác trên các số nguyên có độ dài tùy ý được biểu diễn dưới dạng danh sách chữ số được liên kết, chỉ sử dụng các phép toán chữ số cục bộ, truyền mang/mượn và phân bổ bộ nhớ động. 

Điều này đáp ứng yêu cầu thực hiện số học số nguyên có độ chính xác tùy ý bằng cách sử dụng phân bổ bộ nhớ được liên kết.

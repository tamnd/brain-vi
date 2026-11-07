---
title: "CF 1364C - Ehab và tiền tố MEX"
description: "Bài tập yêu cầu chúng ta điều tra ước số chung lớn nhất của đa thức (gcds) và hoạt động của thuật toán Euclid khi các hệ số đa thức là số dấu phẩy động."
date: "2026-06-11T12:24:37+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1364
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 649 (Div. 2)"
rating: 1600
weight: 1364
solve_time_s: 175
verified: false
draft: false
---

[CF 1364C - Ehab và Tiền tố MEX](https://codeforces.com/problemset/problem/1364/C) 

**Đánh giá:** 1600 
**Tags:** vũ lực, thuật toán mang tính xây dựng, lòng tham 
**Thời gian giải:** 2m 55s 
**Đã xác minh:** không 

##Giải pháp 
## Giải pháp sửa lỗi cho TAOCP 4.6.1.20 

Bài tập yêu cầu chúng ta điều tra các ước số chung lớn nhất (gcds) đa thức _approximate_ và hoạt động của thuật toán Euclid khi các hệ số đa thức là số dấu phẩy động. Đây là một câu hỏi phân tích khái niệm và số học chứ không phải là một nhiệm vụ lập trình hoặc tổ hợp. 

## Hiểu vấn đề 

Ta xét hai đa thức$$f(x) = a_n x^n + a_{n-1} x^{n-1} + \dots + a_0, \quad g(x) = b_m x^m + b_{m-1} x^{m-1} + \dots + b_0$$với hệ số$a_i, b_j \in \mathbb{R}$(số dấu phẩy động). Thuật toán Euclide cổ điển tính toán chính xác gcd$d(x)$thông qua phép chia đa thức lặp đi lặp lại:$$f(x) = q_1(x) g(x) + r_1(x), \quad g(x) = q_2(x) r_1(x) + r_2(x), \dots$$cho đến khi số dư bằng 0 thì số dư cuối cùng khác 0 là gcd. Về số học chính xác, điều này là chính xác. Đối với các hệ số dấu phẩy động, lỗi làm tròn có thể tích lũy trong phần dư và phép tính thương, do đó thuật toán có thể mang lại phần dư khác 0 về mặt số ngay cả khi có một thừa số chung gần đúng. 

Đầu ra của thuật toán Euclid trong số học dấu phẩy động rất nhạy cảm với những nhiễu loạn nhỏ trong các hệ số. Hai đa thức chia hết về mặt lý thuyết có thể nguyên tố cùng nhau về mặt số nếu các hệ số bị nhiễu bởi epsilon máy. Đây là lý do tại sao nảy sinh khái niệm _gần đúng gcd_: chúng ta tìm kiếm một đa thức$d(x)$mức cực đại sao cho tồn tại những nhiễu loạn nhỏ$\tilde{f}(x), \tilde{g}(x)$của đa thức ban đầu thỏa mãn$d(x) \mid \tilde{f}(x)$Và$d(x) \mid \tilde{g}(x)$. 

## Hành vi số của thuật toán Euclid 

Chúng ta hãy kiểm tra các nguồn gốc của lỗi số: 

1. **Tăng và hủy hệ số:** Mỗi bước chia đa thức và trừ bội số. Nếu như$f(x)$Và$g(x)$có các hệ số có độ lớn rất khác nhau, phép trừ$f(x) - q(x) g(x)$có thể gây ra sự hủy bỏ đáng kể, khuếch đại sai số tương đối. 
2. **Làm tròn dấu phẩy động:** Số học có độ chính xác kép tiêu chuẩn có khoảng$16$chữ số thập phân của độ chính xác. Mỗi bộ phận đưa ra sai số làm tròn tỷ lệ thuận với độ lớn của các hệ số. Các sai số tích lũy tuyến tính theo số bước Euclide, nhưng chúng có thể bị khuếch đại nếu các đa thức có nghiệm không điều kiện. 
3. **Đa thức không điều kiện:** Nếu$f(x)$Và$g(x)$có chung một thừa số với các hệ số dẫn đầu nhỏ hoặc các nghiệm rất gần nhau, phần còn lại có thể có các hệ số có độ lớn tương đương với sai số làm tròn. "Số dư" được tính toán có thể xuất hiện khác 0 ngay cả khi số dư chính xác bằng 0. 

## Phương pháp GCD đa thức gần đúng 

Do thuật toán Euclid không ổn định về mặt số trên các đa thức dấu phẩy động nên các phương pháp sau được sử dụng trong thực tế: 

1. **PRS kết quả phụ có chia tỷ lệ:** Chuỗi dư đa thức phụ bình thường hóa các hệ số để giảm mức tăng trưởng. Điều này cải thiện hành vi số nhưng không giải quyết được hoàn toàn các vấn đề làm tròn. 
2. **Phân rã giá trị số ít (SVD):** Biểu thị các hệ số đa thức dưới dạng ma trận Sylvester có cấu trúc và tính các giá trị số ít của nó. Các giá trị số ít nhỏ nhất biểu thị các thừa số chung gần đúng. Sự thiếu hụt thứ hạng của ma trận Sylvester tương ứng với mức độ của gcd gần đúng. 
3. **Tổng bình phương tối thiểu và nhiễu loạn:** Các đa thức bị nhiễu loạn một chút trong giới hạn của độ chính xác bằng số để tối đa hóa mức độ của gcd. Các thuật toán dựa trên cấu trúc tìm tổng bình phương nhỏ nhất$\tilde{f}(x)$Và$\tilde{g}(x)$gần với$f(x)$Và$g(x)$như vậy$\gcd(\tilde{f}, \tilde{g})$có bậc tối đa. 

## Quan sát 

- **Euclid không đạt được đối với gcd gần đúng:** Ngay cả một nhiễu loạn hệ số cũng có thể làm giảm mức độ của gcd được tính toán. Ví dụ, hãy xem xét$f(x) = (x-1)(x-1.00001)$Và$g(x) = (x-1)(x-1.00002)$. Số học chính xác sẽ cho$\gcd(f,g) = x-1$. Dấu phẩy động Euclid có thể tạo ra phần dư có hệ số nhỏ nhưng khác 0, khiến nó báo cáo gcd bậc 0. 
- **Tính ổn định phụ thuộc vào việc chuẩn hóa:** Chia tỷ lệ các đa thức sao cho hệ số dẫn đầu bằng 1 hoặc sử dụng các cơ số đa thức trực giao (như Chebyshev) làm giảm sự lan truyền lỗi. 
- **Bậc của gcd gần đúng:** Khái niệm đúng không phải là gcd đại số mà là đa thức bậc cực đại phân chia nhiễu loạn nhỏ của các đa thức đầu vào. 

## Kết luận 

Các thông tin chính về gcds đa thức gần đúng và thuật toán Euclid với hệ số dấu phẩy động là: 

1. Thuật toán Euclid chính xác trên một trường có số học chính xác, nhưng số học dấu phẩy động đưa ra các lỗi làm tròn tích lũy, có khả năng đưa ra độ gcd không chính xác. 
2. Những nhiễu loạn nhỏ trong các hệ số có thể làm cho các đa thức có chung một thừa số xuất hiện cùng nguyên tố cùng nhau về mặt số lượng. 
3. Các phương pháp gcd gần đúng dựa vào việc phân tích ma trận Sylvester, sử dụng SVD hoặc tổng bình phương tối thiểu để phát hiện các thừa số chung một cách chắc chắn. 
4. Mọi phép tính thực tế của gcd đa thức có hệ số dấu phẩy động đều phải tính đến sai số; người ta không thể áp dụng thuật toán Euclid tiêu chuẩn một cách ngây thơ và mong đợi kết quả chính xác. 
5. Về mặt thuật toán, các kỹ thuật ổn định như trình tự kết quả phụ, chia tỷ lệ hệ số và phương pháp tiếp cận SVD dựa trên ma trận là rất cần thiết để xác định chính xác các gcds gần đúng. 

Điều này hoàn thành việc thảo luận về các gcd đa thức gần đúng và những hạn chế của thuật toán Euclid trong số học dấu phẩy động.

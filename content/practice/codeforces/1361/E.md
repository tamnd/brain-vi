---
title: "CF 1361E - James và cuộc truy đuổi"
description: "Bài tập yêu cầu đơn giản hóa đại số trực tiếp của hai danh tính được hiển thị liên quan đến nội dung và phần nguyên thủy của đa thức trên miền phân tích nhân tử duy nhất $S$."
date: "2026-06-11T12:47:51+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "probabilities", "trees"]
categories: ["algorithms"]
codeforces_contest: 1361
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 647 (Div. 1) - Thanks, Algo Muse!"
rating: 3000
weight: 1361
solve_time_s: 173
verified: false
draft: false
---

[CF 1361E - James và kẻ truy đuổi](https://codeforces.com/problemset/problem/1361/E) 

**Đánh giá:** 3000 
**Thẻ:** dfs và tương tự, đồ thị, xác suất, cây 
**Thời gian giải:** 2m 53s 
**Đã xác minh:** không 

##Giải pháp 
## Giải pháp đúng cho TAOCP 4.6.1.24 

Bài tập yêu cầu đơn giản hóa đại số trực tiếp của hai danh tính được hiển thị liên quan đến nội dung và phần nguyên thủy của đa thức trên một miền phân tích nhân tử duy nhất$S$. Mục đích không phải là giới thiệu các định lý cấu trúc mới mà là viết lại các biểu thức đã cho cho đến khi chúng khớp với các dạng rõ ràng hơn đã được nêu ở phần sau. 

Điểm mấu chốt xuyên suốt là cả hai$\operatorname{cont}(\cdot)$Và$\operatorname{pp}(\cdot)$được xác định bằng hệ số cố định$$u(x) = \operatorname{cont}(u)\,\operatorname{pp}(u(x)),$$với$\operatorname{cont}(u) \in S$Và$\operatorname{pp}(u)$nguyên thủy theo nghĩa của Mục 4.6.1. Sự phân tách này mang tính chuẩn mực theo các quy ước của phần này, do đó không có sự mơ hồ bổ sung nào ngoài định nghĩa này được đưa ra hoặc cần thiết. 

## Rút gọn (19) thành (20) 

Phương trình (19) là định nghĩa hàm lượng ở cấp độ hệ số cho một tích đa thức. Nó định nghĩa$\operatorname{cont}(uv)$là ước chung lớn nhất (trong$S$) của tất cả các hệ số của tích$uv$. 

Viết$$u(x) = \sum_i u_i x^i, \quad v(x) = \sum_j v_j x^j.$$Khi đó các hệ số của$uv$là tổng hữu hạn của các phần tử$u_i v_j$. Mỗi hệ số của$uv$do đó là một$S$-sự kết hợp tuyến tính của các tích của các hệ số của$u$Và$v$. 

Quan sát quan trọng là mọi hệ số của$uv$chia hết cho mọi ước chung của mọi hệ số của$u$, và cũng theo mọi ước số chung của mọi hệ số của$v$. Nếu như$d$chia tất cả$u_i$, thì mỗi hệ số$u_i v_j$chia hết cho$d$, do đó mọi hệ số của$uv$chia hết cho$d$. Điều tương tự cũng xảy ra với mọi ước số chung của$v_j$. Do đó mọi ước chung của các hệ số của$u$và của$v$cũng là ước chung của các hệ số$uv$, điều này mang lại sự chia hết$$\operatorname{cont}(u)\operatorname{cont}(v) \mid \operatorname{cont}(uv).$$Ngược lại, mọi hệ số của$u$chia hết cho$\operatorname{cont}(u)$, và mọi hệ số của$v$chia hết cho$\operatorname{cont}(v)$, vậy mọi hệ số$u_i v_j$chia hết cho$\operatorname{cont}(u)\operatorname{cont}(v)$. Vì các hệ số của$uv$là tổng của các số hạng như vậy, chúng đều chia hết cho$\operatorname{cont}(u)\operatorname{cont}(v)$. Kể từ đây$$\operatorname{cont}(uv) \mid \operatorname{cont}(u)\operatorname{cont}(v).$$Cả hai phép chia cùng nhau tạo ra sự bằng nhau trong UFD cho đến các đơn vị và theo quy ước tiêu chuẩn được sử dụng trong Phần 4.6.1 (nội dung được lấy làm công cụ tạo chính tắc của hệ số lý tưởng), điều này loại bỏ mọi sự mơ hồ về đơn vị. Kết quả chính xác là danh tính đơn giản hóa (20):$$\operatorname{cont}(uv) = \operatorname{cont}(u)\operatorname{cont}(v).$$Việc “đơn giản hóa” từ (19) đến (20) chính xác là sự thừa nhận rằng gcd theo hệ số của các tích phân thành tích của các gcd theo hệ số, bởi vì tính chia hết lan truyền thông qua phép nhân và phép cộng mà không đưa ra các thừa số chung mới ngoài các thừa số chung đã có. 

## Rút gọn (24) thành (24) 

Phương trình (24) liên quan đến phần nguyên thủy của sản phẩm. Bắt đầu từ việc xác định hệ số,$$u(x) = \operatorname{cont}(u)\operatorname{pp}(u(x)), \quad v(x) = \operatorname{cont}(v)\operatorname{pp}(v(x)),$$chúng tôi nhân lên để có được$$uv = \operatorname{cont}(u)\operatorname{cont}(v)\,\operatorname{pp}(u)\operatorname{pp}(v).$$Bây giờ hãy áp dụng định nghĩa về phần nguyên thủy: một đa thức được viết duy nhất khi nội dung của nó nhân với phần nguyên thủy của nó và phần nguyên thủy được đặc trưng bởi có nội dung bằng$1$trong việc chuẩn hóa phần này. 

Đầu tiên chúng ta xác định nội dung của$uv$. Từ thân phận đã được thiết lập (20),$$\operatorname{cont}(uv) = \operatorname{cont}(u)\operatorname{cont}(v).$$Chia$uv$bởi yếu tố nội dung này:$$\operatorname{pp}(uv) = \frac{uv}{\operatorname{cont}(uv)} = \frac{\operatorname{cont}(u)\operatorname{cont}(v)\,\operatorname{pp}(u)\operatorname{pp}(v)}{\operatorname{cont}(u)\operatorname{cont}(v)}.$$Các yếu tố nội dung hủy bỏ chính xác, để lại$$\operatorname{pp}(uv) = \operatorname{pp}(u)\operatorname{pp}(v).$$Đây là sự đơn giản hóa đại số trực tiếp: không cần điều chỉnh đơn vị hoặc lựa chọn chuẩn hóa, vì việc hủy bỏ xảy ra hoàn toàn bên trong việc phân tích nhân tử xác định. 

Do đó, phương trình (24) ngay lập tức rút gọn thành đồng nhất thức nhân rõ ràng cho các phần nguyên thủy, là dạng đơn giản được nêu trong phần này. 

## Tại sao những đơn giản hóa này lại hợp lệ 

Cả hai bước chỉ dựa vào thuộc tính xác định của nội dung dưới dạng hệ số gcd và việc xác định nhân tử của đa thức thành nội dung nhân với phần nguyên thủy. Danh tính đầu tiên là thuộc tính cấu trúc của hệ số chia hết trong phép nhân. Khi đó, đồng nhất thức thứ hai là hệ quả trực tiếp của việc thay thế phân tích nhân tử và hủy bỏ hạng nội dung bằng cách sử dụng kết quả của đồng nhất thức thứ nhất. 

Không cần giả định bổ sung về đơn vị, quy ước chuẩn hóa hoặc bổ đề cấu trúc bên ngoài. Việc đơn giản hóa hoàn toàn là viết lại đại số trong các định nghĩa đã có trong Phần 4.6.1.

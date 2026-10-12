---
title: "CF 105043E - \u0423\u0436\u0438\u043d \u0443 \u043b\u0435\u0441\u043d\u0438\u043a\u0430 \u042f\u043d\u043a\u0438"
description: "Chúng ta được cung cấp một mảng các giá trị, trong đó mỗi vị trí có một số gắn liền với nó. Đối với mỗi truy vấn, chúng tôi lấy một phân đoạn liền kề của mảng này và được phép tự do hoán đổi các giá trị bên trong phân đoạn đó trước khi đánh giá điểm."
date: "2026-06-28T01:32:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105043
codeforces_index: "E"
codeforces_contest_name: "\u0424\u0438\u043d\u0430\u043b \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b \u041d\u0422\u041e: \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0446\u0438\u043e\u043d\u043d\u0430\u044f \u0431\u0435\u0437\u043e\u043f\u0430\u0441\u043d\u043e\u0441\u0442\u044c. \u0421\u0435\u043a\u0446\u0438\u044f - \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0430"
rating: 0
weight: 105043
solve_time_s: 26
verified: false
draft: false
---

[CF 105043E - \u0423\u0436\u0438\u043d \u0443 \u043b\u0435\u0441\u043d\u0438\u043a\u0430 \u042f\u043d\u043a\u0438](https://codeforces.com/problemset/problem/105043/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các giá trị, trong đó mỗi vị trí có một số gắn liền với nó. Đối với mỗi truy vấn, chúng tôi lấy một phân đoạn liền kề của mảng này và được phép tự do hoán đổi các giá trị bên trong phân đoạn đó trước khi đánh giá điểm. Sau khi đánh giá truy vấn, mảng được khôi phục, do đó các truy vấn độc lập. 

Điểm của một đoạn phụ thuộc vào giá trị nhỏ nhất của biểu thức`a[i] - i`, Ở đâu`i`là vị trí trong đoạn sau khi hoán vị được áp dụng. Vì chúng ta được phép sắp xếp lại các giá trị bên trong phân đoạn nên chỉ mục`i`thực sự trở thành một tập hợp các “trọng số” cố định mà chúng ta gán các giá trị. 

Một cách hữu ích để diễn đạt lại truy vấn là: chúng tôi muốn gán các giá trị`a[l..r]`đến vị trí`l..r`theo bất kỳ thứ tự nào và chúng tôi muốn tối đa hóa mức tối thiểu trên tất cả các vị trí của`a[pos] - pos`. 

Mỗi truy vấn yêu cầu giá trị tối thiểu tối đa có thể này. 

Các ràng buộc cho phép tối đa 5·10^4 phần tử và 5·10^4 truy vấn, do đó, bất kỳ giải pháp nào xử lý từng truy vấn theo thời gian tuyến tính trên phân đoạn của nó sẽ quá chậm trong trường hợp xấu nhất. Cách tiếp cận sắp xếp theo truy vấn đơn giản cũng quá chậm nếu được thực hiện nhiều lần mà không xử lý trước. Chúng ta nên mong đợi quá trình xử lý trước khoảng O(n log n) với O(log n) hoặc O(1) cho mỗi truy vấn. 

Một trường hợp thất bại tinh tế phát sinh khi các giá trị được đan xen chặt chẽ với các chỉ số. Ví dụ: nếu chúng ta có một phân khúc trong đó các giá trị lớn xuất hiện sớm nhưng phải khớp với các chỉ số lớn sau khi hoán vị, thì lý luận ngây thơ như “cặp lớn nhất với chỉ số lớn nhất” phải được đưa ra chính xác, vì mục tiêu là giảm thiểu chênh lệch dịch chuyển, không khớp với độ lớn thô. 

## Phương pháp tiếp cận 

Quan sát quan trọng là khi chúng ta hoán vị phân đoạn, chúng ta có thể tự do quyết định giá trị nào sẽ thuộc về vị trí nào, nhưng mỗi vị trí đều đóng góp`a[i] - i`. Điều này tương đương với việc gán giá trị cho chi phí cố định`-i`, vì vậy chúng tôi muốn tối đa hóa mức tối thiểu của`(assigned value minus fixed offset)`. 

Viết lại điều kiện cho câu trả lời của thí sinh`x`. Chúng tôi muốn mọi vị trí`i`TRONG`[l, r]`để thỏa mãn:`a[pos assigned to i] - i ≥ x`, tương đương với`a[pos assigned to i] ≥ x + i`. 

Bây giờ hãy giải thích điều này một cách tham lam: nếu chúng ta sửa`x`, mỗi vị trí`i`yêu cầu một ngưỡng`x + i`. Chúng ta cần kiểm tra xem tập hợp nhiều giá trị trong`[l, r]`có thể được gán cho các vị trí sao cho mỗi vị trí nhận được ít nhất một giá trị ở ngưỡng của nó. Đây là một câu hỏi khả thi phù hợp giữa hai bên cổ điển trên một dòng và vì cả hai bên đều được sắp xếp theo cấu trúc nên chiến lược tối ưu là sắp xếp tham lam. 

Đối với một cố định`x`, sắp xếp các giá trị trong phân đoạn và sắp xếp các ngưỡng bắt buộc`x + i`. Sau đó, tham lam khớp yêu cầu nhỏ nhất với giá trị sẵn có nhỏ nhất. Nếu tất cả các yêu cầu đều được đáp ứng,`x`là khả thi. 

Điều này biến mỗi truy vấn thành một kiểm tra tính khả thi có giá trị O(k log k) nếu được thực hiện một cách ngây thơ. Sau đó chúng tôi tìm kiếm nhị phân câu trả lời cho mỗi truy vấn. Tuy nhiên, điều này vẫn còn quá chậm. 

Việc đơn giản hóa cấu trúc tiếp theo là viết lại điều kiện:`a[i] - i ≥ x`trở thành`a[i] ≥ x + i`. Xác định mảng chuyển đổi`b[i] = a[i] - i`. Khi đó, điểm phân khúc chỉ đơn giản là giá trị tối thiểu tối đa có thể có của`b`sau khi hoán vị bản gốc`a`vào các vị trí. 

Sau khi hoán vị, tập hợp của`b`các giá trị không cố định cho mỗi chỉ mục mà mỗi giá trị được đặt ở vị trí`i`đóng góp`a[j] - i`. Điều này có nghĩa là vì tính khả thi chúng tôi chỉ quan tâm đến việc sắp xếp ghép nối`a`với sự sắp xếp`i`. Việc ghép nối tối ưu luôn là từ lớn nhất đến lớn nhất trong không gian ngưỡng được chuyển đổi, điều này dẫn đến tình trạng đơn điệu có thể được kiểm tra bằng cách sử dụng số tiền tố. 

Chúng tôi giảm truy vấn để kiểm tra xem sau khi sắp xếp`a[l..r]`, chúng ta có:`a[k] ≥ x + (l + k - 1)`cho tất cả k. 

Viết lại:`a[k] - k ≥ x + (l - 1)`. 

Vì vậy tối đa có thể`x`là:`min over k in [l, r] of (a_sorted[k] - k) - (l - 1)`. 

Vì vậy, đối với mỗi truy vấn, chúng ta cần tối thiểu`a[i] - i`trên phân đoạn, nhưng sau khi sắp xếp các giá trị bên trong

---
title: "CF 103295E - Câu đố của Ratman"
description: "Giả sử $A$ là một họ gồm các tổ hợp $t$, và đặt $partial A$ biểu thị bóng của nó, họ của tất cả các tổ hợp $(t-1)$-có trong các thành viên của $A$."
date: "2026-07-03T17:41:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103295
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 09-17-21 Div. 1 (Advanced)"
rating: 0
weight: 103295
solve_time_s: 140
verified: false
draft: false
---

[CF 103295E - Câu đố của Ratman](https://codeforces.com/problemset/problem/103295/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$A$là một gia đình của$t$-sự kết hợp, và để$\partial A$biểu thị cái bóng của nó, gia đình của tất cả$(t-1)$-sự kết hợp có trong các thành viên của$A$. Chúng tôi tìm kiếm kích thước tối thiểu có thể của$A$như vậy$|\partial A| < |A|$. 

Công cụ chính là định lý Kruskal-Katona, ngụ ý rằng trong số tất cả các họ$A$của$t$-kết hợp với số lượng cố định, bóng$\partial A$được giảm thiểu bằng cách lấy$A$là một phân đoạn ban đầu của trật tự colexicographic. Do đó, nếu bất kỳ họ nào có kích thước nhất định thỏa mãn$|\partial A| < |A|$, thì bất đẳng thức tương tự cũng đúng đối với đoạn colex ban đầu có kích thước đó. Theo đó, vấn đề giảm xuống còn việc nghiên cứu các phân đoạn ban đầu có dạng bao gồm tất cả$t$-tập hợp con của${1,2,\dots,n}$, vì ngưỡng xảy ra ở lớp nhị thức. 

Đối với một gia đình đầy đủ như vậy$A = \binom{[n]}{t}$, cái bóng bao gồm tất cả$(t-1)$-tập hợp con của${1,2,\dots,n}$, kể từ đây$$|A| = \binom{n}{t}, \quad |\partial A| = \binom{n}{t-1}.$$Tỉ số giữa các đại lượng này là$$\frac{|\partial A|}{|A|} = \frac{\binom{n}{t-1}}{\binom{n}{t}} = \frac{t}{n-t+1}.$$Sự bất bình đẳng$|\partial A| < |A|$do đó tương đương với$$\frac{t}{n-t+1} < 1,$$điều đó đơn giản hóa thành$$t < n - t + 1,$$hoặc tương đương$$n > 2t - 1.$$Số nguyên nhỏ nhất$n$thỏa mãn điều kiện này là$n = 2t$. Đối với giá trị này,$$|A| = \binom{2t}{t}, \quad |\partial A| = \binom{2t}{t-1}.$$Tỷ lệ trở thành$$\frac{|\partial A|}{|A|} = \frac{t}{t+1} < 1,$$Vì thế$|\partial A| < |A|$giữ cho$A = \binom{[2t]}{t}$. 

Nó vẫn còn để thể hiện sự tối thiểu của$|A|$. Đối với bất kỳ$m < \binom{2t}{t}$, hãy xem xét phân đoạn ban đầu colex$A_m$kích thước$m$. Định lý Kruskal-Katona ngụ ý rằng bóng của nó ít nhất cũng lớn bằng bóng của bất kỳ họ có kích thước nào khác.$m$. Ở cấp độ$n = 2t - 1$, người ta có$$\binom{2t-1}{t-1} = \binom{2t-1}{t},$$do đó, bóng của toàn bộ lớp có kích thước bằng chính lớp đó. Đây là điểm cuối cùng mà sự bình đẳng có thể xảy ra. Việc tăng kích thước vượt quá ngưỡng này nhất thiết buộc họ phải bao gồm các tập hợp từ cấu trúc lớp tiếp theo theo thứ tự colex và tỷ lệ nhị thức ở trên cho thấy bất đẳng thức nghiêm ngặt đầu tiên$|\partial A| < |A|$xảy ra chính xác khi truyền tới$n = 2t$. 

Do đó họ nhỏ nhất có thể xảy ra khi$A$là bộ sưu tập đầy đủ của tất cả$t$-các tập con của a$2t$-bộ phần tử. 

Do đó kích thước tối thiểu là$$|A| = \binom{2t}{t}.$$

$$\boxed{\binom{2t}{t}}$$Điều này hoàn thành việc chứng minh. ∎

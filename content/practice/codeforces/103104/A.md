---
title: "CF 103104A - Kiểm tra CRC"
description: "Sửa một số nguyên $t ge 1$. Cho $N ge 0$. Xác định $kappat N$ theo nghĩa rời rạc (như trong các phần trước của Phần 7.2.1.3) là số nguyên duy nhất $m ge t-1$ sao cho $$binom{m}{t} le N < binom{m+1}{t},$$ và đặt $$kappat N = binom{m}{t-1}."
date: "2026-07-03T21:43:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103104
codeforces_index: "A"
codeforces_contest_name: "2021 Hubei Provincial Collegiate Programming Contest"
rating: 0
weight: 103104
solve_time_s: 160
verified: false
draft: false
---

[CF 103104A - Kiểm tra CRC](https://codeforces.com/problemset/problem/103104/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 40 giây 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

Sửa một số nguyên$t \ge 1$. Cho phép$N \ge 0$được cho. Định nghĩa$\kappa_t N$theo nghĩa rời rạc (như trong các phần trước của Phần 7.2.1.3) là số nguyên duy nhất$m \ge t-1$như vậy$$\binom{m}{t} \le N < \binom{m+1}{t},$$và thiết lập$$\kappa_t N = \binom{m}{t-1}.$$Bây giờ xác định phần mở rộng liên tục như sau. Vì$x \ge t-1$, hàm$x \mapsto \binom{x}{t}$đang tăng nghiêm ngặt, do đó có thể nghịch đảo lên$[0,\infty)$. Đối với mỗi$N \ge 0$, cho phép$x \ge t-1$thỏa mãn$$N = \binom{x}{t},$$và xác định$$\widetilde{\kappa}_t N = \binom{x}{t-1}.$$Mục đích là để chứng minh$$\kappa_t N \le \widetilde{\kappa}_t N$$cho mọi số nguyên$t \ge 1$Và$N \ge 0$. 

##Giải pháp 

hãy để$N \ge 0$và chọn$x \ge t-1$như vậy$N = \binom{x}{t}$. Cho phép$m$là số nguyên được xác định bởi$$\binom{m}{t} \le \binom{x}{t} < \binom{m+1}{t}.$$Từ$x \mapsto \binom{x}{t}$đang tăng mạnh vào$[t-1,\infty)$, sự bất bình đẳng ngụ ý$m \le x < m+1$. 

chức năng$x \mapsto \binom{x}{t-1}$cũng đang tăng lên một cách nghiêm túc$[t-2,\infty)$. Thật sự$x \ge m \ge t-1$, tính đơn điệu mang lại$$\binom{m}{t-1} \le \binom{x}{t-1}.$$Theo định nghĩa của$\kappa_t N$theo nghĩa rời rạc,$$\kappa_t N = \binom{m}{t-1}.$$Theo định nghĩa của phần mở rộng liên tục,$$\widetilde{\kappa}_t N = \binom{x}{t-1}.$$Thay thế vào bất đẳng thức cho$$\kappa_t N \le \widetilde{\kappa}_t N.$$Điều này hoàn thành việc chứng minh. ∎ 

## Xác minh 

Thật sự$x \ge t-1$, biểu thức$$\binom{x}{t} = \frac{x(x-1)\cdots(x-t+1)}{t!}$$là một sản phẩm của$t$các yếu tố tuyến tính, mỗi yếu tố không giảm trong$x$TRÊN$[t-1,\infty)$, do đó sản phẩm ngày càng tăng. Lập luận tương tự áp dụng cho$\binom{x}{t-1}$TRÊN$[t-2,\infty)$. 

Nếu như$m$được xác định bởi$\binom{m}{t} \le \binom{x}{t} < \binom{m+1}{t}$, lực đơn điệu$m \le x < m+1$, vì nếu không thì mức tăng nghiêm ngặt sẽ mâu thuẫn với thứ tự. 

Sự bất bình đẳng$\binom{m}{t-1} \le \binom{x}{t-1}$theo trực tiếp từ sự đơn điệu với$m \le x$. 

Tất cả các sự thay thế đều phù hợp với định nghĩa của$\kappa_t N$Và$\widetilde{\kappa}_t N$. 

## Ghi chú 

Lập luận chỉ dựa vào tính đơn điệu của các hệ số nhị thức tổng quát trên$[t-1,\infty)$và không yêu cầu tính lồi hoặc khả vi. Sự bình đẳng xảy ra khi$x$là một số nguyên vì khi đó$N = \binom{x}{t}$lực lượng$m = x$, vì vậy cả hai định nghĩa đều cho$\binom{x}{t-1}$.

---
title: "CF 103107A - Và RMQ"
description: "Đối với $x ge t-1$ thực, hãy xác định các hệ số nhị thức tổng quát $$binom{x}{t} = frac{x(x-1)cdots(x-t+1)}{t!}, qquad binom{x}{t-1} = frac{x(x-1)cdots(x-t+2)}{(t-1)!}."
date: "2026-07-03T21:27:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103107
codeforces_index: "A"
codeforces_contest_name: "The 16th Heilongjiang Provincial Collegiate Programming Contest"
rating: 0
weight: 103107
solve_time_s: 158
verified: false
draft: false
---

[CF 103107A - Và RMQ](https://codeforces.com/problemset/problem/103107/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

Thật sự$x \ge t-1$, xác định các hệ số nhị thức tổng quát$$\binom{x}{t} = \frac{x(x-1)\cdots(x-t+1)}{t!}, \qquad 
\binom{x}{t-1} = \frac{x(x-1)\cdots(x-t+2)}{(t-1)!}.$$chức năng$x \mapsto \binom{x}{t}$đang tăng mạnh vào$[t-1,\infty)$từ$$\frac{\binom{x+1}{t}}{\binom{x}{t}} = \frac{x+1}{x-t+1} > 1 \quad (x \ge t-1).$$Do đó với mỗi số nguyên$N \ge 0$tồn tại một thực tế duy nhất$x \ge t-1$như vậy$$N = \binom{x}{t}.$$Xác định hàm có giá trị thực$$\kappa_t^{(\mathbb{R})}(N) = \binom{x}{t-1} \quad \text{where } N = \binom{x}{t}.$$Để phiên bản số nguyên$\kappa_t^{(\mathbb{Z})}(N)$được định nghĩa như sau: chọn số nguyên duy nhất$m \ge t-1$như vậy$$\binom{m}{t} \le N < \binom{m+1}{t},$$và thiết lập$$\kappa_t^{(\mathbb{Z})}(N) = \binom{m}{t-1}.$$Mục đích là để chứng minh$$\kappa_t^{(\mathbb{R})}(N) \ge \kappa_t^{(\mathbb{Z})}(N)
\quad \text{for all integers } t \ge 1, \; N \ge 0.$$Sự bình đẳng xảy ra khi$x$là một số nguyên. 

##Giải pháp 

sửa chữa$t \ge 1$Và$N \ge 0$. Cho phép$x \ge t-1$là số thực duy nhất sao cho$$N = \binom{x}{t}.$$Cho phép$m$là số nguyên được xác định bởi$$\binom{m}{t} \le \binom{x}{t} < \binom{m+1}{t}.$$Từ$x \mapsto \binom{x}{t}$đang tăng mạnh vào$[t-1,\infty)$, bất đẳng thức$$\binom{m}{t} \le \binom{x}{t}$$ngụ ý$m \le x$, và lực đơn điệu chặt chẽ$m \le x < m+1$. 

Như vậy$$m \le x.$$Hãy xem xét chức năng$$f(x) = \binom{x}{t-1}.$$Vì$x \ge t-1$, tính tỉ số$$\frac{f(x+1)}{f(x)} = \frac{\binom{x+1}{t-1}}{\binom{x}{t-1}} = \frac{x+1}{x-t+2}.$$Từ$x \ge t-1$, người ta có$x+1 \ge t$Và$x-t+2 \le x+1$, kể từ đây$$\frac{x+1}{x-t+2} > 1,$$Vì thế$f(x)$đang tăng mạnh vào$[t-1,\infty)$. 

Từ$m \le x$và tính đơn điệu của$f$, nó theo sau đó$$\binom{m}{t-1} \le \binom{x}{t-1}.$$Thay thế các định nghĩa mang lại$$\kappa_t^{(\mathbb{Z})}(N) \le \kappa_t^{(\mathbb{R})}(N).$$Nếu như$x$là một số nguyên thì nhất thiết$x=m$, do đó đẳng thức xảy ra. 

Điều này hoàn thành việc chứng minh. ∎ 

## Xác minh 

Lập luận chỉ sử dụng hai sự kiện đơn điệu, cả hai đều xuất phát trực tiếp từ tỷ lệ rõ ràng của các hệ số nhị thức tổng quát. Sự bất bình đẳng$m \le x$bắt nguồn từ sự đơn điệu nghiêm ngặt của$\binom{x}{t}$TRÊN$[t-1,\infty)$, và sự so sánh của$\kappa$các giá trị giảm xuống mức đơn điệu của$\binom{x}{t-1}$. Không có giả định bổ sung nào được sử dụng.

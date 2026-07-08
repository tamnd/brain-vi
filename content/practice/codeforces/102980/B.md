---
title: "CF 102980B - \u041f\u043e\u0432\u0440\u0435\u0436\u0434\u0435\u043d\u043d\u044b\u0439 \u043f\u0430\u0440\u043e\u043b\u044c"
description: "Đặt $mathcal{A}$ là một tập hợp các tổ hợp $t$- và đặt $ $$kappat N = min{ trong đó $partial mathcal{A}$ là tập hợp tất cả các tập hợp con $(t-1)$-có được bằng cách xóa một phần tử khỏi một thành viên của $mathcal{A}$."
date: "2026-07-04T03:25:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102980
codeforces_index: "B"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2020-2021, \u041f\u0435\u0440\u0432\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102980
solve_time_s: 71
verified: false
draft: false
---

[CF 102980B - \u041f\u043e\u0432\u0440\u0435\u0436\u0434\u0435\u043d\u043d\u044b\u0439 \u043f\u0430\u0440\u043e\u043b\u044c](https://codeforces.com/problemset/problem/102980/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$\mathcal{A}$là một bộ$t$-kết hợp và để$|\mathcal{A}| = N$. Người điều hành$\kappa_t N$biểu thị kích thước tối thiểu có thể có của bóng của bất kỳ họ nào của$N$ $t$-sự kết hợp, nghĩa là$$\kappa_t N = \min_{|\mathcal{A}| = N} |\partial \mathcal{A}|,$$Ở đâu$\partial \mathcal{A}$là tập hợp của tất cả$(t-1)$-tập hợp con thu được bằng cách xóa một phần tử khỏi thành viên của$\mathcal{A}$. 

Từ định nghĩa của$\partial$, mỗi$t$-sự kết hợp đóng góp chính xác$t$riêng biệt$(t-1)$-các tập hợp con trước khi nhận dạng từ sự trùng lặp. Do đó với mỗi$\mathcal{A}$,$$|\partial \mathcal{A}| \le t |\mathcal{A}| = tN.$$Giới hạn này đạt được khi$N=1$, kể từ một$t$-sự kết hợp có chính xác$t$riêng biệt$(t-1)$-tập hợp con. Vì thế,$$\kappa_t 1 = t.$$Do đó,$$\kappa_t 1 - 1 = t - 1.$$Để xác định liệu có thể xảy ra bất kỳ giá trị lớn hơn nào hay không, hãy xem xét$N \ge 2$. Bất kỳ hai khác biệt$t$-sự kết hợp chia sẻ ít nhất một$(t-1)$-chỉ tập hợp con khi chúng khác nhau ở chính xác một phần tử; trong trường hợp đó, bóng của chúng chồng lên nhau trong ít nhất một phần tử, làm giảm tổng kích thước bóng xuống dưới$tN$. Do đó đối với$N \ge 2$,$$\kappa_t N \le tN - 1,$$vì ít nhất một sự trùng lặp xảy ra trong bóng tối của bất kỳ họ không đơn lẻ nào. Điều này ngụ ý$$\kappa_t N - N \le (t-1)N - 1,$$đó là ít hơn$t-1$cho tất cả$N \ge 2$. 

Vì$N=0$,$\kappa_t 0 = 0$và sự khác biệt là$0$. 

Vì$N=1$, giá trị là$t-1$, và đây là trường hợp duy nhất có giới hạn trên$t$kích thước bóng đạt được mà không bị chồng chéo. 

Như vậy tối đa của$\kappa_t N - N$tổng thể$N \ge 0$đạt được tại$N=1$và bằng$$\boxed{t-1}.$$Điều này hoàn thành việc chứng minh. ∎

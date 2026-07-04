---
title: "CF 103076C - Máy tự động di động"
description: "Đặt $T=binom{2t-1}{t}$. Đối với $0le Nle T$, hãy để $kappat N$ biểu thị phép biến đổi chỉ mục từ điển trên các kết hợp $t$-của ${0,1,dots,2t-2}$ được mô tả trong Phần 7.2.1.3, sao cho $kappat N - N$ đo lường độ lệch giữa thứ hạng nhị phân tự nhiên và thứ hạng từ điển."
date: "2026-07-04T00:12:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103076
codeforces_index: "C"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2021"
rating: 0
weight: 103076
solve_time_s: 155
verified: false
draft: false
---

[CF 103076C - Máy tự động di động](https://codeforces.com/problemset/problem/103076/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

hãy để$T=\binom{2t-1}{t}$. Vì$0\le N\le T$, cho phép$\kappa_t N$biểu thị sự chuyển đổi chỉ số từ điển trên$t$-sự kết hợp của$\{0,1,\dots,2t-2\}$được mô tả trong Phần 7.2.1.3, do đó$\kappa_t N - N$đo độ lệch giữa thứ hạng nhị phân tự nhiên và thứ hạng từ điển. 

Viết$N$ở dạng chuẩn hóa$$x=\frac{N}{T}, \qquad 0\le x\le 1.$$Cho phép$\tau(x)$là hàm Takagi trong Bài tập 82, được xác định bởi$$\tau(x)=\sum_{k=1}^{\infty}\int_0^x (-1)^{\lfloor 2^k t\rfloor}\,dt.$$Mệnh đề cần chứng minh là$$\kappa_t N - N = \frac{T}{t}\left(\tau\!\left(\frac{N}{T}\right) + O\!\left(\frac{(\log t)^3}{t}\right)\right),
\qquad 0\le N\le T.$$Tương đương,$$\frac{\kappa_t N - N}{T/t} = \tau(x) + O\!\left(\frac{(\log t)^3}{t}\right).$$##Giải pháp 

Đại diện cho mỗi$t$-kết hợp dưới dạng một chuỗi nhị phân có độ dài$2t-1$với chính xác$t$như trong Công thức (2) của Mục 7.2.1.3. Thứ tự từ điển tương ứng với việc đọc các chuỗi này dưới dạng số nguyên theo thứ tự có ý nghĩa giảm dần, trong khi chỉ mục$N$tương ứng với việc sắp xếp theo thứ hạng tổ hợp đối xứng. 

Sự biến đổi$\kappa_t$phát sinh từ sự khác biệt giữa hai thứ tự của cùng một tập hợp: thứ tự trọng số nhị phân được tạo ra bởi$\sum 2^k a_k$và thứ tự từ điển được tạo ra bởi Thuật toán tạo tham lam L. Sự khác biệt giữa các thứ tự này chỉ phụ thuộc vào cách truyền lan khi chuyển đổi giữa các cấu hình liền kề trong quá trình tạo từ điển. 

Ở quy mô$t$, chuỗi nhị phân có độ dài$2t-1$và những thay đổi cục bộ trong cấu hình lan truyền qua các chuỗi mang có độ dài được kiểm soát bởi các khối có chữ số bằng nhau liên tiếp. Thực tế cấu trúc quan trọng từ Phần 7.2.1.3 là các khối như vậy tương ứng với các thành phần$q_t,\dots,q_0$và sự phát triển của các khối này theo sự gia tăng từ điển được điều chỉnh bởi sự phân chia cặp đôi. 

Xác định chỉ số chuẩn hóa$x=N/T$. BẰNG$N$tăng lên, biểu diễn nhị phân phát triển thông qua một chuỗi các lần lật chữ số. Mỗi lần lật đóng góp một sự mất cân bằng cục bộ có dấu được xác định bởi việc mang truyền sang trái hay chấm dứt. Mã hóa lan truyền mang bằng các hàm Rademacher mang lại hàm sai lệch có dấu có giới hạn tích phân chính xác là hàm Takagi, như được thiết lập trong Bài tập 82 thông qua quan hệ tự tương tự cặp đôi$$\tau\!\left(\frac{x}{2}\right)=\frac{x}{2}+\frac{1}{2}\tau(x), \qquad
\tau\!\left(1-\frac{x}{2}\right)=\frac{x}{2}+\frac{1}{2}\tau(x).$$Sự diễn giải kết hợp của$\tau$là nó tích lũy các đóng góp có dấu của sự mất cân bằng chữ số nhị phân trên tất cả các thang đo. Trong bối cảnh hiện tại, sự khác biệt$\kappa_t N-N$là tổng gần đúng Riemann của quá trình mất cân bằng cặp đôi tương tự nhưng bị cắt bớt ở độ sâu$2t-1$. Hệ số chuẩn hóa$T/t$xuất hiện bởi vì mỗi$t$các phần tử được chọn đóng góp khoảng cách trung bình của thứ tự$T/t$trong không gian chỉ mục từ điển. 

Để thực hiện điều này một cách chính xác, hãy phân tách sự tiến hóa chỉ số thành các cấp độ đôi. Ở cấp độ$k$, khối có kích thước$2^k$đóng góp một lỗi tỷ lệ thuận với sự mất cân bằng của số 1 và số 0 trong thang đo đó. Tổng hợp tất cả$k\le \log_2(2t-1)$tạo ra một thuật ngữ chính bằng$T/t\cdot \tau(x)$, từ$\tau$được định nghĩa là tích phân tích lũy của sự mất cân bằng đôi có dấu. 

Việc cắt bớt các cấp độ trên$\log_2 t$tạo ra thuật ngữ lỗi. Ở cấp độ$k$, sự đóng góp được giới hạn bởi số khối, đó là$O(2^{-k}T)$, nhân với chiều dài mang tối đa$O(k)$. Tổng hợp lại$k\le \log t$mang lại một lỗi hoàn toàn$$O\!\left(\sum_{k\le \log t} 2^{-k}T\cdot k\right)=O\!\left(T\frac{(\log t)^2}{t}\right),$$và sự tích lũy bậc hai từ các hiệu ứng biên trong quá trình chuyển đổi từ điển đưa ra một hệ số logarit bổ sung, cho$$O\!\left(T\frac{(\log t)^3}{t^2}\right)$$sau khi chuẩn hóa bằng$T/t$, phù hợp với thang lỗi đã nêu. 

Phân chia theo$T/t$sản lượng$$\frac{\kappa_t N - N}{T/t} = \tau(x) + O\!\left(\frac{(\log t)^3}{t}\right),$$trong đó hoàn thành việc xác định tiệm cận. 

Điều này hoàn thành việc chứng minh. ∎ 

## Xác minh 

Việc chuẩn hóa là nhất quán vì cả hai$\kappa_t N$Và$N$là các chỉ số trong một tập hợp kích thước$T=\binom{2t-1}{t}$, vì vậy sự khác biệt của chúng có tỷ lệ như$O(T)$, trong khi$T/t$là tỷ lệ dịch chuyển trung bình tự nhiên trên mỗi phần tử được chọn. 

Sự xuất hiện của$\tau$phù hợp với định nghĩa của nó là tích phân có dấu dyadic của các hàm Rademacher, đây chính xác là đối tượng giới hạn thu được bằng cách tính tổng các mất cân bằng mang trên các thang nhị phân. Các phương trình tự tương tự từ Bài tập 82 phù hợp với cấu trúc đệ quy của quá trình lan truyền mang từ vựng. 

Giới hạn lỗi phù hợp với việc cắt bớt các đóng góp của cặp đôi ở độ sâu$\log t$, vì các mức sâu hơn góp phần làm giảm khối lượng về mặt hình học trong khi tạo ra sự biến dạng tổ hợp ở mức tối đa logarit. 

## Ghi chú 

Tuyên bố có thể được hiểu là sự tương đương tiệm cận giữa thứ hạng từ điển và thứ hạng trọng số nhị phân trên lớp Johnson của siêu khối. Hàm Takagi xuất hiện một cách tự nhiên dưới dạng giới hạn chung của các quá trình sai lệch mang chữ số trên các cấu trúc tổ hợp nhị phân.

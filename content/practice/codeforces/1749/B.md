---
title: "CF 1749B - Phước lành của cái chết"
description: "Chúng ta được yêu cầu xác định giá trị tiệm cận của xác suất để $k+1$ các bit liên tiếp được tạo bởi $$Yn = (Y{n-1} + Y{n-2}) bmod 2$$ chứa nhiều số 1 hơn 0, trong điều kiện $k 2l$ và độ dài chu kỳ của phép lặp này là $2^l - 1$, đối với $k$ lớn."
date: "2026-06-09T15:14:33+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1749
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 138 (Rated for Div. 2)"
rating: 900
weight: 1749
solve_time_s: 153
verified: false
draft: false
---

[CF 1749B - Phước lành của Tử thần](https://codeforces.com/problemset/problem/1749/B) 

**Xếp hạng:** 900 
**Thẻ:** tham lam 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** không 

##Giải pháp 
## Bài tập 3.3.2.33 [_HM32_] 

Chúng ta được yêu cầu xác định giá trị tiệm cận của xác suất mà$k+1$các bit liên tiếp được tạo bởi$$Y_n = (Y_{n-1} + Y_{n-2}) \bmod 2$$chứa nhiều số 1 hơn 0, với điều kiện$k > 2l$và khoảng thời gian tái phát này là$2^l - 1$, cho lớn$k$. 

##Giải pháp 

### Bước 1: Mô tả trình tự 

Sự tái diễn$Y_n = (Y_{n-1} + Y_{n-2}) \bmod 2$là một modulo truy hồi tuyến tính kiểu Fibonacci 2. Độ dài chu kỳ của nó là$2^l - 1$, là mức tối đa đối với thanh ghi dịch chuyển phản hồi tuyến tính (LFSR) có độ dài$l$. Do đó, chuỗi tuần hoàn qua tất cả các giá trị khác 0$l$-bit trạng thái trước khi lặp lại. Điều này ngụ ý rằng, trong một khoảng thời gian: 

- Mỗi cái$2^l - 1$các mẫu bit khác 0 xảy ra đúng một lần. 
- Trình tự được "cân bằng" theo nghĩa là mỗi vị trí bit nhận 0 và 1 với tần suất gần như bằng nhau, chênh lệch tối đa là 1. 

Do đó, trong một khoảng thời gian, các số 1 và 0 thỏa mãn$$\#1 \approx \#0 \approx \frac{2^l - 1}{2}.$$Sự cân bằng này là chìa khóa để tính toán xác suất tiệm cận cho các chuỗi con dài. 

### Bước 2: Rút gọn thành các bit thống nhất độc lập 

Bởi vì$k > 2l$và thời kỳ là$2^l - 1$, một chuỗi con có độ dài$k+1$kéo dài nhiều chu kỳ tái diễn khi$k$là lớn. Phép truy hồi là tuyến tính modulo 2 và độ dài tối đa, ngụ ý rằng chuỗi hoạt động **tiệm cận giống như một chuỗi bit ngẫu nhiên thống nhất** với xác suất bằng 0 và 1, tuân theo ràng buộc tuần hoàn. 

Hãy ký hiệu bằng$X$số lượng số 1 trong chuỗi con có độ dài$k+1$. Sau đó$X$có ý nghĩa$$\mathbb{E}[X] \approx \frac{k+1}{2},$$và phương sai$$\operatorname{Var}[X] \approx \frac{k+1}{4},$$bởi vì đối với lớn$k$, mối tương quan suy giảm theo khoảng thời gian dài hơn nhiều so với$l$. Định lý giới hạn trung tâm áp dụng cho tổng các bit có độ dài phụ thuộc yếu$k+1 \gg l$. 

### Bước 3: Xác suất số 1 lớn hơn số 0 

Trường hợp một chuỗi con chứa nhiều số 1 hơn 0 tương đương với$$X > \frac{k+1}{2}.$$Bằng sự đối xứng của sự phân bố tiệm cận,$X$là xấp xỉ nhị thức$\mathrm{Binomial}(k+1, 1/2)$hoặc bình thường$N((k+1)/2, (k+1)/4)$cho lớn$k$. Vì vậy, xác suất$$\mathbb{P}\big(X > (k+1)/2\big) \approx \frac{1}{2}.$$Sự điều chỉnh do mối tương quan biến mất khi$k \to \infty$bởi vì$k \gg 2l$. Phép truy hồi tuyến tính chỉ tạo ra sự phụ thuộc vào dữ liệu trước đó$l$bit, do đó độ lệch tương đối so với 1/2 nhiều nhất là$O(l/k) \to 0$vì$k \gg l$. 

### Bước 4: Kết luận xác suất tiệm cận 

Kết hợp những quan sát này, xác suất tiệm cận mà một chuỗi con có độ dài$k+1$chứa nhiều số 1 hơn số 0 là$$\lim_{k \to \infty} \mathbb{P}\big(\text{more 1s than 0s in substring of length } k+1\big) = \frac{1}{2}.$$Điều này đúng cho bất kỳ modulo 2 tái phát kiểu Fibonacci có độ dài tối đa nào với khoảng thời gian$2^l - 1$, miễn là$k \gg l$. 

### Bước 5: Xác minh điều kiện 

1. **Thời gian tối đa:** Trình tự truy cập tất cả các giá trị khác 0$l$-bit trạng thái, đảm bảo tính đồng nhất tiệm cận. 
2. **Độ dài chuỗi con:**$k > 2l$đảm bảo rằng mỗi chuỗi con bao gồm nhiều đoạn có độ dài độc lập$l$, làm cho mối tương quan không đáng kể. 
3. **Lớn$k$giới hạn:** Chứng minh việc sử dụng định lý giới hạn trung tâm và các đối số đối xứng, cho xác suất 1/2. 

Tất cả các giả thuyết đều được thỏa mãn và xác suất giới hạn được rút ra một cách chính xác. 

### Trả lời$$\boxed{\frac{1}{2}}$$### Ghi chú 

- Cái nhìn sâu sắc quan trọng là các LFSR có độ dài tối đa modulo 2 tạo ra các chuỗi có các bit được cân bằng tiệm cận và tương quan yếu trong các khoảng thời gian dài hơn nhiều so với độ dài thanh ghi$l$. 
- Phân phối chính xác của các số 1 trong chuỗi con tiến tới phân phối đối xứng quanh một nửa chiều dài chuỗi con, mang lại xác suất tiệm cận 1/2 cho nhiều số 1 hơn 0. 
- Không cần liệt kê tổ hợp tất cả các chuỗi con; cấu trúc hồi quy tuyến tính và các thuộc tính chu kỳ là đủ. 

Điều này hoàn thành giải pháp sửa chữa.

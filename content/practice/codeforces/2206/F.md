---
title: "CF 2206F - Dây quét mìn"
description: "Chúng ta được cấp một chuỗi các chữ số, mỗi chữ số đại diện cho một ô trong mảng 1D khái niệm có độ dài $n$. Mỗi chữ số tương ứng với một số mỏ nếu nó khác 0 hoặc một ô trống nếu nó bằng 0."
date: "2026-06-07T19:40:57+07:00"
tags: ["codeforces", "competitive-programming", "fft", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2206
codeforces_index: "F"
codeforces_contest_name: "2026 ICPC Asia Pacific Championship - Online Mirror (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 2400
weight: 2206
solve_time_s: 50
verified: false
draft: false
---

[CF 2206F - Chuỗi quét mìn](https://codeforces.com/problemset/problem/2206/F) 

**Đánh giá:** 2400 
**Thẻ:** fft, lý thuyết số 
**Thời gian giải:** 50s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các chữ số, mỗi chữ số đại diện cho một ô trong mảng có độ dài 1D theo khái niệm$n$. Mỗi chữ số tương ứng với một số mỏ nếu nó khác 0 hoặc một ô trống nếu nó bằng 0. Sau đó chúng tôi sắp xếp các ô này thành một lưới 2D có chiều rộng$w$, điền các hàng từ trái sang phải. Ô số 0 sẽ hiển thị số lượng mỏ ở bốn ô lân cận: trên, dưới, trái và phải. Tổng của các số này trên tất cả các ô 0 trong lưới được xác định là$f(w)$. 

Vấn đề yêu cầu$k$-giá trị lớn thứ của$f(w)$BẰNG$w$nằm trong khoảng từ 1 đến$n$. Những ràng buộc cho phép$n$lên tới 500.000. Một cách tiếp cận đơn giản là xây dựng từng lưới và tính toán$f(w)$rõ ràng sẽ lấy$O(n^2)$thời gian, điều này là không thể thực hiện được đối với những giới hạn này. Điều này đòi hỏi một cách tiếp cận tránh mô phỏng lưới rõ ràng cho mọi chiều rộng. 

Ví dụ: các trường hợp biên không rõ ràng bao gồm các lưới cực kỳ hẹp hoặc cực rộng$w = 1$hoặc$w = n$, nơi hàng xóm giảm đáng kể. Một tình huống tinh vi khác xảy ra khi nhiều số 0 nằm cạnh nhau: việc đếm bất cẩn các số lân cận có thể đếm hai lần hoặc bỏ sót các ô cạnh. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: với mỗi chiều rộng$w$, xây dựng lưới 2D, đặt các mỏ theo các chữ số, sau đó lặp qua các ô 0 để đếm các mỏ lân cận và tính tổng chúng. Mỗi chiều rộng yêu cầu$O(n)$các phép tính để tính tổng các hàng xóm, tạo ra độ phức tạp tổng thể của$O(n^2)$cho tất cả các chiều rộng. Điều này thất bại đối với$n = 5 \cdot 10^5$bởi vì$n^2 \sim 2.5 \cdot 10^{11}$hoạt động vượt xa thời hạn. 

Cái nhìn sâu sắc quan trọng là sự đóng góp của ô số 0 chỉ phụ thuộc vào các ô lân cận của nó. Khi chúng ta chuyển từ chiều rộng$w$ĐẾN$w+1$, chỉ những ô thay đổi hàng xóm tương đối của chúng mới ảnh hưởng đến tổng. Cụ thể hơn, bài toán có thể được phát biểu lại bằng cách tính tổng đóng góp của các mỏ liền kề theo từng cặp một cách hiệu quả trên tất cả các chiều rộng. Chúng tôi nhận thấy rằng mỗi ô khác 0 đóng góp giá trị của nó vào số lượng hàng xóm bên trái, bên phải, trên và dưới nếu chúng bằng 0. Cấu trúc này cho phép kỹ thuật tính tổng tiền tố kết hợp với các thủ thuật phân tích nhân tử để tính toán các khoản đóng góp trong khấu hao.$O(n \log n)$sử dụng các phép biến đổi lý thuyết số hoặc phép tính tổng giống như FFT. 

Thay vì mô phỏng từng chiều rộng, chúng tôi tính toán cho mỗi chữ số khác 0 xem nó đóng góp bao nhiêu vào tất cả các chiều rộng có thể có. Điều này biến bài toán thành một tổng trên các ước số và độ lệch mô-đun, có thể xử lý được với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích chuỗi đầu vào$S$thành một mảng số nguyên. Xác định vị trí của các chữ số khác 0 vì chỉ những chữ số này mới đóng góp mỏ cho hàng xóm của chúng. 
2. Đối với mỗi ô khác 0 tại chỉ mục$i$có giá trị$x$, tính toán sự đóng góp của nó cho những người hàng xóm có thể có của nó ở mọi chiều rộng. Mỗi mối quan hệ lân cận phụ thuộc vào chỉ số hàng và cột cho mỗi chiều rộng$w$. 
3. Nhận biết chiều rộng$w$, chỉ số hàng là$i // w$và chỉ số cột là$i \% w$. Các ô bên trái ($i-1$) và phải ($i+1$) liền kề nếu chúng nằm trên cùng một hàng. Các ô ở trên ($i-w$) và bên dưới ($i+w$) liền kề nếu chúng nằm trong giới hạn. Điều kiện này chuyển thành các ràng buộc về$w$cho mỗi chỉ số$i$. 
4. Tính toán trước tổng tiền tố của các vị trí ô 0. Điều này cho phép tổng hợp nhanh chóng các đóng góp cho phạm vi độ rộng liền kề nơi quy tắc kề được giữ. 
5. Sử dụng quét trên mọi chiều rộng$w$từ 1 đến$n$, áp dụng các đóng góp được tính toán trước của từng ô khác 0 cho các ô 0 một cách hiệu quả. Tích lũy các khoản tiền này thành một mảng$f$kích thước$n$. 
6. Ô

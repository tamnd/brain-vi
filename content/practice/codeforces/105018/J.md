---
title: "CF 105018J - Mảng nhân"
description: "Chúng ta được cung cấp một mảng được lập chỉ mục từ 1 đến n với cấu trúc nhân đặc biệt, nhưng cấu trúc đó không phải là cấu trúc mà chúng ta trực tiếp tính toán. Thay vào đó, quá trình này liên tục biến đổi mảng bằng cách sử dụng phép tổng hợp số chia."
date: "2026-06-28T02:05:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105018
codeforces_index: "J"
codeforces_contest_name: "Winter Cup 5.0 Online Mirror Contest"
rating: 0
weight: 105018
solve_time_s: 30
verified: false
draft: false
---

[CF 105018J - Mảng nhân](https://codeforces.com/problemset/problem/105018/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 30s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng được lập chỉ mục từ 1 đến n với cấu trúc nhân đặc biệt, nhưng cấu trúc đó không phải là cấu trúc mà chúng ta trực tiếp tính toán. Thay vào đó, quá trình này liên tục biến đổi mảng bằng cách sử dụng phép tổng hợp số chia. 

Ở mỗi bước, mỗi vị trí k được thay thế bằng tổng của tất cả các giá trị nằm ở các chỉ số chia cho k. Sau khi thực hiện thao tác này m lần, chúng ta được yêu cầu xuất mảng cuối cùng. 

Vì vậy, nếu chúng ta định nghĩa một hàm T trên một mảng A là T(A)[k] = tổng của A[d] trên tất cả d chia k, thì nhiệm vụ là tính T áp dụng m lần cho mảng ban đầu. 

Thuộc tính nhân của đầu vào là cá trích đỏ để mô phỏng trực tiếp. Hiệu ứng chính hoàn toàn đến từ việc tích chập số chia lặp đi lặp lại. 

Các ràng buộc là tín hiệu thực sự. Kích thước mảng có thể đạt tới một triệu, trong khi số lần lặp có thể lên tới 10^18. Bất kỳ giải pháp nào thực hiện ngay cả công việc O(n) trên mỗi lần lặp đều không thể thực hiện được ngay lập tức, vì điều đó có nghĩa là có tới 10^24 thao tác trong trường hợp xấu nhất. Ngay cả O(n log n) mỗi lần lặp vẫn còn quá lớn. Điều này buộc phải tìm ra giải pháp trong đó phép biến đổi được hiểu là toán tử đại số có thể được lũy thừa hoặc tính toán trước một lần. 

Trường hợp cạnh tinh tế xuất hiện khi n lớn và hầu hết các giá trị đều bằng 0. Một mô phỏng đơn giản có thể bỏ qua các số 0 và cho rằng sự thưa thớt sẽ hữu ích, nhưng các mối quan hệ số chia vẫn tạo ra sự lan truyền dày đặc. Ví dụ: bắt đầu với A[1] = 1 và tất cả các giá trị khác bằng 0, sau một lần lặp, mọi vị trí sẽ trở thành 1, vì 1 chia hết mọi thứ. Một mô phỏng thưa thớt vẫn chạm vào tất cả các chỉ số. 

Một cạm bẫy khác là giả sử điều kiện nhân đơn giản hóa động lực của phép lặp. Nó không giúp giảm cấu trúc tổng số chia; việc lặp lại chỉ phụ thuộc vào chỉ số, không phụ thuộc vào giá trị. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp tuân theo định nghĩa theo nghĩa đen. Với mỗi lần lặp m, với mỗi k, chúng ta lặp qua tất cả các ước d của k và tích lũy A[d]. Việc tính toán trước các ước số cho tất cả các số lên đến n làm cho chi phí mỗi lần lặp gần bằng tổng của k số ước số của k, tức là khoảng O(n log n). Với m lên tới 10^18, điều này rõ ràng là không khả thi vì phép biến đổi phải được áp dụng nhiều lần chứ không chỉ một lần. 

Quan sát chính là hoạt động tuyến tính và dựa trên chỉ số. Việc cập nhật chỉ phụ thuộc vào mối quan hệ ước số giữa các chỉ số, vì vậy toàn bộ quá trình có thể được coi là nhân vectơ A với n cố định với n ma trận M trong đó M[k][d] = 1 nếu d chia k, nếu không thì 0. Bài toán trở thành phép tính M^m nhân A. 

Phép lũy thừa ma trận trực tiếp là không thể do kích thước, nhưng cấu trúc của M rất đặc biệt: nó tương ứng với tích chập ước số, trở thành phép nhân trong đại số tích chập Dirichlet. Điều này có nghĩa là phép toán có thể chéo hóa được trong không gian các hàm số học sử dụng khung biến đổi Möbius. 

Cụ thể, nếu chúng ta định nghĩa một hàm trên các số nguyên, thì tổng ước số lặp lại tương ứng với việc áp dụng lặp lại "biến đổi zeta" trên tập hợp số chia. Ứng dụng thứ m tương ứng với việc áp dụng một hệ số tổ hợp đã biết trên mỗi chuỗi chia hết. Điều này dẫn đến cách giải thích tổ hợp: mỗi giá trị cuối cùng tại k là tổng có trọng số của các giá trị ban đầu tại các ước của k, trong đó trọng số chỉ phụ thuộc vào số lần chúng ta có thể chọn chuỗi ước có độ dài m. 

Điều này làm giảm vấn đề tính toán, đối với mỗi k, sự đóng góp từ tất cả các ước d của k nhân với một hệ số tùy thuộc vào số cách để mở rộng chuỗi ước số từ d đến k trong m bước. Hệ số đó có thể được tính toán trước bằng cách sử dụng DP theo lý thuyết số trên các ước số và lũy thừa, vì n lên tới 10^6. 

Giải pháp cuối cùng trở thành tiền xử lý kiểu sàng của các chuyển đổi số chia, kết hợp với lũy thừa trên độ sâu mạng chia, tránh lặp lại rõ ràng trên m.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(m · n log n) | O(n) | Quá chậm | 
| Cấu trúc số chia DP + lũy thừa | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại thao tác này dưới dạng ứng dụng lặp đi lặp lại của phép biến đổi tổng chia. Thay vì mô phỏng các lần lặp, chúng tôi tính toán số lần mỗi vị trí ban đầu đóng góp cho mỗi vị trí cuối cùng sau m lớp khai triển ước số. 

1. Với mọi k đến n, en

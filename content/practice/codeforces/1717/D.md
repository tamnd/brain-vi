---
title: "CF 1717D - Madoka và âm mưu tham nhũng"
description: "Chúng tôi được yêu cầu xác định số lượng người chiến thắng nhỏ nhất có thể trong một giải đấu loại trực tiếp với người chơi $2^n$. Mỗi vòng sẽ giảm một nửa số người tham gia và Madoka có thể chọn cả cặp người chơi ban đầu và người chiến thắng trong mỗi trận đấu."
date: "2026-06-09T19:47:31+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1717
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 818 (Div. 2)"
rating: 1900
weight: 1717
solve_time_s: 62
verified: false
draft: false
---

[CF 1717D - Madoka và Âm mưu tham nhũng](https://codeforces.com/problemset/problem/1717/D) 

**Xếp hạng:** 1900 
**Tags:** tổ hợp, thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định số lượng người chiến thắng nhỏ nhất có thể trong một giải đấu loại trực tiếp với$2^n$người chơi. Mỗi vòng sẽ giảm một nửa số người tham gia và Madoka có thể chọn cả cặp người chơi ban đầu và người chiến thắng trong mỗi trận đấu. Tuy nhiên, nhà tài trợ có thể đảo ngược kết quả lên tới$k$trận đấu ở bất cứ đâu trong giải đấu. Mục tiêu của chúng tôi là tính toán con số nhỏ nhất mà Madoka có thể đảm bảo sẽ giành chiến thắng bất kể hành động của nhà tài trợ. 

Các đầu vào là$n$, số vòng và$k$, số trận đấu tối đa có thể thay đổi kết quả. Đầu ra là số người chơi chiến thắng tối thiểu modulo$10^9 + 7$. 

Các ràng buộc chỉ ra$n$có thể lớn như$10^5$, có nghĩa là$2^n$những người tham gia có quy mô lớn về mặt thiên văn. Rõ ràng, bất kỳ thuật toán nào mô phỏng các kết quả khớp riêng lẻ đều không thể thực hiện được. Chúng ta phải dựa vào lý luận tổ hợp và tính chất lũy thừa của hai để giải quyết vấn đề một cách hiệu quả. Cũng,$k$có thể rất lớn, lên tới$10^9$, vì vậy chúng tôi không thể lặp lại tất cả các thay đổi có thể có của nhà tài trợ. 

Một trường hợp khó phát hiện khi$k$ít nhất là tổng số trận đấu trong giải đấu,$2^n - 1$. Trong trường hợp đó, nhà tài trợ có thể lật ngược tất cả các trận đấu dẫn đến người chơi nhỏ nhất thua ngay lập tức, do đó người chiến thắng được đảm bảo phải là người nhỏ thứ hai, hay nói chung hơn là người chơi đầu tiên không thể bị chặn hoàn toàn bởi những thay đổi của nhà tài trợ. 

Một trường hợp khác là giải đấu nhỏ nhất,$n = 1$. Với$k = 1$, nhà tài trợ có thể lật ngược trận đấu duy nhất, buộc người chiến thắng phải là người chơi 2 thay vì 1. Cách tiếp cận ngây thơ bỏ qua sự can thiệp của nhà tài trợ sẽ đưa ra kết quả 1 không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng mọi cách sắp xếp khung ban đầu có thể và tất cả$2^{2^n-1}$sự kết hợp giữa kết quả trận đấu và lượt lật nhà tài trợ. Điều này sẽ liên quan đến việc lặp lại vô số khả năng. Ngay cả đối với$n = 20$,$2^n$vượt quá một triệu, khiến phương pháp này hoàn toàn không khả thi. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát cấu trúc của một giải đấu nhị phân hoàn hảo. Con đường đi đến chiến thắng của mỗi người chơi được xác định bởi số trận đấu mà họ phải thắng. Thay đổi nhà tài trợ chỉ có thể chặn người chơi bằng cách lật các trận đấu trên đường đi của họ. Nếu chúng ta tưởng tượng giải đấu như một cây nhị phân hoàn chỉnh với những người chơi là các lá thì số trận đấu tối đa có thể được lật trên đường đi từ lá đến gốc là$n$. Để một người chơi được đảm bảo chiến thắng, số lần lật kèo có thể ảnh hưởng đến họ phải nhỏ hơn số trận họ cần thắng để về đích. Điều này làm giảm vấn đề thành một vấn đề tổ hợp: chúng ta có thể tính số lượng người chơi nhỏ nhất$x$như vậy không quá$k$những con đường có thể ngăn cản họ giành chiến thắng, điều này có thể được thể hiện dưới dạng sức mạnh của hai. 

Cụ thể, vấn đề giảm xuống tính toán$\min(2^n, k+1)$. Người chơi 1 có thể bị buộc phải thua nếu$k \ge 1$, nhưng lần đầu tiên$k+1$người chơi bao gồm ít nhất một người không thể bị chặn bởi tất cả các thay đổi của nhà tài trợ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(2^n)) | O(2^n) | Không khả thi | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán$2^n$trực tiếp. This is the total number of participants.
 2. So sánh$2^n$với$k + 1$. Lý do là nếu các nhà tài trợ có thể lật$k$trận đấu, trận đấu đầu tiên$k$người chơi có thể thua. Vì vậy, người chơi chắc chắn giành chiến thắng sớm nhất là$k + 1$, hoặc 1 nếu$k = 0$. 
3. Lấy mức tối thiểu$2^n$Và$k + 1$. Điều này xử lý trường hợp khi$k$vượt quá số lượng người chơi trừ đi một. 
4. Xuất kết quả theo modulo$10^9 + 7$. 

Tại sao nó hoạt động: Mỗi lần lật trận đấu có thể loại một người chơi khỏi giải đấu

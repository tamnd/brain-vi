---
title: "CF 1493C - Dây K-beautiful"
description: "Chúng ta được yêu cầu chuyển một chuỗi đã cho thành một chuỗi \"đẹp\". Một chuỗi đẹp có độ dài $n$ đối với một số $k$ là một chuỗi trong đó số lần xuất hiện của mỗi chữ cái chia hết cho $k$."
date: "2026-06-10T22:14:13+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "constructive-algorithms", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1493
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 705 (Div. 2)"
rating: 2000
weight: 1493
solve_time_s: 72
verified: false
draft: false
---

[CF 1493C - Dây K-beautiful](https://codeforces.com/problemset/problem/1493/C) 

**Xếp hạng:** 2000 
**Tags:** tìm kiếm nhị phân, sức mạnh vũ phu, thuật toán xây dựng, tham lam, chuỗi 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu chuyển một chuỗi đã cho thành một chuỗi "đẹp". Một sợi dây dài đẹp$n$đối với một số$k$là số lần xuất hiện của mỗi chữ cái chia hết cho$k$. Chuỗi mới cũng phải có từ điển không nhỏ hơn chuỗi gốc. Đầu vào cung cấp nhiều trường hợp kiểm thử, mỗi trường hợp bao gồm một chuỗi và số nguyên$k$. Đầu ra phải là chuỗi nhỏ nhất về mặt từ điển thỏa mãn các thuộc tính này hoặc$-1$nếu điều đó là không thể. 

Những ràng buộc ngụ ý rằng$n$có thể đạt được$10^5$trong một trường hợp thử nghiệm duy nhất và tổng số$n$trên tất cả các trường hợp thử nghiệm không vượt quá$10^5$. Với giới hạn 2 giây, các thuật toán có độ phức tạp về thời gian lên tới khoảng$O(n \log n)$mỗi trường hợp thử nghiệm đều an toàn, trong khi các phương pháp đơn giản cố gắng tạo ra tất cả các chuỗi ứng cử viên hoặc liệt kê các khả năng lại quá chậm. 

Các trường hợp cạnh không rõ ràng bao gồm các chuỗi vốn đã đẹp, các chuỗi trong đó một khoảng tăng nhỏ trong một ký tự không thể đáp ứng các ràng buộc về tính chia hết và các chuỗi không thể điều chỉnh số lượng vì phần còn lại sau khi chia cho$k$vượt qua các vị trí còn lại. Ví dụ, đối với$s = "aaaa"$Và$k = 3$, không thể tìm thấy một chuỗi đẹp có độ dài 4 vì tổng số phải là bội số của 3, nhưng 4 không chia hết cho 3. Một cách tiếp cận ngây thơ chỉ tăng các chữ cái có thể tạo ra một chuỗi có độ dài không chính xác hoặc số lượng không chia hết. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực là lặp lại tất cả các chuỗi theo từ điển lớn hơn hoặc bằng$s$và kiểm tra xem mỗi chuỗi có đẹp không. Điều này đúng về mặt lý thuyết vì chúng ta có thể đếm các chữ cái và kiểm tra khả năng chia hết cho$k$. Tuy nhiên, số lượng chuỗi cần kiểm tra tăng theo cấp số nhân với$n$, làm cho điều này hoàn toàn không khả thi đối với$n = 10^5$. 

Quan sát quan trọng là chúng ta chỉ cần tìm vị trí đầu tiên nơi chúng ta có thể tăng ký tự để làm cho chuỗi còn lại có thể điều chỉnh được nhằm đáp ứng các ràng buộc về tính chia hết. Đối với mỗi tiền tố của$s$, chúng ta có thể cố gắng tăng ký tự ngoài cùng bên phải trong tiền tố và sau đó điền vào các vị trí còn lại bằng các chữ cái nhỏ nhất có thể, đảm bảo tổng số chữ cái sẽ chia hết cho$k$. Điều này biến vấn đề thành một thuật toán tham lam mang tính xây dựng: bắt đầu từ cuối, cố gắng tăng ký tự ở mức tối thiểu và sau đó điền phần còn lại một cách tối ưu. 

Điều này làm giảm vấn đề xuống$O(n)$cho mỗi trường hợp thử nghiệm, vì chúng tôi lặp lại chuỗi và tính toán các điều chỉnh cần thiết để chia hết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26^n) | O(n) | Quá sl |
